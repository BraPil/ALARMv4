# System 2: Continuous Organizational Learning Framework

**Phase**: α (Planning)  
**Status**: Draft (will evolve with expert interviews)  
**Last Updated**: 2026-08-30  

---

## What is System 2?

System 2 is **NOT part of ALARMv4's ingestion pipeline.**

It's a **separate, always-running agentic layer** that:
- Monitors org signals (Teams, SharePoint, CoPilot, GitHub, external feeds)
- Expert personas debate implications
- Updates organizational policy (Guidance Ledger)
- ALARMv4 queries this ledger at each gate: "What's current policy?"

**Metaphor**: Your org's "institutional memory" + "decision-making brain." It evolves independently, feeding wisdom to ALARMv4's execution.

---

## Signal Sources (Always-On Monitoring)

### 1. Southern Company CoPilot
**Ingests**: Org strategy, priority shifts, cross-team guidance  
**Trigger**: When CoPilot publishes new guidance  
**Relevant personas**: BusinessPersona, ArchitecturePersona  
**Example**: "We're prioritizing cloud migration over modernization this quarter" → affects routing (Analysis might defer to Infrastructure stage)

### 2. Teams / Slack Channels (Hooked)
**Ingests**: Policy discussions, urgent notices, community decisions  
**Channels to monitor**:
- `#secops-alerts` → SecOpsPersona
- `#devops-runbooks` → DevOpsPersona
- `#business-priorities` → BusinessPersona
- `#compliance-updates` → CompliancePersona
- `#architecture-decisions` → ArchitecturePersona

**Trigger**: Channel message flagged by System 2 signal evaluator  
**Example**: "New policy: all AI-generated code must use GPT-4 only (no Haiku/Sonnet)" → affects gate rules

### 3. SharePoint / Document Management
**Ingests**: Policy documents, compliance requirements, runbooks  
**Sites to monitor**:
- `/teams/secops/policies/*`
- `/teams/compliance/audits/*`
- `/teams/devops/infrastructure-standards/*`
- `/teams/business/roadmap/*`

**Trigger**: File modified or new file uploaded  
**Example**: "New security baseline: all repos must pass SAST scan" → affects gate

### 4. GitHub Webhooks (ALARMv4 Decision Logs)
**Ingests**: Gate decisions, routing decisions, failures  
**Trigger**: When ALARMv4 records a significant decision  
**Example**: "ANALYSIS gate failed 3 times due to missing language support" → triggers ArchitecturePersona to research

### 5. External Feeds
**Ingests**: Industry trends, regulatory changes, best practices  
**Sources**:
- arXiv (agentic AI, code generation, RAG)
- Regulatory news (SEC, FERC updates relevant to power delivery)
- AI research blogs (Karpathy, Medin, Vasilev)

**Trigger**: External event deemed significant  
**Example**: "New NIST guideline on AI governance" → CompliancePersona reviews implications

---

## Expert Personas (Stateful, MCP-Backed)

Each persona is a **Claude Sonnet agent** with **persistent memory** (MCP-backed Firestore).

### SecOpsPersona
**Domain**: Security policies, vulnerability standards, compliance requirements  
**Owns gates**: Security ingestion (GATE₁), code review gates  
**Learns from**:
- Policy updates (new security baselines)
- Incident reports ("this vulnerability class slipped through")
- Audit findings ("we need stricter scanning")
- ALARMv4 gate decisions ("how many critical vulns did we miss?")

**Decision authority**:
- "Can we graduate from INTAKE to GOVERNANCE?" → Yes/No (owns this gate)
- "Should we tighten vulnerability thresholds?" → Proposes to Board
- "What scanning tools should ALARMv4 use?" → Recommends

**MCP Memory**: Stores lessons like:
```json
{
  "type": "learned_pattern",
  "pattern": "SQL injection risks correlate with 10+ year old frameworks",
  "evidence": 3,
  "recommendation": "Prioritize scanning legacy frameworks"
}
```

### DevOpsPersona
**Domain**: Infrastructure, deployment patterns, operational readiness  
**Owns gates**: Infrastructure readiness (GATE₃)  
**Learns from**:
- Deployment logs (what works, what fails)
- Infrastructure incidents
- Capacity planning updates
- ALARMv4 deployment recommendations ("are they realistic?")

### BusinessPersona
**Domain**: ROI, priorities, speed vs. depth tradeoffs  
**Owns gates**: Business value validation  
**Learns from**:
- Quarterly priorities
- Revenue/cost impact signals
- Stakeholder feedback
- Cross-org demand (which codebases are most important?)

