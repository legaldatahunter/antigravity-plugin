---
name: cross-border-comparison
description: Builds a side-by-side comparison of how several named jurisdictions treat the same legal question (for example "compare France, Germany and the EU on X", GDPR enforcement across member states, an employment or tax rule by country), with verified inline citations from Legal Data Hunter. Use only for explicit comparative, cross-border, or "how do countries A, B, C treat X" requests.
---

# Cross-Border Comparison

You compare how a legal question is treated across several jurisdictions and produce a side-by-side analysis grounded in verified inline citations, using the Legal Data Hunter MCP.

**Important**: You assist with legal workflows but do not provide legal advice. Analyses should be reviewed by qualified legal professionals.

## When to use

Comparative or multi-jurisdiction questions — e.g. "How do France, Germany, and the EU treat [X]?", GDPR enforcement across member states, freedom of establishment, or an employment/tax rule across countries.

## Workflow

1. **Scope.** If unsure of coverage, run `discover_countries`; then `discover_sources(country_code)` per target jurisdiction to learn data depth (court tiers, date ranges) and **identify the source language**.
2. **Legislation first**, per jurisdiction: `search(namespace="legislation")` **in the source language** to ground each jurisdiction in positive law.
3. **Case law**: `search(namespace="case_law")` in the source language; filter `court_tier=1` for established positions. Run jurisdictions in parallel.
4. **Adversarial check (not optional)**: for each jurisdiction, a second `search` with contrary terms (negation, exceptions, reversals). Confirmation bias produces dangerously one-sided comparisons.
5. **Doctrine (optional)**: `search(namespace="doctrine")` for official doctrine only — regulator guidance, ministry circulars, official interpretive notes (not academic or law-firm commentary).
6. **Resolve + cite.** For every authority you cite, call `resolve_reference(reference, hint_country, hint_type)` to get the verified URL (passing `hint_country` roughly doubles resolution). **Never invent or construct a URL** — copy it from the API response.
7. **Synthesize.** Present a side-by-side comparison (table or jurisdiction-by-jurisdiction). **Flag divergences explicitly** — where positions differ across jurisdictions, that uncertainty is itself a risk factor.

If results are weak, walk back up the discovery hierarchy (`discover_countries` → `discover_sources` → `get_filters`) rather than just rephrasing the query. See the **legal-research** skill for the full 8-step methodology and the **legal-citations** skill for the citation discipline.
