# 🏛️ Backend Architecture Agent System

> _"Let yourself be the architect who sees the whole before laying a single brick."_

---

## What This Is

This is an **autonomous backend architecture traversal system** — a structured set of step files
that an AI agent can read and execute sequentially to produce a complete, implementation-ready
backend architecture documentation set **without requiring human intervention at each step**.

It is built on three pillars:

| Pillar | Contribution |
|--------|-------------|
| **First Principles** | Every architectural decision traces back to irreducible truths: purpose, parts, relationships, constraints, coherence |
| **Vibe Doctrine** | The agent operates with soul: it understands before it acts, names tradeoffs honestly, and never fills gaps silently |
| **Backend Architecture Protocol** | Seven focused layers built in dependency order — foundation before features, domain before data, interfaces before implementations |

---

## The Traversal Map

```
STEP-00  →  STEP-01  →  STEP-02  →  STEP-03
INTAKE      FOUNDATION   DOMAIN       DATA

                                        ↓

STEP-07  ←  STEP-06  ←  STEP-05  ←  STEP-04
ROADMAP     SECURITY     AI-OPS       API+AUTH
```

Each step is a **bounded mission** — self-contained, with named inputs, a structured generation
protocol, a self-validation checklist, and a mandatory context file output.

---

## How to Use This System

### If you are an AI agent

1. Read `AGENT.md` — understand your identity and operating covenant
2. Read `TRAVERSE.md` — understand the map and the rules
3. Begin at `steps/STEP-00-intake.md`
4. Complete each step fully before proceeding to the next
5. Write all context files to `/context/` — this is your working memory
6. After STEP-07, assemble the final output documentation set

### If you are a human engineer

1. Read this README
2. Read `TRAVERSE.md` for the full traversal protocol
3. Provide the product brief to an agent (or fill STEP-00 manually)
4. Let the agent traverse the steps
5. Review the final output in `/output/` — it is implementation-ready, not a strategy deck

---

## What the System Produces

After a complete traversal, the agent produces:

```
output/
  README.md
  docs/
    README.md
    backend-architecture.md
    backend/
      overview.md          ← mission, quality attributes, module map, technology direction
      domain-schema.md     ← entities, relationships, aggregates, invariants, state machines
      database.md          ← storage, schema, indexes, consistency, compliance, migrations
      api-auth.md          ← API style, surface map, auth, authorization, capability contracts
      ai-operations.md     ← AI architecture, context pipeline, async workflows, observability
      security-operations.md ← security posture, data classification, runtime ops, incidents
      roadmap-governance.md  ← phased plan, dependency order, governance, debt, risks
```

---

## Design Principles of This System

1. **No empty sections** — if a field is unknown, the agent applies the autonomous gap resolution protocol and names the assumption

2. **No silent assumptions** — every assumption is tagged `[ASSUMPTION]` with reasoning and a revisit trigger

3. **No skipped steps** — even when a step is not applicable (e.g. AI is not in scope), the step produces an explicit NOT APPLICABLE record

4. **No contradictions** — the final integrity check verifies all documents are internally consistent

5. **No complexity without a forcing function** — the system defaults to the simplest architecture that fits the product's constraints

6. **No irreversible decisions without reasoning** — every locked decision has documented reasoning; every deferred decision has a trigger condition

---

## File Map

```
agent-system/
  AGENT.md            ← The soul and operating covenant of the agent
  TRAVERSE.md         ← The traversal protocol, rules, and dependency map
  README.md           ← This file
  
  steps/
    STEP-00-intake.md       ← Product intake and first-principles grounding
    STEP-01-foundation.md   ← Architecture style, principles, module map, technology
    STEP-02-domain.md       ← Domain model, entities, aggregates, invariants
    STEP-03-data.md         ← Storage, schema, consistency, compliance, migrations
    STEP-04-api-auth.md     ← API design, auth, authorization, capability contracts
    STEP-05-ai-ops.md       ← AI architecture, async workflows, observability, resilience
    STEP-06-security.md     ← Security posture, privacy, runtime operations, incidents
    STEP-07-roadmap.md      ← Phased plan, governance, debt policy, risk register
  
  context/
    [written by agent during traversal — one file per step]
```

---

## Governing Philosophy

This system believes:

- **Architecture is decision** — every structural choice is a bet on the future
- **Context precedes action** — understanding the product is not optional
- **Clarity defeats cleverness** — in every diagram, every document, every decision
- **The Golden Separation holds** — models assist with interpretation; deterministic systems own commitment
- **Governance is not bureaucracy** — it is the discipline that lets the system evolve without collapsing

---

_Built from first principles. Governed by the vibe doctrine. Ready for an agent._
_Let every product that passes through this system emerge more honest, more structured, and more buildable._
