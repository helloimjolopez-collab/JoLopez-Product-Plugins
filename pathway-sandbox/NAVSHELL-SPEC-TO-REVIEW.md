# NavShell — Spec to Review

**Designer:** Jo Lopez
**Date:** 2026-06-05
**Status:** PENDING HUMAN REVIEW

---

## 1. Spec

# NavShell — Pathway Design System Component Spec

**Status:** `PENDING HUMAN REVIEW`

Complete implementation reference for the Ministry Brands Amplify navigation shell. Covers the full-page layout that wraps every screen: TopNav, SideNav, and the main content area (ScreenTemplate). Agents reading this spec can produce a pixel-accurate, fully interactive implementation for any module, org, or page content by following the rules exactly.

---

## Authorship

Design (Jo Lopez) owns and signs off on: Status, Purpose, Layout, Breakpoints, Token reference, Accessibility intent, Usage rules.
Engineering owns and signs off on: prop types, ARIA implementation in code, browser-specific behaviour.
Neither signs off on the other's section.

---

## Links

| Artefact | URL |
|---|---|
| Figma (ScreenTemplate) | https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/?node-id=40006538-43236 |
| Figma (TopNav) | https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/?node-id=40007067-6508 |
| Figma (SideNav) | https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/?node-id=40003951-2927 |
| Storybook | https://helloimjolopez-collab.github.io/pathway-ds/storybook/?path=/docs/library-navshell--docs (pending) |
| HTML demo | https://helloimjolopez-collab.github.io/pathway-ds/components/nav-shell/nav-shell.html |

---

## Section index

