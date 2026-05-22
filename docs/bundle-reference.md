# Bundle reference — the Agent Squad contract

This is the exact file contract for an Agent Squad bundle. It is the source of truth that
[`manifest.schema.json`](../manifest.schema.json), [`agent.schema.json`](../agent.schema.json),
[`scripts/validate.mjs`](../scripts/validate.mjs), the [`template/`](../template/) skeleton,
and the marketplace's own ingest all agree on.

If you haven't yet, read [`how-squads-work.md`](./how-squads-work.md) first for the concepts.
The examples below point at [`template/`](../template/), the complete, valid skeleton bundle.

## What a bundle is

A **bundle** is the unit the marketplace ingests — one squad, complete and self-contained.

- In **this repo**, a bundle is a `squads/<squad-name>/` directory.
- In a **self-hosted third-party repo**, the bundle is the **repo root itself**.

The file contract below is identical in both cases. Only the location differs.

## Directory layout

Required (✔) and optional (·) members of a bundle:

```
manifest.json                  ✔  package descriptor (validated on ingest)
SQUAD.md                       ✔  marketplace catalog card — frontmatter + Markdown body
ONBOARD.md                     ✔  the onboarding script the co-founder runs after deploy
MEMORY.md                      ·  squad-wide seed memory (used by an agent lacking its own)
skills/<name>.md               ·  squad-wide skills — referenced by manifest.skills[]
TOOLS.md                       ·  optional documentation of the squad's tool surface
agents/<agent-id>/
  agent.json                   ✔  per-agent runtime config (mirrors OpenClaw agents.list[])
  IDENTITY.md                  ✔  per agent — name, role, scope
  SOUL.md                      ✔  per agent — personality, principles, boundaries
  HEARTBEAT.md                 ·* per agent — the procedure run on every wake (required when agent.json declares a heartbeat)
  MEMORY.md                    ·  per agent — seed memory (overrides the squad-wide one)
  skills/<name>.md             ·  agent-specific skills — referenced by agent.json#/skills
crons/jobs.json                ·  native OpenClaw cron jobs
```

On ingest the marketplace **verifies every file the manifest references** — it must exist,
be a regular file, not be a symlink, and resolve **inside the bundle root** (no `..`
escape, no absolute path). The files always checked are `SQUAD.md`, `ONBOARD.md`, every
`skills[]` path, and per agent `agents/<id>/agent.json`, `agents/<id>/IDENTITY.md`,
`agents/<id>/SOUL.md`, every `agent.json#/skills[]` path, and `agents/<id>/HEARTBEAT.md`
when the agent declares a heartbeat. `scripts/validate.mjs` performs the identical check.

## `manifest.json` — the package descriptor

The file the marketplace fully parses and validates. JSON, no comments. It carries the
package-level metadata only — per-agent runtime config lives in `agents/<id>/agent.json`.

**Required:** `name`, `version`, `description`, `author`, `agents`.

