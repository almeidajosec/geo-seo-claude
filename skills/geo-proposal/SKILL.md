---
name: geo-proposal
description: >
  Generate a Prompt-Ready service proposal from an existing scores.json: findings
  translated to business language, recommended package, timeline and terms.
  Prices come from geo/config.md — never invented. Use when the user says
  "proposal", "proposta", "generate proposal", or after an audit for a prospect.
version: 3.0.0
tags: [geo, business, proposal, sales, prompt-ready]
allowed-tools: Read, Write, Bash, Glob
---

# Prompt-Ready Proposal Generator (v3.0)

## Inputs

1. `geo-reports/<domain>/scores.json` (latest). If missing → "run `/geo audit
   <url>` first". Never estimate scores.
2. `geo/config.md` → brand, pricing table, language. If a package price is
   `TBD`, render **[PRICE TBD]** and tell the user before they send it.
3. `geo/scoring.md` → labels and the rubric for the appendix.

## Command

```
/geo proposal <domain> [--package essentials|growth|authority] [--client-name "Name"] [--lang pt|en]
```

## Package recommendation (by total score)

| Total | Recommended package | Rationale shown to client |
|---|---|---|
| 0-39 | Authority | Several measured foundations fail; needs implementation support and authority building |
| 40-59 | Growth | Foundations partly in place; monthly optimisation closes the gaps |
| 60-79 | Essentials | Solid base; targeted fixes and quarterly monitoring |
| 80-100 | Essentials or quarterly check-in | Maintain and monitor |

## Hard rules

- No market statistics unless `verified` in `geo/references.md`.
- No ROI table, no "€ value at risk", no "+X% traffic". If the client shares
  GA4 / Search Console access, a separate, data-based opportunity analysis can
  be offered as a deliverable — say that instead.
- "Expected score improvement" is stated only as **points recoverable from the
  specific checks in scope**, taken from `scores.json` (e.g. "fixing C1, A1, A3
  and E1 recovers up to 13 points"). Never a generic "+25-40 points".
- Results clause: effort and methodology are guaranteed, outcomes are not.

## Template — `geo-reports/<domain>/PROPOSAL-<date>.md`

```markdown
# LLM-Readiness Optimisation Proposal
## [COMPANY] — AI search visibility

**Prepared by:** Prompt-Ready · **For:** [contact], [company]
**Date:** [date] · **Valid until:** [date + 30 days] · **Ref:** PR-[YYMMDD]-[domain]

## Summary
Our LLM-Readiness audit of [domain] on [date] scored **[total]/100 ([label])**.
The three issues with the most impact on whether AI engines can find, trust and
cite [company] are:
1. **[finding tied to check id]** — [what it means for the business]
2. **[finding]** — [meaning]
3. **[finding]** — [meaning]

In our live test, [company] was cited in [x] of 9 AI answers to category
questions; the engines relied instead on [top 3 sources].

We recommend the **[package]** package at **[price]/month**. Work in scope
recovers up to **[n] points** of the current gap, bringing the site to an
expected band of **[label]**.

## Audit results
| Dimension | Score | Status |
|---|---|---|
| Structure & Navigation | [x]/20 | [label] |
| Content Quality | [x]/20 | [label] |
| Technical SEO for LLMs | [x]/20 | [label] |
| AI-Specific Readiness | [x]/20 | [label] |
| Brand Entity & Authority | [x]/20 | [label] |
| **Total** | **[total]/100** | **[label]** |

### Key findings
For each of the top 3-5 failed checks:
**[Check name]** · *Measured* — What we found: … · Why it matters: … · Our fix: … · Points recoverable: +[n]

## Packages

### Essentials — [price]/month · min. 6 months
Quarterly re-audit and progress report · crawler access and indexability fixes
(robots.txt, noindex, canonical) · Organization/Person schema with sameAs ·
llms.txt · sitemap/robots hygiene · email support.
**Checks in scope:** [ids] · **Points recoverable:** up to [n]

### Growth — [price]/month · min. 6 months  ← recommended for [company]
Everything in Essentials plus monthly re-audit and progress report · monthly
call · content restructuring for citability (up to [n] pages/month) · About,
contact and author/credential pages · FAQ blocks · entity work (Wikidata,
LinkedIn company page, directory profiles the engines cite).
**Checks in scope:** [ids] · **Points recoverable:** up to [n]

### Authority — [price]/month · min. 12 months
Everything in Growth plus technical implementation support (SSR, CWV, domain
migration) · dated content hub ([n] articles/month) · third-party presence and
original data publication · quarterly live visibility review across engines.
**Checks in scope:** [ids] · **Points recoverable:** up to [n]

## Timeline
**Month 1** — baseline, quick wins ([ids]) · **Months 2-3** — optimisation
([ids]) · **Months 4-6** — authority ([ids]) · **Month 6** — full re-audit,
before/after report, renewal review.

## How we measure progress
Every month we re-run the same audit with the same published rubric (appendix)
and report the score delta per dimension and per check, plus the live AI
visibility snapshot. 70 of 100 points are measured checks that are fully
reproducible.

## Investment
| Package | Monthly | 6 months | 12 months |
|---|---|---|---|
| Essentials | [price] | [×6] | [×12] |
| Growth | [price] | [×6] | [×12] |
| Authority | [price] | [×6] | [×12] |
Prices exclude VAT. Monthly invoicing, 15-day terms.

## Next steps
1. Review and send questions · 2. 30-minute walkthrough call: [link] ·
3. Service agreement · 4. Kick-off.

## Terms
Minimum term per package · 30-day notice after minimum · confidentiality ·
we guarantee methodology and effort, not specific outcomes · access needed:
CMS/dev contact; GA4 and Search Console read access if available.

## Appendix — Scoring rubric
[§3, §8 of geo/scoring.md]
```

## Output

Save the file, print the path, the recommended package and any `[PRICE TBD]`
placeholders still present.
