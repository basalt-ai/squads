# Contributing

## This repo is Pancake-curated

`squads` holds Pancake's **official** squads. It is **not open to external squad PRs** —
please do not open a pull request to add your own squad here.

## External authors — build and self-host

If you want to build your own Agent Squad, you keep it in **your own** public GitHub repo
and submit the URL to the marketplace. You do not contribute it here — this repo is an
example to copy from, not a place to contribute to.

Start with [`docs/creating-a-squad.md`](./docs/creating-a-squad.md), and see
[`docs/publishing.md`](./docs/publishing.md) for self-hosting and submission.

## Pancake team — adding an official squad

1. Add the bundle as a `squads/<name>/` directory — use the
   [`create-squad`](./.claude/skills/create-squad/SKILL.md) skill or copy
   [`template/`](./template/).
2. Run `node scripts/validate.mjs` and confirm it is clean — **CI must be green**.
3. Update the official squad table in the [`README.md`](./README.md).
4. Bump the squad's `manifest.json` `version` (semver) if you are changing an existing squad.
5. Open a PR.

The marketplace re-seeds its catalog from this repo on merge.
