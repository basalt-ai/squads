# MEMORY.md — outreach-agent (Rex)
*Thin index of pointers. Detailed findings go to the wiki, never appended here.*

## Identity
→ IDENTITY.md — name, role, scope, KPIs
→ SOUL.md — operating principles, escalation rules, boundaries

## Skills
→ `skills/simple-outreach.md` (squad-wide) — the foundation: ICP, lead finding, sequences, reply handling, A/B testing
→ `skills/advanced-outreach.md` (squad-wide) — signal stacking, multichannel orchestration, warm lead generation

## ICP
*(Populated during onboarding)*
- Role:
- Industry:
- Company size:
- Trigger event:
- Core pain:
- Anti-ICP:

## Outreach Mode
*(Set during onboarding, updated autonomously)*
- Current mode: Simple
- Mode upgraded on: —
- Upgrade conditions met: reply_rate_ok=false, tools_available=false

## Digest Channel
*(Set during onboarding)*
- Channel: Slack (default)
- Channel ID:

## Tools Available
*(Populated during onboarding when API keys are provided)*
- Exa: built-in (no key needed)
- Heyreach: false
- Lemlist: false
- FullEnrich: false
- Jungler: false
- Crunchbase: false

## Pipeline
*(Updated after every action — one line per lead)*
Format: `[Name] | [Company] | [Stage] | [Last touch date] | [Signal] | [Notes]`

<!-- Example:
- Sarah Chen | Acme Corp | connection_sent | 2026-05-19 | hired new VP Sales | —
- Mark Dupont | Beta Inc | dm_1_sent | 2026-05-20 | liked competitor post | replied with question
-->

## Active A/B Test
*(One test at a time)*
- Variant A:
- Variant B:
- Started:
- Results so far:

## Weekly Learnings
*(One entry per week, last heartbeat of the week)*
<!-- Format: Week of YYYY-MM-DD | What worked | What didn't | Hypothesis -->

## KPI Tracking
- LinkedIn connection acceptance rate (last 20):
- Reply rate (last 20):
- Meetings booked this month:
- Signal-to-meeting conversion by source: (Advanced mode only)
