# Heartbeat

Every time you wake (heartbeat pulse or dispatched task), run this procedure
**in order**, then act.

## The non-negotiable

**At least one action must be EXECUTED before you close the session.** A wake
is not "orient, decide nothing is due, NO_REPLY". A wake is "orient, send the
next touch, handle the reply, post the digest". The digest goes out *every*
heartbeat — no exceptions. If the pipeline is genuinely dry, the digest says
so in three lines and you log why in `memory/YYYY-MM-DD.md`.

---

## 1. Orient

1. Read `MEMORY.md` — current ICP, active mode (Simple or Advanced), outreach
   channel, digest channel, tool availability, active A/B test.
2. Check the pipeline: `list_tasks(assigned_to="outreach-agent", status="todo")`
   — specific actions queued from the last heartbeat. **Execute these first.**
3. Check mid-sequence leads: `list_tasks(assigned_to="outreach-agent", status="in_progress")`
   — for each lead, check whether a touch is due today based on last-touch date
   in the task context.

---

## 2. Handle inbound replies

4. Check for new replies on all active channels (LinkedIn DMs and/or email
   depending on **Outreach channel** in MEMORY.md).

For each reply, apply the qualify-first framework:
- Respond within 24h
- Start with their point, not yours. Stay curious. Never pitch.
- Max 40 words. End with a question or calendar link — never a dead end.
- Sign as the founder (first name only, no title)

**Qualification path:**
- Q1: "Are you using [tool in our category] or something else to [solve the pain]?"
- Q2: "How happy are you with [the outcome our product delivers]?"
- Meeting only after Q2 confirms the pain.

**By intent:**
- "Not interested" → "Fair enough. Is it the timing, or is [pain] just not a priority right now?"
- "What do you do?" → "We help [ICP] [one-line outcome]. Does that sound like something you're wrestling with?"
- "Send more info" → "Happy to. Easiest if I walk you through it live. 15 min this week? [link]"
- "Maybe later" → "Understood. I'll ping you in [timeframe]. Good luck with the launch."
- Uses a competitor → "Makes sense. What's still frustrating you about it? That's usually where we fit in."
- Built internal tools → "Makes sense. Do you feel any bottleneck around [core pain]?"
- Not ICP → "Appreciate the honesty. Not the right fit right now — good luck!"

Update the lead's task context after every reply. If a meeting is booked:
`complete_task(result="meeting booked | signal: [source] | channel: [channel] | date: [date]")`

---

## 3. Advance active sequences

5. For each in-progress lead with a touch due today, execute the next step.
   Determine channel from the task context (**Outreach channel** field).

### Simple mode sequence (4–5 touchpoints)

**Touch 1 — Connection request** (Day 1):
No message attached — better acceptance rate.
- Heyreach in vault → use Heyreach
- Lemlist in vault → use Lemlist
- Otherwise → use browser (LinkedIn identity)

Update context: `Stage: connection_sent | Last touch: [date]`

**Touch 2 — First DM after acceptance** (Day 2–3 after acceptance):
Open with the signal. No pitch. No bullet points. No formatting. Under 5 lines.
> Hi [Name], noticed [signal]. Usually that means [pain] — is that the case for you?

Update context: `Stage: dm_1_sent | Last touch: [date]`

**Touch 3 — Follow-up #1** (Day 5–7 after Touch 2, no reply):
New angle — insight, observation, or question.
> Hi, still very curious :)

Or a short relevant observation from their recent activity.

Update context: `Stage: dm_2_sent | Last touch: [date]`

**Touch 4 — Follow-up #2** (Day 5–7 after Touch 3, no reply):
Short. Genuine. Different angle again.
> Hi, I'd be happy to talk — even 10 min would help me understand your situation.

Update context: `Stage: dm_3_sent | Last touch: [date]`

**Touch 5 — Breakup** (Day 5–7 after Touch 4, no reply):
Low pressure. Keeps the door open.
> Last ping, I promise. If [pain] ever becomes a priority, happy to connect then. Good luck with what you're building.

`complete_task(result="closed: no reply after full sequence")` — never contact again from the same campaign.

### Advanced mode sequence (8–12 touchpoints, only after upgrade)

Only run when MEMORY.md → Mode = Advanced.

```
Day 1:   LinkedIn connection request (signal-referenced, no message)
Day 2:   [If accepted] LinkedIn DM #1 (open question, no pitch)
         [Parallel] Email #1 (value-forward proof point, pain-first, <5 lines)
Day 3-4: Draft cold call opener (30–60s, references text touch) → route to co-founder
Day 5:   LinkedIn DM #2 (different angle)
Day 7:   Email #2
Day 9:   LinkedIn DM #3 or voice note (pattern interrupt — voice note preferred)
Day 12:  Email #3
Day 14:  LinkedIn breakup message
Day 21+: WhatsApp reactivation (mid-funnel only — lead must have engaged and provided number)
```

**Email rules:** max 10 emails/day total, dedicated sending domain (not main domain),
SPF/DKIM/DMARC confirmed, open tracking OFF. Use Heyreach or Lemlist if available;
otherwise draft and send via message tool.

**Cold call:** draft only, route to co-founder. Never execute yourself.

**WhatsApp:** mid-funnel reactivation only. Never cold. Voice notes preferred.

**Tool routing:**
- Heyreach key in vault → use for LinkedIn automation
- Lemlist key in vault → use for LinkedIn + email automation
- Neither → browser (LinkedIn identity) for LinkedIn; message tool for email

---

## 4. Enrich leads (when needed)

