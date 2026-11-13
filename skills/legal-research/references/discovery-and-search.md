# Discovery and Search

> load: on-demand

The Legal Data Hunter MCP provides access to 38M+ legal documents across 230+ jurisdictions (1,700+ sources). It covers case law, legislation, and doctrine. Use these tools for single-jurisdiction and multi-jurisdictional research alike.

## Core tools

There is a 3-level discovery hierarchy before you search. Use it whenever the right dataset or filter values aren't obvious.

| Tool | Level | Purpose | When to use |
|------|-------|---------|-------------|
| `discover_countries` | 1 — Dataset | List all available countries with document + source counts | When you're unsure which countries have coverage, or results were weak and you suspect you're searching a thin dataset — check coverage here first |
| `discover_sources` | 2 — Dataset | List all data sources for a country: courts, codes, source IDs, tiers, date ranges, document counts | When you need to identify the right source for a country, understand what courts/codes are indexed, or pick the correct `source` ID before calling `get_filters` |
| `get_filters` | 3 — Filter values | Return distinct filter values *within* a specific source: courts, chambers, jurisdictions, decision types, languages, date ranges | Once you know the source, call this to discover what values are valid for `jurisdiction`, `subdivision`, `language`, etc. — never guess these values |
| `search` | — | Hybrid semantic + keyword search across case_law, legislation, or doctrine | The primary research tool — always informed by the discovery hierarchy above |
| `get_document` | — | Retrieve full document text by source + source_id | When you need the complete text of a decision, statute, or article found via search |
| `resolve_reference` | — | Resolve a loose citation (ECLI, CELEX, article number, case number) to the exact document | When you have a specific citation and need to find the corresponding record |
| `report_source_issue` | — | Flag missing data, broken URLs, indexing errors, or quality issues | When research reveals gaps — missing decisions, broken links, or poor data quality (see `data-quality-reporting.md`) |

## Understanding `search` parameters

The `search` tool is the workhorse. Its key parameters:

- **`query`**: Natural language — describe the legal concept you're looking for. Works in any language.
- **`namespace`**: Choose `"case_law"`, `"legislation"`, or `"doctrine"`. Always search the right namespace for what you need. Note: `"doctrine"` contains **official doctrine only** (administrative guidance, regulator commentary, ministry circulars, official interpretive notes) — not academic articles or law-firm commentary.
- **`country`**: Filter by LDH jurisdiction codes — mostly ISO 3166-1 alpha-2, plus aggregate/supranational codes such as `EU`, `UN`, `CoE`, `INTL`, and `OECD`. Call `discover_countries` for the authoritative full list.
- **`court_tier`**: Filter by court level — `1` = supreme/constitutional, `2` = appellate, `3` = first instance. For risk assessment, prioritize tier 1 (supreme court decisions carry the most weight).
- **`date_start` / `date_end`**: Date filters in `YYYY-MM-DD` format. Essential for temporal checks.
- **`alpha`**: Controls semantic vs keyword balance on a `0.0`–`1.0` scale: `1.0` = pure semantic (meaning), `0.0` = pure keyword (exact terms). `0.7` (default) is good for most queries. Use `0.5` for more keyword-heavy searches (specific legal terms), `0.9` for conceptual/thematic searches, and values near `0.0` for exact phrases, identifiers, or article numbers.
- **`top_k`**: Number of results (1-100). Use 10-20 for initial exploration, 5 for targeted follow-ups.
- **`language`**: Filter by language code (e.g., `"fr"`, `"de"`, `"en"`). Useful when you need decisions in a specific language.
- **`jurisdiction`**: Filter by jurisdiction type (e.g. `"civil"`, `"criminal"`, `"administrative"`). Only use values confirmed via `get_filters` — do not guess.
- **`subdivision`**: Filter by geographic subdivision (ISO 3166-2), e.g. `"DE-BY"` for Bavaria, `"US-CA"` for California. Only use values confirmed via `get_filters`.

## When results are weak: walk back up the discovery hierarchy

If a search returns results that are off-topic, too broad, or clearly not what you need, **do not just retry with a different query**. Walk back up the 3-level discovery hierarchy to find the root cause — the problem is usually dataset selection (wrong country/source) rather than query formulation.

