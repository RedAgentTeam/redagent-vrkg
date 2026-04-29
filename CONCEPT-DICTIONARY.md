# Concept Dictionary

**Version**: v1.0  
**Status**: Active  
**Purpose**: Define core terms used across the VRKG system to ensure consistent understanding.

---

## Core Concepts

### VRKG (Verifiable Retrieval Knowledge Graph)

The overall system architecture. A knowledge management approach where every knowledge unit carries its own verification method, confidence score, and expiration signal. Not a vector database, not a traditional RAG — a human-curated, verification-anchored knowledge commons.

> "RAG optimizes for retrieval speed. VRKG optimizes for knowledge trust."

---

### Gene

The atomic unit of knowledge in VRKG. A Gene is a **single verifiable fact + its verification method**, stored together. Think of it as a knowledge chromosome — self-contained, traceable, replaceable.

**Key property**: Immutability after commit. Genes preserve the state of knowledge at sampling time, even if the source later changes.

---

### Capsule

A trigger-to-execution bundle in VRKG. A Capsule pairs a **trigger condition** with **ordered executable steps**, so the system knows not just *what is true* but *what to do when something happens*.

**Key property**: Procedural memory. Genes answer "what is X?" Capsules answer "what to do about X?"

---

### Confidence Score

A numeric value (0.0–1.0) attached to every knowledge unit, indicating how trustworthy it is.

| Score Range | Label | Action |
|------------|-------|--------|
| 0.95–1.0 | High confidence | Serve in primary results |
| 0.70–0.94 | Medium confidence | Serve with caveats |
| 0.50–0.69 | Low confidence | Serve only if no better option |
| < 0.50 | Unusable | Do not serve; store as raw reference |

---

### Evidence Level

A label describing the quality of evidence behind a knowledge unit.

| Label | Meaning |
|-------|---------|
| `原文+实测` | Original text captured AND verification command executed successfully |
| `原文` | Original text captured but verification not executed |
| `第三方` | Third-party source, not directly verified |

---

### Trigger Signal

The natural-language condition that causes a Capsule to be retrieved in response to a query. Written from the perspective of the user seeking help, not the system.

Good trigger: "My EvoMap node went offline, what should I do?"  
Bad trigger: "CAPSULE_TRIGGER_NODE_OFFLINE" (too technical, not user-language)

---

### Knowledge Curation

The human process of reviewing, validating, and approving knowledge units before they enter the VRKG. Unlike automated ingestion, curation means a person has read the original source, confirmed its accuracy, and signed off on storage.

**Why it matters**: Automated ingestion propagates errors. Human curation catches them before they spread.

---

### Staleness

The condition where a knowledge unit's source has changed but the stored content no longer reflects reality. VRKG detects staleness by running `validate_command` periodically; if it fails or returns different content, the unit is flagged.

---

### Token Efficiency

The ratio of knowledge value extracted per LLM token consumed. Traditional RAG burns tokens on every query. VRKG amortizes the cost: knowledge is curated once (token cost borne at intake), then retrieved at near-zero cost on every subsequent query.

---

### Knowledge Commons

The philosophical framing of VRKG: when knowledge is curated once and reusable infinitely, it becomes a **commons** — shared infrastructure, not a consumable resource.

---

## Related Terms

| Term | Relationship |
|------|-------------|
| RAG | Contrast: RAG optimizes speed, VRKG optimizes trust |
| Vector DB | Tool: VRKG can use vector search for retrieval layer |
| Knowledge Graph | Architecture: VRKG uses graph-like relationships between knowledge units |
| Hallucination | Problem: VRKG directly addresses hallucination via verification |
| Curation | Process: VRKG's quality control mechanism |
