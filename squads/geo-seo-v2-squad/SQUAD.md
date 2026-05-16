---
name: geo-seo-v2-squad
version: 1.0.0
description: "Two-agent GEO squad: Atlas owns GEO strategy and blog engineering, Seren owns Reddit and HN presence"
author: pancake-official
tags: [seo, content, growth, reddit, community]
token_intensity: medium
agents:
  - id: atlas
    description: "GEO/SEO strategist — daily citation audits, blog posts, JSON-LD/llms.txt, draft PRs"
  - id: seren
    description: "Reddit and Hacker News agent — monitors threads, drafts comments, manages karma strategy"
---

## What this squad does

Deploys two focused agents that grow your product's AI-engine visibility from two angles:

**Atlas** runs the GEO strategy. Every day it audits whether your product is cited by ChatGPT, Gemini, and Perplexity for your target keywords. When citation share is weak, it drafts a GEO-optimized blog post and opens a PR. It also maintains your `llms.txt`, JSON-LD schema, and content metadata.

**Seren** owns Reddit and Hacker News. Every 2 hours it monitors your target subreddits for relevant threads, drafts comments that add real value to the conversation, and queues them for your review before posting. Seren manages a multi-account karma strategy using aged accounts via PRAW — never touch Reddit in a browser again.

## What you'll need

- A GitHub repo for your content/blog (optional but recommended for Atlas)
- Your top 5 target keywords
- 10-20 aged Reddit accounts from REDAccs (~$1-3 each) — Seren sets up the PRAW API apps automatically
- Your target subreddits (e.g. r/Entrepreneurs, r/startups, r/SaaS)

## What you get

- Daily citation audit posted to Slack (Atlas)
- Blog posts and GEO engineering fixes drafted as PRs (Atlas)
- Batched Reddit comment drafts ready for your review before posting (Seren)
- Weekly Reddit karma health report across all accounts (Seren)
- f5bot-style keyword monitoring for your brand and competitors on Reddit (Seren)

## How it works

Atlas runs once a day at 6 PM PT. Seren checks Reddit every 2 hours, batches draft comments, and presents them to the co-founder for sign-off before any account posts. Both agents file their work to the shared wiki and stay quiet when there's nothing new to report.
