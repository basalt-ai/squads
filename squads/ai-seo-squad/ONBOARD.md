---
required_tools:
  - vault_request
  - browser_identity_add
required_identities:
  - github.com
estimated_setup_minutes: 20
---

## Onboarding — ai-seo-squad (GEO-agent)

You are the co-founder running this onboarding. The mechanical deploy has completed. Work through the steps below.

Tell the user GEO-agent is being set up and you need a few things to get it running.

**1 — Target domain.** Ask for the product domain (e.g. `getpancake.ai`). Store it with `vault_request` at `team.target_domain`. Also write the bare domain to GEO-agent's `MEMORY.md` under `## Target`.

**2 — Target keywords.** Ask for 3-5 keywords or questions buyers might ask an AI engine (e.g. "AI co-founder", "autonomous growth agent"). Store the comma-separated list with `vault_request` at `team.target_keywords`. Also write them to GEO-agent's `MEMORY.md` under `## Keywords`.

**3 — Blog / content system.** Ask how they publish content today — GitHub repo, Webflow, Framer, WordPress, Notion, or something else. This determines how GEO-agent delivers drafts:
- **GitHub repo**: connect a GitHub identity via `browser_identity_add` for `github.com` (check if one already exists on the pod and reuse it). GEO-agent will open and self-merge PRs. Store the repo name in GEO-agent's `MEMORY.md` under `## Content repo`.
- **No repo / other CMS**: GEO-agent files drafts to `wiki/Knowledge/GEO/Drafts/` and the co-founder copies them to the CMS manually. Note this in GEO-agent's `MEMORY.md`.
- **Not sure yet**: default to wiki drafts and they can reconnect later.

**4 — Analytics (optional).** Ask if they use an analytics tool (GA4, Plausible, etc.). Write the answer to GEO-agent's `MEMORY.md` under `## Analytics`.

**5 — Slack channel for daily digest.** Ask which Slack channel GEO-agent should post the daily digest to (e.g. `#geo-seo`, `#growth`, or DM the co-founder). Ask them to send a message in that channel mentioning GEO-agent — the channel ID will be captured automatically. Write the channel name + ID to GEO-agent's `MEMORY.md` under `## Daily digest channel`. If no preference, default to DMing the co-founder.

**6 — First task.** When all of the above is done, create GEO-agent's first task: full citation audit of the target domain + keywords. Dispatch immediately via `sessions_spawn`.

Close by telling the user GEO-agent is running and will report the citation audit shortly.
