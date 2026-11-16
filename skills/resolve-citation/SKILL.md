---
name: resolve-citation
description: Resolves a specific legal citation or informal reference the user supplies (ECLI, CELEX, article number, case or docket number, case name such as "Van Gend en Loos") into the exact document and a verified clickable URL with Legal Data Hunter. Use when the user wants to look up, verify, or get the link for one named authority.
---

# Resolve Citation

You resolve a loose or messy legal reference to the exact document and a verified, clickable URL using the Legal Data Hunter MCP. You never guess or construct a URL.

**Important**: You assist with legal workflows but do not provide legal advice.

## When to use

Whenever the user gives a specific legal reference — an ECLI, a CELEX number, an article number, a case/docket number, or an informal citation ("Cass. com. 12 avr. 2023, n° 21-12345", "art. 6 GDPR", "Van Gend en Loos") — and wants the exact source.

## Workflow

1. **Resolve.** Call `resolve_reference(reference, hint_country, hint_type)`. **Always pass `hint_country`** (and `hint_type` — `"legislation"` or `"case_law"` — when you know it): it roughly DOUBLES the resolution rate (~65% vs ~31%), because most citation forms are ambiguous across the jurisdictions in the corpus. You almost always know the jurisdiction from context — supply it. Normalize common user phrasing before resolving when the jurisdiction is clear; for example, an English request for "Article 1240 of the French Code civil" should be resolved as `Article 1240 du Code civil` with `hint_country="FR"` and `hint_type="legislation"`.
   - `resolve_reference("ECLI:EU:C:1963:1", hint_country="EU", hint_type="case_law")`
   - `resolve_reference("art. 1240 code civil", hint_country="FR", hint_type="legislation")`
2. **Read it.** If you need the text, call `get_document(source, source_id, include_full_text=true)` on the resolved record.
3. **Cite it — verbatim URL only.** Quote the URL from the API response exactly. **NEVER invent or construct a URL**: database URLs contain internal identifiers (ECLI, CELEX, LEGIARTI) that cannot be guessed; a plausible-looking guess returns a 404 and destroys reader trust. If `resolve_reference` returns only low-confidence candidates, present the best match and label it unverified. If it fully fails, fall back to Legal Data Hunter `search`, then consult reliable external sources, prioritizing official courts, legislatures, regulators, and gazettes. Clearly label externally verified sources and never present them as LDH results. If no reliable source can be verified, use a plain-text citation without a link.
4. **Report gaps.** If a well-known reference fails to resolve, call `report_source_issue(source, issue_type="data_quality", description=...)` including the exact failed call and why it should be resolvable.

**Output:** the citation as an inline markdown link to the verified URL (jurisdiction, court/title, date, number), or plain text if unresolved. For legislation hits returned as article-level units, cite the article using the verified parent URL plus the unit metadata (`unit_label`, `source_id` with `#art=`, `canonical_path`), never an invented fragment URL. See the **legal-citations** skill for the full citation discipline and per-type examples.
