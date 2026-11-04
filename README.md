# Cross-Border Recall Evidence

**Evidence-backed consumer-product recall lookup for agents checking Canada and US catalogues.**

> **Status: design only.** No runnable Actor, official-feed integration, verified match-rate measurements or paying customers.

## The problem

A catalogue agent needs to compare product identifiers against official product safety recalls. It cannot safely infer that two similar names are the same product—or that no match means a product is safe.

This proposed Actor combines the **official Canadian recall feeds** and the **US CPSC recall API**, with narrow, explicit match semantics and links to the underlying official notices.

Official sources:
- [Canada — Recalls and safety alerts](https://recalls-rappels.canada.ca/en), which publishes daily CSV/JSON data feeds across several recall categories.
- [United States — CPSC Recalls API](https://www.cpsc.gov/Recalls/CPSC-Recalls-Application-Program-Interface-API-Information), a public REST service for consumer-product recall records.

The MVP is scoped to **consumer products**. Food, drugs, medical devices, motor vehicles, the FDA, NHTSA and EU Safety Gate are **out of scope** until a buyer proves a need. Access, redistribution and attribution terms must be verified before publication.

## Proposed MVP

- Input: a bounded batch of products with customer SKU, brand, model and optional GTIN / lot / serial metadata.
- Output: source-scoped candidate matches with specific fields that justify each match, official notice links, and explicit incomplete-coverage states.
- Exact identifiers where present are prioritized; uncertain text similarity always requires review.
- Never claim `SAFE`, `NOT_RECALLED`, `COMPLIANT`, or `RECALL_FREE`. `NO_EVIDENCE_FOUND` only describes the **queried source coverage at the observation time**.
- No LLM judgment, no auto-blocking or auto-delisting inventory in MVP.

## Design documents

- [Architecture and matching pipeline](docs/architecture.md)
- [Agent-facing contract](docs/contracts.md) / [draft JSON Schema](schemas/result.schema.json)
- [ADR-0001: uncertainty and safety](docs/adr/0001-matching-evidence-not-safety.md)
- [ADR-0002: stateless batch and coverage](docs/adr/0002-stateless-batch-and-coverage.md)
- [Validation gates](docs/validation.md)
- [Apify packaging](docs/apify.md)

## Business hypothesis

Catalogues and integration agents *might* repeatedly pay for reliable, low-friction recall evidence. Existing Canada and US recall sources and commercial offerings already exist, so mere aggregation is **not differentiation**. Demonstrate that evidence quality or integration savings justify payment before implementation.

Not affiliated with Health Canada, the CPSC, or their governments. Always consult official notices when making safety decisions.
