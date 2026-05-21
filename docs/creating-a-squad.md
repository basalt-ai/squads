# Creating an Agent Squad

This guide walks through building a complete, valid Agent Squad bundle — the unit the
Pancake marketplace ingests. By the end you'll have a bundle that passes the validator
and is ready to publish.

If you're using Claude Code in this repo, the **`create-squad` skill** automates every
step below. This guide is the canonical manual reference — what the skill does, and what
you need to know if you're building by hand.

## 1. Prerequisites

You don't need to read anything else first, but it helps to know:

- **What a squad is.** A squad is one or more *focused, single-lane* sub-agents that get
  installed into a Pancake pod and run autonomously — finding leads, monitoring metrics,
  publishing content, whatever the squad is built for. Each agent reports to the user's
  co-founder agent; the user never talks to a squad agent directly.
- **The runtime.** Squads run on OpenClaw inside a Pancake pod. Each agent wakes on a
  *heartbeat* (e.g. every 2 hours, or daily) and runs a procedure you write.
- **The file contract.** A bundle is a directory of files with a specific shape. The
  validator enforces that shape; the marketplace re-checks it on ingest.

The skeleton bundle [`template/`](../template/) is the canonical living example — every
file the contract permits is present with placeholder content. Walking it is the fastest
way to learn the layout.

## 2. Squad anatomy

A bundle's directory layout, with required (✔) and optional (·) members:

```
manifest.json                  ✔  package descriptor (name, version, agents list, …)
SQUAD.md                       ✔  marketplace catalog card (frontmatter + Markdown body)
ONBOARD.md                     ✔  onboarding script the co-founder runs after deploy
MEMORY.md                      ·  squad-wide seed memory
skills/<name>.md               ·  squad-wide skills (every agent receives a copy)
TOOLS.md                       ·  optional documentation of the squad's tool surface
agents/<agent-id>/
  agent.json                   ✔  per-agent runtime config (model, heartbeat, skills)
  IDENTITY.md                  ✔  who the agent is (name, role, scope)
  SOUL.md                      ✔  how the agent behaves (personality, principles)
  HEARTBEAT.md                 ·* the wake procedure (required when heartbeat is declared)
  MEMORY.md                    ·  agent-specific seed memory (overrides squad-wide)
  skills/<name>.md             ·  agent-specific skills
crons/jobs.json                ·  native OpenClaw cron jobs
```

A few things the validator *forbids* inside a bundle:

- `AGENTS.md`, `USER.md`, `BOOTSTRAP.md`, `BOOT.md` — these are pod-level files managed
  by Pancake Cloud, not by squads. Don't ship them.
- `token_intensity` in `manifest.json` or `SQUAD.md` frontmatter — deprecated. Pancake
  Cloud computes token usage automatically.
- `tasks/` directory — squads do not ship task templates. Ad-hoc work is dispatched at
  runtime via the co-founder.

For the exact file-by-file contract see [`bundle-reference.md`](./bundle-reference.md).

## 3. Step-by-step walkthrough

### 3.1 Copy the template

```sh
cp -R template squads/<your-squad-name>      # if you're contributing to this repo
# or, for a self-hosted bundle, copy the template's contents to your own repo's root
```

Every file under `template/` is a complete, valid example with `<!-- TODO -->` comments
and placeholder content. Your job over the next steps is to replace all of it.

### 3.2 Fill `manifest.json`

```json
{
  "name": "your-squad-name",
  "version": "0.1.0",
  "description": "One sentence on what installing this deploys.",
  "author": "your-github-handle",
  "license": "MIT",
  "skills": ["skills/your-shared-playbook.md"],
  "agents": ["your-agent-id"],
  "required_identities": [
    { "site": "github.com", "reason": "why the agent needs github connected" }
  ],
  "required_vault_secrets": [
    { "key": "team.your_setting", "label": "Prompt shown to the user", "type": "string" }
  ],
  "required_tool_permissions": ["web_search", "web_fetch", "message"],
  "min_pancake_version": "1.0.0"
}
```

