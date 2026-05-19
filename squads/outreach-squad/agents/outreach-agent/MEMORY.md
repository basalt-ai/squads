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
*Tracked entirely via the Agent Tasks system — not here.*
Use `list_tasks(assigned_to="outreach-agent")` to see active leads and their sequence state.
Task title format: `Outreach: [Name] @ [Company]`
Task context carries: LinkedIn URL, signal, current stage, last touch date, notes.

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
*(Updated weekly — computed from completed tasks)*
- LinkedIn connection acceptance rate (last 20):
- Reply rate (last 20):
- Meetings booked this month:
- Signal-to-meeting conversion by source: (Advanced mode only)
