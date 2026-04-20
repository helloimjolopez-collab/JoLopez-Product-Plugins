# Individual Skills: MB Smart Product Wiki

The four skills from the MB Smart Product Wiki plugin, each packaged as a standalone file so you can share one-at-a-time with a teammate without giving them the whole plugin.

## What's here

| Skill | `.md` alone | `.zip` (includes references) | Which to send |
|---|---|---|---|
| **MB Persona Maker** | [`mb-persona-maker.md`](./mb-persona-maker.md) | [`mb-persona-maker.zip`](./mb-persona-maker.zip) | **Send the `.md`**. This skill has no references folder, so the single file is complete. |
| **MB Product Knowledgebase Assistant** | [`mb-product-knowledgebase-assistant.md`](./mb-product-knowledgebase-assistant.md) | [`mb-product-knowledgebase-assistant.zip`](./mb-product-knowledgebase-assistant.zip) | **Send the `.zip`**. This skill uses a `references/` folder that must be installed alongside it. |
| **MB Product Knowledgebase Maker** | [`mb-product-knowledgebase-maker.md`](./mb-product-knowledgebase-maker.md) | [`mb-product-knowledgebase-maker.zip`](./mb-product-knowledgebase-maker.zip) | **Send the `.zip`**. Same reason, it has a `references/` folder. |
| **MB User Flows Mapper** | [`mb-user-flows-mapper.md`](./mb-user-flows-mapper.md) | [`mb-user-flows-mapper.zip`](./mb-user-flows-mapper.zip) | **Send the `.zip`**. Uses a `references/` folder with 5 files (canonical template, lane model, Figma render scripts, interview guide, browser exploration). |

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
> 4. Type `/` in any chat. The new skill will appear in the autocomplete.

The zip includes both the `.md` (which becomes the slash command) and the `references/` folder the skill needs to work properly.

## Why references matter

The Knowledgebase skills look up template files, reference schemas, and Confluence-template examples from a `references/` folder. The User Flows Mapper looks up the canonical Figma template spec, lane model, render scripts, interview guide, and browser exploration patterns from its own `references/` folder. Without these folders, the skills will fail with "reference file not found" errors. The zip exists specifically to keep the references bundled with the skill when sharing single skills.

The Persona Maker skill doesn't have any reference files, so its `.md` alone is complete.

## These files are generated from the plugin source

The canonical source is in [`../plugin-source/skills/<name>/`](../plugin-source/skills/). The files here are copies, ready for drop-in installation. See the [parent folder's README](../README.md) for how to regenerate after editing the source.
