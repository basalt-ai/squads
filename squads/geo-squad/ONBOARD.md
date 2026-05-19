---
required_tools:
  - vault_request
  - browser_identity_add
required_identities:
  - github.com
estimated_setup_minutes: 20
---

## Onboarding — geo-squad (Atlas)

You are the co-founder running this onboarding. The mechanical deploy has completed. Work through the steps below.

Tell the user Atlas is being set up and you need a few things to get it running.

**1 — Target domain.** Ask for the product domain (e.g. `getpancake.ai`). Store it with `vault_request` at `team.target_domain`. Also write the bare domain to Atlas's `MEMORY.md` under `## Target`.

**2 — Target keywords.** Ask for 3-5 keywords or questions buyers might ask an AI engine (e.g. "AI co-founder", "autonomous growth agent"). Store the comma-separated list with `vault_request` at `team.target_keywords`. Also write them to Atlas's `MEMORY.md` under `## Keywords`.

**3 — Blog / content system.** Ask how they publish content today — GitHub repo, Webflow, Framer, WordPress, Notion, or something else. This determines how Atlas delivers drafts:
- **GitHub repo**: connect a GitHub identity via `browser_identity_add` for `github.com` (check if one already exists on the pod and reuse it). Atlas will open and self-merge PRs. Store the repo name in Atlas's `MEMORY.md` under `## Content repo`.
- **No repo / other CMS**: Atlas files drafts to `wiki/Knowledge/GEO/Drafts/` and the co-founder copies them to the CMS manually. Note this in Atlas's `MEMORY.md`.
- **Not sure yet**: default to wiki drafts and they can reconnect later.

**4 — Analytics (optional).** Ask if they use an analytics tool (GA4, Plausible, etc.). Write the answer to Atlas's `MEMORY.md` under `## Analytics`.

**5 — Slack channel for daily digest.** Ask which Slack channel Atlas should post the daily digest to (e.g. `#geo-seo`, `#growth`, or DM the co-founder). Ask them to send a message in that channel mentioning Atlas — the channel ID will be captured automatically. Write the channel name + ID to Atlas's `MEMORY.md` under `## Daily digest channel`. If no preference, default to DMing the co-founder.

**6 — First task.** When all of the above is done, create Atlas's first task: full citation audit of the target domain + keywords. Dispatch immediately via `sessions_spawn`.

Close by telling the user Atlas is running and will report the citation audit shortly.
