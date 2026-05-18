# CLAUDE.md

Orientation for a Claude Code session opened in this repo.

## What this repo is

`squads` is a documentation-and-content repo for **Agent Squads** — installable bundles
that deploy proactive sub-agents into a Pancake pod. It holds Pancake's official squads,
the source the Pancake marketplace seeds from, and the public contract for squad authors.

There is **no application code** here — no `package.json`, no build. The only script is the
zero-dependency validator.

## How the repo is organized

- `squads/<name>/` directories are **squad bundles** — the unit the marketplace ingests.
- `template/` is a complete, valid skeleton bundle authors copy from.
- `docs/` is the full contract and authoring guides.
- `scripts/validate.mjs` validates every bundle; `manifest.schema.json` is the editor schema.

## The bundle contract, in one paragraph

A bundle is a directory with a `manifest.json` (the validated machine contract: `name`,
`version`, `description`, `author`, `agents[]`, plus optional skills, identities, and vault
secrets), a `SQUAD.md` catalog card, an `ONBOARD.md` onboarding script, and per agent an
`agents/<id>/IDENTITY.md` and `SOUL.md`. Optionally it carries `MEMORY.md` seed memory,
`skills/` files, `crons/jobs.json`, and `tasks-config/templates.json`. Full detail is in
[`docs/bundle-reference.md`](./docs/bundle-reference.md).

## Working in this repo

- To **author** a new squad, use the `create-squad` skill.
- To **check or fix** a squad, use the `validate-squad` skill.
- To validate directly: `node scripts/validate.mjs` (add a bundle path to scope it).
- Model new squads on [`template/`](./template/) (the skeleton) and the contract in
  [`docs/bundle-reference.md`](./docs/bundle-reference.md).

Any change to a squad bundle must keep `node scripts/validate.mjs` green — CI enforces it.
