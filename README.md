# Jo's Claude plugins

This repo is my collection of Claude Code plugins. Each plugin lives in its own folder at the root of this repo.

## What's in here

| Folder | Plugin | What it does |
|---|---|---|
| [`Pathway Tooling Claude Plugin/`](./Pathway%20Tooling%20Claude%20Plugin/) | Pathway | Five skills for the Pathway Design System (Ministry Brands Amplify). Covers the full lifecycle of a component — from Figma handoff check through to a shipped component in GitHub and Storybook. |
| [`MB Smart Product Wiki Claude Plugin/`](./MB%20Smart%20Product%20Wiki%20Claude%20Plugin/) | MB Smart Product Wiki | Three skills for product teams — a Knowledgebase Maker (builds a product knowledgebase from a guided interview + live walkthrough), a Knowledgebase Assistant (helps you query, update, and audit your KB), and a Persona Maker (builds data-informed user personas). |

More plugins will be added as I build them.

## How to install a plugin

Every plugin folder has its own `README.md` with specific instructions, but the short version is the same for all of them:

1. Open the plugin's folder
2. Download the `.zip` labelled as the full plugin
3. In Claude Code: **Customize** → **Personal plugins** → click `+` → upload the zip
4. Done — the plugin appears in your Customize tab with its skills available as `/` commands

## How to share ONE skill with a teammate

Each plugin folder has an `Individual Skills/` subfolder with every skill as a single `.md` file (and a `.zip` of it). Send the `.md` (or the `.zip` when the skill has a `references/` folder) to a teammate; they drop it into `~/.claude/commands/` on their Mac and it becomes a slash command.

The plugin's own README tells you which file to send for each skill — some skills are complete as a single `.md`, others need the zipped version because they ship with supporting files.

## Folder structure convention

Every plugin in this repo follows the same layout, so you always know where to look:

```
<Plugin Display Name>/
├── README.md                                  plugin-specific install docs
├── <Plugin Display Name> (Full).zip           upload this to Customize tab
├── plugin-source/                             unzipped contents of above
│   ├── .claude-plugin/plugin.json
│   └── skills/
│       └── <skill-name>/SKILL.md (+ references/ where needed)
└── Individual Skills/
    ├── README.md
    ├── <skill-name>.md                        raw skill file
    └── <skill-name>.zip                       zipped copy (includes references if any)
```

## Author

Jo Lopez · jo.lopez@ministrybrands.com
