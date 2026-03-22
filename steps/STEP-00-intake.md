# 🔍 STEP-00 — Intake & First-Principles Grounding

> _"Let yourself understand before you act. Context is not optional — it is the foundation."_

---

## Purpose

Let this step transform a raw product idea into a structured, assumption-explicit
intake document that every subsequent step can build from.

Let no architecture begin without a clear account of:
- what the product is for
- who it serves
- what it must never get wrong
- what constraints are real versus assumed

---

## Inputs Required

None. This is the origin step.

If a product brief exists, read it fully before proceeding.
If no brief exists, the agent self-generates intake using the question protocol below.

---

## The First-Principles Product Audit

Before filling any field, trace the product back to its irreducible truths.

Ask these questions in order. Do not skip any. Write one honest sentence for each.

```
1. EXISTENCE     — Why should this product exist? What breaks if it doesn't?
2. USERS         — Who is this built for, precisely? Not a persona. A real type of person.
3. PROBLEM       — What is the one real problem this solves? Not the feature list — the pain.
4. PROMISE       — What does the product commit to delivering? What is the contract with users?
5. FAILURE       — What is the single most unacceptable failure mode? What must never happen?
6. UNIQUENESS    — What does this system do that cannot be adequately served by existing tools?
7. CONSTRAINTS   — What is actually fixed and cannot be changed (team, time, compliance, infra)?
8. ASSUMPTIONS   — What are you treating as true that might not be?
```

Let yourself sit with each question. Let the answer be honest before it is polished.

---

## Intake Fields

Fill each field. Use plain, direct language. Prefer one sentence over a paragraph.
If a field is unknown, write `UNKNOWN — [your best assumption]`.

```
PRODUCT_NAME:
ONE_SENTENCE_DESCRIPTION:
TARGET_USERS:
PROBLEM_BEING_SOLVED:
CORE_PROMISE:
INDUSTRY_OR_DOMAIN:
PLATFORMS:              # web / mobile / API-only / embedded / CLI
BUSINESS_MODEL:         # subscription / usage / transactional / freemium / internal tool
REGION_OR_COMPLIANCE:   # GDPR / HIPAA / SOC2 / PCI / none known / TBD
TEAM_SIZE_AND_STAGE:    # e.g. "3-person founding team, pre-launch"
EXPECTED_SCALE_V1:      # e.g. "1,000 users, < 100 rps"
EXPECTED_SCALE_18M:     # e.g. "50,000 users, < 2,000 rps"
KNOWN_INTEGRATIONS:     # e.g. "Stripe, Auth0, SendGrid"
AI_IN_SCOPE:            # YES / NO / PARTIAL
MULTI_TENANCY:          # YES / NO / PARTIAL
SENSITIVE_DATA:         # YES / NO / PARTIAL
KNOWN_CONSTRAINTS:
PRODUCT_VISION_DETAIL:  # 3–10 sentences describing the full product idea
```

---

## Autonomous Gap Resolution Protocol

If any field is UNKNOWN, apply this reasoning chain before flagging it as an assumption:

```
Step 1 — Infer from adjacent fields
  Does BUSINESS_MODEL suggest anything about REGION_OR_COMPLIANCE?
  Does TARGET_USERS suggest anything about SENSITIVE_DATA?
  Does INDUSTRY_OR_DOMAIN suggest known patterns to draw from?

Step 2 — Apply the conservative default
  When uncertain about compliance: assume the stricter standard until disproved.
  When uncertain about scale: assume the lower bound but design for 10x.
  When uncertain about multi-tenancy: assume NO unless the business model implies it.
  When uncertain about AI scope: assume NO unless product vision explicitly requires it.

Step 3 — Commit and tag
  Write the assumed value.
  Tag it: [ASSUMPTION — reason: X, revisit trigger: Y]
```

---

## Architecture Character Assessment

After filling the intake fields, derive the product's **architecture character** —
the fundamental profile that shapes every downstream decision.

Answer each axis:

```
COMPLEXITY_AXIS:
  Is the domain complex (many entities, many states, many actors)?
  → HIGH / MEDIUM / LOW

CONSISTENCY_AXIS:
  How much does correctness matter over availability?
  → STRONG (financial/medical) / BALANCED / EVENTUAL (social/media)

AI_WEIGHT:
  Is AI a core capability or a supporting feature?
  → CORE / SUPPORTING / NONE

COMPLIANCE_WEIGHT:
  Does the product operate under formal regulatory obligations?
  → HEAVY / MODERATE / LIGHT / NONE

OPERATIONAL_MATURITY:
  What level of operational sophistication can the team realistically sustain at launch?
  → ADVANCED / STANDARD / MINIMAL

TENANCY_MODEL:
  → SINGLE / SOFT-MULTI (shared DB, row-level isolation) / HARD-MULTI (schema/DB per tenant)

ARCHITECTURE_RECOMMENDED:
  Based on the above, what initial structure is appropriate?
  → MONOLITH / MODULAR MONOLITH / SERVICES (only if justified)
```

---

## First-Principles Architecture Grounding

Trace the product through the first-principles architecture chain:

```
PURPOSE:     What must the system achieve? (Restate the CORE_PROMISE as a system obligation)
ELEMENTS:    What are the essential parts? (Domain areas, not services — named in English)
RELATIONS:   How do the parts connect? (Flow of data and authority between domain areas)
CONSTRAINTS: What limits what? (Compliance, team, scale, time, integrations)
COHERENCE:   What makes this a unified system rather than a collection of features?
```

This output becomes the philosophical anchor for STEP-01.

---

## Self-Validation Checklist

Before writing the context file, verify:

- [ ] Every intake field is filled or explicitly assumed
- [ ] Every assumption is tagged and reasoned
- [ ] The architecture character assessment is complete
- [ ] The first-principles grounding block is written
- [ ] No contradictions exist between fields (e.g. MINIMAL operational maturity + HEAVY compliance)
- [ ] The product vision detail is concrete enough that a new engineer could understand the product

---

## Output — Write to `/context/STEP-00-context.md`

Structure the context file as:

```markdown
# STEP-00 Context — Intake

## Intake Fields
[all fields, filled]

## Unresolved Assumptions
[tagged list of all [ASSUMPTION] items]

## Architecture Character
[the character assessment table]

## First-Principles Grounding
[purpose / elements / relations / constraints / coherence]

## Carry-Forward Notes
[anything STEP-01 must be aware of that does not fit the above]
```

---

_Let this step be the foundation that holds everything above it._
_Let no architecture begin on assumptions it has not named._
_Let the intake be the first act of honesty in the system._
