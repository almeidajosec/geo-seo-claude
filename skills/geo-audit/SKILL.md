---
name: geo-audit
description: Full-site Prompt-Ready LLM-Readiness audit. Collects deterministic data, scores five dimensions (Structure & Navigation, Content Quality, Technical SEO for LLMs, AI-Specific Readiness, Brand Entity & Authority) at 20 points each via parallel agents, and writes an evidence-labelled audit report plus scores.json for monitoring.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
  - WebSearch
  - Write
  - Agent
---

# Prompt-Ready Full Audit (v3.0)

Scoring: `geo/scoring.md` only. Pipeline: `geo/SKILL.md` → "Pipeline for /geo audit".
This file defines the **report template** and the quality checks on the merged result.

## Steps

1. Run Phase 1 (collect) exactly as described in `geo/SKILL.md`. Do not skip
   the AI-UA fetch test, the SSR ratio or the PageSpeed call — they feed
   measured checks. Record failures as errors, not as results.
2. Run Phase 2: launch the five agents in **one message** with the Agent tool.
3. Run Phase 3: merge into `geo-reports/<domain>/scores.json`, validate, then
   write the report below.

## Validation before writing the report

- Each dimension score ≤ 20 and equals the sum of its checks.
- `total` equals the sum of the five dimensions. If it does not, fix the
  numbers — never adjust the total to "feel right".
- Every measured check has an `evidence` string or `status: unverifiable`.
- No statistic appears in the report unless it is `verified` in
  `geo/references.md`.
- No € or traffic figure appears anywhere.
- Business-type detection changed recommendations only, not scores.

## Report template — `geo-reports/<domain>/GEO-AUDIT-<date>.md`

```markdown
# LLM-Readiness Audit — [Brand] ([domain])

**Prepared by:** Prompt-Ready · **Date:** [YYYY-MM-DD] · **Model version:** 3.0
**Pages analysed:** [n] · **Business type (for recommendations):** [type]

## Score

**LLM-Readiness Score: [total]/100 — [label]**

| Dimension | Score | Label | Measured | Assessed |
|---|---|---|---|---|
| A. Structure & Navigation | [x]/20 | [label] | [m]/14 | [a]/6 |
| B. Content Quality | [x]/20 | [label] | [m]/14 | [a]/6 |
| C. Technical SEO for LLMs | [x]/20 | [label] | [m]/14 | [a]/6 |
| D. AI-Specific Readiness | [x]/20 | [label] | [m]/14 | [a]/6 |
| E. Brand Entity & Authority | [x]/20 | [label] | [m]/14 | [a]/6 |
| **Total** | **[total]/100** | **[label]** | [m]/70 | [a]/30 |

Unverifiable checks this run: [list ids or "none"] — these scored 0 and are
flagged below; they are data-collection gaps, not confirmed failures.

## Live AI visibility test (snapshot [date])

Cited in **[x] of 9** answers · brand mentioned in **[y] of 9**.

| Question | ChatGPT | Claude | Perplexity |
|---|---|---|---|
| "[q1]" | cited / mentioned / absent | … | … |
| "[q2]" | … | … | … |
| "[q3]" | … | … | … |

Sources the engines relied on: [list]. AI answers vary between runs and users;
treat this as a signal, not a ranking. Raw answers in Appendix B.

## Core Web Vitals ([field|lab], PageSpeed Insights, mobile)

| Perf score | LCP | CLS | INP (field) | TBT (lab) |
|---|---|---|---|---|
| [x] | [x] s | [x] | [x ms or "no field data"] | [x ms] |

## Dimension findings

### A. Structure & Navigation — [x]/20

| # | Check | Status | Evidence | Pts |
|---|---|---|---|---|
| A1 | sitemap.xml present and valid | pass/fail/partial/na/unverifiable | [evidence] | [p]/2 |
| … | … | … | … | … |
| A9 | Internal-link quality (assessed) | — | [example + gap] | [p]/6 |

[3-4 sentences of interpretation. No restating the table.]

### B. Content Quality — [x]/20
[same structure]

### C. Technical SEO for LLMs — [x]/20
[same structure]

### D. AI-Specific Readiness — [x]/20
[same structure]

### E. Brand Entity & Authority — [x]/20
[same structure]

## Priority recommendations

Ordered by points recoverable ÷ effort. Each row ties to a check id.

| # | Action | Checks | Points recoverable | Effort | Owner |
|---|---|---|---|---|---|
| 1 | [specific action] | C1 | +4 | 1 h | dev |
| … | | | | | |

## Roadmap

**Phase 1 — Quick wins (1-2 weeks):** [checks that are config/markup changes]
**Phase 2 — Optimisation (2-6 weeks):** [content and page-structure work]
**Phase 3 — Authority (1-3 months):** [entity, directories, third-party proof]

## Appendix A — Pages analysed
| URL | Status | Words | H1 | JSON-LD | Notes |

## Appendix B — Live AI visibility test raw log
[question, engine, model/version, timestamp, answer excerpt, sources]

## Appendix C — Scoring model
[Paste §3 and §8 of geo/scoring.md]

## About this assessment
Measured checks come from fetched HTML, HTTP headers, robots.txt, sitemap,
PageSpeed Insights, Wikidata/Wikipedia APIs and live AI-engine answers at the
date above. Assessed checks (30 of 100 points) are model judgements against the
published rubric and may vary by up to ±3 points between runs. The scoring model
is Prompt-Ready's own; no industry standard for LLM readiness exists.
```

## Severity (for the recommendations table)

| Severity | Rule |
|---|---|
| Critical | A measured check worth ≥4 points failed (C1, D1, D2, E1) |
| High | Any other failed measured check worth ≥2 |
| Medium | Partial measured checks; assessed score ≤3 |
| Low | 1-point checks; polish |

## Business-type guidance (recommendations only)

| Type | Emphasise in recommendations |
|---|---|
| SaaS | SoftwareApplication schema, comparison/alternatives pages, docs structure |
| Local service | LocalBusiness schema, Google Business Profile, local directories |
| E-commerce | Product + Offer schema, buying guides, review markup |
| Publisher | Article + Person schema, author pages, dated hub, feed |
| Agency / services | Organization + Service schema, case studies with outcomes, directories the engines cited |
