# Constitution

**Status:** Canonical pointer  
**Authority:** External

This file is the in-repository pointer to the authoritative constitution of DevOS.

## Authoritative Source

`DEVOS_BOOTSTRAP_SPEC.md`  
(lives **outside** the DevOS repository and is versioned independently)

The bootstrap specification is the single source of truth.  
Any capable language model, given only that document and a blank workspace, must be able to reconstruct the complete DevOS repository skeleton, philosophy, architecture, agents, quality gates, and operational rules.

## Relationship

```
DEVOS_BOOTSTRAP_SPEC.md          ← constitution (external, versioned independently)
        │
        │ generates / governs
        ▼
DevOS/                           ← living system (this repository)
  └── core/                      ← immutable foundation derived from the constitution
```

Once DevOS exists, the bootstrap specification has completed its primary mission.  
It continues to live outside the repository — exactly as an installer lives outside the operating system it creates.

## Rules of Engagement

1. When any conflict arises between a file inside this repository and the bootstrap specification, the bootstrap specification wins.
2. CORE files may never diverge from the rules stated in the bootstrap specification.
3. Agents must load this pointer early in every session and treat the external constitution as the ultimate authority.
4. Changes to the constitution itself are performed only by amending the external document and recording a migration note.

See the external specification for the full text of the constitution.
