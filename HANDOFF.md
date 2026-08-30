# Handoff Document — ALARMv4

**Phase**: α (Planning)  
**Status**: In Progress  
**Last Updated**: 2026-08-30  
**Next Phase**: β (Foundation)  
**Next Model**: Claude Sonnet 4  

---

## What's Done

✅ Repo structure created  
✅ Architecture thesis drafted  
✅ System 2 debate framework designed  
✅ Gate ownership matrix defined  
✅ Expert persona charters planned  
✅ Semantic DB schema sketched (GCP Firestore + Vertex AI)  
✅ Moltbook stage definitions drafted  
✅ Phase α timeline locked  

---

## What's In Progress (This Week)

🔄 **Monday-Tuesday**: Expert persona interviews (SecOps, DevOps, Business, Compliance)  
🔄 **Wednesday-Thursday**: System 2 framework bootstrap, capture expert charters  
🔄 **Friday + Monday**: Finalize all design docs  

---

## What's Next (Phase β)

⏳ **Week 3**: Foundation layer
- Build AgentBase + Moltbook harness (OAA integration)
- Implement Intake Moltbook (first swarm)
- Wire up GCP Firestore + Vertex AI clients
- First unit + integration tests

⏳ **Week 4-5**: Core ingestion agents
- Discovery agent (file manifest)
- Analysis agent (AST, graph, complexity)
- Knowledge agent (chunking, embeddings)
- Synthesis agent (recommendations)

---

## Critical Context for Next Model

**Do NOT skip these:**
1. Read `README.md` (overview, why this matters)
2. Read `CLAUDE.md` (developer guide)
3. Skim `docs/0_THESIS.md` (architecture philosophy)
4. Review `docs/3_GATES_AND_ROUTING.md` (decision architecture)
5. Understand System 2 debate layer (not ALARMv4 execution layer)

**Key design decisions:**
- **GCP first** (Azure for legacy, but org is migrating to GCP)
- **Firestore + Vertex AI** (managed, auditable, enterprise SLOs)
- **Moltbooks from OAA** (don't reinvent — reuse proven patterns)
- **Expert personas are internal** (learn over time, not famous AI architects)
- **System 2 is critical** (can't design gates without knowing org policy)

**Org experts (for interviews/collaboration):**
- **SecOps**: Brian Credeur, Joshua Humble, Kamar Peterkin
- **DevOps**: Greg Floyd, Prasad Avvaru, Sindhu Mani, Siddhi Chitgopkar
- **Business**: Linda Harrell, Bonnie Parker, Todd Spinello
- **Compliance**: Joshua Humble (software), Tasha Rast, Leonard Smith (legal)

---

## Why This Approach Matters

**Problem we're solving:**
- ALARMv3 was monolithic, ingestion was shallow (30 min insufficient)
- Users didn't know if it found everything in a codebase
- No learning loop (each ingestion was isolated)
- No org governance (gates didn't reflect real policy)

**Solution:**
- Modular agentic swarms (Moltbooks) that learn from each other
- System 2 debate layer (continuous org learning)
- Confidence scoring at every gate (transparent completeness)
- Centralized semantic DB (one ingestion, many consumers)

**Why it matters for Southern Company:**
- Assembly-line semantics (ingest once, feed many downstream systems)
- Auditable (every decision logged with rationale)
- Org-aware (policies embedded in gates, learned continuously)
- Enterprise-ready (scalable, compliant, testable)

---

## Known Unknowns

**To resolve in Phase β:**
- Exact Intake Moltbook agent roles (Discovery? User Dialog? Clarifier?)
- MCP vs. HTTP API (or both?)
- Cost optimization for large-scale ingestions
- Parallel vs. sequential Moltbook stages
- How to handle cross-repo intelligence efficiently

---

## Files to Review

**Must read:**
- `README.md`
- `CLAUDE.md`
- `docs/0_THESIS.md` (draft, will evolve)

**Should review:**
- `docs/2_SYSTEM_2_FRAMEWORK.md` (understand the brain)
- `docs/3_GATES_AND_ROUTING.md` (understand decisions)
- `docs/PERSONAS/` (understand expert roles)

**Can reference later:**
- `docs/5_SEMANTIC_DB_GCP.md` (detailed schema, after code starts)
- `wiki/` (auto-populated post-Phase α)
- `docs/RUNBOOKS/` (auto-populated post-Phase α)

---

## Questions for Next Model

If something is unclear:
1. Check `CLAUDE.md` §"Key Concepts (Refresher)"
2. Check `docs/DECISIONS.md` for rationale
3. Open a GitHub issue (I'll add context)
4. Slack @BraPil for urgent clarification

---

## Success Criteria (Phase α)

✅ Expert persona charters finalized  
✅ Guidance Ledger schema locked  
✅ Gate ownership matrix documented  
✅ System 2 framework design complete  
✅ AAA + OAA validation passed  
✅ Design docs ready for Phase β  
✅ Team (org experts + technical) aligned  

---

## Welcome, Next Model 🚀

We're building enterprise-grade agentic semantics. It's complex, it's beautiful, it matters. 

You've got solid ground to stand on. Let's keep moving.
