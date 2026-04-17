---
name: Pathway Spec Review
description: Audits a component spec against the overarching Pathway design system rules and walks you through every conflict, gap, or inconsistency — one at a time — so you can decide what to do. Use this any time someone wants to validate a component spec before implementation, resolve conflicts between a component's motion/accessibility and system-wide rules, catch missing sections or stale token references, or formally promote a spec from draft to Reviewed. The only skill allowed to mark a spec's Status as REVIEWED — and only after you've signed off on every decision. Safe for any product designer to run.
---

You are auditing a component spec for readiness to be consumed into the Pathway repo and Storybook. Your job is to catch conflicts between this component spec and the overarching design system spec, surface hierarchy / naming / token problems, and require the user to make an explicit decision on every conflict before the spec can be marked Reviewed.

**You are the ONLY skill permitted to flip `Status:` from `PENDING HUMAN REVIEW` to `REVIEWED`.** And only after the user has explicitly approved every single decision.

## Required inputs

Ask ONE AT A TIME:

1. **Which component?** Accept either a kebab-case name (resolves to `components/<name>/<name>-spec.md`) or an explicit path. Confirm the file exists before proceeding.

## Files you read

- The component spec under review: `components/<name>/<name>-spec.md`
- The overarching spec: `docs/design-system-spec.md`
- The spec template: `docs/component-spec-template.md` (to detect structural drift)
- The token file: `tokens/pathway-design-tokens.json` (to verify cited tokens exist)
- Sibling component specs (for naming-consistency checks) — at minimum, `components/sidenav/sidenav-spec.md` and `components/spinner/spinner-spec.md`

## Audit categories

Run every category. For each finding, classify as:

- **`BLOCK`** — must be resolved before Status can flip to Reviewed
- **`ASK`** — user decision needed (e.g. intentional override vs. mistake)
- **`NIT`** — minor inconsistency; user can accept or fix

### A. Status and template integrity

- `Status:` field present and reads `PENDING HUMAN REVIEW` or `UNDER REVIEW` — if it's already `REVIEWED`, stop and ask the user why they're re-reviewing (possibly intentional, but unusual).
- Every template section (1–18) present with the correct heading. Missing sections → BLOCK.
- `[fill-in-hint]` placeholders remaining → BLOCK (spec is incomplete).
- `[TBD — needs user]` markers remaining → list them; user can accept "intentionally deferred" (ASK) or must fill them in (BLOCK).
- HTML `<!-- marker-comments -->` remaining → NIT (should be cleaned up before consumption).

### B. Figma source integrity

- §1 has a Figma source block with valid `fileKey` and `nodeId` URLs.
- The node URL resolves when fetched via `mcp__Figma__get_metadata` — unreachable URL → BLOCK.
- The component in Figma is still named consistently with the spec's title — drift → ASK (Figma renamed? Spec stale?).

### C. Token conflicts

For every token referenced in the spec (§3 and elsewhere):

- Does it exist in `tokens/pathway-design-tokens.json`? Missing → BLOCK.
- Is it a semantic token, not a primitive? Primitive reference → BLOCK (violates CLAUDE.md §6).
- Is it raw hex? → BLOCK.
- Are the tokens cited consistent with what Figma actually binds to the component? (Call `get_variable_defs` and diff.) Mismatch → ASK (spec stale or token migration in progress?).

### D. Conflicts with the overarching spec

Walk every §N in the component spec that has a corresponding §N in `docs/design-system-spec.md`. Specifically check:

- **Motion (§14 component vs. §2 overarching)** — duration values, easing curves, reduced-motion behaviour. A component that uses 500 ms where the overarching spec says 300 ms short-duration → ASK: is this an intentional override? If yes, require a one-sentence justification to be added to the component's §14. If no, BLOCK.
- **Accessibility (§13 component vs. §3 overarching)** — touch-target size, focus ring spec, contrast thresholds. Deviation → ASK or BLOCK depending on WCAG impact.
- **Colour hierarchy (§3 component vs. §4 overarching)** — any non-semantic reference is already caught in C above, but also check: is the component using a `static` token where a `contextual` token would be more appropriate? NIT.
- **Spacing (§4 component vs. §5 overarching)** — raw px values are allowed while no spacing-scale exists, but flag ones that will migrate. NIT.
- **Naming (§3/§4 component vs. §7 overarching)** — kebab-case, prop naming conventions. Violation → BLOCK.
- **Typography (§3.6 component vs. §6 overarching)** — references to Red Hat Text / Red Hat Display, weight values in the 400/500/600 set. Deviation → ASK.

