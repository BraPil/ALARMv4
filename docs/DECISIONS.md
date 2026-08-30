# ALARMv4 Architecture Decision Record (ADR)

**Format**: One decision per section  
**Status options**: Accepted | Pending | Superseded | Rejected  
**Updated**: 2026-08-30  

---

## ADR-001: Use GCP (not Azure)

**Status**: Accepted ✅  
**Phase**: α (Planning)  
**Decided**: 2026-08-30  

### Context
- Southern Company is migrating infrastructure from Azure to GCP
- Most new projects are landing in GCP
- ALARMv4 is a new system; should use org's strategic platform

### Decision
**Use Google Cloud Platform (GCP) for all centralized storage and services.**

### Rationale
- **Org alignment**: GCP is strategic direction; reduces lock-in risk if we diverge later
- **Managed services**: Vertex AI Search + Firestore are tightly integrated (RAG + vectors in one DB)
- **Cost**: GCP pricing is competitive for managed AI services
- **Team expertise**: Org is investing in GCP skills

### Alternatives Considered
- **Azure**: Legacy platform, org moving away; would require custom RAG setup
- **Postgres + pgvector**: Open source, portable; costs more to manage; less integrated

### Consequences
- ✅ Aligned with org strategy
- ✅ Managed services reduce ops burden
- ⚠️ Vendor lock-in (Vertex AI is GCP-specific)
- ⚠️ If org changes strategy, migration effort required

### Mitigation for Lock-In
Layer abstraction over Vertex AI. If we switch later, swap the backend without rewriting core logic.

---

## ADR-002: Hybrid Relational + Vector DB (Firestore + Vertex)

**Status**: Accepted ✅  
**Phase**: α (Planning)  
**Decided**: 2026-08-30  

### Context
- Need to store two types of data:
  - **Structured**: Session metadata, gate decisions, audit logs (queryable by ID, time, etc.)
  - **Semantic**: Code embeddings, architecture patterns (queryable by similarity)
- Two separate databases (Postgres + Pinecone) adds complexity
- GCP offers integrated solution

### Decision
**Use Firestore (relational) + Vertex AI Search (vector) as hybrid DB.**

### Rationale
- **Firestore**: Transactional, ACID, easy to query sessions/gates
- **Vertex AI Search**: Managed RAG, semantic search built-in, enterprise SLA
- **Integration**: Firestore can reference Vertex embeddings; no sync issues
- **Scalability**: Both handle enterprise workloads
- **Cost**: Managed services cheaper than self-hosted alternatives at scale

### Alternatives Considered
- **Postgres + pgvector**: Open source, portable; would require us to manage embedding infrastructure
- **Vector-only (Pinecone)**: Fast but can't do relational queries (audit trails, filtering)
- **Postgres only**: No vector search (poor semantic capabilities)

### Consequences
- ✅ Unified data store (no sync overhead)
- ✅ Enterprise-grade SLAs
- ✅ Semantic + relational queries in one product
- ⚠️ GCP-specific (lock-in)
- ⚠️ Managed service costs can be opaque

---

## ADR-003: Organic Moltbooks (not Fixed Orchestration)

**Status**: Accepted ✅  
**Phase**: α (Planning)  
**Decided**: 2026-08-30  

### Context
- ALARMv3 used fixed orchestration (discovery → analysis → synthesis pipeline)
- When deep analysis revealed new patterns, synthesis couldn't adapt
- Users felt system was "shallow" even after 6-hour runs

### Decision
**Use organic Moltbooks (agentic swarms with emergent topology) instead of fixed pipeline.**

Each stage (Intake, Analysis, Synthesis) is a **temporary Moltbook**:
- Agents start undifferentiated (stem cells)
- Sense environment via pressure field
- Differentiate into specialists (organs)
- Topology emerges (slime mold network)
- Dissolve when stage completes

