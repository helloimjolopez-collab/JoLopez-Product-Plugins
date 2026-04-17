# MB Product Intelligence Claude Plugin

Ministry Brands Product Intelligence — three Claude Code skills for product teams. Builds and maintains product knowledgebases and user personas; all knowledgebases save to the [MB DesignOps Confluence template](https://ministrybrands.atlassian.net/wiki/spaces/DR/pages/6534463521/Product+Knowledge+Base+Template).

## What's in this folder

| File / folder | What it is |
|---|---|
| **[`MB Product Intelligence Claude Plugin (Full).zip`](./MB%20Product%20Intelligence%20Claude%20Plugin%20%28Full%29.zip)** | The full plugin — all 3 skills. Download this and upload it to the Customize tab to get everything. |
| [`plugin-source/`](./plugin-source/) | The unzipped contents of the zip above. Browse/edit here. |
| [`Individual Skills/`](./Individual%20Skills/) | Each of the 3 skills as a single file, for sharing one-at-a-time with teammates. |

## The three skills in this plugin

| Skill | What it does |
|---|---|
| **MB Product Knowledgebase Maker** | Builds a two-page product knowledgebase — a Snapshot dashboard plus a Product Bible deep reference — from a guided interview and live product walkthrough. |
| **MB Product Knowledgebase Assistant** | Your companion for using a product knowledgebase day-to-day. Helps you query, update, audit, and walk KB integrity against specific tasks. |
| **MB Persona Maker** | Guides you through building useful, data-informed user personas with gap analysis and validation guides. Can read an existing knowledgebase to jumpstart persona creation. |

All three are **safe for any product manager, designer, or researcher** to run.

## Install the full plugin (for yourself)

1. Download **`MB Product Intelligence Claude Plugin (Full).zip`** (the file in this folder).
2. Open Claude Code → **Customize** → **Personal plugins**.
3. Click the `+` button, select the zip.
4. Done. All three skills are now available under `/mb-*` slash commands.

## Share ONE skill with a teammate

1. Go to [`Individual Skills/`](./Individual%20Skills/).
2. Pick the skill you want to share.
3. For **`mb-persona-maker`** — send them the `.md` file; they drop it in `~/.claude/commands/`.
4. For **`mb-product-knowledgebase-assistant`** and **`mb-product-knowledgebase-maker`** — these skills have a `references/` folder alongside `SKILL.md`. Send the **`.zip`** instead (it preserves the references). They unzip into `~/.claude/commands/` so the skill plus its references live together.

See [`Individual Skills/README.md`](./Individual%20Skills/README.md) for the exact instructions to give to a teammate.

## Edit or update the plugin

Source files live in `plugin-source/`. Edit there, regenerate the zip, commit, push.

```sh
cd "MB Product Intelligence Claude Plugin"

# Regenerate the full-plugin zip
cd plugin-source
zip -r "../MB Product Intelligence Claude Plugin (Full).zip" . -x ".DS_Store"
cd ..

# Regenerate individual skill files + zips
cp plugin-source/skills/mb-persona-maker/SKILL.md                    "Individual Skills/mb-persona-maker.md"
cp plugin-source/skills/mb-product-knowledgebase-assistant/SKILL.md  "Individual Skills/mb-product-knowledgebase-assistant.md"
cp plugin-source/skills/mb-product-knowledgebase-maker/SKILL.md      "Individual Skills/mb-product-knowledgebase-maker.md"

cd "Individual Skills"
zip -q mb-persona-maker.zip mb-persona-maker.md

cp -R ../plugin-source/skills/mb-product-knowledgebase-assistant/references .
zip -q -r mb-product-knowledgebase-assistant.zip mb-product-knowledgebase-assistant.md references
rm -rf references

cp -R ../plugin-source/skills/mb-product-knowledgebase-maker/references .
zip -q -r mb-product-knowledgebase-maker.zip mb-product-knowledgebase-maker.md references
rm -rf references
```

Bump `version` in `plugin-source/.claude-plugin/plugin.json` for non-trivial changes.

## Author

Ministry Brands Product Team
