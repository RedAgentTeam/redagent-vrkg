# Knowledge Sampling & Curation Standard Operating Procedures

**Version**: v2.1  
**Status**: Active  
**Maintained by**: Red Agent Team  

---

## Overview

Knowledge sampling is the process of acquiring raw information from external sources, validating it against the original, and solidifying it into verifiable knowledge assets within the VRKG system.

This SOP defines the mandatory workflow, quality gates, and format requirements for every knowledge unit entering the system.

---

## The Four-Phase Pipeline

```
Phase 1: Raw Sampling     → Get the original, unmodified content
Phase 2: Evidence Locking  → Pair content with its source URL/command
Phase 3: Gene/Capsule      → Transform into VRKG-native format
Phase 4: Commit            → Store in the knowledge base with citation
```

---

## Phase 1: Raw Sampling

### What to Sample

- Official documentation pages
- API references and technical specs
- Product announcements and changelogs
- Service descriptions and capability statements
- Any publicly verifiable factual content

### Sampling Rules

1. **Copy verbatim**: No rephrasing, no summarizing, no interpreting. What you read is what you store.
2. **Keep metadata**: Source URL, access timestamp, page title, section context.
3. **Include command evidence**: For web content, include the `curl` command that fetched it.
4. **Capture structure**: Preserve headings, lists, tables as-is. Do not flatten.

### Prohibited

- Summarized content (use original or don't use)
- Third-party interpretations of data (go to the source)
- Social media posts or unverified claims
- Content behind authentication without valid access

---

## Phase 2: Evidence Locking

### Required Evidence Fields

| Field | Description |
|-------|-------------|
| `source_url` | Original URL or location |
| `source_type` | `web_page`, `api_response`, `document`, `command_output` |
| `access_timestamp` | When the content was retrieved |
| `validate_command` | Executable command that reproduces the content |
| `raw_output` | Actual output from the validation command |

### Validation Standard

A knowledge unit **fails validation** if:
- The `validate_command` does not execute successfully
- The `raw_output` differs from the stored original content
- The source URL returns 404 or access denied

A knowledge unit **passes validation** if:
- Command executes with exit code 0
- Output matches stored content byte-for-byte or semantically identical

---

## Phase 3: Gene & Capsule Transformation

### When to Use Gene

Gene is for **atomic facts**: a single capability statement, a pricing rule, a function signature, a product name.

Gene structure:
```json
{
  "gene_id": "<domain>_<concept>_001",
  "name": "Human-readable name",
  "description": "Plain-language description of the fact",
  "source_url": "https://...",
  "validate_command": "...",
  "validate_output": "...",
  "confidence": 1.0,
  "evidence_level": "原文+实测 | 原文 | 第三方"
}
```

### When to Use Capsule

Capsule is for **procedural knowledge**: steps to achieve something, a trigger-to-action mapping, a multi-step process.

Capsule structure:
```json
{
  "capsule_id": "<domain>_<action>_001",
  "trigger_signal": "When to invoke this capsule",
  "executable_steps": [
    {
      "step_id": 1,
      "step_description": "What this step does",
      "executable_code": "actual command or code",
      "expected_output": "what correct execution looks like"
    }
  ],
  "purpose": "Why this capsule exists"
}
```

---

## Phase 4: Commit & Index

### Commit Rules

1. Every commit must include:
   - The knowledge unit file
   - Any raw evidence files
   - Updated index entries

2. Commit message format:
   ```
   feat: <domain> — <brief description>
   
   <longer explanation if needed>
   
   Evidence: <validate_command>
   Confidence: <score>
   ```

3. Never overwrite existing Gene/Capsule without adding a new entry. Keep history.

### Index Update

After committing, update the relevant index file:
- `index.md` — top-level navigation
- `wiki/index.md` — knowledge domain index
- `raw/index.md` — raw source index

---

## Confidence Score Guidelines

| Score | When to Assign |
|-------|---------------|
| **1.0** | Original text + field verification (command executed successfully) |
| **0.9** | Original text + not field-tested but source is authoritative |
| **0.7–0.8** | Authoritative source but content is conceptual (no executable proof) |
| **0.5–0.6** | Third-party source, not directly verifiable |
| **< 0.5** | Do not use for Gene/Capsule; store as raw reference only |

---

## Review Checklist

Before committing, verify:

- [ ] Source URL is accessible and returns the claimed content
- [ ] `validate_command` executes successfully
- [ ] `raw_output` matches stored content
- [ ] Confidence score is assigned per guidelines
- [ ] Index files are updated
- [ ] Commit message follows standard format

---

## Knowledge Expiry

Knowledge units are not permanent. They expire when:

1. **Source URL returns different content** — confidence drops to 0.5, flagged for review
2. **Validation command fails** — unit is quarantined, not served in queries
3. **Manual override** — curator explicitly marks as deprecated

Re-validation runs on a schedule defined in `health-check.sh`.

---

## Related Documents

- [GENE-FORMAT.md](./GENE-FORMAT.md)
- [CAPSULE-FORMAT.md](./CAPSULE-FORMAT.md)
- [CONCEPT-DICTIONARY.md](./CONCEPT-DICTIONARY.md)
