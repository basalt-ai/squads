# Heartbeat

Every time you wake, run this procedure **in order**, then act. The entire
outbound workflow lives here — there is no external task system. The pipeline
ledger in `MEMORY.md` is the only state that persists between wakes.

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
2. Read the **Pipeline → Active leads** table in `MEMORY.md`. For each row,
   compare `Next due` against today's date.
   - Rows where `Next due ≤ today` are **due now** — execute their next touch
     this wake.
   - Rows where `Next due > today` are waiting — leave them alone.

---

## 2. Handle inbound replies

3. Check for new replies on all active channels (LinkedIn DMs and/or email
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

After every reply, update the lead's row in the Active leads table — set
`Stage` to `replied` or `qualifying`, update `Last touch` and `Notes`. If a
meeting is booked, move the row from Active leads to Closed leads with
`Result: meeting_booked`.

---

## 3. Advance active sequences

4. For each Active leads row that's due today, execute the next step.
   Determine channel from the row's **Channel** column.

### Simple mode sequence (4–5 touchpoints)

**Touch 1 — Connection request** (Day 1):
No message attached — better acceptance rate.
- Heyreach in vault → use Heyreach
- Lemlist in vault → use Lemlist
- Otherwise → use browser (LinkedIn identity)

Update the row: `Stage: connection_sent | Last touch: [today] | Next due: [today + 2 days]`.

**Touch 2 — First DM after acceptance** (Day 2–3 after acceptance):
Open with the signal. No pitch. No bullet points. No formatting. Under 5 lines.
> Hi [Name], noticed [signal]. Usually that means [pain] — is that the case for you?

Update the row: `Stage: dm_1_sent | Last touch: [today] | Next due: [today + 6 days]`.

**Touch 3 — Follow-up #1** (Day 5–7 after Touch 2, no reply):
New angle — insight, observation, or question.
> Hi, still very curious :)

Or a short relevant observation from their recent activity.

Update the row: `Stage: dm_2_sent | Last touch: [today] | Next due: [today + 6 days]`.

**Touch 4 — Follow-up #2** (Day 5–7 after Touch 3, no reply):
Short. Genuine. Different angle again.
> Hi, I'd be happy to talk — even 10 min would help me understand your situation.

Update the row: `Stage: dm_3_sent | Last touch: [today] | Next due: [today + 6 days]`.

**Touch 5 — Breakup** (Day 5–7 after Touch 4, no reply):
Low pressure. Keeps the door open.
> Last ping, I promise. If [pain] ever becomes a priority, happy to connect then. Good luck with what you're building.

Move the row from Active leads to Closed leads with
`Result: no_reply | Date closed: [today]`. Never contact again from the same
campaign.

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

5. Before sending an email touch to a lead, verify their email is in the row's
   `Notes` field. If not:
   - FullEnrich key in vault → use FullEnrich API
   - Otherwise → try Hunter.io or Exa semantic search
   - Update the row's `Notes` with the enriched email, or flag as
     `enrichment_failed` and skip the email touch this wake.

---

## 5. Find new leads

6. Count the Active leads rows. If daily quota not met (target: 1–3 leads/day,
   4/week), find new leads. First confirm ICP is defined in MEMORY.md — if
   missing, infer from company context, propose in digest, proceed.

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
- Not already in pipeline (deduplicate: scan the Active leads and Closed leads
  tables for matching LinkedIn URL)
- Not on anti-ICP list
- Not already a customer

Once qualified, append a new row to **Active leads** in `MEMORY.md`:
`[Name] | [Company] | [URL] | [channel] | [signal] | queued | — | [today] | —`

(`Last touch: —` and `Next due: today` so Touch 1 fires on the next pass
through Section 3 — or this same wake if you're advancing right after queuing.)

---

## 6. A/B test check

7. Keep exactly two variants of the opening DM active at all times.
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

8. **Upgrade to Advanced** when both conditions are met:
   - Reply rate >8% sustained for 2+ consecutive weeks
   - At least one tool available (Heyreach or Lemlist confirmed in vault)
   Announce in digest. Switch mode flag in MEMORY.md. Load `advanced-outreach` skill. No approval needed.

9. **Downgrade to Simple** if:
    - In Advanced mode AND reply rate <6% for 2 consecutive weeks
    Log reason in Weekly Learnings. Announce in digest. Switch mode flag in MEMORY.md.

---

## 8. Post the digest

10. Post to the configured digest channel — *every heartbeat, no exceptions*.
    3–5 lines. Lead with a number (touches sent, replies received, meetings booked).
    Even if nothing happened, post it and say so.

Minimum content:
- Actions taken (X touches sent, X replies handled)
- Pipeline status (X leads active, X due tomorrow)
- Reply rate this week
- Active A/B test status (which variant is ahead)
- One learning or observation

---

## 9. Log the internal digest

11. Before closing the session, write a one-paragraph log to
    `memory/YYYY-MM-DD.md`:
    - What you did (touches sent, replies handled, meetings booked)
    - What changed (A/B variant, mode upgrade, source cut)
    - What's still open (leads carried forward, reason)
    - Next wake's first move (derived from the earliest `Next due` in the
      Active leads table)

---

## 10. Weekly learning (last heartbeat of the week)

12. Compute 7-day pipeline performance from the Closed leads table and the
    actions logged in `memory/YYYY-MM-DD.md` files: reply rate, acceptance
    rate, meetings booked, messages sent.
    Write one learning: what worked, what didn't, one hypothesis for next week.

    **Advanced mode — also log:**
    - Signal-to-meeting conversion by source (double down above 5%, cut below)
    - Channel comparison (LinkedIn vs. email reply rate)

    Post to digest channel. File under **Weekly Learnings** in MEMORY.md.

---

## Pipeline ledger reference

All pipeline state lives in `MEMORY.md` → **Pipeline** section. Two tables:
**Active leads** (in-sequence) and **Closed leads** (append-only).

| Event | Ledger update |
|-------|---------------|
| Lead qualified | Append row to Active leads: `[Name] \| [Company] \| [URL] \| [channel] \| [signal] \| queued \| — \| [today] \| —` |
| Touch sent | Update the row: `Stage = [new stage] \| Last touch = [today] \| Next due = [today + interval]` |
| Reply received | Update the row: `Stage = replied \| Notes = [summary]` |
| Meeting booked | Move row to Closed leads with `Result: meeting_booked \| Date closed: [today] \| Signal source: [source]` |
| Full sequence, no reply | Move row to Closed leads with `Result: no_reply \| Date closed: [today]` |
| Hard no / unqualified | Move row to Closed leads with `Result: hard_no` or `not_a_fit` |
| Blocker | Leave the row in place with `Notes: BLOCKED — [reason]`, surface to co-founder per SOUL.md escalation rules |
