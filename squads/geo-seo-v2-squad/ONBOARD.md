---
required_tools:
  - vault_request
  - browser_identity_add
required_identities:
  - github.com
estimated_setup_minutes: 12
---

## Onboarding — geo-seo-v2-squad (Atlas + Seren)

You are the co-founder running this onboarding. The mechanical deploy has completed. Work through the steps below — it should take about 12 minutes.

Tell the user both agents are being set up and you need a few things to get them running.

**1 — Target domain.** Ask for the product domain (e.g. `getpancake.ai`). Store it with `vault_request` at `team.target_domain`. Also write the bare domain to Atlas's `MEMORY.md` under `## Target`.

**2 — Target keywords.** Ask for 3-5 keywords or questions buyers might ask an AI engine (e.g. "AI co-founder", "autonomous growth agent"). Store the comma-separated list with `vault_request` at `team.target_keywords`. Also write them to Atlas's `MEMORY.md` under `## Keywords`.

**3 — GitHub for Atlas (optional).** Ask if they publish blog posts from a GitHub repo. If yes, connect a GitHub identity via `browser_identity_add` for `github.com` — check first if one already exists on the pod and reuse it. If no repo, note in Atlas's `MEMORY.md` that drafts go to `wiki/Knowledge/GEO/Drafts/` only.

**4 — Analytics (optional).** Ask if they use an analytics tool (GA4, Plausible, etc.). Write the answer to Atlas's `MEMORY.md` under `## Analytics`.

**5 — Reddit accounts.** Tell the user Seren needs 10-20 aged Reddit accounts from https://redaccs.com (~$1-3 each, buy ones with existing karma). They should send the credentials as a JSON array like `[{"username":"acct_01","password":"delivered_pw"}]`. Use `vault_request` at `team.reddit_accounts` with `type: json`. Remind them: this is the only human step — Seren sets up the PRAW API apps automatically via browser automation on old.reddit.com.

**6 — Target subreddits.** Ask which subreddits to monitor (e.g. `Entrepreneurs`, `startups`, `SaaS`, `AItools`). Store comma-separated with `vault_request` at `team.reddit_target_subreddits`. Also write them to Seren's `MEMORY.md` under `## Target Subreddits`.

**7 — First tasks.** When all of the above is done:
- Create Atlas's first task: full citation audit of the target domain + keywords. Dispatch immediately via `sessions_spawn`.
- Create Seren's first task: set up PRAW API apps for all accounts in `team.reddit_accounts` using browser automation on old.reddit.com, then run an initial scan of the target subreddits to identify the top 3 threads worth commenting on. Dispatch immediately via `sessions_spawn`.

Close by telling the user both agents are running and Atlas will report the citation audit shortly. Seren will surface comment drafts for review within 2 hours.
