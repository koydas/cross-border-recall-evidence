# ADR-0002 — Stateless batch lookup and explicit per-source coverage

- **Status:** Proposed
- **Date:** 2026-11-03

## Context

Daily catalogue checks are naturally recurring. Yet storing tenant catalogues or watchlists before demand validation creates authentication, deletion, data retention and support requirements.

## Decision

The MVP is stateless for **customer catalogue data**. A client supplies a bounded batch on every invocation and receives a per-SKU result. Public official recall datasets may be cached with timestamps and deterministic integrity controls.

Each result records which official authorities were requested and which were successfully consulted. Partial failures make the overall product outcome `SOURCE_INCOMPLETE`, even if one feed returned potential evidence.

A scheduled consumer is responsible for storing the previous output and comparing recall identifiers between runs. Internal history and push alerts are future options **only after a paid recurring workflow requires them**.

## Trade-offs

Repeated submission of a catalogue costs bandwidth; absence of hosted alerts reduces convenience. In return, there is no multi-tenant product database, the integration surface is small and the provenance of each answer is clearer.

## Deferred

Private catalog persistence, webhook subscriptions, customer accounts, broad external web scraping, paid proxies, LLM-based semantic matching and custom MCP hosting.
