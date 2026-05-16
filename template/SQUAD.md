---
name: example-squad
version: 0.1.0
description: "TODO: one-line description — keep it consistent with manifest.json"
author: pancake-official
tags: [example, growth]
token_intensity: medium
agents:
  - id: example-agent
    description: "TODO: one-line role description"
preview_image: https://example.com/example-squad-preview.png
---

<!-- TODO: SQUAD.md is the marketplace catalog card. The marketplace ingest
     reads ONLY `tags` and `token_intensity` from the frontmatter above — the
     rest (name/version/description/author/agents) is display duplication of
     manifest.json, so keep it consistent. `preview_image` is optional. The
     body below renders as the squad's store detail page. Strip every TODO
     before publishing. -->

## What this squad does

<!-- TODO: 2-3 sentences. What does installing this squad deploy, and what
     problem does it solve? Name each agent and the one job it owns. -->

TODO

## What you'll need

<!-- TODO: bullet list — identities to connect, secrets to provide, accounts
     the user must already have. Mark optional items as optional. -->

- TODO

## What you get

<!-- TODO: bullet list of the concrete, recurring outcomes the user receives. -->

- TODO

## How it works

<!-- TODO: a short paragraph on the operating rhythm — what runs on a cron or
     heartbeat, and how the user interacts with the squad through the
     co-founder (the user never talks to a squad agent directly). -->

TODO
