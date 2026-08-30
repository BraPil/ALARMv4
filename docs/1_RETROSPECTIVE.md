# ALARMv4: Retrospective on v1, v2, v3

**Phase**: α (Planning)  
**Status**: Draft (will evolve with expert interviews)  
**Last Updated**: 2026-08-30  

---

## ALARM v1: The C# Era (2024)

### What It Was
- **Language**: C# (.NET Framework)
- **Focus**: AutoCAD Map 3D + Oracle 19c migrations
- **Approach**: Specialized adapter patterns for each target system
- **Output**: Migration playbooks + effort estimates

### What Worked ✅
- **Adapter pattern**: Clean separation of concerns (very elegant)
- **Safety-first design**: Never modified source, only analyzed
- **Targeted scope**: Knew exactly what problem it was solving
- **Team velocity**: Fast to build, easy to iterate

### What Failed ❌
- **Monolithic scope**: Each new legacy system = new custom tool
- **Limited language support**: C# ecosystem only
- **No reusability**: Code from one migration couldn't be used for another
- **Shallow semantic understanding**: Regex patterns, not AST
- **No cross-repo intelligence**: Each system analyzed in isolation

### Key Lesson
**Specialized tools don't scale.** Every new domain (Oracle→PostgreSQL, VB→Python, AutoCAD→cloud) required a new tool. Maintenance burden grew faster than value.

---

## ALARM v2: The Python + RAG Era (2024-2025)

### What It Was
- **Language**: Python
- **Focus**: Comprehensive reverse engineering (any legacy system)
- **Approach**: AST parsing + RAG (retrieval-augmented generation)
- **Output**: Architecture diagrams + modernization recommendations

### What Worked ✅
- **Multi-language support**: Python, C++, Java, JS, VB (via regex)
- **Semantic understanding**: RAG layer could reason about *intent*, not just syntax
- **Cross-repo patterns**: Started recognizing common architecture patterns
- **Session management**: Durable SQLite store (survive crashes)
- **Flexibility**: Could adapt to new domains relatively quickly

### What Failed ❌
- **RAG quality issues**: Vector DB was local (Chroma), sometimes unreliable
- **Shallow ingestion**: Still missed large portions of large codebases
- **No confidence scoring**: Users didn't know what was missed
- **Monolithic synthesis**: All analysis funneled into one big recommendation phase
- **No org context**: Didn't know what "good" meant in customer's environment
- **Unclear completeness**: Even with 6 hour runs, felt like 70-80% coverage

### Field Results (ADDS AutoCAD/Oracle Archive)
- **First ingestion**: 30 min → obviously incomplete (user: "feels like 20% of code")
- **Second ingestion**: Deep analysis + RAG → 6 hours
- **Result**: Architecture diagrams looked plausible but had gaps
- **Completeness confidence**: ~70-80% (user uncertain)
- **Problem**: Users couldn't trust it to make major decisions

### Key Lesson
**Monolithic synthesis + unclear scope = user distrust.**

When all analysis funnels into one big Claude call, system can't adapt mid-run. When user can't see what was covered and what wasn't, they default to skepticism.

---

## ALARM v3: The MCP-First Era (2025-2026)

### What It Was
- **Language**: Python (pure core)
- **Focus**: MCP-first interface + guardrails + deep analysis
- **Approach**: Linear state machine + three-layer architecture (interface/core/adapters) + Phase 6 deep analysis (subsystem partitioning)
- **Output**: Semantic DB + wiki + modernization recommendations + implementation guidance

### What Worked ✅
- **Three-layer architecture**: Pure Python core with zero MCP dependency → testable, extensible
- **Guardrails**: WORM audit log, read-only source zone, explicit state machine → compliant
- **Deep analysis**: Subsystem-partitioned multi-pass synthesis (Phase 6) → more thorough
- **Runtime grammar inference**: Self-improving support for unknown languages (Phase 7)
- **Adversarial evaluation**: Separate Claude call critiques recommendations → catches problems
- **Codebase-policy overlays**: Per-codebase governance (Phase P0) → org-aware
- **Cross-repo intelligence**: Started tracking dependencies between applications

### What Failed ❌
- **Still shallow**: 30-minute runs clearly insufficient; even 6 hours felt incomplete
- **Monolithic design**: All orchestration in one Orchestrator class → hard to parallelize
- **No completeness guarantee**: Confidence scoring existed but wasn't transparent to users
- **No learning loop**: Each ingestion ignored patterns from previous runs
- **No org governance integration**: Gates were hardcoded, didn't evolve with policy
- **No continuous policy adaptation**: When org policy changed, gates didn't update
- **Confidence opaque**: Users had to guess: "Did we cover everything?"

### Field Results (ADDS again)
- **Ingestion time**: Best case 30 min (incomplete), realistic 6+ hours
- **Coverage**: Deep analysis improved quality, but completeness still unclear
- **Architecture**: Diagrams were better but had known gaps
- **User confidence**: ~75-85% (better than v2, but still insufficient for major decisions)

### What We Learned

#### Lesson 1: Monoliths Don't Scale
All logic in one pipeline → can't adapt when analysis reveals new patterns. A single synthesis call can't handle discovering new complexity mid-run.

#### Lesson 2: Completeness Needs Transparency
If users can't see what was covered and what wasn't, they assume the worst. Confidence scoring must be explicit and continuous.

#### Lesson 3: Org Context Is Mandatory
Gates can't be hardcoded. What "secure" means, what "deployable" means—these evolve with org policy. System must learn.

