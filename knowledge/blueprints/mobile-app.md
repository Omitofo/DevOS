# Mobile App Blueprint

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-06  
**Primary consumers:** Architect, Planner, Tech Lead, Security & Quality Auditor

Opinionated starting architecture and decision framework for products whose primary client is a native or hybrid mobile application.  
Use this blueprint when the primary product type is **Mobile / Native-First Application** (see [knowledge/classification/website-types.md](../classification/website-types.md)).

This file does **not** prescribe a single stack or a single distribution model. It supplies a coherent default shape, the critical decision points that must be resolved early, and the knowledge links agents are expected to follow.

## When to Apply

**Strong signals that this blueprint is appropriate**
- The primary user experience is delivered through a mobile app (iOS, Android, or both) rather than a browser.
- App-store distribution, push notifications, device capabilities (camera, biometrics, location, sensors), or offline-first behaviour are core to the value proposition.
- Interaction patterns, navigation, and design language are oriented to mobile platform conventions (or deliberately cross-platform).
- A web surface, if present, is secondary (companion site, admin console, marketing, or shared backend).

**Counter-signals (consider a different blueprint)**
- Primary experience is a responsive or progressive web application with no native packaging intent → treat as [saas.md](saas.md), [dashboard.md](dashboard.md), or the relevant web blueprint.
- Pure marketing / conversion site that happens to be viewed on mobile → [landing-page.md](landing-page.md) or [portfolio.md](portfolio.md).
- Internal operational tool used mainly on desktop with optional mobile access → [dashboard.md](dashboard.md).

If the product is a clear hybrid (native mobile + substantial authenticated web product), treat the **mobile client and its supporting backend** under this blueprint and record the web product surface under the appropriate application blueprint ([saas.md](saas.md), [dashboard.md](dashboard.md), etc.).

## Recommended Starting Shape

A typical modern mobile-first architecture that balances delivery speed, platform quality, and operational sanity for complexity bands S–L:

| Layer | Default posture | Notes |
|-------|-----------------|-------|
| **Client(s)** | Cross-platform framework with strong native interop, or dual native (Swift/Kotlin) when platform depth justifies it | Decide early; switching cost is high |
| **Navigation & UX shell** | Platform-aligned navigation (tabs, stacks, modals) with deliberate shared design language | Respect platform conventions unless brand requires otherwise |
| **Local state & offline** | Local-first or offline-capable data layer with clear sync boundaries | Assume intermittent connectivity; design for it |
| **API boundary** | Versioned HTTP / RPC API (REST, GraphQL, or typed RPC) with auth and tenancy enforcement on the server | Prefer a single coherent backend surface |
| **Identity** | Platform-native auth flows + secure token storage; optional social / passkey / enterprise IdP | See [patterns/authentication.md](../patterns/authentication.md) |
| **Push & device services** | Managed push provider + careful permission UX; use device APIs only when they create clear value | Battery, privacy, and permission fatigue matter |
| **Background work** | Platform background tasks / workers for sync, uploads, and notifications | Never block the UI thread or critical paths |
| **Observability** | Crash reporting, structured logs, performance traces, and user-session context from day one | Mobile support without crash and ANR data is blind |
| **Distribution** | App Store / Play Store (or enterprise / sideload policy) with staged rollouts and crash-free release gates | Release process is part of the architecture |

**Default complexity target:** Medium. The shape above scales to Large with disciplined boundaries; pure dual-native or heavy offline multiplayer shapes push toward Large and require explicit justification.

## Core Decision Points (must be resolved in Architecture Blueprint)

These decisions are high-leverage and expensive to reverse. Record the choice **and** the decisive criteria.

### 1. Client technology strategy
- Cross-platform (React Native, Flutter, Kotlin Multiplatform, etc.) vs. dual native (Swift + Kotlin) vs. progressive web packaged as app.
- Criteria: team skills, required platform APIs, performance/animation needs, long-term maintenance cost, hiring market.
- Whether a single codebase truly serves both platforms or whether platform-specific modules will dominate.
- Link: [technologies/frontend.md](../technologies/frontend.md) (mobile section) and language/runtime notes in [technologies/backend.md](../technologies/backend.md) where relevant.

### 2. Offline, sync & local data model
- Online-only vs. offline-capable vs. local-first.
- Conflict resolution strategy (last-write-wins, CRDTs, server authority, user-mediated).
- What data is cached, what is authoritative on device, and how long it may be stale.
- Storage technology (SQLite / Realm / WatermelonDB / platform equivalents) and encryption-at-rest requirements.

### 3. Identity, sessions & secure storage
- Auth model: password, social, passkeys, magic links, enterprise SSO, or combination.
- Token / session lifetime, refresh strategy, and secure storage (Keychain / Keystore).
- Biometric unlock and device binding expectations.
- Links: [patterns/authentication.md](../patterns/authentication.md), [patterns/authorization.md](../patterns/authorization.md)

