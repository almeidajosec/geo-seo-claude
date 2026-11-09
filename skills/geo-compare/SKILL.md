---
name: geo-compare
description: >
  Progress report between two Prompt-Ready audits of the same domain: compares
  scores.json files check by check across the five dimensions, lists what was
  fixed, what regressed, and what is still open. Use when the user says
  "compare", "delta", "progress", "monthly report", "progresso", "relatório mensal".
version: 3.0.0
tags: [geo, business, delta, monthly, reporting, prompt-ready]
allowed-tools: Read, Write, Bash, Glob
---

# Prompt-Ready Progress Report (v3.0)

## Inputs

Two `scores.json` files for the same domain in `geo-reports/<domain>/`
(default: oldest as baseline, newest as current; or two explicit paths).
Both must be `model_version: 3.0`.

- If the baseline is a v2 (six-category) audit: **do not compute a delta**.
  Report "Re-baselined on model v3.0 on [date]" and present the current scores
  as the new baseline. Legacy totals are not comparable (see `scoring.md` §9).
- If a file is missing: say so and point to `/geo audit <url>`. Never estimate
  a baseline from a markdown report.

## Delta logic

For each check id: `delta = current.points - baseline.points`; status change
(fail→pass = "fixed", pass→fail = "regressed", unverifiable→anything =
"now measured", anything→unverifiable = "could not verify this run").
Dimension and total deltas are sums. Trend symbols: ▲ (+), ▼ (−), ── (0).

Because 30 points are assessed, flag a dimension as **"within assessment
variance"** when its delta is ±1 and all its measured checks are unchanged.

## Template — `geo-reports/<domain>/PROGRESS-<date>.md`

```markdown
# LLM-Readiness Progress Report — [Brand]
Prompt-Ready · period [baseline date] → [current date]

## Summary
[2-3 sentences: total change, which measured checks were fixed, what regressed,
focus for next period. No celebration language beyond the facts.]

## Score
| | Baseline | Current | Change |
|---|---|---|---|
| **LLM-Readiness Score** | [x]/100 ([label]) | [y]/100 ([label]) | [▲/▼ ±n] |

| Dimension | Baseline | Current | Change | Note |
|---|---|---|---|---|
| A. Structure & Navigation | [x]/20 | [y]/20 | [±n] | [fixed: A1, A3 / within variance] |
| B. Content Quality | | | | |
| C. Technical SEO for LLMs | | | | |
| D. AI-Specific Readiness | | | | |
| E. Brand Entity & Authority | | | | |

## Live AI visibility (snapshot vs snapshot)
| | Baseline | Current |
|---|---|---|
| Cited (of 9) | [x] | [y] |
| Mentioned (of 9) | [x] | [y] |
| Same 3 questions used? | yes/no | |

## Fixed this period
| Check | What changed | Evidence (current) | Points |
|---|---|---|---|

## Regressed
| Check | What changed | Evidence | Points |

## Still open (ordered by points recoverable)
| Check | Status | Points recoverable | Planned for |

## Could not verify this run
[checks that were measured in the baseline but unverifiable now — explain why
(collector error, site blocked the fetch, etc.). These are data gaps, not
regressions.]

## Next period focus
| Priority | Action | Checks | Points recoverable | Owner |

## Trajectory
| Audit date | Score | Change | Main change |
|---|---|---|---|
[one row per scores.json in the client folder]

## Appendix — Scoring rubric
[§3, §8 of geo/scoring.md]
```

## Hard rules

- No "estimated business impact", traffic or € figures.
- A decline is reported as a decline. Do not reframe as "newly discovered
  opportunities" unless the cause is a check that moved from unverifiable to
  measured — then say exactly that.
- Target scores for future months are stated only as "points recoverable from
  checks planned in scope", never as a generic trajectory.
