# CLAUDE.md — ALARMv4

ALARMv4 is an enterprise-scale agentic semantic cartography platform. This guide is for developers, AI architects, and anyone working on Phase α (planning) and beyond.

---

## What We're Building

ALARMv4 = **Continuous organizational semantic intelligence**

Unlike ALARMv3 (which ingested one repo in isolation), ALARMv4:
- **Ingests multiple codebases** with 99%+ confidence via organic agentic swarms
- **Stores centrally** (GCP Firestore + Vertex AI) for cross-app intelligence
- **Learns continuously** via System 2 debate layer (expert personas that evolve)
- **Feeds downstream agents** (security, devops, modernization, compliance)

**The vision**: One deep ingestion. Many consumers. Continuous learning.

---

## Phase α: Planning (This Week & Next)

We are **here**. Code comes after design is locked.

**What's happening:**
1. **Monday-Tuesday**: Interview org experts (SecOps, DevOps, Business, Compliance)
2. **Wednesday-Thursday**: Bootstrap System 2 framework, capture expert charters
3. **Friday + Monday**: Finalize architecture thesis, retrospective, design docs
4. **Tuesday (Week 2)**: AAA validation, OAA research cycle
5. **Wednesday (Week 2)**: Incorporate feedback, lock design
6. **Thursday (Week 2)**: Ready for Phase β (Foundation)

---

## How to Read These Docs

