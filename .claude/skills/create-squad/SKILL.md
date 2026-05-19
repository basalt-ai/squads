---
name: create-squad
description: Author a new Agent Squad bundle — scaffold it from the template, interview the author, fill every file, and validate. Use when the user wants to create, build, or add a new squad (a deployable bundle of proactive Pancake sub-agents).
---

# Create a squad

Author a complete, valid Agent Squad bundle with the user. Work through the steps in order
— later steps depend on earlier ones.

## Step 1 — Load the contract

Before writing anything, read, in this order:

1. [`docs/bundle-reference.md`](../../../docs/bundle-reference.md) — the exact file contract.
2. [`docs/authoring-a-squad.md`](../../../docs/authoring-a-squad.md) — the build process and
   authoring guidance.
3. The skeleton bundle [`template/`](../../../template/) — every file. Your output mirrors
   its structure.

Do not invent the contract from memory — read these files.

## Step 2 — Interview the author

Ask the user what they want, and don't scaffold until you have answers for all of it:

- **The squad** — its purpose, and a kebab-case `name` (globally unique, ≤ 64 chars).
- **Each agent** — `id` (kebab-case), role / `description`, `model` (`haiku`/`sonnet`/`opus`),
  `heartbeat` (`15m`/`30m`/`2h`/`daily`). Keep each agent single-lane and focused.
- **Skills** — which are squad-wide (every agent gets them) vs agent-specific.
- **Required identities** — external sites the squad needs connected, each with a reason.
- **Required vault secrets** — each `{ key, label, type }`.
- **Crons** — any scheduled jobs, and what each one does.
- **Task templates** — manual and/or recurring tasks the co-founder can dispatch.
- **Catalog metadata** — `tags` and `token_intensity` (`low`/`medium`/`high`).

## Step 3 — Scaffold

Copy [`template/`](../../../template/) to `squads/<name>/`, then fill every file:

- `manifest.json` — all the answers from Step 2; delete optional sections the squad doesn't use.
- `agents/<id>/IDENTITY.md`, `SOUL.md`, and `HEARTBEAT.md` for every agent; add
  `agents/<id>/MEMORY.md` if useful. `HEARTBEAT.md` is the imperative wake procedure
  OpenClaw loads on every pulse — keep it out of `SOUL.md` (behaviour) and `MEMORY.md`
  (pointer index).
- Every skill file referenced by the manifest, in SKILL.md format (frontmatter `name` +
  `description`, then a procedure written as steps).
- `SQUAD.md` (catalog card) and `ONBOARD.md` (the onboarding script).
- Add or delete the optional `crons/jobs.json`, `tasks-config/templates.json`, and squad-wide
  `MEMORY.md` depending on Step 2.
- **Strip every `<!-- TODO -->` comment and placeholder** the template ships with.

> If this repo has no `squads/` directory — i.e. it is a third-party self-host repo — scaffold
> at the **repo root** instead of under `squads/<name>/`. See
> [`docs/publishing.md`](../../../docs/publishing.md).

## Step 4 — Bake in the conventions

- Each agent is a **focused, single-lane specialist** that reports to the co-founder — not a
  generalist.
- `ONBOARD.md` is a **runnable script** the co-founder executes: collect secrets via
  `vault_request`, connect identities via `browser_identity_add`, save answers to the agent's
  `MEMORY.md`, and create + dispatch a first task. It must fit `estimated_setup_minutes`.
- `MEMORY.md` is a **thin index of pointers**, never a notebook.
- `HEARTBEAT.md` is the **imperative wake procedure** OpenClaw loads on every pulse —
  not behaviour (that's `SOUL.md`), not pointers (that's `MEMORY.md`). It must require
  the agent to **execute at least one task before closing the session** (no
  orient-and-bail), and to write a **digest** to `memory/YYYY-MM-DD.md` before ending
  the turn — what was done, what changed, what's still open, the next wake's first
  move. `NO_REPLY` is only acceptable when nothing is actionable, with the reason
  logged first.
- Crons and task templates target **only this squad's own agents**.
- A cron run with nothing to report must reply with the single literal token `NO_REPLY`.

## Step 5 — Validate

```sh
node scripts/validate.mjs squads/<name>
```

Fix every error and re-run until the bundle is clean. Address warnings too.

## Step 6 — Hand off

Tell the user the bundle is ready, summarize what was built, and point them to
[`docs/publishing.md`](../../../docs/publishing.md) for getting it into the marketplace.
