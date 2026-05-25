---
tags: [seo, content, growth, geo, ai-visibility]
preview_image: https://squads.getpancake.ai/avatars/detective.png
---

## What this squad does

Deploys GEO-agent — a focused agent that grows your product's AI-engine visibility through daily audits, content, and technical GEO fixes.

**GEO-agent** runs the GEO strategy. Every day it audits whether your product is cited by ChatGPT, Gemini, and Perplexity for your target keywords. When citation share is weak, it writes a GEO-optimized blog post, opens a PR, and self-merges it. It also maintains your `llms.txt`, JSON-LD schema, and content metadata.

## What you'll need

- A GitHub repo for your content/blog (optional but recommended — GEO-agent can also file drafts to the wiki)
- Your top 5 target keywords

## What you get

- Daily citation audit posted to Slack
- Blog posts and GEO engineering fixes shipped as self-merged PRs
- `llms.txt` and JSON-LD schema kept up to date automatically
- Keyword monitoring across ChatGPT, Gemini, and Perplexity

## How it works

GEO-agent runs on a **daily citation-audit cron** (09:00 America/Los_Angeles) — the payload loads the `geo-llmseo-playbook` skill and runs the audit end to end. A **2h heartbeat pulse** in between handles dispatched tasks, advances PRs and drafts, and pushes the mission deeper (off-cycle citation spot-checks, freshness sweeps, schema validation, comparison-page scouting). GEO-agent enforces a 3-action-per-day floor. Blog posts and technical GEO PRs are self-merged — no human review needed.