| Field | Type | Req | Rules |
|---|---|---|---|
| `name` | string | ✔ | kebab-case `^[a-z0-9]+(?:-[a-z0-9]+)*$`, ≤ 64 chars. Globally unique — the marketplace catalog key. |
| `version` | string | ✔ | semver `^\d+\.\d+\.\d+(?:-[0-9A-Za-z.-]+)?(?:\+[0-9A-Za-z.-]+)?$` |
| `description` | string | ✔ | non-empty, ≤ 200 chars |
| `author` | string | ✔ | non-empty. Official squads: `pancake-official`. External: a GitHub user/org. |
| `license` | string | · | any string when present |
| `skills` | string[] | · | bundle-relative paths to squad-wide skill files (e.g. `skills/playbook.md`) |
| `agents` | string[] | ✔ | non-empty array of kebab-case agent ids. Each id must have a matching `agents/<id>/agent.json`. |
| `required_identities` | object[] | · | each `{ site, reason }` — both non-empty. `site` is an eTLD+1, e.g. `github.com`. |
| `required_vault_secrets` | object[] | · | each `{ key, label, type }` — `key`/`label` non-empty; `type` ∈ `string` \| `api_key` \| `token` |
| `required_tool_permissions` | string[] | · | each entry must be an accepted Pancake tool key (see [*Tool permissions*](#tool-permissions) below). Unknown keys are an error. |
| `min_pancake_version` | string | · | informational only |

Validation returns **all** problems found, not just the first — so a bad manifest can be
fixed in one pass. The authoritative validator in production is
`apps/marketplace/src/services/manifest.ts`; `manifest.schema.json` in this repo is its
JSON Schema mirror, and `scripts/validate.mjs` is a behaviour-identical port. A bundle that
passes `validate.mjs` passes marketplace ingestion.

See [`template/manifest.json`](../template/manifest.json) for the complete file structure.

## `agents/<id>/agent.json` — per-agent runtime config

The file the OpenClaw deploy plugin reads to register each squad agent. JSON, no comments.
The schema mirrors the subset of OpenClaw's `agents.list[]` that bundles are allowed to
declare — see [the OpenClaw config-agents reference](https://docs.openclaw.ai/gateway/config-agents)
for the canonical upstream spec.

**Required:** `id`, `description`.

| Field | Type | Req | Rules |
|---|---|---|---|
| `id` | string | ✔ | kebab-case. Must match the directory name `agents/<id>/` and the entry in `manifest.json#/agents`. |
| `description` | string | ✔ | non-empty one-liner — what this agent owns. |
| `model` | string | · | enum `haiku` \| `sonnet` \| `opus`. Defaults to the pod's `agents.defaults.model` (`sonnet`) when omitted. |
| `heartbeat` | object | · | Curated subset of [OpenClaw's `agents.list[].heartbeat`](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-heartbeat) — `{ every, model, lightContext, isolatedSession, skipWhenBusy, timeoutSeconds }`. All sub-fields optional; the validator rejects anything else (pod-level fields like `prompt`, `target`, `directPolicy`, `session`, `to`, `ackMaxChars` etc. are not authorable from a bundle). `every` is an OpenClaw duration string (units `ms`/`s`/`m`/`h`, e.g. `"30m"`, `"2h"`, `"24h"`, or `"0m"` to disable) — named values like `"daily"` are rejected. `heartbeat.model` follows the same `haiku`/`sonnet`/`opus` enum as the top-level `model`. Inherits any omitted sub-field from the pod's `agents.defaults.heartbeat`. When the heartbeat object is present, `agents/<id>/HEARTBEAT.md` must exist. |
| `skills` | string[] | · | bundle-relative paths to this agent's skill files. |
| `contextInjection` | string | · | enum `always` \| `continuation-skip` \| `never`. Pod default applies when omitted. |
| `bootstrapMaxChars` | integer | · | positive. OpenClaw bootstrap budget; pod default applies when omitted. |
| `params` | object | · | free-form provider params passed through to OpenClaw (e.g. `{ "cacheRetention": "1h" }`). |

`additionalProperties` is false — unknown keys are an error. The validator rejects
unknown fields rather than silently dropping them, so any new OpenClaw field a squad needs
must be added here first.

See [`template/agents/example-agent/agent.json`](../template/agents/example-agent/agent.json).

## `SQUAD.md` — the marketplace catalog card

Markdown with a YAML frontmatter block, then a body.

**Frontmatter** is minimal — only two fields are read by the marketplace:

```yaml
---
tags: [gtm, outbound, linkedin, sales, growth]
preview_image: https://squads.getpancake.ai/avatars/astronaut.png  # optional
---
```

- `tags` — string array. Drives catalog filtering. Recommended; if absent, the card shows
  no tags.
- `preview_image` — optional URL. Shown as the squad's avatar in the marketplace.

Every other package-level field (name, version, description, author) lives in
`manifest.json` and must not be duplicated here. The deprecated `token_intensity` field
is a validation error — Pancake Cloud now computes token usage automatically.

**The body** renders as the squad's store detail page **and is the catalog's source of
truth for per-agent prose**. The marketplace reads each agent's user-facing description
from this body, not from `manifest.json` or `agent.json`. Describe every agent here in
plain language — the recommended sections cover it naturally: *What this squad does*,
*What you'll need*, *What you get*, *How it works*.

## `ONBOARD.md` — the onboarding script

Markdown with frontmatter, then prose.

**Frontmatter:** `required_tools: [...]`, `required_identities: [...]`,
`estimated_setup_minutes: <n>`.

**The body is a script the co-founder agent executes** after the mechanical deploy — it is
*instructions, not documentation*. Write it in the imperative, addressed to the co-founder.
It tells the co-founder:

- what to ask the user;
- which secrets to collect — always via `vault_request`, never in chat;
- which identities to connect — via `browser_identity_add`, reusing an existing pod
  identity when one matches;
- where to save the answers — usually the agent's `MEMORY.md`;
- what first task to create and dispatch.

Keep the script short enough to complete within `estimated_setup_minutes`. A step may be
tagged `dispatch: later` to defer its first task to the agent's heartbeat; otherwise the
first task is dispatched immediately.

## `IDENTITY.md` and `SOUL.md` — per agent

Both are deployed verbatim into the agent's workspace at install. Together they define the
agent.

**`IDENTITY.md` — who the agent is.** Recommended sections (mirror
[`template/agents/example-agent/IDENTITY.md`](../template/agents/example-agent/IDENTITY.md)):

- A header block: **Name**, **Role**, **Scope**, **Emoji**, **Created** / **Created by**.
- **What I Do** — the concrete, recurring responsibilities.
- **What I Don't Do** — the edges of the lane; what it routes back to the co-founder.
- **KPI / Goal** — the single outcome the agent exists to move.
- **How To Reach Me** — the reporting line (the user never talks to it directly).
- **Voice / Personality** — a pointer to `SOUL.md`.

**`SOUL.md` — how the agent behaves.** Recommended sections (mirror
[`template/agents/example-agent/SOUL.md`](../template/agents/example-agent/SOUL.md)):

- An opening paragraph: a focused contributor reporting to the co-founder, not a generalist.
- **Scope** — what it owns and explicitly does not own.
- **Personality** — concrete behavioural traits.
- **Operating Principles** — how it works day to day.
- **Escalation Rules** — when to escalate vs decide alone.
- **Boundaries (Inviolable)** — the *Never* / *Always* hard limits.
- **What Success Looks Like** — the bar.

The step-by-step wake procedure lives in [`HEARTBEAT.md`](#heartbeatmd--the-wake-procedure),
not in `SOUL.md` — keep behavioural rules here and the procedure there.

## `HEARTBEAT.md` — the wake procedure

Per agent. **Required when `agent.json` declares a `heartbeat`** — the validator errors
otherwise. OpenClaw loads `agents/<id>/HEARTBEAT.md` on **every wake** — both heartbeat
pulses and dispatched tasks — before the agent starts work. This is the right home for
the recurring procedure the agent runs each tick: what to read, what to decide, what to
file. Keeping it out of `SOUL.md` is the convention because:

- **`SOUL.md` is about behaviour** — personality, principles, escalation rules.
  It should not also carry the step-by-step procedure.
- **`MEMORY.md` is an index of pointers**, not a script. Burying wake steps
  there hides them and bloats memory.
- **Authors can iterate on the wake procedure without touching `SOUL.md`**,
  which keeps personality/principles stable across releases.

Write it in the imperative, addressed to the agent. A solid structure
(mirrored in [`template/agents/example-agent/HEARTBEAT.md`](../template/agents/example-agent/HEARTBEAT.md)):

1. **The non-negotiable** — *at least one task must be EXECUTED before the
   session closes.* A wake is "orient, find the highest-leverage action in
   the lane, do it, file the result" — not "orient and NO_REPLY". `NO_REPLY`
   is only acceptable when nothing is actionable, and the reason must be
   logged to `memory/YYYY-MM-DD.md` first.
2. **Orient** — read `MEMORY.md`, skim recent daily logs, `list_tasks`.
3. **Decide what this wake is for** — dispatched task, recurring duty, draft
   to advance, or genuinely nothing (with a logged reason).
4. **Recurring duty** — the agent's specific heartbeat work and its cadence.
5. **Execute** — actually produce the artifact; don't just plan.
6. **Digest** — before closing the session, append a one-paragraph digest to
   `memory/YYYY-MM-DD.md`: *what you did, what changed, what's still open,
   and the single first move for the next wake.* The digest is for
   future-you; only escalate to the co-founder when there is material news.
   A wake without a digest is an unfinished wake.
7. **Close the loop** — `complete_task` / `fail_task`, surface blockers.

If `heartbeat` is omitted from `agent.json`, `HEARTBEAT.md` is optional and the pod's
default wake template is used when the agent does wake.

## `MEMORY.md` — seed memory

A thin **index of pointers**, not a notebook. It is seeded into the agent's workspace at
install and gives the agent its bearings: where its identity lives, its reporting line,
which squad and skills it has, which vault keys it uses, where it files its outputs.

- Keep every entry a one-line pointer. Detailed findings belong in the shared wiki.
- A bundle may ship a squad-wide `MEMORY.md` (used by any agent without its own) and/or a
  per-agent `agents/<id>/MEMORY.md`. **The agent-specific file overrides the squad-wide
  one.** If neither exists, the pod's own memory template is used.

See [`template/MEMORY.md`](../template/MEMORY.md).

## Skills

A skill is a procedure an agent can load — a method written as steps, not reference docs.
Skill files are in **SKILL.md format**: a YAML frontmatter block with `name` and
`description`, then a Markdown body.

```markdown
---
name: my-skill
description: One or two sentences on what the skill does and when to load it.
---

# My skill

...the procedure...
```

Two levels:

- **Squad-wide skills** — listed in the top-level `manifest.skills[]`, files under
  `skills/<name>.md`. Copied into **every** agent of the squad at install.
- **Agent-specific skills** — listed in `agents/<id>/agent.json#/skills`, files under
  `agents/<id>/skills/<name>.md`. Deployed only into that one agent.

**Skill isolation:** at install, every referenced skill is deployed into each agent's *own*
folder, `workspace/agents/<id>/skills/<name>/SKILL.md`, and the agent's skill allowlist is
`["<agent-id>", "shared"]`. Squad agents never inherit the main co-founder's skills, and a
squad-wide skill is *duplicated* into each agent — not shared by reference.

## `crons/jobs.json` — native cron jobs

Optional. Native OpenClaw cron jobs registered at install.

```json
{
  "version": 1,
  "jobs": [
    {
      "id": "daily-citation-audit",
      "name": "Daily GEO citation audit",
      "enabled": true,
      "schedule": { "kind": "cron", "expr": "0 18 * * *", "tz": "America/Los_Angeles" },
      "sessionTarget": "atlas",
      "payload": { "kind": "systemEvent", "text": "<instructions for the agent>" },
      "failureAlert": false,
      "state": {}
    }
  ]
}
```

- **`sessionTarget` must be an agent id declared in `manifest.agents`.** Squad crons may
  only target the squad's own agents — this is the *squad-only targeting* invariant, and it
  is enforced at install (and by `validate.mjs`).
- At install, job ids are namespaced `<squad-name>__<id>` so two squads cannot collide.
- A cron run that intentionally produces no output must instruct the agent to reply with
  the single literal token **`NO_REPLY`** — OpenClaw's silent-turn sentinel. Never write
  "do not respond"; that trips a false-positive failure alert.

## Dispatchable work

Squads do not ship task templates. The agent's recurring wake procedure lives in
`HEARTBEAT.md`, and ad-hoc work is dispatched by the co-founder at runtime via the
tasks plugin (`create_task`) — there is no per-bundle template file. A `tasks/` directory
inside a bundle has no meaning to the runtime; do not create one.

## Forbidden files

These filenames are **pod-managed by Pancake Cloud** and live at the pod workspace root
(alongside `CLAUDE.md`), not inside any bundle. The validator rejects them at any depth
inside a bundle directory (case-insensitive):

- `AGENTS.md`
- `USER.md`
- `BOOTSTRAP.md`
- `BOOT.md`

A bundle's `MEMORY.md` is allowed (and idiomatic) to *reference* these files by relative
path (e.g. `../../USER.md` as the user-pointer in an agent's MEMORY) — the validator only
forbids the *files themselves*, not references to them.

`TOOLS.md` is explicitly **allowed** inside a bundle — it is bundle-authored documentation
of the squad's tool surface, distinct from the pod-level files above.

## Tool permissions

`manifest.required_tool_permissions` is the list of Pancake-shipped tools the squad needs
access to. The marketplace will not grant a permission for a tool Pancake does not ship, so
the validator rejects anything outside this list.

Each tool has one or more **accepted keys**. Either snake_case or kebab-case variants are
accepted where listed; pick one and stick with it. Duplicates within the same array are
rejected.

| Tool | Accepted keys |
|---|---|
| Browser (Anchor) | `browser` |
| Web search (Exa) | `exa`, `web_search` |
| GitHub | `github` |
| Google Workspace | `google-workspace`, `google_workspace` |
| Notion | `notion` |
| Email (AgentMail) | `agentmail` |
| Identity vault | `vault` |
| Preview hosting | `preview-host`, `publish_preview` |
| Slack Block Kit | `slack-block-kit`, `slack_block_kit_send` |
| MCP installer | `mcp-installer` |
| Image generation | `image-generation`, `image_generate`, `image` |
| Voice / TTS | `voice`, `tts` |
| Scheduling | `cron` |

When Pancake ships a new tool, this list — and the validator's `ACCEPTED_TOOL_PERMISSIONS`
table in `scripts/validate.mjs` — are updated together.

## Deprecated fields

The validator emits an error if it finds any of these:

- `token_intensity` in `manifest.json` or in `SQUAD.md` frontmatter. Pancake Cloud now
  computes token usage automatically from the model, tools called, and crons declared —
  the author-declared field is no longer trusted.

## Naming conventions

- **Squad name** — kebab-case, globally unique, ≤ 64 chars.
- **Agent id** — kebab-case, unique within the squad. Becomes the OpenClaw sub-agent id.
- **Skill files** — kebab-case `<name>.md`, in SKILL.md format.
- **`version`** — semver. Bump it on every release; the marketplace keeps version history.

## Validating

Run the validator before publishing — it mirrors marketplace ingestion exactly:

```sh
node scripts/validate.mjs                      # every squads/* bundle and template/
node scripts/validate.mjs squads/<bundle-name> # one bundle
```

It exits non-zero on any error; warnings (e.g. a missing `tags` line) never fail the run.
Error categories the validator emits:

| Category | Example |
|---|---|
| Manifest schema | `agents[0]  "Geo Agent" must be kebab-case` |
| `agent.json` missing | `agents/foo/agent.json  not found` |
| `agent.json` schema | `agents/foo/agent.json#/model  must be one of: haiku, sonnet, opus` |
| Referenced file | `agents/foo/HEARTBEAT.md  referenced by the manifest but not found` |
| Cron targeting | `crons/jobs.json  cron job "x" sessionTarget "y" is not a declared agent id` |
| Forbidden file | `agents/foo/USER.md  forbidden filename — …` |
| Deprecated field | `SQUAD.md  frontmatter has a deprecated 'token_intensity:' line` |
| Unresolved TODO | `SQUAD.md  unresolved TODO marker on line 12` |

Next: [`creating-a-squad.md`](./creating-a-squad.md) for the step-by-step authoring guide,
and [`publishing.md`](./publishing.md) for getting your squad into the marketplace.
