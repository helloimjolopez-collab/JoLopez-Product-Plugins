# Freshness Check: Detailed Audit Checklist

Use this checklist when running a full freshness audit on a product knowledgebase (the Knowledge Base Overview and the Knowledgebase). Not every item applies to every product. Skip what is irrelevant and go deeper on what matters.

Writing style: no em dashes. Use periods, commas, colons, or parentheses. Hyphens between single words are fine.

## Pre-Audit: Determine Scope

Understand the user's intent first:

| Intent | Scope | Depth |
|--------|-------|-------|
| General update check | Everything, even coverage | Medium, flag obvious drift |
| Task-targeted check | Sections relevant to the task | Deep on data that could affect task quality |
| Knowledgebase Integrity review | The Knowledgebase Integrity table only | Walk rows, ask what's been resolved |
| Post-incident check | Sections related to what changed | Deep, something specific happened |
| Periodic maintenance | Everything, weighted by staleness | Medium, prioritize oldest sections |

For task-targeted checks, identify the critical data dependencies first. Example: "If you're about to prototype a new flow, the sections that matter most are Knowledgebase Section 2 (Architecture), Section 3 (Features), Section 4 (Core User Flows), and the Knowledge Base Overview's Integrations table and Knowledgebase Integrity rows that touch the flow or any of its assumptions."

---

## Document-Level Scan: The Knowledge Base Overview

### Platform URL

- [ ] **Platform URL present on the Knowledge Base Overview Product Overview?** If missing, bare text, or a placeholder like "TBD", this is **P0**.
- [ ] **Platform URL resolves to a working environment?** Open it. If it returns a 404, redirects somewhere unexpected, or lands on a broken page, **P0**.

### Product Overview links

- [ ] Core User Flows (links) resolve?
- [ ] Customer Journey Maps (links) resolve? If "TBD, not yet created", confirm whether a Knowledgebase Integrity row exists for creating them.
- [ ] Service Blueprints (links) resolve? Same TBD handling as journey maps.
- [ ] Pendo Dashboard link resolves?
- [ ] Figma Links resolve?
- [ ] Environments links resolve?

### Key Stakeholders sub-table

Every role should have a name or "N/A". Check each role explicitly, not as a catch-all.

- [ ] Customer Success lead, same person?
- [ ] Relationship Manager, same person? (or still "N/A")
- [ ] Sales Lead (or GTM counterpart), same person?
- [ ] Compliance / Legal contact, same person?
- [ ] Integration / Dev Partner contacts (e.g., Rock Developer Partner), same people?
- [ ] Executive sponsor (beyond VP), same person?
- [ ] Any other stakeholders still in role?

### Business Context

- [ ] Customer count still approximately accurate?
- [ ] Revenue / ARR still approximately accurate?
- [ ] Pricing model changed?
- [ ] Competitors list still current?
- [ ] Key differentiator still accurate?
- [ ] TAM estimate still reasonable?

### Integrations & Third Parties

For every row in each of the three sub-tables (Host System, Product Integrations, Backend Systems):

- [ ] Still integrated with this partner or still using this service?
- [ ] Integration type still the same?
- [ ] Any new integrations added?
- [ ] Any removed, replaced, or vendor changes?

### Product Initiatives (three tables)

**In Progress table:**

- [ ] Each row still actively being built?
- [ ] Status value is one of: In Design, Refining, Implementing, QA, Commercializing, Releasing. Any invalid?
- [ ] Any items here have actually shipped? (If yes, move to Recently Completed.)
- [ ] Any items have stalled? (Update status or move to a different table.)
- [ ] All linked docs still resolve?

**Planned table:**

- [ ] Each row still planned, or has priority shifted?
- [ ] Status value is one of: Discovering, Validating, Awaiting Approval. Any invalid?
- [ ] Any items should now be in In Progress?
- [ ] All linked docs still resolve?

**Recently Completed table:**

- [ ] Any items older than ~6 months that should be pruned?
- [ ] Released dates accurate?

### Discovery Backlog

- [ ] Every row has a Status value from the discovery lifecycle list (Pre-Discovery, Kick-Off, Problem Framing, Researching, Initial Prototyping, Discovery Workshops, Gathering Data, Ideation, Prototyping, Early Team Refinement, Work Planning)?
- [ ] Any items have completed discovery and moved to Planned?
- [ ] Any items deprioritized or removed?
- [ ] All linked docs still resolve?

### Knowledgebase Integrity

This is the trust layer. It must be tight.

