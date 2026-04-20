# Lane Model

A journey map has two kinds of lanes: locked (always present, structural) and per-project (chosen by the user at the start of each run).

## Locked lanes (7, in order)

These define what makes the output a journey map rather than a generic table. They are always in the template and always appear in this order.

1. **Steps**. Header row. Step number plus short step title. Height 175.
2. **User Actions**. What the user does at this step, from their perspective. 1 to 3 short lines. Height 205. Removable if the user opts out at the start.
3. **Screens**. The product UI at this step. A screenshot inside a rectangle slot. Height 307. Removable if the user opts out, or per-flow if the flow is non-UI (email-only, admin-only, support-initiated).
4. **Storyboard**. Illustration showing the persona's emotional or situational state at this step. Height 262. Removable if the user opts out.
5. **Emotions**. 5-line grid with an emoji marker. The emotion curve is the heart of a journey map. Keep it.
6. **Pain Points**. Friction, confusion, drop-off risks at this step. Pink background (`#F7C2AA`). Height 175.
7. **Opportunities**. What could be improved. Green background (`#C9F8D1`). Height 175.

Rule: Steps, Emotions, Pain Points, and Opportunities are structurally locked. The skill never removes them, even at user request. If the user insists on removing one of those four, push back: without an emotion curve and pain and opportunity rows, it is not a journey map. Offer a simpler deliverable (plain user flow diagram via `generate_diagram`) instead.

The other three locked lanes (User Actions, Screens, Storyboard) can be skipped for a specific run. They default to on.

## Per-project lanes

These live between Storyboard and Emotions in the rendered output. Ask the user at the start of each run which to include, and in what order. If the user does not specify, default to including Support Processes, Backend Interactions, and Communications.

### Canonical per-project lanes

- **Support Processes**. What happens on the support or internal team side during this step. Sales calls, onboarding sessions, manual reviews, approval queues, CS follow-up.
- **Backend Interactions**. System processes triggered by this step. Credentialing, data syncs, async jobs, webhooks, job queues.
- **Communications**. Emails, SMS, push notifications, phone calls sent or received during or after this step. Confirmation emails, verification codes, sales follow-ups, SMS reminders.
- **Third Parties / Vendors**. External systems involved. The name of this lane is configurable per project. Common options: "Third Parties", "Vendors", "Integrations", or vendor-specific names like "Stripe", "Salesforce", "CHMS Host". Multiple lanes are allowed if different vendors have meaningfully different involvement across the flow.

### Custom lanes

Users can add any custom lane by name. Common patterns:

- Legal / Compliance. Approval gates, audit trails.
- Content Ops. Content creation or moderation happening alongside.
- Data. Analytics events fired, data captured.
- Accessibility. A11y considerations per step.

## Styling for per-project lanes

Per-project lanes use the same style as the "body" locked lanes.

- Label background: `#F7E2AA` (label cream).
- Cell background: `#FEF2D2` (body cream).
- Height: 175 by default. Increase if the user knows the content will run long.
- Icon (label column): pick a sensible emoji. Suggestions:
  - Support Processes: 🎧 or 🛟
  - Backend Interactions: ⚙️ or 🔄
  - Communications: 📧 or 📬
  - Third Parties / Vendors: 🔌 or 🤝
  - Legal / Compliance: ⚖️
  - Content Ops: 📝
  - Data: 📊
  - Accessibility: ♿

If the user provides an icon, use theirs. Otherwise pick from above.

## Final row order in the rendered map

Top to bottom:

1. Steps (locked).
2. User Actions (locked, optional).
3. Screens (locked, optional).
4. Storyboard (locked, optional).
5. Per-project lanes, in the order the user listed them.
6. Emotions (locked).
7. Pain Points (locked).
8. Opportunities (locked).

This puts the user's observable experience first, then the invisible stuff behind it, then the evaluative lanes. Do not deviate. Designers expect this order.
