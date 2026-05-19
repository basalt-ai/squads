---
name: geo-llmseo-playbook
description: The geo-seo-squad v2 shared playbook — how to run a daily GEO/LLM-SEO citation audit, score the result, and decide what content to draft. Used by every agent in the squad.
---

# GEO / LLM-SEO playbook

GEO (Generative Engine Optimization) is making a product show up — and show up
*accurately* — when a buyer asks an AI assistant instead of typing into a search box. This
playbook is the squad's shared method. The daily citation audit runs it end to end.

## The daily citation audit

Run once per day. Target domain and keywords are in `MEMORY.md` (`## Target`,
`## Keywords`) and the vault (`team.target_domain`, `team.target_keywords`).

1. **Query each engine.** For every target keyword, pose a realistic buyer question to
   ChatGPT, Gemini, and Perplexity (use `web_search` / `web_fetch`; for engines behind a
   login use `browser_task` on a connected identity). Phrase it the way a buyer would —
   "what's the best X for Y" — not as a keyword string.
2. **Record the citation.** For each (engine × keyword) capture: was the product mentioned?
   cited with a link? ranked above competitors? Which competitors appeared instead?
3. **Score citation share.** Per keyword: `cited_engines / total_engines`. Per squad: the
   mean across keywords. Track the delta vs. yesterday — the *movement* is the signal.
4. **File the audit.** Write the full table to
   `wiki/Knowledge/GEO/Audits/YYYY-MM-DD.md`. Update the rolling trend in
   `wiki/Knowledge/GEO/citation-share.md`.
5. **Post the delta to Slack.** Three lines max: overall share + change, the keyword that
   moved most, and the one recommended action. Never post the full table to Slack — link
   the wiki page. If nothing changed, say so in one line; do not pad.

## Deciding what to draft

A keyword with citation share below 0.5 (cited by fewer than half the engines) is a gap.
Pick the single worst gap and draft to close it:

- **No content exists** for the buyer question → draft a blog post answering it directly.
- **Content exists but isn't cited** → the page is not extractable. Rewrite the opening to
  state the answer in the first paragraph; add an FAQ block; add JSON-LD `FAQPage` schema.
- **A competitor owns the citation** → draft an honest comparison page. Never disparage —
  AI engines drop sources that read as biased.

If GitHub is connected, open a PR and self-merge (squash merge). If not, file to `wiki/Knowledge/GEO/Drafts/`.

## What makes content citable by AI engines

- **Answer first.** The first 1–2 sentences must directly answer the question. Engines
  extract the lede.
- **Structured.** Clear H2/H3 headings phrased as questions. Lists and tables over prose.
- **Sourced.** Concrete numbers, dates, named sources. Unsourced claims get dropped.
- **Fresh.** A visible "last updated" date. Stale pages lose citations.
- **Machine-readable.** `llms.txt` at the domain root, JSON-LD schema on every page.

## Task system

All work runs through the task system — `create_task`, `complete_task`, `fail_task`. Never
track work in STATE.md or comments.

- On every audit: identify the 3 highest-value tasks and `create_task` for each
  (`assigned_to: geo-agent`, priority: `urgent` / `today` / `later` based on citation gap severity).
- Pick the top task and execute it immediately in the same session — don't queue and sleep.
- `complete_task` with the deliverable (PR link, wiki draft path, etc.).
- Before going idle: confirm a next task exists in the queue.

## Daily digest (post to Slack after every audit run)

Three sections, tight:
1. *Citation delta* — overall share + change, biggest mover.
2. *Shipped today* — what GEO-agent executed (PR opened, draft filed, fix deployed).
3. *Tomorrow's plan* — top task queued for next run.

If nothing moved and nothing shipped: one line saying so. Never pad.

## Cadence

The daily audit runs on GEO-agent's daily heartbeat. One-off posts arrive as dispatched tasks.
Default is to execute — don't ask the co-founder for permission to draft content or open
a PR. Ship it, report back via `complete_task`.
