---
name: legal-citations
description: Citation discipline for legal answers built with the Legal Data Hunter MCP — cite every legal claim inline with a verified URL, never invent or construct a URL, verify through resolve_reference, get_document or search, and end with a Sources section. Use when drafting any cited legal analysis or when the user asks how to cite cases, statutes, treaties, or doctrine.
---

# Legal Citations

Read this before writing any cited legal analysis. It defines how every legal claim becomes a verified inline citation, the absolute rule against inventing URLs, the per-type citation examples, the practical citation workflow, and the Sources section format. It applies to all Legal Data Hunter research output.

## Inline Citation Standards

Every legal claim must be traceable to its source. Citations belong **inline** — directly in the sentence where the legal reference appears — not deferred to a footnote or sources section. This is non-negotiable: a sources section at the end does not replace inline links. Both should exist, but the inline link is primary.

**Cite inline aggressively.** Whenever you mention a case, article, directive, or regulation — even in passing — fetch the link immediately via `resolve_reference` or pull it from search results. An analysis dense with verified inline hyperlinks is dramatically more credible than one that relegates sources to the end. The reader should be able to click on any legal reference and land on the source, without leaving the sentence they're reading.

### The golden rule: NEVER invent a URL — always use `resolve_reference` or `get_document`

This is the single most important rule in the citation workflow. Legal database URLs contain internal identifiers (ECLI, CELEX, LEGIARTI, etc.) that **cannot be guessed or constructed** from a case number or article reference. A URL that looks plausible will return a 404 if the identifier is wrong. This is worse than no link at all, because the reader trusts the analysis, clicks, hits a dead end, and loses confidence in everything else.

**The rule is absolute:**

1. **Every hyperlink must come from a verified source.** Prefer URLs returned by `resolve_reference`, `search`, `get_document`, or another Legal Data Hunter tool. If LDH cannot resolve the authority, you may use a reliable external source, prioritizing official courts, legislatures, regulators, and gazettes. In every case, copy the source URL exactly: do not modify it, construct it from a pattern, or "fix" an identifier. Model memory alone is never verification.

2. **Use `resolve_reference` as your primary citation tool.** Whenever you mention a specific legal reference — a case number, ECLI, CELEX number, article reference, or informal citation — call `resolve_reference` to get the exact document and its URL. **Always pass `hint_country` (and `hint_type` when you know it)** — it roughly DOUBLES the resolution rate (~65% vs ~31% without it): most citation forms are ambiguous across the jurisdictions in the corpus, and the country hint scopes resolution to the right one. You almost always know the jurisdiction from context — supply it. Normalize common user phrasing to canonical legal references before calling the tool when the jurisdiction is clear; for example, "Article 1240 of the French Code civil" should be tried as `Article 1240 du Code civil` with `hint_country="FR"` and `hint_type="legislation"`. Examples:
   - `resolve_reference("art. 49 TFEU", hint_country="EU", hint_type="legislation")` → gets the actual Treaty article
   - `resolve_reference("C-212/97 Centros", hint_country="EU", hint_type="case_law")` → gets the CJEU decision
   - `resolve_reference("ECLI:EU:C:1999:126", hint_country="EU", hint_type="case_law")` → resolves an ECLI to the exact case
   - `resolve_reference("Regulation (EU) 2016/679", hint_country="EU", hint_type="legislation")` → gets the GDPR text
   - `resolve_reference("art. 1240 code civil", hint_country="FR", hint_type="legislation")` → gets the French Civil Code article
   - `resolve_reference("Supreme Court of Cassation, 599/19", hint_country="BG", hint_type="case_law")` → without `hint_country="BG"` this is ambiguous (many countries have a Court of Cassation) and won't resolve

3. **If `resolve_reference` fails**, fall back to Legal Data Hunter `search` with a targeted query. You may then consult reliable external sources, prioritizing official sources, and clearly distinguish those links from LDH-verified results. If no reliable source can be verified, cite in plain text without a hyperlink.

4. **ALWAYS file feedback when a reference fails to resolve** (see `references/data-quality-reporting.md` in the legal-research skill for full guidance). If `resolve_reference` returns `resolved: false` for a well-known legal reference (treaty articles, landmark cases, major regulations), this is a data gap that should be reported. Call `report_source_issue` with:
   - `source`: The most likely source (e.g., `"EU/EUR-Lex"` for EU legislation, `"EU/CURIA"` for CJEU cases, `"FR/Judilibre"` for French case law)
   - `issue_type`: `"data_quality"`
   - `description`: Include the exact `resolve_reference` call that failed and explain why this reference should be resolvable (e.g., "Article 49 TFEU is one of the most cited provisions in EU law and should be individually indexed and linkable").

   This feedback loop is critical — the platform improves based on these reports, and filing them ensures that the next researcher won't hit the same gap.