- [ ] **Every row has all seven columns populated: Name, Description, Urgency or Importance, Assigned To, Status, Date Flagged, Target Resolution Date.** Rows missing any column are incomplete. Fill them in during the audit.
- [ ] **"Description" contains a concrete action, not a method label.** "Pull Pendo report filtering by [feature] over 90 days" is concrete. "Check analytics" is not.
- [ ] **"Assigned To" names a specific person or role**, not "TBD" or "someone".
- [ ] **"Target Resolution Date" has a date or cadence**, not blank.
- [ ] **"Urgency or Importance" is one of**: Critical, High, Medium, Low (with a one-line reason).
- [ ] **Status is one of**: Open, Planned, In Progress, Resolved. Any invalid values?
- [ ] Any rows with Status "In Progress" that have stalled? Update or reassign.
- [ ] Any rows marked "Resolved" that can be cleaned up on this pass?
- [ ] Are there claims in the Knowledgebase that should be new Knowledgebase Integrity rows? Add them.

---

## Document-Level Scan: The Knowledgebase

### Human Review Status (top of the document)

- [ ] "Fully reviewed by a human end-to-end?" Yes or No.
- [ ] If No or if last review date is older than 6 months, flag it. Ask the user if they want to run a review now or schedule one.
- [ ] Reviewer name populated?
- [ ] Any notes from last review still relevant?

### Date-stamped claims

Scan the Knowledgebase for any of these patterns:

- "as of [date]..."
- "since [month/year]..."
- "in [quarter/year] we..."
- "currently..." (was this true at writing time?)
- "we haven't yet..." / "we don't yet..."
- "planned for [date]..."
- "expected by [date]..."
- "recently..." (when was "recently"?)
- "soon..." / "upcoming..."
- "new..." (may no longer be new)

For each, ask: "The Knowledgebase says '[claim]'. Is this still accurate?"

### Section 4 Core User Flows (critical)

For each documented flow:

- [ ] **Artifact link populated in the subheader table?** If missing, this is a Knowledgebase Integrity row: "Flow [name] has no artifact link".
- [ ] **Artifact link resolves?**
- [ ] **"Last verified against live product" date present?** If older than ~3 months, re-verify against the live product.
- [ ] Steps still match the live product?
- [ ] Any steps added, removed, or reordered since the last verification?
- [ ] Commit point still where the document says?
- [ ] Pain Points still accurate?

### Section 3 Features & Functionality

- [ ] Features described still present in the live product?
- [ ] New features in the live product not yet documented?
- [ ] Settings, defaults, configuration options still match?

### Section 2 Platform Architecture

- [ ] Tech stack description still matches? (Ask the user; hard to verify visually.)
- [ ] Data model description still matches?
- [ ] Any architectural changes (monolith to microservices, new caching layer, new API endpoints)?

### Section 5 Current Challenges

- [ ] Listed UX issues still present in the live product?
- [ ] Technical debt items still relevant, or has any been addressed?
- [ ] Missing capabilities still missing?
- [ ] Customer-reported issues still showing up in support?

### Section 6 Personas

- [ ] Personas still accurate or have user segments shifted?
- [ ] Persona data gaps listed that match open Knowledgebase Integrity rows?

### Section 7 Terminology

- [ ] Any new internal terms that should be added?
- [ ] Any terms that have fallen out of use?

### Section 8 Known Risks & Dependencies

- [ ] Compliance exposure still current?
- [ ] Vendor concentration still accurate?
- [ ] Key person risk items still relevant?
- [ ] Any new risks surfaced?

---

## Presenting Findings

Organize findings into four buckets:

### 1. Confirmed Changes

Definitively different. Must be applied.

- What the document says
- What is actually true now
- Proposed update text
- Which page and which section to update

### 2. Possible Changes

Looks different but needs user confirmation.

### 3. Still Current

Confirmed accurate. Reassuring and helps the user know what to trust.

### 4. New Knowledgebase Integrity Rows

- Missing artifacts (flows without links)
- Newly flagged unvalidated claims
- New maintenance to-dos

---

## Applying Updates

For each confirmed change:

1. Show current text and proposed update.
2. Get user confirmation.
3. Apply to the correct page. Remember: most content lives in exactly one place. Team and Platform URL are the exceptions that touch both the Knowledge Base Overview Product Overview and are referenced from context sections in the Knowledgebase.
4. Update the Knowledgebase Integrity table: mark resolved rows "Resolved", add new rows with all seven columns filled in (Name / Description / Urgency or Importance / Assigned To / Status="Open" / Date Flagged / Target Resolution Date).
5. If Section 4 flows were updated, note which HTML / Figma / Lucid artifacts need regeneration and surface that to the user.

Give the user both markdown files clearly labeled:

- `[Product Name] Knowledge Base Overview.md` (parent page)
- `[Product Name] Knowledgebase.md` (child page)

Tell them exactly what to re-paste into Confluence: "Knowledge Base Overview only", "Knowledgebase only", or "Both". Do not say "both updated" if only one was.
