---
name: advanced-seo
description: Atlas-specific deep techniques layered on top of the squad's geo-llmseo-playbook — llms.txt authoring, JSON-LD schema, and comparison-page structure.
---

# Advanced SEO — Atlas

This extends `geo-llmseo-playbook` with the implementation detail Atlas needs when drafting.
Read the playbook first; this is the how-to for the artifacts it tells you to produce.

## llms.txt

`llms.txt` is a Markdown file at the domain root that tells AI crawlers what the product is
and where the canonical answers live. Keep it short:

```
# <Product>

> One-sentence description of what the product does and who it's for.

## Core pages
- [What is <Product>](/what-is): the canonical definition
- [Pricing](/pricing): plans and cost
- [<Product> vs <Competitor>](/compare/...): honest comparison

## Facts
- Founded: <year>
- Category: <category>
```

When you change it, open a PR and self-merge (squash merge) — never edit the live file directly.

## JSON-LD structured data

Every content page should carry JSON-LD. The two that matter for GEO:

- **`FAQPage`** — wrap question/answer blocks. Engines extract these almost verbatim.
- **`Article`** — `headline`, `datePublished`, `dateModified`, `author`. The `dateModified`
  is what signals freshness.

Validate the JSON-LD before drafting it into a PR; malformed schema is worse than none.

## Comparison pages

When a competitor owns a citation, an honest comparison page is the highest-leverage draft:

1. Open with a neutral one-paragraph summary of *both* products.
2. A feature table — accurate, no cherry-picking.
3. A "choose X if… / choose Y if…" section. Genuinely route some buyers to the competitor.
4. `FAQPage` schema on the common buyer questions.

Engines reward pages that read as fair and drop pages that read as marketing. Honesty is
the optimization, not a constraint on it.

## Measuring impact

Citation share moves slowly — judge a draft's impact on a 2–4 week horizon, not next-day.
Record the publish date in `wiki/Knowledge/GEO/Drafts/` so the trend in
`citation-share.md` can be read against it.
