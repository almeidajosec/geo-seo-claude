---
name: geo
description: >
  Prompt-Ready LLM-Readiness / GEO analysis toolkit. Audits how visible and
  citable a website is to AI search engines (ChatGPT, Claude, Perplexity, Google
  AI Overviews, Gemini, Copilot) using a fixed five-dimension, 100-point scoring
  model shared with the Prompt-Ready LLM-Readiness Report. Use when the user
  says "geo", "llm readiness", "ai visibility", "ai search", "audit", "auditoria",
  "citability", "llms.txt", "schema", "brand mentions", "prompt-ready report",
  or gives a URL to analyse.
allowed-tools: Read, Grep, Glob, Bash, WebFetch, WebSearch, Write, Agent
---

# Prompt-Ready GEO Toolkit — Orchestrator (v3.0)

Read first, every time:
- `geo/scoring.md` — the ONLY scoring model. Five dimensions × 20 points.
- `geo/config.md` — brand, language, pricing, paths.
- `geo/references.md` — the only statistics allowed in client output.

## Commands

| Command | What it does | Output |
|---|---|---|
| `/geo audit <url>` | Full audit: collect → 5 parallel agents → merged report | `geo-reports/<domain>/GEO-AUDIT-<date>.md` + `audit-data.json` + `scores.json` |
| `/geo quick <url>` | Measured checks only (no agents, no assessed points), ~2 min | inline summary + `scores.json` |
| `/geo visibility <url>` | Live AI visibility test only (§7 of scoring.md) | `geo-reports/<domain>/VISIBILITY-<date>.md` |
| `/geo citability <url>` | Dimension D6 + B7 deep dive on one page | `GEO-CITABILITY-<date>.md` |
| `/geo crawlers <url>` | Dimension D2/D3 deep dive, robots.txt recommendation | `GEO-CRAWLERS-<date>.md` |
| `/geo llmstxt <url>` | Validate or generate llms.txt | `llms.txt` + notes |
| `/geo brands <url>` | Dimension E deep dive | `GEO-BRAND-<date>.md` |
| `/geo schema <url>` | Dimension C3/E1 deep dive + JSON-LD generation | `GEO-SCHEMA-<date>.md` |
| `/geo technical <url>` | Dimensions A + C deep dive | `GEO-TECHNICAL-<date>.md` |
| `/geo content <url>` | Dimension B deep dive | `GEO-CONTENT-<date>.md` |
| `/geo report <domain>` | Client-facing report from the latest `scores.json` | `GEO-CLIENT-REPORT-<date>.md` |
| `/geo report-pdf <domain>` | PDF of the client report | `LLM-Readiness-<domain>-<date>.pdf` |
| `/geo proposal <domain>` | Proposal from latest scores (prices from config.md) | `PROPOSAL-<date>.md` |
| `/geo compare <domain>` | Delta between two `scores.json` files | `PROGRESS-<date>.md` |

`/geo platforms` was removed: platform readiness is now covered by D5/E5 (live
test) and the platform-optimizer skill is used as guidance inside the content and
brand agents, not scored separately. `/geo prospect` was removed — use ClickUp.

All outputs go to `geo-reports/<domain>/` relative to the current working
directory (create with `mkdir -p`). Keep every run's `scores.json` so
`/geo compare` has history.

---

## Pipeline for `/geo audit`

### Phase 1 — Collect (deterministic, no model judgement)

Run the collector and save its output. Agents score **this file**, they do not
re-fetch pages with WebFetch (WebFetch returns a summarised page, not raw HTML
or headers, and cannot support measured checks).

```bash
mkdir -p geo-reports/<domain>
python3 ~/.claude/skills/geo/scripts/fetch_page.py <url> full > geo-reports/<domain>/audit-data.json
```

`fetch_page.py full` returns: homepage raw HTML analysis (status, redirect
chain, headers incl. security and X-Robots-Tag, meta, canonical, headings,
word count, text, links, images, JSON-LD blocks, SSR flag), robots.txt parsed
per AI crawler, llms.txt status, sitemap pages (max 50).

Then extend `audit-data.json` with:

1. **Key-page sample** — homepage + up to 6 key pages (about, contact, top
   service/product pages, 2 most recent articles). Run `fetch_page.py <page> page`
   for each and append under `pages[]`.
2. **AI-UA fetch test (D3)** — fetch the homepage once per Tier-1 crawler UA
   (list in `fetch_page.py AI_CRAWLERS`) and record status code + body length.
