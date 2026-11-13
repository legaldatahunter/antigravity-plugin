# Legal Data Hunter for Google Antigravity

Connect [Legal Data Hunter](https://legaldatahunter.com) to Antigravity so agents can search and resolve primary legal sources: 38M+ legal documents across 230+ jurisdictions, covering legislation, case law, and official doctrine. Citation resolution covers 55+ countries.

Legal Data Hunter is an official partner of Anthropic and OpenAI, with official Claude and ChatGPT connectors.

## Tools

Remote MCP tools served from `https://legaldatahunter.com/mcp` (Streamable HTTP):

- `search`: hybrid semantic and keyword search
- `resolve_reference`: turn citations and informal references into exact documents
- `get_document`: full text and metadata
- `discover_countries`, `discover_sources`, `get_filters`: coverage and filter discovery
- `report_source_issue`: report data quality problems

The plugin also includes a `legal-search` skill that guides agents through the recommended tool order.

## Install

- From the Antigravity Marketplace, once listed.
- From a local checkout: `agy plugin install .`
- From GitHub: clone https://github.com/legaldatahunter/antigravity-plugin and run `agy plugin install .` inside it.

## Authentication

On first use, sign in with GitHub or Google in your browser (OAuth). No API key is needed.

## Example prompts

- Search recent GDPR enforcement decisions in France and the EU
- Resolve this citation and fetch the full judgment text
- Which countries and courts does Legal Data Hunter cover for company law?

## Validate

```bash
agy plugin validate --strict .
```

## Links

- Product: https://legaldatahunter.com
- Connect guide: https://legaldatahunter.com/connect
- Docs: https://legaldatahunter.com/docs
- Support: https://legaldatahunter.com/support or zach@legaldatahunter.com
- Claude connector: https://claude.com/marketplace/connectors/legal-data-hunter
- ChatGPT plugin: https://openai.com/business/plugins/legal-data-hunter/
