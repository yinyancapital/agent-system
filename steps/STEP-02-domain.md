# 🧭 STEP-02 — Domain Model & Schema Design

> _"Let the data model drive the architecture. A bad model haunts every layer above it."_

---

## Purpose

Let this step define the **language and structure of the backend's core domain**.

Before any table is created, before any endpoint is named, the domain must be understood.
The domain model is the agreement between engineers, the product, and reality.
When it is wrong, everything built on top of it is wrong in the same way.

---

## Inputs Required

- `context/STEP-00-context.md`
- `context/STEP-01-context.md` — especially the module area map and architectural principles

---

## First-Principles Domain Audit

Before modeling entities, trace the product's domain through its basic truths:

```
IDENTITY:    What are the things in this system? (nouns — not tables, not classes — things)
BEHAVIOR:    What do those things do or have done to them? (verbs — lifecycle, transitions)
AUTHORITY:   Who owns each thing? Who can change it? Who cannot?
TRUTH:       Where does the authoritative state of each thing live?
TIME:        How do things change over time? What is the history that matters?
```

This audit prevents the most common domain modeling mistake: modeling the database instead of the domain.

---

## What This Step Produces

1. Ubiquitous language glossary — the terms engineers must agree on
2. Bounded context map — which domain areas own which concepts
3. Entity definitions — purpose, ownership, key fields, constraints, lifecycle
4. Relationship model — how entities relate and depend on each other
5. Aggregate recommendations — what belongs together in one consistency boundary
6. Invariants — truths the system must always maintain
7. State machines — for entities with meaningful lifecycle transitions
8. Logical schema outline — the structural shape of the data, not the SQL

---

## Generation Protocol

### Phase 1 — Ubiquitous Language Glossary

Define every term that appears in the product with precision.
A glossary entry is not a definition from a dictionary. It is the agreed meaning of a term **in this specific product**.

Format:
```
TERM: [name]
  Meaning in this product: [one sentence]
  Distinct from: [common confusion or related term it is not]
  Owner (domain area): [which module area owns this concept]
```

Standard terms to consider (add product-specific terms):
- User / Member / Account / Tenant / Organization / Workspace (clarify which apply)
- Role / Permission / Policy / Claim
- Session / Token / Credential
- [Core domain entity — name it specifically, not generically]
- Audit / Event / Log / Trace (clarify the distinctions)
- Notification / Message / Alert
- Job / Task / Workflow / Process

---

### Phase 2 — Bounded Context Map

From the module areas in STEP-01, assign domain ownership:

```
BOUNDED CONTEXT: [area name from STEP-01]
  Owns these entities: [list]
  Receives from other contexts: [list — what it consumes but does not own]
  Publishes to other contexts: [list — events or data it produces]
  Integration pattern: [direct call / event / shared read model]
```

Anti-pattern to avoid: allowing one entity to be "owned" by two bounded contexts.
Every entity has one authoritative home. Other contexts get projections or events — not authority.

---

### Phase 3 — Entity Definitions

For every major entity, produce a complete definition:

```
ENTITY: [name]
  Purpose: [why this entity exists — one sentence]
  Bounded context owner: [which area owns it]
  
  Key fields:
    - [field]: [type] — [purpose and constraints]
    - [field]: [type] — [purpose and constraints]
  
  Constraints:
    - [invariant that must always hold]
    - [uniqueness rules]
    - [foreign key or relationship rules]
  
  Lifecycle states: [if entity has states]
    [state] → [state]: triggered by [event/action], guarded by [condition]
  
  Creation pattern: [how it is created — by whom, under what conditions]
  Update pattern: [what can change, by whom, under what audit requirements]
  Deletion/archival policy: [hard delete / soft delete / archive / legal hold]
  
  Tenant scope: [global / tenant-scoped / user-scoped]
  Security sensitivity: [HIGH / MEDIUM / LOW — PII, financial, medical, etc.]
  
  Notable relationships:
    - [entity]: [relationship type — owns / belongs-to / many-to-many / event-linked]
```

Standard entities to evaluate (adjust for product):
- User
- Organization / Account / Tenant (if multi-tenant)
- Membership / Role Assignment
- [Product-specific core entity — name it]
- Audit Record
- Notification / Message
- Integration Configuration
- AI Conversation / Generation Record (if AI in scope)
- Job / Background Task (if async workflows exist)

---

### Phase 4 — Relationship Model

Produce a relationship map across all entities.

For each relationship:
```
[Entity A] → [Entity B]
  Type: owns / references / event-triggered / polymorphic
  Cardinality: one-to-one / one-to-many / many-to-many
  Direction of authority: [which side owns the relationship]
  Cascade behavior: [what happens to B when A is deleted/archived]
  Cross-context: YES / NO (if YES — how is the boundary enforced?)
```

