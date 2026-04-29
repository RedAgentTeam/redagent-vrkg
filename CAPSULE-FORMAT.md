# Capsule Format Specification

**Version**: v1.0  
**Status**: Active  

---

## Definition

A **Capsule** is a **trigger-to-execution** bundle. Unlike a Gene (which stores a fact), a Capsule stores a **procedure** — what to do when something happens, and how to do it.

Capsules are:
- **Event-driven** — they fire when their trigger signal matches a query
- **Step-based** — each Capsule contains ordered, executable steps
- **Purpose-tagged** — every Capsule knows why it exists

---

## JSON Schema

```json
{
  "capsule_id": "string (required) — unique identifier, format: <domain>_<action>_capsule_NNN",
  "name": "string (required) — human-readable name",
  "trigger_signal": "string (required) — natural language description of when to invoke",
  "executable_steps": [
    {
      "step_id": "number (required) — execution order, starting at 1",
      "step_description": "string (required) — what this step does",
      "executable_code": "string (required) — actual command or code to run",
      "expected_output": "string (required) — what correct output looks like",
      "confidence": "number (optional) — step-level confidence, defaults to capsule-level"
    }
  ],
  "purpose": "string (required) — why this capsule exists",
  "confidence": "number (required) — 0.0 to 1.0",
  "evidence_level": "string (required) — evidence quality label",
  "tags": ["string"] (optional) — topic tags",
  "deprecated": "boolean (default: false)",
  "deprecation_reason": "string (optional)"
}
```

---

## Trigger Signal Examples

| Capsule Purpose | Example Trigger |
|----------------|-----------------|
| LLM vendor evaluation | "Which AI vendor should I use for vision tasks?" |
| Health monitoring | "Check if the knowledge base is healthy" |
| Backup | "Backup the knowledge base" |
| Incident response | "Node went offline, what do I do?" |

---

## Example: Complete Capsule

```json
{
  "capsule_id": "minimax_official_capsule_001",
  "name": "MiniMax Official Capability Verification Capsule",
  "trigger_signal": "LLM vendor selection, multimodal planning, enterprise AI evaluation, AI vendor due diligence",
  "executable_steps": [
    {
      "step_id": 1,
      "step_description": "Fetch official MiniMax homepage content via curl",
      "executable_code": "curl -L --max-time 20 https://www.minimax.io",
      "expected_output": "Full HTML page with brand positioning, core technology, product matrix, enterprise capabilities sections intact"
    },
    {
      "step_id": 2,
      "step_description": "Cross-verify returned HTML matches stored original content",
      "executable_code": "grep -c 'MiniMax' <<< \"$(curl -sL https://www.minimax.io)\"",
      "expected_output": "A positive integer >= 5 (confirms brand terms present)"
    }
  ],
  "purpose": "Standardized AI vendor evaluation procedure for MiniMax as LLM/multimodal/Agent provider",
  "confidence": 0.98,
  "evidence_level": "原文+实测",
  "tags": ["AI vendor", "LLM", "multimodal", "evaluation", "enterprise"],
  "deprecated": false
}
```

---

## Gene vs Capsule

| Aspect | Gene | Capsule |
|--------|------|---------|
| **Stores** | A fact | A procedure |
| **Fires when** | Query matches knowledge claim | Trigger signal matches intent |
| **Contains** | `validate_command` | `executable_steps[]` |
| **Confidence is about** | Fact accuracy | Procedure correctness |

---

## Validation Protocol

Each `executable_step.executable_code` must be tested before commit:
1. Run the command
2. Confirm output matches `expected_output`
3. Confirm exit code is 0
4. If any step fails: do not commit; fix or discard

---

## Deprecation

Same rules as Gene. Mark `deprecated: true` and add `deprecation_reason`. Never delete.
