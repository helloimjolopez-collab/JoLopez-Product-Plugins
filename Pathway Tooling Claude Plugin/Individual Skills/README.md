# Individual Skills

The five skills from the Pathway Tooling plugin, each packaged as a standalone file so you can share one-at-a-time with a teammate without giving them the whole plugin.

## What's here

| Skill | `.md` file (for direct drop-in) | `.zip` (zipped copy of the .md) | Use case |
|---|---|---|---|
| **Pathway Component Readiness** | [`pathway-component-readiness.md`](./pathway-component-readiness.md) | [`pathway-component-readiness.zip`](./pathway-component-readiness.zip) | Audit a Figma component against the handoff checklist. Safe for any designer. |
| **Pathway Component Spec Maker** | [`pathway-component-spec-maker.md`](./pathway-component-spec-maker.md) | [`pathway-component-spec-maker.zip`](./pathway-component-spec-maker.zip) | Draft a component spec from Figma. Safe for any designer. |
| **Pathway Spec Review** | [`pathway-spec-review.md`](./pathway-spec-review.md) | [`pathway-spec-review.zip`](./pathway-spec-review.zip) | Audit a draft spec against system rules. Safe for any designer. |
| **Pathway Component Pipeline** | [`pathway-component-pipeline.md`](./pathway-component-pipeline.md) | [`pathway-component-pipeline.zip`](./pathway-component-pipeline.zip) | Ship a component end-to-end. **DS owner only.** |
| **Pathway Tokens Sync** | [`pathway-tokens-sync.md`](./pathway-tokens-sync.md) | [`pathway-tokens-sync.zip`](./pathway-tokens-sync.zip) | Sync Figma tokens into the repo. **DS owner only.** |

## How to give a skill to a teammate

1. Pick the skill you want to share (look at the table above).
2. Download the `.md` file (not the zip).
3. Send it to your teammate — Slack, email, AirDrop, whatever.
4. Tell them: *"Save this file to `~/.claude/commands/` on your Mac, then restart Claude Code. To use it, type `/pathway-component-readiness` (or whatever the file was called) in any chat."*

That's it. They don't need GitHub access. They don't need the plugin. They don't need a zip. Just the file.

If you want to write fuller instructions, paste this into the message:

> **To install the skill I'm sending you:**
>
> 1. Download the `.md` file I sent.
> 2. Open Finder and press `Cmd+Shift+G`. Paste `~/.claude/commands/` and press Enter. (If the folder doesn't exist, create it.)
> 3. Drag the `.md` file into that folder.
> 4. Open Claude Code. In any chat, type `/reload-plugins` and hit enter (or just restart the app).
> 5. Type `/` in chat — you should see the new skill in the autocomplete list. Pick it to run.

## Why both `.md` and `.zip`?

- **`.md`** — the actual skill. Drop into `~/.claude/commands/` on a Mac and it works. **This is almost always what you want.**
- **`.zip`** — the same `.md` file, zipped. Included in case some chat/email system strips `.md` attachments or your teammate prefers a zip.

If you're not sure which to use, use the `.md`. It's simpler.

## These files are generated from the plugin source

The canonical source for each skill is in [`../plugin-source/skills/<name>/SKILL.md`](../plugin-source/skills/). The files in this folder are copies, pre-named with the `pathway-` prefix so they work immediately as flat slash commands (without the `/pathway:` namespace that comes from the full plugin install). See the [parent folder's README](../README.md) for how to regenerate these files after editing the source.
