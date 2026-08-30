# Gates & Routing: Expert-Owned Decision Architecture

**Phase**: α (Planning)  
**Status**: Draft (will refine with expert interviews)  
**Last Updated**: 2026-08-30  

---

## Gate Ownership Matrix

Each **gate** (stage transition) has an **owner** (expert persona) and **adversarial reviewers**:

### GATE₁: INTAKE → GOVERNANCE

**Owner**: **SecOpsPersona** (code ingestion = security risk)  
**Adversarial Reviewers**: DevOps, Business, Compliance, Architecture  

**Why SecOps owns it**:
- We're ingesting potentially untrusted legacy code
- Must scan for vulnerabilities, secrets, license violations
- SecOps expertise = "is this codebase safe to analyze?"

**Gate Conditions** (from Guidance Ledger v24):
```python
can_graduate = (
    confidence >= 0.95
    AND code_scanned == True
    AND critical_vulns == 0
    AND no_exposed_secrets == True
    AND license_audit_passed == True
)
```

**Adversarial Challenges**:
- **DevOps**: "Are we sure our scanning infrastructure can handle this codebase size?"
- **Business**: "Do we have time for security scanning, or should we expedite?"
- **Compliance**: "Does this codebase have regulatory implications we should know about upfront?"
- **Architecture**: "Are there architectural red flags we should flag before moving to governance?"

**Owner's Response**: SecOps incorporates feedback but owns the final call. "I'm confident on security, DevOps concerns are valid but not blocking."

**Deadlock Scenario**: SecOps says "safe to proceed," but DevOps says "we found deployment issues." 
- → SecOps owns gate, so SecOps decides: "Proceed, but flag for DevOps review before IMPLEMENTATION."
- → Decision logged: "Gate₁ passed with DevOps caveat."
- → System 2 learns: "This codebase's DevOps issues should be reviewed early."

---

### GATE₂: GOVERNANCE → ANALYSIS

**Owner**: **CompliancePersona** (governance = compliance risk)  
**Adversarial Reviewers**: SecOps, DevOps, Business, Architecture  

**Why Compliance owns it**:
- Governance stage validates compliance requirements
- Compliance expertise = "does this codebase meet org/regulatory requirements?"
- Compliance approval is typically mandatory before deep analysis

**Gate Conditions**:
```python
can_graduate = (
    confidence >= 0.95
    AND audit_trail_complete == True
    AND regulatory_checklist_passed == True
    AND compliance_approval_received == True
    AND governance_documentation_complete == True
)
```

**Adversarial Challenges**:
- **SecOps**: "Are there compliance-adjacent security implications?"
- **Business**: "Is this going to require org process changes?"
- **DevOps**: "Are there infrastructure compliance requirements?"
- **Architecture**: "Is the architecture compliant with org standards?"

---

### GATE₃: GOVERNANCE → ANALYSIS (or alternate routing)

**Owner**: **DevOpsPersona** (infrastructure readiness)  
**Adversarial Reviewers**: SecOps, Business, Architecture  

**Why DevOps owns it**:
- Analysis stage requires infrastructure resources (parsing, embeddings, RAG)
- DevOps expertise = "are we ready to scale?"

**Gate Conditions**:
```python
can_graduate = (
    confidence >= 0.95
    AND infrastructure_capacity >= required_capacity
    AND deployment_environment_ready == True
    AND monitoring_and_logging_configured == True
)
```

---

### GATE₄: SYNTHESIS → IMPLEMENTATION

**Owner**: **ArchitecturePersona** (code quality & safety)  
**Adversarial Reviewers**: SecOps, DevOps, Business  

**Why Architecture owns it**:
- Implementation means we're writing code (high risk)
- Architecture expertise = "is this design safe, sound, valuable?"

**Gate Conditions**:
```python
can_graduate = (
    confidence >= 0.95
    AND evaluator_verdict in ["accept", "revise_minor"]
    AND implementation_plan_sound == True
    AND risk_assessment_complete == True
    AND no_critical_architectural_issues == True
)
```

---

### ROUTING: INTAKE → ?

**Owner**: **GateKeeperPersona** (orchestration decisions)  
**Adversarial Reviewers**: All (each has input on priorities)  

**Routing Logic** (from Guidance Ledger):

```python
if repo_marked_as_critical:
    next_stage = GOVERNANCE  # Compliance first
elif repo_involves_security_sensitive_data:
    next_stage = GOVERNANCE  # SecOps review first
elif org_priority == "cloud_migration":
    next_stage = ANALYSIS    # Skip governance, speed matters
else:
    next_stage = GOVERNANCE  # Default (safest)
```

**Adversarial Input**:
- **SecOps**: "Is skipping governance safe?"
- **Business**: "What's the speed gain?"
- **DevOps**: "Can we parallelize stages?"
- **Compliance**: "Are we meeting audit requirements?"

**GateKeeper Decision**: "Based on Business priority and Compliance approval, route to ANALYSIS with mandatory SecOps async review."

---

## Confidence Scoring (Per Gate)

Each gate has a **confidence threshold** (currently 95%). Confidence grows through:

