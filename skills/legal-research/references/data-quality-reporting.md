# Data Quality Reporting

> load: on-demand

## Data Gap Protocol

The Legal Data Hunter is a growing platform — data improves daily. When you encounter gaps, filing an issue helps the platform improve for everyone. This is part of the research workflow, not an afterthought.

### When to file an issue

File a `report_source_issue` when:

- **A jurisdiction you searched has zero or very few results** on a topic where you'd expect significant case law or legislation. Issue type: `"data_quality"`.
- **A document URL returned by the API is broken** (404, redirect to wrong page). Issue type: `"invalid_url"`.
- **Search results are clearly mis-indexed** — e.g., a German decision appearing under French sources, or a case law result that's actually legislation. Issue type: `"indexing"`.
- **A known important source is missing entirely** — e.g., you know a country's constitutional court should be indexed but `discover_sources` doesn't list it. Issue type: `"unavailable"`.
- **Document text is garbled, truncated, or in the wrong language**. Issue type: `"data_quality"`.

### How to file

Use `report_source_issue` with:
- `source`: The source identifier (e.g., `"DE/BVerfG"`, `"FR/Judilibre"`). Use `discover_sources` to find valid source identifiers.
- `issue_type`: One of `"unavailable"`, `"indexing"`, `"invalid_url"`, `"data_quality"`, `"other"`.
- `description`: Be specific. Include the query you ran, what you expected to find, and what you actually got (or didn't get). Example: `"Searched for 'unfair dismissal' in UK case law with court_tier=1 and date_start=2020-01-01. Expected Employment Tribunal and EAT decisions but got 0 results. The UK/Bailii source shows 48,000+ documents so coverage should exist for this topic."`.

### How to report to the user

When you encounter a data gap during research, tell the user transparently:

> **Data gap noted**: [Country/Source] returned limited results for [topic]. I've filed an issue with the platform (issue type: [type]). The platform's coverage improves daily, so this may be resolved soon. In the meantime, [alternative approach — e.g., "I've supplemented with web search for doctrinal commentary" or "the analysis for this jurisdiction relies on legislation only"].

Never silently skip a jurisdiction because of missing data. Always flag the gap, file the issue, and explain what you did instead.

## Feedback Protocol — Aggressive Quality Signaling

The Legal Data Hunter is a commercial product with a feedback loop. **Use it aggressively.** Filing feedback is cheap (free API call), but the signal is high-value for product improvement. You should be filing feedback far more often than feels comfortable — err heavily on the side of filing.

### What to flag (beyond data gaps)

File `report_source_issue` or use the feedback mechanism for **any** of the following:

**Data quality issues:**
- Zero results where results are expected
- Results that are clearly off-topic or mis-indexed
- Broken/dead document URLs
- Truncated or garbled document text
- Wrong language for the indexed source
- Duplicate documents in search results
- Date fields that look wrong (e.g., 1970-01-01 placeholder dates)

**Tool design issues (flag these explicitly):**
- A parameter that's confusing or underdocumented — e.g., `alpha` behavior is not self-explanatory
- A tool that requires two calls where one would do — e.g., needing `discover_sources` before `search` just to learn the language of a source
- Search results that include fields with no documentation (what does `score` mean? how is it normalized?)
- Error messages that don't tell you what went wrong or how to fix it
- A tool that would be significantly more useful with a small addition — e.g., `search` returning a `url` field inconsistently

**Workflow inefficiencies:**
- Having to call 3 tools to accomplish what should be 1 (e.g., discover → filter → search for a simple query)
- Rate limiting or latency that degrades the multi-jurisdictional parallel search workflow
- A common research pattern (adversarial search, temporal check) that could be exposed as a built-in tool parameter

### How to frame feedback

Be specific and direct. Good feedback is actionable. Bad feedback is vague.

**Good**: `"The 'search' tool returns a 'url' field for ~60% of results and omits it for the rest, with no indication of why. This forces a fallback to 'get_document' for every result just to check if a URL exists — a significant overhead. Either always return the URL or add a 'has_url' boolean so the caller can decide whether to fetch."`

**Bad**: `"Some results don't have URLs."`

Always include:
- What you were trying to do
- What you expected to happen
- What actually happened
- Concrete suggestion if you have one

### Frequency

If you complete a multi-jurisdictional research task without filing at least one piece of feedback, ask yourself whether you were paying attention. On any non-trivial query across 3+ jurisdictions, you will encounter something worth flagging. The platform is actively developed — your signal matters and reaches the team directly.
