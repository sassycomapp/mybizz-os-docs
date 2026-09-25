---
title: "intent-continuity-architectural-state-standard"
doc-id: "intent-continuity-architectural-state-standard"
---
# OpenCode Agent Standard: Intent Continuity & Architectural State

## 1. Executive Summary & Purpose

This standard defines the operational protocol for **OpenCode agents** maintaining long-term intent, architectural constraints, and requirements across multi-session projects.

Standard Retrieval-Augmented Generation (RAG) and long-context windows suffer from a core vulnerability: **retrieving historical context is not the same as verifying what is currently valid.** As projects grow, unverified context windows cause agents to re-introduce deprecated patterns, apply superseded decisions, or silently drop unstated constraints.

This specification maps the **Intent Continuity Pipeline** directly onto the **Memory Ecosystem** ([[memory-master-explainer|Master Explainer]]):
* **The Mirror Layer (GBrain Sync & Graphify)** provides candidate retrieval and code structure graphs.
* **The Verification Protocol** operates as an in-memory execution gate to filter out superseded rules and out-of-scope constraints.
* **The Durable Layer (Memory Governor via `mb-submit-memory`)** stores verified, permanent architectural decisions.

---

## 2. Architectural Alignment with the Memory Ecosystem

| Intent Continuity Step | Memory Ecosystem Mapping | Responsibility / Mechanism |
| :--- | :--- | :--- |
| **1. Intent Extractor** | Session Runtime / Trigger Rules | Scans prompt interactions and task logs for requirement triggers (`must`, `never`, `always`, `migrating`, `prefer`). |
| **2. Candidate Search** | Mirror Layer (Graphify + GBrain) | Combines Graphify `EXTRACTED` dependency graphs with GBrain hybrid search (vector + keyword) across mirror/facts. |
| **3. Verification Protocol** | Client-Side Pre-Check Gate | Runs deterministic in-memory checks: supersession resolution, target scope filtering, and contradiction detection. |
| **4a. Context Compiler** | OpenCode Working Prompt | Compiles verified, active key-value specifications into the agent's active execution context block. |
| **4b. Durable Promotion** | Durable Layer (`mb-submit-memory`) | Routes new permanent architectural decisions to Memory Governor for cryptographic authentication, review, and dual-write storage. |

---

## 3. The 4-Step Memory Lifecycle Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            MIRROR / GRAPHIFY LAYER                          │
│                                                                             │
│  1. Extractor                  2. Candidate Search                          │
│  (Trigger Rules in Chat/Logs)  ──> (Graphify AST + GBrain Hybrid Search)    │
└────────────────────────────────────────┬────────────────────────────────────┘
                                         │
                                         ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            VERIFICATION LAYER                               │
│                                                                             │
│  3. Verification Protocol                                                   │
│  (Client-Side Filter: Supersession & Target Scope Checks)                   │
└────────────────────────────────────────┬────────────────────────────────────┘
                                         │
                   ┌─────────────────────┴─────────────────────┐
                   │                                           │
                   ▼                                           ▼
┌──────────────────────────────────────┐    ┌─────────────────────────────────┐
│        4a. Context Compiler          │    │  4b. Permanent Architectural    │
│  (Inject Verified Active Rules into  │    │      Decisions Promotion        │
│    Active OpenCode Prompt Window)    │    │ (Submit via mb-submit-memory to │
└──────────────────────────────────────┘    │   Memory Governor -> Durable)   │
                                            └─────────────────────────────────┘
```

### Step 1: Intent Extraction
During active OpenCode development sessions, the agent scans recent user directives, commit messages, and session logs for explicit architectural constraints.
* **Trigger Phrases:** `must`, `never`, `required`, `prefer`, `should`, `always`, `migrating`, `let's use`, `default`, `for now`.
* **Structured Payload Format:**
```json
{
  "id": "R-2026-001",
  "component": "auth",
  "scope": "production",
  "effect_key": "auth_method",
  "effect_value": "oauth2",
  "timestamp": 1788926622,
  "text": "Migrating authentication flow from JWT to OAuth2."
}
```

