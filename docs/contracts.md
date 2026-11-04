# Agent-facing result contract — draft

**Design only.** Input fields and example timestamps are illustrative. Official record mappings and match criteria must be verified against live source documentation before implementation.

## Input example

```json
{
  "products": [
    {
      "sku": "SHOP-SAMPLE-001",
      "brand": "EXAMPLE BRAND",
      "model": "MODEL-123",
      "gtin": null,
      "lot": null
    }
  ],
  "jurisdictions": ["CA", "US"],
  "maxProducts": 100
}
```

Internal SKU is a **caller identifier**, not an official recall identifier. The Actor must not persist client catalogues.

## Output example

```json
{
  "schemaVersion": 1,
  "sku": "SHOP-SAMPLE-001",
  "status": "POSSIBLE_MATCH",
  "coverage": "COMPLETE",
  "checkedAuthorities": ["HEALTH_CANADA", "CPSC"],
  "matches": [
    {
      "authority": "CPSC",
      "officialNoticeId": "EXAMPLE-NOTICE",
      "officialNoticeUrl": "https://www.cpsc.gov/Recalls",
      "matchType": "POSSIBLE",
      "matchedFields": ["brand"],
      "missingEvidence": ["model_or_lot_qualifier"],
      "requiresHumanReview": true
    }
  ],
  "checkedAt": "2026-11-03T15:00:00Z",
  "dataAsOf": null,
  "requiresHumanReview": true
}
```

## Meaning

| Status | Interpretation |
| --- | --- |
| `IDENTIFIER_EVIDENCE` | A documented official identifier agrees; **not** proof that an individual unit is affected |
| `POSSIBLE_MATCH` | Partial or ambiguous candidate; manual verification required |
| `NO_EVIDENCE_FOUND` | No candidate in the successfully searched enabled sources; **not** `SAFE` |
| `SOURCE_INCOMPLETE` | At least one enabled source could not be fully checked; any available matches remain visible |

Only `NO_EVIDENCE_FOUND` can have zero matches and still count as a completed lookup; `SOURCE_INCOMPLETE` is the only valid status with `coverage: INCOMPLETE`.

For simplicity, a product with multiple candidates gets `IDENTIFIER_EVIDENCE` if at least one strong identifier candidate exists, otherwise `POSSIBLE_MATCH`; each candidate retains its own match type and required qualifiers. No score is inferred. The top-level status is `SOURCE_INCOMPLETE` whenever any requested authority fails.

This schema is a **draft JSON payload contract** and not an Apify Actor configuration. A false negative is not evidence of safety; consumers must always link to official notices for final decisions.
