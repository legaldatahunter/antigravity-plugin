# Multi-Jurisdictional Workflow

> load: on-demand

## Research strategy for multi-jurisdictional questions

For a typical comparative analysis, the research flow looks like this:

1. **Scope the jurisdictions**: Run `discover_countries` if unsure what's covered. Then `discover_sources` for each target country to understand data depth (court tiers, date ranges, document volume) and **identify the source language**.

2. **Search legislation first**: For each jurisdiction, `search` with `namespace: "legislation"` **in the source language** to identify the relevant statutory framework. This grounds the analysis in positive law before looking at how courts interpret it.

3. **Search case law for established positions**: `search` with `namespace: "case_law"` **in the source language** using descriptive terms for the legal concept. Filter by `court_tier: 1` to prioritize supreme court rulings. Run searches for multiple jurisdictions in parallel.

4. **Adversarial check**: For each jurisdiction, run a second `search` with contrary terms **in the source language** — negation keywords, exceptions, reversals. The goal is to find decisions that contradict the established position. This is not optional: confirmation bias will produce dangerously one-sided analysis.

5. **Doctrinal check**: `search` with `namespace: "doctrine"` for official doctrine — regulator guidance, ministry circulars, and other official interpretive material. The `doctrine` namespace contains official doctrine only, *not* academic commentary or law-firm analysis. It may be available in English even for non-English jurisdictions — try both the source language and English.

6. **Temporal check**: If the most recent relevant case is more than 3 years old, run additional searches with `date_start` set to 2 years ago. Flag older jurisprudence as potentially outdated.

7. **Deep dive**: Use `get_document` on the most important decisions to get full text. Use `resolve_reference` when you have specific ECLI numbers, CELEX references, or case numbers.

8. **Re-target weak results via the discovery hierarchy**: If results from any step were weak or off-topic, walk back up — `discover_countries` to check dataset coverage, `discover_sources` to identify the right source, `get_filters` to get valid filter values — then re-run with more precise parameters. See "When results are weak" in `discovery-and-search.md` for the full workflow.

9. **File data gaps**: If a jurisdiction you expected to have data doesn't, or if results are sparse or clearly incomplete, use `report_source_issue` (see `data-quality-reporting.md`).

When running searches across multiple jurisdictions, launch them in parallel to save time. The tools support concurrent calls.

## Jurisprudential Research Methodology

A risk assessment is only as good as the legal analysis that feeds it. The most dangerous failure mode in legal research is anchoring on an established position without checking for recent reversals. A well-known ruling from 5 or 10 years ago may have been overturned, narrowed, or contradicted by a more recent decision — and basing a risk assessment on outdated case law can lead to dramatically wrong conclusions.

This risk is amplified in multi-jurisdictional analysis: a position that's settled in one country may have been reversed in another, or the same EU directive may be interpreted differently across member states.

### Step 1: Adversarial search for contradicting jurisprudence

After identifying the established legal position in each jurisdiction, actively search for decisions that contradict it. Formulate queries using terms that express the opposite position, exceptions, nullity, or reversal.

Confirmation bias is natural. If you search only for cases that support a position, you will find them — and miss the ones that undermine it. A good legal analyst always argues against their own thesis before presenting it.

### Step 2: Doctrinal cross-check

Search `namespace: "doctrine"` for official doctrine — regulator guidance, ministry circulars, and official interpretive notes (not academic or law-firm commentary). Official doctrine synthesizes and contextualizes — it tells you not just what a court decided, but why it matters and what changed. This is especially valuable in multi-jurisdictional work where you may not be deeply familiar with every legal system's nuances.

### Step 3: Temporal confidence check

Before finalizing your analysis for each jurisdiction, check the date of the most recent supporting decision:

- If the most recent case is **< 3 years old**: confidence is high.
- If **3-5 years old**: moderate confidence — flag it and run a targeted search for the last 24 months.
- If **> 5 years old**: low confidence — the position may have evolved. Run date-filtered searches, check doctrine, and explicitly flag the uncertainty in your analysis.

### Applying these steps (per jurisdiction)

For each jurisdiction in the analysis:

1. Initial search for the established position (`search` with `namespace: "case_law"` + `namespace: "legislation"`)
2. Adversarial search with contrary terms (`search` with negation/exception keywords)
3. Doctrinal search (`search` with `namespace: "doctrine"`)
4. Temporal check: if the newest supporting case is >3 years old, run date-filtered searches for the last 24 months
5. If any step reveals a gap, file an issue via `report_source_issue`

Only after completing all steps for all jurisdictions should you proceed to the analysis. If any step reveals a contradiction or reversal, the analysis must account for it.