3. **Server-rendered ratio (D1)** — raw text length from `fetch_page.py` ÷
   rendered text length from Playwright (`python3 -c` with `playwright.sync_api`,
   `page.inner_text('body')`). Record both numbers.
4. **Core Web Vitals (C6)** — PageSpeed Insights API
   `https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=<url>&strategy=mobile`.
   Record CrUX field metrics if present (label **field**); otherwise lab LCP,
   CLS, TBT (label **lab**). Never label TBT as INP.
5. **Entity check (E2)** — Wikipedia search API on `en.wikipedia.org` AND
   `pt.wikipedia.org`, plus Wikidata `wbsearchentities`. Use the brand name as
   shown on the site. Record URL/Q-id or "not found".
6. **Live AI visibility test (D5, E5)** — follow §7 of `scoring.md`: 3 unbranded
   category questions × ChatGPT, Claude, Perplexity (use the browser tools or
   WebFetch on each engine's web UI/API as available). Record mentions, citations
   and the sources each engine relied on. If an engine cannot be queried, record
   "not run" — do not infer.

If any collection step fails, record the failure in `audit-data.json`
(`"errors": [...]`). The corresponding checks are reported **Unverifiable**.

### Phase 2 — Analyse (5 agents in parallel)

Launch all five with the **Agent tool in one message**, passing the path to
`audit-data.json`. Each agent returns a JSON block in the `scores.json` format
below for its dimension only.

| Agent file | Dimension | Loads skill(s) |
|---|---|---|
| `agents/geo-structure.md` | A — Structure & Navigation | geo-technical (crawlability), geo-citability (structure) |
| `agents/geo-content.md` | B — Content Quality | geo-content, geo-citability |
| `agents/geo-technical.md` | C — Technical SEO for LLMs | geo-technical, geo-schema |
| `agents/geo-ai-readiness.md` | D — AI-Specific Readiness | geo-crawlers, geo-llmstxt, geo-citability, geo-platform-optimizer |
| `agents/geo-brand.md` | E — Brand Entity & Authority | geo-brand-mentions, geo-schema, geo-content (trust) |

### Phase 3 — Merge

1. Concatenate the five dimension blocks into `scores.json`. Total = sum.
2. Validate: each dimension ≤ 20, each check's points ≤ its max in
   `scoring.md`, every measured check has an `evidence` string or is
   `"status": "unverifiable"`.
3. Write `GEO-AUDIT-<date>.md` using the `geo-audit` skill template.
4. Tell the user the total, the weakest dimension, and the top 3 measured
   failures. Do not restate the whole report.

## `scores.json` format

```json
{
  "model_version": "3.0",
  "domain": "example.com",
  "date": "2026-11-09",
  "total": 0,
  "label": "Weak",
  "dimensions": {
    "A": {
      "name": "Structure & Navigation",
      "score": 0,
      "checks": [
        {"id": "A1", "type": "measured", "points": 2, "max": 2,
         "status": "pass|fail|partial|na|unverifiable",
         "evidence": "sitemap.xml 200, 14 URLs, 14 lastmod"},
        {"id": "A9", "type": "assessed", "points": 3, "max": 6,
         "evidence": "…example URL + snippet…", "missing": "…"}
      ]
    },
    "B": {}, "C": {}, "D": {}, "E": {}
  },
  "visibility_test": {"questions": [], "answers": [], "cited": 0, "mentioned": 0, "of": 9, "sources": []},
  "cwv": {"source": "field|lab", "lcp": null, "cls": null, "inp": null, "tbt": null, "perf_score": null},
  "errors": []
}
```

## `/geo quick`

Phase 1 steps 1-5 only (skip the live visibility test unless asked), then
score every **measured** check from the data. Report measured points out of 70
and state that assessed points (30) and D5/E5 are not included. Takes ~2
minutes. Never present a quick score as a full LLM-Readiness Score.

## Fact rules (non-negotiable)

Apply §10 of `scoring.md`. In short: label every line Measured / Assessed /
Unverifiable; never score from inference; no market statistics unless verified
in `references.md`; no € or traffic projections without the client's own data;
verify crawler names and platform policies before stating them; append the
rubric to every full report.

## Business-type adjustments

Detect SaaS / local / e-commerce / publisher / agency from the homepage as
before, but it changes **recommendations only** (which schema type, which
directories, which page types) — it never changes the scoring model.

## Quality gates

Max 50 sitemap URLs, 7 fetched key pages, 30 s timeout per fetch, 1 s between
fetches, respect robots.txt, HTML only.
