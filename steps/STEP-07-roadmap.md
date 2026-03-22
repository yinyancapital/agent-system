# 🗺️ STEP-07 — Roadmap, Governance & Evolution

> _"Let the roadmap be honest about what is now, what is next, and what must be earned before it can exist."_

---

## Purpose

Let this step translate everything built in STEP-01 through STEP-06 into a phased,
risk-ordered build sequence with governance controls that prevent architecture drift over time.

A roadmap is not a wish list. It is a structured commitment:
- what to build first, and why
- what to defer, and what the trigger is to revisit
- how decisions are made, recorded, and enforced
- how the architecture stays coherent as the team and product grow

---

## Inputs Required

All prior context files.

---

## Generation Protocol

### Phase 1 — Phased Build Sequence

Apply the four-phase adoption model, grounded in this product's specific risk profile:

```
PHASE 1 — Foundation (v1 Launch)
  Scope: [what gets built — specific capabilities, not platitudes]
  Excludes: [explicit list of what is NOT built yet]
  Risk this phase removes: [what failure mode is eliminated by completing this phase]
  Quality gate: [what must be true before moving to Phase 2]
  Governing rule: [e.g. "Models advise; deterministic systems commit"]

PHASE 2 — Orchestration
  Scope: [what gets built]
  Prerequisite: [what Phase 1 must have produced]
  Risk this phase removes: [...]
  Quality gate: [...]
  Governing rule: [...]

PHASE 3 — Platform
  Scope: [capability surfaces, shared infrastructure, agent-ready APIs]
  Prerequisite: [...]
  Risk this phase removes: [...]
  Quality gate: [...]
  Governing rule: [...]

PHASE 4 — Agentic Operations (only if scope justifies it)
  Scope: [constrained autonomy in well-bounded domains]
  Prerequisite: [...]
  Governing rule: ["Grant autonomy gradually, never romantically"]
```

---

### Phase 2 — Dependency Ordering

```
DEPENDENCY CHAIN:
  [capability A] must exist before [capability B] because [reason]

CRITICAL PATH:
  [ordered list of capabilities that block the most downstream work]

PARALLEL TRACKS:
  [capabilities that can be built simultaneously without blocking each other]
```

---

### Phase 3 — Locked vs Deferred Decisions

```
LOCK NOW — these decisions are made and must not be revisited without an ADR:
  [decision]: [reasoning for locking]

DEFER INTENTIONALLY — these are not decided yet, and that is correct:
  [decision]: 
    Why deferred: [reason]
    Trigger to revisit: [specific condition — e.g. "when DAU exceeds 50,000"]
    Risk of waiting: [what gets harder if this is deferred too long]

ASSUMPTIONS TO VALIDATE:
  [assumption]: validated by [experiment / user data / technical spike]
```

---

### Phase 4 — Architecture Governance Model

```
DECISION RECORD POLICY:
  What requires an ADR: [all architectural decisions / only those affecting service boundaries / ...]
  ADR template: [location and required fields]
  Review process: [who reviews, what threshold requires consensus]

ARCHITECTURE REVIEW:
  Cadence: [weekly / monthly / before each phase gate]
  Participants: [who must attend]
  Inputs: [what is reviewed — new ADRs, tech debt register, metric trends]

QUALITY GATES:
  Before each phase:
    [ ] All ADRs from the phase are approved
    [ ] All non-negotiables are verified by tests or policy
    [ ] Tech debt register is reviewed
    [ ] Security review is complete
    [ ] Observability is validated (can you debug a production incident?)
```

---

### Phase 5 — Technical Debt Policy

```
DEBT CLASSIFICATION:
  INTENTIONAL — taken deliberately with a named resolution plan:
    [item]: [why taken, when resolved, who owns resolution]
  
  DISCOVERED — found in existing code:
    [item]: [severity, risk, resolution plan]
  
REFACTOR TRIGGERS:
  [condition] → [refactor action]
  — e.g. "Module X exceeds 50% of bug reports → decompose into focused services"
  — e.g. "Query latency p99 > 500ms → introduce read replica and optimize indexes"

DEBT REVIEW CADENCE:
  [frequency] — reviewed by [who] — output: [what decisions are made]
```

---

### Phase 6 — Risk Register

```
RISK: [name]
  Category: [technical / operational / compliance / product]
  Likelihood: [HIGH / MEDIUM / LOW]
  Impact: [HIGH / MEDIUM / LOW]
  Current mitigation: [what is in place now]
  Residual risk: [what remains after mitigation]
  Owner: [who monitors this risk]
  Trigger: [what condition would escalate this risk]
```

---

### Phase 7 — Success Criteria

For each phase, define what "done" looks like — not in features, but in measurable outcomes:

```
PHASE [N] SUCCESS:
  Task completion quality: [how measured]
  Factual grounding rate: [for AI capabilities — how measured]
  Unsafe action prevention: [how measured]
  Policy adherence rate: [how measured]
  Cost per successful outcome: [how measured]
  Mean time to recovery: [target]
  Operator debugging time: [target — a proxy for observability quality]
```

---

## Final Assembly — Complete Architecture Output

After STEP-07 context is written, assemble the full documentation set:

```
output/
  README.md             ← product overview + who this is for + how to use this docs set
  docs/
    README.md           ← navigation hub + reading paths
    backend-architecture.md  ← entry point, reading order, principles index
    backend/
      overview.md       ← mission, quality attributes, module map, technology direction
      domain-schema.md  ← ubiquitous language, entities, relationships, aggregates, invariants
      database.md       ← storage decisions, schema strategy, indexes, consistency, compliance
      api-auth.md       ← API style, surface map, auth, authorization, capability contracts
      ai-operations.md  ← AI architecture, context pipeline, async workflows, observability
      security-operations.md ← security posture, data classification, runtime ops, incident response
      roadmap-governance.md  ← phased plan, dependency order, governance, debt policy, risk register
```

Each document is assembled from its corresponding context file, written in plain engineering prose.
No section is left as a template placeholder. No section uses vague language.
Every claim is either a decision with reasoning or an assumption with a tag.

---

## Final Traversal Integrity Check

- [ ] All 8 context files exist and are complete
- [ ] No document contradicts another
- [ ] Every `[ASSUMPTION]` has reasoning and a revisit trigger
- [ ] Every `[DEFERRED]` has a trigger condition
- [ ] Every `[RISK]` has a mitigation and an owner
- [ ] Non-negotiables are enforced by mechanism, not intent
- [ ] Phase quality gates are measurable (not "feels ready")
- [ ] The output can be handed to an engineer with no prior context and they can build from it

---

_Let this roadmap be the commitment made before the first commit is pushed._
_Let the governance model survive the moment when the team grows faster than the architecture._
_Let the architecture outlive the circumstances that created it._