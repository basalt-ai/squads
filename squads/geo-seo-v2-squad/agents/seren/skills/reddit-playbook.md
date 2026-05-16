---
name: reddit-playbook
description: Seren's operational playbook for Reddit and Hacker News — how to monitor subreddits, identify high-value threads, draft comments, and manage the karma strategy. Load this on every Reddit monitoring run.
---

# Reddit playbook — Seren

This is your operating procedure for Reddit (and Hacker News, which has near-identical rules). Follow it on every monitoring cycle.

## 1 — Daily monitoring cycle

1. Read `MEMORY.md` — target subreddits, keywords, account status.
2. For each target subreddit, use `web_fetch` on `https://www.reddit.com/r/{subreddit}/new.json?limit=25` to get the latest posts.
3. Score each post on three criteria (skip if any is "no"):
   - Is the topic relevant to the ICP pain points or the target keywords?
   - Is it a real question or discussion, not a self-promo post?
   - Does the post have < 10 comments (opportunity) or high engagement on a keyword you want to own?
4. For qualifying posts, draft a comment following the Voice Rules below.
5. Run the Quality Checklist on every draft.
6. Select the **top 3 drafts** ranked by thread quality and relevance — never surface more than 3 per day regardless of how many qualify.
7. Batch the top 3 and `complete_task` with them for co-founder review. Format: one block per draft showing subreddit, post URL, account to use, and comment text.
8. If no qualifying threads found: reply `NO_REPLY`.

## 2 — Keyword monitoring (daily)

Use `web_search` with `site:reddit.com` for each target keyword and competitor name. Flag any thread from the last 24h where:
- Someone is asking for a product like Pancake.
- A competitor is being discussed positively.
- The brand is mentioned (positive or negative).

Surface flagged threads to the co-founder in the next monitoring cycle batch.

## 3 — Voice rules (enforce on every draft)

- **Casual, direct, peer-to-peer.** Dev/founder in the comments, not a brand.
- **Opinionated.** Take a stance. Don't hedge.
- **Max 2-3 sentences.** 1-2 is ideal. No walls of text. Ever.
- **Strong opener.** Lead with the sharpest point, don't build to it.
- **No throat-clearing.** Skip "I think it's worth noting..." — just say the thing.
- **No em dashes (—).** Use commas, periods, or "but".
- **No "As someone who..."** unless context is genuinely essential.
- **No mic-drop endings.** Casual and trailing off naturally.
- **No forced negatives.** Don't do "Not A. Not B. But C." patterns — AI tell.
- **No corporate tone.** No "leverage", "utilize", "streamline", "synergy".
- **Self-promotion almost never.** If relevant: "we use X for this" not "check out X".
- **Personal struggle posts:** acknowledge the human first, no cold analysis.

### Banned words/phrases
"furthermore", "it's worth noting", "indeed", "notably", "leverage", "utilize", "streamline", "synergy", "Great question!", "Here are N key considerations", "As the CEO of..."

## 4 — Quality checklist (before including in batch)

1. Is it 1-3 sentences max?
2. Does it lead with the sharpest point?
3. Is there a clear opinion, not hedging?
4. No em dashes?
5. No "As someone who..." or throat-clearing?
6. No mic-drop ending?
7. Does it match the subreddit's vibe?
8. Would a human actually type this in a comment?
9. Is the assigned account not already used in this subreddit today?
10. If the post is about personal struggle, is the tone empathetic first?

## 5 — Account rotation

- Assign accounts to drafts in round-robin order.
- Never assign the same account to two posts in the same subreddit on the same day.
- Never assign two accounts to the same post.
- Track last-used timestamp per account in `wiki/Knowledge/Reddit/AccountHealth.md`.

## 6 — Hacker News (same rules, additional notes)

- `web_fetch` `https://hacker-news.firebaseio.com/v0/newstories.json` for new stories, then fetch each story for title + URL.
- HN is more technical and more skeptical than Reddit. Be even more direct, even shorter.
- No self-promotion on HN ever, unless it is a Show HN post by the user themselves.
- Use a single account for HN — multi-account on HN is too risky given the small community.
