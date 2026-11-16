# Authority Hierarchy

> load: on-demand

This module is synthesized from the research-strategy ordering used elsewhere in this skill — search legislation first, then case law, then doctrine, prioritizing `court_tier: 1`. It is a short orientation on how to weight authorities, not a substitute for jurisdiction-specific legal knowledge.

## Default ordering of authorities

Ground the analysis from the most binding source down:

1. **Legislation (positive law)** — search `namespace: "legislation"` first. Statutes, codes, regulations, and treaty provisions establish the framework that everything else interprets. Anchor the analysis here before looking at how courts read it.
2. **Case law (binding interpretation)** — search `namespace: "case_law"`. Filter by `court_tier: 1` (supreme/constitutional) first, because the highest courts carry the most weight. Use `court_tier: 2` (appellate) and `court_tier: 3` (first instance) as supporting or trend evidence, not as controlling authority.
3. **Doctrine (official interpretive material)** — search `namespace: "doctrine"`. The `doctrine` namespace holds official doctrine — regulator guidance, ministry circulars, and official interpretive notes (not academic or law-firm commentary). It synthesizes and contextualizes; it explains why a decision matters and what changed, but it is persuasive, not binding.

## Court tiers

The `court_tier` filter encodes the binding hierarchy directly:

- `1` = supreme/constitutional — most weight; prioritize for any risk assessment.
- `2` = appellate.
- `3` = first instance.

When a tier-1 ruling exists, it controls. Lower-tier decisions matter most where tier-1 authority is silent, where they show an emerging trend, or where the temporal check flags the leading tier-1 case as potentially outdated.

## Hierarchy by legal system

- **Civil-law jurisdictions (e.g. FR, DE, ES, IT, PT, NL, EE, BG, AT, BE)**: codified legislation is the primary authority; supreme/constitutional court decisions (`court_tier: 1`) interpret it; doctrine is influential but persuasive. Search legislation first, then tier-1 case law.
- **Common-law jurisdictions (e.g. UK, IE)**: statute and binding precedent from the highest courts (`court_tier: 1`) sit at the top together; lower-court decisions and doctrine are persuasive. Treat both legislation and tier-1 case law as controlling.
- **EU level**: EU legislation (Treaties, Regulations, Directives) and CJEU case law (CURIA) are paramount where they apply, and can override or constrain national positions. When a question spans member states, resolve the EU-level position before reconciling national divergences. English queries work well for EU-level sources (CURIA, EuroParl, EUR-Lex).

Where jurisprudence diverges across jurisdictions — or where an EU directive is interpreted differently across member states — flag the divergence explicitly. It is a risk factor in itself because it creates uncertainty about how the issue will be resolved in practice.
