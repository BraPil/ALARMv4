# ALARMv4 Architecture Thesis

**Phase**: α (Planning)  
**Status**: Draft (will evolve based on expert interviews)  
**Last Updated**: 2026-08-30  

---

## The Problem

### What ALARMv3 Got Right

- **MCP-first interface**: Thin, stateless wrapper over pure Python core ✅
- **Safety guardrails**: Linear state machine, read-only source zone, WORM audit log ✅
- **Deterministic analysis**: AST parsing, call graphs, complexity metrics (no LLM until synthesis) ✅
- **Adversarial evaluation**: Separate Claude call critiques recommendations (catches sycophancy) ✅

### What ALARMv3 Got Wrong

- **Shallow ingestion**: 30 minutes for a 500k LOC codebase → missed large portions
- **No completeness guarantee**: User can't know if ALARMv3 found everything
- **Monolithic design**: All logic in one phase → hard to parallelize, extend, test
- **No learning loop**: Each ingestion isolated; patterns aren't captured
- **No org context**: Gates don't reflect real Southern Company policy
- **Unclear confidence**: What's our 95% confidence actually measuring?

### The Field Reality

When applied to ADDS (AutoCAD/Oracle archive):
- First run: 30 minutes → clearly insufficient (user felt 20% coverage)
- Eventually: 6 hours → better, but user still only ~80% confident
- Architecture diagrams: Partially correct but incomplete
- Why? **Monolithic synthesis** couldn't adapt when deep analysis revealed new patterns

---

## The Solution: ALARMv4

### Core Thesis

**ALARMv4 is an assembly-line conveyor belt for organizational AI intelligence.**

Instead of building specialized tools, build infrastructure that:
1. **Learns everything about any codebase** (99%+ confidence)
2. **Stores it centrally** in a queryable semantic DB
3. **Learns continuously** from org policy, feedback, and ingestion patterns
4. **Feeds downstream systems** (security, devops, modernization, compliance)

**One deep ingestion. Many consumers. Continuous evolution.**

### Why This Matters for Southern Company

- **Reusability**: Ingest a legacy app once; feed it to 10+ downstream tools
- **Organizational alignment**: Gates and policies embed real org constraints
- **Auditability**: Every decision logged with rationale (compliance requirement)
- **Scalability**: Modular design scales to enterprise-wide adoption
- **Continuous improvement**: System 2 learns and evolves governance

---

## Architecture Principles

### 1. **Organic Design (Biomimicry)**

Each ingestion stage (Intake, Analysis, Synthesis) runs a **Moltbook**:
- Agents start undifferentiated (stem cells)
- Sense environment via stigmergic pressure field
- Differentiate into specialists (cells → organs → body)
- Communication topology emerges (slime mold network)
- Dissolve when stage completes

**Why**: Emergent systems are more resilient, adaptive, and learnable than rigid pipelines.

### 2. **Expert-Owned Gates**

Each stage transition is owned by the relevant expert persona:
- **SecOps owns security gates** (code ingestion, vulnerability scanning)
- **DevOps owns infrastructure gates** (deployment readiness, scaling)
- **Business owns priority gates** (ROI, effort, speed vs. depth)
- **Compliance owns governance gates** (audit, regulatory requirements)
- **GateKeeper owns routing** (which stage comes next?)

**Why**: Clear accountability. Owner can't hide behind consensus, but adversarial reviewers catch blind spots.

### 3. **Confidence-Based Progression**

No stage graduates until:
- Confidence ≥ 95% (measured continuously by analysis agents)
- All mandatory policies satisfied (from Guidance Ledger)
- Adversarial review passed (other personas challenged and owner responded)
- Artifacts ready for handoff (wiki, runbooks, semantic graphs)

**Why**: Transparent completeness. User knows what we found and what we didn't.

### 4. **System 2 Debate Layer (Continuous Learning)**

