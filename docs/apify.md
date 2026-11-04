# Apify publication checklist — deferred

**No runnable Actor, SDK, API endpoint or deployment exists yet.**

After business and official-source verification:

1. Implement a small Actor using Apify SDK and two bounded official-source adapters, with offline fixtures.
2. Add correct Apify-specific `.actor/actor.json`, input/output configuration schemas and Store documentation. The draft `schemas/result.schema.json` only defines an **event payload**, not platform configuration.
3. Validate run time, cache freshness and SKU batch limits against real source sizes. Keep private catalogues out of shared storage.
4. Publish a controlled test, invoke via standard Apify API and Apify's existing MCP bridge; no custom MCP server first.
5. Price per bounded catalogue-check batch, including **no-match** checks, with measured platform fees and retries.
6. Document source coverage, uncertainty and official notice links. Never promise safety certification, guaranteed recall detection or an automated inventory ban.

Source references:
- https://recalls-rappels.canada.ca/en
- https://www.cpsc.gov/Recalls/CPSC-Recalls-Application-Program-Interface-API-Information
- https://docs.apify.com/platform/actors/publishing
