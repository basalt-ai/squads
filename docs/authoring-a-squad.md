# Authoring a squad

How to build an Agent Squad bundle, start to finish.

## Prerequisites

Read these first — this guide assumes them:

- [`how-squads-work.md`](./how-squads-work.md) — the concepts and the install lifecycle.
- [`bundle-reference.md`](./bundle-reference.md) — the exact file contract.

You build by copying [`template/`](../template/), the complete, valid skeleton bundle.

## Fast path — the `create-squad` skill

If you have this repo open in Claude Code, just run the **`create-squad`** skill. It loads
the contract, interviews you about the squad, scaffolds the bundle from `template/`, fills
every file, and runs the validator until clean. That is the recommended path.

The manual path below is the same process done by hand — useful to understand what the
skill does, or if you are not using Claude Code.

## Manual path

### 1. Copy the template

```sh
cp -R template squads/<your-squad-name>     # in this repo
# or, for a self-hosted repo, copy template/'s contents to your repo root
```

[`template/`](../template/) is a complete, valid skeleton. Every file has `<!-- TODO -->`
guidance comments and placeholder content — your job is to replace all of it.

### 2. Fill `manifest.json`

Set `name` (kebab-case, globally unique), `version` (start at `0.1.0`), `description`
(≤ 200 chars), and `author`. For each agent set `id`, `description`, and optionally `model`
and `heartbeat`. Declare `skills`, `required_identities`, and `required_vault_secrets` —
delete any optional section the squad does not use. Full field rules:
[`bundle-reference.md`](./bundle-reference.md#manifestjson--the-machine-readable-contract).

### 3. Write each agent's `IDENTITY.md`, `SOUL.md`, and `HEARTBEAT.md`

For every agent in `manifest.agents[]` there must be `agents/<id>/IDENTITY.md` and
`agents/<id>/SOUL.md`. `IDENTITY.md` is who the agent is; `SOUL.md` is how it behaves.
Follow the section structure in
[`bundle-reference.md`](./bundle-reference.md#identitymd-and-soulmd--per-agent).

For every agent that does recurring work on its heartbeat (most of them), write
`agents/<id>/HEARTBEAT.md` — the imperative, step-by-step procedure the agent runs on
every wake. OpenClaw loads it automatically. Keep it out of `SOUL.md` (which is for
behavioural rules) and out of `MEMORY.md` (which is an index of pointers). See
[`bundle-reference.md`](./bundle-reference.md#heartbeatmd--the-wake-procedure).

Optionally add a per-agent `agents/<id>/MEMORY.md`.

### 4. Write the skills

Write each skill file referenced by `manifest.skills[]` (squad-wide, under `skills/`) and
each `agents[].skills[]` path (agent-specific, under `agents/<id>/skills/`). Every skill is
SKILL.md format — frontmatter `name` + `description`, then a Markdown body that is a
*procedure written as steps*.

### 5. Write `SQUAD.md` and `ONBOARD.md`

`SQUAD.md` is the catalog card — frontmatter (keep `tags` and `token_intensity` accurate)
plus a body describing what the squad does, what the user needs, and what they get.

`ONBOARD.md` is a **script the co-founder executes** — see the guidance below.

### 6. Optionally add `crons/`, `tasks-config/`, `MEMORY.md`

Add `crons/jobs.json` for scheduled work, `tasks-config/templates.json` for dispatchable
task templates, and a squad-wide `MEMORY.md` for seed memory. All optional — delete the
template's copies if unused.

### 7. Validate

```sh
node scripts/validate.mjs squads/<your-squad-name>
```

Fix every error and re-run until clean. The validator mirrors marketplace ingestion, so a
clean run here means a clean ingest. Then publish — see [`publishing.md`](./publishing.md).

## Authoring guidance

The contract tells you what is *valid*. This tells you what is *good*.

- **One agent, one lane.** A squad agent is a focused specialist that reports to the
  co-founder — not a generalist. Give it one role with clear edges. If you're tempted to
  make an agent do two unrelated things, that's two agents (or the second thing belongs to
  the co-founder).

- **Default to autonomous execution.** A squad agent should do the work end to end and
  report back with a digest — not pause mid-task to ask the user "is this OK?". The user
  is a busy board member; their attention is the scarce resource, not the agent's tokens.
  Write `SOUL.md` so the agent escalates only in the narrow cases that genuinely need a
  human: out-of-scope work, hard blockers, irreversible/external commitments, or
  user-facing decisions. Everything else, the agent decides and ships. If you find
  yourself writing "ask the co-founder before X" for a reversible in-scope action,
  delete it.

- **Track work in the tasks system, not in markdown.** Pancake's tasks plugin is the
  shared SQLite store the co-founder and every sub-agent read and write through
  `list_tasks`, `update_task`, `complete_task`, `fail_task` — that's the single source
  of truth for queued / in-flight / blocked / done across the pod. Don't design a squad
  that maintains parallel to-do lists, kanban tables, or status trackers in `.md` files;
  use `tasks-config/templates.json` for dispatchable work and the task tools for state.
  Per-agent daily memos (`memory/YYYY-MM-DD.md`) are for context and decisions, not for
  ticket tracking.
- **Name agents by their job, not with a persona.** An agent's **Name** is a description of
  what it does — `Outreach agent`, `GEO audit agent`, `Content writer` — not a personal name
  like `Atlas`, `Nova`, or `Sage`. The user already has a named co-founder; sub-agents are
  specialists, and a job-shaped name makes the lane obvious at a glance. Personality lives
  in `SOUL.md`, not in the label.

- **Prefer fewer agents.** Sub-agents report to the co-founder, never to each other — they
  don't share context. If agent B needs data agent A produced, the co-founder has to relay
  it, which is slow and lossy. Only split work across two agents when it is genuinely
  distinct — different cadence, different skills, different identities, or work that would
  bloat a single agent's `MEMORY.md` and lane. When in doubt, one agent.

- **`ONBOARD.md` is a runnable script, not a README.** Write it in the imperative, to the
  co-founder: "Ask the user X. Store it with `vault_request` at key Y. Write it to the
  agent's `MEMORY.md`." It must finish within `estimated_setup_minutes` — if it can't,
  shorten it.

- **Collect secrets only via `vault_request`.** Never have the co-founder ask for a secret
  in plain chat. Route even non-sensitive setup values through the vault when they're
  declared in `required_vault_secrets`.

- **Heartbeat first, cron only when timing matters.** A heartbeat (`15m` | `30m` | `2h` |
  `daily`) is a state-driven trigger — the agent wakes on its pulse and decides what to do
  by consulting its identity, memory, and inbox. A cron is clock-driven: it fires at an
  exact time with a hard-coded `payload.text` that overrides that judgment with a specific
  instruction. Reach for a cron only when the time itself matters to someone outside the
  agent — an 18:00 PT end-of-day report, a Monday-morning digest. If you'd be happy with
  the work happening anytime in a window, raise the heartbeat. If a cron's `payload.text`
  just restates `IDENTITY.md`, delete it.

- **Crons stay quiet unless something changed.** A scheduled run that has nothing to report
  must reply with the single literal token `NO_REPLY`. A chatty cron that posts "nothing
  changed" every day trains the user to ignore it.

- **`MEMORY.md` is an index, not a notebook.** One-line pointers only. Detailed, growing
  findings go to the shared wiki — never appended to `MEMORY.md`.

- **Wake procedure goes in `HEARTBEAT.md`, not `SOUL.md` or `MEMORY.md`.** OpenClaw loads
  `HEARTBEAT.md` on every wake. Authors who bury wake steps in `SOUL.md`'s personality
  section or in `MEMORY.md` pointers end up with a procedure that's hard to find and hard
  to evolve. Keep `SOUL.md` for behaviour, `MEMORY.md` for pointers, and `HEARTBEAT.md`
  for the script.

- **Crons and tasks target only this squad's agents.** `sessionTarget` and `assigned_to`
  must name an agent in your own `manifest.agents[]`. The validator enforces this.

- **Strip every TODO.** The template is full of `<!-- TODO -->` comments and placeholder
  text. None of it should survive into a published bundle.

When your squad is valid and reviewed, publish it: [`publishing.md`](./publishing.md).
