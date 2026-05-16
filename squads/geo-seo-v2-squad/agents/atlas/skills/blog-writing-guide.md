---
name: blog-writing-guide
description: Write, review, and improve blog posts for any company or founder following high-quality writing standards, authentic voice, and a credibility bar that earns real shares. Use this skill whenever someone asks to write a blog post, draft an article, review blog content, improve a draft, write a product announcement, create a founder story, or produce any written content for a company blog or business audience. Also trigger when the user mentions "blog post," "blog draft," "write-up," "announcement post," "founder story," "deep dive," or asks for help with content writing.

source: Adapted from the Sentry blog-writing-guide skill (https://github.com/getsentry/skills/blob/main/skills/blog-writing-guide/SKILL.md) by the Pancake team. Core principles, structure, and editorial standards are borrowed and modified for a broader, non-technical business audience.
---

# Blog Writing Skill

This skill enforces high-quality blog writing standards across every post — whether you're helping a founder write their first article, drafting a product update, or ghost-writing for a company.

**The bar:** Every post should be something the target reader would share with a peer, screenshot, or reference when making a real decision. If it wouldn't clear that bar, it belongs in a changelog or internal doc, not a blog.

---

## Step 0 — Know the Company Before You Write

Before drafting anything, establish:

- **Company name and one-liner** — what they do and for whom
- **Target reader** — who reads this blog? What do they care about? What do they distrust?
- **Voice brief** — how does the company sound? (3–5 adjectives + one anti-example)
- **Goal of this post** — awareness, SEO, credibility, conversion, or community?

If this information isn't provided, ask. Writing without it produces generic content that serves no one.

---

## Voice

Every company has a different voice. The skill adapts — but some things are universal:

*Sound like:* The smartest, most credible person in the room who also happens to be easy to talk to. Knowledgeable, specific, direct. Opinions welcome.

*Never sound like:* A press release. A corporate blog from 2015. An AI-generated summary. A LinkedIn thought-leadership post with five bullet points and a closing question.

Use "we" (the company) and "you" (the reader). This is a conversation, not a paper.

**Tone calibration by audience:**

| Audience | Tone |
|----------|------|
| Founders / operators | Direct, practical, no-hype, time-aware |
| Developers / engineers | Technical precision, honest trade-offs, no marketing fluff |
| Business / enterprise | Professional but not corporate, ROI-grounded |
| Consumers | Warm, relatable, benefit-first |

When in doubt: be more direct, less corporate.

---

## Banned Language

These phrases kill credibility in any industry. Never use them:

- "We're excited/thrilled to announce" — just announce it
- "Best-in-class" / "industry-leading" / "cutting-edge" — show, don't tell
- "Seamless" / "effortless" — nothing is
- "Empower" / "leverage" / "unlock" — say what you actually mean
- "Robust" — describe what makes it robust
- "Streamline" — everyone claims this, no one believes it
- "At [Company], we believe..." — just state the belief
- Filler transitions: "That being said," "It's worth noting that," "At the end of the day," "Without further ado"
- "In this post, we will explore..." — just start
- "Game-changing" / "revolutionary" / "transformative" — overused to the point of meaninglessness
- "AI-powered" as a standalone claim — explain what the AI actually does

---

## The Opening (First 2–3 Sentences)

The opening must do one of two things: *state the problem* or *state the conclusion*. Never start with background, company history, or hype.

**Good (problem first):**
> "We spent three months building a feature our users never asked for. Here's the process failure that made it happen — and how we caught it too late."

**Good (conclusion first):**
> "Hiring your first sales rep before product-market fit is almost always the wrong call. Here's what we did instead."

**Bad:**
> "At [Company], we've always believed in putting customers first. Today, we're excited to share some updates we think you'll love."

---

## Structure: Follow the Reader's Questions

Structure every post around what the reader is actually wondering, not the company's internal narrative:

1. **What problem does this solve for me?** (1–2 paragraphs — be concrete, name the pain)
2. **How does it actually work?** (The mechanism, not the marketing copy — specific enough that the reader can explain it to someone else)
3. **What are the trade-offs or alternatives?** (This separates useful content from promo copy)
4. **What should I do next?** (Concrete, actionable, low-friction)

For founder stories and behind-the-scenes posts, also address:
5. **What didn't work?** (Builds trust faster than any feature claim)
6. **What would you do differently?** (Shows intellectual honesty)

---

## Formatting for Skimmability

People skim. Earn every line.

**Break paragraphs at contrast points.** When a sentence introduces a "but," "however," or shifts perspective, start a new paragraph. Don't bury the turn inside a block of text.

**Bad:**
> Most companies try to build brand awareness before they've nailed their ICP. The instinct is understandable. But spending on awareness before you know who you're talking to is just burning money.

**Good:**
> Most companies try to build brand awareness before they've nailed their ICP. The instinct is understandable.
>
> But spending on awareness before you know who you're talking to is just burning money.

**One idea per paragraph.** If a paragraph covers two distinct points, split it. Three-sentence paragraphs are fine. One-sentence paragraphs are fine for emphasis.

**No em dashes.** Use commas, periods, or line breaks instead.

**Use concrete numbers whenever possible.** "47% of sessions ended at checkout" beats "a significant drop-off at checkout."

---

## SEO and GEO for Blog Content

Blog posts are dual-purpose: they need to rank on Google *and* get cited by AI engines (ChatGPT, Perplexity, Claude, Gemini, Google AI Overviews). Traffic from AI citations converts 4x better than organic Google traffic — treat GEO as equal priority.

### Structure for both Google and AI

**Lead generic, close specific.** The first 50–60% of the post should be audience-agnostic educational content — the problem space, the concepts, the landscape. Introduce the company's product or approach in the second half. Google ranks guides higher than product pages for informational queries. AI engines cite the same guides when answering user questions.

**Open with a direct 2–4 sentence answer, then a TL;DR.** Over 80% of AI citations come from the first third of a page. If the answer isn't up top, AI skips it. Put the clearest version of your conclusion in the first paragraph, then back it up.

**Put keywords in H2s.** Generic headings are invisible to search. "How to reduce customer churn in SaaS" beats "Our approach." Make headings useful to someone scanning — AI engines extract H2s when building summaries.

**Include a definitional section.** For any concept you're introducing, include a plain "What is X?" section — even if it feels basic. Top-ranking pages almost always have one. AI uses these definitional sections heavily when answering "what is" queries.

**Add an FAQ section at the bottom of every post.** 3–4 questions targeting long-tail queries. These win featured snippets, People Also Ask boxes, and are directly extracted by AI engines when answering similar questions. Use natural language in the questions — write how people actually speak, not how they search.

**Use comparison tables for head-to-head content.** "X vs Y" and "Best X for Y" formats are among the most-cited content types by AI engines. Tables are highly extractable. If the post compares options, put the comparison in a table.

**Tables must use GFM (GitHub Flavored Markdown) syntax** — standard pipe tables (`| col | col |` with `|---|---|` separator row). The Pancake blog uses `react-markdown` with the `remark-gfm` plugin, which is required for table rendering. Without it, pipe syntax renders as plain text. If tables appear broken, verify `remark-gfm` is installed in `package.json` and `remarkPlugins={[remarkGfm]}` is passed to `<ReactMarkdown>` in `app/blog/[slug]/page.tsx`.

### Quality signals AI engines look for

**Replace vague claims with hard numbers.** "Many users prefer" → "71% of users in our cohort." AI engines weight specificity heavily when deciding what to cite.

**Include named expert quotes with credentials.** Anonymous assertions get ignored. Named sources with context ("according to [Name], [Title] at [Company]") get cited.

**Include at least one original data point or framework.** Something that can't be found anywhere else — a stat from your own users, a framework you developed, a finding from your own testing. This is the single biggest GEO differentiator.

**Display a clear "Last Updated" date on every article.** Updated content gets significantly more AI citations. Even minor content refreshes justify updating this date.

**Use natural language, not keyword strings.** AI engines are trained on conversational text. "How do founders hire their first ops person" outperforms "founder ops hiring guide" as a framing.

---

## AI Writing Patterns to Avoid

LLM-generated prose has tells. Flag and rewrite these:

**Staccato dramatic fragments.**
- Bad: "No leads. No pipeline. No revenue."
- Good: "We had no leads, no pipeline, and no revenue going into Q3."

**Bumper-sticker aphorisms.**
- Bad: "You can't grow what you can't measure."
- Good: "Without tracking activation rate, we had no idea which onboarding changes were actually working."

**Three-beat reveals.**
- Bad: "Not a product problem. Not a pricing problem. A positioning problem."
- Good: "It wasn't a product or pricing problem. We had a positioning problem."

**Smug simplicity.**
- Bad: [screenshot] "That's it. That's all you need."
- Good: [screenshot] then explain what it does or why it matters.

**Personality only in the bookends.** AI drafts open with a personal anecdote, go impersonal for 80%, then close with a CTA. The author's voice should persist throughout.
- Bad: Personal intro → clinical middle → "Sign up free."
- Good: First-person asides woven throughout: "this is where we made the expensive mistake" / "I would have gotten this wrong six months ago."

---

## Section Headings Must Convey Information

**Weak:** "Background," "Our Approach," "Results," "Conclusion"

**Strong:** "Why we stopped A/B testing our pricing page," "The onboarding step that doubled week-2 retention," "Where this breaks down: the ICP edge case"

---

## Content Quality Standards

**Numbers over adjectives.** If you make a claim, include the number.
- Bad: "This significantly improved our conversion rate."
- Good: "This improved our trial-to-paid conversion from 6% to 11% in 60 days."

**Every post must include at least one image.** A post without a hero image gets skipped in feeds, social previews, and newsletter digests. Minimum: one image per post, ideally one every 300–500 words for longer pieces.

**Image types in order of preference:**
1. Real product screenshots (annotated to show what matters)
2. Original diagrams or charts built from your own data
3. Process illustrations (flowcharts, before/after comparisons)
4. High-quality contextual photography — only if nothing above is available

Never use generic stock photos (handshakes, lightbulbs, people staring at laptops). They signal low-effort content immediately.

**Image SEO and performance — non-negotiable:**
- *Format:* WebP for all images. Fallback to JPEG for older CMS compatibility. Never PNG for photos.
- *File size:* Hero images under 150KB. Inline images under 100KB. Use a compressor (Squoosh, TinyPNG, or equivalent) before uploading — uncompressed images are one of the top PageSpeed killers.
- *Dimensions:* Hero images at 1200×630px (also the Open Graph standard for social sharing). Inline images sized to their actual display width — never upload a 3000px image that renders at 800px.
- *Alt text:* Every image needs a descriptive alt tag. Write what the image actually shows — not "image1.png" or keyword-stuffed strings. Alt text is both an accessibility requirement and a minor SEO signal.
- *File names:* Use descriptive, hyphenated filenames before uploading: `churn-rate-dashboard-screenshot.webp`, not `IMG_4521.png`. Crawlers read file names.
- *Lazy loading:* Ensure images below the fold use `loading="lazy"` — most modern CMS platforms do this by default, but verify.

**Compression workflow (run before every upload):**
1. Convert to WebP using `ffmpeg -i input.png -vf "scale='min(1200,iw)':-1" -q:v 85 output.webp` or Squoosh.app
2. Verify hero images are under 150KB and inline images under 100KB after conversion
3. If still over limit, reduce `-q:v` to 75 or scale down with `-vf "scale='min(900,iw)':-1"`
4. Reference the `.webp` file in the post — not the original PNG or JPEG

**Visuals must show real things.** Screenshots, diagrams, and charts should be real — not stock or illustrative. Annotate them to show what matters.

**Honesty over hype.** Never overstate what a product does. Acknowledge limitations. If something is in beta or has rough edges, say so. If a competitor does something well, it's fine to note it. Credibility compounds; hype erodes it.

**Cite your sources.** If you reference data, link to it. If you're sharing a personal experience, own it explicitly: "in our experience," "when we tested this with 50 users."

---

## After Publishing: GEO Distribution

Writing a great post is half the job. Where it lives and who links to it determines whether AI engines cite it.

**Republish on high-authority platforms.** Publish structured versions of your post on Medium, Substack, and Quora. These platforms already have domain authority AI engines trust. Put the company at the top of any "best of" or comparison framing.

---

## Title Guidelines

The title is the highest-leverage sentence in the post. It must stop the target reader mid-scroll.

**Strong titles** make a specific claim, tell a story, or promise a specific payoff:
- "We killed our freemium tier. Signups dropped 30% and revenue went up."
- "Why we stopped hiring AEs and started closing deals ourselves"
- "The email sequence that reactivated 18% of churned users"
- "Our onboarding had a 40% drop-off at step 3. Here's what fixed it."

**Weak titles** are vague announcements:
- "Introducing our new onboarding flow"
- "How we're improving the customer experience"
- "Exciting updates to our platform"

---

## Post Types

| Type | Goal | Voice |
|------|------|-------|
| Founder / Operator Story | Share a real decision with trade-offs so peers learn | First person, specific, honest |
| Product Update | Explain what shipped, why it matters, how to use it | Practical, concrete, no hype |
| "How We Did X" | Show the mechanics of running something at a specific stage | Detailed enough to replicate |
| Opinion / Take | A specific, defensible point of view on an industry question | Direct, opinionated, willing to be wrong |
| Data / Research | Original insight from the company's unique position or data | Factual, sourced, let numbers do the work |
| Tutorial / Guide | Help the reader accomplish something specific | Step-by-step, honest about prerequisites |
| Postmortem | Transparent failure analysis with timeline and lessons | Candid, specific, no blame-deflection |

---

## The "Would I Forward This?" Test

Before publishing, ask: Would the target reader forward this to a peer? Does it have a shot at getting shared in a relevant Slack group, subreddit, or industry newsletter? If the answer is no, the post either needs more depth, more original insight, or it belongs in a changelog.

Posts worth sharing contain at least one of:
- A real decision explained with trade-offs (not just the winning option)
- Original data or insight not found elsewhere
- A story with specific details — numbers, timelines, context
- An honest accounting of something that went wrong
- A how-to that saves the reader real time or a real mistake

---

## Non-Negotiables (Quick Reference)

1. Never publish without a real person's name on it. No "[Company] Team" bylines.
2. Never say "we're excited to announce." Just announce it.
3. If you make a performance or efficiency claim, include the number.
4. If you describe a decision, explain what you didn't choose and why.
5. Every post must have a clear "who is this for" before writing starts.
6. Changelogs belong in the changelog. Blog posts should offer something more.
7. When in doubt, go deeper. Too shallow is a far greater risk than too detailed.
8. Write the post the reader wishes existed when they were dealing with this problem.
9. No AI-sounding prose. If it reads like it was generated, rewrite it.
10. Don't pad for length. A great 400-word post beats a mediocre 1,500-word post.

---

## When Reviewing or Editing a Draft

**Content Review:**
- Does the opening hook land within 2 sentences?
- Is it clear who this is for?
- Are claims backed by specific numbers or examples?
- Does it pass the "would I forward this?" test?
- No corporate language, filler, or AI-pattern prose?
- Do headings convey information?
- Is it the right length — not padded, not too thin?
- Is the title specific and compelling?

**Accuracy Review:**
- All claims accurate and honest?
- Visuals show real product/data?
- Numbers and benchmarks correct?
- Limitations acknowledged where relevant?

**Final Check:**
- Author byline is a real person's name
- Links to relevant docs, signup, or next step included
- Post doesn't duplicate what's already in the changelog
- Source attribution present if content is adapted or based on external research
- Article schema implemented (author, publish date, last updated date)
- "Last Updated" date visible on page
- TL;DR or direct answer in the first 2–4 sentences

When providing feedback: quote the weak passage, explain why it's weak, and rewrite it to show the standard.
