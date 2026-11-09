<p align="center">
  <img src="assets/banner.svg" alt="Prompt-Ready GEO Toolkit" width="900"/>
</p>

<p align="center">
  <strong>Prompt-Ready GEO Toolkit</strong> — LLM-Readiness audits for Claude Code.<br/>
  Five dimensions, 100 points, evidence-labelled, reproducible month to month.
</p>

---

## What it does

Audits how visible and citable a website is to AI search engines (ChatGPT,
Claude, Perplexity, Google AI Overviews, Gemini, Copilot) and produces a
client-ready **LLM-Readiness Report** using the same scoring model as the
automated Prompt-Ready report, so any site can be tracked on one scale.

| Dimension | Points |
|---|---|
| A. Structure & Navigation | 20 |
| B. Content Quality | 20 |
| C. Technical SEO for LLMs | 20 |
| D. AI-Specific Readiness | 20 |
| E. Brand Entity & Authority | 20 |

Each dimension = **14 measured points** (deterministic checks on fetched data:
HTML, headers, robots, sitemap, PageSpeed, Wikidata, live AI answers) +
**6 assessed points** (model judgement against a published rubric). The rubric
is in [`geo/scoring.md`](geo/scoring.md) and is appended to every report.

## Principles

- **One scoring model.** `geo/scoring.md` is the only place weights, checks and
  labels are defined. Total always equals the sum of the five dimensions.
- **Measured before assessed.** A collector (`scripts/fetch_page.py` + PageSpeed,
  Wikidata, Playwright, live engine queries) produces `audit-data.json`; agents
  score that file. Nothing is scored from inference.
- **Every line labelled** Measured / Assessed / Unverifiable.
- **No invented numbers.** Market statistics only from `geo/references.md`
  when verified; no € or traffic projections without the client's own data.

## Install

```bash
git clone <this repo> && cd geo-projects
./install.sh          # copies geo/, skills/, agents/, scripts/, schema/ to ~/.claude
pip install -r requirements.txt
```

Edit `geo/config.md` (brand, language, pricing) before producing proposals.

## Commands

| Command | Output |
|---|---|
| `/geo audit <url>` | `geo-reports/<domain>/GEO-AUDIT-<date>.md`, `audit-data.json`, `scores.json` |
| `/geo quick <url>` | Measured checks only (out of 70), inline |
| `/geo visibility <url>` | Live AI visibility test: 3 questions × ChatGPT, Claude, Perplexity |
| `/geo citability` · `crawlers` · `llmstxt` · `brands` · `schema` · `technical` · `content` | Single-dimension deep dives |
| `/geo report <domain>` | Client report (business language) from `scores.json` |
| `/geo report-pdf <domain>` | A4 PDF, Prompt-Ready palette |
| `/geo proposal <domain>` | Proposal; prices from `geo/config.md` |
| `/geo compare <domain>` | Check-by-check progress report between two audits |

## Layout

```
geo/
  SKILL.md        orchestrator: pipeline, commands, scores.json format
  scoring.md      THE scoring model (v3.0)
  config.md       brand, language, pricing, paths
  references.md   statistics register (verified / unverified / refuted)
agents/           five thin dimension agents (A-E) — input audit-data.json, output JSON
skills/           method and recommendation guidance per topic + report templates
scripts/          fetch_page.py (collector), citability_scorer.py, brand_scanner.py,
                  generate_pdf_report.py (A4, v3)
schema/           JSON-LD templates
docs/             prompt-ready-engine-spec.md — change spec for the automated report
_legacy/          v2 six-category agents/skills, kept for reference, not installed
```

## Monitoring

Every run writes `geo-reports/<domain>/scores.json`. `/geo compare` diffs two
of them check by check. Audits made with the v2 model are not comparable; the
first v3 run becomes the new baseline.

## Licence

See `LICENSE`. Originally derived from an open-source GEO toolkit; scoring model,
agents, report templates and engine spec rewritten for Prompt-Ready (v3.0, Nov 2026).
