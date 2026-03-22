# 🤖 STEP-05 — AI Operations & Async Workflows

> _"Let models assist. Let deterministic systems commit. Let the boundary between them be explicit."_

---

## Purpose

Let this step define how the backend integrates AI capabilities and manages
asynchronous workflows — without letting either become an uncontrolled subsystem.

If AI is NOT in scope (from STEP-00), this step produces a brief `NOT APPLICABLE` record
and completes. Do not skip the step — record the explicit decision.

---

## Inputs Required

- `context/STEP-00-context.md` — AI_IN_SCOPE, async workflow needs
- `context/STEP-01-context.md` — architectural principles, module map
- `context/STEP-04-context.md` — capability contracts, API auth model

---

## Generation Protocol

### Phase 1 — AI Architecture

```
AI SCOPE: [CORE / SUPPORTING / NONE — with one-sentence justification]

AI USE CASES:
  [use case name]:
    Type: [generation / classification / retrieval / synthesis / planning]
    Trigger: [user action / event / scheduled / API call]
    Synchronous or async: [which — and why]
    Data access: [what data the AI may see]
    Data prohibition: [what the AI must never see — explicit list]
    Output: [what form the output takes]
    Confidence required: YES / NO
    Human review gate: [when is a human required before the output is used?]
```

---

### Phase 2 — The Golden Separation in AI

For each AI use case, apply the separation explicitly:

```
USE CASE: [name]
  INTERPRETATION layer: [what the AI does — understands, synthesises, proposes]
  COMMITMENT layer: [what the deterministic system does — validates, authorises, executes]
  
  The AI may NOT: [direct list of actions the AI cannot trigger without deterministic mediation]
  The AI output is: [advisory / structured proposal / data extraction — never a direct command]
```

---

### Phase 3 — Context & Retrieval Architecture

```
CONTEXT PIPELINE:
  Sources: [where context is retrieved from — DB, vector store, cache, documents]
  Access control: [how tenant and user permissions are enforced before retrieval]
  Freshness: [how stale context is detected and rejected]
  Compression: [how context is trimmed to fit token limits without losing fidelity]
  Provenance: [how sources are tracked for attribution and audit]

MEMORY MODEL:
  Conversation memory: [how prior turns are stored and managed]
  Long-term memory: [user preferences, task progress — where stored, how governed]
  Memory limits: [when is old memory purged? by what policy?]
```

---

### Phase 4 — Prompt & Model Governance

```
PROMPT MANAGEMENT:
  Where prompts live: [code / config / database — and the implications of each]
  Versioning: [how prompt versions are tracked and deployed]
  Regression detection: [how a prompt change is validated before production]

MODEL GOVERNANCE:
  Model selection rule: [when to use which model, by capability class]
  Vendor abstraction: [is there an adapter layer so models can be swapped?]
  Fallback: [what happens when the primary model is unavailable or exceeds latency]
  Cost controls: [token budget per request/user/day — how enforced]
```

---

### Phase 5 — Async Workflow Architecture

```
QUEUE/WORKER DESIGN:
  Technology: [e.g. BullMQ, Celery, SQS, Pub/Sub — with reasoning]
  
  Job types:
    [job name]:
      Trigger: [event / schedule / API call]
      Worker: [which service/module runs it]
      Idempotency key: [how duplicates are prevented]
      Retry policy: [max attempts, backoff strategy]
      Dead-letter: [where failed jobs go, who is alerted, how they are replayed]
      Tenant context: [how tenant scope is propagated to the worker]
      Expected duration: [SLA for job completion]

SCHEDULING:
  Scheduled jobs: [list with cron expression and owner module]
  Distributed lock: [how concurrent schedule execution is prevented]
```

---

### Phase 6 — Observability

```
SIGNALS:
  Logs: [structured JSON / what fields are always present: request_id, tenant_id, user_id, duration]
  Metrics: [what is measured — latency, error rate, queue depth, token usage, cost per request]
  Traces: [distributed tracing — which requests are traced end-to-end]
  AI-specific: [prompt version, model, token count, confidence score, retrieval source count]

ALERTING:
  [condition]: alert [who] via [channel] — e.g. "queue depth > 1000 for 5m → on-call via PagerDuty"

COST ACCOUNTABILITY:
  Token spend tracked per: [request / user / tenant / job type]
  Budget enforcement: [how requests are rejected when budget is exceeded]
```

---

### Phase 7 — Resilience & Fallback

```
AI FAILURE MODES:
  [failure]: [what happens — degraded mode, error response, human escalation]

ASYNC FAILURE MODES:
  [failure]: [what happens — retry, dead-letter, compensating action, alert]

CIRCUIT BREAKER:
  Applied to: [external AI API calls, external integrations]
  Open condition: [error rate threshold]
  Recovery: [how the circuit resets]
```

---

## Self-Validation Checklist

- [ ] Every AI use case has explicit data access and prohibition lists
- [ ] The Golden Separation is applied explicitly to each AI use case
- [ ] Context pipeline has access control before retrieval (not just after)
- [ ] Prompt versioning and regression detection are defined
- [ ] Every async job has idempotency key, retry policy, and dead-letter handling
- [ ] Tenant context propagation to workers is defined
- [ ] Observability covers AI-specific signals (token usage, confidence, cost)
- [ ] All failure modes have defined behavior (not undefined)

---

## Output — Write to `/context/STEP-05-context.md`

---

---