---
name: validate-squad
description: Validate an Agent Squad bundle and fix what's broken — runs the repo validator, explains each error in plain language, and corrects the offending files. Use when the user wants to check, validate, or debug a squad bundle.
---

# Validate a squad

Run the validator, explain what it found, and fix it.

## Step 1 — Run the validator

```sh
node scripts/validate.mjs                 # every squads/* bundle and template/
node scripts/validate.mjs squads/<name>   # one specific bundle
```

Scope it to a single bundle if the user named one. The validator exits non-zero if any
bundle has an error; warnings never fail the run.

## Step 2 — Explain each error

For every error, tell the user in plain language **what** is wrong and **why** it matters,
referencing [`docs/bundle-reference.md`](../../../docs/bundle-reference.md) for the rule.
The validator's checks fall into four groups:

- **Manifest errors** (dotted path, e.g. `agents[0].id`) — a field in `manifest.json`
  breaks a rule: bad kebab-case or semver, a missing required field, a value outside an
  enum, a duplicate agent id. See the field reference in `bundle-reference.md`.
- **Referenced-file errors** — a file the manifest points to (`SQUAD.md`, `ONBOARD.md`, a
  skill, an agent's `IDENTITY.md`/`SOUL.md`) is missing, is a symlink, is not a regular
  file, or resolves outside the bundle root.
- **Targeting errors** (`crons/jobs.json`, `tasks-config/templates.json`) — a cron's
  `sessionTarget` or a template's `assigned_to` names an agent the squad does not declare.
  Squad crons and tasks may target only the squad's own agents.
- **Frontmatter warnings** — `SQUAD.md` is missing `tags` / `token_intensity`, or
  `ONBOARD.md` has no frontmatter block. These do not fail the run, but advise the user to
  fix them so the catalog card renders correctly.

## Step 3 — Fix the offending files

Correct each error in the relevant file. Make the smallest change that satisfies the
contract — don't rewrite content that isn't broken. For a missing referenced file, either
create the file or remove the manifest reference, depending on the user's intent (ask if
unclear).

## Step 4 — Re-run until clean

Run the validator again. Repeat Steps 2–3 until it exits 0 with no errors. Then report the
result: confirm the bundle is valid, and list any warnings the user chose to leave.
