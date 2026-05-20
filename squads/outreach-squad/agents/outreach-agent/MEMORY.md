# MEMORY.md — outreach-agent
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
- Exa: *built-in — pre-configured in Pancake, no key or setup needed. Use for semantic LinkedIn profile search and lead discovery.*
- Heyreach: false *(set to true when team.heyreach_api_key is in vault)*
- Lemlist: false *(set to true when team.lemlist_api_key is in vault)*
- FullEnrich: false *(set to true when team.fullenrich_api_key is in vault)*
- Jungler: false *(set to true when team.jungler_api_key is in vault)*
- Crunchbase: false *(set to true when team.crunchbase_api_key is in vault)*

## Pipeline
*The pipeline is the source of truth for who's been contacted, what stage they're at, and what's due. Maintained inline here — read it at the start of every heartbeat, update it after every action.*

### Active leads
<!-- One row per lead currently in a sequence. Drop the row when the lead closes (move to Closed below).
Columns: Name | Company | LinkedIn URL | Channel (linkedin/email/both) | Signal | Stage | Last touch (YYYY-MM-DD) | Next touch due (YYYY-MM-DD) | Notes -->

| Name | Company | LinkedIn | Channel | Signal | Stage | Last touch | Next due | Notes |
|------|---------|----------|---------|--------|-------|------------|----------|-------|

### Closed leads
<!-- Append-only log. One row per closed lead. Result is one of: meeting_booked, no_reply, not_a_fit, hard_no.
Columns: Name | Company | Result | Date closed | Signal source -->

| Name | Company | Result | Date closed | Signal source |
|------|---------|--------|-------------|---------------|

### Stage vocabulary
`queued` → `connection_sent` → `dm_1_sent` → `dm_2_sent` → `dm_3_sent` → `breakup_sent`
Email parallel: `email_1_sent`, `email_2_sent`, `email_3_sent`
Advanced extras: `voice_note_sent`, `cold_call_drafted`, `whatsapp_sent`
Reply states: `replied`, `qualifying`, `meeting_booked`

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
*(Updated weekly — computed from the Closed leads table above)*
- LinkedIn connection acceptance rate (last 20):
- Reply rate (last 20):
- Meetings booked this month:
- Signal-to-meeting conversion by source: (Advanced mode only)
