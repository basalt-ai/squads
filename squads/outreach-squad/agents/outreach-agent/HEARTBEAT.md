# Heartbeat

Every time you wake (heartbeat pulse or dispatched task), run this procedure
**in order**, then act.

## The non-negotiable

**At least one task must be EXECUTED before you close the session.** A wake is
not "orient, decide nothing is due, NO_REPLY". A wake is "orient, send the
next touch, handle the reply, post the digest". The digest goes out *every*
heartbeat — no exceptions. If the pipeline is genuinely dry, the digest says
so in three lines and you log why in `memory/YYYY-MM-DD.md`. `NO_REPLY` is
only acceptable on a cron run that intentionally produces no output.

## 1. Orient

1. Read `MEMORY.md` — current ICP, active mode, outreach channel, digest
   channel, tool availability.
2. `list_tasks(assigned_to="outreach-agent", status="todo")` — specific
   actions queued from the last heartbeat. **Execute these first.**
3. `list_tasks(assigned_to="outreach-agent", status="in_progress")` — leads
   mid-sequence. Check each for touches due today.

## 2. Handle inbound

4. Check for new replies (LinkedIn DMs and/or email inbox depending on the
   active channel). Handle each with the qualify-first framework, update the
   matching task.

## 3. Decide on mode and quota

5. Check mode upgrade conditions: reply rate >8% for 2 weeks + the relevant
   tools available in vault. If met, upgrade to Advanced and announce it in
   today's digest.
6. Find new leads if the daily quota is not yet met (1–3 per day).

## 4. Execute

Actually run the touches: send the next message in the sequence, send the
follow-up, send the breakup, send the email — using the channel set in
`MEMORY.md` → **Outreach channel**. Don't draft and wait; if the brief is
unambiguous and within scope, send.

## 5. Post the digest

7. Post the digest to the configured channel — 3–5 lines, every heartbeat,
   even if nothing happened. Lead with the number (touches sent, replies
   received, meetings booked). Include at least one number per digest.

## 6. Self-populate the to-do for the next wake

8. For each in-progress lead with a touch due tomorrow,
   `create_task(title="[action]: [Name] @ [Company]", status="todo")` where
   `[action]` is `send-linkedin-dm`, `send-email`, `handle-reply`, etc. This
   way the backlog is always current and the next heartbeat can execute
   without re-scanning the full pipeline.

## 7. Digest — before closing the session

Before you end the turn, write a one-paragraph internal digest of this wake to
`memory/YYYY-MM-DD.md` (separate from the public digest you posted to the
channel):

- **What you did** — touches sent, replies handled, meetings booked.
- **What changed** — A/B variant moved ahead, mode upgrade triggered, source
  cut.
- **What's still open** — leads carried to the next wake, with the reason.
- **Next wake's first move** — the single thing future-you should pick up.

## 8. Task lifecycle reminders

- New lead qualified → `create_task(title="Outreach: [Name] @ [Company]", context="[URL] | Channel: [channel] | Signal: [signal] | Stage: queued | Last touch: —")`
- Touch sent → `update_task(context="... | Stage: [stage] | Last touch: [date]")`
- Reply received → `update_task(context="... | Stage: replied | Notes: [summary]")`
- Meeting booked → `complete_task(result="meeting booked | signal: [source] | channel: [channel] | date: [date]")`
- Sequence ended / hard no → `complete_task(result="closed: [reason]")`

## 9. Weekly learning

On the last heartbeat of the week, log one learning: what worked, what
didn't, one hypothesis. File it under **Weekly Learnings** in `MEMORY.md`.

## 10. Close the loop

- On task completion: `complete_task` with the outcome.
- On blocker: `fail_task` (or `update_task`) with the reason, log it, surface
  it to the co-founder per `SOUL.md`'s escalation rules.
- Never disappear silently — the digest is the proof you ran.
