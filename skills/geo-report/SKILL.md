---
name: geo-report
description: Generate the client-facing Prompt-Ready LLM-Readiness Report from an existing scores.json — business-language, evidence-labelled, five dimensions × 20 points, no market statistics, no revenue projections.
version: 3.0.0
tags: [geo, report, client-deliverable, prompt-ready]
---

# Prompt-Ready Client Report (v3.0)

Input: `geo-reports/<domain>/scores.json` (latest). If missing, say so and
point to `/geo audit <url>`. Never rebuild scores from markdown or memory.

Brand, language and paper size come from `geo/config.md`. Scoring and labels
come from `geo/scoring.md`. Statistics only from `geo/references.md` (verified).

## Audience and tone

Business owner or marketing lead. Short sentences, concrete, no jargon without
a one-line gloss. State findings as what was observed ("the homepage carries a
`noindex` tag"), not as predictions ("you will lose traffic"). No hedging
filler, no hype, no "AI is eating search" framing.

## Hard rules

1. Every finding line is labelled **Measured**, **Assessed** or **Unverifiable**
   (a small tag or column — see template).
2. The total shown equals the sum of the five dimensions shown.
3. No market statistics unless `verified` in `references.md`. No € values, no
   traffic estimates, no "could increase citations by X%".
4. Unverifiable checks are described as data gaps, not as site failures.
5. Competitor numbers appear only if the competitor was scored with the same
   collector and the same checks; otherwise show presence/absence on the
   engine-cited sources only.
6. The live visibility test is described as a snapshot with date, questions and
   engines named.

## Template — `geo-reports/<domain>/GEO-CLIENT-REPORT-<date>.md`

```markdown
# LLM-Readiness Report — [Brand]
Prompt-Ready · [date] · [domain]

## Your score: [total]/100 — [label]

| Dimension | Score | What it means for you |
|---|---|---|
| Structure & Navigation | [x]/20 | [one plain sentence] |
| Content Quality | [x]/20 | [one plain sentence] |
| Technical SEO for LLMs | [x]/20 | [one plain sentence] |
| AI-Specific Readiness | [x]/20 | [one plain sentence] |
| Brand Entity & Authority | [x]/20 | [one plain sentence] |

[Radar or bar chart in the PDF version.]

## Are AI engines recommending you today? (snapshot, [date])

We asked ChatGPT, Claude and Perplexity three questions a potential client
would ask, without naming your brand:
1. "[q1]" 2. "[q2]" 3. "[q3]"

**Your site was cited in [x] of 9 answers and your brand was mentioned in
[y] of 9.** The engines relied mainly on: [sources]. Being present and well
described on these sites is how your category gets recommended today.
AI answers change over time and between users — this is a signal, not a ranking.

## Site performance (Google PageSpeed, mobile, [field|lab] data)
| Performance | LCP | CLS | INP / TBT |
|---|---|---|---|
| [x]/100 | [x] s | [x] | [x ms ([field INP|lab TBT])] |

## Company data (as found on your site)
- Company: … · Activity: … · Services: … · Contact: … · Social: …
- Founded: [value or "not stated on site"] · Team: [value or "not stated on site"]

## What we found

### Structure & Navigation — [x]/20
| Check | Result | Evidence | Type |
|---|---|---|---|
| Sitemap | ✓ / ✗ / partial | [evidence] | Measured |
| … | | | |
[3-4 sentences.]

### Content Quality — [x]/20
[same]

### Technical SEO for LLMs — [x]/20
[same]

### AI-Specific Readiness — [x]/20
[same]

### Brand Entity & Authority — [x]/20
[same]

## Top 5 priorities
Each item: **what to do**, **why it matters** (one sentence, tied to a check),
**points recoverable**, **effort**.

## Roadmap
**Phase 1 — Quick wins (1-2 weeks)** · **Phase 2 — Optimisation (2-6 weeks)** ·
**Phase 3 — Authority (1-3 months)**

## Page-by-page plan
| URL | Priority | Actions |

## About this assessment
[Paragraph from geo-audit template "About this assessment", plus: "Scores are
reproducible: 70 of 100 points come from measured checks that give the same
result on every run; 30 points are expert assessment against a published rubric
(Appendix)." ]

## Appendix — Scoring rubric
[§3 and §8 of geo/scoring.md]

## Appendix — Live test log
[questions, engines, timestamps, excerpts, sources]

---
Prompt-Ready · www.prompt-ready.com · AI visibility starts with knowing where you stand.
```

## Length

1,500-3,000 words. Tables carry the facts; prose carries the interpretation.
Do not pad.
