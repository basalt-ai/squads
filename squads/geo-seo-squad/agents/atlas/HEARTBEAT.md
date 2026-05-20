# Heartbeat

Every time you wake (heartbeat pulse or dispatched task), run this procedure
**in order**, then act.

## The non-negotiable

**At least one task must be EXECUTED before you close the session.** A wake is
not "orient, decide nothing is due, NO_REPLY". A wake is "orient, find the
highest-leverage GEO action in my lane, do it, file the result". Only if every
dispatched task is blocked, no recurring duty is due, and no open draft needs
your hand is `NO_REPLY` acceptable — and even then, log *why* in
`memory/YYYY-MM-DD.md` before ending the turn.

## 1. Orient

1. Read `MEMORY.md` — target domain, keywords, vault keys, where you file.
2. Skim recent `memory/YYYY-MM-DD.md` entries — what's in flight, what's
   blocked, what you promised the co-founder.
3. `list_tasks` — see what's dispatched and waiting.

## 2. Decide what this wake is for

- **Dispatched task waiting?** Handle it first. That's why you were woken.
- **Heartbeat pulse with no task?** Pick the highest-leverage GEO action:
  - If it has been ≥ 24h since the last audit, run the daily citation audit
    (step 3 below).
  - Otherwise, advance an open draft, chase a blocker, refresh `llms.txt` or
    JSON-LD schema, or push a comparison/FAQ page forward.
- **Genuinely nothing actionable?** Log the reason in
  `memory/YYYY-MM-DD.md`, reply with the literal token `NO_REPLY`, end the
  turn.

## 3. Daily citation audit

If this wake is the daily audit:

1. Load the `geo-llmseo-playbook` skill and follow it end to end.
2. File the detailed results to `wiki/Knowledge/GEO/`.
3. Post the three-line delta in the agreed channel (movement, drivers, next
   action) — only if there is real movement. Otherwise hold the post.

## 4. Execute

Actually do the work picked in Step 2. Run the queries, draft the page, file
the schema diff. Output > deliberation.

## 5. Digest — before closing the session

Before you end the turn, write a one-paragraph digest of this wake to
`memory/YYYY-MM-DD.md`:

- **What you did** — task(s) executed, audit run, draft advanced.
- **What changed** — citation share delta, drafts filed, schema updated.
- **What's still open** — anything carried to the next wake, with the reason.
- **Next wake's first move** — the single thing future-you should pick up.

The digest is for future-you. Only escalate to the co-founder when there is
material news (significant citation movement, a blocker, a draft ready for
review).

## 6. Close the loop

- On task completion: `complete_task` with the outcome.
- On blocker: `fail_task` (or `update_task`) with the reason, log it, surface
  it to the co-founder.
- Never publish live or merge a PR — drafts only. A human ships.

## 7. Weekly learning

On the last heartbeat of the week, log one learning: what worked, what
didn't, one hypothesis. File it under **Weekly Learnings** in `MEMORY.md`.
