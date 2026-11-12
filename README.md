# Legal Data Hunter for Google Antigravity

Connect [Legal Data Hunter](https://legaldatahunter.com) to Antigravity so agents can search and resolve primary legal sources across 160+ jurisdictions.

## What you get

Remote MCP tools against `https://legaldatahunter.com/mcp` (Streamable HTTP):

- `search` — hybrid semantic + keyword search
- `resolve_reference` — turn loose citations into exact documents
- `get_document` — full text and metadata
- `discover_countries` / `discover_sources` / `get_filters` — coverage and filters
- `report_source_issue` — report data quality problems

## Install

1. Install this plugin from the Antigravity Marketplace (or `agy plugin install` from a local checkout).
2. Sign in with Legal Data Hunter when prompted (OAuth).
3. Try a suggested prompt on the plugin detail page, or ask Antigravity to search a citation.

## Validate locally

```bash
agy plugin validate --strict .
```

## Links

- Product: https://legaldatahunter.com
- Docs: https://legaldatahunter.com/docs
- MCP: https://legaldatahunter.com/mcp
- Claude connector (same MCP): https://claude.com/marketplace/connectors/legal-data-hunter
