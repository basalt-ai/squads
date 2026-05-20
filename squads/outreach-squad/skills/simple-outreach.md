---
name: simple-outreach
description: The foundation outreach playbook. Run this first. Covers ICP definition, signal-based lead finding (4/day), LinkedIn sequence execution (4-5 touchpoints), reply handling with the qualify-first framework, and weekly A/B testing. Default mode for all new deployments.
---

# Simple Outreach

The foundation. Run this before anything else. Default mode — start here regardless of company stage.

---

## Step 1 — Confirm the ICP

Before finding a single lead, confirm ICP is defined in MEMORY.md. If it's missing or stale, ask the co-founder for confirmation before proceeding. If the co-founder doesn't know, infer the most logical ICP from what you know about the company's product and customers, propose it in the digest, and proceed.

ICP fields required:
- Role (who feels the pain)
- Industry / vertical
- Company size range or funding stage
- Trigger event (what makes them ready to buy)
- Core pain the product solves
- Anti-ICP (who never to contact)

---

## Step 2 — Find 1–3 Leads Per Day (target: 4/week, up to 20/week)

Quality over volume. One perfect lead beats ten mediocre ones.

### Option A — Signal-based (default, higher conversion)

Use Exa (built-in, no API key needed) for semantic LinkedIn profile search first. Then check these signals in order of availability:

1. Liked/commented a competitor's post or relevant influencer post → use Jungler (if available) or manual LinkedIn check
2. Starred a GitHub repo in the product category → GitHub public stars API (free, no auth for public repos)
3. Company is hiring for a role that signals the pain → LinkedIn Jobs search (free)
4. Company just raised funding → Crunchbase free search (or API if key available)

One signal per campaign. Never mix signals in a single sequence.

### Option B — ICP search (when no signal available)

Use Exa for semantic LinkedIn search with the ICP criteria (role + industry + company size). Fall back to direct LinkedIn search. Minimum: role + company size + industry must all match exactly. No fuzzy matches.

### Lead qualification bar

Before adding to the pipeline, verify:
- Role matches ICP exactly
- Company size/stage within range
- Not already in pipeline (deduplicate: scan the **Pipeline → Active leads** and **Closed leads** tables in `MEMORY.md` for a matching LinkedIn URL)
- Not on the anti-ICP list
- Not already a customer

Once qualified, append a row to **Pipeline → Active leads** in `MEMORY.md`:
`[Name] | [Company] | [LinkedIn URL] | [channel] | [signal] | queued | — | [today] | —`

---

## Step 3 — Execute the Sequence

Minimum 4 touchpoints, maximum 5. Run each touch on its scheduled day — do not skip, do not batch.

**Touch 1 — Connection request** (Day 1):
Send with no message attached. No message = better acceptance rate. Just the request.
If the tool (Heyreach/Lemlist) is available, load the request there. Otherwise use the browser (LinkedIn identity) directly.
After sending, update the lead's row in `MEMORY.md`: `Stage: connection_sent | Last touch: [today] | Next due: [today + 2 days]`

**Touch 2 — First DM after acceptance** (Day 2–3 after acceptance):
Open with curiosity and the signal. No pitch. No bullet points. No formatting.
> Hi [Name], noticed [signal]. Usually that means [pain] — is that the case for you?

Adapt the signal reference to what you actually found. Generic openers perform badly.
After sending, update the row: `Stage: dm_1_sent | Last touch: [today] | Next due: [today + 6 days]`

**Touch 3 — Follow-up #1** (Day 5–7 after Touch 2, if no reply):
New angle — insight, observation, or question. Can be as simple as:
> Hi, still very curious :)

Or a short relevant observation about something you found in their activity.

**Touch 4 — Follow-up #2** (Day 5–7 after Touch 3, if no reply):
Short. Genuine. Different angle again.
> Hi, I'd be happy to talk — even 10 min would help me understand your situation.

**Touch 5 — Breakup message** (Day 5–7 after Touch 4, if no reply):
Low pressure. Keeps the door open.
> Last ping, I promise. If [pain] ever becomes a priority, happy to connect then. Good luck with what you're building.

After the breakup, move the row from **Active leads** to **Closed leads** with `Result: no_reply | Date closed: [today]`. Never contact again from the same campaign.

---

## Step 4 — Handle Replies

**Rules (non-negotiable):**
1. Respond within 24h
2. Start with their point, not yours
3. Stay curious — ask, don't pitch
4. Never pitch the solution in a reply — the solution closes on the call
5. Don't propose a meeting until Q2 is answered
6. Max 40 words. End with a question or calendar link. Never a dead end.
7. Sign as the founder (first name only, no title)

**Qualification path:**

Q1 (current approach): "Are you using [tool in our category] or something else to [solve the pain]?"
Q2 (how bad): "How happy are you with [the outcome our product delivers]?"

Meeting comes after Q2 confirms the pain.

**Reply handling by intent:**

- "Not interested" → "Fair enough. Is it the timing, or is [pain] just not a priority right now?"
- "What do you do?" → "We help [ICP] [one-line outcome]. Does that sound like something you're wrestling with?"
- "Send more info" → "Happy to. Easiest if I walk you through it live. 15 min this week? [link]"
- "Maybe later" → "Understood. I'll ping you in [timeframe]. Good luck with the launch."
- Uses a competitor → "Makes sense. What's still frustrating you about it? That's usually where we fit in."
- Built internal tools → "Makes sense. Do you feel any bottleneck around [core pain]?"
- Not ICP → "Appreciate the honesty. Not the right fit right now — good luck!"

When a reply leads to a meeting, move the row from **Active leads** to **Closed leads** with `Result: meeting_booked | Date closed: [today] | Signal source: [source]`. The signal source is logged for KPI tracking.

---

## Step 5 — A/B Test Continuously

Run exactly two variants of the opening DM at all times. Pick the winner after 20+ replies on each variant. Kill the loser, introduce a new challenger.

Test order (highest leverage first):
1. Opening line / signal reference
2. First DM angle (pain vs. curiosity vs. insight)
3. Follow-up #2 angle
4. CTA wording

Log each test in MEMORY.md under Active A/B Test. Log the winner and learning in Weekly Learnings.

**Iteration triggers:**
- Reply rate <3% → opener is broken. Rewrite it immediately.
- Replies but all "not interested" → ICP is wrong. Surface to co-founder before continuing.
- Reply rate OK, no meetings → qualifier questions need work.
- "Not relevant" replies → signal source too broad.

---

## Step 6 — Mode Upgrade Decision

At every heartbeat, check upgrade conditions:
1. Reply rate >8% sustained for 2+ weeks
2. At least one tool beyond LinkedIn available (Heyreach or Lemlist confirmed in vault)

If both conditions are met: announce upgrade to Advanced mode in digest, load the `advanced-outreach` skill, and switch the mode flag in MEMORY.md. Do not wait for approval.

---

## KPI Benchmarks

- LinkedIn connection acceptance: >30%
- Reply rate: >8%
- Meetings booked per 20 leads: 1–2
- Email bounce rate (if used): <5%
