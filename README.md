# Jo's Claude plugins

> **NOT OPEN SOURCE. NOT FOR DOWNLOAD. NOT FOR REDISTRIBUTION.**
>
> This repository is public only because the Claude Code marketplace install mechanism requires it. You are NOT permitted to download, clone, fork, copy, redistribute, or mirror the contents, whether as a ZIP, via `git clone`, raw file URLs, or any other means. The only legitimate way to use these plugins is to install them through the Claude Code marketplace, as invoked automatically by the Claude Code client on authorized users' behalf.
>
> This rule applies to everyone, including colleagues at Ministry Brands.
>
> For permission to use the Work in any other way, contact Jo Lopez (jo.lopez@ministrybrands.com). See [LICENSE](./LICENSE) for the full terms.

This repo is the public home of the **MB Smart Product Wiki** Claude Code plugin.

## What's in here

| Folder | Plugin | What it does |
|---|---|---|
| [`MB Smart Product Wiki Claude Plugin/`](./MB%20Smart%20Product%20Wiki%20Claude%20Plugin/) | MB Smart Product Wiki | Four skills for product teams: a Knowledgebase Maker (builds a product knowledgebase from a guided interview plus a live walkthrough), a Knowledgebase Assistant (helps you query, update, and audit your KB), a Persona Maker (builds data-informed user personas), and a User Flows Mapper (maps the core user flows of an existing product and renders each as a customer journey map in Figma). |

The Pathway Design System plugin previously lived in this repo. It has moved to its own **private** repository at `helloimjolopez-collab/pathway-design-system-claude-plugin` and is no longer part of this marketplace.

## How to install a plugin (authorized users only)

Installation is through the Claude Code marketplace. If the author has authorized you and provided the marketplace reference, use it through Claude Code directly. Every plugin folder has its own `README.md` with specific instructions for the author's own reference. Direct download of zips or individual files from this repo is NOT a permitted install path and is covered by the prohibition in the [LICENSE](./LICENSE).

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
