# Website / Product Types

**Status:** Active  
**Confidence:** High  
**Last review:** 2026-08-05  
**Primary consumers:** Architect, Planner (orientation), Tech Lead (sizing)

Canonical categories for the primary product or site under design.  
A project receives **one primary type**. Secondary characteristics may be noted but do not replace the primary label.

## Decision Process

1. Read the Project Brief and Requirements for stated goals, users, and content/interaction model.
2. Match against the criteria tables below.
3. Prefer the most specific type that fully covers the core value proposition.
4. If the product is a clear hybrid (e.g. SaaS + heavy marketing site), choose the type that drives the majority of engineering risk and record the secondary type as a note.
5. Record the chosen type and the decisive criteria in Architecture Blueprint §1.

## Type Catalogue

### 1. Landing / Marketing Page

**Definition:** Single-purpose or short multi-page site whose primary goal is conversion, lead capture, or brand presentation. Little or no authenticated application state.

**Strong signals**
- Primary success metric is conversion rate, sign-ups, or demos booked
- Content is largely static or CMS-driven
- Forms are lead-gen or contact only
- No persistent user accounts or complex workflows

**Weak / counter signals**
- Multi-step authenticated product experience
- Real-time collaboration or dashboards as core value

**Typical related blueprints:** landing-page, portfolio (when personal brand is the product)

---

### 2. Portfolio / Brochure

**Definition:** Showcase of work, people, or offerings. Emphasis on visual presentation and storytelling rather than transactional flows.

**Strong signals**
- Gallery, case-study, or project-list oriented
- Content updates are infrequent and editorial
- Call-to-action is contact or “view more”

**Typical related blueprints:** portfolio, documentation-site (when the “product” is knowledge)

---

### 3. Documentation / Content Site

**Definition:** Primary value is organised, searchable, versioned content (docs, knowledge base, blog, learning materials).

**Strong signals**
- Hierarchical or tag-based information architecture
- Search is a first-class feature
- Content lifecycle (draft → review → publish) matters
- Versioning or multi-language content is common

**Typical related blueprints:** documentation-site

---

### 4. Dashboard / Internal Tool

**Definition:** Authenticated interface for viewing metrics, managing operational data, or performing internal workflows. Users are known (employees, operators, partners).

**Strong signals**
- Authentication required for almost all screens
- Data tables, filters, charts, and CRUD dominate
- Performance and correctness of data presentation are critical
- Often multi-tenant or role-based

**Typical related blueprints:** dashboard

---

### 5. SaaS Application

**Definition:** Multi-user product delivered as a service. Core value is ongoing use of features under an account, usually with subscription or usage billing.

**Strong signals**
- User accounts, organisations / workspaces, roles
- Persistent application state and workflows
- Billing, plans, or usage metering present or planned
- Onboarding, retention, and feature adoption matter

**Typical related blueprints:** saas

---

### 6. E-commerce / Marketplace

**Definition:** Primary flows are product discovery, cart, checkout, and order management. May include seller-side tooling (marketplace).

**Strong signals**
- Catalogue, pricing, inventory, cart, payment, fulfilment
- Strong SEO and conversion optimisation needs
- Order lifecycle and customer support tooling

**Typical related blueprints:** ecommerce

---

### 7. Mobile / Native-First Application

**Definition:** Primary client is a native or hybrid mobile app. Web may exist as companion or admin surface.

**Strong signals**
- App-store distribution, push notifications, device APIs, offline-first requirements
- Design language and interaction patterns oriented to mobile platforms

**Typical related blueprints:** mobile-app

---

### 8. Hybrid / Platform

**Definition:** Product that deliberately combines two or more of the above at comparable importance (e.g. public marketing + authenticated SaaS + partner portal).

**Use only when** no single type accounts for the majority of engineering risk and user value.  
Always list the constituent types and which one drives the primary architecture decisions.

## Decision Table (quick reference)

| If the dominant need is…                  | Prefer type              |
|-------------------------------------------|--------------------------|
| Convert visitors / capture leads          | Landing / Marketing      |
| Showcase work or brand                    | Portfolio / Brochure     |
| Organise & search knowledge               | Documentation / Content  |
| Operate on internal data & metrics        | Dashboard / Internal Tool|
| Deliver ongoing product features under account | SaaS Application    |
| Sell physical or digital goods            | E-commerce / Marketplace |
| Native mobile experience first            | Mobile / Native-First    |
| Multiple of the above at similar weight   | Hybrid / Platform        |

## Output Expectations for Agents

In Architecture Blueprint §1 Classification, record at minimum:

```markdown
- **Primary type:** <Type name>  
  - Criteria matched: …  
  - Knowledge: knowledge/classification/website-types.md  
- **Secondary notes:** (optional)
```

If classification is ambiguous, list the candidate types and the open question that would resolve the choice.
