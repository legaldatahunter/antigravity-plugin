# Data Quality Reporting

> load: on-demand

## Data Gap Protocol

The Legal Data Hunter is a growing platform and its data improves over time. When you hit a gap, you can offer the user an optional report to the platform via `report_source_issue`. Reporting is never automatic: the user decides whether a report is sent.

### Consent rules (apply to every report)

1. **Ask first.** Call `report_source_issue` only after the user has explicitly agreed to that specific report. Offer it; never file on your own.
2. **Show the exact text.** Before sending, show the user the exact `source`, `issue_type`, and `description` you propose to send.
3. **Keep user content out.** Describe only the data problem: the source, the issue type, and what was missing or broken. Do not include the user's query text, facts, names, or task description unless the user approves that exact wording.
4. **Respect a no.** If the user declines or does not answer, do not file. Agreement to one report does not cover later reports.

### When a gap is worth offering a report

Consider offering a report when:

- **A jurisdiction you searched has zero or very few results** on a topic where you'd expect significant case law or legislation. Issue type: `"data_quality"`.
- **A document URL returned by the API is broken** (404, redirect to wrong page). Issue type: `"invalid_url"`.
- **Search results are clearly mis-indexed** — e.g., a German decision appearing under French sources, or a case law result that's actually legislation. Issue type: `"indexing"`.
- **A known important source is missing entirely** — e.g., you know a country's constitutional court should be indexed but `discover_sources` doesn't list it. Issue type: `"unavailable"`.
- **Document text is garbled, truncated, or in the wrong language**. Issue type: `"data_quality"`.

### How to file (after the user agrees)

Use `report_source_issue` with:
- `source`: The source identifier (e.g., `"DE/BVerfG"`, `"FR/Judilibre"`). Use `discover_sources` to find valid source identifiers.
- `issue_type`: One of `"unavailable"`, `"indexing"`, `"invalid_url"`, `"data_quality"`, `"other"`.
- `description`: Neutral and limited to what was missing or broken in the source. No query text, user facts, names, or task description. Example: `"UK/Bailii: Employment Appeal Tribunal decisions from 2020 onward appear to be missing; the source lists 48,000+ documents but returns none for this court and date range."`. Another example: `"FR/Judilibre: document URLs for several Cour de cassation decisions return 404."`.

### How to report to the user

When you encounter a data gap during research, tell the user transparently in your answer, then offer an optional report and show the exact proposed text:

> **Data gap noted**: [Country/Source] returned limited results for this research. [Alternative approach — e.g., "I've supplemented with web search for doctrinal commentary" or "the analysis for this jurisdiction relies on legislation only"].
>
> If you'd like, I can send an optional report to Legal Data Hunter about this gap. It would contain only:
> - `source`: [source]
> - `issue_type`: [type]
> - `description`: "[neutral description of what was missing or broken]"
>
> Should I send it?

Call `report_source_issue` only if the user agrees to that report. If they decline, do not file; the gap stays disclosed in your answer.

Never silently skip a jurisdiction because of missing data. Always disclose the gap and explain what you did instead, whether or not a report is filed.

## Tool and Workflow Feedback

Beyond data gaps, the user may want to report problems with the tools themselves. The same consent rules apply: mention the problem, offer a report, show the exact text, and file only after the user agrees.

### What you may offer to report (beyond data gaps)

**Data quality issues:**
- Zero results where results are expected
- Results that are clearly off-topic or mis-indexed
- Broken/dead document URLs
- Truncated or garbled document text
- Wrong language for the indexed source
- Duplicate documents in search results
- Date fields that look wrong (e.g., 1970-01-01 placeholder dates)

**Tool design issues:**
- A parameter that's confusing or underdocumented — e.g., `alpha` behavior is not self-explanatory
- A tool that requires two calls where one would do — e.g., needing `discover_sources` before `search` just to learn the language of a source
- Search results that include fields with no documentation (what does `score` mean? how is it normalized?)
- Error messages that don't tell you what went wrong or how to fix it
- A tool that would be significantly more useful with a small addition — e.g., `search` returning a `url` field inconsistently

**Workflow inefficiencies:**
- Having to call 3 tools to accomplish what should be 1 (e.g., discover → filter → search for a simple query)
- Rate limiting or latency that degrades the multi-jurisdictional parallel search workflow
- A common research pattern (adversarial search, temporal check) that could be exposed as a built-in tool parameter

### How to word a report

Be specific and neutral. Describe the data or tool problem, not the user's research.

**Good**: `"The 'search' tool returns a 'url' field for some results and omits it for others, with no indication of why. Either always return the URL or add a 'has_url' boolean so the caller can decide whether to fetch."`

**Bad**: `"Some results don't have URLs."`

Include only:
- Source
- Issue type
- What was missing or broken (expected vs. actual, described in terms of the source or tool)
- Optionally, a concrete suggestion about the data or tool, with no user content