```
INTAKE Moltbook (agents analyzing repo)
    │
    ├─ Discovery Agent: "Found 4,532 files, all parseable"
    │  └─ Confidence +0.10 → 0.10 total
    │
    ├─ Security Scanner Agent: "Scanned all files, 0 critical vulns"
    │  └─ Confidence +0.15 → 0.25 total
    │
    ├─ License Audit Agent: "All licenses compliant"
    │  └─ Confidence +0.10 → 0.35 total
    │
    ├─ Secret Detection Agent: "No exposed secrets found"
    │  └─ Confidence +0.15 → 0.50 total
    │
    ├─ Complexity Analyzer: "Complexity map complete, no anomalies"
    │  └─ Confidence +0.20 → 0.70 total
    │
    └─ Consistency Checker: "All agents agree, no gaps detected"
       └─ Confidence +0.25 → 0.95 total

GATE₁ EVALUATION:
    confidence = 0.95 ≥ 0.95 threshold ✓
    Can graduate to GOVERNANCE
```

**Why transparent confidence?**
- User sees how confident we are
- User sees what blocks graduation ("complexity analyzer at 0.70, needs improvement")
- User can intervene: "Force graduation anyway" → logs as escalation

---

## Gate Decision Log

Every gate decision is **logged to Guidance Ledger** (feeds System 2):

```json
{
  "session_id": "sess-2026-08-30-001",
  "gate_id": "gate_1",
  "timestamp": "2026-08-30T14:45:00Z",
  "owner_persona": "secops",
  "decision": "APPROVED",
  "confidence": 0.95,
  "conditions_checked": {
    "code_scanned": true,
    "critical_vulns": 0,
    "license_audit_passed": true,
    "no_secrets": true
  },
  "adversarial_feedback": {
    "devops": {
      "concern": "Repo size (2GB) might stress scanning infrastructure",
      "severity": 2,
      "suggested_condition": "Monitor resource usage during GOVERNANCE"
    },
    "business": {
      "concern": "None",
      "severity": 0
    }
  },
  "owner_response": "Approved. DevOps concern noted; we'll monitor closely. Not blocking graduation.",
  "guidance_ledger_version_used": 24,
  "audit_trail": {
    "gate_opened": "2026-08-30T10:00:00Z",
    "gate_closed": "2026-08-30T14:45:00Z",
    "duration_minutes": 285
  }
}
```

**System 2 sees this and learns**:
- "Repo size 2GB triggers resource concerns. Note this for future large-repo handling."
- "This repo type graduated quickly (285 min). What made it smooth?"
- "DevOps repeatedly flags infrastructure concerns. Should we tighten gate conditions?"

---

## Adversarial Review Pattern

When owner persona makes a decision:

```
1. Owner forms opinion
   └─ SecOps: "This code is secure; confidence 0.95"

2. Adversarial personas challenge
   ├─ DevOps: "But what about deployment surface?"
   ├─ Business: "Can we speed this up?"
   └─ Compliance: "What about data privacy?"

3. Owner incorporates feedback (or explicitly rejects)
   └─ SecOps: "DevOps point valid; adding deployment review note.
                Business: speed not in my domain; defer to GateKeeper.
                Compliance: privacy check passed; approved."

4. Final decision issued
   └─ "GATE₁ APPROVED with DevOps caveat (deployment review required before IMPLEMENTATION)"

5. Logged to Guidance Ledger
   └─ Full debate recorded; System 2 learns from it
```

**This is NOT consensus-seeking.** Owner decides. Others provide perspective.

---

## Escalation to Board

When does a gate decision escalate to the Board?

**Automatic escalation**:
- Repo marked as critical/classified
- Confidence can't reach 95% (stuck at 87%)
- Multiple personas strongly object (severity ≥ 4/5)
- Security vs. Speed conflict (can't reconcile)

**Manual escalation**:
- Owner persona: "I'm uncertain; need Board input"
- Adversarial reviewer: "This is above my pay grade"
- User intervention: "I want Board review before proceeding"

**Board review**:
- Reviews full debate log (System 2 provides)
- Makes judgment call
- Decision recorded with rationale
- Feeds into System 2 learning

---

## Guidance Ledger Schema (Firestore)

```
collection: guidance_ledger_versions
  doc: v001
    ├─ timestamp: <datetime>
    ├─ gates: {gate_1: {...}, gate_2: {...}, ...}
    ├─ routing_rules: {...}
    ├─ confidence_thresholds: {...}
    └─ source_debates: ["debate-id-1", "debate-id-2", ...]
  
  doc: v002
    ├─ timestamp: <datetime>
    ├─ gates: {gate_1: {...updated...}, ...}
    └─ source_debates: [..., "debate-id-3"]

collection: gate_decisions
  doc: sess-2026-08-30-001__gate_1
    ├─ session_id: "sess-2026-08-30-001"
    ├─ gate_id: "gate_1"
    ├─ owner_persona: "secops"
    ├─ decision: "APPROVED" | "BLOCKED" | "ESCALATED"
    ├─ confidence: 0.95
    ├─ adversarial_feedback: {...}
    └─ timestamp: <datetime>
```

---

## Success Criteria (Gate System)

✅ Expert owner clearly identified for each gate  
✅ Adversarial review pattern documented  
✅ Confidence scoring transparent and measurable  
✅ All decisions logged (audit compliance)  
✅ System 2 learns from gate patterns  
✅ Board can override when escalated  
✅ Users understand why gates block/approve  

---

## Next Steps (Phase β)

1. **Implement GateBase class** (abstract gate with owner, reviewers, conditions)
2. **Wire up Guidance Ledger queries** (gates fetch current policy)
3. **Build confidence scoring** (agents track what they've completed)
4. **Test gate evaluations** (mock repo, simulate decisions)
5. **Implement logging** (all decisions → Guidance Ledger)
