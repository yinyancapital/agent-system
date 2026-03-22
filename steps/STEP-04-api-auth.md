# 🔌 STEP-04 — API Design, Auth & Authorization

> _"Let the interface be the contract. Let the contract be discoverable, constrained, and honest."_

---

## Purpose

Let this step define how the backend exposes its capabilities — to humans, agents, and integrations.
An API is not a list of endpoints. It is a set of contracts that communicate what the system can do,
for whom, under what conditions, with what guarantees.

---

## Inputs Required

- `context/STEP-00-context.md`
- `context/STEP-01-context.md`
- `context/STEP-02-context.md`
- `context/STEP-03-context.md`

---

## Generation Protocol

### Phase 1 — API Style Decision

```
STYLE DECISION: [REST / GraphQL / gRPC / hybrid]
  Reasoning: [tied to client types, team capability, tooling ecosystem]
  Rejected alternatives: [with reasons]

VERSIONING STRATEGY:
  Approach: [URL versioning /v1/ / header versioning / no versioning with evolution rules]
  Breaking change policy: [what constitutes a breaking change, what requires a new version]

ERROR MODEL:
  Format: [JSON structure for all errors]
  Required fields: [code, message, request_id, timestamp — what is mandatory]
  Sensitive information: [what must never appear in error responses]
```

---

### Phase 2 — API Surface Organisation

For each domain area (from STEP-01), define the API surface:

```
DOMAIN AREA: [name]
  API ownership: [which module/service owns these routes]
  
  Resource groups:
    [resource]: [CRUD operations that exist — not all CRUD is required]
      GET /[resource] — [purpose, auth requirement, pagination type]
      GET /[resource]/:id — [purpose, auth requirement]
      POST /[resource] — [purpose, auth requirement, idempotency key required?]
      PATCH /[resource]/:id — [purpose, what fields are patchable]
      DELETE /[resource]/:id — [purpose, soft or hard delete?]
  
  Command endpoints (actions that are not CRUD):
    POST /[resource]/:id/[action] — [what it does, why it is a command not a CRUD op]
  
  Query endpoints (complex reads that are not simple GET):
    GET /[resource]/search — [parameters, auth, pagination]
```

---

### Phase 3 — Authentication Architecture

```
AUTH MODEL: [JWT / session cookie / API key / OAuth2 — specify]

TOKEN STRATEGY:
  Access token: TTL = [value], claims included = [list], signing algorithm = [e.g. RS256]
  Refresh token: TTL = [value], rotation policy = [rotating / absolute], storage = [httpOnly cookie / DB]
  
SESSION MANAGEMENT:
  Where state lives: [stateless JWT / stateful server session / hybrid]
  Invalidation: [how is a token revoked immediately if needed?]

SPECIAL AUTH FLOWS:
  [flow]: [description — e.g. magic link, SSO, step-up MFA, break-glass admin]
```

---

### Phase 4 — Authorization Architecture

```
AUTHORIZATION MODEL: [RBAC / ABAC / policy-based — specify]

ACTOR TYPES:
  [actor]: [what they are, what they can do at maximum, what they can never do]

ROLE HIERARCHY:
  [role] → [role]: [parent role grants all child role permissions?]

PERMISSION EVALUATION RULE:
  [state the algorithm — e.g. "deny by default; evaluate role permissions; evaluate resource ownership; evaluate policy overrides"]

RESOURCE OWNERSHIP:
  [resource]: owned by [actor type], modifiable by [actor types], readable by [actor types]

TENANT ADMIN POWERS:
  [what a tenant admin can do within their tenant]
  [what a tenant admin can never do across tenants]

OPERATOR ACCESS:
  Internal operator access: [what can support/ops access without breaking tenant isolation?]
  Audit requirement: [every operator action is logged? how?]
  Break-glass: [how does an operator access a tenant in emergency? what is the audit trail?]
```

---

### Phase 5 — Request & Response Design Principles

```
IDEMPOTENCY:
  Which operations require an idempotency key: [list]
  How is idempotency enforced: [client-provided key stored in DB with result]

PAGINATION:
  Standard: [cursor / offset — choose one for consistency]
  Response envelope: { data: [], cursor: "", total: null | number }

CONCURRENCY CONTROL:
  Optimistic: [ETag / If-Match header / version field — for which resources]
  Conflict response: [409 Conflict with current version]

FILTERING & SORTING:
  Allowed filter fields: [whitelist, never allow arbitrary field filtering]
  Sort fields: [whitelist]
  Dangerous query prevention: [how are unbounded queries rejected]
```

---

### Phase 6 — Capability Contracts

For AI-native or agent-serving APIs (if AI is in scope), define capability contracts:

```
CAPABILITY: [name]
  Mode: [DETERMINISTIC / COGNITIVE / ORCHESTRATED]
  Description: [what this capability does in plain language]
  Required permissions: [actor and role requirements]
  Confidence output: YES / NO
  Provenance output: YES / NO
  Side effects: [what it may change — explicit]
  Approval threshold: [confidence level above which it acts autonomously]
  Human escalation: [when and how it escalates to human review]
  Audit requirement: [what is logged]
```

This is the Golden Separation in practice: suggestion capabilities never execute; execution capabilities never synthesize.

---

### Phase 7 — Integration & Webhook Design

```
WEBHOOKS (if applicable):
  Event types exposed: [list]
  Payload structure: [event_type, timestamp, data, signature]
  Delivery guarantee: [at-least-once / at-most-once]
  Retry policy: [exponential backoff, max attempts, dead-letter]
  Secret rotation: [how webhook signing secrets are rotated without downtime]

EXTERNAL INTEGRATIONS:
  [integration]: [what data flows in/out, what auth it uses, what happens if it goes down]
```

---

## Self-Validation Checklist

- [ ] API style decision has explicit reasoning and rejected alternatives
- [ ] Every domain area has defined API surface with auth requirements
- [ ] Auth model covers token lifecycle, invalidation, and special flows
- [ ] Authorization covers all actor types with explicit permission evaluation algorithm
- [ ] Idempotency is defined for all non-idempotent operations
- [ ] Capability contracts are defined for all AI-serving endpoints (if in scope)
- [ ] No endpoint returns more data than the requestor is authorized to see
- [ ] Error model never leaks sensitive information

---

## Output — Write to `/context/STEP-04-context.md`

Include: API style, versioning, error model, surface map, auth architecture, authorization model, request/response principles, capability contracts, integration design, all tagged assumptions.

---

---