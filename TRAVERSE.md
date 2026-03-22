# 🗺️ TRAVERSE.md — The Traversal Protocol

> _"Let the plan be the path. Let the path be visible before the first step is taken."_

---

## Before You Begin

Read `AGENT.md` completely.
Let its oath settle. Let its covenant be active before you touch a single step.

Then read this file. Understand the map before you enter the territory.

---

## The Seven-Step Architecture Traversal

```
STEP-00  →  STEP-01  →  STEP-02  →  STEP-03
INTAKE      FOUNDATION   DOMAIN       DATA

                                        ↓

STEP-07  ←  STEP-06  ←  STEP-05  ←  STEP-04
ROADMAP     SECURITY     AI-OPS       API+AUTH
```

Each step is a **bounded mission** with:
- a clear purpose
- named inputs (what must exist before you start)
- a generation protocol (what you reason through and produce)
- a self-validation checklist (what you verify before exiting)
- named outputs (what you write to `/context/`)

---

## Traversal Rules

### Rule 1 — Read before you write
Let yourself fully read the step file before generating any output.
Let the shape of the task settle before you begin.

### Rule 2 — Context files are mandatory
After every step, write a context file to `/context/STEP-XX-context.md`.
This is your working memory. Without it, later steps will drift.

### Rule 3 — Carry forward, never repeat
Later steps do not re-explain what earlier steps decided.
They reference the context file and build on it.
Let the architecture be cumulative — not circular.

### Rule 4 — Flag assumptions in context files
Every assumption you make must appear in the context file with the tag `[ASSUMPTION]`.
Every deferred decision must appear with the tag `[DEFERRED]`.
Every known risk must appear with the tag `[RISK]`.

### Rule 5 — Apply first principles at every decision point
When a structural choice arises, trace it back:
- What is the purpose this serves?
- What are the parts and their relationships?
- What constraints apply?
- What is the simplest form that holds under those constraints?

### Rule 6 — Apply the vibe doctrine at every transition
Between steps, pause and ask:
- Have I understood before I acted?
- Is the output clear without decoding?
- Have I named the tradeoffs honestly?
- Does the next step have everything it needs?

### Rule 7 — The agent never blocks
If information is missing, the agent applies the decision protocol from `AGENT.md`,
names the assumption, commits to a direction, and proceeds.
The agent does not stall. The agent does not produce empty sections.

---

## Step Dependency Map

| Step | File | Depends On | Produces |
|------|------|------------|---------|
| 00 | `steps/STEP-00-intake.md` | Nothing | `context/STEP-00-context.md` |
| 01 | `steps/STEP-01-foundation.md` | STEP-00 context | `context/STEP-01-context.md` |
| 02 | `steps/STEP-02-domain.md` | STEP-01 context | `context/STEP-02-context.md` |
| 03 | `steps/STEP-03-data.md` | STEP-02 context | `context/STEP-03-context.md` |
| 04 | `steps/STEP-04-api-auth.md` | STEP-02 + STEP-03 context | `context/STEP-04-context.md` |
| 05 | `steps/STEP-05-ai-ops.md` | STEP-01 + STEP-04 context | `context/STEP-05-context.md` |
| 06 | `steps/STEP-06-security.md` | ALL prior contexts | `context/STEP-06-context.md` |
| 07 | `steps/STEP-07-roadmap.md` | ALL prior contexts | `context/STEP-07-context.md` + final output |

---

## Final Output

After STEP-07, the agent assembles the complete backend architecture documentation set:

```
output/
  README.md
  docs/
    README.md
    backend-architecture.md
    backend/
      overview.md
      domain-schema.md
      database.md
      api-auth.md
      ai-operations.md
      security-operations.md
      roadmap-governance.md
```

This output is implementation-ready. It is not a strategy deck.
It is the structural memory that engineers build from.

---

## Traversal Integrity Check

Before calling the traversal complete, verify:
- [ ] All 8 step context files exist
- [ ] No context file contains empty sections
- [ ] Every `[ASSUMPTION]` tag has a reasoning note
- [ ] Every `[DEFERRED]` tag has a trigger condition for when to revisit
- [ ] Every `[RISK]` tag has a mitigation or acceptance note
- [ ] The final output files cross-reference each other correctly
- [ ] No output document contradicts another

---

_Let this traversal be the skeleton on which the architecture grows._
_Let it be ordered, deliberate, and complete._
_Let every agent who follows this path leave the product more buildable than they found it._
