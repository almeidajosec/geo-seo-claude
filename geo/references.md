# References — Market Statistics and Platform Facts

> **Rule:** a statistic may appear in a client report ONLY if it is listed here
> with a `Status: verified` line, a source URL and a verification date. Anything
> marked `unverified` MUST NOT be used in client-facing output. When you verify
> or refute an entry, update it here — do not edit the statistic in a report.

Statistics previously hardcoded across the toolkit were moved here on
2026-11-09. None of them has been verified against a primary source yet.

| Claim | Original attribution | Status | Source URL | Verified on |
|---|---|---|---|---|
| GEO services market $850M-$886M (2025), $7.3B by 2031 | "Yahoo Finance / Superlines" | unverified | — | — |
| AI-referred sessions +527% (Jan-May 2025) | "SparkToro" | unverified | — | — |
| AI traffic converts 4.4x vs organic | "Industry data" | unverified | — | — |
| Google AI Overviews reach 1.5B users/month | "Google" | unverified | — | — |
| ChatGPT 900M+ weekly active users | "OpenAI" | unverified | — | — |
| Perplexity 500M+ monthly queries | "Perplexity" | unverified | — | — |
| Gartner: -50% search traffic by 2028 | "Gartner" | unverified | — | — |
| Only 23% of marketers investing in GEO | "Industry surveys" | unverified | — | — |
| Brand mentions 3x stronger correlation than backlinks; YouTube ~0.737, DR ~0.266 | "Ahrefs, Dec 2025, 75K brands" | unverified | — | — |
| 47.9% of ChatGPT citations are Wikipedia | none given | unverified | — | — |
| 46.7% of Perplexity citations are Reddit | none given | unverified | — | — |
| Only 11% of domains cited by both ChatGPT and Google AIO | none given | unverified | — | — |
| 92% of AIO citations from top-10 organic; 47% from below position 5 | none given | unverified | — | — |
| GEO-optimised content +30-115% visibility | "Princeton / Georgia Tech / IIT Delhi 2024 (GEO paper)" | unverified | — | — |
| Optimal citation passage 134-167 words | "Bortolato 2025" | unverified | — | — |
| >35% of top-1000 sites block ≥1 AI crawler | "Originality.ai 2025" | unverified | — | — |
| <5% of websites have llms.txt | none given | unverified | — | — |
| 10-point GEO score gain ≈ 15-25% more AI citations | none given | **refuted — internal invention, never use** | — | 2026-11-09 |
| Industry-average GEO score 45/100; top performers 75+ | none given | **refuted — internal invention, never use** | — | 2026-11-09 |
| 12% of sites have llms.txt / 78% of top performers | none given | **refuted — internal invention, never use** | — | 2026-11-09 |

## Platform facts to re-verify before each audit cycle

These change often. Verify with a web search and record the date.

| Fact | Last verified | Notes |
|---|---|---|
| OpenAI crawler UAs: GPTBot, OAI-SearchBot, ChatGPT-User | — | https://platform.openai.com/docs/bots |
| Anthropic crawler UAs: ClaudeBot, Claude-User, Claude-SearchBot | — | https://support.anthropic.com (search "crawler") |
| Perplexity UAs: PerplexityBot, Perplexity-User | — | https://docs.perplexity.ai (search "bots") |
| Google-Extended controls Gemini training only, not Search/AIO | — | Google Search Central crawler docs |
| FAQPage rich results restricted (Aug 2023); HowTo removed (Sep 2023) | — | Google Search Central blog |
| INP replaced FID as CWV (Mar 2024) | — | web.dev |
| Google guidance on JS-injected structured data | — | Google Search Central |
