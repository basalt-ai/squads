---
name: advanced-outreach
description: Scale layer for when Simple Outreach is working (reply rate >8%, 1-2 meetings/20 leads). Adds signal stacking (only reach out when 2+ signals converge), multichannel orchestration (LinkedIn + email + warm call + WhatsApp reactivation), and warm lead generation from post engagers. Load only after upgrading from Simple mode.
---

# Advanced Outreach

*Load this skill only after Simple Outreach is working: reply rate >8%, 1–2 meetings per 20 leads sustained for 2+ weeks. If you're below that bar, go back to `simple-outreach.md`.*

Advanced adds three layers on top of Simple Outreach — it does not replace it.

---

## Layer 1 — Signal Stacking

In Simple mode, one signal is enough. In Advanced, wait for signals to stack.

**The principle:** don't ask "does this company fit my ICP?" Ask "is this company visibly changing right now?" A mid-market company with 3 converging signals = high urgency. A company that technically fits but has no momentum = low conversion.

### Signal categories

Track these across sources:
- Organizational momentum: new hires in relevant roles, promotions, team growth, funding rounds
- Tech stack signals: competitor tool in stack, recent tool change, job posts mentioning your category
- Engagement signals: liked/commented your content, competitor content, or relevant influencer posts
- Intent signals: G2 reviews, competitor mentions, website engagement (if tracking available)
- People movement: past users resurfacing at new companies, champion moved to a new org

### Stacking rule

Only queue an outreach when 2+ signals converge on the same company or person.

Priority tiers:
- High (3+ signals): reach out within 48h
- Medium (2 signals): reach out this week
- Low (1 signal): add to Simple Outreach queue instead

### Signal sources

- Post likers/commenters: Jungler (requires API key in vault)
- Tech stack + hiring: Sumble, BuiltWith (web search fallback if no API)
- Funding + firmographics: Crunchbase (requires API key for automation, free search otherwise)
- GitHub engagement: GitHub public API (no auth needed for public repos)
- Semantic LinkedIn search: Exa (built-in, no key needed)
- Contact enrichment: FullEnrich (requires API key in vault)

Data hygiene: deduplicate across sources. Normalize company names. Enforce one-person-one-campaign rule.

---

## Layer 2 — Multichannel Orchestration

Simple uses LinkedIn only. Advanced uses LinkedIn + email + warm call + WhatsApp as an interconnected system.

**The principle:** make every call a warm call. Text first, call D+1/D+2. By the time you call, they've seen your name.

### Full sequence (8–12 touchpoints)

```
Day 1:   LinkedIn connection request (personalized, signal-referenced)
Day 2:   [If accepted] LinkedIn DM #1 (open question, no pitch)
         [Parallel] Email #1 (value-forward proof point)
Day 3-4: Warm call preparation (draft 30-60s opener, flag to co-founder for execution)
Day 5:   LinkedIn DM #2 (different angle)
Day 7:   Email #2
Day 9:   LinkedIn DM #3 or voice message note (pattern interrupt)
Day 12:  Email #3
Day 14:  LinkedIn breakup message
Day 21+: WhatsApp reactivation (mid-funnel only — if they engaged but went cold)
```

### Channel-specific rules

**LinkedIn DMs:**
- No bullet points, no formatting
- Voice message note in Day 9 is a strong pattern interrupt — use it
- Keep under 5 lines always

**Email:**
- Max 10 emails/day/inbox if using Heyreach or Lemlist
- Dedicated sending domain (not the main domain) — check vault for email config
- SPF, DKIM, DMARC confirmed before sending
- Open tracking OFF — optimize for replies, not opens
- Keep warmup running while sending

**Cold call:**
- Draft the 30–60s opener and route it to the co-founder to execute
- Opener always references the text touch: "I sent you a LinkedIn message earlier this week about X"
- Goal: qualify or book, not pitch

**WhatsApp:**
- Mid-funnel reactivation only. Never cold.
- Only if the lead provided a WhatsApp number and has already engaged somewhere else
- Voice notes work well — personal, hard to ignore
- Light value touch, not a pitch

### Tool routing

- If Heyreach key in vault: use for LinkedIn automation
- Else if Lemlist key in vault: use for LinkedIn + email automation
- Else: use browser (LinkedIn identity) for LinkedIn; draft emails for manual send

---

## Layer 3 — Warm Lead Generation from Post Engagers

The highest-converting leads are the ones who already engaged with your content.

**Workflow:**
1. Pull post engagers: use Jungler to extract everyone who liked or commented on the user's recent posts. Filter by ICP criteria (role, company size).
2. Score: apply signal stacking — people who engaged AND have another signal get priority.
3. Outreach opener references the post: "Saw you liked my post about [topic] — curious if that's something you're dealing with at [Company]."
4. Loop feedback: log which post topics drove the most ICP engagement. Surface to co-founder weekly to feed back into content planning.

If Jungler is not available: use competitor post likers as a proxy instead (manual LinkedIn check or Exa search).

Note: this layer assumes *someone* is creating content. If no content operation is running, skip Layer 3 and focus on Layers 1 and 2 until a Content Squad is deployed.

---

## Advanced A/B Testing

Beyond Simple's message testing, also run:
- Channel testing: LinkedIn DM vs. email — which gets more replies for this ICP?
- Timing testing: call D+1 vs. D+3 after text touch — which gets more pickups?
- Signal testing: which signal source produces the highest meeting conversion? (log by source)
- Voice note testing: does adding a LinkedIn voice note in Day 9 increase reply rate?

Log signal-to-meeting conversion by source in MEMORY.md. Double down on sources above 5% conversion. Cut sources below.

---

## KPI Benchmarks (Advanced)

- LinkedIn connection acceptance: >35%
- Reply rate (LinkedIn): >10%
- Reply rate (email): >6%
- Warm lead reply rate (post engagers): >20%
- Signal-to-meeting conversion: track by source, cut below 5%
- Meetings/week at scale: 10+

---

## Downgrade Trigger

If reply rate drops below 6% for 2 consecutive weeks after upgrading, downgrade back to Simple mode. Log the reason in Weekly Learnings. Announce the downgrade in the digest.
