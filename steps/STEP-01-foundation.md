# 🏗️ STEP-01 — Architecture Foundation & Principles

> _"Let yourself see the building before you lay the bricks. Let the structure earn its existence."_

---

## Purpose

Let this step establish the architectural foundation that every other step obeys.

This is where the backend gets its **character, its boundaries, and its governing principles**.
It does not produce code. It does not produce database tables.
It produces the **non-negotiable structural decisions** that prevent architecture drift downstream.

---

## Inputs Required

- `context/STEP-00-context.md` — read fully before proceeding

---

## What This Step Produces

1. Backend mission statement — one paragraph, permanently referenceable
2. Quality attributes — ranked by priority for this specific product
3. Architectural style decision — with explicit reasoning and tradeoffs documented
4. Service/module boundary map — named areas, not boxes
5. Deployment topology recommendation — v1 and growth-phase
6. Technology direction — primary choices with tradeoffs stated
7. Architectural principles — the rules that govern every downstream decision
8. Non-negotiables — what the architecture must never violate

---

## Generation Protocol

### Phase 1 — Read the character

From STEP-00 context, extract:
- Architecture character: complexity, consistency, AI weight, compliance weight, operational maturity
- First-principles grounding: purpose, elements, relations, constraints, coherence

Let these drive every choice in this step. Do not introduce concerns that STEP-00 did not surface.

---

### Phase 2 — Write the Backend Mission Statement

One tight paragraph. It must answer:
- What is this backend responsible for?
- Who does it serve (users, agents, integrations)?
- What does it guarantee?
- What does it explicitly not own?

Format:
```
The [PRODUCT_NAME] backend is responsible for [core responsibility].
It serves [actors].
It guarantees [promises].
It does not own [explicit exclusions].
```

This statement is permanent. Every downstream decision should be defensible against it.

---

### Phase 3 — Rank Quality Attributes

Score each attribute 1–5 for this product (5 = highest priority).
Do not default to 5 for everything — forced ranking creates honest priorities.

| Quality Attribute | Score (1–5) | Reasoning |
|-------------------|-------------|-----------|
| Correctness — right answers, always | | |
| Availability — system stays up | | |
| Consistency — data never contradicts itself | | |
| Latency — fast responses | | |
| Throughput — handles volume | | |
| Security — protected from misuse | | |
| Privacy — user data respected | | |
| Observability — system explains itself | | |
| Maintainability — engineers can change it | | |
| Cost efficiency — spending is justified | | |
| Simplicity — complexity has a cost | | |

Top 3 ranked attributes **govern** every architectural tradeoff that follows.
State them explicitly:
```
PRIMARY ATTRIBUTES: [1st], [2nd], [3rd]
THESE WIN when any tradeoff arises.
```

---

### Phase 4 — Architectural Style Decision

Apply the first-principles test:

```
Purpose:     What does the architecture style need to enable?
Constraints: What limits what styles are viable? (team size, operational maturity, scale)
Options:     What are the realistic candidates?
Tradeoffs:   What does each option cost?
Decision:    Which style fits this product at this stage?
```

Candidates to evaluate (eliminate those that do not fit, justify eliminations):

**Monolith**
- Fits when: small team, early stage, domain not yet stable, operational maturity is minimal
- Costs: scaling certain components independently is harder later
- Anti-pattern: never choose because it feels safer — choose because the constraints justify it

**Modular Monolith**
- Fits when: domain has clear bounded contexts, team is growing, operational maturity is standard
- Costs: requires module boundary discipline; breaks apart into services later if needed
- Recommended default: this is the right answer for most serious early-stage products

**Microservices**
- Fits when: independent deployment velocity is a real requirement, team has operational maturity, domain is stable and well-understood
- Costs: distributed systems complexity, network latency, operational overhead
- Anti-pattern: never choose because it is scalable in theory — only choose if the forcing function is real

State the decision with full reasoning:
```
ARCHITECTURAL STYLE: [chosen style]
REASONING: [why this fits the product's constraints and attributes]
TRADEOFFS ACCEPTED: [what this style costs]
EVOLUTION PATH: [when and why would this change, and what would trigger that]
```

---

### Phase 5 — Module / Service Boundary Map

Name the **domain areas** of the backend. These are not services yet — they are bounded contexts.

For each area, answer:
- What is this area responsible for?
- Who owns the data in this area?
- What does this area expose to other areas?
- What must this area never reach into from another area?

