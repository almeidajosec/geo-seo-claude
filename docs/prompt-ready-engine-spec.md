# Prompt-Ready LLM-Readiness Report — Engine Change Spec (v2.4 → v3.0)

Applies to the automated report (currently "Assessment Engine v2.4 · Powered by
Claude Opus 5"). Goal: same five dimensions and the same 0-20 rubric as the GEO
toolkit (`geo/scoring.md`), reproducible scores, every line evidence-labelled.

Reference sample: `LLM-Readiness-jose-ai-solutions.web.app-2026-11-06.pdf`.

---

## 1. Scoring (breaking change → v3.0)

| Change | Why |
|---|---|
| Adopt `geo/scoring.md` §3 verbatim: 5 dimensions × (14 measured + 6 assessed) | Scores become comparable with the toolkit and across months |
| Compute measured checks in code, not in the LLM prompt | Removes the ±5 run-to-run variance for 70 of 100 points |
| LLM scores only the five assessed checks (0-6 each) with the §4 rubric, and must return an example + a gap | Bounds variance to ≈±3 total and makes every assessed score explainable |
| Total = sum of dimensions, asserted in code | Prevents the "24.5 → 28" class of error |
| Labels per §8 (Strong / Good / Developing / Weak / Critical) | One vocabulary across both tools |
| Store `scores.json` (format in `geo/SKILL.md`) per run, per domain | Enables monthly deltas with the same file the toolkit uses |
| Add "scoring model v3.0" to the footer; mark pre-v3 reports as non-comparable | Honest change management |

## 2. Collector (deterministic layer)

One collector produces `audit-data.json`; the LLM never fetches.

| Data | Method | Feeds |
|---|---|---|
| Raw HTML + headers for homepage and up to 6 key pages | HTTP fetch with a browser UA; keep `X-Robots-Tag`, canonical, meta robots, OG/Twitter, JSON-LD blocks, headings, text | A4-A5, A8, B1-B6, C1-C5, E1, E4 |
| robots.txt parsed per AI crawler | Fetch + parse (`fetch_page.py robots`) | A3, D2 |
| sitemap.xml / index, lastmod | Fetch + parse (`fetch_page.py sitemap`) | A1, A2, A6, A7 |
| AI-UA fetch test | Fetch homepage once per Tier-1 UA; record status + body length | D3 |
| Server-rendered ratio | raw text length ÷ Playwright `inner_text(body)` length | D1 |
| PageSpeed Insights API (mobile) | CrUX field metrics if present → label **field**; else lab LCP/CLS/**TBT** → label **lab** | C6 |
| Wikipedia (en + pt) search API, Wikidata `wbsearchentities` | Brand name as on site | E2 |
| LinkedIn company page | Link on site, else web search `site:linkedin.com/company "<brand>"` | E3 |
| Live AI visibility test | 3 unbranded category questions × ChatGPT, Claude, Perplexity (live search) → 9 answers; record mention, citation, sources, model, timestamp | D5, E5 |
| Presence on engine-cited sources | For each source domain from the test: web search `site:<source> "<brand>"` | E5 |

Any failed step is logged in `errors[]`; the dependent checks render
**Unverifiable (0 pts)** with the reason. No inference from absence.

## 3. Fixes to the current report

| Item | Current | Change |
|---|---|---|
| CWV box | "TBT: 870 ms" under the **INP** label | Show `INP (field)` when CrUX has it; otherwise `TBT (lab)`. Never mix. Add "field/lab" to the box title |
| Visibility headline | "Recommended by 0 of 3 AI engines" from one question | "Cited in X of 9 answers · mentioned in Y of 9" from 3 questions; keep the per-question table; keep the snapshot caveat |
| Competitor benchmark | 66/100 for jelly.pt with no evidence; "roadmap above" (it is below) | Either run the full collector on the competitor and show its five dimension scores with evidence, or show only "competitor present on N of the sources the engines cited". Fix the cross-reference |
| Radar "LLM Minimum Threshold" | Undefined | Define as 12/20 per dimension ("Good" floor) and say so in a footnote, or remove |
| Score explainability | Dimension prose only | Checklist table first (Check · Status · Evidence · Pts), then 3-4 sentences |
| Score variance disclaimer | "±5 points" | "70 of 100 points are measured and reproducible; 30 are assessed against the published rubric and may vary by up to ±3" |
| Brand spelling | Prompt-Ready / PromptReady / PROMPTREADY | **Prompt-Ready** everywhere (`geo/config.md`) |
| FAQPage recommendation | No caveat | Add "no longer earns Google rich results; still useful for AI parsing" |
| llms.txt | Correctly cautious | Keep; worth 1 point (D4) |
| Company data "N/A" | Shown | Prefer "not stated on site" — it is a finding about the site, not missing data |
| Rubric | Not shown | Append §3 + §8 of `scoring.md` as the last page |
| Raw test log | Not shown | Append questions, engines, model, timestamp, answer excerpt, sources |

## 4. Report layout (v3)

1. Header: Prompt-Ready · LLM-Readiness Report · date · domain
2. Score panel: total /100 + label; five-axis radar (0-20); page/link/image counts
3. Live AI visibility test: "cited X of 9 · mentioned Y of 9"; per-question table; sources the engines relied on; caveat
4. Core Web Vitals (field/lab labelled)
5. Company data (as found on site)
6. Executive summary (facts only; no market stats; no projections)
7. Five dimension sections, each: checklist table → short interpretation
8. Top 5 priorities (what · why · check id · points recoverable · effort)
9. Page-by-page plan
10. Roadmap (Phase 1 / 2 / 3)
11. About this assessment (measured vs assessed; model version; snapshot caveat)
12. Appendix A — scoring rubric · Appendix B — live test log
13. CTA + footer (Prompt-Ready · www.prompt-ready.com)

## 5. Prompt rules for the assessed layer (the only LLM-scored part)

- Input: `audit-data.json` + the §4 rubric. Output: for each of A9, B7, C7, D6,
  E6 → `{points 0-6, example (URL + snippet), missing}` plus 3-5 recommendations
  tied to check ids.
- Forbidden in output: any statistic not present in the input, any traffic or
  revenue estimate, any claim about engine algorithms, any platform fact not
  dated and sourced.
- Language: the site's primary language (PT-PT or EN) per `geo/config.md`.

## 6. Monitoring

- Keep every run's `scores.json` under `clients/<domain>/`.
- Monthly delta = check-by-check comparison (see `skills/geo-compare/SKILL.md`).
- Visibility test: reuse the same 3 questions per client every run; change them
  only with a note in the report.