### 4. API contract & versioning
- Transport and style (REST, GraphQL, gRPC/Connect, tRPC-style, etc.).
- How mobile clients tolerate backend evolution (versioned endpoints, feature flags, graceful degradation).
- Pagination, real-time channels (WebSocket / SSE / push), and file upload/download patterns.
- Links: [patterns/api-design.md](../patterns/api-design.md), [patterns/real-time.md](../patterns/real-time.md), [patterns/file-uploads.md](../patterns/file-uploads.md)

### 5. Push notifications & device capabilities
- Which notifications are transactional vs. marketing; consent and preference model.
- Required device APIs (camera, location, contacts, biometrics, sensors) and the privacy justification for each.
- Permission request timing and fallback UX when permission is denied.
- Background execution limits and battery impact.

### 6. Multi-tenancy & account model (if applicable)
- User-centric vs. organisation / workspace tenancy.
- How the current tenant or context is selected and bound to the authenticated principal on a mobile device.
- Link: [patterns/multi-tenancy.md](../patterns/multi-tenancy.md)

### 7. Release, distribution & observability
- App-store vs. enterprise distribution; staged rollout and forced-update policy.
- Crash-free session targets, ANR budgets, and performance budgets (startup, navigation, list scroll).
- Feature-flag and remote-config strategy for mobile clients.
- Support tooling needs (session replay boundaries, log collection, impersonation constraints).
- Links: [technologies/observability.md](../technologies/observability.md), [technologies/deployment.md](../technologies/deployment.md)

### 8. Design system & platform adaptation
- Shared design tokens and components vs. fully platform-native look-and-feel.
- Navigation patterns, typography, spacing, and motion language that respect (or intentionally diverge from) platform guidelines.
- Accessibility requirements on both platforms (VoiceOver, TalkBack, dynamic type, reduced motion, contrast).
- Links: [design/](../design/index.md) — especially accessibility-patterns, typography, spacing, motion, color-systems.

## Technology Family Guidance

Prefer coherence, platform quality, and long-term maintainability over novelty. Record the concrete choices and the criteria that selected them.

| Concern | Preferred starting families (2026) | Avoid early unless justified |
|---------|------------------------------------|------------------------------|
| Client framework | Mature cross-platform (React Native / Expo, Flutter) or dual native when depth demands it | Experimental or single-platform-only frameworks for multi-platform products; pure WebView wrappers for core UX |
| Local data & sync | SQLite-based or established offline-first libraries with clear conflict policy | Ad-hoc file storage or unconstrained client-side databases without migration discipline |
| Backend / API | Type-safe or well-versioned HTTP/RPC layer the team already ships reliably | Premature microservices or polyglot explosion for a single mobile product |
| Auth & secure storage | Platform Keychain/Keystore + battle-tested auth library or managed service | Homegrown crypto, storing secrets in plain SharedPreferences / UserDefaults |
| Push | Established providers (APNs + FCM via a managed layer) | Building a custom push infrastructure |
| Observability | Crash + performance + structured logging SDKs with release correlation | Shipping without crash reporting or without mapping crashes to exact builds |
| CI / distribution | Automated builds, signed artifacts, staged rollouts, store metadata as code where possible | Manual release processes that cannot be reproduced or rolled back cleanly |

Deeper criteria live in [knowledge/technologies/](../technologies/index.md) and [knowledge/patterns/](../patterns/index.md).

## Essential Patterns to Apply

Every Mobile App Architecture Blueprint should explicitly reference (and decide against where appropriate) at least:

- [patterns/authentication.md](../patterns/authentication.md) and [patterns/authorization.md](../patterns/authorization.md)
- [patterns/api-design.md](../patterns/api-design.md)
- [patterns/file-uploads.md](../patterns/file-uploads.md) (when media or documents are involved)
- [patterns/real-time.md](../patterns/real-time.md) (when live updates or presence matter)
- [patterns/caching.md](../patterns/caching.md) (client and edge)
- [design/accessibility-patterns.md](../design/accessibility-patterns.md)
- [classification/rendering-strategies.md](../classification/rendering-strategies.md) (for any companion web surface)

Optional but frequent: [patterns/payments.md](../patterns/payments.md) (in-app purchases or subscriptions), [patterns/multi-tenancy.md](../patterns/multi-tenancy.md), [patterns/search.md](../patterns/search.md).

## Recommended Project Skeleton (high-level)

```
/
├── apps/
│   ├── mobile/              # primary mobile client (or ios/ + android/ if dual native)
│   ├── web/                 # optional companion / admin / marketing surface
│   └── api/ or backend/     # shared backend services
├── packages/                # shared types, design tokens, API clients, config
├── tools/ or infra/         # CI, store metadata, release scripts, feature flags
└── ...
```

