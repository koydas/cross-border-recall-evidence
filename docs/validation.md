# Validation gates and test plan

## Gate 0 — Buyers before code

- Identify ten real catalogue-integration, retail-compliance or ecommerce automation consumers.
- Produce a handful of real, sourced candidate matches from the Canada and CPSC public sources (no fabricated SKU matches).
- Offer a low-cost paid trial for repeated checks of a bounded test catalogue.
- **Pass:** at least two paid pilots and evidence of a planned second check within 14 days.
- **Stop:** no paying interest, no identifiable catalogue fields in official notices, or buyers already obtain equivalent results free.
- External spend limit: CAD 50; track hours and costs separately.

## Gate 1 — Source and licensing

- Confirm official URLs for Canadian daily datasets and CPSC REST API; record actual refresh behavior and available category fields.
- Restrict to consumer products and identify missing model/GTIN/lot fields as an observed limitation.
- Verify source-specific terms and commercial redistribution rights; do not ship before resolved.
- Test timeout, rate limit, pagination, malformed data, partial results and provenance.
- Establish allowed cache refresh/retention and attribution requirements.

## Gate 2 — Correctness

| Test | Expected |
| --- | --- |
| Exact model + brand/identifier documented by authority | `IDENTIFIER_EVIDENCE`, with official proof and qualifiers |
| Brand only, missing model/lot | `POSSIBLE_MATCH`, not a certain recall |
| No candidate after both sources checked | `NO_EVIDENCE_FOUND`, with clear non-safety semantics |
| Canada source fails, US source returns none | `SOURCE_INCOMPLETE`, never `NO_EVIDENCE_FOUND` |
| Canada source fails, US has a possible match | `SOURCE_INCOMPLETE`, preserve partial evidence |
| Truncated source, duplicates, invalid notice | Fail closed or explicit incomplete coverage |
| Incomplete coverage but status `NO_EVIDENCE_FOUND` | Schema rejects |
| Complete coverage with `SOURCE_INCOMPLETE` | Schema rejects |
| Supported candidate without evidence link | Schema rejects |
| Two customer catalogues | No persisted private input or cross-client leakage |

Add negative cases with different product variants and affected-lot limits; sample false positives and missed matches against official notices. High-stakes automated decisions remain out of scope.

## Gate 3 — Actor and economics

Validate Apify runtime schemas, API outputs, cost per 100 products at measured feed volumes, cache hit rate and recurring *paid* usage. Avoid any claim of real-time updates or universal product coverage.