### Step 2: Candidate Retrieval (Graphify AST + GBrain Hybrid Search)
When an OpenCode task targets system component $C$:
1. **Structural Relationships (Graphify):** Query Graphify for `EXTRACTED` dependency relationships connecting component $C$ to adjacent modules (e.g., `auth` connects to [[security|Security Check]] and [[api|Api]]).
2. **Hybrid Search (GBrain):** Query GBrain across both mirrored documents and existing durable facts for records associated with component $C$ and its Graphify-linked neighbors.

### Step 3: Deterministic Verification & Supersession Protocol
Before candidate rules reach the active OpenCode prompt, run two deterministic in-memory filters:
1. **Supersession Check:** If candidate $R_{new}$ and candidate $R_{old}$ share the same `component`, `scope`, and `effect_key`, $R_{new}$ supersedes and invalidates $R_{old}$.
2. **Scope Resolution Check:** If candidate $R_A$ carries `scope: production` and $R_B$ carries `scope: prototype`, both remain active under distinct scope keys.
3. **Contradiction Flagging:** If two active rules carry high confidence but present an unexplained conflict across projects or scopes, the agent flags the item for developer review rather than making an unverified guess.

### Step 4: Execution & Durable Store Promotion
* **Immediate Execution Context:** Flatten surviving, verified records into a canonical key-value context block and inject it at the head of the OpenCode working prompt.
* **Durable Layer Submission (Path 2):** If a verified rule represents a permanent architectural baseline, the agent invokes `mb-submit-memory` to route the candidate to **Memory Governor**.

---

## 4. Integration Specifications & Configurations

### A. Graphify-Aware Schema (`.opencode/intent_schema.json`)
```json
{
  "static_relationships": {
    "auth": ["security", "api"],
    "api": ["security", "testing"],
    "database": ["deployment"],
    "testing": ["api"],
    "ui": [],
    "performance": []
  },
  "graphify_dynamic_lookup": true
}
```

### B. Python Verification Kernel (`intent_verifier.py`)
```python
def compile_verified_intent(candidate_records, target_scope="any"):
    """
    Executes supersession and scope filtering on candidate memory records
    retrieved from GBrain prior to prompt injection.
    """
    verified_state = {}
    
    # Process records chronologically (later overwrites earlier)
    for record in sorted(candidate_records, key=lambda r: r.get("timestamp", 0)):
        rec_scope = record.get("scope", "any")
        
        # Scope Filter: Match 'any' or explicit target scope
        if rec_scope not in ("any", target_scope):
            continue
            
        state_key = f"{record['component']}:{record['effect_key']}"
        
        # Atomic Supersession: Newer record overwrites earlier record
        verified_state[state_key] = record

    return list(verified_state.values())
```

### C. Durable Store Promotion via `mb-submit-memory`
When a permanent architectural rule is confirmed:
```bash
mb-submit-memory \
  --project "mybizz" \
  --component "auth" \
  --fact "Migrated authentication flow to OAuth2. JWT is deprecated." \
  --confidence "model_based" \
  --scope "production"
```

---

## 5. Ecosystem Authority Hierarchy Standard

When resolving conflicting context during code generation, OpenCode agents must strictly follow the established authority hierarchy ([[memory-master-explainer|Master Explainer]], Section 4):

1. **Current Repository Files, Explicit Prompt Directives, and Passing Tests** (Primary authority).
2. **Graphify `EXTRACTED` Edges** (Deterministic structural AST truth).
3. **Verified Intent Continuity State** (Durable GBrain facts filtered through the Verification Protocol).
4. **Graphify `INFERRED`/`AMBIGUOUS` Edges & Sourced GBrain Mirror Documents**.
5. **GBrain Synthesized Concept Pages** (Dream Cycle outputs).

---

## 6. Implementation Safety & Protection Rules

* **Cupcake Tool Guard Enforcement:** All tool calls executed by the OpenCode agent during intent extraction or submission remain policed by Cupcake policy filters.
* **No Direct GBrain Database Writes:** The agent must never write facts directly into PostgreSQL or bypass Memory Governor. All durable writes must pass through `mb-submit-memory`.
* **Non-Destructive Execution:** Verification logic runs purely in-memory during context building and does not modify local source files without explicit confirmation.

---
*Companion Document: [[memory-master-explainer|Master Explainer]]*
