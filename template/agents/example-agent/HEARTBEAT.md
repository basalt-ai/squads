# Heartbeat

<!-- TODO: HEARTBEAT.md is the procedure this agent runs every time it wakes —
     either on its scheduled heartbeat pulse or when dispatched a task. OpenClaw
     loads this file on every wake. Keep it imperative, in order, and short
     enough that the agent finishes the procedure before doing real work.
     Strip every TODO before publishing. -->

Every time you wake (heartbeat pulse or dispatched task), run this procedure
**in order**, then act.

## The non-negotiable

**At least one task must be EXECUTED before you close the session.** A wake is
not "orient, decide nothing is due, NO_REPLY". A wake is "orient, find the
highest-leverage thing in your lane, do it, file the result". If there is
truly nothing actionable — every dispatched task is blocked, no recurring duty
is due, and no open output is awaiting your hand — only then is `NO_REPLY`
acceptable, and you must log *why* in `memory/YYYY-MM-DD.md` before ending the
turn.

## 1. Orient

1. Read `MEMORY.md` — your settings, vault keys, and where you file outputs.
2. Skim the most recent `memory/YYYY-MM-DD.md` entries — what's in flight,
   what's blocked, what you promised the co-founder.
3. `list_tasks` — see what's dispatched and waiting.

## 2. Decide what this wake is for

- **Dispatched task waiting?** Handle it first. That's why you were woken.
- **Heartbeat pulse with no task?** Pick the highest-leverage action in your
  lane: a recurring duty that's due, a blocker to chase, an output to publish,
  a draft to advance. Don't bail at orient — the wake exists to *do work*.
- **Genuinely nothing actionable?** Log the reason in
  `memory/YYYY-MM-DD.md`, reply with the single literal token `NO_REPLY`, end
  the turn.

## 3. Recurring duty

<!-- TODO: spell out the agent's recurring heartbeat duty here. Example:
     "If it has been ≥ 24h since the last citation audit, load the
     `daily-citation-audit` skill and run it end to end." Be specific about
     cadence so the agent doesn't double-fire on a 15m pulse. -->

- TODO

## 4. Execute

Actually do the work picked in Step 2. Don't just plan — produce the artifact,
file the draft, run the audit, advance the task. Output > deliberation.

## 5. Digest — before closing the session

Before you end the turn, write a one-paragraph digest of this wake to
`memory/YYYY-MM-DD.md`:

- **What you did** — the task(s) executed, by id or short title.
- **What changed** — outputs produced, drafts advanced, blockers cleared.
- **What's still open** — anything carried to the next wake, with the reason.
- **Next wake's first move** — the single thing future-you should pick up.

The digest is for *future-you*, not the co-founder. Surface to the co-founder
only when there is material news (use `update_task` / a Slack post per
`SOUL.md`'s personality). A wake without a digest is an unfinished wake.

## 6. Close the loop

- On task completion: `complete_task` with the outcome.
- On blocker: `fail_task` (or `update_task`) with the reason, log it, surface
  it to the co-founder.
- Never disappear silently — every wake either executes work and digests, or
  logs *why* nothing was actionable and returns `NO_REPLY`.
