# Domain 03: Primitive & Synthesis Worker Tiers
**Governed Multi-Agent Decision-Support Substrate (GMADSS) Specification**

## 1. Executive Summary & Worker Taxonomy
The **Governed Multi-Agent Decision-Support Substrate (GMADSS)** achieves deterministic reliability by decomposing complex administrative analysis into discrete, single-purpose worker micro-agents. Rather than employing an unconstrained, all-in-one general model, GMADSS divides systemic execution into two strict, isolated functional tiers:

1. **Primitive Worker Tier:** Bounded micro-agents with narrowed, single-task execution contracts (e.g., entity extraction, document parsing, rigid policy clause lookup).
2. **Synthesis Worker Tier:** Higher-order analytical units tasked with cross-referencing primitive outputs against the non-negotiable, systemic **Authority Hierarchy**.

---

## 2. The Worker Contract Registry (Operational Specifications)
Every agent operating within GMADSS must strictly adhere to a deterministic **Worker Contract**. A contract programmatically defines the agent's explicit input parameters, bounded output validation schemas, allowed tool boundaries, and forbidden operations.

### 2.1 Primitive Worker Tier Specifications

#### Worker 01: Statutory Legal Extractor (`worker-prim-statute`)
* **Role:** Parses active legislative acts, statutory regulations, and legal charters to extract verbatim legal constraints.
* **Input Schema:**
  ```json
  {
    "session_id": "STRING",
    "deidentified_query_tokens": ["STRING"],
    "target_statutory_corpus_hash": "SHA256_HASH"
  }
  ```
* **Output Schema:**
  ```json
  {
    "extracted_clauses": [
      {
        "act_name": "STRING",
        "section_id": "STRING",
        "verbatim_text_span": "STRING",
        "mandatory_obligation_markers": ["STRING"]
      }
    ]
  }
  ```
* **Forbidden Operations:** Extrapolating beyond literal text spans, offering operational advice, interpreting legislative intent, or attempting to autonomously resolve linguistic ambiguity.

#### Worker 02: Operational Policy Matcher (`worker-prim-policy`)
* **Role:** Maps administrative query parameters against departmental operational directives, practice manuals, and localized rulebooks.
* **Input Schema:**
  ```json
  {
    "session_id": "STRING",
    "query_metadata_tags": ["STRING"],
    "policy_vector_index_id": "STRING"
  }
  ```
* **Output Schema:**
  ```json
  {
    "matched_policy_segments": [
      {
        "manual_title": "STRING",
        "chapter_section": "STRING",
        "procedural_checklist_requirements": ["STRING"],
        "mandatory_documentation_fields": ["STRING"]
      }
    ]
  }
  ```
* **Forbidden Operations:** Overriding statutory level constraints, ignoring missing documentation flags, or autonomously generating case-level operational instructions.

---

### 2.2 Synthesis Worker Tier Specifications

#### Worker 03: Authority Precedence Synthesizer (`worker-synth-precedence`)
* **Role:** Integrates the structured outputs from active Primitive Workers and systematically evaluates them against the Authority Hierarchy (Legislation > Policy > Manuals).
* **Input Schema:** Consolidated JSON arrays from `worker-prim-statute` and `worker-prim-policy`.
* **Output Schema:**
  ```json
  {
    "unified_policy_alignment_matrix": "OBJECT",
    "precedence_resolution_status": "ENUM [CLEAR_ALIGNMENT, CONTESTED_DIVIDED]",
    "authority_divergence_flags": [
      {
        "flag_type": "[CONTESTED_AUTHORITY_FLAG]",
        "statute_reference": "STRING",
        "conflicting_policy_reference": "STRING",
        "contradiction_description": "STRING"
      }
    ]
  }
  ```
* **Execution Boundary:** Must explicitly trigger and output a `[CONTESTED_AUTHORITY_FLAG]` if a lower-level manual or operational directive diverges from, minimizes, or contradicts a higher-level statute.

#### Worker 04: Documentation Clarity & Compliance Auditor (`worker-synth-clarity`)
* **Role:** Evaluates administrative draft notes, templates, or compliance documentation to verify structural alignment with statutory and policy reporting standards.
* **Input Schema:** Anonymized human draft text + required procedural verification checklists passed from `worker-synth-precedence`.
* **Output Schema:**
  ```json
  {
    "documentation_gap_analysis_report": {
      "missing_required_fields": ["STRING"],
      "ambiguous_temporal_timelines": ["STRING"],
      "unaddressed_policy_steps": ["STRING"]
    }
  }
  ```
* **Execution Boundary:** Bounded to read-only feedback loops. The worker is completely disabled from auto-correcting, editing, or overwriting the human professional's independent notes.

---

## 3. Inter-Worker Execution Architecture

```text
┌─────────────────────────┐
│   De-Identified Query   │
└────────────┬────────────┘
             │
 ┌───────────┴───────────┐
 ▼                       ▼
┌─────────────────────────────┐                 ┌─────────────────────────────┐
│    worker-prim-statute      │                 │     worker-prim-policy      │
│ (Statutory Clause Extraction)│                 │ (Policy Manual Index Lookups)│
└──────────────┬──────────────┘                 └──────────────┬──────────────┘
               │                                               │
               └───────────────────────┬───────────────────────┘
                                       ▼
                        ┌─────────────────────────────┐
                        │   worker-synth-precedence   │
                        │ (Authority Hierarchy Map)   │
                        └──────────────┬──────────────┘
                                       │
                                       ▼
                        ┌─────────────────────────────┐
                        │    worker-synth-clarity     │
                        │  (Documentation Gap Check)  │
                        └──────────────┬──────────────┘
                                       │
                                       ▼
                        ┌─────────────────────────────┐
                        │   Outbound to Critic Loop   │
                        └─────────────────────────────┘
```

---

## 4. Contract Enforcement & Failure Protocols

1. **Schema Validation Enforcer:** If any primitive or synthesis worker produces data payloads falling outside its explicitly defined JSON schema parameters, the entire message package is dropped, and a system exception is logged.
2. **Hallucination Containment:** Workers are restricted from generating references, spans, or indices not actively present in the verified, hashed database index. The detection of an ungrounded anchor automatically terminates the active worker thread.
3. **Execution Determinism:** Temperature parameters for all primitive-tier workers are locked to exactly `0.0`. Top-P metrics are set to `1.0`. This guarantees completely repeatable, deterministic text parsing across all query states.
