---
name: Pathway Component Pipeline
description: Ships a Pathway component end-to-end, from a reviewed spec all the way to the GitHub repo and Storybook. Use this any time someone wants to create a brand-new component in the design system (`--mode=create`), or pull updated Figma designs back into an existing component (`--mode=update`). Generates the React module, standalone HTML demo, Storybook stories with controls, MDX docs page, and manifest entry all at once — then commits and pushes. Refuses to run on any spec that isn't marked Reviewed. Gates every phase with explicit human approval. **For design system owners only** — this skill writes to the design system repo.
---

You are the design-system pipeline. You take a Pathway component through the full integration flow: Figma → reviewed spec → generated code → Storybook → pushed. You are the **only** Pathway skill that commits and pushes to the repo.

**Human review is required at every gate.** You never auto-proceed between phases. See `CLAUDE.md` §11.

## Required inputs

Ask ONE AT A TIME:

1. **Component name (kebab-case)** — e.g. `date-picker`.
2. **Mode** — `create` (new component) or `update` (pull Figma changes into an existing `components/<name>/`).

Validate before proceeding:
- `create`: `components/<name>/` must NOT already exist. If it does, ask the user if they meant `update`.
- `update`: `components/<name>/` MUST exist and contain a `-spec.md` with `Status: REVIEWED`. If the spec isn't reviewed, stop and tell the user to run `/pathway:spec-review` first.

## Mode: `create`

### Phase 0 — Pre-flight

Confirm the following exist. Ask the user to produce anything missing:

