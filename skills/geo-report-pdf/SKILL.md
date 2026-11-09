---
name: geo-report-pdf
description: Render the Prompt-Ready LLM-Readiness Report as an A4 PDF from scores.json using ReportLab (five dimensions × 20, score gauge, dimension chart, crawler table, findings, roadmap, rubric appendix).
version: 3.0.0
tags: [geo, pdf, report, client-deliverable, prompt-ready]
---

# Prompt-Ready PDF Report (v3.0)

## Prerequisites

- `pip install reportlab`
- Script: `~/.claude/skills/geo/scripts/generate_pdf_report.py` (A4, Prompt-Ready palette)
- A completed audit: `geo-reports/<domain>/scores.json` **and** the client report
  `GEO-CLIENT-REPORT-<date>.md` (for the executive summary and findings text).

Never build the PDF from a markdown report alone — scores come from
`scores.json`, text comes from the client report.

## Input JSON for the script

```json
{
  "url": "https://example.com",
  "brand_name": "Example",
  "date": "2026-11-09",
  "geo_score": 46,
  "scores": {
    "structure": 9,
    "content": 12,
    "technical": 11,
    "ai_readiness": 8,
    "brand": 6
  },
  "executive_summary": "4-6 sentences from the client report. Facts only.",
  "findings": [
    {"severity": "critical|high|medium|low", "title": "C1 — noindex on homepage",
     "description": "Measured: <meta name=\"robots\" content=\"noindex\"> present in raw HTML of /."}
  ],
  "quick_wins": ["Remove noindex (C1, +4 pts)"],
  "medium_term": ["…"],
  "strategic": ["…"],
  "crawler_access": {
    "GPTBot": {"platform": "ChatGPT", "status": "Allowed|Blocked|403", "recommendation": "…"}
  }
}
```

`geo_score` must equal the sum of the five `scores`; the script will warn and
use the sum if it does not. `scores.*` are 0-20. Do not pass `platforms` (legacy
v2 field); the live visibility test lives in the client report, not the PDF
chart.

## Steps

1. Build the JSON above from `scores.json` + the client report.
   Each finding title starts with its check id (e.g. "E1 — …") and each
   description starts with `Measured:` / `Assessed:` / `Unverifiable:`.
2. Save to `geo-reports/<domain>/pdf-data.json`.
3. Run:
   ```bash
   python3 ~/.claude/skills/geo/scripts/generate_pdf_report.py \
     geo-reports/<domain>/pdf-data.json \
     "geo-reports/<domain>/LLM-Readiness-<domain>-<date>.pdf"
   ```
4. Report the path and size. Open the PDF (Read tool) and check: total equals
   the dimension sum, labels match `scoring.md` §8, no legacy six-category table.

## Notes

- Paper: A4. Palette: dark `#111827`, indigo `#4F46E5`, orange `#F97316`.
- Score colours in the gauge and tables use the v3 bands (80/60/40/20).
- Statistics in `executive_summary` must be verified in `geo/references.md`.
