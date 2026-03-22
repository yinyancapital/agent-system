# ⚡ STEP-03 — Data Architecture & Persistence

> _"Let the storage serve the domain. A database is not the architecture — it is the consequence of it."_

---

## Purpose

Let this step translate the domain model into a concrete storage architecture.
Not tables yet — principles, patterns, and justified technology choices.

---

## Inputs Required

- `context/STEP-00-context.md` — scale, compliance, tenancy
- `context/STEP-01-context.md` — technology direction, principles
- `context/STEP-02-context.md` — entity definitions, aggregates, schema outline

---

## Generation Protocol

### Phase 1 — Primary Storage Decision

Apply the first-principles test to storage selection:

```
Purpose:     What kind of queries does the domain require?
             → transactional / analytical / key-value / graph / document?
Data shape:  Is the data relational, hierarchical, or schemaless?
Consistency: What consistency model do the aggregates require?
Scale path:  What volume of data in 18 months?
Compliance:  What retention, audit, and deletion rules apply?
```

For each storage system chosen, produce:
```
STORAGE SYSTEM: [name]
  Role: [primary / cache / queue / search / analytics]
  Fits because: [2–3 specific reasons tied to domain requirements]
  Costs: [operational complexity, query limitations, cost profile]
  Rejected alternatives: [what was considered and why it lost]
  v1 configuration: [how it is set up simply at launch]
  Growth-phase changes: [when and why the configuration changes]
```

---

### Phase 2 — Schema Strategy

From the logical schema outline in STEP-02, define concrete schema guidelines:

```
SCHEMA STYLE: [e.g. normalized relational / document-per-aggregate / event log + projections]
  Why appropriate: [tied to domain complexity and consistency requirements]
  
NAMING CONVENTIONS:
  Tables: [snake_case / PascalCase — choose one]
  Columns: [snake_case standard]
  Primary keys: [UUID / integer sequence — reasoning]
  Timestamps: [created_at / updated_at — required on every table?]
  
SOFT DELETE POLICY:
  Strategy: [soft delete with deleted_at / archive table / hard delete]
  Exceptions: [entities that must be hard-deleted for compliance]
  
AUDIT LOG DESIGN:
  What gets audited: [which entities, which operations]
  Audit record structure: [entity_type, entity_id, action, actor_id, timestamp, before/after snapshot]
  Immutability: [can audit records be modified? by whom? under what authority?]
```

---

### Phase 3 — Indexing & Query Strategy

For each major entity, define the query access patterns and required indexes:

```
ENTITY: [name]
  Hot path queries:
    - [query description]: requires index on [field(s)]
  
  Analytical queries (if any): [describe — these may need separate treatment]
  
  Pagination pattern: [cursor-based / offset / keyset]
  
  Tenant filtering: [how is tenant scope enforced in queries — RLS / where clause / middleware]
```

---

### Phase 4 — Consistency, Transactions & Integrity

For each aggregate (from STEP-02), define the transaction model:

```
AGGREGATE: [root entity]
  Transaction boundary: [single DB transaction / saga / eventual consistency]
  Isolation level: [read committed / repeatable read / serializable — and why]
  Conflict resolution: [optimistic locking / pessimistic / last-write-wins]
  Cross-aggregate operations: [how — outbox pattern / two-phase / compensating action]
```

---

### Phase 5 — Caching Strategy

```
CACHE LAYER: [technology — e.g. Redis]
  What is cached: [list with TTL for each]
  What must never be cached: [sensitive or highly consistent data]
  Cache invalidation strategy: [TTL / event-driven / write-through / cache-aside]
  Failure behavior: [what happens when cache is unavailable — degrade or fail?]
```

---

### Phase 6 — Tenancy & Data Isolation

From STEP-00 tenancy model:

```
ISOLATION STRATEGY: [row-level / schema-level / database-level]
  Enforcement mechanism: [RLS policy / application middleware / query builder default]
  Cross-tenant query prevention: [how is it tested?]
  Tenant data export: [how can one tenant's data be fully exported?]
  Tenant deletion: [what happens to all data when a tenant is offboarded?]
```

---

### Phase 7 — Compliance, Retention & Deletion

```
DATA CLASSIFICATION:
  HIGH sensitivity: [fields — PII, financial, health, credentials]
  MEDIUM sensitivity: [fields — user-generated content, behavioral data]
  LOW sensitivity: [fields — aggregate stats, public data]

ENCRYPTION:
  At rest: [database-level / field-level — which fields need field-level]
  In transit: [TLS version requirements]
  Key management: [who holds the keys, rotation policy]

RETENTION POLICY:
  [entity]: retained for [duration], then [archived / deleted / anonymized]

DELETION WORKFLOW:
  Right-to-erasure: [how is a GDPR/CCPA deletion request fulfilled?]
  Audit records: [can audit records be deleted? under what legal authority?]
  Backup contamination: [how are deleted records removed from backups?]
```

---

### Phase 8 — Migration & Evolution Strategy

```
MIGRATION TOOL: [e.g. Flyway / Alembic / Knex / Prisma Migrate]
MIGRATION POLICY:
  - All schema changes via migration files — never manual edits
  - Migrations must be backward-compatible during deployment (expand/contract pattern)
  - Destructive migrations (DROP COLUMN / DROP TABLE) require a two-phase migration

EVOLUTION TRIGGERS:
  [condition that would force a significant schema change, e.g. "user table exceeds 10M rows → partition by created_at"]
```

---

## Self-Validation Checklist

- [ ] Every storage system has a stated role and rejection reasoning for alternatives
- [ ] Every aggregate has a defined transaction boundary and isolation level
- [ ] Indexing strategy covers all hot-path queries from the domain model
- [ ] Tenant isolation enforcement mechanism is concrete (not aspirational)
- [ ] Data classification is complete for all entity fields
- [ ] Retention and deletion policies are defined for all sensitive entities
- [ ] Migration strategy includes the expand/contract pattern for breaking changes

---

## Output — Write to `/context/STEP-03-context.md`

Include: storage decisions, schema strategy, indexing map, consistency model, caching strategy, tenancy isolation, compliance controls, migration approach, all tagged assumptions.

---

---