Runs **24/7**, separate from ingestion:
- Ingests org signals (CoPilot, Teams, SharePoint, GitHub, external feeds)
- Expert personas debate implications (\"does this change our gate rules?\")
- Reaches consensus or escalates to Board
- Updates Guidance Ledger (policies)
- ALARMv4 queries at each gate: \"what's current policy?\"

**Why**: Org governance evolves; gates and policies must stay aligned.

### 5. **Centralized Semantic DB (GCP)**

One source of truth:
- **Firestore** (transactional, audit trail)
- **Vertex AI Search** (semantic, cross-repo intelligence)
- **Versioned** (every ingestion is a snapshot)
- **Queryable** (downstream agents don't re-ingest)

**Why**: Eliminates silos. Security, devops, modernization teams all use same semantic understanding.

---

## What Changes from ALARMv3

| Aspect | ALARMv3 | ALARMv4 |
|--------|---------|----------|
| **Ingestion time** | 30 min (incomplete) | 6-12 hours (99% confidence) |
| **Architecture** | Monolithic pipeline | Modular organic swarms |
| **Learning** | Per-session isolation | Continuous System 2 layer |
| **Storage** | Per-session silos | Centralized + versioned |
| **Policy** | Hardcoded in code | Guidance Ledger (evolves) |
| **Gates** | Linear state machine | Expert-owned + adversarial |
| **Confidence** | Implicit, unclear | Explicit, scored at every checkpoint |
| **Org alignment** | No | Yes (System 2 learns org policy) |

---

## Key Design Decisions

### Decision #1: Why GCP (not Azure)?

**Org context**: Southern Company is migrating to GCP. Most new infra lands there.

**Technical**:
- Vertex AI Search: Managed RAG (scale, SLOs)
- Firestore: Transactional + vector-capable
- Tight integration with Claude API (easy auth)

**Deferred decision**: If cost optimization needed later, can migrate to Cloud SQL + pgvector.

### Decision #2: Why Organic Moltbooks (not Fixed Workflow)?

**Problem ALARMv3 had**: Synthesis was monolithic. When deep analysis found new patterns, synthesis couldn't adapt.

**Solution**: Spawn a Moltbook per stage. Let agents organically specialize. When analysis finds a gap, a new agent "bids" to fill it. Topology emerges.

**Trade-off**: More complex to implement, but much more resilient and learnable.

### Decision #3: Why System 2 Is Critical

**You can't design ALARMv4's gates without knowing org policy.**

- What defines \"secure code\"? SecOps knows.
- What defines \"deployable\"? DevOps knows.
- What defines \"valuable\"? Business knows.
- What defines \"compliant\"? Compliance knows.

These answers **change over time** (new regulations, org priorities shift). System 2 captures the debate and keeps gates aligned.

---

## Roadmap

| Phase | Deliverable | Owner | Timeline |
|-------|-------------|-------|----------|
| **α** | Design docs + expert charters + Guidance Ledger schema | You + me | Week 1-2 |
| **β** | Moltbook harness + Intake stage foundation | Sonnet | Week 3 |
| **γ** | Discovery, Analysis, Knowledge agents | Sonnet | Week 4-5 |
| **δ** | Synthesis, Documentation, Diagramming agents | Sonnet | Week 6 |
| **ε** | Confidence gates, testing, hardening | Sonnet | Week 7-8 |

---

## Success Criteria

✅ **At end of Phase α:**
- Expert persona charters finalized (org context locked)
- Guidance Ledger schema designed (policies queryable)
- Gate ownership matrix documented (decisions clear)
- System 2 framework specification complete

✅ **At end of Phase β:**
- Moltbook harness working (agents can spawn, differentiate, dissolve)
- Intake Moltbook operational (user conversation + repo detection)
- First gate evaluation passing tests
- Wiki + runbook generation starting

✅ **At end of Phase γ (MVP complete):**
- Full ingestion pipeline: Intake → Analysis → Synthesis → Implementation
- Confidence scoring at every checkpoint
- Semantic DB populated for first codebase
- 99%+ confidence on test codebase

---

## Open Questions (To Resolve in Phase β)

1. **Exact agent roles in Intake Moltbook**: Discovery? User Dialog? Clarifier? How many stem cells?
2. **MCP vs. HTTP API**: Which is primary? Support both?
3. **Parallel stages**: Can Analysis start before Intake completes? (risky but faster)
4. **Cost model**: Budget per ingestion? Alerts when overspent?
5. **Cross-repo intelligence**: How to efficiently query \"which other apps use this tech stack?\"

---

## Why This Matters (The Vision)

ALARMv4 is not just \"ALARMv3 but better.\"

It's **the foundation for organizational agentic intelligence**. Once we deeply understand a legacy codebase, that understanding becomes a **reusable asset**:

- Security team queries it: \"Find all SQL injection risks\"
- DevOps queries it: \"Plan a cloud migration\"
- Modernization queries it: \"What's the highest-ROI refactor?\"
- Business queries it: \"What's the replacement effort estimate?\"
- Compliance queries it: \"Show me the audit trail\"

**One ingestion. Many uses. Continuous evolution.**

That's the vision. Let's build it.
