# Data Quality Reporting

> load: on-demand

## Data Gap Protocol

The Legal Data Hunter is a growing platform — data improves daily. When you encounter gaps, offering to file an issue can help the platform improve for everyone. Disclose the gap as part of the research workflow; file a report only after the user agrees.

### When to offer to file an issue

Offer to file a `report_source_issue` only when something should clearly be in Legal Data Hunter but isn't (a source, document, or reference that should exist but is missing or broken) — only for important data gaps, not for minor issues. Where reasonable, make at most one consolidated offer per task, listing the gaps briefly. Typical cases:

- **A jurisdiction you searched has zero or very few results** on a topic where you'd expect significant case law or legislation. Issue type: `"data_quality"`.
- **A document URL returned by the API is broken** (404, redirect to wrong page). Issue type: `"invalid_url"`.
- **Search results are clearly mis-indexed** — e.g., a German decision appearing under French sources, or a case law result that's actually legislation. Issue type: `"indexing"`.
- **A known important source is missing entirely** — e.g., you know a country's constitutional court should be indexed but `discover_sources` doesn't list it. Issue type: `"unavailable"`.
- **Document text is garbled, truncated, or in the wrong language**. Issue type: `"data_quality"`.

### How to file

Offer to file a report; call the tool only after the user agrees to that specific report. Use `report_source_issue` with:
- `source`: The source identifier (e.g., `"DE/BVerfG"`, `"FR/Judilibre"`). Use `discover_sources` to find valid source identifiers.
- `issue_type`: One of `"unavailable"`, `"indexing"`, `"invalid_url"`, `"data_quality"`, `"other"`.
- `description`: Be specific. Describe the data problem (source, issue type, what was missing or broken). Leave out the user's query text, facts, names and task description unless the user approves the exact wording. Example: `"UK/Bailii: Employment Appeal Tribunal decisions from 2020 onward appear to be missing; the source lists 48,000+ documents but returns none for this court and date range."`.

### How to report to the user

When you encounter a data gap during research, tell the user transparently:

> **Data gap noted**: [Country/Source] returned limited results for [topic]. I can file an issue about this gap — want me to? (issue type: [type]). The platform's coverage improves daily, so this may be resolved soon. In the meantime, [alternative approach — e.g., "I've supplemented with web search for doctrinal commentary" or "the analysis for this jurisdiction relies on legislation only"].

Never silently skip a jurisdiction because of missing data. Always flag the gap and explain what you did instead. Offer to file a report only when something should clearly be in the data but isn't, as a single consolidated offer where reasonable (file only after the user agrees).

## Feedback Protocol

The Legal Data Hunter is a commercial product with a feedback loop. Offer a report only for an important data or tool problem (something that should be there but isn't, or a broken or misleading result), not for minor issues. Where reasonable, make at most one consolidated offer per task, listing the gaps briefly. Call `report_source_issue` only after the user agrees to that specific report.

### What to flag (beyond data gaps)

Offer to file a `report_source_issue` or use the feedback mechanism only for a clear problem in the following (not every minor issue), and call the tool only after the user agrees:

**Data quality issues:**
- Zero results where results are expected
- Results that are clearly off-topic or mis-indexed
- Broken/dead document URLs
- Truncated or garbled document text
- Wrong language for the indexed source
- Duplicate documents in search results
- Date fields that look wrong (e.g., 1970-01-01 placeholder dates)

**Tool design issues (report these only with the user's approval):**
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
- Source, issue type, and what was missing or broken
- What you expected to happen
- What actually happened
- Concrete suggestion if you have one

Leave out the user's query text, facts, names and task description unless the user approves the exact wording.