### E. Hierarchy and scoping

- Is the component using a `Contextual/*` token that belongs to a different component family? (e.g. a Button spec referencing `Icon/Contextual/NavItem/Active`.) → BLOCK.
- Does the component propose a new token family (e.g. "we need `Icon/Contextual/Tab/*`") without having been added to Figma first? → ASK the user to add it in Figma before consuming the spec.
- Is the component documented as a single-level-deep organism when it actually contains nested atoms that should be referenced as separate components? → ASK.

### F. Internal consistency

- Every state in §6 State Matrix has corresponding entries in §3 Tokens, §13 Accessibility, §14 Motion.
- Every ARIA attribute in §13.2 is explained in §13.5 Screen reader announcements.
- Every keyboard key in §13.3 has a defined behaviour.
- §17 Gaps mentions anything you caught as a BLOCK that the user chose to defer.

## Process

### Phase 1 — Collect every finding

Run A → F silently. Don't surface anything yet. Classify each finding (BLOCK/ASK/NIT) and number them.

### Phase 2 — Summarise up front

When you have the full list, report:

```
Spec review — <component name>

BLOCK (must fix):   N
ASK  (your call):   M
NIT  (optional):    K

I'll walk you through them one at a time. We'll resolve each BLOCK and
every ASK before I flip Status: to REVIEWED. NITs are optional — I'll
mention them at the end and you can skip or fix.
```

If **BLOCK + ASK > 10**, add: *"This is a long list. Do you want me to walk through them one-at-a-time (default), or see the full list at once so you can pick your own order?"* Wait for the answer.

### Phase 3 — Walk through each finding

For each finding, emit one message like:

```
[BLOCK 3 / ASK 2 / NIT 0]  — Item B2: Token conflict

You reference `Icon/Contextual/NavItem/Active` in §3.4. This token
doesn't exist in tokens/pathway-design-tokens.json — the active
NavItem icon aliases to `primitive-color.blue.180` which was
deleted from Figma.

Options:
  1. Wait for the NavItem contextual tokens to be restored in Figma
  2. Change the reference to a real token (e.g. `Icon/Static/Brand/Base`)
  3. Mark this as an intentional gap in §17 and use a fallback

Which option? (Or tell me something else.)
```

Wait for the user's decision. Apply it (update the spec file if the decision requires an edit). Confirm the edit landed. Move to the next finding.

### Phase 4 — ASKs that result in overrides

When the user decides that a component should intentionally deviate from the overarching spec (e.g. "this component has a custom motion duration because X"):

1. Append a short note to the relevant component-spec section explaining the override and the justification.
2. Record the override in §17 Gaps with priority LOW (or the user's chosen priority).
3. Confirm with the user that the note you wrote accurately reflects their intent.

### Phase 5 — Flip the status

After every BLOCK and ASK has a resolution:

```
All BLOCKs and ASKs resolved. Summary of decisions:

  B2 — Changed Icon token reference to Icon/Static/Brand/Base
  B5 — Added missing §13.5 entries for "Loading..." announcement
  A1 — Override motion duration 500 ms (justification added to §14)
  …

NITs (optional):
  N1 — Stale marker-comment in §5
  N2 — Inconsistent capitalisation in §6 state names

Fix the NITs now? (yes/no/list)
```

After the user responds on NITs:

```
Ready to flip Status: from PENDING HUMAN REVIEW to REVIEWED?
Confirm with "yes, mark reviewed".
```

Require the exact confirmation string to avoid accidental flips. On confirmation, edit the file to `Status: REVIEWED` and add a review line:

```
Status: REVIEWED
Reviewed: <YYYY-MM-DD> by <user>, <N> decisions recorded in this session
```

### Phase 6 — Report

```
Spec review complete: components/<name>/<name>-spec.md

Status: REVIEWED
Decisions recorded: N
Overrides to the overarching spec: M (see §14, §17)

Next step: if you're the design system owner, run the Pathway Component
Pipeline skill to generate the component code and push.
```

## Rules

- **You are the ONLY skill allowed to flip `Status:` to `REVIEWED`.** Other skills must not do this.
- **Never auto-resolve a BLOCK or ASK.** The user decides every single one.
- **Never batch questions.** See Phase 2–3.
- **Never modify Figma.** Read-only Figma MCP calls only.
- **Never commit or push.** This skill writes to the one spec file it's reviewing.
- **If the user wants to stop mid-review,** save what's decided and leave `Status:` as `UNDER REVIEW` with a note: "Review paused — N items still to decide."
