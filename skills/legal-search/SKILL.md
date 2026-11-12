---
name: legal-search
description: >-
  Search and resolve primary legal sources with Legal Data Hunter.
  Use when the user asks for case law, legislation, a citation lookup,
  ECLI/CELEX resolution, or multi-jurisdiction legal research.
---

Use the Legal Data Hunter MCP tools in this order when helpful:

1. `discover_countries` or `discover_sources` if coverage is unclear.
2. `resolve_reference` when the user gives a citation or informal reference.
3. `search` for topic or keyword research (tune `alpha` toward 1.0 for meaning, 0.0 for exact terms).
4. `get_document` for full text after you have `source` and `source_id`.
5. `report_source_issue` only when a source URL or record looks wrong.

Prefer official primary sources returned by the tools. Do not invent citations or holdings.
