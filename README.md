# Jo's Claude plugins

This repo is my collection of Claude Code plugins. Each plugin lives in its own folder at the root of this repo.

## What's in here

| Folder | Plugin | What it does |
|---|---|---|
| [`Pathway Tooling Claude Plugin/`](./Pathway%20Tooling%20Claude%20Plugin/) | Pathway | The Pathway Design System tooling for Ministry Brands Amplify. Five skills covering the full lifecycle of a component — from Figma handoff check through to a shipped component in GitHub and Storybook. |

More plugins will be added as I build them.

## How to install a plugin

Every plugin folder in this repo has its own `README.md` telling you exactly what to do, but the short version:

1. Open the plugin's folder
2. Download the `.zip` file that's labelled as the full plugin
3. In Claude Code, go to **Customize** → **Personal plugins** → click the `+` button
4. Upload the zip
5. Done — the plugin appears in your Customize tab with all its skills available as `/` commands

If you only want ONE skill from a plugin (not the whole thing), open the plugin folder and go into `Individual Skills/`. Each skill is available there as a single `.md` file (and a zipped version of it). Send the `.md` to a teammate, they save it to `~/.claude/commands/` on their Mac, and it works as a flat slash command.

## Author

Jo Lopez · jo.lopez@ministrybrands.com
