# Pathway skills for designers — install

Hi! You've been given one or more Pathway skills as individual `.md` files. These are Claude Code **slash commands** — little instruction files that Claude follows when you type the corresponding `/` command in a chat.

You don't need GitHub, a plugin, or any special tooling. You just need to drop the file into a specific folder on your Mac.

## Three skills are available here

| File | What it does |
|---|---|
| `pathway-component-readiness.md` | Checks your Figma component against the Pathway prep checklist — tells you what's ready, wrong, missing. Read-only. |
| `pathway-component-spec-maker.md` | Drafts a component spec from your Figma component following the Pathway template. Asks questions, writes a file, marks it `PENDING HUMAN REVIEW`. |
| `pathway-spec-review.md` | Audits a draft spec against the overarching design system rules. Walks you through every conflict. The only skill that can mark a spec as `REVIEWED`. |

You can install any subset. They're independent.

## Install (per file)

### 1. Save the file

Save the `.md` file you were given into this folder on your Mac:

```
~/.claude/commands/
```

(That's `~/.claude/commands/` — a hidden folder in your home directory. If the folder doesn't exist, create it.)

The filename matters: keep it exactly as given — `pathway-component-readiness.md` (or whichever).

You can do this in Finder (press `Cmd+Shift+G` and paste `~/.claude/commands/`) or in Terminal:

```sh
mkdir -p ~/.claude/commands
mv ~/Downloads/pathway-component-readiness.md ~/.claude/commands/
```

### 2. Reload Claude Code

If you have a Claude Code session open, run `/reload-plugins` to pick up the new file. Otherwise just start a new session.

### 3. Use it

In Claude Code, type `/` and you'll see the new command in the autocomplete list. Pick it, add any arguments (e.g. a Figma URL), and it runs.

Examples:

```
/pathway-component-readiness https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/...?node-id=40006622-50003
```

```
/pathway-component-spec-maker
```

(The skill will ask for the inputs it needs one at a time.)

## Updating

If the skill author sends you a new version of the file, just replace the existing one in `~/.claude/commands/` and run `/reload-plugins`. No versioning, no auto-updates — your copy is the copy you have.

## Uninstalling

Delete the file from `~/.claude/commands/`. Run `/reload-plugins` or restart Claude Code. Gone.

## Requirements

These skills call the Figma MCP server to read your Figma components. If Figma MCP isn't configured in your Claude Code, the skills will fail with a "no Figma tool" error. Ask the person who shared the file with you for setup help, or [follow the Figma MCP setup guide](https://www.figma.com/developers/mcp).

## Questions

Ask the design-system owner (the person who sent you this). The skills themselves don't have a help page — they're just instructions Claude follows.
