# Individual Skills — MB Product Intelligence

The three skills from the MB Product Intelligence plugin, each packaged as a standalone file so you can share one-at-a-time with a teammate without giving them the whole plugin.

## What's here

| Skill | `.md` alone | `.zip` (includes references) | Which to send |
|---|---|---|---|
| **MB Persona Maker** | [`mb-persona-maker.md`](./mb-persona-maker.md) | [`mb-persona-maker.zip`](./mb-persona-maker.zip) | **Send the `.md`** — this skill has no references folder, so the single file is complete. |
| **MB Product Knowledgebase Assistant** | [`mb-product-knowledgebase-assistant.md`](./mb-product-knowledgebase-assistant.md) | [`mb-product-knowledgebase-assistant.zip`](./mb-product-knowledgebase-assistant.zip) | **Send the `.zip`** — this skill uses a `references/` folder that must be installed alongside it. |
| **MB Product Knowledgebase Maker** | [`mb-product-knowledgebase-maker.md`](./mb-product-knowledgebase-maker.md) | [`mb-product-knowledgebase-maker.zip`](./mb-product-knowledgebase-maker.zip) | **Send the `.zip`** — same reason; it has a `references/` folder. |

## How to give a skill to a teammate

### If you're sending the `.md` (Persona Maker only)

1. Download the `.md` file.
2. Send it to the teammate.
3. Tell them: *"Save this file to `~/.claude/commands/` on your Mac, restart Claude Code, then type `/mb-persona-maker` in chat to use it."*

### If you're sending the `.zip` (Knowledgebase skills)

1. Download the `.zip` file.
2. Send it to the teammate.
3. Tell them:

> 1. Save the zip somewhere (e.g. Downloads).
> 2. Open Terminal and run:
>    ```
>    mkdir -p ~/.claude/commands
>    cd ~/.claude/commands
>    unzip ~/Downloads/mb-product-knowledgebase-assistant.zip
>    ```
>    (Substitute the actual filename you received.)
> 3. Restart Claude Code (or run `/reload-plugins`).
> 4. Type `/` in any chat — the new skill will be in the autocomplete.

The zip includes both the `.md` (which becomes the slash command) and the `references/` folder the skill needs to work properly.

## Why references matter

The Knowledgebase skills look up template files, reference schemas, and Confluence-template examples from a `references/` folder. Without it, the skills will fail with "reference file not found" errors. The zip exists specifically to keep the references bundled with the skill when sharing single skills.

The Persona Maker skill doesn't have any reference files, so its `.md` alone is complete.

## These files are generated from the plugin source

The canonical source is in [`../plugin-source/skills/<name>/`](../plugin-source/skills/). The files here are copies, ready for drop-in installation. See the [parent folder's README](../README.md) for how to regenerate after editing the source.