**Diagnosis by symptom:**

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| Zero or very few results | Country may have thin coverage, or wrong namespace | `discover_countries` → check document count for that country |
| Results from unexpected countries or source types | Searching too broadly without country filter | `discover_sources(country_code)` → identify the right source IDs, then filter by source |
| Correct country but wrong court/chamber | You don't know what courts are indexed | `discover_sources(country_code)` → see which courts exist and their tiers |
| Right source, but results span irrelevant jurisdictions or chambers | Valid filter values unknown | `get_filters(source)` → get exact `jurisdiction`, `subdivision`, `language` values, re-run with them |
| Uniformly low relevance scores | Source may not cover this topic at all | `discover_sources` → check document count and date range; if thin, file `report_source_issue` |

**The re-targeting workflow:**

```
Step 1 — Dataset check (discover_countries)
  → Is the country actually covered? Does it have meaningful document volume?
  → If not: note the gap, file report_source_issue, pivot to adjacent jurisdiction or EU-level sources

Step 2 — Source check (discover_sources)
  → Which specific sources (courts, codes) exist for this country?
  → Are the right court tiers indexed? What date ranges are covered?
  → Pick the correct source ID(s) to target in Step 3

Step 3 — Filter check (get_filters)
  → For each relevant source, what are the valid filter values?
  → Courts, chambers, jurisdictions, decision types, languages, date ranges
  → Apply these as search() parameters — never guess filter values

Step 4 — Re-run search() with correct source + filters
  → Use the source-confirmed language, jurisdiction, and chamber values
  → If still weak after this: it's a genuine data gap → report_source_issue
```

**Example — weak results on German administrative law:**
```
# Initial search(query="Verwaltungsrecht Ermessen", country=["DE"]) → mixed, unfocused results
# Step 1: discover_countries() → DE has 480k+ documents, coverage is fine
# Step 2: discover_sources(country_code="DE") →
#   reveals DE/BVerwG (Bundesverwaltungsgericht, tier 1, administrative),
#   DE/VGH-Bayern (Bavarian admin appeals, tier 2), DE/OVG-NRW (NRW, tier 2)
# Step 3: get_filters(source="DE/BVerwG") →
#   jurisdictions=["administrative"], chambers=["1. Senat", "4. Senat", ...], language="de"
# Step 4: search(query="Verwaltungsrecht Ermessen", country=["DE"],
#               jurisdiction="administrative", court_tier=1, language="de")
# → Precise, on-point results from the right court
```

**Reporting discovery gaps:** If `discover_sources` shows very few sources for a country you'd expect to be well-covered, or `get_filters` returns sparse metadata (missing `jurisdiction` or `chamber` values), flag it via `report_source_issue` with `issue_type="data_quality"`. These gaps limit precision for everyone — reporting them directly improves the platform.

## Critical: search in the source language

Many national legal databases are indexed in their native language. Searching in English against a French, German, Estonian, or Bulgarian database will produce poor or irrelevant results. **Always formulate your search query in the language of the target jurisdiction.**

Language mapping for key jurisdictions:

| Country | Search language | Example query (corporate tax) |
|---------|----------------|-------------------------------|
| FR | French | `"impôt sur les sociétés taux réduit startup"` |
| DE | German | `"Körperschaftsteuer Steuersatz Gründung Unternehmen"` |
| ES | Spanish | `"impuesto de sociedades tipo reducido empresa"` |
| IT | Italian | `"imposta sul reddito delle società aliquota ridotta"` |
| PT | Portuguese | `"imposto sobre o rendimento das pessoas colectivas taxa reduzida"` |
| NL | Dutch | `"vennootschapsbelasting tarief startup"` |
| EE | Estonian | `"tulumaks juriidiline isik jaotamata kasum"` |
| BG | Bulgarian | `"корпоративен данък ставка дружество"` |
| AT | German | `"Körperschaftsteuer Satz Unternehmensgründung"` |
| BE | French/Dutch | Use French for Wallonia sources, Dutch for Flemish |
| EU | English | English works well for CURIA, EuroParl, EUR-Lex |
| UK | English | English |
| IE | English | English |

When searching a country where you don't know the legal terminology, use `discover_sources` first to check what language the source uses, then formulate your query accordingly. For EU-level sources (CURIA, EuroParl, EUR-Lex), English queries work well since these institutions publish in multiple languages.

You can also run parallel searches: one in the source language and one in English, then merge the results. This catches documents that may have been indexed in translation.
