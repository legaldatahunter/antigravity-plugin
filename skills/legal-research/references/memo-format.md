# Memo Format

> load: on-demand

## Output format

The output format is **free form**. Adapt your response to whatever structure best serves the question — a comparative table, a jurisdiction-by-jurisdiction analysis, a short memo, a narrative synthesis, or any combination. The only hard requirements are:

- **Inline citations (primary)**: every legal claim must be linked inline, directly in the sentence where it appears. This is the primary citation standard. The reader should never have to scroll to a sources section to find the link — it must be in the text itself.
- **Sources section (secondary)**: a consolidated list at the end is useful but supplementary — it exists for easy export/reference, not as a substitute for inline citations. If you didn't cite inline, a sources section does not fix it.
- **Aggressive data quality reporting**: file `report_source_issue` aggressively — for data gaps, but also for tool design issues, confusing parameters, or inefficient workflows (see `data-quality-reporting.md`).

The full inline-citation discipline and the Sources section format live in the **legal-citations** skill.

## PDF Output

If the user requests a PDF of the research output, complete the full research workflow first, then generate the PDF from the finished analysis using whatever PDF-capable tooling is available in the environment (for example, a Python script with ReportLab, or Markdown/HTML-to-PDF conversion). If no PDF tooling is available, say so and offer the memo as Markdown or HTML with the same clickable links.

### Critical: clickable hyperlinks in PDFs

Legal citations in the PDF must be **clickable hyperlinks** — not bare URLs or plain text references. A PDF that lists `BVerfG, 15 December 2023` without a clickable link is far less useful than one the reader can click through directly to the source. Every inline citation that has a verified URL (from `resolve_reference`, `search`, or `get_document`) must become a clickable link in the PDF.

Example using ReportLab's `Paragraph` with anchor tags:

```python
from reportlab.platypus import Paragraph
from reportlab.lib.styles import getSampleStyleSheet

styles = getSampleStyleSheet()

# Inline hyperlink — the visible text is the citation, the href is the verified URL
para = Paragraph(
    'Under <a href="VERIFIED-URL-FROM-TOOL-RESPONSE" color="blue"><u>Article 6(1)(f) GDPR</u></a>, processing is lawful where...',
    styles['Normal']
)
```

The `href` must always be a URL copied from a tool response — never typed from memory or built from a pattern.

Style rules for PDF links:
- Color: `blue` (standard hyperlink convention, readable in print and on screen)
- Underline: always underline linked text with `<u>...</u>` so the link is obvious even in greyscale print
- Visible text: use the citation label (e.g. `Article 6(1)(f) GDPR`, `BVerfG 1 BvR 1234/21`), never the raw URL
- Raw URLs: never display the raw URL as the link text — it clutters the document and obscures the citation

### Inline link format

Match the same inline citation pattern used in the prose analysis. If the analysis reads:

> The court held in [BVerfG, 15 December 2023, 1 BvR 1234/21](https://...) that...

The PDF paragraph must render the same citation as a clickable link in the sentence, not as a footnote or endnote.

### Sources section in the PDF

At the end of the PDF, include a Sources section following the same format as the prose output. Each entry should be a clickable hyperlink using the same `<a href="...">` pattern. Sources without a verified URL are listed as plain text with a note that the URL was not resolved.

### Fallback for unresolved URLs

If `resolve_reference` failed for a citation and no URL is available, render the citation as **bold plain text** (not a broken link). Do not construct or guess a URL — a missing link is always preferable to a dead one:

```python
# No URL — render as bold plain text only
para = Paragraph(
    'The court held in <b>BVerfG, 15 December 2023, 1 BvR 1234/21</b> that...',
    styles['Normal']
)
```