---

### Phase 5 — Aggregate Recommendations

An aggregate is a cluster of entities that must change together in one consistent transaction.
Choosing the wrong aggregate boundary is one of the most expensive modeling mistakes.

For each aggregate:
```
AGGREGATE ROOT: [the entity that controls the aggregate]
  Members: [entities inside this aggregate]
  Consistency requirement: [what must be true across all members at all times]
  Transaction boundary: [this aggregate = one database transaction]
  Size concern: [is this aggregate growing in ways that will become a problem?]
```

Apply this test: if two entities must be consistent with each other at all times, they probably belong in the same aggregate.
If they can be eventually consistent, they should be in different aggregates, linked by events or IDs.

---

### Phase 6 — System Invariants

Invariants are truths the system must always maintain.
When an operation would violate an invariant, it must be rejected — not silently ignored.

Format:
```
INVARIANT: [statement of truth that must always hold]
  Enforced by: [database constraint / application rule / event validation]
  Violation consequence: [what breaks if this is ever false]
```

Required categories to cover:
- Identity uniqueness (a user/account cannot exist in two conflicting states)
- Ownership integrity (entities cannot reference owners that do not exist)
- Permission consistency (a role cannot grant permissions the tenant is not entitled to)
- Financial accuracy (if billing — balances must always be reconcilable)
- Audit completeness (if compliance — audit records cannot be deleted or modified)

---

### Phase 7 — State Machines

For every entity with a meaningful lifecycle, produce a state machine:

```
ENTITY: [name]
  States: [list all valid states]
  
  Transitions:
    [from state] → [to state]:
      Triggered by: [event or action]
      Guard condition: [what must be true for the transition to be allowed]
      Side effects: [what else happens when this transition occurs]
      Reversible: YES / NO
  
  Terminal states: [states the entity never leaves]
  Invalid transitions: [combinations that must be explicitly rejected]
```

---

### Phase 8 — Logical Schema Outline

This is not SQL. This is the structural shape of the data — organized by domain area.

```
DOMAIN AREA: [name]
  Tables / Collections:
    [entity_table]:
      Core fields: [list]
      Index candidates: [fields likely to be queried]
      Foreign keys: [relationships]
      Partitioning concern: [will this grow large? how should it be scoped?]
```

Note v1 simplifications explicitly:
```
V1 SIMPLIFICATION: [what is simplified for now]
  Future expansion: [what changes when scale or complexity requires it]
  Trigger: [what condition would force this expansion]
```

---

## Domain Modeling Anti-Patterns — Check Against These

Before completing this step, verify none of these apply:

- **God entity** — one entity that holds too much (User that is also Account, Profile, Preferences, and Billing)
- **Implicit states** — entity has states but they are encoded as boolean flags instead of an explicit state field
- **Anemic model** — entities are just bags of fields with no rules or constraints expressed
- **Cross-context data ownership** — two bounded contexts both believe they own the same entity
- **Missing audit trail** — entities that change state with no record of what changed, when, and by whom
- **Soft-delete without policy** — soft-deleted records that pile up with no archival or purge strategy

---

## Self-Validation Checklist

- [ ] Ubiquitous language glossary covers all product-specific terms
- [ ] Every entity has an identified bounded context owner
- [ ] Every entity definition includes lifecycle states (or explicitly states it is stateless)
- [ ] All aggregate boundaries are stated with consistency requirements
- [ ] All invariants are listed with enforcement mechanisms
- [ ] Every relationship has a direction of authority and cascade behavior defined
- [ ] V1 schema simplifications are noted with future expansion triggers
- [ ] No anti-patterns apply (check each one)

---

## Output — Write to `/context/STEP-02-context.md`

```markdown
# STEP-02 Context — Domain Model

## Ubiquitous Language Glossary
[all terms]

## Bounded Context Map
[all contexts with ownership, inputs, outputs]

## Entity Definitions
[all entities with full definition blocks]

## Relationship Model
[all relationships]

## Aggregates
[all aggregate definitions]

## System Invariants
[all invariants]

## State Machines
[all stateful entities]

## Logical Schema Outline
[by domain area]

## V1 Simplifications
[tagged list with triggers]

## Assumptions & Deferred Decisions
[tagged list]

## Carry-Forward Notes
[anything STEP-03 or STEP-04 must be aware of]
```

---

_Let the domain model be the shared language of the system._
_Let every engineer who reads it know what the words mean._
_Let the model be honest about what exists, not aspirational about what might exist someday._