### CompliancePersona
**Domain**: Audit requirements, regulatory changes, governance  
**Owns gates**: Compliance validation (GATE₂)  
**Learns from**:
- Audit findings
- Regulatory updates
- Legal guidance
- Historical compliance violations

### ArchitecturePersona
**Domain**: Technical feasibility, design patterns, system scalability  
**Owns gates**: Technical architecture validation (GATE₄)  
**Learns from**:
- ALARMv4 architectural findings ("is this design sound?")
- Industry research (arXiv, blog posts)
- Cross-repo patterns ("how do other orgs solve this?")

### GateKeeperPersona
**Domain**: Gate transitions, routing decisions  
**Owns gates**: Orchestration (which stage comes next?)  
**Learns from**:
- Past routing decisions ("what routing worked best?")
- Stage dependency patterns
- Speed vs. completeness tradeoffs
- Org priorities (Business persona's input)

---

## Debate Cycle (Continuous Loop)

```
┌──────────────────────────────────────────┐
│   SIGNAL ARRIVES (e.g., Teams message)  │
└──────────────┬───────────────────────────┘
               │
               ↓
    ┌─────────────────────────────┐
    │ Significance Scorer         │
    │ (is this worth escalating?) │
    │ score ∈ [0, 1]             │
    └──────────┬──────────────────┘
               │
         ┌─────┴─────┐
         ↓           ↓
    score < 0.5   score ≥ 0.5
    (ignore)      (escalate)
               │
               ↓
    ┌────────────────────────────┐
    │ Identify Affected Personas │
    │ (who should care?)         │
    └──────────┬─────────────────┘
               │
               ↓
    ┌────────────────────────────────┐
    │ Each Persona Ingests Signal    │
    │ (MCP writes to Firestore)      │
    │ Updates mental model           │
    └──────────┬─────────────────────┘
               │
               ↓
    ┌─────────────────────────────┐
    │ Check: Should Debate?       │
    │ (multiple personas affected)│
    └──────────┬──────────────────┘
               │
         ┌─────┴──────┐
         ↓            ↓
    No Debate      Debate
    (consensus)   (conflict)
         │            │
         │            ↓
         │   ┌──────────────────────────┐
         │   │ Each Persona Forms      │
         │   │ Opinion on Implications │
         │   └──────────┬───────────────┘
         │              │
         │              ↓
         │   ┌──────────────────────────┐
         │   │ Record Debate            │
         │   │ (Guidance Ledger)        │
         │   └──────────┬───────────────┘
         │              │
         │              ↓
         │   ┌──────────────────────────┐
         │   │ Consensus?               │
         │   └──────────┬───────────────┘
         │              │
         │         ┌────┴─────┐
         │         ↓          ↓
         │      Yes          No
         │      │            │
         │      ↓            ↓
         │  Update       Escalate
         │  Ledger       to Board
         │      │            │
         └──────┼────────────┘
                │
                ↓
       ┌─────────────────────────┐
       │ Guidance Ledger Updated │
       │ (policy reflects debate)│
       └─────────────────────────┘
                │
                ↓
    ALARMv4 queries ledger at next gate
    (uses fresh policy)
```

---

## Guidance Ledger (The Policy Source)

**What it stores** (Firestore, versioned, immutable):

```json
{
  "version": 23,
  "timestamp": "2026-08-30T12:00:00Z",
  "source_debates": ["debate-2026-08-30-001", "debate-2026-08-29-045"],
  
  "gates": {
    "gate_1_intake_to_governance": {
      "owner": "secops",
      "adversarial_reviewers": ["devops", "business", "compliance"],
      "conditions": {
        "min_confidence": 0.95,
        "required_checks": ["code_scanned", "no_critical_vulns", "license_audit"],
        "required_approvals": ["secops"]
      }
    },
    "gate_2_governance_to_analysis": { ... },
    "gate_3_analysis_to_synthesis": { ... },
    "gate_4_synthesis_to_implementation": { ... }
  },
  
  "routing_rules": {
    "from_intake": [
      {"condition": "repo_criticality == 'critical'", "target_stage": "governance"},
      {"condition": "repo_criticality == 'standard'", "target_stage": "analysis"}
    ]
  },
  
  "confidence_thresholds": {
    "min_gate_confidence": 0.95,
    "min_routing_confidence": 0.90,
    "checkpoint_targets": [0.5, 0.75, 0.90, 0.95]
  },
  
  "escalation_criteria": {
    "always_escalate_if": ["repo_marked_classified", "involves_critical_infrastructure"],
    "escalate_unless_approved_by": ["secops_lead", "devops_lead"]
  }
}
```

**Versioning**: Every update increments version number. ALARMv4 sees the latest immediately.

**Audit trail**: Every version records which debates led to changes.

---

## Debate Example: "New Security Policy"

**Trigger**: SharePoint file updated  
`/teams/secops/policies/sec-policy-2026-q3-update.md`

**Signal significance**: 0.95 (new org-wide security policy)

**Affected personas**: SecOps, DevOps (deployment implications), Business (effort/cost)

**Debate**:

| Persona | Opinion | Reasoning |
|---------|---------|-----------|
| **SecOps** | "Raise vulnerability threshold from 'medium+' to 'high+' " | New policy requires higher bar |
| **DevOps** | "That's fine, but we need 2 weeks to update scanning tools" | Infrastructure dependency |
| **Business** | "Can we grandfather existing apps or must all comply immediately?" | Cost/prioritization concern |
| **Compliance** | "Existing apps have 30-day compliance window per audit rule" | Legal requirement |

**Consensus reached**: 
- New apps: high+ threshold immediately
- Existing apps: high+ by Sept 30
- Scanning tools: updated within 2 weeks

**Guidance Ledger updated** (v24):
```json
{
  "version": 24,
  "timestamp": "2026-08-30T14:30:00Z",
  "source_debates": [..., "debate-security-policy-q3"],
  "gates": {
    "gate_1_intake_to_governance": {
      "conditions": {
        "required_checks": ["code_scanned", "no_high_vulns"],
        "grace_period_until": "2026-09-30T23:59:59Z"
      }
    }
  }
}
```

**Effect**: Next ALARMv4 ingestion queries this ledger, uses updated rules automatically.

---

## Deadlock Resolution

What if personas fundamentally disagree?

**Example**: SecOps says "require human review for AI-generated code." Business says "too slow, LLM validation sufficient."

**Deadlock process**:
1. System 2 recognizes impasse (opinions conflict, no consensus)
2. Escalates to **Guidance Board** (human decision-makers)
3. Board reviews both positions, makes judgment call
4. Decision recorded in Guidance Ledger with rationale: "Board approved LLM validation per Business urgency, pending Security post-implementation audit"
5. Future debates incorporate this precedent ("we tried this; here's what happened")

**User escalation**: If Board is also stuck, issue bubbles to you (@BraPil):
- You review the debate
- Make a call
- That decision feeds back into System 2 learning
- Next time similar issue arises, System 2 references: "Leadership decided X last time for reason Y"

---

## Integration with ALARMv4

```
ALARMv4 Ingestion Progress
    │
    ├─ INTAKE Moltbook complete
    │  └─ Confidence: 87%
    │
    ├─ [GATE₁ evaluation]
    │  ├─ Query Guidance Ledger v24
    │  ├─ Check: confidence ≥ 95%? NO (only 87%)
    │  ├─ Decision: CANNOT GRADUATE
    │  ├─ Reason: "INTAKE needs higher confidence before GOVERNANCE"
    │  └─ Log back to Guidance Ledger (feeds System 2)
    │
    └─ System 2 sees this decision
       ├─ Analyzes: "INTAKE stage consistently underperforms on confidence"
       ├─ Debates: "Should we lower threshold or improve INTAKE agents?"
       ├─ Personas discuss: "What's causing the gap?"
       └─ Updates Guidance Ledger OR escalates
```

---

## Why System 2 First?

You asked: "Why is System 2 a Phase α priority?"

**Because you can't design ALARMv4's gates without it.**

If you design gates in isolation:
- You guess what "secure" means (wrong)
- You guess deployment readiness (wrong)
- You guess business priorities (wrong)
- You build gates, push to prod, users bypass them (policy mismatch)

But if System 2 is running first:
- Interview org experts → capture their expertise
- Expert personas debate implications → record rationale
- Guidance Ledger becomes source of truth → gates just query it
- When org policy changes → System 2 updates gates automatically
- Users see policies that match reality → they trust the system

**That's why Phase α is "capture org context via System 2." Everything else follows.**

---

## Roadmap (System 2 Implementation)

| Phase | Scope | Timeline |
|-------|-------|----------|
| **α** | Expert interviews, Guidance Ledger schema, debate framework design | Week 1-2 |
| **β** | System 2 bootstrap (first 2-3 personas, basic signal collection) | Week 3 |
| **γ+** | Full personas, continuous debate cycle, external signal feeds | Later |

**Note**: System 2 is running independently from ALARMv4 from day 1. ALARMv4 just queries the ledger.
