# Prompt-Ready LLM-Readiness Scoring Model (v3.0)

> **Single source of truth.** Every skill, agent, report template, proposal and
> delta report in this toolkit MUST use the dimensions, point values and labels
> defined here. Do not define weights or thresholds anywhere else. If you need to
> change the model, change this file and bump the version.

This model is shared with the automated Prompt-Ready LLM-Readiness Report, so a
site scored by either tool can be tracked on the same 0-100 scale over time.

---

## 1. Dimensions

Five dimensions, 20 points each, total 100.

| Code | Dimension | Points |
|---|---|---|
| A | Structure & Navigation | 20 |
| B | Content Quality | 20 |
| C | Technical SEO for LLMs | 20 |
| D | AI-Specific Readiness | 20 |
| E | Brand Entity & Authority | 20 |

**Total LLM-Readiness Score = A + B + C + D + E** (integer, 0-100). No weighting,
no rounding tricks. The total in the report MUST equal the sum of the five
dimension scores shown in the same report.

## 2. Measured vs Assessed points

Each dimension is split into:

- **Measured points (14/20)** — binary or threshold checks on fetched data
  (raw HTML, HTTP headers, robots.txt, sitemap, PageSpeed/CrUX API, Wikidata API,
  live AI-engine answers). Identical across runs and across tools. A measured
  check is scored only from evidence in `audit-data.json`; if the evidence is
  missing, the check scores **0 and is reported as "Unverifiable"**, never
  estimated.
- **Assessed points (6/20)** — model judgement on a 0-6 scale, following the
  rubric below. This is the only part of the score that may vary between runs.
  Maximum expected run-to-run variance of the total: ±3.

Every line in a report is labelled **Measured**, **Assessed** or **Unverifiable**.

## 3. Rubric

### A. Structure & Navigation (20)

| # | Check | Type | Points | How to score |
|---|---|---|---|---|
| A1 | `sitemap.xml` (or index) present and valid XML | Measured | 2 | 2 if 200 + parses; 0 otherwise |
| A2 | `<lastmod>` present on sitemap entries | Measured | 1 | 1 if ≥50% of entries have lastmod |
| A3 | `robots.txt` present with `Sitemap:` directive | Measured | 2 | 2 both; 1 robots only; 0 none |
| A4 | Exactly one H1 on key pages | Measured | 2 | 2 if all sampled pages; 1 if most; 0 |
| A5 | No skipped heading levels (H1→H2→H3) | Measured | 1 | 1 if clean on all sampled pages |
| A6 | Addressable URLs — site is not anchor-only / single-page | Measured | 3 | 3 if ≥5 indexable URLs discovered; 1 if 2-4; 0 if 1 |
| A7 | Key pages within 3 clicks of homepage | Measured | 2 | 2 if all key pages depth ≤3; 1 if most; 0 |
| A8 | Clean URL pattern (lowercase, hyphens, no session params) | Measured | 1 | 1 if clean on sampled URLs |
| A9 | Internal-link quality and topic clustering | Assessed | 6 | 0-6 per §4 rubric |

### B. Content Quality (20)

| # | Check | Type | Points | How to score |
|---|---|---|---|---|
| B1 | Word count above floor for page type (see §5) | Measured | 3 | 3 if all sampled pages ≥ floor; 2 if ≥75%; 1 if ≥50%; 0 |
| B2 | Visible publication / modified dates on content pages | Measured | 2 | 2 both dates; 1 one date; 0 none (N/A→score on remaining checks, see §6) |
| B3 | Q&A / FAQ block present (question headings + direct answers) | Measured | 2 | 2 if ≥5 Q&A pairs; 1 if 1-4; 0 |
| B4 | Fact density: ≥3 specific figures/dates/named entities per 500 words | Measured | 3 | 3 if ≥3; 2 if 2; 1 if 1; 0 |
| B5 | About page with verifiable credentials (names, years, qualifications) | Measured | 2 | 2 full; 1 About exists but no credentials; 0 |
| B6 | Dated content hub or RSS/Atom feed | Measured | 2 | 2 if feed or ≥3 dated articles; 0 |
| B7 | Answer-first writing, originality, readability | Assessed | 6 | 0-6 per §4 rubric |

### C. Technical SEO for LLMs (20)

| # | Check | Type | Points | How to score |
|---|---|---|---|---|
| C1 | No `noindex` (meta or `X-Robots-Tag`) on key pages | Measured | 4 | 4 if none; 0 if any key page is noindexed |
| C2 | Self-referencing canonical on key pages | Measured | 2 | 2 all; 1 most; 0 |
| C3 | JSON-LD present in raw HTML and parses as valid JSON | Measured | 3 | 3 valid; 1 present but invalid / JS-injected only; 0 none |
| C4 | Open Graph complete (`og:title`, `og:description`, `og:image`, `og:url`, `og:type`) + Twitter Card | Measured | 2 | 2 all; 1 partial; 0 |
| C5 | `hreflang` correct, or N/A for single-language site | Measured | 1 | 1 if correct or N/A; 0 if multilingual without hreflang |
| C6 | Core Web Vitals within thresholds (field CrUX preferred; lab PSI fallback, labelled as lab) | Measured | 2 | 2 if LCP<2.5s & CLS<0.1 & (INP<200ms or TBT<300ms lab); 1 if 2 of 3; 0 |
| C7 | Overall technical health (security headers, redirects, page weight, mobile) | Assessed | 6 | 0-6 per §4 rubric |

### D. AI-Specific Readiness (20)

