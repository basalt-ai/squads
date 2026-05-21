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
The validator's checks fall into the following categories:

- **Manifest schema** (e.g. `agents[0]  "Geo Agent" must be kebab-case`) — a field in
  `manifest.json` breaks a rule: bad kebab-case or semver, a missing required field, a
  value outside an enum, a duplicate agent id, or `agents` is no longer an object array
  (it must now be a string array of agent ids).
- **`agent.json` missing** (`agents/<id>/agent.json  not found`) — every id in
  `manifest.agents` must have a matching `agents/<id>/agent.json` file.
- **`agent.json` schema** (e.g. `agents/<id>/agent.json#/model  must be one of: haiku,
  sonnet, opus`) — the per-agent config is invalid. Common causes: wrong `model` value
  (string enum), `heartbeat` written as a plain string instead of the object shape
  `{ "every": "daily" }`, an unknown field, or `id` not matching the directory name.
- **Referenced-file errors** — a file the manifest or agent.json points to (`SQUAD.md`,
  `ONBOARD.md`, a skill, `IDENTITY.md`, `SOUL.md`, `HEARTBEAT.md` when the agent has a
  heartbeat) is missing, is a symlink, is not a regular file, or resolves outside the
  bundle root.
- **Targeting errors** (`crons/jobs.json`) — a cron's `sessionTarget` names an agent the
  squad does not declare. Squad crons may target only the squad's own agents.
- **Forbidden file** (e.g. `agents/<id>/USER.md  forbidden filename`) — the bundle
  contains a file named `AGENTS.md`, `USER.md`, `BOOTSTRAP.md`, or `BOOT.md`. Those are
  pod-managed by Pancake Cloud and must not appear inside a bundle. Delete the file.
  `TOOLS.md` is *allowed* and is not flagged.
- **Deprecated field** (e.g. `SQUAD.md  frontmatter has a deprecated 'token_intensity:'
  line`) — `token_intensity` has been removed from the contract. Pancake Cloud computes
  token usage automatically; delete the line.
- **Unresolved TODO** (e.g. `SQUAD.md  unresolved TODO marker on line 12`) — the bundle
  still contains `<!-- TODO`, `TODO:`, or a bare `TODO` line left over from the template.
  Strip the placeholder.
- **Frontmatter warnings** — `SQUAD.md` missing `tags`, or `ONBOARD.md` missing its
  frontmatter block. These do not fail the run but advise the user to fix them so the
  catalog card renders correctly.

## Step 3 — Fix the offending files

Correct each error in the relevant file. Make the smallest change that satisfies the
contract — don't rewrite content that isn't broken. For a missing referenced file, either
create the file or remove the manifest reference, depending on the user's intent (ask if
unclear). For a forbidden filename, delete the file (or rename it if the content is
worth keeping — e.g. `USER.md` content can move into `MEMORY.md` as a pointer).

## Step 4 — Re-run until clean

Run the validator again. Repeat Steps 2–3 until it exits 0 with no errors. Then report the
result: confirm the bundle is valid, and list any warnings the user chose to leave.
