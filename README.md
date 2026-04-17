# pathway-claude-skills

Claude Code plugin for the Pathway Design System (Ministry Brands Amplify). Bundles the tooling that turns Figma work into shipped components in the `pathwaytokens` repo and Storybook.

**Audience.** Any Amplify designer can install this plugin and use the three shareable skills (readiness, spec-maker, spec-review). The full pipeline and token-sync skills are intended for the design-system owner only — they write to and push the `pathwaytokens` repo.

---

## What's in the plugin

Once installed, five commands become available in Claude Code under the `/pathway:` namespace:

| Command | Audience | What it does |
|---|---|---|
| `/pathway:component-readiness` | Any designer | Checks a Figma component against the Pathway prep checklist. Read-only. |
| `/pathway:component-spec-maker` | Any designer | Drafts a `<name>-spec.md` from a Figma component using the Pathway template. Marks output `Status: PENDING HUMAN REVIEW`. |
| `/pathway:spec-review` | Any designer | Audits a draft spec against the overarching design system spec. Walks the user through every conflict. The only skill that can flip `Status:` to `REVIEWED`. |
| `/pathway:component-pipeline` | DS owner | Full repo integration: generates `.jsx` + `.html` + stories + MDX + manifest, commits, pushes. Two modes — `create` and `update`. |
| `/pathway:tokens-sync` | DS owner | Syncs the Figma token export into the repo, rebuilds Style Dictionary + Storybook, commits and pushes. |

All skills gate every step with explicit human approval. None auto-proceed. None batch questions.

---

## Install

You'll need:

- Claude Code installed
- Access to this private GitHub repo (you're a member of the Ministry Brands org, or have been added as a collaborator)
- A `GITHUB_TOKEN` environment variable set in your shell with `repo` scope, so Claude Code can pull from the private repo

In any Claude Code session, run:

```
/plugin marketplace add helloimjolopez-collab/pathway-claude-skills
/plugin install pathway@pathway-claude-skills
/reload-plugins
```

Type `/pathway:` and you should see all five commands in the completions list.

To update later:

```
/plugin update pathway@pathway-claude-skills
```

To uninstall:

```
/plugin uninstall pathway@pathway-claude-skills
```

---

## What you need locally to use the skills

The skills assume you have the Pathway design system repo cloned at `/Users/<you>/Desktop/FigmaWork/pathwaytokens/`. They also need:

- Figma MCP server configured (to read Figma components)
- `gh` CLI authenticated (the pipeline skill commits and pushes)
- `node` + `npm` (for the token-sync and pipeline skills)

The skills read rules from these files in the `pathwaytokens` repo:

- `CLAUDE.md` — agent rules (source of truth, human-review principle)
- `docs/design-system-spec.md` — overarching system spec (motion, accessibility, colour, spacing)
- `docs/component-spec-template.md` — the template spec-maker copies from
- `docs/figma-prep-checklist.md` — the checklist readiness runs against
- `docs/storybook-authoring.md` — MDX standards the pipeline follows

If any of these files are missing or have drifted from what the skills expect, the skills will say so.

---

## How the skills compose

```
Designer prepares Figma (follows docs/figma-prep-checklist.md)
  │
  ▼
/pathway:component-readiness <figma-url>
  │         (reports ✓/✗/⚠/? against the checklist)
  ▼
/pathway:component-spec-maker <component-name> <figma-url>
  │         (drafts -spec.md, marks PENDING HUMAN REVIEW)
  │         — Claude asks questions one at a time
  ▼
Designer reads the draft, fills in [TBD] markers
  │
  ▼
/pathway:spec-review <component-name>
  │         (walks through every conflict with the overarching spec)
  │         — user approves each decision
  │         — flips Status: to REVIEWED when all resolved
  ▼
/pathway:component-pipeline <component-name> --mode=create
          (DS-owner-only from here on)
          — generates .jsx + .html + stories + MDX + manifest
          — commits and pushes after explicit user approval
```

The first three skills are shareable with any designer. The pipeline skill writes to the `pathwaytokens` repo and should only be run by the DS owner.

---

## Development

Plugin files live in this repo. To edit a skill:

1. Clone this repo
2. Edit `commands/*.md` as needed
3. Either (a) commit and push, then `/plugin update` on each user's Mac, or (b) test locally first with `/plugin marketplace add ./path/to/this/repo` (overrides the cached version during development) and `/reload-plugins`

Versioning: bump `version` in `.claude-plugin/plugin.json` for any non-trivial change.

---

## License

UNLICENSED. Internal Ministry Brands tooling.
