---
name: legal-risk-assessment
description: Scores legal risk with a severity × likelihood matrix (1–5 each, score 1–25, GREEN/YELLOW/ORANGE/RED), scores each jurisdiction separately with the highest-risk one driving the overall level, and sets triggers for escalating to outside counsel. Use when the user asks to rate, score, or prioritize legal or compliance risk, or whether to involve counsel.
---

# Legal Risk Assessment

Use this skill when legal research feeds into a risk assessment. It defines the severity × likelihood matrix, multi-jurisdictional scoring, and when to escalate to outside counsel. A risk assessment is only as good as the legal analysis that feeds it — complete the research (including the adversarial, doctrinal, and temporal checks, see the **legal-research** skill) before scoring.

## Risk Assessment Framework

When the research feeds into a risk assessment, use the severity x likelihood matrix below.

### Severity (impact if the risk materializes)

| Level | Label | Description |
|---|---|---|
| 1 | **Negligible** | Minor inconvenience; no material financial, operational, or reputational impact. |
| 2 | **Low** | Limited impact; minor financial exposure (< 1% of relevant value); minor operational disruption. |
| 3 | **Moderate** | Meaningful impact; material financial exposure (1-5% of relevant value); noticeable disruption. |
| 4 | **High** | Significant impact; substantial financial exposure (5-25% of relevant value); regulatory scrutiny likely. |
| 5 | **Critical** | Severe impact; major financial exposure (> 25% of relevant value); fundamental business disruption; regulatory action likely. |

### Likelihood (probability the risk materializes)

| Level | Label | Description |
|---|---|---|
| 1 | **Remote** | Highly unlikely; no known precedent; would require exceptional circumstances. |
| 2 | **Unlikely** | Could occur but not expected; limited precedent; specific triggers needed. |
| 3 | **Possible** | May occur; some precedent exists; triggering events are foreseeable. |
| 4 | **Likely** | Probably will occur; clear precedent; common triggering events. |
| 5 | **Almost Certain** | Expected to occur; strong precedent; triggers are present or imminent. |

### Risk Score = Severity x Likelihood

| Score Range | Risk Level | Color |
|---|---|---|
| 1-4 | Low Risk | GREEN |
| 5-9 | Medium Risk | YELLOW |
| 10-15 | High Risk | ORANGE |
| 16-25 | Critical Risk | RED |

### Multi-jurisdictional risk scoring

When assessing risk across jurisdictions, score each jurisdiction independently. The overall risk level is driven by the **highest-risk jurisdiction** that applies to the user's situation — because a single high-exposure jurisdiction can dominate the overall risk profile.

Present the per-jurisdiction scores in a comparison table:

```
| Jurisdiction | Severity | Likelihood | Score | Level |
|---|---|---|---|---|
| France | 3 | 4 | 12 | ORANGE |
| Germany | 2 | 3 | 6 | YELLOW |
| EU (CJEU) | 4 | 3 | 12 | ORANGE |
| **Overall** | | | **12** | **ORANGE** |
```

Where jurisprudence diverges across jurisdictions, flag the divergence explicitly — it's a risk factor in itself, because it creates uncertainty about how the issue will be resolved in practice.

## When to Escalate to Outside Counsel

Engage outside counsel when:

- **Active litigation** in any covered jurisdiction
- **Government investigation** or regulatory inquiry
- **Criminal exposure** for the organization or personnel
- **Novel legal issues** or questions of first impression
- **Jurisdictional conflict**: different jurisdictions reach opposite conclusions on the same question — this is inherently high-risk and benefits from local counsel in each jurisdiction
- **Material financial exposure** exceeding the organization's risk tolerance
- **Regulatory changes** requiring compliance program development across multiple countries
