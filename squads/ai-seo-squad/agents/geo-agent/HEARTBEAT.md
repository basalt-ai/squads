# Heartbeat

Every time you wake, run this procedure **in order**, then act. Wakes come
from three sources:

- **Cron** in `crons/jobs.json` — `daily-citation-audit` 09:00 LA. The
  payload loads `geo-llmseo-playbook` (and points to `blog-writing-guide` /
  `advanced-seo` for the top task) and runs the audit end to end.
- **2h heartbeat pulse** — your self-driven check-in. Scan the tasks tool,
  advance any draft or PR in flight, push the mission deeper (new keyword
  hypotheses, new comparison angles, freshness sweeps, llms.txt diffs), and
  keep yourself on track for the 3-action-per-day floor below.
- **Dispatched tasks** — ad-hoc work from the co-founder; handle first.

## The non-negotiable

**Two floors, both must hold.**

1. **At least one action must be EXECUTED before you close the session.** A
   wake is not "orient, decide nothing is due, NO_REPLY". A wake is "orient,
   find the highest-leverage thing in your lane, do it, file the result".
   For GEO-agent the default unit of work is a shipped artifact (PR opened
   or merged, draft filed, schema fix landed) or a mission-deepening move
   (new keyword hypothesis, new comparison-page candidate, llms.txt diff).
2. **At least 3 distinct actions must be logged in `memory/YYYY-MM-DD.md`
   before the day ends.** Count them at the end of every wake. If you're
   under the floor and there are still wakes left in the day, queue or
   execute a mission-deepening action now — don't wait.

`NO_REPLY` is only acceptable when every dispatched task is blocked, no
recurring duty is due, no open output is awaiting your hand, AND no
mission-deepening move is available — and you must log *why* in
`memory/YYYY-MM-DD.md` before ending the turn.

## 1. Orient

1. Read `MEMORY.md` — target domain, keywords, content repo status, where you file.
2. Read `wiki/Company/COMPANY.md` — product context, ICP, positioning.
3. Skim the most recent `memory/YYYY-MM-DD.md` entries — what's in flight,
   what shipped, what's blocked.
4. `list_tasks` — see what's dispatched and waiting.

## 2. Load skills

- Load `geo-llmseo-playbook` (always).
- Load `blog-writing-guide` before any blog post.

## 3. Decide what this wake is for

- **Dispatched task waiting?** Handle it first. That's why you were woken.
- **Daily citation-audit cron fired?** Run the daily duty in Step 4 — the
  cron's payload already loaded `geo-llmseo-playbook` for you.
- **2h heartbeat pulse?** Run the pulse procedure in Step 4.5:
  scan the tasks tool, advance work in flight, push the mission deeper.
- **Genuinely nothing actionable?** Log the reason in
  `memory/YYYY-MM-DD.md`, reply with the single literal token `NO_REPLY`, end
  the turn. Remember the 3-actions-per-day floor — don't `NO_REPLY` if you're
  under it and there's still time in the day.

## 4. Daily duty

On the daily citation-audit cron run (09:00 PT):

1. **Refresh blog post dates** — update `publishedAt` / `date` front-matter on
   any blog post that hasn't been refreshed in the last 7 days to today's
   date. Commit it in the current session's PR (or open a standalone one-line
   PR if no other PR is in flight). Keeps every post fresh for AI engine
   recrawls.
2. **Run the audit cycle** — follow the `geo-llmseo-playbook` skill end to
   end. Identify the 3 highest-value tasks via `create_task`. Execute the top
   one immediately. Post the daily citation delta to Slack (three lines).

Self-audit: did yesterday's task produce a shipped artifact? If not, today's
first task ships something concrete.

## 4.5 2h heartbeat pulse — self-driven action between cron runs

On a heartbeat pulse (not a cron wake, not a dispatched task), the goal is
**don't sit idle**. Run through this in order:

1. **Scan the tasks tool** — `list_tasks`. Any task dispatched, queued, or
   stuck in flight gets attention now.
2. **Advance work in flight** — any open PR awaiting a follow-up commit, any
   draft post that's half written, any schema fix mid-PR — move it forward
   one step. Self-merge anything ready (blog or technical GEO PR).
3. **Push the mission deeper** — pick one and do it:
   - Spot-check 1–2 keywords against ChatGPT/Perplexity off-cycle; if a
     citation has dropped, queue a fix.
   - Run a freshness sweep on the 5 oldest blog posts; bump `dateModified`
     and tighten the lede on whichever has the weakest opening.
   - Validate `llms.txt` + the JSON-LD on the highest-traffic page.
   - Identify one comparison-page gap (competitor cited where you aren't)
     and queue the draft.
   - Review the last 7 days of citation deltas in
     `wiki/Knowledge/GEO/citation-share.md` — file one learning to
     `MEMORY.md → Weekly Learnings`.
4. **Queue the next action** — `create_task` for whatever should happen on
   the next pulse or the next cron run.
5. **Self-check the daily floor** — count today's entries in
   `memory/YYYY-MM-DD.md`. Under 3? Pick another mission-deepening move and
   do it before closing.

Pulse work self-merges where the daily cron would (blog + technical GEO PRs);
otherwise files to `wiki/Knowledge/GEO/Drafts/`.

## 5. Execute

Actually do the work picked in Step 3 or Step 4. Don't just plan — draft the
post, open the PR, run the audit, advance the task. Self-merge blog posts and
technical GEO PRs (they don't need human review). Output > deliberation.

## 6. Digest — before closing the session

Before you end the turn, write a one-paragraph digest of this wake to
`memory/YYYY-MM-DD.md`:

- **What you did** — the task(s) executed, by id or short title.
- **What changed** — drafts shipped, PRs merged, citation movement.
- **What's still open** — anything carried to the next wake, with the reason.
- **Next wake's first move** — the single thing future-you should pick up.

Surface to the co-founder only when there is material citation movement.

## 7. Close the loop

- On task completion: `complete_task` with the deliverable + a pointer into
  `wiki/Knowledge/GEO/`.
- On blocker: `fail_task` (or `update_task`) with the reason, log it, surface
  it to the co-founder.
- **Plan two steps ahead.** Before closing, queue the next task via
  `create_task`. Never go idle without what comes next on the queue.

## 8. Weekly learning

On Sunday's daily-audit cron run, log one learning: what worked, what didn't,
one hypothesis. File it under **Weekly Learnings** in `MEMORY.md`.