Format:
```
AREA: [name]
  Responsibility: [one sentence]
  Owns: [data it controls]
  Exposes: [what other areas can use]
  Must never: [what it cannot do or know]
```

Standard areas to evaluate (add or remove based on STEP-00):
- Identity & Auth
- Tenancy & Accounts (if multi-tenant)
- Core Domain (product-specific — name it precisely)
- AI / Intelligence (if in scope)
- Notifications
- Integrations
- Billing (if applicable)
- Audit & Compliance
- Operations & Telemetry

---

### Phase 6 — Deployment Topology

Answer: what does the running system look like?

```
V1 TOPOLOGY:
  Where does it run? (cloud provider / region)
  What runtime model? (containers / serverless / VMs)
  What does a single deploy unit contain?
  What is the database topology?
  What is the cache topology?
  What is the queue/messaging topology?

GROWTH TOPOLOGY (18 months):
  What changes first under load?
  What new infrastructure appears?
  What gets extracted if it needs to?
```

Apply the vibe principle: let complexity earn its existence.
Do not add infrastructure components the product does not need yet.

---

### Phase 7 — Technology Direction

For each category, state the primary recommendation and the rejected alternatives with reasons.

| Category | v1 Recommendation | Why | Rejected Alternatives |
|----------|-------------------|-----|-----------------------|
| Primary language/runtime | | | |
| Web framework | | | |
| Primary database | | | |
| Cache | | | |
| Queue/messaging | | | |
| Auth | | | |
| Hosting/cloud | | | |
| Container orchestration | | | |
| Observability | | | |
| AI model access | | (if in scope) | |

Apply the conservative default: choose boring, proven technology unless there is a specific forcing function for something newer.

---

### Phase 8 — Architectural Principles

State 6–9 principles that govern every downstream decision.
Each principle must have:
- a name
- a rule (imperative sentence)
- a consequence (what violating it causes)

Format:
```
PRINCIPLE: [name]
  Rule: [imperative — always/never/prefer]
  Violation consequence: [what breaks if this is ignored]
```

Required principles to consider:
- The Golden Separation (interpretation vs commitment — models advise, deterministic systems decide)
- Single Source of Truth (every piece of data has one authoritative home)
- Explicit over Implicit (configuration, contracts, and errors are never hidden)
- Fail Safe (the default behavior under failure must be safe, not undefined)
- Least Privilege (every component has the minimum access it needs to function)
- Observability as a First Citizen (if you cannot measure it, you cannot govern it)
- Reversibility (prefer structural decisions that can be undone)

Add product-specific principles based on STEP-00 character.

---

### Phase 9 — Non-Negotiables

List the architectural commitments that must never be broken regardless of schedule pressure, feature urgency, or technical convenience.

These are the **hard constraints** of the system. Violating them creates systemic risk.

```
NON-NEGOTIABLE: [statement]
  Reason: [why this is a hard constraint, not a preference]
  Enforcement: [how it is enforced — tests, code review, ADR, policy]
```

---

## Self-Validation Checklist

- [ ] Backend mission statement is written and defensible
- [ ] Quality attributes are ranked (not all 5s)
- [ ] Architectural style has explicit reasoning and stated tradeoffs
- [ ] All module areas are named with ownership and boundary rules
- [ ] v1 and growth topology are both described
- [ ] Every technology choice has rejected alternatives documented
- [ ] At least 6 architectural principles are stated with violation consequences
- [ ] Non-negotiables are listed with enforcement mechanisms
- [ ] No contradictions exist between quality rankings and architectural choices

---

## Output — Write to `/context/STEP-01-context.md`

```markdown
# STEP-01 Context — Architecture Foundation

## Backend Mission Statement
[statement]

## Primary Quality Attributes
[ranked top 3 + full table]

## Architectural Style Decision
[decision + full reasoning block]

## Module / Domain Area Map
[all areas with ownership and boundary rules]

## Deployment Topology
[v1 and growth-phase]

## Technology Direction
[full table]

## Architectural Principles
[all principles]

## Non-Negotiables
[all non-negotiables with enforcement]

## Assumptions & Deferred Decisions
[tagged list]

## Carry-Forward Notes
[anything STEP-02 must be aware of]
```

---

_Let this foundation be the law of the architecture._
_Let every future decision trace back to this step._
_Let the principles be strong enough to survive the pressure of a deadline._
