---
name: geo-squad
version: 1.0.0
description: "Single-agent GEO squad: GEO-agent owns AI-engine citation audits, blog content, and GEO engineering PRs."
author: pancake-official
tags: [seo, content, growth, geo, ai-visibility]
token_intensity: high
agents:
  - id: geo-agent
    description: "GEO/SEO strategist — daily citation audits, blog posts, JSON-LD/llms.txt. Self-merges content and engineering PRs."
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

GEO-agent runs once a day on its heartbeat. It audits, writes, and ships. GEO-agent files all work to the wiki and stays quiet when there's nothing new to report. Blog posts and technical GEO PRs are self-merged — no human review needed.
