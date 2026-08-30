# ALARMv4 Wiki — Searchable Knowledge Base

**Status**: Phase α (Planning)  
**Last Updated**: 2026-08-30  
**Purpose**: Persistent, evolving institutional knowledge for ALARMv4

---

## Quick Navigation

### 📋 Getting Started
- **New to ALARMv4?** → Start with `README.md` + `CLAUDE.md` (in repo root)
- **Want to understand the vision?** → `docs/0_THESIS.md`
- **Curious about history?** → `docs/1_RETROSPECTIVE.md`

### 🧠 Core Concepts
- [System 2 Framework](gates/system_2_debate.md) — Continuous org learning
- [Expert Personas](personas/index.md) — Who makes decisions
- [Gate Architecture](gates/index.md) — How stages transition
- [Moltbook Swarms](agents/moltbook_dynamics.md) — Organic agent organization
- [Semantic DB](schemas/firestore_schema.md) — Centralized storage

### 🛠️ Building ALARMv4
- [Agent Protocol](agents/agent_protocol.md) — How agents communicate
- [Stem Cell Lifecycle](agents/stem_cell_protocol.md) — Birth → specialization → death
- [Slime Mold Network](agents/slime_mold_routing.md) — Adaptive topology
- [Confidence Scoring](gates/confidence_scoring.md) — Measuring certainty

### 📊 Data & Storage
- [Firestore Schema](schemas/firestore_schema.md) — Session, gate, decision data
- [Guidance Ledger Schema](schemas/guidance_ledger_schema.md) — Policy versioning
- [Handoff Protocol](schemas/handoff_protocol.md) — Agent → agent messages

### 🎯 Decisions & Rationale
- [Why GCP?](decisions/why_gcp.md) — Platform choice
- [Why Firestore + Vertex?](decisions/why_firestore_vertex.md) — Storage choice
- [Why System 2 First?](decisions/why_system2_first.md) — Priority rationale
- [Why Moltbooks?](decisions/why_moltbooks.md) — Architecture choice

### 📖 Patterns & Best Practices
- [Gate Evaluation Pattern](patterns/gate_evaluation.md) — How to implement a gate
- [Confidence Scoring Pattern](patterns/confidence_scoring.md) — How to measure progress
- [Adversarial Review Pattern](patterns/adversarial_review.md) — How to challenge decisions
- [Expert Persona Pattern](patterns/expert_persona.md) — How to build a learnable expert

### 📚 Runbooks (Phase β+)
- [System 2 Refresh Cycle](runbooks/system2_refresh.md) — How debate cycle works
- [Moltbook Spawn](runbooks/moltbook_spawn.md) — How to create a stage
- [Gate Validation](runbooks/gate_validation.md) — How to test a gate
- [Debugging Confidence](runbooks/debug_confidence.md) — Why is confidence stuck?

---

## The Knowledge Hierarchy

```
DECISIONS (Why we chose this direction)
  ↓
PATTERNS (How to implement that direction)
  ↓
SCHEMAS (What data structures support it)
  ↓
RUNBOOKS (Step-by-step operational guides)
  ↓
TROUBLESHOOTING (When things go wrong)
```

Start at the top (decisions) to understand **philosophy**.  
Drill to runbooks for **implementation details**.

---

## Contributing to the Wiki

**After each Phase milestone:**

1. **Document what you learned**
   - What assumptions were wrong?
   - What patterns emerged?
   - What would you do differently?

2. **Update decision rationale**
   - Did this decision hold up in practice?
   - Should we revisit?

3. **Add patterns**
   - "Every ingestion of large codebases needs X"
   - "When Y happens, Z is the right response"

4. **Improve runbooks**
   - Add troubleshooting sections
   - Document gotchas
   - Add examples

---

## Search Tips

**Looking for...**

| You want to find | Search these files |
|-----------------|-------------------|
| Why ALARMv4 exists | docs/1_RETROSPECTIVE.md |
| How gates work | docs/3_GATES_AND_ROUTING.md + gates/index.md |
| How agents learn | docs/2_SYSTEM_2_FRAMEWORK.md + patterns/expert_persona.md |
| Storage schema | schemas/firestore_schema.md |
| Step-by-step setup | runbooks/moltbook_spawn.md |
| Why we chose X | decisions/why_*.md |
| How to debug Y | runbooks/debug_*.md |

---

## Key Files (Keep These Updated)

- `docs/DECISIONS.md` — Master decision log (add ADRs here)
- `wiki/decisions/` — Detailed decision rationale (one file per decision)
- `docs/RUNBOOKS/` — Operational guides (updated after every sprint)
- `HANDOFF.md` — For handing off to next model

---

## Phase Timeline & Wiki Evolution

| Phase | Wiki Additions | Owner |
|-------|---|---|
| **α** | Architecture decisions, persona charters, gate specs | @BraPil + Interviews |
| **β** | Agent protocol, Moltbook patterns, first runbooks | Sonnet |
| **γ** | Ingestion agent specs, confidence scoring details | Sonnet |
| **δ** | Documentation agent specs, synthesis patterns | Sonnet |
| **ε** | Hardening learnings, troubleshooting guides | Sonnet |

---

## Questions?

If something is unclear:
1. Check the wiki (you're here!)
2. Search decision rationale in `decisions/`
3. Look for patterns in `patterns/`
4. Check runbooks in `runbooks/`
5. Open a GitHub issue

Knowledge should be here. If it's not, it's a documentation debt. Help us fix it.