### Rationale
- **Resilience**: If one agent fails, swarm adapts (other agents cover)
- **Adaptability**: New patterns trigger new agents (don't need to rebuild pipeline)
- **Learnable**: Emergent behavior is more transparent (easier to debug than hardcoded flows)
- **Proven**: You've already validated OAA + Agentic-AI-Architect use this pattern

### Alternatives Considered
- **Fixed orchestration** (like v3): Simpler, but brittle
- **Pure agent swarm** (no stages): Chaotic, hard to reason about progress

### Consequences
- ✅ More adaptable to discovered complexity
- ✅ Resilient (no single points of failure)
- ✅ More learnable (patterns visible in behavior)
- ⚠️ Harder to test (emergent behavior)
- ⚠️ More compute overhead (more agents running)

---

## ADR-004: Expert Personas are Internal (not Famous AI Architects)

**Status**: Accepted ✅  
**Phase**: α (Planning)  
**Decided**: 2026-08-30  

### Context
- System 2 needs expert personas to debate org policy
- Could model them after famous AI architects (Karpathy, Medin, etc.)
- Could instead make them internal (learn from YOUR org's context)

### Decision
**Expert personas are internal to Southern Company and learn over time.**

Personas: SecOps, DevOps, Business, Compliance, Architecture, GateKeeper  
Each persona learns from your org's signals, debates, decisions.

### Rationale
- **Org-aware**: Policies embedded in personas reflect YOUR org (not generic best practices)
- **Evolving**: As org policy changes, personas adapt automatically
- **Auditable**: Every decision is linked to org context (compliance)
- **Reusable**: Personas can be queried by other systems (not just ALARMv4)

### Alternatives Considered
- **Famous architect personas**: More recognizable, but generic to org context
- **Pure policy registry** (no personas): Policies don't adapt, can't debate

### Consequences
- ✅ Org-aligned (policies reflect reality, not best practices)
- ✅ Self-evolving (no manual policy updates)
- ✅ Auditable (full debate record)
- ⚠️ More complex to implement (stateful agents)
- ⚠️ Requires bootstrapping (need to interview org experts)

---

## ADR-005: System 2 is Phase α Priority (before coding ALARMv4)

**Status**: Accepted ✅  
**Phase**: α (Planning)  
**Decided**: 2026-08-30  

### Context
- ALARMv4's gates need to know org policy (what's "secure"? "deployable"? "valuable"?)
- Policy answers come from experts (SecOps, DevOps, Business, Compliance)
- These answers evolve over time

### Decision
**Bootstrap System 2 BEFORE designing ALARMv4's gates.**

Phase α: Interview org experts → capture charters → design gates based on their expertise  
Phase β+: Build ALARMv4, which queries System 2 policies continuously

### Rationale
- **Prevents wrong gates**: If you hardcode gates without org context, users bypass them
- **Captures expertise**: Expert interviews create charters (permanent knowledge)
- **Enables evolution**: System 2 keeps gates aligned as policy changes
- **Improves adoption**: Users see gates that reflect reality, they trust system

### Alternatives Considered
- **Design gates generically, then refine**: Leads to rework; users bypass gates
- **Hardcode all gates in code**: No evolution; policy changes require code changes

### Consequences
- ✅ Gates aligned with org reality
- ✅ System self-evolves (no manual policy updates)
- ✅ High user trust
- ⚠️ Adds 1-2 weeks to Phase α (expert interviews)
- ⚠️ More complex implementation (stateful debate layer)

---

## ADR-006: Centralized Semantic DB (not per-session silos)

**Status**: Accepted ✅  
**Phase**: α (Planning)  
**Decided**: 2026-08-30  

### Context
- ALARMv3 stored each ingestion in `.alarmv3/sessions/<id>/` (isolated)
- Realized: understanding a codebase is valuable to MANY downstream systems (security, devops, modernization, compliance)
- Duplication: Every system that needs codebase understanding would re-ingest

### Decision
**Store all ingested codebases in centralized, queryable Semantic DB (Firestore + Vertex).**

One deep ingestion. Many consumers. No re-ingestion.

### Rationale
- **Reusability**: Security team queries it, DevOps queries it, modernization queries it
- **Efficiency**: One ingestion serves many use cases
- **Intelligence**: Queries can cross-reference ("which other apps use this tech stack?")
- **ROI**: Ingestion cost amortized across many consumers

### Alternatives Considered
- **Per-session silos** (like v3): Simpler, but no cross-repo intelligence
- **Replicate to multiple DBs**: Wasteful, sync problems

### Consequences
- ✅ High ROI (one ingestion, many consumers)
- ✅ Cross-repo intelligence
- ✅ Central source of truth
- ⚠️ Higher initial setup cost (centralized infrastructure)
- ⚠️ Data governance complexity (who can query what?)

---

## ADR-007: Confidence Scoring at Every Checkpoint (not end-of-stage)

**Status**: Accepted ✅  
**Phase**: α (Planning)  
**Decided**: 2026-08-30  

### Context
- ALARMv3: "We ran for 6 hours. Did we get everything?" → Unclear
- Users need transparency: What did we analyze? What's still uncertain?

### Decision
**Track and report confidence continuously. Gates require ≥95% confidence.**

Every checkpoint (agent completes a task) contributes to confidence score.  
User sees progress: "10% → 25% → 50% → 75% → 90% → 95%".

### Rationale
- **Transparency**: User knows what we've covered
- **Early warnings**: If confidence stalls at 70%, user knows something is wrong
- **Gate enforcement**: Can't graduate without 95% confidence (auditable)
- **Debugging**: Stuck confidence points to which agent/check is lagging

### Alternatives Considered
- **Confidence at end only**: Hidden progress, hard to debug
- **No confidence scoring**: Users guess; distrust results

### Consequences
- ✅ Transparent progress
- ✅ Auditable gates (can't fake completion)
- ✅ Early debugging
- ⚠️ More instrumentation (every agent must report confidence delta)
- ⚠️ Users might game the system (artificially inflate confidence)

---

## Pending Decisions (Phase β)

### ADR-008: MCP vs. HTTP API (or both?)
**Status**: Pending ⏳  
**Timeline**: Phase β  

ALARMv4 will expose gates/results via:
- MCP server (like v3)?
- HTTP API (more universal)?
- Both (more maintenance)?

### ADR-009: Local Staging vs. Direct Firestore Writes
**Status**: Pending ⏳  
**Timeline**: Phase β  

Ingestion writes:
- Locally first (safer, faster testing)?
- Directly to Firestore (simpler)?
- Both (with sync protocol)?

### ADR-010: Cost Model & Budgeting
**Status**: Pending ⏳  
**Timeline**: Phase β  

How do we manage costs?
- Budget per ingestion?
- Auto-alert when costs exceed threshold?
- Cost optimization tuning?

---

## Superseded Decisions

(None yet — Phase α is early)

---

## How to Add a Decision

When you make a significant architectural choice:

1. **Document it here** (use ADR-NNN format)
2. **Include**: Context, Decision, Rationale, Alternatives, Consequences
3. **Link it** from `wiki/decisions/why_*.md`
4. **Update HANDOFF.md** if it affects next model
5. **Reference it** in code comments when implementing

This way, future maintainers understand not just WHAT was decided, but WHY.