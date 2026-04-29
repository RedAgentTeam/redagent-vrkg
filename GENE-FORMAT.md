# Gene Format Specification

**Version**: v1.0  
**Status**: Active  

---

## Definition

A **Gene** is the atomic unit of knowledge in the VRKG system. It represents a single, verifiable, factual knowledge claim — stored together with its verification method.

Genes are:
- **Immutable once committed** — they represent the knowledge as it existed at sampling time
- **Self-verifying** — each Gene carries the command to check if it's still valid
- **Confidence-scored** — every Gene has an explicit confidence score

---

## JSON Schema

```json
{
  "gene_id": "string (required) — unique identifier, format: <domain>_<concept>_NNN",
  "name": "string (required) — human-readable name",
  "description": "string (required) — plain-language description of the knowledge fact",
  "source_url": "string (required) — original source URL or location",
  "source_type": "string (required) — web_page | api_response | document | command_output",
  "access_timestamp": "string (required) — ISO 8601 timestamp of when content was retrieved",
  "validate_command": "string (required) — executable command that reproduces the source content",
  "validate_output": "string (required) — expected output from validate_command",
  "raw_content": "string (optional) — verbatim original text if available",
  "confidence": "number (required) — 0.0 to 1.0",
  "evidence_level": "string (required) — evidence quality label",
  "tags": ["string"] (optional) — topic tags for retrieval",
  "deprecated": "boolean (default: false) — if true, Gene is quarantined",
  "deprecation_reason": "string (optional) — why this Gene was deprecated"
}
```

---

## Field Definitions

### gene_id
Format: `<domain>_<concept>_<3-digit-sequence>`

Examples:
- `minimax_official_gene_001`
- `openclaw_cron_task_sop_001`
- `evomap_node_heartbeat_001`

### source_type
| Value | When to Use |
|-------|-------------|
| `web_page` | Content retrieved via HTTP |
| `api_response` | JSON/XML response from an API |
| `document` | PDF, DOCX, or file-based content |
| `command_output` | Output from a CLI command |

### evidence_level
| Label | Meaning |
|-------|---------|
| `原文+实测` | Original text + command verified successfully |
| `原文` | Original text captured, not field-tested |
| `第三方` | Third-party source, not directly verifiable |

### confidence
| Range | Meaning |
|-------|---------|
| 1.0 | Exact original + verified — primary source |
| 0.8–0.9 | Original, authoritative source, not field-tested |
| 0.5–0.7 | Third-party, needs verification |
| < 0.5 | Do not use in Gene; store as raw reference only |

---

## Example: Complete Gene

```json
{
  "gene_id": "minimax_official_gene_001",
  "name": "MiniMax Official Capability Overview",
  "description": "MiniMax is a general AI R&D company focused on LLM, multimodal, Agent, and long-context capabilities. Provides API, agent tools, knowledge base hosting, and private deployment.",
  "source_url": "https://www.minimax.io",
  "source_type": "web_page",
  "access_timestamp": "2026-04-29T05:00:00+08:00",
  "validate_command": "curl -L --max-time 20 https://www.minimax.io",
  "validate_output": "HTML returns full homepage with brand positioning, core technology, product matrix, and enterprise capabilities intact",
  "confidence": 1.0,
  "evidence_level": "原文+实测",
  "tags": ["AI vendor", "LLM", "multimodal", "agent", "enterprise"],
  "deprecated": false
}
```

---

## Validation Protocol

Every Gene **must be validated** before commit:

1. Execute `validate_command`
2. Confirm command exits with code 0
3. Confirm `validate_output` matches actual command output
4. If validation fails: do not commit; fix or discard

---

## Naming Conventions

- Gene IDs: lowercase, underscores, numeric suffix
- Tags: lowercase, underscores, no spaces
- Timestamps: ISO 8601 with timezone
- Confidence: two decimal places (e.g., 0.98, not 1)

---

## Deprecation

A Gene is deprecated when:
- Source URL returns 404 or substantially different content
- Validation command fails on re-run
- A newer, more accurate Gene replaces it

Deprecation is marked by setting `deprecated: true` and adding `deprecation_reason`. **Never delete** a deprecated Gene — its history must be preserved.
