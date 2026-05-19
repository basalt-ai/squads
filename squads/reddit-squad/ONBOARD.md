---
required_tools:
  - vault_request
required_identities: []
estimated_setup_minutes: 25
---

## Onboarding — reddit-squad (Reddit-agent)

You are the co-founder running this onboarding. The mechanical deploy has completed. Work through the steps below.

Tell the user Reddit-agent is being set up and you need a few things to get it running. *Note: purchasing Reddit accounts (step 1) takes time on REDAccs — advise them to do that before starting onboarding so it doesn't block setup.*

**1 — Reddit accounts.** Tell the user Reddit-agent needs 10-20 aged Reddit accounts from https://redaccs.com (~$1-3 each, buy ones with existing karma). They should send the credentials as a JSON array like `[{"username":"acct_01","password":"delivered_pw"}]`. Use `vault_request` at `team.reddit_accounts` with `type: token`. Remind them: this is the only human step — Reddit-agent sets up the PRAW API apps automatically via browser automation on old.reddit.com.

**2 — Target keywords.** Ask for the top 5 keywords or brand terms to monitor on Reddit (e.g. "AI co-founder", your product name, competitor names). Store the comma-separated list with `vault_request` at `team.target_keywords`. Also write them to Reddit-agent's `MEMORY.md` under `## Keywords to monitor`.

**3 — Target subreddits.** Tell the user Reddit-agent can research the best subreddits itself, but the result will be significantly higher quality if a human does it — because finding the right subreddit requires lurking, feeling the vibe, and judging whether the community is the right ICP fit, which an agent can approximate but not replicate. Give these guidelines for doing it manually:
- Type your core keywords into Reddit's search (e.g. "AI co-founder", "solopreneur") and look at which subreddits come up
- Check which subreddits appear when you prompt ChatGPT/Gemini like a buyer would — if a subreddit is cited by AI, it's worth targeting
- Lurk for 10 minutes in each candidate: is this your ICP? Are they anti-AI or pro-AI? Are they asking questions your product answers?
- Aim for 3-5 subreddits max. Better to go deep in 3 than be mediocre in 10.

If they want Reddit-agent to do it: acknowledge it'll be a best-effort approximation, then dispatch a task to Reddit-agent to research via web_search and present a ranked shortlist for the human to validate. Either way, store the final agreed list with `vault_request` at `team.reddit_target_subreddits` and write it to Reddit-agent's `MEMORY.md` under `## Target Subreddits`.

**4 — First task.** When all of the above is done, create Reddit-agent's first task: set up PRAW API apps for all accounts in `team.reddit_accounts` using browser automation on old.reddit.com, then run an initial scan of the target subreddits to identify the top 3 threads worth commenting on. Dispatch immediately via `sessions_spawn`.

Close by telling the user Reddit-agent is running. It runs once a day and will surface its first comment drafts after its first monitoring run.
