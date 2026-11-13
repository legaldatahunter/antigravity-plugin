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

## Skills

The plugin ships six skills that load on demand. Run them as slash commands or let the agent pick them up from your request.

| Skill | Use it for | Example |
|---|---|---|
| `legal-research` | General legal research: statutes, case law, official doctrine, single or multi-jurisdiction questions, memo-style answers | `/legal-research GDPR Article 46 SCC transfers and the Schrems II line of cases` |
| `cross-border-comparison` | Explicit side-by-side comparison of how several jurisdictions treat the same question | `/cross-border-comparison France, Germany and the EU on the GDPR one-stop-shop` |
| `resolve-citation` | Turning one citation or informal reference (ECLI, CELEX, article, docket number) into the exact document and a verified URL | `/resolve-citation Article 1240 of the French Code civil` |
| `legal-citations` | Citation discipline: verified inline links, no invented URLs, Sources section | `/legal-citations How should I cite CJEU case law in this memo?` |
| `legal-risk-assessment` | Severity × likelihood scoring per jurisdiction and escalation to outside counsel | `/legal-risk-assessment Score our cross-border data-transfer exposure` |
| `multi-issue-memo` | Long-form memo that resolves each enumerated sub-issue with its own statute and leading case | `/multi-issue-memo (1) liability of the building's keeper, (2) fault of the victim` |

Legal Data Hunter assists with legal workflows but does not provide legal advice; have qualified professionals review the output.

## Install

- From the Antigravity Marketplace, once listed.
- From a local checkout: `agy plugin install .`
- From GitHub: clone https://github.com/legaldatahunter/antigravity-plugin and run `agy plugin install .` inside it.

## Authentication

On first use, sign in with GitHub or Google in your browser (OAuth). No API key is needed.

## Example prompts

- Analyze EU-to-third-country data transfers under GDPR Article 46 SCCs, with verified citations
- Find Article 1240 of the French Code civil and give me the official source link
- Compare how France, Germany, and the EU handle the GDPR one-stop-shop mechanism

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