5. **FORBIDDEN**: Fabricating a URL by guessing the identifier format. Never construct a URL like `https://eur-lex.europa.eu/...CELEX:someGuessedId`. Never point a treaty article link to a case URL. Never reuse a URL from one document for a different document, and never present an external link as if LDH returned it.

### How to build citations from search results

Each search result includes `source`, `source_id`, and often `url` fields. After retrieving a document via `get_document` or `resolve_reference`, use the metadata to build a proper citation.

**Case law**: Always call `resolve_reference` with the case number or ECLI, then cite with jurisdiction, court, date, and case number, linked to the verified URL:

> The Bundesverfassungsgericht held in [BVerfG, 15 December 2023, 1 BvR 1234/21](URL-from-resolve_reference) that...

> La Cour de cassation a confirmé dans [Cass. com., 9 juillet 2025, n° 24-10.428](URL-from-resolve_reference) que...

**Legislation**: Always call `resolve_reference` with the article reference, then cite with the full reference linked to the verified URL:

> Under [Article 6(1)(f) GDPR](URL-from-resolve_reference), processing is lawful where...

> Selon l'[article 1240 du Code civil](URL-from-resolve_reference), tout fait quelconque de l'homme...

**Treaty articles**: Call `resolve_reference` — do NOT link treaty articles to case URLs. Treaty articles and court decisions are different documents:

> Freedom of establishment under [Articles 49 and 54 TFEU](URL-from-resolve_reference) protects...

**Doctrine** (the `doctrine` namespace is **official doctrine only** — regulator guidance, ministry circulars, official interpretive notes; *not* academic treatises or law-firm commentary): use the issuing body and title, linked if available from search results:

> As clarified in [EDPB Guidelines 05/2020 on consent under Regulation (EU) 2016/679](URL-from-search-results)...

**When no URL is available** (resolve_reference failed, search returned nothing): Cite in plain text without a hyperlink. This is honest:

> Article 823(1) BGB provides for liability in cases of...

**Article-level legislation units**: If `search` returns an article-level legislation unit rather than a separate official article URL, cite it as the article/provision using the verified parent URL plus the returned article metadata. Do not treat a parent-document URL as a failure when the result also provides article-level evidence such as `source_id` containing `#art=`, `unit_label`, `unit_number`, `canonical_path`, or a snippet/text fragment. Write the citation so the reader sees the precise article and the official document link, and include a short quoted or paraphrased text fragment when useful:

> Under [Article 17 GDPR — right to erasure](URL-from-search-result), the returned LDH unit `32016R0679#art=17` / `... > Article 17` states that the data subject has the right to obtain erasure where one of the listed grounds applies.

The verified URL must still come from `search`, `resolve_reference`, or `get_document`; the article identity may come from the returned unit metadata and text fragment. Never append or invent a fragment (`#...`) to a URL yourself.

### Practical workflow for citation

**Before writing**: As you research, keep a running list of every legal reference you'll need to cite. Before writing the analysis, batch-resolve all of them by calling `resolve_reference` in parallel for each reference. This front-loads the citation work and ensures you have verified URLs ready when you start writing.

**While writing**: If you realize mid-sentence that you need to cite something you haven't resolved yet, don't guess — add it to a batch. Finish the paragraph, then call `resolve_reference` for all missing references in parallel, and fill in the links. This is fast and guarantees every link is verified.

**The investment is always worth it**: An inline-cited analysis is dramatically more credible and useful. Every `resolve_reference` call takes a moment, but the payoff in reader trust is enormous. When in doubt, resolve and cite. It's better to have 20 verified inline links than 5 links and 15 bare text references.

### Sources section

At the end of every analysis, include a consolidated "Sources" section listing all authorities relied upon. Format each entry as a markdown link if you have a verified URL, or as plain text if you don't:

```
## Sources

**Case law:**
- [BVerfG, 15 December 2023, 1 BvR 1234/21](URL) — Germany
- [Cass. com., 9 juillet 2025, n° 24-10.428](URL) — France
- CJEU, Case C-311/18, Schrems II — EU (reference not verified)

**Legislation:**
- [Article 6(1)(f) GDPR](URL) — EU
- [Article 1240 Code civil](URL) — France

**Doctrine:**
- [Author, Title (Year)](URL) — Jurisdiction
```

Group by type (case law, legislation, doctrine) and indicate the jurisdiction for each source. This makes comparative analysis easy to navigate.
