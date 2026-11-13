---
name: multi-issue-memo
description: Writes a long-form legal memo that splits an enumerated multi-part question ("(1)… (2)… (3)…") into sub-issues and resolves each with its own controlling statute and leading case, fully inline-cited via Legal Data Hunter, optionally exported to PDF. Use for bar-exam-style fact patterns or advisory questions with several distinct issues.
---

# Multi-Issue Legal Memo

You produce a long-form legal memo that decomposes a multi-part question into its sub-issues and resolves each one with its own controlling authority, fully inline-cited, using the Legal Data Hunter MCP.

**Important**: You assist with legal workflows but do not provide legal advice. Analyses should be reviewed by qualified legal professionals.

## When to use

Bar-exam-style or multi-issue advisory questions — anything that enumerates sub-questions ("(1)… (2)… (3)…") or spans several distinct legal issues.

## Workflow

1. **Decompose.** Break the question into its distinct sub-issues; they are usually enumerated. Treat **each enumerated point as its own required-authority target**.
2. **Per sub-issue, retrieve the controlling authority.** For each one, run the research loop — `search(namespace="legislation")` for the governing article, then `search(namespace="case_law", court_tier=1)` for the leading case, in the source language. Do **not** anchor the whole memo to one headline authority: a memo that states the right outcome but cites only some of the controlling provisions still under-supports its reasoning.
3. **Adversarial + temporal checks** per sub-issue: search contrary terms; if the newest supporting case is >3 years old, re-search the last 24 months and flag potential staleness.
4. **Resolve every citation.** Batch-call `resolve_reference(reference, hint_country, hint_type)` in parallel for every authority before writing (passing `hint_country` roughly doubles resolution). Every material proposition gets its own verified inline citation. **NEVER invent or construct a URL**; if an authority cannot be resolved or linked (e.g. a treaty/constitutional provision or an out-of-coverage decision), name it in prose and anchor that proposition to the closest resolvable authority that states the same rule.
5. **Write.** Structure the memo by sub-issue, each with its controlling statute + leading case cited inline. End with a consolidated **Sources** section grouped by type and jurisdiction. For PDF output with clickable hyperlinks, follow `references/memo-format.md` in the legal-research skill.

See the **legal-research** skill (task-specific retrieval discipline) and the **legal-citations** skill (golden rule + Sources format) for depth.