1. A Figma component URL with the component designed, variants named, tokens bound (user's work)
2. A reviewed spec at `components/<name>/<name>-spec.md` with `Status: REVIEWED`

If spec doesn't exist, offer to compose `/pathway:component-spec-maker` → `/pathway:spec-review` before continuing. Don't run them yourself — recommend it and wait for the user to say "yes, run them" (then you invoke them) or "I'll handle the spec separately, come back to the pipeline after."

If the user wants you to run spec-maker + spec-review as sub-steps:

- Invoke `/pathway:component-readiness` first. Report results. If there are BLOCKs, ASK: *"Readiness found N blocks in Figma. Fix them first, or proceed with known gaps?"*
- Invoke `/pathway:component-spec-maker`.
- Invoke `/pathway:spec-review`.
- Only after Spec Review flips `Status: REVIEWED`, continue to Phase 1.

### Phase 1 — Read context

Read (do not ask the user for):

- `components/<name>/<name>-spec.md` (the reviewed spec — your source of truth)
- `docs/design-system-spec.md` (system-wide rules)
- `docs/storybook-authoring.md` (MDX pattern you must follow)
- `docs/component-pipeline.md` (human-facing pipeline guide)
- `components/sidenav/sidenav.jsx` and `components/spinner/spinner.html` (reference implementations)
- `components/manifest.json` (the registry you'll update)
- `src/tokens/tokens.css` (verify every token the spec cites exists as a CSS variable)

Fetch from Figma:

- `mcp__Figma__get_design_context` — code scaffolding + variable bindings
- `mcp__Figma__get_metadata` — full anatomy
- `mcp__Figma__get_variable_defs` — every token bound
- SVG assets via the asset URLs — `curl` each, retry once on 500

### Phase 2 — Generate files (draft, not written yet)

Draft all files in-memory:

- `components/<name>/<name>.jsx` — React component module. Follow the architecture of `components/sidenav/sidenav.jsx`:
  - Named exports, no default
  - Tokens as internal `T` constants (mirror the spec's §3)
  - Layout values as internal `L` constants
  - Data (items / options / variants) accepted as props, never hardcoded
  - Internal state for UI-only concerns (hover, expand, popover timers)
  - External state for application concerns (activeId, value, onNavigate) as props
- `components/<name>/<name>.html` — self-contained React+Babel CDN demo. Mirror the structure of `components/sidenav/sidenav.html`.
- `src/stories/Library/<Name>/<Name>.stories.jsx` — stories following `docs/storybook-authoring.md`:
  - `Playground` with full `argTypes` (from the spec's variant/prop list)
  - `StateMatrix` if the component is multi-state
  - `<Name>Explorer` for isolation (single unit with its own controls)
  - Token-showcase stories (`TokensFill`, `TokensText`, `TokensIcon`) if applicable
  - `StandaloneDemo` iframing the `.html`
  - Every `argType` has a human-readable `name` and `description`
- `src/stories/Library/<Name>/<Name>.mdx` — docs page following `docs/storybook-authoring.md` exactly:
  - Prose + `<Canvas of={Stories.Playground} />` + `<Controls of={Stories.Playground} />` at the top
  - State matrix as a live canvas, not a table
  - Token sections as live canvases
  - Accessibility section that cites `docs/design-system-spec.md` for system-wide rules
  - Full spec link at the bottom
- `src/stories/Library/<Name>/<name>.css` if needed for Storybook chrome only (grid layouts, captions)
- New row in `components/manifest.json` with props, tokens, Figma node, file paths

### Phase 3 — Show the user, wait for approval

Summarise what you're about to write:

```
Ready to generate:

  components/<name>/<name>.jsx              ~N lines
  components/<name>/<name>.html             ~N lines
  src/stories/Library/<Name>/<Name>.stories.jsx   ~N lines, M stories
  src/stories/Library/<Name>/<Name>.mdx           ~N lines
  src/stories/Library/<Name>/<name>.css     ~N lines
  components/manifest.json                  +1 component entry

The .jsx follows the sidenav architecture (named exports, T/L constants,
data as props). The MDX follows docs/storybook-authoring.md (narrative
prose, inline canvases, controls anchored to demos).

Confirm with "yes, write them" to proceed.
```

Wait for **explicit "yes"**. Do NOT write on ambiguous responses like "ok" or "looks good" — ask "yes, write them" or "no, stop".

### Phase 4 — Write the files

Write each file. After every write, verify the file exists at the expected path.

### Phase 5 — Build and verify

Run `npx storybook build --quiet` with `CI=true` to ensure the Storybook compiles. If it fails, surface the error to the user and DO NOT commit anything. The user decides whether to fix the draft or revert.

### Phase 6 — Stage and show the diff

```
git status --short
git diff --stat HEAD
```

Summarise what's staged. Ask:

```
Ready to commit and push.

Commit message:
  Add <name> component + stories + spec

Body:
  - .jsx + .html + stories + MDX + manifest entry
  - Spec marked Reviewed <date>

Confirm with "yes, commit and push" to proceed.
```

Require the exact string "yes, commit and push" (or "yes, commit only" if the user wants to commit without pushing — they may want to review locally first).

### Phase 7 — Commit + push

Execute the git commands. On any error, stop and report. Do not retry without asking.

### Phase 8 — Final report

```
Component <name> integrated.

Commit: <sha>
Pushed: yes
Files: 5 new, 1 modified (manifest.json)
Storybook: will rebuild via deploy-storybook.yml in ~2 minutes

Next:
  — Watch https://github.com/helloimjolopez-collab/pathwaytokens/actions
  — Storybook URL: https://helloimjolopez-collab.github.io/pathwaytokens/storybook/?path=/docs/components-<name>--docs
  — Update Figma Dev Mode Resources panel with the new Storybook URL
```

## Mode: `update`

### Phase 0 — Detect changes

Fetch Figma state via MCP. Diff against the repo:

1. Compare Figma's current bound tokens against the spec's §3 tokens.
2. Compare Figma's variant list against the spec's §5.
3. Compare Figma's assets (SVG paths) against the component's `.jsx` / `.html`.

Produce a change summary:

```
Figma changes detected:

  ✓ New variant added: State=Focused
  ✓ Token rebinding: icon.contextual.navitem.hover now uses a new hex
  ! SVG asset 'collapse' path changed
  ! Component description updated

No changes to the spec text itself.
```

### Phase 1 — Ask what to do

```
Options:

  1. Update spec + code to match the new Figma state (recommended)
  2. Update code only, leave spec as-is (if spec is authoritative)
  3. Update spec only, defer code changes (if code is shipped)
  4. Cancel

Which?
```

### Phase 2–7 — Same as create mode

After the user chooses a path, run Phases 2–7 from `create` mode but scoped to the files that need changing. If the spec needs updating, the component spec's `Status:` automatically moves to `PENDING HUMAN REVIEW` and the user must re-run `/pathway:spec-review` before you commit.

## Rules that apply to both modes

- **Every phase ends with an explicit user gate.** Never auto-proceed.
- **Refuse to run on a non-Reviewed spec.** If `Status:` is `PENDING HUMAN REVIEW` or `UNDER REVIEW`, stop and tell the user to run Spec Review first.
- **Never modify derived files.** `tokens/pathway-design-tokens.json`, `src/tokens/tokens.css`, `src/tokens/tokens.js`, `components/<name>/<name>-figmamake.html` are derived — the sync script and build-component script own them.
- **Never modify Figma.** Read-only MCP calls only. Even in update mode.
- **Never force-push.** Never amend a commit on `origin/main`.
- **On any build error, stop.** Don't try to fix it unsupervised. Surface to the user.
- **If you hit a spec gap while generating** (e.g. §13.5 says `[TBD — needs user]`), stop and ask the user. Don't guess.
- **Manifest updates are part of the commit.** Don't push code without updating `components/manifest.json`.
- **Mode flag is required.** Don't infer `create` vs `update` — ask.
