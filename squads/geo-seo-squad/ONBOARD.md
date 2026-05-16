---
required_tools:
  - vault_request   # collect target_domain + target_keywords
  - browser_identity_add   # optional GitHub identity for content publishing
required_identities:
  - github.com   # optional — only if the user publishes content from a repo
estimated_setup_minutes: 8
---

## Onboarding — Atlas (GEO/SEO)

You are the main coordinator running the install skill. The mechanical deploy has already
completed (agent files written, crons registered, skills deployed). Your job now is the
short onboarding conversation. Keep it tight — the user was promised ~8 minutes.

Start by telling the user Atlas is being set up and you need two quick things.

**1 — Target domain.** Ask for the product domain Atlas should track (e.g. `getpancake.ai`).
When they answer, store it with `vault_request` at the vault key `team.target_domain`. This
is not a real secret, but routing it through `vault_request` keeps the value off the Slack
transcript and out of the wiki. Also record the bare domain in Atlas's `MEMORY.md` under
`## Target` so Atlas has it without a vault read on every run.

**2 — Target keywords.** Ask for the 3–5 keywords or buyer questions Atlas should monitor
across AI engines (e.g. "AI co-founder", "autonomous growth agent"). Store the
comma-separated list with `vault_request` at `team.target_keywords`, and also write the list
into Atlas's `MEMORY.md` under `## Keywords`.

**3 — Content publishing (optional).** Ask whether the user keeps blog posts in a GitHub
repo. If yes, connect a GitHub identity with `browser_identity_add` for `github.com` so
Atlas can open draft PRs — but first check whether a GitHub identity already exists on this
pod and reuse it if the site matches. If they don't publish from a repo, skip this entirely
and tell Atlas to hand finished drafts to the co-founder instead.

**4 — Analytics (optional, one question).** Ask if they use an analytics tool (GA4,
Plausible, etc.). Whatever they answer — including "none" — write it to Atlas's `MEMORY.md`
under `## Analytics`. Atlas adapts its reporting to what's available; don't push for a
connection.

When all of the above is done, confirm the daily citation audit cron is registered (it runs
6 PM Pacific). Then create Atlas's first task — a full citation audit of the target domain
and keywords — and dispatch it immediately: `sessions_spawn` Atlas on the task, then mark it
`in_progress`. Don't leave it in `todo` waiting for the 6 PM cron; the user is here now, so
Atlas should start now. Close by telling the user the audit is already running and Atlas
will report back shortly.
