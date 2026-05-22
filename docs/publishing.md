# Publishing a squad

How a finished bundle gets into the Pancake marketplace. There are two paths — official
squads and self-hosted third-party squads.

## Official squads — the Pancake team

Official squads live in **this repo**, under `squads/<name>/`. The marketplace seeds its
catalog from this repo's inner `squads/` directory.

To publish one:

1. Add the bundle as a new `squads/<name>/` directory (use the
   [`create-squad`](../.claude/skills/create-squad/SKILL.md) skill, or copy
   [`template/`](../template/) — see [`creating-a-squad.md`](./creating-a-squad.md)).
2. Run `node scripts/validate.mjs` and confirm it is clean.
3. Update the squad table in the [`README.md`](../README.md).
4. Open a PR. CI runs the validator on every push — it must be green.
5. On merge, the marketplace re-seeds and the squad appears in the catalog.

Bump `manifest.json` `version` (semver) whenever you change a squad — the marketplace keeps
version history. See [`CONTRIBUTING.md`](../CONTRIBUTING.md) for the curation policy.

## Self-hosted squads — external authors

**This repo is Pancake-curated and not open to outside PRs.** If you are not on the Pancake
team, you do not add your squad here. Instead you **self-host**: keep the bundle in your
own public GitHub repo and submit its URL to the marketplace.

### Your repo *is* the bundle

In a self-hosted repo the bundle is the **repo root itself** — `manifest.json`, `SQUAD.md`,
`ONBOARD.md`, `agents/`, and the rest sit directly at the root:

```
your-squad-repo/
├── manifest.json
├── SQUAD.md
├── ONBOARD.md
├── MEMORY.md
├── agents/<agent-id>/IDENTITY.md, SOUL.md, skills/...
├── skills/...
└── crons/jobs.json
```

There is **no `squads/` directory** in a self-hosted repo — that nesting is specific to
*this* multi-squad repo. The file contract inside the bundle is otherwise identical; see
[`bundle-reference.md`](./bundle-reference.md).

### Requirements

- A **public** repo on `github.com`.
- The repo URL must match `https://github.com/<owner>/<repo>` (a `.git` suffix is allowed).
- The bundle must pass the validator. Copy [`scripts/validate.mjs`](../scripts/validate.mjs)
  and [`manifest.schema.json`](../manifest.schema.json) into your repo and run
  `node scripts/validate.mjs .`, or simply build to the contract in
  [`bundle-reference.md`](./bundle-reference.md).
- **Tag your releases.** The marketplace can ingest a specific tag or branch, so a git tag
  per `version` gives you stable, reproducible releases.

### Submitting

<!-- TODO [USER]: fill in the concrete submission channel — a form URL, an email
     address, or the marketplace publish endpoint details — once it exists. -->

> **Today's reality:** there is no self-serve submission form yet. The marketplace's
> publish endpoint (`POST /squads`) is **admin-token-gated**, so a squad is currently added
> by the Pancake team on your behalf. To submit, send your repo URL (and the tag you want
> ingested) to the Pancake team. A self-serve submission flow is planned but not yet
> available.

### What happens on ingest

When your repo is ingested, the marketplace:

1. Clones your repo (a specific tag/branch if you specified one).
2. Runs the **identical** manifest validation and file verification this repo's validator
   runs — same rules, same checks.
3. Rejects the bundle if it contains symlinks or any path that escapes the bundle root.
4. Archives a verified `.tar.gz` and lists the squad in the catalog.

Because the checks are identical, **a bundle that passes `node scripts/validate.mjs` passes
ingestion.** Validate locally before you submit.

### Versioning

Bump `manifest.json` `version` (semver) on every release and tag the commit. The
marketplace keeps version history, so users can see — and the catalog can pin — specific
versions.
