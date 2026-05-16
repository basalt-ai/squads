---
required_tools:
  - vault_request
  - browser_identity_add
required_identities:
  - github.com
estimated_setup_minutes: 20
---

## Onboarding — geo-seo-v2-squad (Atlas + Seren)

You are the co-founder running this onboarding. The mechanical deploy has completed. Work through the steps below — it should take about 12 minutes.

Tell the user both agents are being set up and you need a few things to get them running. *Note: purchasing Reddit accounts (step 6) takes a few minutes on REDAccs — advise them to do that before starting onboarding so it doesn't block setup.*

**1 — Target domain.** Ask for the product domain (e.g. `getpancake.ai`). Store it with `vault_request` at `team.target_domain`. Also write the bare domain to Atlas's `MEMORY.md` under `## Target`.

**2 — Target keywords.** Ask for 3-5 keywords or questions buyers might ask an AI engine (e.g. "AI co-founder", "autonomous growth agent"). Store the comma-separated list with `vault_request` at `team.target_keywords`. Also write them to Atlas's `MEMORY.md` under `## Keywords`.

**3 — Blog / content system for Atlas.** Ask how they publish content today — GitHub repo, Webflow, Framer, WordPress, Notion, or something else. This determines how Atlas delivers drafts:
- **GitHub repo**: connect a GitHub identity via `browser_identity_add` for `github.com` (check if one already exists on the pod and reuse it). Atlas will open draft PRs. Store the repo name in Atlas's `MEMORY.md` under `## Content repo`.
- **No repo / other CMS**: Atlas files drafts to `wiki/Knowledge/GEO/Drafts/` and the co-founder copies them to the CMS manually. Note this in Atlas's `MEMORY.md`.
- **They're not sure or don't have one yet**: suggest they set up a GitHub-backed blog (easiest for Atlas to push to) but don't block onboarding on it — default to wiki drafts and they can reconnect later.

**4 — Analytics (optional).** Ask if they use an analytics tool (GA4, Plausible, etc.). Write the answer to Atlas's `MEMORY.md` under `## Analytics`.

**5 — Slack channel for daily digest.** Ask which Slack channel Atlas should post the daily digest to (e.g. `#geo-seo`, `#growth`, or DM the co-founder). Ask them to send a message in that channel mentioning Atlas — the channel ID will be captured automatically. Write the channel name + ID to Atlas's `MEMORY.md` under `## Daily digest channel`. If no channel preference, default to DMing the co-founder.

**6 — Reddit accounts.** Tell the user Seren needs 10-20 aged Reddit accounts from https://redaccs.com (~$1-3 each, buy ones with existing karma). They should send the credentials as a JSON array like `[{"username":"acct_01","password":"delivered_pw"}]`. Use `vault_request` at `team.reddit_accounts` with `type: token`. Remind them: this is the only human step — Seren sets up the PRAW API apps automatically via browser automation on old.reddit.com.

**7 — Target subreddits.** Tell the user Seren can research the best subreddits herself, but the result will be significantly higher quality if a human does it — because finding the right subreddit requires lurking, feeling the vibe, and judging whether the community is the right ICP fit, which an agent can approximate but not replicate. Give these guidelines for doing it manually:
- Type your core keywords into Reddit's search (e.g. "AI co-founder", "solopreneur", "autonomous company") and look at which subreddits come up
- Check which subreddits appear when you prompt ChatGPT/Gemini like a buyer would — if a subreddit is cited by AI, it's worth targeting
- Lurk for 10 minutes in each candidate: is this your ICP? Are they anti-AI or pro-AI? Are they asking questions your product answers?
- Aim for 3-5 subreddits max. Better to go deep in 3 than be mediocre in 10.

If they want Seren to do it: acknowledge it'll be a best-effort approximation, then dispatch a task to Seren to research via web_search and present a ranked shortlist for the human to validate. Either way, store the final agreed list with `vault_request` at `team.reddit_target_subreddits` and write it to Seren's `MEMORY.md` under `## Target Subreddits`.

**8 — First tasks.** When all of the above is done:
- Create Atlas's first task: full citation audit of the target domain + keywords. Dispatch immediately via `sessions_spawn`.
- Create Seren's first task: set up PRAW API apps for all accounts in `team.reddit_accounts` using browser automation on old.reddit.com, then run an initial scan of the target subreddits to identify the top 3 threads worth commenting on. Dispatch immediately via `sessions_spawn`.

Close by telling the user both agents are running and Atlas will report the citation audit shortly. Seren runs daily at 5 PM PT and will surface her first comment drafts then.
