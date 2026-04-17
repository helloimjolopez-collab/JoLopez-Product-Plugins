---
name: Pathway Tokens Sync
description: Sync Figma token export into the pathwaytokens repo, rebuild Style Dictionary and Storybook, commit and push. DS-owner skill.
---

Full pipeline: sync tokens from the Figma export, auto-fix naming, rebuild Style Dictionary and Storybook, push to GitHub.

Canonical rules for this repo live in `/Users/jotemporary/Desktop/FigmaWork/pathwaytokens/CLAUDE.md`. The ones that apply here:

- **The Figma export is the only source of truth for tokens.** Do not hand-edit `tokens/pathway-design-tokens.json`, `src/tokens/tokens.css`, or `src/tokens/tokens.js` — they are derived.
- **Do not patch broken aliases as part of this command.** `sync-tokens.js` drops orphans automatically with a warning. Report the warnings in the summary; do not patch them here.
  - *One-time data migrations are separate.* When Figma renames or restructures a primitive group (e.g. the past `Indigo → Brand` rename), a one-off script may be needed to rewrite existing references. Those are ad-hoc jobs the user requests explicitly — they belong in their own run, not bolted onto this command.
- **Dark mode is currently excluded** from the token sync. Do not re-enable it unless the user explicitly asks. See CLAUDE.md §2.1.
- **Kebab-case lowercase everywhere** in the token tree.

Steps:

1. `cd /Users/jotemporary/Desktop/FigmaWork/pathwaytokens`
2. Pull latest from remote: `git stash; git pull --rebase; git stash pop` (handle errors gracefully).
3. Check that `tokens/figma-export/pathwaytokens.json` exists. If not, tell the user: "No export file found at `tokens/figma-export/pathwaytokens.json`. Export your variables from Figma (Variables Import Export plugin) and save the JSON at that path first."
4. Run: `node scripts/sync-tokens.js`
   - The script logs any aliases dropped because their target was deleted in Figma, and any modes skipped (dark mode is filtered by design). Capture both for the summary.
5. Run: `node scripts/fix-tokens.js`
6. Verify no PascalCase or camelCase keys remain in `tokens/pathway-design-tokens.json`. Convert any that do, to kebab-case.
   - `grep -P '"[a-z]+[A-Z]' tokens/pathway-design-tokens.json`
   - `grep -P '"[A-Z][a-z]+[A-Z]' tokens/pathway-design-tokens.json`
7. Rebuild Style Dictionary: `node style-dictionary.config.js`
8. Rebuild Storybook: `npx storybook build --quiet`
9. Deploy Storybook: `rm -rf storybook && mv storybook-static storybook`
10. Stage everything: `git add tokens/ src/tokens/ storybook/`
11. Commit and push: `git commit -m "chore: sync tokens from Figma and rebuild Storybook $(date -u +%Y-%m-%dT%H:%M:%SZ)" && git push`
12. **Reconcile components against the new token set.** For each folder in `components/*/`:
    - Grep the component's files (`*.html`, `*.md`, any `src/stories/Library/<Name>/*`) for token names that no longer exist in `tokens/pathway-design-tokens.json` or in the CSS variables emitted to `src/tokens/tokens.css`.
    - If the component references any stale token:
      - Extract the Figma file key and node ID from the spec's `### Figma source` section (regex the `figma.com/design/<fileKey>/...?node-id=<nodeId>` URL).
      - Call `mcp__Figma__get_variable_defs` with that node ID to see what tokens Figma currently binds to that component.
      - If every token Figma reports exists in the new token set, update the component files (HTML, CSS, stories, spec) to use the current names. Commit per component with a message that says what changed and why.
      - If Figma itself references a missing token, or the Figma fetch fails after a retry, add the component to a "needs manual attention" list. **Do not guess** a replacement token.
    - After reconciling, rebuild Storybook again and commit: `npx storybook build --quiet && rm -rf storybook && mv storybook-static storybook && git add storybook/ && git commit -m "chore: rebuild Storybook after component reconciliation" && git push`.
    - See `pathwaytokens/CLAUDE.md` §3 for the full reconciliation algorithm.
13. Summarise in plain language: what changed in tokens, how many tokens, any aliases dropped (= variables deleted in Figma), any modes skipped (dark mode should always appear here until Pathway adopts it), whether push succeeded, components auto-reconciled (one line each), components needing manual attention (listed: name · file · stale token · reason · next step), and the Storybook URL (`https://helloimjolopez-collab.github.io/pathwaytokens/storybook/`).
14. If anything fails, show the error and suggest what to do.