**Start here:**
- `README.md` (this overview)
- `CLAUDE.md` (you're reading it)

**Then read (in order):**
1. `docs/0_THESIS.md` — What we're building, why, and the philosophy
2. `docs/1_RETROSPECTIVE.md` — What v1-v3 taught us, why we're starting fresh
3. `docs/2_SYSTEM_2_FRAMEWORK.md` — The continuous learning brain
4. `docs/3_GATES_AND_ROUTING.md` — How stages transition, who owns what
5. `docs/4_MOLTBOOK_STAGES.md` — Organic agentic swarms, slime mold topology
6. `docs/5_SEMANTIC_DB_GCP.md` — Firestore + Vertex AI schema, design rationale

**Reference:**
- `docs/DECISIONS.md` — ADRs (Architecture Decision Records)
- `docs/PERSONAS/*.md` — Expert persona charters (SecOps, DevOps, etc.)
- `wiki/` — Searchable knowledge base (post-Phase α)
- `docs/RUNBOOKS/` — Operational guides (post-Phase α)

---

## Key Concepts (Refresher)

### System 2: The Continuous Learning Brain

Runs **24/7**, separate from ingestion. Monitors:
- Southern Company CoPilot (org strategy)
- Teams/Slack/Outlook (policy, guidance)
- SharePoint (compliance, runbooks)
- GitHub webhooks (ALARMv4 decision logs)
- External feeds (industry trends)

**Expert personas** ingest these signals, debate implications, update **Guidance Ledger**.

When ALARMv4 ingests a repo, it queries the Guidance Ledger: "What's the current policy on security gates?" System 2's answer is always fresh.

### Guidance Ledger: Persistent Policy

Versioned, immutable record (Firestore) of:
- Gate conditions per stage ("what must be true to graduate?")
- Routing rules ("if X, go to stage Y")
- Debate history ("why we decided X")
- Confidence thresholds
- Escalation criteria

**Updated by System 2.** Queried by ALARMv4 at each gate.

### Gates: Expert-Owned Decision Points

Each stage transition (e.g., INTAKE → GOVERNANCE) is a **gate**:

```
GATE₁: Intake → Governance
├── Owner: SecOpsPersona (security expertise)
├── Adversarial Reviewers: DevOps, Business, Compliance, Architecture
├── Rules: confidence ≥95% AND code_scanned=true AND no_critical_vulns=0
├── Deadlock Resolution: SecOpsPersona decides; if still stuck, escalate to user
└── Logged to Guidance Ledger (feeds System 2 learning)
```

**Why this design?**
- Clear accountability (owner persona can't hide behind consensus)
- Adversarial review catches blind spots
- Escalation to user = system learns from human expertise
- Everything logged = auditable, compliant

### Moltbooks: Organic Swarms

Each ingestion **stage** (Intake, Analysis, Synthesis) runs in a temporary **Moltbook**:

```
Moltbook = temporary agentic ecosystem
├── Stem cells (undifferentiated agents, seeking roles)
├── Pressure field (stigmergic signals: "we need a parser!")
├── Differentiation (accumulate role_commitment → specialist)
├── Organs (cells with shared function cluster)
├── Slime mold network (adaptive communication topology)
└── Everything logged to wiki (persistent knowledge)
```

When stage completes, Moltbook dissolves. But its lessons persist in wiki + Guidance Ledger.

### Semantic DB: Hybrid Relational + Vector

**Firestore** (structured, transactional):
- Session metadata
- Gate decisions
- Routing logs
- Audit trail

**Vertex AI Search** (semantic, at scale):
- Code embeddings ("find similar patterns")
- Architecture graphs ("show me dependencies")
- Cross-repo intelligence ("which other apps use this tech stack?")

---

## Current Architecture Decisions

**Where:** `docs/DECISIONS.md`

**Key ones locked in (Phase α):**
- ✅ GCP (not Azure) — org strategic shift
- ✅ Firestore (not Postgres) — managed, scales, integrates with Vertex
- ✅ Vertex AI Search (not Ollama) — centralized, auditable, enterprise SLA
- ✅ Moltbooks from OAA (not custom) — reuse proven stem cell + organ mechanics
- ✅ Expert personas (internal, learning-based) — not famous AI architects
- ✅ System 2 first — cannot design ALARMv4 without knowing org policy

**Still open (Phase β):**
- MCP vs. HTTP API (or both?)
- Local staging vs. direct Firestore writes
- Rate limiting strategy
- Cost optimization tuning

---

## Org Context

**Expert Personas (your org):**
- **SecOps**: Brian Credeur, Joshua Humble, Kamar Peterkin
- **DevOps**: Greg Floyd (lead), Prasad Avvaru, Sindhu Mani, Siddhi Chitgopkar
- **Business**: Linda Harrell, Bonnie Parker, Todd Spinello
- **Compliance**: Joshua Humble (software), Tasha Rast, Leonard Smith (legal)

**Guidance Board (resolves policy debates):**
- When System 2 personas disagree, this board decides
- Updated frequently as new signals come in
- Clones itself to Stage Board for each ingestion

**Stage Board (orchestration, per-ingestion):**
- Uses Guidance Board's latest snapshot
- Makes gate/routing decisions for current session
- Logs all decisions back to Guidance Ledger

---

## How to Contribute (Phase α)

### Reading & Understanding
1. Clone the repo
2. Read docs in order (0 → 1 → 2 → 3 → 4 → 5)
3. Open GitHub issues if anything is unclear
4. Add wiki entries if you discover patterns

### Interviews (This Week)
If you're one of the org experts, you'll be contacted for a 30 min interview:
- What policies/standards do you enforce?
- What decisions do you own?
- What conflicts do you see between departments?
- What would make your job easier in ALARMv4?

**Outputs:** Expert Persona Charter (your domain expertise, encoded)

### Design Refinement (Next Week)
Review draft design docs, comment on:
- Gate conditions (are they realistic?)
- Routing rules (do they match org flow?)
- Persona ownership (is this the right expert?)
- Confidence thresholds (99% realistic or too high?)

### Phase β Preparation (Week 3)
Help prioritize which agents to build first:
- What's the MVP for Intake Moltbook?
- What gates are most critical?
- What should wiki/runbook capture first?

---

## Architecture Principles

**Enterprise Grade from Day 1**
- No technical debt — every code path tested + documented
- Operational readiness — structured logging, metrics, traces
- Audit compliance — every decision logged with rationale
- Runbooks maintained — updated after every sprint

**Organic Design (Biomimicry)**
- Agents start undifferentiated, specialize based on environment
- Communication emerges (slime mold), not hardcoded
- System learns from every run (System 2 captures patterns)
- Resilient by design (no single points of failure)

**Clear Boundaries (Separation of Concerns)**
- System 2 (policy) ≠ ALARMv4 (execution)
- Guidance Board (debate) ≠ Stage Board (decision)
- Moltbook (swarm) ≠ Core (coordination)
- Wiki (memory) ≠ Guidance Ledger (policy)

---

## Next Steps

**Your job this week:**
1. Schedule interviews with your 4 org expert groups
2. Read through `README.md` + `CLAUDE.md`
3. Skim `docs/0_THESIS.md` (draft, will evolve)
4. Give feedback on any architectural decisions you want to revisit

**My job this week:**
1. Conduct interviews, extract expert charters
2. Build System 2 framework scaffolding
3. Finalize all design docs
4. Prepare for AAA + OAA validation

**By EOW Week 2:**
- Design docs locked
- AAA validation complete
- Ready to start Phase β (code)

---

## Questions?

Open GitHub issues or Slack @BraPil.

We're building something exceptional here. Let's get it right.
