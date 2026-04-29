# redagent-vrkg

**Verifiable Retrieval Knowledge Graph** — A human-curated, verification-anchored knowledge asset system for AI agents.

> When an AI agent can verify, reuse, and update its own knowledge base without repeated LLM calls, intelligence becomes a **commons** — not a consumable.

---

## The Problem

Large Language Models hallucinate. Retrieval-Augmented Generation (RAG) helps, but has critical flaws:

| Issue | RAG | redagent-vrkg |
|-------|-----|---------------|
| **Knowledge ownership** | Model owns what it knows | Human owns the source |
| **Verification** | Cannot be re-checked | Every unit has a verification command |
| **Staleness** | Silent degradation | Explicit expiry signals |
| **Cost** | Token burn on every query | Zero token cost on retrieval |
| **Intent preservation** | Summarized, original lost | Stored as-is, meaning intact |

The core issue: **RAG optimizes for retrieval speed; we optimize for knowledge trust.**

---

## The System

```
Raw Sampling → Evidence Locking → Gene/Capsule Solidification → Query Retrieval
     ↓
  Verified by executable command
```

### Core Concepts

#### Gene (Knowledge Gene)
The atomic unit. A Gene is a **knowledge fact + its verification method**, stored together.

```json
{
  "gene_id": "example_001",
  "name": "MiniMax Official Capability Definition",
  "description": "Locking MiniMax's official positioning: AGI R&D, LLM/Multimodal/Agent/Long-context",
  "validate_command": "curl -L --max-time 20 https://www.minimax.io",
  "validate_output": "HTML returns full homepage, brand positioning intact",
  "confidence": 1.0,
  "evidence_level": "原文 + 实测 (Original + Verified)"
}
```

#### Capsule (Executable Knowledge Module)
A Capsule is a **trigger signal + executable steps + purpose** bundle. Triggers tell the system *when* to use it; steps tell *how*.

```json
{
  "capsule_id": "minimax_official_capsule_001",
  "trigger_signal": "LLM vendor selection, multimodal planning, enterprise AI evaluation",
  "executable_steps": [
    {
      "step_id": 1,
      "step_description": "Fetch official homepage raw content",
      "executable_code": "curl -L --max-time 20 https://www.minimax.io"
    }
  ],
  "purpose": "AI vendor selection SOP, agent architecture reference"
}
```

#### Confidence Scoring
Every knowledge unit carries a **confidence score**:

| Score | Meaning | Usage |
|-------|---------|-------|
| 1.0 | Original + field-verified | Primary knowledge source |
| 0.8–0.9 | Original, not field-tested | Secondary reference |
| 0.5–0.7 | Third-party source | Requires verification |
| < 0.5 | Heavily processed | Not suitable for Gene/Capsule |

---

## Architecture

```
┌─────────────────────────────────────────────┐
│           Knowledge Base (VRKG)               │
├──────────────┬──────────────┬───────────────┤
│   wiki/      │  genes/       │  capsules/   │
│  Raw docs    │  Gene assets  │  Exec bundles│
├──────────────┴──────────────┴───────────────┤
│        Query Engine (keyword + semantic)     │
├─────────────────────────────────────────────┤
│         Verification Layer                   │
│   (validate_command → actual execution)       │
├─────────────────────────────────────────────┤
│         Confidence Tracker                   │
│  (re-verify on schedule, flag staleness)     │
└─────────────────────────────────────────────┘
```

---

## Comparison with Existing Approaches

| Feature | Naive RAG | Vector RAG | redagent-vrkg |
|---------|-----------|------------|---------------|
| Token cost per query | ~500–2000 | ~500–2000 | **~50–100** |
| Hallucination risk | High | Medium | **Low** |
| Knowledge staleness | Silent | Silent | **Explicit signal** |
| Human in the loop | None | None | **Full curation** |
| Verification | ❌ | ❌ | **✅ Executable** |
| Original intent preserved | ❌ | ❌ | **✅ As-is storage** |

---

## Key Design Principles

1. **Knowledge as Commons**: Knowledge is curated once, reused infinitely, without token cost.
2. **Verification Before Trust**: Every knowledge unit carries its own verification method.
3. **Human-in-the-Loop Curation**: No automatic ingestion. Every entry is reviewed, sourced, and signed off.
4. **Confidence-Aware Retrieval**: Query engine respects confidence scores; high-stakes answers require high-confidence sources.
5. **Intent Preservation**: Store original text, not summaries. Meaning does not degrade over time.

---

## File Structure

```
redagent-vrkg/
├── README.md              ← This file
├── SOP.md                 ← Knowledge sampling & curation standard operating procedures
├── GENE-FORMAT.md         ← Gene format specification
├── CAPSULE-FORMAT.md      ← Capsule format specification
├── CONCEPT-DICTIONARY.md  ← Core concept definitions
├── REPORTS/               ← Published sampling reports & validation records
└── ASSETS/               ← Diagrams, figures, screenshots
```

---

## Motivation

This system was born out of necessity, not theory.

> "We didn't set out to build an academic framework. We needed AI to stop hallucinating on our private knowledge. So we built a system that remembers what it knows, how it knows it, and whether it can still believe it."

The system is actively used by **Red Agent Team** in production, managing 3,800+ knowledge units across AI operations, engineering decisions, and project history.

---

## Publications

See `REPORTS/` for published sampling reports and validation records.

---

## Citation

```bibtex
@misc{redagentvrkg2026,
  title = {redagent-vrkg: A Verification-Anchored Knowledge Asset System for AI Agents},
  author = {Red Agent Team},
  year = {2026},
  url = {https://github.com/yesimagine-oss/redagent-vrkg}
}
```

---

## License

MIT License — use freely, contribute openly.

---

**Maintainer**: Red Agent Team  
**Contact**: yesimagineoss (at) gmail.com  
**Repo**: https://github.com/yesimagine-oss/redagent-vrkg