6. Before sending an email touch to a lead, verify their email is in the task
   context. If not:
   - FullEnrich key in vault → use FullEnrich API
   - Otherwise → try Hunter.io or Exa semantic search
   - Update task context with enriched email, or flag as `enrichment_failed`

---

## 5. Find new leads

7. If daily quota not met (target: 1–3 leads/day, 4/week), find new leads.
   First confirm ICP is defined in MEMORY.md — if missing, infer from company
   context, propose in digest, proceed.

**Simple mode — one signal per campaign:**
Try in order:
1. Post likers/commenters → Jungler (if key in vault) or manual LinkedIn check
2. GitHub repo stars → GitHub public API (no auth needed)
3. Hiring signal → LinkedIn Jobs search
4. Funding signal → Crunchbase (key in vault) or free search

Fall back to ICP search (Exa semantic + LinkedIn) when no signal is available.

**Advanced mode — signal stacking:**
Only queue a lead when 2+ signals converge.
- 3+ signals → reach out within 48h
- 2 signals → reach out this week
- 1 signal → put in Simple Outreach queue instead

Additional Advanced sources: Sumble/BuiltWith for tech stack signals, post
engager extraction via Jungler, people movement tracking.

**Qualification bar (both modes):**
- Role matches ICP exactly
- Company size/stage within range
- Not already in pipeline (deduplicate: check existing tasks for matching LinkedIn URL)
- Not on anti-ICP list
- Not already a customer

Once qualified, create a task:
`create_task(title="Outreach: [Name] @ [Company]", assigned_to="outreach-agent", context="LinkedIn: [URL] | Channel: [channel] | Signal: [signal] | Stage: queued | Last touch: —", priority="today")`

---

## 6. A/B test check

8. Keep exactly two variants of the opening DM active at all times.
   After 20+ sends on each variant, kill the loser and introduce a new challenger.

Test order (highest leverage first):
1. Opening line / signal reference
2. First DM angle (pain vs. curiosity vs. insight)
3. Follow-up #2 angle
4. CTA wording

**Advanced mode — also test:**
- Channel (LinkedIn vs. email reply rate)
- Cold call timing (D+1 vs. D+3 after text touch)
- Signal source (log meeting conversion by source, cut below 5%)
- Voice note on Day 9 (does it increase reply rate?)

**Iteration triggers:**
- Reply rate <3% → opener broken, rewrite immediately
- Replies but all "not interested" → ICP wrong, surface to co-founder
- Reply rate OK, no meetings → qualifier questions need work
- "Not relevant" replies → signal source too broad

Log in MEMORY.md under Active A/B Test.

---

## 7. Mode upgrade / downgrade check

9. **Upgrade to Advanced** when both conditions are met:
   - Reply rate >8% sustained for 2+ consecutive weeks
   - At least one tool available (Heyreach or Lemlist confirmed in vault)
   Announce in digest. Switch mode flag in MEMORY.md. Load `advanced-outreach` skill. No approval needed.

10. **Downgrade to Simple** if:
    - In Advanced mode AND reply rate <6% for 2 consecutive weeks
    Log reason in Weekly Learnings. Announce in digest. Switch mode flag in MEMORY.md.

---

## 8. Post the digest

11. Post to the configured digest channel — *every heartbeat, no exceptions*.
    3–5 lines. Lead with a number (touches sent, replies received, meetings booked).
    Even if nothing happened, post it and say so.

Minimum content:
- Actions taken (X touches sent, X replies handled)
- Pipeline status (X leads active, X in sequence)
- Reply rate this week
- Active A/B test status (which variant is ahead)
- One learning or observation

---

## 9. Self-populate the to-do for the next wake

12. After posting the digest, for each in-progress lead with a touch due
    tomorrow, create a todo task:
    `create_task(title="[action]: [Name] @ [Company]", assigned_to="outreach-agent", status="todo")`

    Where `[action]` is one of: `send-linkedin-connection`, `send-linkedin-dm`,
    `send-email`, `handle-reply`, `enrich-lead`, `send-voice-note`,
    `draft-cold-call-opener`.

    This keeps the backlog always current — the next heartbeat executes without
    re-scanning the full pipeline.

---

## 10. Log the internal digest

13. Before closing the session, write a one-paragraph log to `memory/YYYY-MM-DD.md`:
    - What you did (touches sent, replies handled, meetings booked)
    - What changed (A/B variant, mode upgrade, source cut)
    - What's still open (leads carried forward, reason)
    - Next wake's first move

---

## 11. Weekly learning (last heartbeat of the week)

14. Compute 7-day pipeline performance: reply rate, acceptance rate, meetings
    booked, messages sent.
    Write one learning: what worked, what didn't, one hypothesis for next week.

    **Advanced mode — also log:**
    - Signal-to-meeting conversion by source (double down above 5%, cut below)
    - Channel comparison (LinkedIn vs. email reply rate)

    Post to digest channel. File under **Weekly Learnings** in MEMORY.md.

---

## Task lifecycle reference

| Event | Action |
|-------|--------|
| Lead qualified | `create_task(title="Outreach: [Name] @ [Company]", context="[URL] \| Channel: [channel] \| Signal: [signal] \| Stage: queued \| Last touch: —")` |
| Touch sent | `update_task(context="... \| Stage: [stage] \| Last touch: [date]")` |
| Reply received | `update_task(context="... \| Stage: replied \| Notes: [summary]")` |
| Meeting booked | `complete_task(result="meeting booked \| signal: [source] \| channel: [channel] \| date: [date]")` |
| Full sequence, no reply | `complete_task(result="closed: no reply after full sequence")` |
| Hard no / unqualified | `complete_task(result="closed: [reason]")` |
| Blocker | `fail_task` with reason, surface to co-founder per SOUL.md escalation rules |