| Section | Jump |
|---|---|
| §1 Purpose | [↓](#1-purpose) |
| §2 Anatomy | [↓](#2-anatomy) |
| §3 Design tokens | [↓](#3-design-tokens) |
| §4 Layout and spacing | [↓](#4-layout-and-spacing) |
| §5 Breakpoints | [↓](#5-breakpoints) |
| §6 Nested components | [↓](#6-nested-components) |
| §7 ScreenTemplate content area | [↓](#7-screentemplate-content-area) |
| §8 Interaction | [↓](#8-interaction) |
| §9 Agent implementation guide | [↓](#9-agent-implementation-guide) |
| §10 Accessibility | [↓](#10-accessibility) |
| §11 Motion | [↓](#11-motion) |
| §12 Responsiveness | [↓](#12-responsiveness) |
| §13 Figma gaps and deferred decisions | [↓](#13-figma-gaps-and-deferred-decisions) |

---

## 1. Purpose

The NavShell is the fixed application frame that wraps every page in Ministry Brands Amplify. It is not a page — it is the container that every page lives inside. Its three jobs:

1. **Identity and context** — always tells the user which module (ModuleSwitcher), which organisation (OrgSwitcher), and which sub-section (SideNav) they are in.
2. **Navigation** — provides access to all modules (TopNav), all pages within the current module (SideNav), and system-level actions (search, notifications, profile).
3. **Layout** — defines the stable three-zone layout (TopNav + SideNav + Content) that every module shares. This predictability is what makes the product feel like a system rather than a collection of pages.

The NavShell never changes between pages. The SideNav items change per module. The content area changes per page. Everything else is constant.

---

## 2. Anatomy

```
NavShell
├── TopNav (fixed, full width, 56px tall)
│   ├── Slot.RowStart
│   │   ├── [Hamburger button]       ← mobile only
│   │   ├── ModuleSwitcher           ← always present
│   │   └── OrgSwitcher              ← always present
│   └── Slot.RowEnd
│       ├── TopNavSearch             ← always present
│       ├── [Notification bells]     ← desktop only
│       ├── [more_vert button]       ← tablet + mobile
│       └── ProfileAvatar            ← always present
│
├── SideNav (fixed below TopNav, left edge)
│   ├── NavHeader                    ← module label + collapse button
│   ├── NavMenu                      ← nav items (tree pattern)
│   │   ├── [NavSectionLabel]        ← optional grouping label
│   │   └── NavItem (×N)             ← one per module page
│   └── NavFooter                    ← collapse toggle button
│
└── Shell.Main (scrollable content area)
    └── ScreenTemplate               ← page content (changes per page)
        ├── Slot.PageNavigation      ← Tabs (optional)
        ├── PageHeading              ← page title + subtitle + actions
        ├── ToolBar                  ← search + FilterChips + actions
        └── Section (×N)            ← section heading + card grid
```

The three zones are always present. Their dimensions change at breakpoints (see §5) but their presence is constant.

---

## 3. Design tokens

### TopNav surface (dark mode — brand blue)

| Token | CSS variable | Resolved | Usage |
|---|---|---|---|
| `fill.static.brand.base` | `--semantic-color-light-mode-fill-static-brand-base` | #2d4889 | Nav bar background |
| `fill.action.tertiary.base` (dark) | dark-mode | rgba(160,181,230,0.04) | OrgSwitcher resting fill |
| `stroke.action.tertiary.base` (dark) | dark-mode | rgba(160,181,230,0.16) | OrgSwitcher border |
| `fill.action.primaryinverse.hover` (dark) | dark-mode | rgba(10,18,35,0.16) | All nav controls hover |
| `text.action.mono.base` (dark) | dark-mode | #fbfbfb | All text + icons on nav |
| `fill.static.accent.amethyst.base` | `--semantic-color-light-mode-fill-static-accent-amethyst-base` | #dcd9ef | Profile avatar bg |
| `text.static.accent-amethyst.contrast` | `--semantic-color-light-mode-text-static-accent-amethyst-contrast` | #221e3f | Profile avatar initials |

### SideNav surface (light mode)

| Token | CSS variable | Resolved | Usage |
|---|---|---|---|
| `surface.nav.light` | `--semantic-color-light-mode-surface-nav-light` | #f9f9f9 | SideNav background |
| `stroke.static.neutral.light` | `--semantic-color-light-mode-stroke-static-neutral-light` | #f6f6f6 | Right border + section separators |
| `fill.contextual.navitem.hover` | `--semantic-color-light-mode-fill-contextual-navitem-hover` | rgba(69,77,94,0.06) | Nav item hover |
| `fill.contextual.navitem.active` | `--semantic-color-light-mode-fill-contextual-navitem-active` | #eef2fb | Active nav item fill |
| `text.contextual.navitem.base` | `--semantic-color-light-mode-text-contextual-navitem-base` | #484848 | Nav item label |
| `text.contextual.navitem.active` | `--semantic-color-light-mode-text-contextual-navitem-active` | #3555a0 | Active nav item label |
| `icon.contextual.navitem.base` | `--semantic-color-light-mode-icon-contextual-navitem-base` | #606060 | Nav item icon (resting) |
| `icon.contextual.navitem.active` | `--semantic-color-light-mode-icon-contextual-navitem-active` | #3555a0 | Active nav item icon |

### ScreenTemplate surface (light mode)

| Token | CSS variable | Resolved | Usage |
|---|---|---|---|
| `surface.canvas.light` | `--semantic-color-light-mode-surface-canvas-light` | #fafafa | Page background |
| `fill.static.neutral.light` | `--semantic-color-light-mode-fill-static-neutral-light` | #ffffff | Card background |
| `stroke.action.secondary-inverse.base` | `--semantic-color-light-mode-stroke-action-secondary-inverse-base` | #d2d2d2 | Card border + toolbar search border |
| `text.static.primary.base` | `--semantic-color-light-mode-text-static-primary-base` | #202020 | Page heading, card title |
| `text.static.secondary.base` | `--semantic-color-light-mode-text-static-secondary-base` | #484848 | Page subtitle |
| `text.static.secondary.subtle` | `--semantic-color-light-mode-text-static-secondary-subtle` | #606060 | Section heading, card body, search placeholder |
| `fill.action.tertiary.base` | `--semantic-color-light-mode-fill-action-tertiary-base` | #eef2fb | Active filter chip bg |
| `text.action.primary.base` | `--semantic-color-light-mode-text-action-primary-base` | #3555a0 | Active tab, active filter chip text |
| `fill.static.accent_amethyst.light` | `--semantic-color-light-mode-fill-static-accent-amethyst-light` | #f4f2fa | Badge background |
| `text.static.accent-amethyst.contrast` | `--semantic-color-light-mode-text-static-accent-amethyst-contrast` | #221e3f | Badge text |

---

## 4. Layout and spacing

### Shell structure

```
┌─────────────────────────────────────────────────────────────────┐
│ TopNav (position: fixed, height: 56px, z-index: 100)            │
├─────────┬───────────────────────────────────────────────────────┤
│ SideNav │ Shell.Main (overflow-y: auto)                         │
│ fixed   │  └── ScreenTemplate                                   │
│ top:56px│       px:36, pt:12, pb:56, gap:24                     │
│         │                                                       │
└─────────┴───────────────────────────────────────────────────────┘
```

### Contextual spacing tokens (from Figma ScreenTemplate)

| Property | Token | Value |
|---|---|---|
| Page horizontal padding | `contextual.page.padding.horizontal` | 36px |
| Page top padding | `contextual.page.padding.top` | 12px |
| Page bottom padding | `contextual.page.padding.bottom` | 56px |
| Page gap (between sections) | `contextual.page.gap.vertical` | 16px (gap-relaxed = 24px) |
| PageHeading padding-top | `contextual.page-heading.padding.top` | 8px |
| PageHeading padding-bottom | `contextual.page-heading.padding.bottom` | 16px |
| PageHeading gap | `contextual.page-heading.gap.vertical` | 8px |
| ToolBar padding-vertical | `contextual.toolbar.padding.vertical` | 8px |
| ToolBar gap-vertical | `contextual.toolbar.gap.vertical` | 8px |
| SectionHeading padding-vertical | `contextual.section-heading.padding.vertical` | 6px |
| Section padding-top | `contextual.section.padding.top` | 4px |
| Section padding-bottom | `contextual.section.padding.bottom` | 16px |
| Section gap | `contextual.section.gap.vertical` | 8px |
| Card padding | `contextual.card.padding.medium.horizontal` | 16px |
| Card gap | `contextual.card.gap.gap` | 12px |
| Card border-radius | `contextual.card.cornerradius.cornerradius` | 8px |
| Card border width | `contextual.card.border-width.base.base` | 0.5px |

> IMPLEMENTATION RULE: Toolbar search input uses border-radius 6px.
> This is NOT `cornerradius.medium` (8px). The 6px value is confirmed from Figma node 40006537:42540 and must not be changed to match the standard button radius.

---

## 5. Breakpoints

| Breakpoint | Width | SideNav state | Content margin | TopNav changes |
|---|---|---|---|---|
| **Desktop** | ≥1024px | Expanded (240px) or Collapsed (72px rail) — user controls | 240px or 72px | Full labels, 2× notification bells |
| **Tablet** | 768–1023px | Always collapsed rail (72px, no labels) | 72px fixed | Module label hidden, bells → more_vert |
| **Mobile** | <768px | Hidden by default, overlay on hamburger tap | 0 | Hamburger visible, org label abbreviated, initials 11px |

### Desktop SideNav states

The desktop SideNav has two user-controlled states:

- **Expanded (240px):** labels + icons visible. Push layout — content area shifts right.
- **Collapsed (72px):** icons only, labels hidden. Tooltip shown on hover. Push layout — content shifts to 72px.

The user can toggle between these by clicking the collapse button at the bottom of the SideNav, or the `left_panel_close` / `right_panel_close` icon in the nav header.

### Mobile overlay

On mobile, the SideNav slides in as a 240px overlay (not push) when the hamburger is tapped. A dark scrim covers the content area. Tapping the scrim or pressing Escape closes the SideNav.

> IMPLEMENTATION RULE: The SideNav never uses CSS `display: none`.
> All three states (expanded, collapsed, hidden) are controlled by `width` transition. This preserves focus management and avoids layout flicker.

---

## 6. Nested components

The NavShell is composed from these existing DS components. Each has its own spec and implementation files:

| Component | Role | Spec | Module |
|---|---|---|---|
| `TopNav` | Fixed top bar — module + org + search + profile | `components/top-nav/top-nav-spec.md` | `components/top-nav/top-nav.jsx` |
| `SideNav` | Left navigation rail — module page nav | `components/sidenav/sidenav-spec.md` | `components/sidenav/sidenav.jsx` |
| `SearchInput` / `TopNavSearch` | Search trigger inside TopNav | `components/search/search-spec.md` | `components/search/search.jsx` |
| `OrgSwitcher` | Org name trigger inside TopNav | `components/org-switcher/org-switcher-spec.md` | `components/org-switcher/org-switcher.jsx` |

The **ScreenTemplate** content area (PageHeading, ToolBar, FilterChips, Tabs, Cards) contains sub-components that are not yet standalone DS components. They are implemented inline in `nav-shell.html` with full token bindings. Each will become its own component in a future pipeline run. See §13.

---

## 7. ScreenTemplate content area

The ScreenTemplate fills the Shell.Main scrollable area. It is composed from four zones stacked vertically.

### 7.1 Slot.PageNavigation — Tabs

Optional. When present, renders above the PageHeading.

- Background: `surface.canvas.light` (#fafafa)
- Active tab: `text.action.primary.base` (#3555a0), font-weight 600
- Active indicator: 2px bar at bottom, `fill.action.primary.base` (#3555a0), border-radius 2px 2px 0 0
- Inactive tab: `text.static.secondary.subtle` (#606060), font-weight 500
- Border-bottom on the tab container: 1px `stroke.static.neutral.light`

### 7.2 PageHeading

- **Title:** `heading.page.base.semibold` — 24px / 600 / 30px line-height / 0.1px tracking
  - Color: `text.static.primary.base` = #202020
- **Subtitle:** `text.body.base.regular` — 16px / 400 / 22px / 0.1px tracking
  - Color: `text.static.secondary.base` = #484848
- **Padding:** top 8px, bottom 16px, gap 8px between title and subtitle
- **Trailing actions slot:** right-aligned, contains action buttons (Button DS component)

### 7.3 ToolBar

Stacks vertically at 8px gap. Default: one row.

**Leading slot:**
- Search bar: max-width 400px, min-width 200px, height 36px
  - Border: 0.75px `stroke.action.secondary-inverse.base` = #d2d2d2
  - **Border-radius: 6px** (NOT 8px — confirmed from Figma)
  - Padding: 8px horizontal
  - Search icon: 16×16, `icon.action.secondary-inverse.base` = #6b6b6b
- FilterChips: inline row, gap 4px, each chip min-height 48px
  - Resting: transparent background, `text.action.secondary.base` = #292724, font 12px/500
  - Active: `fill.action.tertiary.base` = #eef2fb, `text.action.primary.base` = #3555a0, font-weight 600
  - Border-radius: 4px (`border-radius.xs`) — NOT 8px

**Trailing slot:** action buttons (right-aligned)

### 7.4 Content sections

Each section has:
1. **SectionHeading:** 14px/600 uppercase, `text.static.secondary.subtle` (#606060), letter-spacing 0.6px, padding-vertical 6px
2. **Card grid:** `grid-template-columns: repeat(auto-fill, minmax(280px, 1fr))`, gap 8px

**Card anatomy:**
```
Card (bg: white, border: 0.5px #d2d2d2, radius: 8px, padding: 16px, gap: 12px)
├── CardHeading
│   ├── IconContainer (32×32, radius: 8px, color variant background)
│   │   └── Icon (16×16, Material Symbols Rounded)
│   └── CardTitle (16px/600/22px, text.static.primary.base = #202020)
├── Slot.CardSubtitle
│   └── Badge (bg: fill.static.accent_amethyst.light = #f4f2fa, radius: 12px)
│       └── label.badge.small.medium: 11px/500/16px, tracking 0.3px
│           color: text.static.accent-amethyst.contrast = #221e3f
└── CardBody (14px/400/20px, text.static.secondary.subtle = #606060)
```

Card icon color variants (32×32 container):
- **Accent/amethyst:** bg `fill.static.accent_amethyst.light` (#f4f2fa)
- **Positive/green:** bg `fill.static.positive.light` (#f0faf1)
- **Info/blue:** bg `fill.static.info.light` (#eef2fb)
- **Warning:** bg `fill.static.warning.light` (#fff8e1)

---

## 8. Interaction

### SideNav collapse (desktop)

- Clicking the NavHeader collapse button or NavFooter toggle button collapses/expands the SideNav.
- Width transitions with `380ms cubic-bezier(0.32,0.72,0,1)` (same easing as SideNav component).
- The Shell.Main margin-left matches the SideNav width and transitions simultaneously.
- Labels and section headings fade via `opacity` transition (200ms ease), not `display: none`.

### Mobile hamburger

- The hamburger button only appears at <768px viewports.
- Its sole purpose is to reveal/hide the module SideNav. There is no other trigger for this on mobile.
- When tapped, the SideNav animates in from the left at 240px width over the content (overlay, not push).
- A dark scrim (rgba(0,0,0,0.32)) covers the content. Tapping it closes the SideNav.
- Escape key also closes the SideNav and returns focus to the hamburger.

### OrgSwitcher

- Clicking the OrgSwitcher trigger opens a panel listing all organisations the user has access to.
- Clicking outside or pressing Escape closes it.
- The active org is highlighted with `fill.action.primaryinverse` in the panel.

### TopNavSearch (TopNavSearch component)

- Collapsed state: 48×48 icon button on the brand-blue surface.
- Tap/click expands into a 320px search bar with spring animation (350ms, cubic-bezier overshoot).
- Search icon inside the expanded bar collapses it. Escape also collapses.
- See `components/search/search-spec.md` for full spec.

---

## 9. Agent implementation guide

This section is written specifically for AI agents implementing the NavShell. Follow every rule precisely.

### 9.1 The single place to configure the shell

All content-level customisation happens through `SHELL_CONFIG` at the top of the implementation file. Never hardcode module names, org names, or nav items anywhere else.

```javascript
const SHELL_CONFIG = {
  // REQUIRED — the active module
  module: {
    id:       "giving",                // unique string
    label:    "Amplify Giving",        // display name in TopNav
    icon:     "volunteer_activism",    // Material Symbols Rounded ligature
    isCustom: false,                   // true ONLY for Amplify Home (uses custom SVG)
  },

  // REQUIRED — the active organisation
  org: {
    name:   "NorthPoint Church",
    campus: "Atlanta",                 // empty string if no campus
    // logoUrl: "https://..."          // omit for default no-logo behavior
  },

  // REQUIRED — the logged-in user (for profile avatar)
  user: {
    name:     "Jo Lopez",
    initials: "JL",                    // 2–3 chars, derived from name
    email:    "jo@northpoint.org",
  },

  // REQUIRED — the SideNav navigation items
  // Order matters — items appear in this exact order
  navItems: [
    {
      id:           "home",            // unique string
      label:        "Home",            // displayed label
      icon:         "home",            // Material Symbols Rounded ligature
      active:       true,              // true = current page (exactly one item)
      sectionLabel: undefined,         // optional: adds a NavSectionLabel above this item
    },
    {
      id:    "teams",
      label: "Teams",
      icon:  "group",
    },
    {
      id:    "projects",
      label: "Projects",
      icon:  "folder",
    },
  ],

  // REQUIRED — the current page content
  page: {
    title:     "Teams",
    subtitle:  "Manage your ministry teams and volunteers",
    tabs:      ["All Teams", "Active", "Archived"],   // empty array = no tabs
    activeTab: 0,
    toolbar: {
      searchPlaceholder: "Search teams...",
      filters:           ["All", "Active", "Archived"],
      activeFilter:      0,
    },
  },
};
```

### 9.2 Icon rules

- Every nav item icon must be a Material Symbols Rounded ligature name.
- To find an icon: browse `https://github.com/google/material-design-icons` (folder name = ligature) or search at `https://fonts.google.com/icons?style=rounded`.
- Active nav items use `FILL=1` (filled icon). Resting items use `FILL=0` (outlined).
- The Amplify Home module icon is a custom branded SVG (not a Material Symbol). Use `HomeModuleIcon` component. Do NOT replace it with `home` ligature.
- All other module icons use Material Symbols Rounded.

### 9.3 Layout rules — never break these

1. The TopNav is always `position: fixed; top: 0; z-index: 100; height: 56px`. Never change height.
2. The SideNav is always `position: fixed; top: 56px; left: 0; bottom: 0`. Never absolute.
3. The Shell.Main always has `margin-left` equal to the current SideNav width. This creates the push layout.
4. At mobile (<768px), margin-left is 0 and the SideNav is an overlay (z-index > 50).
5. The SideNav uses width transitions, never display:none. States: 240px (expanded), 72px (collapsed), 0px (mobile hidden).

### 9.4 Breakpoint behaviour — exactly as designed

**Desktop (≥1024px):**
- SideNav: 240px expanded OR 72px collapsed (user toggles)
- TopNav: full module label, 2× notification bells, no hamburger
- Content: margin-left matches SideNav width

**Tablet (768–1023px):**
- SideNav: always 72px rail (no labels, no collapse button action)
- TopNav: module icon only (label hidden), bells → more_vert, no hamburger
- Content: margin-left always 72px

**Mobile (<768px):**
- SideNav: 0px default, 240px overlay on hamburger tap
- TopNav: hamburger button visible (ONLY way to access SideNav on mobile), org label abbreviated, profile initials 11px
- Content: margin-left always 0

### 9.5 Organisation label abbreviation rules (mobile)

On mobile, the org trigger shows abbreviated text. The rules:

- **Org abbreviation:** Take first letter of first 3 significant words (skip articles: the, a, an, of, in, at, for, and, or). If 2 words, repeat last. Example: "NorthPoint Church" → "NPC".
- **Campus abbreviation:** If campus is a single place name, use first letter + first letter of suffix (e.g. "Knoxville" → "KV"). If two words, first letter each.
- **Full label:** `"NPC | KV"` (org abbreviation + pipe + campus abbreviation). No pipe if no campus.

### 9.6 What to never do

- Never hardcode `#2d4889` — always use `var(--semantic-color-light-mode-fill-static-brand-base, #2d4889)`.
- Never hardcode `#fafafa` — always use `var(--semantic-color-light-mode-surface-canvas-light, #fafafa)`.
- Never use `display: none` on the SideNav for any state — use `width: 0` with `overflow: hidden`.
- Never set `border-radius: 8px` on the ToolBar search input — it is 6px (confirmed from Figma).
- Never put `overflow: hidden` on Shell.Main — the content area must scroll.
- Never show a logo or avatar in the OrgSwitcher nav trigger unless `logoUrl` is explicitly provided.
- Never use `Material Icons` (outdated) — always `Material Symbols Rounded`.

### 9.7 Common implementation prompt template

When asking an agent to implement a page in the NavShell, use this template:

```
Use the Pathway NavShell from components/nav-shell/nav-shell.html.
Import TopNav from components/top-nav/top-nav.jsx and SideNav from components/sidenav/sidenav.jsx.

Configure SHELL_CONFIG as follows:
- Module: [MODULE_NAME] (icon: [ICON_LIGATURE])
- Org: [ORG_NAME], campus [CAMPUS]
- User: [NAME] ([INITIALS])
- Nav items: [ITEM_1 (active)], [ITEM_2], [ITEM_3]...
- Page title: [TITLE]
- Page subtitle: [SUBTITLE]
- Tabs: [TAB_1, TAB_2, ...]
- Filter chips: [FILTER_1, FILTER_2, ...]

For the card content, use [DESCRIBE_CONTENT].

Apply all Pathway semantic tokens. Do not hardcode colours. Follow nav-shell-spec.md §9.
```

---

## 10. Accessibility

| What | How |
|---|---|
| TopNav landmark | `<header role="banner">` |
| SideNav landmark | `<nav aria-label="Module navigation">` |
| Main content landmark | `<main id="main-content">` |
| Skip link | Add `<a href="#main-content">Skip to content</a>` as first focusable element |
| SideNav tree | `role="tree"` on nav menu, `role="treeitem"` on each item, `aria-current="page"` on active |
| Collapsed SideNav items | `aria-label={item.label}` on the button when labels are hidden |
| Mobile hamburger | `aria-label="Toggle navigation menu"`, `aria-expanded` reflects SideNav state |
| OrgSwitcher | `aria-haspopup="true"`, `aria-expanded` reflects panel state |
| TopNavSearch | Full spec in `components/search/search-spec.md §13` |
| Focus trap | Mobile SideNav overlay traps focus inside the nav panel |
| Touch targets | All interactive controls: 48×48px minimum (WCAG 2.5.5) |
| Reduced motion | All width/opacity transitions suppressed under `prefers-reduced-motion: reduce` |

---

## 11. Motion

| Element | Property | Duration | Easing |
|---|---|---|---|
| SideNav expand/collapse | `width` | 380ms | `cubic-bezier(0.32,0.72,0,1)` (accordion) |
| Shell.Main margin-left | `margin-left` | 380ms | same |
| Nav item labels | `opacity` | 200ms | `ease` |
| Nav section labels | `opacity` | 200ms | `ease` |
| TopNavSearch expand | `width` + opacity | 350ms | `cubic-bezier(0.34,1.56,0.64,1)` (spring) |
| OrgSwitcher panel | drop-in fade | 150ms | `ease-out` |
| Card hover | `box-shadow` + `border-color` | 150ms | `ease` |

All transitions registered as contextual overrides per `docs/design-system-spec.md §2.3`.

---

## 12. Responsiveness

Detailed per-breakpoint layout values (pixel-accurate from Figma and implementation):

### Desktop (≥1024px)

```
┌── TopNav: 100vw × 56px, px:16 ──────────────────────────────────┐
│ [ModuleSwitcher + label] [OrgSwitcher]       [Search][Bell×2][Avatar] │
├── SideNav: 240px × (100vh-56px) ───┬── Shell.Main: (100vw-240px) ┤
│ NavHeader: 48px                    │ ScreenTemplate: px:36 pt:12  │
│ NavMenu: flex-1, overflow-y: auto  │                              │
│ NavItem: 48px min-height           │                              │
│ NavFooter: 48px                    │                              │
└────────────────────────────────────┴──────────────────────────────┘
Collapsed rail: SideNav = 72px, Shell.Main = (100vw-72px)
```

### Tablet (768–1023px)

```
┌── TopNav: 100vw × 56px ─────────────────────────────────────────┐
│ [ModuleIcon only] [OrgSwitcher]              [Search][more_vert][Avatar] │
├── SideNav: 72px × (100vh-56px) ────┬── Shell.Main: (100vw-72px) ┤
│ Icons only, no labels              │ ScreenTemplate: px:36 pt:12 │
└────────────────────────────────────┴─────────────────────────────┘
```

### Mobile (<768px)

```
┌── TopNav: 100vw × 56px ─────────────────────────────────────────┐
│ [Hamburger][ModuleIcon][OrgSwitcher abbr]  [Search][more_vert][Avatar 11px] │
├── Shell.Main: 100vw ────────────────────────────────────────────┤
│ ScreenTemplate: px:16 pt:8                                       │
│ Card grid: 1 column                                              │
└─────────────────────────────────────────────────────────────────┘
SideNav: 240px overlay (z:50), positioned fixed top:56px, slides in from left
Scrim: rgba(0,0,0,0.32), covers full content area below TopNav
```

---

## 13. Figma gaps and deferred decisions

### ScreenTemplate sub-components — future standalone DS components

These are implemented inline in `nav-shell.html`. Each will get its own pipeline run:

| Component | Figma node | Status |
|---|---|---|
| `PageHeading` | Inside 40006538:43236 | Not yet a standalone DS component |
| `ToolBar` | `40006537:42570` | Not yet a standalone DS component |
| `FilterChip` | `40006526:36972` | Not yet a standalone DS component |
| `Tabs` | Inside 40006718:5996 | Not yet a standalone DS component |
| `Card` | Inside 40006538:42775 | Not yet a standalone DS component |
| `Badge` | Inside card | Not yet a standalone DS component |
| `SectionHeading` | `40006538:43364` | Not yet a standalone DS component |

### Token gaps

| Gap | Priority | Notes |
|---|---|---|
| ToolBar search border-radius 6px | MEDIUM | Not a standard token value. `cornerradius.medium` = 8px, `cornerradius.xs` = 4px. 6px is a raw value — no token. Flag for Figma to add as a contextual token. |
| `contextual.page.*` tokens not in token file | HIGH | Figma uses `--contextual/page/padding/horizontal`, etc. These are not in `tokens/pathway-design-tokens.json`. Raw values used as fallbacks. |
| `contextual.card.*` tokens not in token file | HIGH | Same issue — raw fallback values used. |
| `contextual.toolbar.*` tokens not in token file | MEDIUM | Same issue. |
| Mobile ScreenTemplate padding | LOW | Reduced to `px:16 pt:8` on mobile — no explicit Figma spec. Current values are reasonable defaults. |

---

## Changelog

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-06-05 | Jo Lopez + Claude | Initial draft. NavShell spec + HTML demo. Figma ScreenTemplate node 40006538-43236 read. TopNav + SideNav reuse existing components. ScreenTemplate sub-components inline pending standalone pipeline runs. |

---

## 2. Figma

- **ScreenTemplate (page content template):** [Open in Figma](https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/?node-id=40006538-43236)
- **TopNav:** [Open in Figma](https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/?node-id=40007067-6508)
- **SideNav:** [Open in Figma](https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/?node-id=40003951-2927)
- **File:** https://www.figma.com/design/3sw45aVcngFAmpbP6cfrXP/

---

## 3. Token reference

Key tokens — see nav-shell-spec.md §3 for full table.

| Token path | CSS variable | Resolved | Usage |
|---|---|---|---|
| fill.static.brand.base | --semantic-color-light-mode-fill-static-brand-base | #2d4889 | TopNav background |
| surface.canvas.light | --semantic-color-light-mode-surface-canvas-light | #fafafa | Page background |
| surface.nav.light | --semantic-color-light-mode-surface-nav-light | #f9f9f9 | SideNav background |
| fill.contextual.navitem.active | --semantic-color-light-mode-fill-contextual-navitem-active | #eef2fb | Active nav item |
| text.static.primary.base | --semantic-color-light-mode-text-static-primary-base | #202020 | Page heading |

---

## 4. Icon reference

| Icon | Ligature | Usage |
|---|---|---|
| Amplify Home module | custom SVG (HOME_ICON_PATH) | Do NOT replace with Material Symbol |
| Menu (hamburger) | menu | Mobile SideNav toggle |
| All standard UI icons | Material Symbols Rounded | Per CLAUDE.md §12 |

---

## 5. HTML demo

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>NavShell — Pathway Design System</title>

  <!-- Pathway tokens -->
  <link rel="stylesheet" href="../../src/tokens/tokens.css" />

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Red+Hat+Text:wght@400;500;600;700&family=Red+Hat+Display:wght@600&family=Material+Symbols+Rounded:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200&display=swap" rel="stylesheet" />

  <style>
    /* ── RESET ──────────────────────────────────────────────────────────────── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body, #root {
      height: 100%; overflow: hidden;
      font-family: 'Red Hat Text', sans-serif;
      background: var(--semantic-color-light-mode-surface-canvas-light, #fafafa);
    }
    ::-webkit-scrollbar { width: 6px; height: 6px; }
    ::-webkit-scrollbar-track { background: transparent; }
    ::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.12); border-radius: 4px; }

    .material-symbols-rounded {
      font-variation-settings: 'FILL' 0, 'wght' 400, 'GRAD' 0, 'opsz' 20;
      line-height: 1; display: block; user-select: none; flex-shrink: 0;
    }

    /* ── TOPNAV ─────────────────────────────────────────────────────────────── */
    .topnav {
      position: fixed; top: 0; left: 0; right: 0; z-index: 100;
      height: 56px;
      background: var(--semantic-color-light-mode-fill-static-brand-base, #2d4889);
      display: flex; align-items: center; justify-content: space-between;
      padding: 0 16px;
    }
    .topnav__start { display: flex; align-items: center; gap: 0; min-width: 0; }
    .topnav__end   { display: flex; align-items: center; gap: 4px; flex-shrink: 0; }

    /* Hamburger — mobile/tablet only */
    .topnav__hamburger {
      display: none; align-items: center; justify-content: center;
      width: 48px; height: 48px; border-radius: 8px;
      background: transparent; border: none; cursor: pointer; padding: 8px;
      color: rgba(251,251,251,0.9); flex-shrink: 0;
      transition: background 120ms ease;
    }
    .topnav__hamburger:hover { background: rgba(160,181,230,0.16); }

    /* Module switcher */
    .topnav__module {
      display: flex; align-items: center; gap: 4px;
      height: 36px; padding: 4px; border-radius: 8px;
      background: transparent; border: none; cursor: pointer;
      color: rgba(251,251,251,0.9); transition: background 120ms ease;
    }
    .topnav__module:hover { background: rgba(160,181,230,0.16); }
    .topnav__module-icon {
      width: 26px; height: 26px; display: flex; align-items: center;
      justify-content: center; flex-shrink: 0;
    }
    .topnav__module-label {
      font-size: 14px; font-weight: 500; line-height: 20px;
      letter-spacing: 0.3px; color: #fbfbfb;
      white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
      max-width: 160px;
    }
    .topnav__module-label.hidden { display: none; }

    /* Org switcher */
    .topnav__org {
      position: relative; padding: 4px 2px;
    }
    .topnav__org-btn {
      display: flex; align-items: center; gap: 4px;
      height: 36px; padding: 4px 6px 4px 12px; border-radius: 8px;
      background: rgba(160,181,230,0.04);
      border: 1px solid rgba(160,181,230,0.16);
      cursor: pointer; color: #fbfbfb; font-family: inherit;
      transition: background 120ms ease, border-color 120ms ease;
    }
    .topnav__org-btn:hover {
      background: rgba(160,181,230,0.16);
      border-color: rgba(160,181,230,0.20);
    }
    .topnav__org-btn.open {
      background: rgba(255,255,255,0.08);
      border-color: rgba(160,181,230,0.30);
    }
    .topnav__org-label {
      font-size: 14px; font-weight: 500; line-height: 20px;
      letter-spacing: 0.3px; color: #fbfbfb;
      white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
      max-width: 240px;
    }
    .topnav__org-label.mobile { max-width: 70px; }

    /* Search, action icons, profile */
    .topnav__search-btn {
      display: flex; align-items: center; justify-content: center;
      width: 36px; height: 36px; border-radius: 9999px;
      background: rgba(160,181,230,0.08);
      border: 0.5px solid rgba(251,251,251,0.4);
      cursor: pointer; padding: 8px;
      color: rgba(251,251,251,0.9); transition: background 120ms ease;
    }
    .topnav__search-btn:hover { background: rgba(160,181,230,0.16); }
    .topnav__action-btn {
      display: flex; align-items: center; justify-content: center;
      width: 48px; height: 48px; border-radius: 8px;
      background: transparent; border: none; cursor: pointer; padding: 8px;
      color: rgba(251,251,251,0.9); transition: background 120ms ease;
    }
    .topnav__action-btn:hover { background: rgba(10,18,35,0.16); }
    .topnav__profile-btn {
      display: flex; align-items: center; justify-content: center;
      width: 44px; height: 44px; border-radius: 50%;
      background: transparent; border: none; cursor: pointer; padding: 6px;
      transition: background 120ms ease;
    }
    .topnav__profile-btn:hover { background: rgba(10,18,35,0.16); }
    .topnav__avatar {
      width: 32px; height: 32px; border-radius: 50%;
      background: var(--semantic-color-light-mode-fill-static-accent-amethyst-base, #dcd9ef);
      display: flex; align-items: center; justify-content: center;
      font-size: 14px; font-weight: 600; letter-spacing: 0.3px;
      color: var(--semantic-color-light-mode-text-static-accent-amethyst-contrast, #221e3f);
      line-height: 1; flex-shrink: 0;
    }
    .topnav__avatar.mobile { font-size: 11px; }

    /* Org panel dropdown */
    .topnav__org-panel {
      position: absolute; top: calc(100% + 4px); left: 0; min-width: 260px;
      background: #fff; border: 1px solid #e5e7eb; border-radius: 8px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.10), 0 2px 6px rgba(0,0,0,0.06);
      z-index: 200; padding: 4px 0; overflow: hidden;
      animation: dropIn 150ms ease-out;
    }
    .topnav__org-panel-heading {
      padding: 6px 12px 4px;
      font-size: 11px; font-weight: 600; letter-spacing: 0.07em;
      text-transform: uppercase; color: #a0a0a0;
    }
    .topnav__org-item {
      display: flex; align-items: center; gap: 10px;
      padding: 8px 12px; cursor: pointer; border-radius: 0;
      transition: background 120ms ease;
    }
    .topnav__org-item:hover { background: #f5f5f5; }
    .topnav__org-item.active { background: #eef2fb; }
    .topnav__org-item-avatar {
      width: 32px; height: 32px; border-radius: 4px;
      display: flex; align-items: center; justify-content: center;
      font-size: 9px; font-weight: 700; color: #fff; flex-shrink: 0;
    }
    .topnav__org-item-name { font-size: 13px; font-weight: 500; color: #252525; }
    .topnav__org-item-campus { font-size: 11px; color: #6b6b6b; margin-top: 1px; }

    /* ── SHELL LAYOUT ───────────────────────────────────────────────────────── */
    .shell {
      display: flex; height: 100%; padding-top: 56px; /* TopNav height */
    }
    .shell__sidenav {
      position: fixed; top: 56px; left: 0; bottom: 0;
      display: flex; flex-direction: column;
      background: var(--semantic-color-light-mode-surface-nav-light, #f9f9f9);
      border-right: 1px solid var(--semantic-color-light-mode-stroke-static-neutral-light, #f6f6f6);
      z-index: 50;
      transition: width 380ms cubic-bezier(0.32,0.72,0,1);
      overflow: hidden;
    }
    .shell__sidenav.expanded  { width: 240px; }
    .shell__sidenav.collapsed { width: 72px; }
    .shell__sidenav.hidden    { width: 0; }

    /* Overlay (mobile) */
    .shell__overlay {
      display: none; position: fixed; inset: 56px 0 0 0; z-index: 49;
      background: rgba(0,0,0,0.32);
    }
    .shell__overlay.visible { display: block; }

    /* Main content area */
    .shell__main {
      flex: 1; min-width: 0;
      overflow-y: auto; overflow-x: hidden;
      transition: margin-left 380ms cubic-bezier(0.32,0.72,0,1);
    }
    .shell__main.expanded  { margin-left: 240px; }
    .shell__main.collapsed { margin-left: 72px; }
    .shell__main.full      { margin-left: 0; }

    /* ── SIDENAV INTERNALS ──────────────────────────────────────────────────── */
    .nav-header {
      display: flex; align-items: center; justify-content: space-between;
      padding: 8px 16px 8px 12px; min-height: 48px; flex-shrink: 0;
      border-bottom: 1px solid var(--semantic-color-light-mode-stroke-static-neutral-light, #f6f6f6);
    }
    .nav-header__logo {
      font-size: 13px; font-weight: 600; color: var(--semantic-color-light-mode-text-contextual-navitem-base, #484848);
      letter-spacing: 0.05em; text-transform: uppercase; white-space: nowrap;
      overflow: hidden; opacity: 1; transition: opacity 200ms ease;
    }
    .nav-header__logo.hidden { opacity: 0; }
    .nav-collapse-btn {
      display: flex; align-items: center; justify-content: center;
      width: 32px; height: 32px; border-radius: 6px;
      background: transparent; border: none; cursor: pointer; flex-shrink: 0;
      color: var(--semantic-color-light-mode-icon-contextual-navitem-base, #606060);
      transition: background 120ms ease;
    }
    .nav-collapse-btn:hover { background: var(--semantic-color-light-mode-fill-contextual-navitem-hover, rgba(69,77,94,0.06)); }

    .nav-menu { flex: 1; overflow-y: auto; overflow-x: hidden; padding: 8px 0; }

    .nav-item {
      display: flex; align-items: center; gap: 12px;
      min-height: 48px; padding: 0 12px; cursor: pointer;
      border-radius: 8px; margin: 0 8px 2px;
      color: var(--semantic-color-light-mode-text-contextual-navitem-base, #484848);
      text-decoration: none; border: none; background: transparent;
      font-family: inherit; font-size: 14px; font-weight: 400; line-height: 1;
      letter-spacing: 0.3px; white-space: nowrap;
      transition: background 120ms ease, color 120ms ease;
      position: relative;
    }
    .nav-item:hover {
      background: var(--semantic-color-light-mode-fill-contextual-navitem-hover, rgba(69,77,94,0.06));
    }
    .nav-item.active {
      background: var(--semantic-color-light-mode-fill-contextual-navitem-active, #eef2fb);
      color: var(--semantic-color-light-mode-text-contextual-navitem-active, #3555a0);
      font-weight: 500;
    }
    .nav-item.active .nav-item__indicator {
      position: absolute; left: -8px; top: 50%; transform: translateY(-50%);
      width: 4px; height: 24px; border-radius: 0 4px 4px 0;
      background: var(--semantic-color-light-mode-icon-contextual-navitem-active, #3555a0);
    }
    .nav-item__icon {
      width: 24px; height: 24px; display: flex; align-items: center;
      justify-content: center; flex-shrink: 0;
      color: var(--semantic-color-light-mode-icon-contextual-navitem-base, #606060);
    }
    .nav-item.active .nav-item__icon {
      color: var(--semantic-color-light-mode-icon-contextual-navitem-active, #3555a0);
    }
    .nav-item__label {
      flex: 1; min-width: 0; overflow: hidden; text-overflow: ellipsis;
      opacity: 1; transition: opacity 200ms ease;
    }
    .nav-item__label.hidden { opacity: 0; width: 0; }

    .nav-section-label {
      display: flex; align-items: center; min-height: 32px;
      padding: 0 16px; margin: 4px 8px 0;
      font-size: 11px; font-weight: 600; letter-spacing: 0.07em;
      text-transform: uppercase;
      color: var(--semantic-color-light-mode-text-static-secondary-subtle, #606060);
      white-space: nowrap; overflow: hidden;
      opacity: 1; transition: opacity 200ms ease;
    }
    .nav-section-label.hidden { opacity: 0; }

    .nav-footer {
      padding: 8px; border-top: 1px solid var(--semantic-color-light-mode-stroke-static-neutral-light, #f6f6f6);
      flex-shrink: 0;
    }

    /* Tooltip for collapsed items */
    .nav-item:hover .nav-tooltip {
      display: flex;
    }
    .nav-tooltip {
      display: none; position: absolute; left: calc(100% + 8px); top: 50%;
      transform: translateY(-50%); z-index: 200;
      background: var(--semantic-color-light-mode-text-contextual-navitem-base, #484848);
      color: #fff; font-size: 12px; font-weight: 500; padding: 4px 8px;
      border-radius: 4px; white-space: nowrap; pointer-events: none;
    }
    .nav-tooltip::before {
      content: ''; position: absolute; right: 100%; top: 50%;
      transform: translateY(-50%);
      border: 4px solid transparent; border-right-color: #484848;
    }

    /* ── SCREEN TEMPLATE (PAGE CONTENT) ────────────────────────────────────── */
    /* Token: contextual.page.padding.horizontal = 36px
       Token: contextual.page.padding.top = 12px
       Token: contextual.page.padding.bottom = 56px
       Token: contextual.page.gap.vertical = 16px
       Background: surface.canvas.light = #fafafa */
    .screen {
      min-height: 100%;
      padding: 12px 36px 56px;
      display: flex; flex-direction: column;
      gap: var(--semantic-layout-units-gap-relaxed, 24px);
      background: var(--semantic-color-light-mode-surface-canvas-light, #fafafa);
    }

    /* Tabs — page top navigation */
    /* Source: Figma ScreenTemplate Slot.PageNavigation */
    .page-tabs {
      display: flex; border-bottom: 1px solid var(--semantic-color-light-mode-stroke-static-neutral-light, #f6f6f6);
      margin-bottom: 0; flex-shrink: 0;
    }
    .page-tab {
      display: flex; align-items: center; justify-content: center;
      padding: 14px 16px; cursor: pointer; position: relative;
      font-size: 14px; font-weight: 500; line-height: 20px; letter-spacing: 0.3px;
      color: var(--semantic-color-light-mode-text-static-secondary-subtle, #606060);
      border: none; background: transparent; font-family: inherit;
      transition: color 120ms ease;
    }
    .page-tab:hover { color: var(--semantic-color-light-mode-text-static-primary-base, #202020); }
    .page-tab.active {
      color: var(--semantic-color-light-mode-text-action-primary-base, #3555a0);
      font-weight: 600;
    }
    .page-tab.active::after {
      content: ''; position: absolute; bottom: -1px; left: 0; right: 0;
      height: 2px; border-radius: 2px 2px 0 0;
      background: var(--semantic-color-light-mode-fill-action-primary-base, #3555a0);
    }

    /* Page heading */
    /* Source: Figma PageHeading component — node 40006538:42661
       heading.page.base.semibold: 24px/600/30px, text.static.primary.base = #202020
       subheading: text.body.base.regular: 16px/400/22px, text.static.secondary.base = #484848
       padding-top: 8px, padding-bottom: 16px, gap: 8px */
    .page-heading {
      display: flex; flex-direction: column;
      padding-top: 8px; padding-bottom: 16px;
      gap: 8px;
    }
    .page-heading__row {
      display: flex; align-items: flex-start; justify-content: space-between;
    }
    .page-heading__title {
      font-size: 24px; font-weight: 600; line-height: 30px; letter-spacing: 0.1px;
      color: var(--semantic-color-light-mode-text-static-primary-base, #202020);
    }
    .page-heading__subtitle {
      font-size: 16px; font-weight: 400; line-height: 22px; letter-spacing: 0.1px;
      color: var(--semantic-color-light-mode-text-static-secondary-base, #484848);
    }
    .page-heading__actions { display: flex; align-items: center; gap: 8px; }

    /* Toolbar */
    /* Source: Figma ToolBar component — node 40006537:42570
       padding-vertical: 8px, gap-vertical: 8px */
    .toolbar {
      display: flex; flex-direction: column;
      padding: 8px 0; gap: 8px;
    }
    .toolbar__row {
      display: flex; align-items: center; justify-content: space-between;
      gap: 16px;
    }
    .toolbar__leading { display: flex; align-items: center; gap: 24px; flex: 1; min-width: 0; }
    .toolbar__trailing { display: flex; align-items: center; gap: 8px; flex-shrink: 0; }

    /* Search field — Toolbar search bar
       Source: Figma ToolBar Container.SearchBar
       width: 400px, border-radius: 6px (unusual — not cornerradius.medium),
       border: 0.75px stroke.action.secondary-inverse.base = #d2d2d2
       padding: 8px (tight) */
    .toolbar__search {
      display: flex; align-items: center; gap: 8px;
      width: 100%; max-width: 400px; min-width: 200px;
      height: 36px; padding: 0 8px;
      border: 0.75px solid var(--semantic-color-light-mode-stroke-action-secondary-inverse-base, #d2d2d2);
      border-radius: 6px;
      background: var(--semantic-color-light-mode-fill-static-neutral-light, #ffffff);
      transition: border-color 120ms ease;
    }
    .toolbar__search:focus-within {
      border-color: var(--semantic-color-light-mode-stroke-action-primary-pressed, #6e8bd4);
      border-width: 1px;
    }
    .toolbar__search input {
      flex: 1; border: none; outline: none; background: transparent;
      font-family: 'Red Hat Text', sans-serif;
      font-size: 16px; font-weight: 400; line-height: 22px; letter-spacing: 0.1px;
      color: var(--semantic-color-light-mode-text-static-secondary-base, #484848);
    }
    .toolbar__search input::placeholder { color: var(--semantic-color-light-mode-text-static-secondary-subtle, #606060); }

    /* Filter chips
       Source: Figma FilterChip component
       min-height: 48px (touch target), border-radius: 4px (border-radius.xs)
       text: label.option.s.medium 12px/500/18px, text.action.secondary.base = #292724
       padding: 4px */
    .filter-chips { display: flex; align-items: center; gap: 4px; flex-wrap: wrap; }
    .filter-chip {
      display: inline-flex; align-items: center; justify-content: center;
      min-height: 48px; padding: 6px 2px;
      border: none; background: transparent; cursor: pointer; font-family: inherit;
    }
    .filter-chip__inner {
      display: flex; align-items: center; justify-content: center;
      padding: 4px 8px; border-radius: 4px;
      font-size: 12px; font-weight: 500; line-height: 18px; letter-spacing: 0.3px;
      color: var(--semantic-color-light-mode-text-action-secondary-base, #292724);
      transition: background 120ms ease, color 120ms ease;
      white-space: nowrap;
    }
    .filter-chip:hover .filter-chip__inner { background: rgba(0,0,0,0.05); }
    .filter-chip.active .filter-chip__inner {
      background: var(--semantic-color-light-mode-fill-action-tertiary-base, #eef2fb);
      color: var(--semantic-color-light-mode-text-action-primary-base, #3555a0);
      font-weight: 600;
    }

    /* Section heading
       Source: Figma SectionHeading component
       font: heading.section.xs.semibold 14px/600/22px, letter-spacing: 0.6px
       color: text.static.secondary.subtle = #606060 (subtle, uppercase label)
       padding-vertical: 6px */
    .section-heading {
      display: flex; align-items: center; justify-content: space-between;
      padding: 6px 0;
    }
    .section-heading__label {
      font-size: 14px; font-weight: 600; line-height: 22px; letter-spacing: 0.6px;
      text-transform: uppercase;
      color: var(--semantic-color-light-mode-text-static-secondary-subtle, #606060);
    }
    .section-heading__action {
      font-size: 12px; font-weight: 500; line-height: 18px;
      color: var(--semantic-color-light-mode-text-action-primary-base, #3555a0);
      cursor: pointer; border: none; background: transparent; font-family: inherit;
    }

    /* Content section */
    .content-section {
      display: flex; flex-direction: column;
      gap: 8px;
      padding-top: 4px; padding-bottom: 16px;
    }
    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
      gap: 8px;
    }

    /* Card
       Source: Figma Card component inside ScreenTemplate
       bg: fill.static.neutral.light = white
       border: 0.5px contextual.card.border-width.base.base, stroke.action.secondary-inverse.base = #d2d2d2
       border-radius: contextual.card.cornerradius = 8px
       padding: contextual.card.padding.medium.horizontal = 16px
       gap: contextual.card.gap = 12px */
    .card {
      background: var(--semantic-color-light-mode-fill-static-neutral-light, #ffffff);
      border: 0.5px solid var(--semantic-color-light-mode-stroke-action-secondary-inverse-base, #d2d2d2);
      border-radius: 8px;
      padding: 16px;
      display: flex; flex-direction: column; gap: 12px;
      transition: box-shadow 150ms ease, border-color 150ms ease;
    }
    .card:hover {
      box-shadow: 0 2px 8px rgba(0,0,0,0.07);
      border-color: var(--semantic-color-light-mode-stroke-action-primary-hover, #86a0dd);
    }
    .card__heading {
      display: flex; align-items: center; gap: 8px;
    }
    /* Card icon container: 32×32px, border-radius.s = 8px
       Color variants: accent_amethyst (purple), positive (green), etc */
    .card__icon {
      width: 32px; height: 32px; border-radius: 8px; flex-shrink: 0;
      display: flex; align-items: center; justify-content: center;
    }
    .card__icon--accent {
      background: var(--semantic-color-light-mode-fill-static-accent-amethyst-light, #f4f2fa);
      color: var(--semantic-color-light-mode-icon-static-accent-amethyst-base, #6a5acd);
    }
    .card__icon--positive {
      background: var(--semantic-color-light-mode-fill-static-positive-light, #f0faf1);
      color: var(--semantic-color-light-mode-icon-static-positive-base, #2e7d32);
    }
    .card__icon--info {
      background: var(--semantic-color-light-mode-fill-static-info-light, #eef2fb);
      color: var(--semantic-color-light-mode-fill-action-primary-base, #3555a0);
    }
    .card__icon--warning {
      background: var(--semantic-color-light-mode-fill-static-warning-light, #fff8e1);
      color: var(--semantic-color-light-mode-icon-static-warning-base, #e65100);
    }
    .card__title {
      flex: 1; min-width: 0;
      font-size: 16px; font-weight: 600; line-height: 22px; letter-spacing: 0.1px;
      color: var(--semantic-color-light-mode-text-static-primary-base, #202020);
      white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
    }
    /* Badge
       Source: Figma Badge component
       bg: fill.static.accent_amethyst.light = #f4f2fa
       text: label.badge.small.medium 11px/500/16px, text.static.accent-amethyst.contrast = #221e3f
       border-radius: 12px, padding: 2px horizontal */
    .card__badge {
      display: inline-flex; align-items: center; gap: 2px;
      padding: 1px 6px; border-radius: 12px;
      background: var(--semantic-color-light-mode-fill-static-accent-amethyst-light, #f4f2fa);
      font-size: 11px; font-weight: 500; line-height: 16px; letter-spacing: 0.3px;
      color: var(--semantic-color-light-mode-text-static-accent-amethyst-contrast, #221e3f);
    }
    .card__badge--positive {
      background: var(--semantic-color-light-mode-fill-static-positive-light, #f0faf1);
      color: #1b5e20;
    }
    .card__body {
      font-size: 14px; font-weight: 400; line-height: 20px; letter-spacing: 0.3px;
      color: var(--semantic-color-light-mode-text-static-secondary-subtle, #606060);
    }

    /* ── ACTION BUTTONS ─────────────────────────────────────────────────────── */
    .btn {
      display: inline-flex; align-items: center; gap: 6px;
      padding: 8px 14px; border-radius: 8px;
      font-family: 'Red Hat Text', sans-serif; font-size: 14px;
      font-weight: 500; line-height: 20px; letter-spacing: 0.3px;
      cursor: pointer; border: none; transition: background 120ms ease;
    }
    .btn--primary {
      background: var(--semantic-color-light-mode-fill-action-primary-base, #3555a0);
      color: #ffffff;
    }
    .btn--primary:hover { background: var(--semantic-color-light-mode-fill-action-primary-hover, #4b6ec3); }
    .btn--secondary {
      background: var(--semantic-color-light-mode-fill-static-neutral-light, #fff);
      color: var(--semantic-color-light-mode-text-action-secondary-base, #292724);
      border: 1px solid var(--semantic-color-light-mode-stroke-action-secondary-base, #292724);
    }
    .btn--secondary:hover { background: var(--semantic-color-light-mode-fill-action-secondaryinverse-hover, #f7f5f3); }
    .btn--naked {
      background: transparent;
      color: var(--semantic-color-light-mode-text-action-primary-base, #3555a0);
    }
    .btn--naked:hover { background: var(--semantic-color-light-mode-fill-action-tertiary-base, #eef2fb); }

    /* ── CHEVRON ANIMATION ──────────────────────────────────────────────────── */
    .chev {
      display: inline-flex; font-variation-settings: 'FILL' 0,'wght' 400,'GRAD' 0,'opsz' 20;
      transition: transform 180ms ease;
    }
    .chev.open { transform: rotate(180deg); }

    /* ── DROP IN ANIMATION ──────────────────────────────────────────────────── */
    @keyframes dropIn {
      from { opacity: 0; transform: translateY(-4px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* ── RESPONSIVE ─────────────────────────────────────────────────────────── */

    /* Tablet: 768–1023px
       - SideNav collapses to 72px rail
       - Module label hidden
       - Notification bells → more_vert */
    @media (max-width: 1023px) {
      .shell__sidenav { width: 72px !important; }
      .shell__main { margin-left: 72px !important; }
      .nav-item__label { opacity: 0 !important; width: 0 !important; }
      .nav-section-label { opacity: 0 !important; }
      .nav-header__logo { opacity: 0 !important; }
      .nav-item { justify-content: center; }
      .topnav__module-label { display: none !important; }
      .topnav__org-label { max-width: 80px !important; }
      .topnav__desktop-bells { display: none !important; }
      .topnav__tablet-more { display: flex !important; }
    }

    /* Mobile: <768px
       - SideNav hidden by default (revealed by hamburger)
       - TopNav shows hamburger
       - Module icon only (no label, no chevron)
       - Org label max 70px */
    @media (max-width: 767px) {
      .shell__main { margin-left: 0 !important; }
      .topnav__hamburger { display: flex !important; }
      .topnav__module-label { display: none !important; }
      .topnav__org-label { max-width: 70px !important; }
      .topnav__avatar { font-size: 11px !important; }
      .screen { padding: 8px 16px 32px; }
      .card-grid { grid-template-columns: 1fr; }
      .toolbar__leading { flex-wrap: wrap; }
      .toolbar__search { max-width: 100%; }
    }

    .topnav__tablet-more { display: none; }
    .topnav__desktop-bells { display: flex; align-items: center; }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
      }
    }

    /* ── BREAKPOINT INDICATOR ───────────────────────────────────────────────── */
    .bp-indicator {
      position: fixed; bottom: 12px; left: 50%; transform: translateX(-50%);
      z-index: 999;
      display: flex; align-items: center; gap: 6px;
      background: rgba(45,72,137,0.9); color: #fbfbfb;
      padding: 4px 12px; border-radius: 9999px;
      font-size: 11px; font-weight: 500; letter-spacing: 0.05em;
      pointer-events: none;
    }
    .bp-indicator .bp-desktop { display: inline; }
    .bp-indicator .bp-tablet  { display: none; }
    .bp-indicator .bp-mobile  { display: none; }
    @media (max-width: 1023px) and (min-width: 768px) {
      .bp-indicator .bp-desktop { display: none; }
      .bp-indicator .bp-tablet  { display: inline; }
    }
    @media (max-width: 767px) {
      .bp-indicator .bp-desktop { display: none; }
      .bp-indicator .bp-mobile  { display: inline; }
    }
  </style>
</head>
<body>
  <div id="root"></div>

  <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    const { useState, useEffect, useRef, useCallback } = React;

    // ════════════════════════════════════════════════════════════════════════════
    // SHELL CONFIGURATION
    // ════════════════════════════════════════════════════════════════════════════
    //
    // This is the single place an agent needs to edit to customize the shell.
    // Change SHELL_CONFIG to produce a pixel-accurate implementation of the
    // Pathway navigation shell for any module, org, or page.
    //
    // USAGE EXAMPLE:
    //   To implement "Amplify Giving" module with NorthPoint Church org and
    //   three SideNav items (Home, Teams, Projects):
    //
    //   const SHELL_CONFIG = {
    //     module: { id: "giving", label: "Amplify Giving", icon: "volunteer_activism" },
    //     org:    { name: "NorthPoint Church", campus: "" },
    //     user:   { name: "Jo Lopez", initials: "JL", email: "jo@northpoint.org" },
    //     navItems: [
    //       { id: "home",    label: "Home",    icon: "home",    active: true },
    //       { id: "teams",   label: "Teams",   icon: "group" },
    //       { id: "projects",label: "Projects",icon: "folder" },
    //     ],
    //     page: {
    //       title: "Teams",
    //       subtitle: "Manage your ministry teams and volunteers",
    //       tabs: ["All Teams", "Active", "Archived"],
    //     },
    //   };

    const SHELL_CONFIG = {
      module: {
        id:    "home",
        label: "Amplify Home",
        icon:  "home",               // Material Symbols Rounded ligature name
        isCustom: true,              // true = use HOME_ICON_PATH SVG instead of font
      },
      org: {
        name:   "Grace Community Church",
        campus: "Knoxville",
        // logoUrl: "" — omit or leave empty for org-name-only trigger (DS default)
      },
      user: {
        name:     "Jo Lopez",
        initials: "JL",
        email:    "jo@gracecommunity.org",
      },
      // SideNav items — ordered exactly as they appear in the nav
      // Each item: { id, label, icon, active?, sectionLabel? }
      //   id           — unique string, used for active state tracking
      //   label        — visible label text
      //   icon         — Material Symbols Rounded ligature name (check CLAUDE.md §12)
      //   active       — true for the current page (only one item active at a time)
      //   sectionLabel — optional section heading shown ABOVE this item
      navItems: [
        { id: "home",      label: "Home",          icon: "home",              active: true },
        { id: "people",    label: "People",         icon: "group" },
        { id: "giving",    label: "Giving",         icon: "volunteer_activism",sectionLabel: "Ministry" },
        { id: "events",    label: "Events",         icon: "event" },
        { id: "comms",     label: "Communications", icon: "mail" },
        { id: "settings",  label: "Settings",       icon: "settings",          sectionLabel: "Admin" },
      ],
      // Page content (ScreenTemplate)
      page: {
        title:    "Dashboard",
        subtitle: "Welcome back, Jo. Here's what's happening today.",
        tabs:     ["Overview", "Activity", "Reports"],
        activeTab: 0,
        // ToolBar configuration
        toolbar: {
          searchPlaceholder: "Search...",
          filters: ["All", "This week", "This month", "This year"],
          activeFilter: 0,
        },
      },
    };

    // ════════════════════════════════════════════════════════════════════════════
    // HOME MODULE ICON (custom Figma SVG — Amplify Home branded asset)
    // Source: top-nav.jsx HOME_ICON_PATH — do NOT replace with Material Symbol
    // ════════════════════════════════════════════════════════════════════════════
    const HOME_ICON_PATH =
      "M16.5811 38.6244V32.8265C16.5811 31.3486 17.7937 30.1359 19.2716 30.1359H24.7095C25.4295 30.1359 26.1116 30.4202 26.6232 30.9128C27.1347 31.4054 27.4189 32.0875 27.4189 32.8075V38.6054C27.4189 39.2307 27.6653 39.8181 28.1011 40.2538C28.5368 40.6896 29.1242 40.9359 29.7495 40.9359H33.4632C35.1874 40.9359 36.8547 40.2538 38.0863 39.0412C39.3179 37.8286 40 36.1802 40 34.4559V17.9717C40 16.5696 39.3747 15.2623 38.2947 14.3717L25.6568 4.34858C23.4589 2.58647 20.3137 2.64331 18.1726 4.48121L5.83789 14.3528C4.72 15.2244 4.03789 16.5317 4 17.9528V34.437C4 38.037 6.93684 40.9549 10.5368 40.9359H14.1747C15.4632 40.9359 16.5053 39.9128 16.5242 38.6244H16.5811Z";

    function HomeModuleIcon({ size = 22 }) {
      return (
        <svg viewBox="0 0 44 44" width={size} height={size}
          fill="none" xmlns="http://www.w3.org/2000/svg"
          aria-hidden="true" style={{ display:"block", flexShrink:0 }}>
          <path d={HOME_ICON_PATH} fill="white" />
        </svg>
      );
    }

    // ════════════════════════════════════════════════════════════════════════════
    // ICON HELPER
    // All standard UI icons use Material Symbols Rounded (FILL=0, wght 400, opsz 20)
    // Exception: HomeModuleIcon (custom Figma asset) — use HomeModuleIcon component
    // ════════════════════════════════════════════════════════════════════════════
    function Icon({ name, size = 20, color, fill = 0, style: extra = {} }) {
      return (
        <span className="material-symbols-rounded" aria-hidden="true" style={{
          fontSize: size, color, lineHeight: 1, display: "block",
          userSelect: "none", flexShrink: 0,
          fontVariationSettings: `'FILL' ${fill},'wght' 400,'GRAD' 0,'opsz' 20`,
          ...extra,
        }}>{name}</span>
      );
    }

    // ════════════════════════════════════════════════════════════════════════════
    // SAMPLE CONTENT
    // In production, replace with real data. Structure must be preserved.
    // ════════════════════════════════════════════════════════════════════════════
    const SAMPLE_CARDS = [
      {
        id: 1, title: "Sunday Service Attendance",
        icon: "group", iconVariant: "accent",
        badge: "This week", body: "1,247 attendees across all campuses. Up 8% from last Sunday.",
      },
      {
        id: 2, title: "Upcoming Events",
        icon: "event", iconVariant: "positive",
        badge: "3 this week", body: "Youth retreat, worship night, and community outreach planned.",
      },
      {
        id: 3, title: "Giving Summary",
        icon: "volunteer_activism", iconVariant: "info",
        badge: "This month", body: "$42,800 received. On track for monthly goal of $50,000.",
      },
      {
        id: 4, title: "New Members",
        icon: "person_add", iconVariant: "accent",
        badge: "This month", body: "14 new member applications received, 9 approved.",
      },
      {
        id: 5, title: "Communications Sent",
        icon: "mail", iconVariant: "info",
        badge: "This week", body: "3 bulk emails, 1 SMS blast. 73% open rate on last email.",
      },
      {
        id: 6, title: "Volunteer Hours",
        icon: "schedule", iconVariant: "positive",
        badge: "This week", body: "246 volunteer hours logged across 12 ministry teams.",
      },
    ];

    const SECTION_DATA = [
      {
        id: "overview", heading: "Overview", action: "View all",
        cards: SAMPLE_CARDS.slice(0, 3),
      },
      {
        id: "activity", heading: "Recent Activity",
        cards: SAMPLE_CARDS.slice(3, 6),
      },
    ];

    // ════════════════════════════════════════════════════════════════════════════
    // ORG ABBREVIATION UTILITIES (for mobile label)
    // Source: top-nav.jsx abbreviateOrg + abbreviateCampus
    // ════════════════════════════════════════════════════════════════════════════
    const SKIP = new Set(["the","a","an","of","in","at","for","and","or","but"]);
    function abbreviateOrg(name) {
      const sig = name.replace(/-/g," ").split(/\s+/).filter(w => !SKIP.has(w.toLowerCase()));
      if (sig.length >= 3) return (sig[0][0]+sig[1][0]+sig[2][0]).toUpperCase();
      if (sig.length === 2) return (sig[0][0]+sig[1][0]+sig[1][0]).toUpperCase();
      return (sig[0]||"?").slice(0,3).toUpperCase();
    }
    const PLACE_SUFFIXES = ["ville","field","burg","town","ford","wood","land","dale"];
    function abbreviateCampus(campus) {
      if (!campus) return "";
      const sig = campus.replace(/-/g," ").split(/\s+/).filter(w => !SKIP.has(w.toLowerCase()));
      if (!sig.length) return campus.slice(0,2).toUpperCase();
      if (sig.length >= 2) return (sig[0][0]+sig[1][0]).toUpperCase();
      const w = sig[0], wl = w.toLowerCase();
      for (const sfx of PLACE_SUFFIXES) {
        if (wl.endsWith(sfx) && w.length > sfx.length+1) return (w[0]+sfx[0]).toUpperCase();
      }
      return (w[0]+w[w.length-1]).toUpperCase();
    }

    // ════════════════════════════════════════════════════════════════════════════
    // TOP NAV
    // Breakpoints:
    //   Desktop ≥1024px: full labels, 2× bells, no hamburger
    //   Tablet 768–1023px: module icon only, more_vert, no hamburger
    //   Mobile <768px: hamburger, abbreviated org, more_vert, 11px initials
    // ════════════════════════════════════════════════════════════════════════════
    function TopNav({ config, onSideNavToggle, isMobile }) {
      const [orgOpen, setOrgOpen] = useState(false);
      const orgRef = useRef(null);

      const orgLabel = isMobile
        ? `${abbreviateOrg(config.org.name)}${config.org.campus ? " | "+abbreviateCampus(config.org.campus) : ""}`
        : `${config.org.name}${config.org.campus ? " | "+config.org.campus : ""}`;

      useEffect(() => {
        const h = e => {
          if (orgRef.current && !orgRef.current.contains(e.target)) setOrgOpen(false);
          if (e.key === "Escape") setOrgOpen(false);
        };
        document.addEventListener("mousedown", h);
        document.addEventListener("keydown", h);
        return () => { document.removeEventListener("mousedown", h); document.removeEventListener("keydown", h); };
      }, []);

      return (
        <header className="topnav" role="banner">
          <div className="topnav__start">
            {/* Hamburger — mobile only (opens/closes SideNav) */}
            <button className="topnav__hamburger" aria-label="Toggle navigation menu"
              onClick={onSideNavToggle}>
              <Icon name="menu" size={20} color="rgba(251,251,251,0.9)" />
            </button>

            {/* ModuleSwitcher */}
            <button className="topnav__module" aria-label={`Switch module — ${config.module.label}`}
              aria-haspopup="menu">
              <span className="topnav__module-icon">
                {config.module.isCustom
                  ? <HomeModuleIcon size={22} />
                  : <Icon name={config.module.icon} size={20} color="rgba(251,251,251,0.9)" />
                }
              </span>
              <span className={`topnav__module-label${isMobile ? " hidden" : ""}`}>
                {config.module.label}
              </span>
              <Icon name="expand_more" size={16} color="rgba(251,251,251,0.7)" />
            </button>

            {/* OrgSwitcher */}
            <div className="topnav__org" ref={orgRef}>
              <button className={`topnav__org-btn${orgOpen ? " open" : ""}`}
                aria-haspopup="true" aria-expanded={orgOpen}
                aria-label={`Switch organisation — ${config.org.name}`}
                onClick={() => setOrgOpen(p => !p)}>
                <span className={`topnav__org-label${isMobile ? " mobile" : ""}`}>{orgLabel}</span>
                <span className={`chev material-symbols-rounded${orgOpen ? " open" : ""}`}
                  aria-hidden="true"
                  style={{ fontSize:16, fontVariationSettings:"'FILL' 0,'wght' 400,'GRAD' 0,'opsz' 20",
                    color:"rgba(251,251,251,0.7)" }}>
                  expand_more
                </span>
              </button>
              {orgOpen && (
                <div className="topnav__org-panel" role="dialog" aria-label="Switch organisation">
                  <div className="topnav__org-panel-heading">Organisations</div>
                  {[
                    { name: config.org.name, campus: config.org.campus, initials: abbreviateOrg(config.org.name), bg: "#2d4889", active: true },
                    { name: "Fellowship Community Church", campus: "Main Campus", initials: "FCC", bg: "#22386b", active: false },
                  ].map((org, i) => (
                    <div key={i} className={`topnav__org-item${org.active ? " active" : ""}`}
                      role="option" aria-selected={org.active} tabIndex={0}
                      onClick={() => setOrgOpen(false)}
                      onKeyDown={e => e.key==="Enter" && setOrgOpen(false)}>
                      <div className="topnav__org-item-avatar" style={{ background: org.bg }}>
                        {org.initials}
                      </div>
                      <div>
                        <div className="topnav__org-item-name">{org.name}</div>
                        {org.campus && <div className="topnav__org-item-campus">{org.campus}</div>}
                      </div>
                    </div>
                  ))}
                </div>
              )}
            </div>
          </div>

          <div className="topnav__end">
            {/* Search */}
            <button className="topnav__search-btn" aria-label="Open search">
              <Icon name="search" size={16} color="rgba(251,251,251,0.9)" />
            </button>

            {/* Desktop: 2 bell icons */}
            <div className="topnav__desktop-bells">
              <button className="topnav__action-btn" aria-label="Notifications">
                <Icon name="notifications" size={20} color="rgba(251,251,251,0.9)" />
              </button>
              <button className="topnav__action-btn" aria-label="Alerts">
                <Icon name="notifications" size={20} color="rgba(251,251,251,0.9)" />
              </button>
            </div>

            {/* Tablet/Mobile: more_vert */}
            <button className="topnav__action-btn topnav__tablet-more" aria-label="More actions">
              <Icon name="more_vert" size={20} color="rgba(251,251,251,0.9)" />
            </button>

            {/* Profile */}
            <button className="topnav__profile-btn"
              aria-label={`Account — ${config.user.name}`}>
              <div className={`topnav__avatar${isMobile ? " mobile" : ""}`}>
                {config.user.initials}
              </div>
            </button>
          </div>
        </header>
      );
    }

    // ════════════════════════════════════════════════════════════════════════════
    // SIDE NAV
    // States: expanded (240px), collapsed (72px rail), hidden (mobile overlay)
    // Collapse button: bottom of SideNav — toggles expanded/collapsed on desktop
    // ════════════════════════════════════════════════════════════════════════════
    function SideNav({ config, expanded, onToggleExpand }) {
      const [activeId, setActiveId] = useState(
        (config.navItems.find(i => i.active) || config.navItems[0]).id
      );

      return (
        <nav className={`shell__sidenav ${expanded ? "expanded" : "collapsed"}`}
          aria-label="Module navigation">

          {/* Nav header */}
          <div className="nav-header">
            <span className={`nav-header__logo${!expanded ? " hidden" : ""}`}>
              {config.module.label}
            </span>
            <button className="nav-collapse-btn"
              aria-label={expanded ? "Collapse navigation" : "Expand navigation"}
              title={expanded ? "Collapse" : "Expand"}
              onClick={onToggleExpand}>
              <Icon name={expanded ? "left_panel_close" : "right_panel_close"}
                size={18} color="var(--semantic-color-light-mode-icon-contextual-navitem-base, #606060)" />
            </button>
          </div>

          {/* Nav menu */}
          <div className="nav-menu" role="tree" aria-label={`${config.module.label} navigation`}>
            {config.navItems.map((item, idx) => (
              <React.Fragment key={item.id}>
                {item.sectionLabel && (
                  <div className={`nav-section-label${!expanded ? " hidden" : ""}`}
                    aria-hidden={!expanded}>
                    {item.sectionLabel}
                  </div>
                )}
                <button
                  className={`nav-item${activeId === item.id ? " active" : ""}`}
                  role="treeitem"
                  aria-current={activeId === item.id ? "page" : undefined}
                  aria-label={!expanded ? item.label : undefined}
                  onClick={() => setActiveId(item.id)}
                >
                  {activeId === item.id && <span className="nav-item__indicator" aria-hidden="true" />}
                  <span className="nav-item__icon">
                    <Icon name={item.icon} size={20}
                      color={activeId === item.id
                        ? "var(--semantic-color-light-mode-icon-contextual-navitem-active, #3555a0)"
                        : "var(--semantic-color-light-mode-icon-contextual-navitem-base, #606060)"
                      }
                      fill={activeId === item.id ? 1 : 0}
                    />
                  </span>
                  <span className={`nav-item__label${!expanded ? " hidden" : ""}`}>
                    {item.label}
                  </span>
                  {/* Tooltip on collapsed rail */}
                  {!expanded && <span className="nav-tooltip">{item.label}</span>}
                </button>
              </React.Fragment>
            ))}
          </div>

          {/* Nav footer — collapse button (desktop) */}
          <div className="nav-footer">
            <button
              className={`nav-item${!expanded ? "" : ""}`}
              style={{ width: "100%", margin: 0 }}
              onClick={onToggleExpand}
              aria-label={expanded ? "Collapse sidebar" : "Expand sidebar"}
            >
              <span className="nav-item__icon">
                <Icon name={expanded ? "chevron_left" : "chevron_right"}
                  size={20} color="var(--semantic-color-light-mode-icon-contextual-navitem-base, #606060)" />
              </span>
              <span className={`nav-item__label${!expanded ? " hidden" : ""}`}>
                Collapse
              </span>
            </button>
          </div>
        </nav>
      );
    }

    // ════════════════════════════════════════════════════════════════════════════
    // SCREEN TEMPLATE (Main content area)
    // Source: Figma ScreenTemplate node 40006538:43236
    // Implements: Tabs, PageHeading, ToolBar with FilterChips, SectionHeadings, Cards
    // Note: Sub-components (FilterChip, Card, PageHeading, ToolBar) are inline here.
    //   They will become standalone DS components in future pipeline runs.
    // ════════════════════════════════════════════════════════════════════════════
    function ScreenTemplate({ page }) {
      const [activeTab, setActiveTab] = useState(page.activeTab || 0);
      const [activeFilter, setActiveFilter] = useState(page.toolbar.activeFilter || 0);
      const [searchValue, setSearchValue] = useState("");

      return (
        <div className="screen">
          {/* Tabs — Slot.PageNavigation
              Source: Figma Tabs component inside ScreenTemplate
              bg: surface.canvas.light (#fafafa), border-bottom on container */}
          {page.tabs && page.tabs.length > 0 && (
            <div className="page-tabs" role="tablist" aria-label="Page sections">
              {page.tabs.map((tab, i) => (
                <button key={i} role="tab"
                  className={`page-tab${activeTab === i ? " active" : ""}`}
                  aria-selected={activeTab === i}
                  onClick={() => setActiveTab(i)}>
                  {tab}
                </button>
              ))}
            </div>
          )}

          {/* PageHeading
              Source: Figma PageHeading component — node 40006538:42661
              heading.page.base.semibold: 24px/600/30px, tracking 0.1px
              text.static.primary.base = #202020
              subheading: text.body.base.regular 16px/400/22px
              text.static.secondary.base = #484848 */}
          <div className="page-heading">
            <div className="page-heading__row">
              <div>
                <h1 className="page-heading__title">{page.title}</h1>
                {page.subtitle && (
                  <p className="page-heading__subtitle">{page.subtitle}</p>
                )}
              </div>
              <div className="page-heading__actions">
                <button className="btn btn--secondary">
                  <Icon name="tune" size={16} />
                  Configure
                </button>
                <button className="btn btn--primary">
                  <Icon name="add" size={16} />
                  New item
                </button>
              </div>
            </div>
          </div>

          {/* ToolBar
              Source: Figma ToolBar component — node 40006537:42570
              padding-vertical: 8px, gap-vertical: 8px
              Leading: search (400px, border-radius 6px) + FilterChips
              Trailing: action buttons */}
          <div className="toolbar">
            <div className="toolbar__row">
              <div className="toolbar__leading">
                {/* Search bar
                    border-radius: 6px (unusual — confirmed from Figma, not 8px)
                    border: 0.75px stroke.action.secondary-inverse.base = #d2d2d2 */}
                <div className="toolbar__search">
                  <Icon name="search" size={16}
                    color="var(--semantic-color-light-mode-icon-action-secondary-inverse-base, #6b6b6b)" />
                  <input
                    type="text"
                    placeholder={page.toolbar.searchPlaceholder}
                    value={searchValue}
                    onChange={e => setSearchValue(e.target.value)}
                    aria-label="Search"
                  />
                  {searchValue && (
                    <button onClick={() => setSearchValue("")}
                      aria-label="Clear search"
                      style={{ background:"none",border:"none",cursor:"pointer",padding:0,display:"flex" }}>
                      <Icon name="cancel" size={16} fill={1}
                        color="var(--semantic-color-light-mode-icon-action-secondary-inverse-base, #6b6b6b)" />
                    </button>
                  )}
                </div>

                {/* FilterChips
                    Source: Figma FilterChip — min-height 48px, border-radius.xs 4px
                    text: label.option.s.medium 12px/500/18px
                    active: fill.action.tertiary.base = #eef2fb, text.action.primary = #3555a0 */}
                <div className="filter-chips" role="group" aria-label="Filter options">
                  {page.toolbar.filters.map((filter, i) => (
                    <button key={i}
                      className={`filter-chip${activeFilter === i ? " active" : ""}`}
                      aria-pressed={activeFilter === i}
                      onClick={() => setActiveFilter(i)}>
                      <span className="filter-chip__inner">{filter}</span>
                    </button>
                  ))}
                </div>
              </div>

              <div className="toolbar__trailing">
                <button className="btn btn--naked">
                  <Icon name="sort" size={16} />
                  Sort
                </button>
                <button className="btn btn--naked">
                  <Icon name="view_module" size={16} />
                </button>
              </div>
            </div>
          </div>

          {/* Content sections with cards
              Source: Figma Section + SectionHeading + Card components
              Section padding: top 4px, bottom 16px, gap 8px */}
          {SECTION_DATA.map((section) => (
            <div key={section.id} className="content-section">
              {/* SectionHeading
                  heading.section.xs.semibold: 14px/600/22px, uppercase, tracking 0.6px
                  text.static.secondary.subtle = #606060
                  padding-vertical: 6px */}
              <div className="section-heading">
                <span className="section-heading__label">{section.heading}</span>
                {section.action && (
                  <button className="section-heading__action btn btn--naked"
                    style={{ padding: "2px 6px" }}>
                    {section.action}
                    <Icon name="chevron_right" size={14} />
                  </button>
                )}
              </div>

              {/* Card grid
                  grid-template-columns: auto-fill with 280px minimum */}
              <div className="card-grid">
                {section.cards.map((card) => (
                  <div key={card.id} className="card">
                    {/* Card heading: icon (32×32) + title */}
                    <div className="card__heading">
                      <div className={`card__icon card__icon--${card.iconVariant}`}>
                        <Icon name={card.icon} size={16}
                          fill={card.iconVariant === "accent" ? 0 : 1} />
                      </div>
                      <span className="card__title">{card.title}</span>
                    </div>
                    {/* Badge */}
                    <div>
                      <span className={`card__badge${card.iconVariant === "positive" ? " card__badge--positive" : ""}`}>
                        {card.badge}
                      </span>
                    </div>
                    {/* Card body */}
                    <p className="card__body">{card.body}</p>
                  </div>
                ))}
              </div>
            </div>
          ))}
        </div>
      );
    }

    // ════════════════════════════════════════════════════════════════════════════
    // NAV SHELL — ROOT COMPONENT
    // Composes TopNav + SideNav + ScreenTemplate
    // Layout logic:
    //   Desktop (≥1024px): SideNav expanded or collapsed rail, content shifts
    //   Tablet (768–1023px): SideNav always collapsed rail (72px), content shifts
    //   Mobile (<768px): SideNav hidden by default, overlay on hamburger tap
    // ════════════════════════════════════════════════════════════════════════════
    function NavShell() {
      const [isMobile, setIsMobile]     = useState(window.innerWidth < 768);
      const [isTablet, setIsTablet]     = useState(window.innerWidth >= 768 && window.innerWidth < 1024);
      const [sideNavOpen, setSideNavOpen] = useState(false);    // mobile overlay state
      const [sideNavExpanded, setSideNavExpanded] = useState(true); // desktop expand/collapse

      useEffect(() => {
        const update = () => {
          const w = window.innerWidth;
          setIsMobile(w < 768);
          setIsTablet(w >= 768 && w < 1024);
        };
        window.addEventListener("resize", update);
        return () => window.removeEventListener("resize", update);
      }, []);

      // Close mobile overlay on Escape
      useEffect(() => {
        const h = e => { if (e.key === "Escape") setSideNavOpen(false); };
        document.addEventListener("keydown", h);
        return () => document.removeEventListener("keydown", h);
      }, []);

      const handleSideNavToggle = () => {
        if (isMobile) {
          setSideNavOpen(p => !p);
        } else {
          setSideNavExpanded(p => !p);
        }
      };

      // Determine shell layout class
      const sideNavClass = isMobile
        ? (sideNavOpen ? "expanded" : "hidden")
        : isTablet
        ? "collapsed"
        : (sideNavExpanded ? "expanded" : "collapsed");

      const mainClass = isMobile
        ? "full"
        : isTablet
        ? "collapsed"
        : (sideNavExpanded ? "expanded" : "collapsed");

      return (
        <>
          <TopNav
            config={SHELL_CONFIG}
            onSideNavToggle={handleSideNavToggle}
            isMobile={isMobile}
          />
          <div className="shell">
            {/* Mobile overlay — tapping closes SideNav */}
            {isMobile && sideNavOpen && (
              <div className="shell__overlay visible"
                onClick={() => setSideNavOpen(false)}
                aria-hidden="true" />
            )}

            <SideNav
              config={SHELL_CONFIG}
              expanded={isMobile ? sideNavOpen : (isTablet ? false : sideNavExpanded)}
              onToggleExpand={handleSideNavToggle}
            />

            <main className={`shell__main ${mainClass}`} id="main-content">
              <ScreenTemplate page={SHELL_CONFIG.page} />
            </main>
          </div>

          {/* Breakpoint indicator */}
          <div className="bp-indicator" aria-hidden="true">
            <span className="material-symbols-rounded" style={{fontSize:12,fontVariationSettings:"'FILL' 0,'wght' 400,'GRAD' 0,'opsz' 20"}}>responsive_layout</span>
            <span className="bp-desktop">Desktop (≥1024px) — SideNav push</span>
            <span className="bp-tablet">Tablet (768–1023px) — SideNav rail</span>
            <span className="bp-mobile">Mobile (&lt;768px) — SideNav overlay</span>
          </div>
        </>
      );
    }

    ReactDOM.createRoot(document.getElementById("root")).render(<NavShell />);
  </script>
</body>
</html>
```

---

## 6. Design System Updates Required

### Nested instance placeholders (ScreenTemplate sub-components)

The following components are implemented inline in nav-shell.html. Each needs its own DS component spec + pipeline run:

| Component | Figma node | Notes |
|---|---|---|
| PageHeading | Inside 40006538:43236 | Title + subtitle + action slots |
| ToolBar | 40006537:42570 | Search + FilterChips + actions |
| FilterChip | 40006526:36972 | Active/resting states |
| Tabs | Inside 40006718:5996 | Page-level tab navigation |
| Card | Inside 40006538:42775 | Icon + title + badge + body |
| Badge | Inside card | label.badge.small.medium |
| SectionHeading | 40006538:43364 | Uppercase section label |

### Proposed contextual tokens (not yet in Figma variables)

| Token name | Value | Rationale |
|---|---|---|
| contextual.page.padding.horizontal | 36px | Page content horizontal padding |
| contextual.page.padding.top | 12px | Page content top padding |
| contextual.page.padding.bottom | 56px | Page content bottom padding |
| contextual.toolbar.search.border-radius | 6px | ToolBar search bar radius (confirmed from Figma) |
| contextual.card.border-width | 0.5px | Card border width |

