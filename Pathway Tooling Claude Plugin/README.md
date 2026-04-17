# Pathway Tooling Claude Plugin

The Pathway Design System tooling for Ministry Brands Amplify. Five Claude Code skills that cover the full component lifecycle.

## What's in this folder

| File / folder | What it is |
|---|---|
| **[`Pathway Tooling Claude Plugin (Full).zip`](./Pathway%20Tooling%20Claude%20Plugin%20%28Full%29.zip)** | The full plugin — all 5 skills. Download this and upload it to the Customize tab to get everything. |
| [`plugin-source/`](./plugin-source/) | The unzipped contents of the zip above. Browse/edit the plugin's files here. |
| [`Individual Skills/`](./Individual%20Skills/) | Each of the 5 skills as a single file, for sharing one-at-a-time with teammates. |

## The five skills in this plugin

| Skill | Who should run it | What it does |
|---|---|---|
| **Pathway Component Readiness** | Any designer | Checks a Figma component against the Pathway handoff checklist — ready / broken / missing. Read-only. |
| **Pathway Component Spec Maker** | Any designer | Drafts a component spec from a Figma component. Marks output `PENDING HUMAN REVIEW`. |
| **Pathway Spec Review** | Any designer | Audits a draft spec against system-wide rules; resolves every conflict. Only skill that can mark a spec `REVIEWED`. |
| **Pathway Component Pipeline** | DS owner only | Ships a component end-to-end: Figma → `.jsx` + `.html` + stories + MDX + manifest → GitHub → Storybook. |
| **Pathway Tokens Sync** | DS owner only | Syncs the Figma token export into the repo, rebuilds Storybook, pushes. |

## Install the full plugin (for yourself)

1. Download **`Pathway Tooling Claude Plugin (Full).zip`** (the file in this folder).
2. Open Claude Code → **Customize** → **Personal plugins**.
3. Click the `+` button, select the zip.
4. Done. All five skills are now available in your Customize tab. Invoke via `/` in any chat (e.g. `/pathway:component-readiness`).

## Share ONE skill with a teammate

1. Go to [`Individual Skills/`](./Individual%20Skills/).
2. Pick the skill you want to share (e.g. `pathway-component-readiness.md`).
3. Send the `.md` file to the teammate (Slack, email, anything).
4. They save it to `~/.claude/commands/` on their Mac, restart Claude Code, and use it via `/pathway-component-readiness` in chat.

A teammate doesn't need GitHub access or a plugin install — just the file.

## Edit or update the plugin

Source files live in `plugin-source/`. Edit there, then regenerate the zip and individual skill files. From the repo root:

```sh
cd "Pathway Tooling Claude Plugin"

# Regenerate the full-plugin zip
cd plugin-source
zip -r "../Pathway Tooling Claude Plugin (Full).zip" . -x ".DS_Store"
cd ..

# Regenerate the individual skill files
cp plugin-source/skills/component-readiness/SKILL.md   "Individual Skills/pathway-component-readiness.md"
cp plugin-source/skills/component-spec-maker/SKILL.md  "Individual Skills/pathway-component-spec-maker.md"
cp plugin-source/skills/spec-review/SKILL.md           "Individual Skills/pathway-spec-review.md"
cp plugin-source/skills/component-pipeline/SKILL.md    "Individual Skills/pathway-component-pipeline.md"
cp plugin-source/skills/tokens-sync/SKILL.md           "Individual Skills/pathway-tokens-sync.md"

# Regenerate individual zips
cd "Individual Skills"
for f in pathway-*.md; do zip -q "${f%.md}.zip" "$f"; done
cd ..
```

Then commit and push. After that you can either `/plugin update pathway@jolopez-product-plugins` in Claude Code (CLI install) or re-upload the new zip to the Customize tab.

## The plugin's design principles

Every skill in this plugin gates every step with explicit human approval — never auto-proceeds, never batches questions, never commits without confirmation. See `CLAUDE.md` §11 in the [`pathwaytokens`](https://github.com/helloimjolopez-collab/pathwaytokens) repo for the canonical statement of this principle.
