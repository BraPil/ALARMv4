# ALARMv4

> **Enterprise-scale agentic semantic cartography platform**
>
> The assembly-line conveyor belt for building organizational AI intelligence. ALARMv4 ingests legacy codebases with 99%+ confidence, producing comprehensive semantic understanding that feeds downstream agentic processes.

---

## What This Is

ALARMv4 is a **persistent, continuously-learning system** that:

1. **Ingests codebases** (legacy systems, any language, any size) through organic agentic swarms (Moltbooks)
2. **Maps them exhaustively** using structural analysis (AST, call graphs, complexity), behavioral tracking (git history), and semantic understanding (vector embeddings)
3. **Stores everything centrally** (GCP Firestore + Vertex AI) as a queryable hybrid semantic DB
4. **Learns continuously** through System 2 debate layer—expert personas (SecOps, DevOps, Business, Compliance, Architecture) that evolve based on org signals and ingestion results
5. **Produces enterprise artifacts**: comprehensive wiki, runbooks, architecture diagrams, semantic graphs
6. **Hands off to downstream agents** (via MCP or HTTP API) who use this knowledge to maintain, modernize, secure, or analyze the legacy system

**Field-tested philosophy:**
- ALARMv3 proved the core approach works but revealed gaps: shallow ingestion (30 min insufficient), monolithic design (hard to extend), unclear completeness
- ALARMv4 addresses all three: **thorough organic swarms** (6-12 hours per codebase), **modular agents** (reusable, testable, composable), **confidence scoring** (99%+ certainty, tracked at every gate)

---

## The Big Idea: Assembly Line Semantics

**Traditional approach:** Build one custom tool per modernization problem (ALARMv1, v2, v3 were each specialized).

**ALARMv4 approach:** Build the *conveyor belt* that learns everything about any codebase once. Then:
- Other agents query the semantic DB (no re-ingestion)
- Security teams scan it for vulnerabilities
- DevOps teams use it for deployment planning
- Business uses it for ROI/effort estimation
- Modernization agents use it to drive refactoring
- Compliance uses it for audit trails

**One ingestion. Many consumers.**

---

## Architecture at a Glance

```
┌─────────────────────────────────────────────────────────────┐
│         SYSTEM 2: Continuous Org Learning (24/7)             │
│  Expert Personas: SecOps, DevOps, Business, Compliance      │
│  Debate Loop: Ingests org signals → updates policies         │
└─────────────────┬───────────────────────────────────────────┘
                  │
                  ↓
    ┌─────────────────────────────────────────┐
    │   Guidance Ledger (GCP Firestore)       │
    │   - Gate conditions (current policy)    │
    │   - Routing rules (which stage next)    │
    │   - Decision history (why we decided X) │
    └─────────────────┬───────────────────────┘
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
    INTAKE        ANALYSIS     SYNTHESIS
    Moltbook      Moltbook      Moltbook
    (swarm A)     (swarm B)     (swarm C)
        │             │             │
        └─────────────┼─────────────┘
                      ↓
    ┌─────────────────────────────────────────────────────────┐
    │   Centralized Semantic DB (GCP Firestore + Vertex AI)    │
    │   - Ingested codebases (per-app semantic graph)          │
    │   - Vector embeddings (code intent + architecture)       │
    │   - Session state + decision logs                        │
    │   - Expert persona learnings (MCP-backed memory)         │
    └─────────────────────────────────────────────────────────┘
                      ↑
    Queries from downstream agents (security, devops, modernization, etc.)
```

---

## Quick Start (For Now: Design Phase)

This repo is in **Phase α: Planning**. Code comes later.

**Current state:**
- ✅ Repo structure created
- ✅ Expert personas defined
- ✅ Gate ownership matrix designed
- ✅ System 2 debate framework sketched
- 🔄 **This week**: Interviews with org experts (SecOps, DevOps, Business, Compliance)
- 🔄 **Next week**: Finalize design docs, validate with AAA + OAA
- 🔄 **Week 3**: Phase β (Foundation) — start building

**To understand ALARMv4:**
1. Read `docs/0_THESIS.md` (what we're building and why)
2. Read `docs/1_RETROSPECTIVE.md` (what v1-v3 taught us)
3. Read `docs/2_SYSTEM_2_FRAMEWORK.md` (continuous learning layer)
4. Explore `docs/PERSONAS/` (who makes decisions)
5. Check `docs/GATES_AND_ROUTING.md` (how stages transition)

---

## Key Concepts

### System 2: Continuous Organizational Learning
Not separate from ALARMv4—it's the **brain** that learns from every ingestion and updates guidance. Monitors:
- Southern Company CoPilot (org strategy)
- Teams/Slack (policy discussions)
- SharePoint (compliance docs)
- GitHub (decision logs from past ingestions)
- External feeds (industry trends, regulatory changes)

Expert personas debate, reach consensus (or escalate to Board), publish updated policies.

### Moltbooks: Organic Agentic Swarms
Each ingestion stage (Intake, Analysis, Synthesis) runs in its own temporary **Moltbook**:
- Agents start undifferentiated (stem cells)
- Sense their environment via stigmergic pressure field
- Differentiate into specialists (cells → organs → body)
- Dissolve when stage completes
- Everything logged to wiki for persistence + future learning

Inspired by **Organic Agentic AutoDev** (your OAA framework).

### Gates: Expert-Owned, Adversarial-Reviewed
Each stage transition is a **gate** with:
- **Owner**: The expert persona for that domain (e.g., SecOps owns security gates)
- **Adversarial reviewers**: Other personas challenge the decision
- **Guidance rules**: Current policy (from System 2) that gate checks
- **Confidence threshold**: Must be ≥95% to graduate
- **Deadlock resolution**: Owner persona decides; if still stuck, escalate to user for human judgment (feeds System 2)

### Semantic DB: Hybrid Relational + Vector
**Firestore** (transactional, queryable):
- Session metadata, gate decisions, routing logs
- Audit trail (who decided what, when, why)

**Vertex AI Search** (semantic, at-scale):
- Code embeddings (what does this function do conceptually?)
- Architecture patterns (where are similar designs?)
- Cross-repo intelligence (which other apps have this tech stack?)

---

## Roadmap (High Level)

| Phase | Scope | Timeline | Status |
|-------|-------|----------|--------|
| **α** | System 2 bootstrap, design docs, expert persona charters | Week 1-2 | 🔄 In Progress |
| **β** | Foundation: agent harness, Moltbook scaffolding, first Intake stage | Week 3 | ⏳ Next |
| **γ** | Core ingestion agents (discovery, analysis, knowledge, synthesis) | Week 4-5 | ⏳ Next |
| **δ** | Documentation + diagramming agents, confidence gates | Week 6 | ⏳ Next |
| **ε** | Polish, testing, hardening, enterprise readiness | Week 7-8 | ⏳ Next |

---

## For Developers

See `CLAUDE.md` for:
- How to read these docs
- How to contribute during planning phase
- How to interpret architecture decisions
- Next steps after Phase α

---

## Contact & Escalation

**Phase α lead**: @BraPil  
**Org expert contacts** (for System 2 debates):
- **SecOps**: Brian Credeur, Joshua Humble, Kamar Peterkin
- **DevOps**: Greg Floyd, Prasad Avvaru, Sindhu Mani, Siddhi Chitgopkar
- **Business**: Linda Harrell, Bonnie Parker, Todd Spinello
- **Compliance**: Joshua Humble (software), Tasha Rast, Leonard Smith (legal)

---

## License

Proprietary — Southern Company Power Delivery & Customer Service