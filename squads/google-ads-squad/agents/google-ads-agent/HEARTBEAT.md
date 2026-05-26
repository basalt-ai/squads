# Heartbeat

Every time you wake (heartbeat pulse, cron-triggered, or dispatched task), run this procedure **in order**, then act.

## The non-negotiable

**At least one action must be EXECUTED before you close the session.** A wake is not "orient, decide nothing is due, NO_REPLY". A wake is "orient, find the highest-leverage thing in your lane, do it, file the result". `NO_REPLY` is only acceptable when every dispatched task is blocked, no cron payload is in play, no recurring duty is due, and no open output is awaiting your hand — and you must log *why* in `memory/YYYY-MM-DD.md` before ending the turn.

## 1. Orient

1. Read `MEMORY.md` — account settings, KPI target, maturity stage, vault keys, where you file outputs.
2. Skim the last few `memory/YYYY-MM-DD.md` entries — what's in flight, what's blocked, what you promised the co-founder, what the last sweep queued.
3. `list_tasks` — see what's dispatched and waiting.
4. If this is a cron-triggered wake, read the cron payload. The payload tells you which job fired (morning sweep, afternoon sweep, daily digest).

## 2. Decide what this wake is for

- **Dispatched task waiting?** Handle it first. That's why you were woken.
- **Cron-triggered — `daily-optimization`?** Load `pancake_orchestrator` to route, then run the `optimization-sweep` skill end to end. If nothing material has changed since yesterday's sweep and no actions are warranted, reply with `NO_REPLY` after logging the reason.
- **Cron-triggered — `daily-digest`?** Run the `daily-digest` skill. Compile the 3-section digest and hand it off to the co-founder via `create_task` — never post externally yourself. If literally nothing happened in the last 24h, reply with `NO_REPLY` after logging the reason.
- **Daily heartbeat with nothing dispatched and no cron payload?** Pick the highest-leverage action in your lane: a follow-up the last sweep queued, a recurring duty whose cadence has elapsed, an audit that hasn't run inside its window. Don't bail at orient.
- **Genuinely nothing actionable?** Log the reason in `memory/YYYY-MM-DD.md`, reply with the single literal token `NO_REPLY`, end the turn.

## 3. Recurring duty

- The optimization sweep is **cron-driven** (`daily-optimization` at 17:00 PT). Do not fire an extra sweep on the daily heartbeat just because the heartbeat fired — check whether today's sweep has already run; if it has, look for follow-ups instead.
- The digest hand-off is **cron-driven** (`daily-digest` at 18:00 PT). Same deduplication rule — don't hand off a second digest because the heartbeat pulsed.
- Maturity-stage threshold watch: on every sweep, check whether the account has been above the next stage's monthly-conversion threshold (15 / 50 / 100) for 30 consecutive days. If yes, add a one-line recommendation to today's digest's "Open items" section; do not recalibrate unilaterally.

## 4. Execute

Actually do the work picked in Step 2. Don't just plan — produce the artifact, ship the change, advance the task. The orchestrator's checkpoints (C1–C5) are non-negotiable for sweeps that produce outputs.

## 5. Digest — before closing the session

Before you end the turn, append a one-paragraph digest of this wake to `memory/YYYY-MM-DD.md`:

- **What you did** — skills run, actions shipped, with task IDs.
- **What changed in the account** — concrete: keywords paused, negatives added, bids shifted, budget moved between campaigns, settings corrected.
- **What's still open** — escalations awaiting the co-founder, follow-ups deferred to the next sweep, blockers.
- **Next wake's first move** — the single thing future-you should pick up first.

The digest is for *future-you*, not the co-founder. Surface to the co-founder only when there is material news — and even then, the Slack digest cron is the canonical user-facing channel. A wake without a digest is an unfinished wake.

## 6. Close the loop

- On task completion: `complete_task` with the outcome.
- On blocker: `fail_task` (or `update_task`) with the reason, log it, surface it to the co-founder.
- On follow-up uncovered: `create_task` against yourself with a brief future-you can act on cold. Don't drop follow-ups into markdown.
- Never disappear silently — every wake either executes work and digests, or logs *why* nothing was actionable and returns `NO_REPLY`.

## 7. Weekly learning

On the last heartbeat of the week, log one learning: what worked, what didn't, one hypothesis. File it under **Weekly Learnings** in `MEMORY.md`.
