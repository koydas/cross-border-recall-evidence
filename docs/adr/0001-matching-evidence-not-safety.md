# ADR-0001 — A candidate match is evidence, not a product safety verdict

- **Status:** Proposed
- **Date:** 2026-11-03

## Context

Recall notices may omit normalized GTINs, exact model IDs or affected lot ranges. Catalogue products can reuse similar brands, names or variant codes. The cost of falsely confirming or dismissing a recall is high.

## Decision

1. Use exact authoritative product identifiers when available; also return tentative matches separately, with matched and missing fields.
2. Output `IDENTIFIER_EVIDENCE` when an official identifier agrees with an input identifier **and conflicting constraints are absent**. This only confirms matching evidence; it does **not** establish that a given physical unit or lot is recalled.
3. Use `POSSIBLE_MATCH` for name/brand or incomplete model matches. Require human review.
4. Use `NO_EVIDENCE_FOUND` solely when all enabled official feeds were checked successfully and there are no supported candidates; it is not a declaration of product safety.
5. If any selected source is unavailable, out of date beyond an agreed threshold, truncated, malformed or only partially processed, emit `SOURCE_INCOMPLETE`, preserving any evidence from successfully checked sources.
6. Include jurisdiction, official notice ID/URL, matching criteria, missing qualifiers and last checked time. No model-invented certainty percentage.
7. Do not take automated delisting, shipment hold or other safety-critical action based solely on this service.

## Alternatives rejected for MVP

- LLM-generated probabilistic matching and a single confidence score.
- Inferring safe status from absence in a limited feed.
- Combining unrelated regimes (food, drugs, vehicles) without their distinct identifier semantics.
- Rehosting full copyrighted notice bodies or product photography.

## Consequences

The service may produce more `POSSIBLE_MATCH` outcomes than a consumer would like, but its uncertainty remains inspectable and testable. A future trained matcher requires a labeled evaluation set, precision/recall measures and explicit use-case risk thresholds.

## Sources

- https://recalls-rappels.canada.ca/en
- https://www.cpsc.gov/Recalls/CPSC-Recalls-Application-Program-Interface-API-Information
