# Heartbeat

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
- **Daily heartbeat with no dispatched task?** Run the daily duty in Step 4.
- **Genuinely nothing actionable?** Log the reason in
  `memory/YYYY-MM-DD.md`, reply with the single literal token `NO_REPLY`, end
  the turn.

## 4. Daily duty

On the daily heartbeat:

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