- `name` must be globally unique, kebab-case, ≤ 64 chars.
- `version` follows semver. Start at `0.1.0`; bump on every release.
- `description` is the one-line catalog card subtitle, ≤ 200 chars.
- `agents` is a string array of kebab-case agent ids — each id must have a matching
  `agents/<id>/agent.json` file (next step).
- Delete every optional section your squad doesn't use. The validator complains about
  empty values, not absent fields.

### 3.3 Write each agent's `agent.json`

For every id in `manifest.agents`, create `agents/<id>/agent.json`:

```json
{
  "id": "your-agent-id",
  "description": "One line on what this agent owns.",
  "model": "sonnet",
  "heartbeat": { "every": "daily" },
  "skills": ["agents/your-agent-id/skills/your-skill.md"]
}
```

This file is the bundle's slice of OpenClaw's agent runtime config. See
[*4. agent.json reference*](#4-agentjson-reference) below for every field.

### 3.4 Write `IDENTITY.md`, `SOUL.md`, and `HEARTBEAT.md`

For every agent:

- **`IDENTITY.md` — who the agent is.** A header (Name, Role, Scope, Created by) plus
  *What I Do*, *What I Don't Do*, *KPI / Goal*, *How To Reach Me*, *Voice / Personality*.
  Mirror [`template/agents/example-agent/IDENTITY.md`](../template/agents/example-agent/IDENTITY.md).

- **`SOUL.md` — how the agent behaves.** Personality, *Operating Principles*,
  *Escalation Rules*, *Boundaries (Inviolable)*, *What Success Looks Like*. Mirror
  [`template/agents/example-agent/SOUL.md`](../template/agents/example-agent/SOUL.md).

- **`HEARTBEAT.md` — the wake procedure.** *Required* when `agent.json#/heartbeat` is set.
  See [*6. HEARTBEAT.md contract*](#6-heartbeatmd-contract) below.

### 3.5 Write the skills

For every entry in `manifest.skills` (squad-wide, under `skills/`) and every entry in any
`agent.json#/skills` (agent-specific, under `agents/<id>/skills/`), create the referenced
file in **SKILL.md format**:

```markdown
---
name: my-skill
description: One or two sentences on what the skill does and when to load it.
---

# My skill

…the procedure, written as steps…
```

A skill is a *procedure*, not reference docs. Squad-wide skills are duplicated into every
agent at install; agent-specific skills are deployed only into that agent.

### 3.6 Write `SQUAD.md` and `ONBOARD.md`

**`SQUAD.md`** is the marketplace catalog card. Frontmatter is minimal:

```yaml
---
tags: [growth, gtm, content]
preview_image: https://example.com/your-squad-avatar.png  # optional
---
```

`tags` drives catalog filtering; `preview_image` is the squad's avatar URL.

**The body of `SQUAD.md` is the catalog's source of truth for per-agent prose** — describe
each agent here in user-facing language, not in `manifest.json`. Recommended sections:
*What this squad does*, *What you'll need*, *What you get*, *How it works*.

**`ONBOARD.md`** is a **script the co-founder agent executes** after the mechanical deploy.
See [*5. ONBOARD.md contract*](#5-onboardmd-contract) below.

### 3.7 Optional: add `crons/jobs.json` and `MEMORY.md`

- `crons/jobs.json` for native OpenClaw cron jobs. Each job's `sessionTarget` must be an
  agent id declared in your own `manifest.agents`. A cron run with nothing to report must
  reply with the literal token `NO_REPLY`.
- A squad-wide `MEMORY.md` if multiple agents share the same seed pointers.

### 3.8 Strip every placeholder

The template ships with `<!-- TODO -->` comments and placeholder prose. The validator
errors on any unresolved TODO marker outside `template/` itself, so strip them all before
publishing.

### 3.9 Validate

```sh
node scripts/validate.mjs squads/<your-squad-name>     # in this repo
# or in a self-hosted repo:
node validate.mjs .
```

Fix every error and re-run until clean. The validator mirrors marketplace ingestion
exactly — a green local run means a green ingest.

## 4. `agent.json` reference

Every field accepted in `agents/<id>/agent.json`:

| Field | Type | Required | Rules |
|---|---|---|---|
| `id` | string | ✔ | kebab-case. Must match the directory name and the `manifest.agents` entry. |
| `description` | string | ✔ | Non-empty. One-line role description. |
| `model` | string | · | Enum: `haiku` \| `sonnet` \| `opus`. Defaults to the pod default (`sonnet`). |
| `heartbeat` | object | · | `{ "every": "15m" \| "30m" \| "2h" \| "daily" }`. Object shape mirrors OpenClaw's `agents.list[].heartbeat`. Defaults to the pod default (`{ every: "2h" }`). When set, `agents/<id>/HEARTBEAT.md` must exist. |
| `skills` | string[] | · | Bundle-relative paths to this agent's skill files. |
| `contextInjection` | string | · | Enum: `always` \| `continuation-skip` \| `never`. Pod default applies when omitted. |
| `bootstrapMaxChars` | integer | · | Positive integer. OpenClaw bootstrap budget. |
| `params` | object | · | Free-form provider params passed through to OpenClaw (e.g. `{ "cacheRetention": "1h" }`). |

Unknown fields are rejected — the validator does not silently drop them. If you need a
new OpenClaw field, add it to the schema first.

## 5. `ONBOARD.md` contract

`ONBOARD.md` is **a runnable script, not a README.** The co-founder agent executes it
verbatim after the squad is deployed — collecting secrets, connecting identities, seeding
the agent's MEMORY, and dispatching the first task. It must finish within
`estimated_setup_minutes`; if it can't, shorten it.

Frontmatter:

```yaml
---
required_tools: [vault_request, browser_identity_add]
required_identities:
  - { site: github.com, reason: "push generated PRs" }
estimated_setup_minutes: 5
---
```

The body, in the imperative addressed to the co-founder:

- **Ask the user** the questions needed to configure the squad. Group them — don't ping-pong.
- **Collect secrets only via `vault_request`.** Never have the co-founder ask for a secret
  in plain chat. Even non-sensitive setup values declared in `required_vault_secrets` go
  through the vault.
- **Connect identities via `browser_identity_add`,** reusing an existing pod identity when
  one matches the `site`.
- **Save answers** to the agent's `MEMORY.md` (or a wiki page the MEMORY indexes).
- **Create the first task** with `create_task` and dispatch it immediately — unless you
  add `dispatch: later` to defer the first run to the agent's heartbeat.

## 6. `HEARTBEAT.md` contract

OpenClaw loads `agents/<id>/HEARTBEAT.md` on **every wake** — both heartbeat pulses and
dispatched tasks. This is the right home for the procedure the agent runs each tick;
keeping it out of `SOUL.md` (which is for behavioural rules) and out of `MEMORY.md`
(which is an index of pointers) lets you iterate on the procedure without touching the
agent's personality.

Write it in the imperative, addressed to the agent. A solid structure:

1. **The non-negotiable** — at least one task must be **executed** before the session
   closes. A wake is "orient, find the highest-leverage action in the lane, do it, file
   the result" — not "orient and `NO_REPLY`". `NO_REPLY` is only acceptable when nothing
   is actionable, and the reason must be logged to `memory/YYYY-MM-DD.md` first.
2. **Orient** — read `MEMORY.md`, skim recent daily logs, call `list_tasks`.
3. **Decide what this wake is for** — a dispatched task, a recurring duty, a draft to
   advance, or genuinely nothing (with a logged reason).
4. **Recurring duty** — the agent's specific heartbeat work and its cadence.
5. **Execute** — actually produce the artifact; don't just plan.
6. **Digest** — before closing the session, append a one-paragraph digest to
   `memory/YYYY-MM-DD.md`: *what you did, what changed, what's still open, and the single
   first move for the next wake.*
7. **Close the loop** — `complete_task` / `fail_task`, surface blockers.

## 7. Authoring principles

The contract tells you what's valid. This tells you what's good.

- **One agent, one lane.** A squad agent is a focused specialist. If you're tempted to
  make an agent do two unrelated things, that's two agents — or the second thing belongs
  to the user's co-founder, not to a squad.

- **Default to autonomous execution.** Squad agents do the work end to end and report
  back with a digest — they don't pause mid-task to ask the user "is this OK?". Escalate
  only in the narrow cases that genuinely need a human: out-of-scope work, hard blockers,
  irreversible commitments, or user-facing decisions. Everything else, the agent decides
  and ships.

- **Track work in the tasks system, not in markdown.** Pancake's tasks plugin is the
  shared store every agent reads and writes through `list_tasks`, `create_task`,
  `complete_task`, `fail_task`. Don't maintain parallel to-do lists or kanban tables
  in `.md` files — the task tools own state. Daily memos (`memory/YYYY-MM-DD.md`) are
  for context and decisions, not for ticket tracking.

- **Name agents by their job, not with a persona.** `Outreach agent`, `GEO audit agent`,
  `Content writer` — not personal names like `Atlas` or `Nova`. The user already has a
  named co-founder; sub-agents are specialists, and a job-shaped name makes the lane
  obvious at a glance.

- **Prefer fewer agents.** Sub-agents report to the co-founder, never to each other —
  they don't share context. If agent B needs data agent A produced, the co-founder has
  to relay it. Only split when work is genuinely distinct: different cadence, different
  skills, different identities. When in doubt, one agent.

- **Heartbeat first, cron only when timing matters.** A heartbeat is a state-driven
  trigger — the agent wakes on its pulse and decides what to do. A cron is clock-driven:
  it fires at an exact time with a hard-coded instruction. Reach for a cron only when the
  time itself matters to someone outside the agent — an 18:00 PT end-of-day report, a
  Monday-morning digest. Otherwise raise the heartbeat.

- **Crons stay quiet unless something changed.** A scheduled run with nothing to report
  must reply with the single literal token `NO_REPLY`. A chatty cron that posts "nothing
  changed" every day trains the user to ignore it.

- **`MEMORY.md` is an index, not a notebook.** One-line pointers only. Detailed findings
  go to the shared wiki.

- **Wake procedure in `HEARTBEAT.md`, behaviour in `SOUL.md`, pointers in `MEMORY.md`.**
  Three files, three concerns. Burying wake steps in `SOUL.md` or pointers in
  `HEARTBEAT.md` makes both hard to maintain.

## 8. Testing your squad

The validator is your test suite. It's a zero-dependency Node.js script that mirrors the
marketplace's ingest checks exactly — a clean local run means a clean ingest.

```sh
node scripts/validate.mjs                       # every bundle in the repo
node scripts/validate.mjs squads/your-squad     # one bundle
```

It exits non-zero on any error; warnings (e.g. a missing `tags` line) never fail the run.
The CI workflow in this repo runs the same command on every push and PR, so a passing
local run means a passing CI run.

While iterating, **re-run the validator after every batch of edits** — it catches
unresolved TODO markers, forbidden filenames, schema drift, and broken file references
that compound if left until the end.

## 9. Publishing

How a finished bundle gets into the marketplace.

### Official squads (the Pancake team)

Official squads live in the [`squads/`](../squads/) directory of this repo. To publish:

1. Add the bundle as `squads/<name>/`.
2. Run `node scripts/validate.mjs` and confirm clean.
3. Update the squad table in [`README.md`](../README.md).
4. Open a PR. CI must be green.
5. On merge, the marketplace re-seeds and the squad appears in the catalog.

Bump `version` (semver) on every release.

### Self-hosted squads (external authors)

This repo is Pancake-curated and not open to outside PRs. External authors **self-host**:
keep the bundle in a public GitHub repo of your own, with the bundle's files (`manifest.json`,
`SQUAD.md`, `agents/`, …) at the **repo root** — no `squads/` nesting.

To submit, send your repo URL and the tag you want ingested to the Pancake team. A
self-serve submission flow is planned but not yet available. Full details:
[`publishing.md`](./publishing.md).

---

That's the whole contract. The fastest way to confirm you've got it right is to copy
[`template/`](../template/), fill it in, and run the validator. When it exits 0, you
have a publishable squad.
