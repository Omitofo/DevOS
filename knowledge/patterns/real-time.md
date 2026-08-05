# Real-Time

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Tech Lead

Mechanisms for pushing updates to clients or enabling low-latency bidirectional communication.

## Mechanism Families

| Mechanism | Direction | Typical use | Complexity |
|-----------|-----------|-------------|------------|
| **Short polling** | Client → Server | Simple status checks | Low |
| **Long polling** | Client → Server (held) | Near-real-time without persistent connections | Medium |
| **Server-Sent Events (SSE)** | Server → Client | One-way updates, notifications, feeds | Medium |
| **WebSockets** | Bidirectional | Collaborative editing, chat, live dashboards, games | Higher |
| **WebTransport / HTTP/3** | Bidirectional | Emerging; lower latency, multiplexed | Higher (ecosystem maturity) |

## Preference Guidance

**Prefer polling (short or long) when**
- Update frequency is low or tolerance for latency is high.
- Operational simplicity and broad compatibility outweigh push efficiency.
- Complexity band is S and real-time is a nice-to-have rather than core.

**Prefer SSE when**
- The dominant need is server-to-client updates (notifications, live scores, progress).
- Browser-native EventSource is sufficient and reconnection logic is acceptable.
- Bidirectional messaging is not required.

**Prefer WebSockets when**
- True bidirectional, low-latency messaging is a product requirement.
- Multiple concurrent streams or high message rates are expected.
- The team can own connection lifecycle, scaling, and fallback behaviour.

## Essential Decision Points

1. **Is real-time actually required?**  
   - Many “live” needs are satisfied by short polling or cache + refresh.  
   - Introduce persistent connections only when the latency or frequency justifies the operational cost.

2. **Connection lifecycle & presence**  
   - Authentication of the connection.  
   - Heartbeats / keep-alive.  
   - Graceful handling of network changes (mobile) and server restarts.  
   - Presence (“user is online”) is a separate concern that often needs its own design.

3. **Fan-out & backplane**  
   - Single-server WebSockets do not scale; a pub/sub backplane (Redis, NATS, managed service) is usually required for multi-instance deployments.  
   - Document the fan-out topology.

4. **Ordering, delivery guarantees, and idempotency**  
   - At-most-once vs. at-least-once.  
   - Clients must be prepared for duplicates or gaps depending on the guarantee.

5. **Fallback**  
   - WebSocket failure should degrade to SSE or polling where the product can still function.

6. **Security**  
   - Same authentication and authorization model as the rest of the API.  
   - Origin checks, rate limiting per connection, and message-size limits.

## Anti-Patterns

- Opening a WebSocket for every page load when simple polling would suffice.
- Assuming messages arrive exactly once and in order without designing for it.
- Putting business logic only on the WebSocket path while leaving the REST/HTTP path incomplete.
- No backplane in a multi-instance deployment (messages lost when the connected instance changes).
- Unauthenticated or long-lived anonymous sockets.

## Recording Requirements

In the Architecture Blueprint:

- Chosen mechanism(s) and the product events that use them
- Scaling / backplane approach
- Delivery guarantees and fallback behaviour
- Link to this file and the criteria used

## Related Patterns

- [api-design.md](api-design.md) — how real-time relates to the request/response API
- [caching.md](caching.md) — when cache invalidation + poll is enough
- knowledge/technologies/ — concrete libraries and managed services
