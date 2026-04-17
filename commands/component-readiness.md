---
name: Pathway Component Readiness
description: Check a Figma component against the Pathway prep checklist and report what's missing, wrong, or OK. Read-only — never modifies Figma or the repo. Shareable with any product designer.
---

You are auditing a Figma component against the handoff checklist in `/Users/jotemporary/Desktop/FigmaWork/pathwaytokens/docs/figma-prep-checklist.md`. Run every item, report findings in a clean block, and recommend concrete fixes for anything that fails.

## Input

The user should pass a Figma node URL or a `fileKey + nodeId` pair, e.g.:

- `https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/...?node-id=40006622-50003`
- `fileKey=3sw45aVcngFAmpbP6cfrXP, nodeId=40006622:50003`

If neither was provided in the command arguments, ask the user for the URL before doing anything else. Do not guess.

## Tools to use

- `mcp__Figma__get_design_context` — code + variable bindings for the component
- `mcp__Figma__get_metadata` — structural overview, variant list, children
- `mcp__Figma__get_variable_defs` — which tokens are bound to this node
- `mcp__Figma__get_screenshot` — visual reference

Read `docs/figma-prep-checklist.md` first so you know the current checklist items (it can be updated independently of this command).

## Audit algorithm

For each of the six sections in the checklist, run the relevant Figma MCP calls and classify each item as:

- **OK** — visible and correct in the Figma export
- **MISSING** — not present in the Figma export when it should be
- **WRONG** — present but incorrect (e.g. raw hex instead of a semantic variable)
- **UNCHECKABLE** — cannot be verified via MCP alone (note it clearly so the user checks manually in Figma)

### 1. Component structure (checklist §1)

From `get_metadata` on the node:
- Is this a published component (look for `type: "COMPONENT"` or `"COMPONENT_SET"`) or a detached frame? Frame = MISSING "published component".
- Does the component use Auto Layout? Check the metadata for `layoutMode` — `NONE` means no auto layout.
- If the node is a Component Set, are its variants named with `Key=Value` property names? Look at `variantProperties` in the metadata. If you see properties like `Variant 2`, flag them WRONG.

### 2. Variables and tokens (checklist §2)

From `get_variable_defs` on the node:
- Count the number of variable bindings. A component with zero variable bindings is MISSING tokens — every colour, spacing, typography value should be bound.
- Inspect the variable paths. Any variable whose path starts with `Primitive:` (e.g. `Primitive: Color / Brand / 300`) is WRONG — components must reference semantic variables, never primitives directly.
- If `get_design_context` reveals raw hex values (`#3555a0`, `rgb(...)`) in the generated code instead of var references, that means some fills are un-bound. Flag those specific properties as MISSING.

### 3. States and interactions (checklist §3)

From `get_metadata` on the parent Component Set:
- List the variant values for a property named `State` (or similar). Expected: Base, Hover, Active, Focused, Disabled — at minimum.
- If no `State` variant exists at all, that's MISSING "every interactive state has its own variant".
- If some are present but others missing (e.g. Base/Hover/Active but no Focused), flag which specific states are missing.
- Transition timing + reduced-motion behaviour cannot be inspected via MCP — mark UNCHECKABLE and advise the user to verify that those annotations exist on the component page in Figma.

### 4. Responsive behaviour (checklist §4)

UNCHECKABLE via MCP in most cases — breakpoint frames are usually siblings, not children. Report which ancestors/siblings exist (from `get_metadata` on the parent) and advise the user to confirm that breakpoint frames are authored.

### 5. Accessibility (checklist §5)

- Touch target size: read `width` and `height` from the root node. If either is below 48px without a clear reason, flag WRONG. If both are ≥48px, OK.
- ARIA/focus annotations cannot be read via MCP. Mark UNCHECKABLE.
- Contrast cannot be computed from MCP alone. Mark UNCHECKABLE.

### 6. Handoff metadata (checklist §6)

- The Figma node URL is what the user gave us — always OK.
- Icon export format: if the component contains icons, call `get_design_context` and check whether image URLs returned are SVG-format. If they're PNG, flag WRONG.
- Component description: not surfaced reliably via MCP. Mark UNCHECKABLE.

## Output format

Emit exactly this block. Keep it scannable.

```
Figma component check — <component name from metadata>
File:  <fileKey>
Node:  <nodeId>
URL:   <original URL>

§1 Component structure
  ✓ Published as a Component Set
  ✗ Auto Layout not enabled on the root frame
  ✓ Variant property "State" has 4 named values: Base, Hover, Active, Focused
  ⚠ Variant property "Variant 2" — rename to something descriptive

§2 Variables and tokens
  ✓ 14 variable bindings found
  ✗ Fill "#3555a0" on the active state — not bound to a variable
  ⚠ `Surface.NavLight` binds to a primitive (Cool Neutral.0) instead of a semantic variable

§3 States and interactions
  ✓ Base, Hover, Active variants present
  ✗ Focused variant missing
  ✗ Disabled variant missing
  ? Hover timing / reduced-motion — check component page annotations

§4 Responsive behaviour
  ? Breakpoint frames not visible from this node — check siblings in Figma

§5 Accessibility
  ✓ Touch target 48×48px
  ? ARIA role/label — check component page annotations
  ? Colour contrast — verify with a contrast tool

§6 Handoff metadata
  ✓ Node URL captured
  ✓ Icons export as SVG
  ? Component description — check in Figma right panel

Summary: 2 fixes needed, 6 items to verify manually.
Highest-priority fixes:
  1. Bind the active-state fill to a semantic variable
  2. Add Focused and Disabled variants
```

Use `✓` for OK, `✗` for missing/wrong (needs a fix), `⚠` for present-but-concerning, `?` for UNCHECKABLE (needs human eyes).

End with a one-line summary of counts and the top-three highest-priority fixes.

## Rules

- Never auto-fix anything in Figma. This command is read-only — it reports only.
- If a Figma MCP call fails, retry once. On second failure, include the specific call that failed in the report and continue with what you can check.
- If the user gives a URL whose node clearly isn't a component (a whole page, a random frame), say so and ask for a component-level URL.
- Do not infer items not in the checklist. This command audits the checklist verbatim; expanding scope makes it unpredictable.
