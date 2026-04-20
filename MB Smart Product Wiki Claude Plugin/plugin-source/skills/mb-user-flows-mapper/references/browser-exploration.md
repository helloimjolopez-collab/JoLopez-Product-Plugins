# Browser Exploration

How to use the Claude in Chrome MCP to do the Phase 1 breadth crawl and the Phase 2 per-flow walkthrough.

## Setup

Before exploring:

1. Get the product URL and credentials from the user.
2. Open the URL via `navigate`. If a login screen appears, fill the form with `form_input` and submit.
3. If the product uses SSO, 2FA, or has a weird login path, ask the user to log in manually in the shared Chrome session. Wait for them to confirm.
4. Confirm you are logged in by reading the page and looking for user-specific content (user name, dashboard, or similar).

## Phase 1: breadth crawl

Goal: enumerate the major destinations, not go deep. Aim for about 5 to 10 minutes of exploration.

1. Read the landing page (`read_page` or `get_page_text`) and identify:
   - Main navigation items (top nav, side nav).
   - Primary CTAs on the dashboard or home.
   - User profile or settings entry point.
   - Admin surfaces if visible.
2. Visit each major destination once. Read the page. Take note of what's there. Do not drill into every sub-page.
3. Capture a short mental map: "This product has a Dashboard, a Contacts section, a Settings area, and an Admin panel. The main actions on the dashboard are X, Y, Z."
4. Draft a list of 4 to 8 candidate core flows based on what you saw. Examples:
   - Sign up
   - Onboard a new user
   - Create a [primary object of the product]
   - Complete a transaction
   - Invite a team member
   - Configure integrations
   - Export data
   - Resolve a support issue (if visible)

5. Confirm the list with the user via AskUserQuestion. Prompt for flows that exist outside the app (marketing-site signup, email-driven verification, support-initiated processes).

## Phase 2: per-flow walkthrough

Goal: capture every meaningful step with a screenshot and draft User Actions text.

1. Navigate to the starting state of the flow.
2. At each meaningful state, take a screenshot via `computer` or `get_screenshot`. Name the file `<flow-slug>_step<N>.png` and save locally.
3. Read the page and draft User Actions text: what the user clicks, types, or sees. 1 to 3 short lines per step.
4. Advance to the next state by clicking, typing, or submitting forms. If a step requires data you do not have (a real credit card, real identity info), stop and ask the user how to proceed (a sandbox value, skip the step, or take the screenshot from the user).
5. Note the total step count at the end.

## What counts as a "meaningful" state change

- New screen or route.
- Modal opens.
- Confirmation or success message appears.
- User is redirected to a third party site.
- Tab or panel change that materially alters what the user can do.

Skip:

- Hovers.
- Intermediate loading spinners.
- Form field focus changes.
- Auto-complete dropdowns.

## Traps to watch for

- **Protected routes that log you out.** If you get bounced to login mid-walkthrough, the session expired. Log in again.
- **Destructive actions.** Do not actually delete, cancel, or submit something irreversible in a production environment. Ask the user before triggering anything destructive. Prefer a testing environment.
- **Missing data.** If a flow requires records that do not exist (an order to refund, a user to edit), ask the user to pre-seed the account or point you to existing test data.
- **Third party flows.** When the flow redirects to a vendor (Stripe, an identity provider, a payments page), note it but do not try to complete it with real data. Screenshot the redirect and ask the user to describe what happens on the vendor side.

## Saving screenshots for Phase 5

Save all screenshots in a per-flow directory so Phase 5 can batch upload them:

```
<project-workspace>/flows/<flow-slug>/
  step1.png
  step2.png
  ...
  steps.json     # step count, titles, user-action drafts, screenshot paths
```

`steps.json` is a lightweight manifest the skill reads in Phase 5 when rendering to Figma.