| # | Check | Type | Points | How to score |
|---|---|---|---|---|
| D1 | Server-rendered content ratio (raw HTML text ÷ rendered text) | Measured | 4 | 4 if ≥70%; 2 if 40-69%; 0 if <40% |
| D2 | Tier-1 AI crawlers not blocked in robots.txt / meta (GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-SearchBot, PerplexityBot, Googlebot, Bingbot) | Measured | 4 | 4 none blocked; 2 one blocked; 0 two or more |
| D3 | AI crawler user-agents receive HTTP 200 with real content (live fetch with each UA) | Measured | 2 | 2 all 200; 1 some 403/5xx; 0 all blocked |
| D4 | `llms.txt` present at root | Measured | 1 | 1 present; 0 absent (deliberately low weight — benefit unconfirmed by engines) |
| D5 | Live AI visibility test: site cited or brand mentioned in ≥1 of N engine answers (§7) | Measured | 3 | 3 if cited by ≥2 engines; 2 if 1; 1 if mentioned but not cited; 0 |
| D6 | Citability of key passages (self-contained, extractable statements) | Assessed | 6 | 0-6 per §4 rubric |

### E. Brand Entity & Authority (20)

| # | Check | Type | Points | How to score |
|---|---|---|---|---|
| E1 | Organization/Person (or LocalBusiness) JSON-LD with `sameAs` ≥2 valid URLs | Measured | 4 | 4 if ≥2 sameAs; 2 entity without sameAs; 0 none |
| E2 | Wikidata item and/or Wikipedia article (API check, en + pt) | Measured | 3 | 3 Wikipedia article; 2 Wikidata only; 0 none |
| E3 | LinkedIn company page (not only a personal profile) | Measured | 2 | 2 company page; 1 personal profile only; 0 |
| E4 | Contact page + legal pages (privacy, terms) reachable | Measured | 2 | 2 all; 1 some; 0 |
| E5 | Listed on ≥2 of the third-party sources the AI engines cited for the category (§7) | Measured | 3 | 3 if ≥2; 1 if 1; 0 |
| E6 | Third-party corroboration and trust signals (reviews, press, testimonials, client proof) | Assessed | 6 | 0-6 per §4 rubric |

## 4. Assessed-points rubric (0-6, used for A9, B7, C7, D6, E6)

| Points | Meaning |
|---|---|
| 6 | Strong — clear, consistent evidence across sampled pages; nothing material missing |
| 4-5 | Good — mostly present; one notable gap |
| 2-3 | Weak — isolated signals only; several gaps |
| 0-1 | Absent — no meaningful evidence |

The report MUST quote at least one concrete example (URL + snippet) justifying
any assessed score of 4 or more, and name the missing element for any score of
3 or less.

## 5. Word-count floors (for B1)

| Page type | Floor |
|---|---|
| Homepage | 300 |
| Service / product page | 300 |
| Blog / article | 800 |
| About page | 200 |
| FAQ page | 400 |

## 6. Not-applicable checks

If a check does not apply (e.g. B2 on a site with no content pages, C5 on a
single-language site), mark it **N/A** and award its points **proportionally
from the remaining measured checks in that dimension** so the dimension still
totals 20. State the N/A in the report.

## 7. Live AI visibility test protocol (feeds D5 and E5)

1. Formulate **3 unbranded category questions** a buyer would ask, in the site's
   primary language (e.g. "Agências de vídeo com IA em Portugal").
2. Ask each question to **ChatGPT, Claude and Perplexity** with live web search
   (9 answers).
3. Record per answer: engine, model/version if shown, timestamp, whether the
   brand is **mentioned**, whether the site is **cited** (URL in sources), and
   the list of **sources the engine relied on**.
4. Report the result as "cited in X of 9 answers · mentioned in Y of 9".
5. Store the raw questions and answers in the report appendix. This is a
   snapshot, not a ranking — say so.

## 8. Score labels

| Total | Label |
|---|---|
| 80-100 | Strong |
| 60-79 | Good |
| 40-59 | Developing |
| 20-39 | Weak |
| 0-19 | Critical |

Per-dimension labels use the same bands scaled to 20: 16-20 Strong, 12-15 Good,
8-11 Developing, 4-7 Weak, 0-3 Critical.

## 9. Mapping from legacy categories (v2 six-category model)

Used by `geo-compare` when the baseline audit predates v3. Do not use for new audits.

| Legacy category | Maps to |
|---|---|
| AI Citability & Visibility | D (and B7) |
| Brand Authority Signals | E |
| Content Quality & E-E-A-T | B |
| Technical Foundations | A + C |
| Structured Data | C3, E1 |
| Platform Optimization | D5, E5 |

Legacy totals are **not comparable** to v3 totals. When a baseline is v2, the
delta report must say "re-baselined on v3" and show the v3 score as the new
baseline rather than computing a delta.

## 10. Evidence and fact rules (apply to every report)

1. Every finding is labelled **Measured**, **Assessed** or **Unverifiable**.
2. A measured check is never scored from inference ("no rich results in Google
   → no schema"). It is scored from the fetched artifact or marked Unverifiable.
3. No market statistics in client reports. Any figure must come from
   `geo/references.md` with a source and date, or be omitted.
4. No € / traffic / ROI projections unless computed from the client's own
   GA4 / Search Console data, with the method stated.
5. Before asserting a crawler user-agent, a platform policy or a schema
   deprecation, verify with a web search and date the statement.
6. The rubric table (this file, §3) is included as an appendix in every full
   report so every score is explainable and every delta is auditable.
