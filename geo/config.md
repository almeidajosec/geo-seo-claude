# Prompt-Ready — Toolkit Configuration

Edit this file to brand and localise every output. Skills read these values;
do not hardcode brand, pricing or language anywhere else.

## Brand

| Key | Value |
|---|---|
| brand_name | Prompt-Ready |
| brand_url | https://www.prompt-ready.com/ |
| tagline | AI visibility starts with knowing where you stand. |
| report_name | LLM-Readiness Report |
| primary_colour | #4F46E5 |
| dark_colour | #111827 |
| accent_colour | #F97316 |
| paper_size | A4 |

Spelling: always **Prompt-Ready** (hyphenated, capital P and R). Never
"PromptReady", "Prompt Ready" or "PROMPTREADY".

## Language

| Key | Value |
|---|---|
| default_output_language | match the site's primary language (PT-PT or EN); otherwise EN |
| analyst_language | EN |

Trigger words for skills are English and Portuguese only.

## Pricing (used by geo-proposal)

Set these before sending any proposal. Leave as `TBD` and the proposal will
render "[PRICE TBD]" rather than inventing a number.

| Package | Monthly | Minimum term |
|---|---|---|
| Essentials | TBD | 6 months |
| Growth | TBD | 6 months |
| Authority | TBD | 12 months |

## Paths

| Key | Value |
|---|---|
| reports_dir | `geo-reports/` in the current working directory |
| clients_dir | `geo-reports/clients/<domain>/` (audit history for delta reports) |
| scripts_dir | `~/.claude/skills/geo/scripts/` |
