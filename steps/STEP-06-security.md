# 🔒 STEP-06 — Security, Privacy & Runtime Operations

> _"Let security be designed in — not bolted on. Let operations be observable before they are needed."_

---

## Purpose

Let this step define the security posture, privacy controls, and operational model
that make the backend trustworthy in production.

Security is not a feature. It is the consequence of how everything else is built.
This step checks that everything built in STEP-01 through STEP-05 holds under pressure.

---

## Inputs Required

All prior context files (STEP-00 through STEP-05).

---

## Generation Protocol

### Phase 1 — Security Principles & Threat Model

```
THREAT MODEL:
  Who is the adversary? [external attacker / malicious insider / compromised dependency / misconfigured operator]
  What are they after? [user PII / financial data / system access / service disruption]
  What is the most likely attack vector? [credentials / injections / over-privileged API / supply chain]

SECURITY POSTURE:
  Default stance: [deny by default — no access unless explicitly granted]
  Defense depth: [how many independent controls must be bypassed to reach sensitive data]
```

---

### Phase 2 — Data Classification & Sensitive Data Handling

From STEP-03, refine the data classification into concrete handling rules:

```
CLASS: HIGH SENSITIVITY
  Fields: [list — credentials, PII, financial, health]
  Storage: [encrypted at field level / separate table / vault]
  Transit: [TLS 1.3 minimum / mutual TLS for internal services]
  Logging: [NEVER log these fields — explicit prohibition]
  Access: [who can read — minimum necessary principle]

CLASS: MEDIUM SENSITIVITY
  Fields: [list]
  Handling: [...]

CLASS: LOW SENSITIVITY
  Fields: [list]
  Handling: [standard — no special controls]
```

---

### Phase 3 — Secrets & Environment Management

```
SECRETS MANAGEMENT:
  Tool: [e.g. AWS Secrets Manager / HashiCorp Vault / environment variables at runtime]
  
  Rotation policy:
    [secret type]: rotated every [duration], rotation is [automated / manual]
  
  Never in code: [list what must never appear in source — API keys, DB passwords, signing secrets]
  Never in logs: [list what must never appear in log output]
  
ENVIRONMENT SEPARATION:
  Environments: [development / staging / production — what each contains and who can access]
  Config promotion: [how config moves from dev to prod without human error]
  Production access: [who can access production, how it is gated, how it is logged]
```

---

### Phase 4 — Audit & Compliance Controls

```
AUDIT LOG:
  Captured events: [all sensitive actions — list the categories]
  Required fields per event: [actor_id, action, resource_type, resource_id, timestamp, ip, result]
  Immutability: [how audit records are protected from modification]
  Retention: [how long audit records are kept — and why that duration]
  Access: [who can query audit logs — and under what authority]

COMPLIANCE CONTROLS:
  [regulation — e.g. GDPR]:
    Requirements: [list]
    Implementation: [how each is met — specific, not generic]
    Gaps at v1: [what is not yet implemented and the plan to close it]
```

---

### Phase 5 — Runtime Operations Model

```
ENVIRONMENTS:
  [env name]: [purpose, who deploys to it, what data it contains, who can access it]

DEPLOYMENT:
  Process: [CI/CD pipeline — what triggers a deploy, what gates exist]
  Rollback: [how a bad deploy is reversed in < 10 minutes]
  Feature flags: [how new features are enabled incrementally without code deploys]

ON-CALL:
  Rotation: [who is on call, what escalation path exists]
  Runbooks: [what runbooks must exist before launch — list the scenarios they cover]
  Incident severity: [S1/S2/S3 definition for this product]

SLOs:
  [capability]: availability = [%], latency p99 = [ms], error rate < [%]
  Error budget policy: [what happens when error budget is consumed]
```

---

### Phase 6 — Vulnerability & Dependency Management

```
DEPENDENCY POLICY:
  Scanning: [tool + frequency — e.g. Dependabot / Snyk / weekly]
  Critical vulnerability SLA: [patched within X days of disclosure]
  License compliance: [what licenses are prohibited]

INFRASTRUCTURE HARDENING:
  [control]: [how it is implemented]
  — e.g. No public database endpoints
  — e.g. All inter-service communication uses service accounts, not shared credentials
  — e.g. Container images scanned before deployment
```

---

### Phase 7 — Incident Response

```
INCIDENT DETECTION:
  [signal]: [alert → response]

INCIDENT RESPONSE:
  Detection → Acknowledgment: < [duration]
  Acknowledgment → Mitigation: < [duration]
  Mitigation → Resolution: < [duration]
  Post-mortem: [required for S1/S2, optional for S3 — template exists?]

BREACH PROTOCOL:
  Step 1: [contain]
  Step 2: [assess scope]
  Step 3: [notify — who, in what timeframe, regulatory obligation?]
  Step 4: [remediate]
  Step 5: [document and improve]
```

---

## Self-Validation Checklist

- [ ] Threat model names specific adversaries and attack vectors (not generic)
- [ ] Every HIGH sensitivity field has field-level encryption or equivalent
- [ ] Secrets rotation is automated for all critical credentials
- [ ] Audit log covers all sensitive actions with immutability protection
- [ ] Every compliance requirement maps to a specific implementation (not "we will comply")
- [ ] Rollback procedure is defined with a time target
- [ ] Incident response SLAs are defined and owned

---

## Output — Write to `/context/STEP-06-context.md`

---

---