#### Lesson 4: One Ingestion, Many Uses
Each ALARMv ingestion was treated as a one-off. But a comprehensive semantic understanding of a codebase is valuable for *many* downstream systems (security, devops, modernization, compliance). Why re-ingest?

#### Lesson 5: Learning Must Be Persistent
v3 learned within a session, then forgot everything. Patterns should persist and inform future ingestions.

---

## What ALARMv4 Fixes

### Problem #1: Shallow Ingestion
**v3**: Monolithic synthesis can't adapt when discovery reveals new complexity.  
**v4**: Organic Moltbooks. When Intake finds "this repo is critical," Analysis stage spawns more agents. When deep_analysis finds new patterns, they feed back into synthesis.

### Problem #2: Unclear Completeness
**v3**: "We ran for 6 hours. Coverage unknown."  
**v4**: Every agent tracks what they analyzed. Checkpoints record confidence growing. Gate requires ≥95%. User sees the journey.

### Problem #3: Monolithic Architecture
**v3**: All orchestration in one class.  
**v4**: Modular agents. Each can be tested, extended, reused independently. Swarms emerge organically.

### Problem #4: No Learning Loop
**v3**: Each ingestion was isolated.  
**v4**: System 2 debate layer captures patterns. Guidance Ledger evolves. Future ingestions benefit from past learnings.

### Problem #5: Hardcoded Gates
**v3**: Gates were in code. To change policy, change code.  
**v4**: Guidance Ledger. Policies evolve via System 2 debate. Gates always reflect current org context.

### Problem #6: One-Off Ingestions
**v3**: Ingest, generate recommendations, done. No asset reuse.  
**v4**: Centralized semantic DB. Security, devops, modernization all query the same understanding. One deep ingestion, many consumers.

---

## The Discontinuity: Why v3 Can't Become v4

You might ask: "Why not just patch v3?"

Because the core issue is **architectural**, not tactical:

- **v3 is orchestration-centric**: One pipeline, parallel workers within phases
- **v4 is emergence-centric**: Multiple Moltbooks, agents self-organize

- **v3 gates are hardcoded**: State machine in code
- **v4 gates are policy-driven**: Guidance Ledger + System 2 debate

- **v3 storage is per-session**: `.alarmv3/sessions/<id>/` silos
- **v4 storage is centralized**: GCP Firestore + Vertex AI (shareable, queryable)

- **v3 has no learning layer**: Insights stay in audit logs
- **v4 has System 2**: Continuous org-aware debate

**These require rewriting the core.** v3 is the foundation (guardrails, audit log, layer separation). But v4 is a different system on top.

---

## Why Now? (The Context)

### Southern Company's Evolution

1. **Multi-agent awakening**: You're building Agentic-AI-Architect, Organic-Agentic-AutoDev, etc.
2. **Central governance need**: Need to coordinate these agents (hence System 2, Board orchestration)
3. **GCP migration**: Org is moving cloud infrastructure to GCP (timing works)
4. **Semantic data value**: Realized that *understanding* a codebase is itself a valuable asset
5. **Cross-org reuse**: Want to feed one semantic DB to many downstream systems

### Technical Convergence

1. **Moltbook pattern works**: You've proven organic swarms in OAA + Agentic-AI-Architect
2. **Firestore matured**: Can now do relational + vector queries in one DB
3. **Vertex AI Search is ready**: Managed RAG at enterprise scale
4. **Claude is capable enough**: Sonnet/Opus can handle complex synthesis + debate

---

## What We Keep From v3

✅ **Guardrails**: State machine + WORM audit log → take as-is  
✅ **Three-layer architecture**: Interface / Core / Adapters → reuse the pattern  
✅ **Language support**: tree-sitter parsers + runtime grammar inference → reuse  
✅ **Deep analysis philosophy**: Subsystem partitioning + multi-pass synthesis → reuse  
✅ **Adversarial evaluation**: Separate Claude call to critique → reuse  
✅ **Session persistence**: SQLite WORM logs → reuse pattern  

---

## Success Metrics

### Coverage & Confidence
- **v2**: 70-80% confidence, user uncertain  
- **v3**: 75-85% confidence, slightly better  
- **v4 target**: 95-99% confidence, user certain

### Ingestion Completeness
- **v2/v3**: "We ran for 6 hours. Did we get everything?" → unknown  
- **v4**: "Confidence is 97%. Here's what we analyzed and what we skipped." → transparent

### Org Alignment
- **v3**: Gates don't reflect org policy; users bypass them  
- **v4**: Policies evolve via System 2; gates always relevant

### Reusability
- **v3**: One semantic DB per session; hard to share  
- **v4**: Central DB; 10+ downstream systems query it; ROI multiplied

---

## The Path Forward

**Phase α (Week 1-2)**: Capture org context via expert interviews. Lock architecture via System 2 debate framework.  

**Phase β (Week 3)**: Build the foundation (Moltbook harness, first Intake stage, gate framework).  

**Phase γ (Week 4-5)**: Build core ingestion agents (discovery, analysis, knowledge, synthesis).  

**Phase δ (Week 6)**: Build documentation agents (wiki, diagrams, runbooks).  

**Phase ε (Week 7-8)**: Polish, test, harden, achieve enterprise readiness.  

**End of Q3**: v4 MVP ready for real ingestions. v3 becomes reference/fallback.

---

## One More Thing

ALARMv3 wasn't a failure. It proved:
- ✅ The core approach works
- ✅ Deep analysis can drive recommendations
- ✅ Users trust structured, auditable processes
- ✅ Org governance matters

ALARMv4 takes those lessons and builds something **bigger, more modular, more learnable, more aligned with org reality.**

That's not a rewrite—that's an evolution.