Exact layout is language- and tooling-dependent. The important invariants are:

- Clear separation between mobile client(s), any web surfaces, and the backend.
- Shared contracts (types, OpenAPI/GraphQL schema, design tokens) that both clients and server can consume.
- A reproducible build and signing pipeline; secrets never live in the client binary.
- Local data migrations and offline behaviour treated as first-class, versioned artefacts.
- Release and observability configuration that maps every crash and metric to a precise build.

## Anti-Patterns

- Choosing a cross-platform framework and then writing the majority of the app in platform-specific modules without revisiting the original decision.
- Treating offline and poor-connectivity scenarios as edge cases instead of core paths.
- Storing tokens or PII in non-secure storage or logging them in clear text.
- Requesting sensitive permissions (location, contacts, tracking) on first launch without clear user value.
- Blocking the main thread or navigation on network calls; ignoring startup-time and frame-rate budgets.
- Shipping without crash reporting, without mapping crashes to exact versions, or without a staged rollout path.
- Building a custom sync engine or conflict-resolution system before validating that simpler server-authoritative approaches are insufficient.
- Ignoring platform accessibility and dynamic-type expectations while polishing only the “happy path” visual design.
- Letting the mobile client become a second, divergent implementation of business rules that already exist on the server.
- Treating app-store review guidelines, privacy nutrition labels, and permission strings as last-minute checklist items.

## Recording Requirements

In the project’s Architecture Blueprint the Architect must record at minimum:

```markdown
## Classification
- Primary type: Mobile / Native-First Application
- Knowledge: knowledge/classification/website-types.md
- Blueprint: knowledge/blueprints/mobile-app.md
- Secondary surfaces (if any): …

## Client Strategy
- Approach: <cross-platform | dual native | …>
- Framework / languages and decisive criteria
- Platform coverage (iOS, Android, others) and minimum OS versions

## Offline & Local Data
- Capability level: <online-only | offline-capable | local-first>
- Sync and conflict policy
- Storage technology and encryption expectations

## Identity & Security
- Auth flows and token / session model
- Secure storage approach
- Biometrics / device binding (if used)

## API & Backend Boundary
- Style and versioning strategy
- Real-time / push channels
- Links to decisive pattern files

## Device Capabilities & Notifications
- Required permissions and justification
- Push strategy and consent model

## Release, Distribution & Observability
- Distribution channels and rollout policy
- Crash / performance / analytics tooling
- Feature-flag / remote-config approach

## Design System & Accessibility
- Shared vs. platform-native posture
- Accessibility baseline commitments (VoiceOver, TalkBack, dynamic type, reduced motion)

## Key Technology Decisions
- Client family + local data approach
- Backend / API family
- Links to the decisive knowledge files and criteria
```

Deviations from this blueprint’s recommendations are allowed; they must be explicit and justified.

## Related Knowledge

- [classification/website-types.md](../classification/website-types.md) — type definition
- [classification/project-complexity.md](../classification/project-complexity.md) — sizing
- [classification/rendering-strategies.md](../classification/rendering-strategies.md) — companion web surfaces
- [classification/architecture-selection.md](../classification/architecture-selection.md)
- [patterns/authentication.md](../patterns/authentication.md), [patterns/authorization.md](../patterns/authorization.md)
- [patterns/api-design.md](../patterns/api-design.md), [patterns/real-time.md](../patterns/real-time.md), [patterns/file-uploads.md](../patterns/file-uploads.md)
- [patterns/caching.md](../patterns/caching.md), [patterns/multi-tenancy.md](../patterns/multi-tenancy.md), [patterns/payments.md](../patterns/payments.md)
- [design/](../design/index.md) — accessibility, motion, typography, spacing, color
- [technologies/frontend.md](../technologies/frontend.md), [technologies/backend.md](../technologies/backend.md), [technologies/deployment.md](../technologies/deployment.md), [technologies/observability.md](../technologies/observability.md)
- [standards/](../standards/index.md) — naming, documentation, testing conventions that apply to the resulting codebase

## Evolution Notes

This blueprint targets the common case of consumer and B2B mobile applications where a native or hybrid client is the primary experience.

When the product is essentially a mobile shell over a full SaaS or dashboard product, keep the mobile client decisions under this blueprint and let the shared product surface follow [saas.md](saas.md) or [dashboard.md](dashboard.md).  
When offline multiplayer, heavy local compute, or deep platform integration (AR, Bluetooth fleets, etc.) become dominant, extend this file with specialised decision points rather than forking it until a recurring specialised shape warrants its own blueprint.  
Companion marketing or documentation sites remain under their respective blueprints; do not force them into the mobile architecture.
