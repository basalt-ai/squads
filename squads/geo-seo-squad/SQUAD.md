---
name: geo-seo-squad
version: 1.0.0
description: "GEO/LLM SEO squad — keeps your product visible in AI engine citations"
author: pancake-official
tags: [seo, content, growth]
token_intensity: medium
agents:
  - id: atlas
    description: "GEO/SEO specialist — audits citations, writes blog posts"
preview_image: https://squads.getpancake.ai/squads/geo-seo-squad/preview.png
---

## What this squad does

Deploys **Atlas**, a daily GEO/LLM SEO agent that audits AI engine citations (ChatGPT,
Gemini, Perplexity) and writes optimized blog posts to improve your product's citation
share. GEO — Generative Engine Optimization — is SEO for the era where buyers ask an AI
assistant instead of a search box. Atlas keeps your product in the answer.

## What you'll need

- A blog or content hub (optional but recommended)
- Your top 5 target keywords
- GitHub access for content publishing (optional)

## What you get

- Daily citation audit → posted to your Slack channel
- Blog posts drafted and ready to publish
- `llms.txt`, JSON-LD schema, and comparison pages maintained automatically

## How it works

Atlas runs once a day. Each run it queries the major AI engines for your target keywords,
records whether your product was cited, files the findings to the shared wiki, and posts a
short delta to Slack — what moved, what slipped, what to do about it. When citation share
on a keyword is weak, Atlas drafts a GEO-optimized blog post to close the gap and (if you
connected GitHub) opens it as a draft for your review.

You can also hand Atlas a one-off post on demand — just ask the co-founder to have Atlas
write about a topic.
