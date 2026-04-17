# pathway-claude-skills

Claude Code plugin for the **Pathway Design System** (Ministry Brands Amplify). Five skills that cover the full lifecycle of a component — from a Figma handoff check, through drafting the spec, reviewing it, and finally shipping it into the design system repo and Storybook.

---

## Who this is for

**Product designers** can use the first three skills freely. They don't write to any repo — they just read Figma, ask questions, and produce or audit local files.

**Design system owners** (currently the team maintaining `pathwaytokens`) can use all five, including the two that commit and push to the design system repo.

| Skill | Audience | One-line use |
|---|---|---|
| `Pathway Component Readiness` | Any designer | *"Is my Figma component ready for engineering?"* |
| `Pathway Component Spec Maker` | Any designer | *"Draft a spec document from my Figma component."* |
| `Pathway Spec Review` | Any designer | *"Audit this spec against the system rules and flag conflicts."* |
| `Pathway Component Pipeline` | DS owner | *"Ship this component end-to-end — Figma to Storybook to GitHub."* |
| `Pathway Tokens Sync` | DS owner | *"Pull the latest Figma tokens into the repo and Storybook."* |

---

## How to get the plugin

Three install paths depending on what you want.

### 1. Full plugin via Claude Code CLI (recommended for DS owners)

In any Claude Code session:

```
/plugin marketplace add helloimjolopez-collab/pathway-claude-skills
/plugin install pathway@pathway-claude-skills
/reload-plugins
```

All five skills become available under `/pathway:*` (e.g. `/pathway:component-readiness`).

### 2. Full plugin via the Customize tab (Claude Code desktop)

If you prefer the Claude Code desktop UI:

1. Download `pathway-claude-skills.zip` from the Releases page of this repo (or build it locally from source).
2. Open Claude Code → **Customize** → **Personal plugins** → click the `+` button.
3. Upload the `.zip`.

The plugin appears in your Customize tab, with a toggle to enable/disable and the full list of skills. Invoke skills the same way: type `/` in chat and pick one.

### 3. Single-skill files via `for-designers/` (for ad-hoc sharing)

If you only want to give a specific designer a specific skill — no full plugin install, no GitHub access — grab a file from [`for-designers/`](./for-designers/) and send it to them. They drop it into `~/.claude/commands/` on their Mac and it becomes a flat slash command (`/pathway-component-readiness` etc.).

See [`for-designers/INSTALL.md`](./for-designers/INSTALL.md) for a non-technical one-pager aimed at designers.

---

## Prerequisites

Whichever install path you use, the skills themselves assume:

- **Figma MCP server** is configured in Claude Code (used to read components from Figma)
- **`gh` CLI** authenticated (only needed for the two DS-owner skills that push to the repo)
- **Node + npm** installed (only needed for Tokens Sync and Component Pipeline — they build tokens and Storybook locally)
- **The `pathwaytokens` repo** cloned at `~/Desktop/FigmaWork/pathwaytokens/` (or wherever you've put it; the skills read spec templates and rules from there)

If any of these is missing, the skills will tell you what they can't do and stop — they never guess or fake results.

---

## How the skills compose

```
Designer prepares Figma (follows the prep checklist in pathwaytokens/docs/)
  │
  ▼
/pathway:component-readiness <figma-url>
  │    reports ✓ / ✗ / ⚠ / ? for each checklist item
  ▼
/pathway:component-spec-maker <component-name> <figma-url>
  │    drafts components/<name>/<name>-spec.md
  │    marks it Status: PENDING HUMAN REVIEW
  │    asks clarifying questions one at a time
  ▼
Designer reads the draft, fills in any [TBD] markers
  │
  ▼
/pathway:spec-review <component-name>
  │    walks through every conflict with the system spec
  │    user approves each decision
  │    flips Status: to REVIEWED when all resolved
  ▼
/pathway:component-pipeline <component-name> --mode=create
       (DS owner only from here)
       generates React module + HTML demo + Storybook stories + MDX + manifest
       commits and pushes after explicit approval at every gate
```

The first three skills are shareable with any designer. The pipeline skill writes to the design system repo and is intended for the DS owner.

---

## Human review at every step

Every skill in this plugin follows one non-negotiable rule: **the human approves every gate.** Never batched questions, never auto-proceed between phases, never auto-commit, never auto-push, never auto-mark a spec as Reviewed. Claude drafts, flags, recommends — humans decide.

See `pathwaytokens/CLAUDE.md` §11 for the canonical statement of this principle.

---

## Development

Plugin source lives in this repo. To edit a skill:

1. Clone this repo
2. Edit `skills/<skill-name>/SKILL.md`
3. Test locally: `/plugin marketplace add ./path/to/this/repo` → `/plugin install pathway@pathway-claude-skills` → `/reload-plugins`
4. Commit and push
5. Bump `version` in `.claude-plugin/plugin.json` for non-trivial changes

To regenerate the distributable zip:

```sh
cd /path/to/pathway-claude-skills
zip -r pathway-claude-skills.zip . -x ".git/*" ".DS_Store" "for-designers/*"
```

---

## License

UNLICENSED. Internal Ministry Brands tooling.
