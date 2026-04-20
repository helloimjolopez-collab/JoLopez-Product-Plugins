# Product Knowledge Base Overview - Page Template

> **CANONICAL TEMPLATE.** This file is the single source of truth for the Knowledge Base Overview structure. Both the MB Product Knowledgebase Maker and the MB Product Knowledgebase Assistant read from this file. A byte-identical mirror of this file exists at `skills/mb-product-knowledgebase-assistant/references/cover-page-template.md` so the Assistant has access to it when installed standalone. When editing this file, also update the mirrored copy (the plugin's regenerate script handles this automatically).
>
> **SECTIONS AND ROWS DEFINED IN THIS TEMPLATE ARE NEVER REMOVED from a real KB, even if unpopulated.** If a value is unknown, write "TBD" (or "N/A" for role rows that genuinely do not apply). Never delete the row, heading, or table. Never collapse a multi-table section into a single table. The template may add sections or rows going forward, but the Maker and Assistant skills may only add to an existing KB or populate what is missing. They must never remove a section, row, column, or table that this template defines.

This template is for the **parent Confluence page** for a product. In Confluence it is named "[Product Name] Knowledge Base Overview" (for example, "Horizon Knowledge Base Overview" or "Protect My Ministry Knowledge Base Overview"). It is the single scannable dashboard for everything operational and business about the product.

The deep technical reference lives as a child page nested under this one, called "[Product Name] Knowledgebase". See `references/kb-template.md`.

The Knowledge Base Overview should be scannable in about 60 seconds. A VP, a new team member, or a cross-functional partner should be able to glance at it and understand: what is this product, who owns it, what are we working on, what is in the pipeline, and what is currently incomplete or unvalidated about this document.

Writing style: no em dashes. Use periods, commas, colons, or parentheses. Hyphens between single words (for example "data-informed") are fine. No formulaic contrast phrasing. Keep prose plain and direct.

---

## Where to save this in Confluence

Ministry Brands has a standard Confluence template for product knowledgebases. Use it. Do not invent your own page structure.

**Template URL:** https://ministrybrands.atlassian.net/wiki/spaces/DR/pages/6534463521/Product+Name+Knowledge+Base+Overview

This template lives in the MB Design Ops space. If the user cannot access it, tell them to request access from the design team (Slack them or file a Confluence access request). Do not proceed by making up a different page structure.

Saving process (tell the user to do these steps):

1. Open the template URL above and **duplicate** it into the user's own Confluence space.
2. Rename the duplicated parent page to "[Product Name] Knowledge Base Overview".
3. Rename the nested child page to "[Product Name] Knowledgebase".
4. Copy the full Knowledge Base Overview markdown output and paste it into the "[Product Name] Knowledge Base Overview" page.
5. Copy the full Knowledgebase markdown output and paste it into the "[Product Name] Knowledgebase" child page.
6. Spot-check that tables and headings rendered correctly on paste.

Updating going forward: always run updates through the **MB Product Knowledgebase Assistant** skill, not by hand-editing Confluence. The Assistant produces updated markdown that preserves the template format for clean re-paste.

---

## Template Structure

```markdown
# [Product Name] Knowledge Base Overview

---

## Product Overview

The Product Overview table is the first thing any human or machine reader sees. It must always include every row below. If a value is not yet known, write "TBD" rather than omitting the row, except for Platform URL which is non-optional (see Style Notes).

| | |
|---|---|
| **Product Name(s)** | [Official product name(s) and any internal codenames. Example: "Protect My Ministry (internally: Horizon)"] |
| **Product Modality** | [How the product is deployed. Example: "Standalone with integrations available", "Embedded-only", "SaaS multi-tenant"] |
| **Status** | [Current lifecycle status. Example: "Live, Active Development", "Live, Maintenance", "Beta", "Sunset"] |
| **Platform URL** | [REQUIRED, non-optional. The single URL any future human or machine auditor should use to explore this product in production. Must be a real clickable link, not bare text, not a placeholder, not "TBD".] |
| **Testing Environment URL** | [The canonical test or staging URL used for audits, walkthroughs, and comparisons against documented flows.] |
| **Point of Contact for Testing Environment Credentials** | [Specific person (name + role) who grants access to the test/staging environment. Not a team alias. This person is the one to ping when someone needs to log in.] |
| **Integrated Partners** | [Host systems the product integrates into. Example: "Planning Center, Rock CRM, Ministry Platform". For the detailed breakdown of each integration, see the Integrations & Third Parties section below.] |
| **Backend or Third-Party Systems** | [Key backend vendors, services, and APIs the product depends on. Example: "Stripe (payments), Twilio (SMS), Digital Delve (background checks)". Detailed breakdown in the Integrations & Third Parties section below.] |
| **Total Customers** | [Number or approximate, e.g., "~2,500"] |
| **Yearly Revenue / ARR** | [Amount or range, e.g., "~$X.XM ARR"] |
| **TAM** | [Total Addressable Market description, e.g., "US churches and faith-based organizations"] |
| **Relevant Competitors** | [Comma-separated list of primary competitors] |
| **Customer Journey Map Links** | [Direct links to journey map artifacts. If none exist, write "TBD, not yet created" and add a row in the Knowledgebase Integrity table.] |
| **Service Blueprint Links** | [Direct links to service blueprint artifacts. If none exist, write "TBD, not yet created" and add a row in the Knowledgebase Integrity table.] |
| **Core User Flows (Links)** | [Links to the flow artifacts themselves, for example interactive HTML flow maps, Figma flow pages, Lucid charts. One link per flow. Each linked flow must also appear with a detail table in Knowledgebase Section 4.] |
| **Pricing Model** | [Per-seat, usage-based, tiered plans, enterprise quotes] |
| **Key Differentiator** | [One sentence on what makes this product distinctive in its market] |
| **Main Personas** | [Short list of the primary and secondary personas for this product, names and one-line role each. No deep detail here. Example: "Sarah, Volunteer Coordinator (primary); Pastor Mike, Campus Pastor (secondary); Admin Andy, IT Contact (tertiary)". Full persona detail lives in Knowledgebase Section 6.] |
| **Pendo Dashboard (Link)** | [Direct link to the product's Pendo dashboard or space] |
| **Figma Links** | [Direct link to Figma project] |
| **Other Relevant Resources** | [Design system, API docs, any other reference links. One per line or comma separated.] |

---

## Team & Stakeholders

Two separate tables: the product team (internal ownership) and the key stakeholders (cross-functional partners and external contacts). Every role must always be present in both tables. Use "N/A" for any role that genuinely does not apply. Do not omit rows.

### Team

| Role | Name(s) | Notes |
|------|---------|-------|
| **VP** | [Name] | [Optional context, e.g., VP scope or portfolio] |
| **Dev Director** | [Name] | |
| **Dev Manager** | [Name] | |
| **Product Manager** | [Name] | |
| **Product Designers** | [Name(s)] | [Plural: include every designer on the team] |
| **Engineers** | [Names and affiliations. Example: "Falgun Gachi (Hexaware), Divyang Patel (Hexaware), Donnacha Connolly"] | |
| **QA** | [Names and affiliations] | |

### Key Stakeholders

| Role | Name | Contact |
|------|------|---------|
| **Customer Success** | [Name] | [Email] |
| **Relationship Manager** | [Name or "N/A"] | [Email] |
| **Sales Lead** | [Name or "N/A"] | [Email] |
| **Compliance** | [Name or "N/A"] | [Email] |
| **Relevant Partner Stakeholders** | [One row per partner. Name, org, and why relevant. Example: "Dmitri Volkov (Rock Developer Partner), integration contact"] | [Email] |

---

## Knowledgebase Integrity

This is the trust layer for the entire knowledgebase. Every unvalidated claim, missing data point, unresolved assumption, vague description, and flagged gap lives here. It lives on the Knowledge Base Overview (this page) so that anyone consulting the KB, human or machine, sees the open questions and assumptions before relying on the KB for a task (prototyping, prioritisation, writing a PRD, making a decision).

The MB Product Knowledgebase Assistant populates and maintains this table automatically. During every session, as it reads any part of the KB, it scans for assumptions, data without sources, vague or imprecise claims, and "TBD" values, and adds a row here for each one it finds that is not already tracked. Do not treat this table as something a human has to hand-curate from scratch.

Every row must have all nine columns filled in. If you cannot fill in a column, that is itself a gap to flag.

| Name | Where Mentioned | Description | Importance | Typical Data Source | Assigned To | Status | Date Flagged | Target Resolution Date |
|------|-----------------|-------------|------------|---------------------|-------------|--------|--------------|------------------------|
| [Short, descriptive title, e.g., "Invite vs manual order split assumption"] | [Which page/section/row the unvalidated piece lives in, e.g., "KB Overview > Product Overview > Total Customers" or "Knowledgebase Section 4 > Flow 1 > Steps"] | [What specifically is unvalidated plus the concrete action to validate it. E.g., "Assumed 90/10 invite-to-manual split. No data source confirmed. Validate via Pendo: Orders > Order Type > last 90 days."] | [Critical / High / Medium / Low. Critical = decisions today depend on it. High = decisions this quarter depend on it. Medium = good to resolve, not blocking. Low = housekeeping. Include a one-line reason.] | [Where this data normally lives. Pick from: Pendo / Customer Success (CSM) / Relationship Manager / Stakeholder Interview / User Research / Sales / RevOps / Compliance / Legal / DevOps Ticket / Competitive Audit / Finance / Ministry Brands Corporate / Other (specify).] | [Specific person or role responsible for resolving. Not "TBD" or "someone".] | [Status value from list below] | [YYYY-MM-DD when first flagged] | [YYYY-MM-DD target for resolution, or a cadence like "Before next discovery sprint"] |

Valid Status values:

- **Open** (acknowledged, no one assigned or started yet)
- **Planned** (scheduled, owner and target date set, not yet in flight)
- **In Progress** (actively being validated)
- **Resolved** (integrated into the document; the row can be removed on the next cleanup pass)

Typical Data Source vocabulary (use one of these where possible; use "Other (specify)" only when none fit):

- **Pendo** — product analytics, usage data
- **Customer Success (CSM)** — qualitative signals from existing customers, support trends
- **Relationship Manager** — existing customer relationship context
- **Stakeholder Interview** — internal stakeholders (leadership, cross-functional partners)
- **User Research** — user interviews, diary studies, usability testing, surveys
- **Sales / RevOps** — pipeline, quotes, commercial terms, ICP data
- **Compliance / Legal** — regulatory exposure, contractual constraints
- **DevOps Ticket** — engineering ticket to pull data from logs, DB, instrumentation
- **Competitive Audit** — competitor research, positioning, feature comparison
- **Finance** — revenue, ARR, TAM sizing
- **Ministry Brands Corporate** — portfolio-level data, BD, partnerships

---

## Integrations & Third Parties

### Host System Integrations

Products or platforms where this product is embedded as an integration.

| Host System | Integration Type | What It Does |
|-------------|------------------|--------------|
| [Name] | [Embedded UI, API, webhook] | [Short description] |

### Product Integrations

Third-party services integrated INTO this product.

| Service | Purpose | Integration Type |
|---------|---------|------------------|
| [Name] | [What it does for the product] | [API, SDK, webhook] |

### Backend Systems

| System | Role | Relationship |
|--------|------|--------------|
| [Name] | [What it handles] | [Vendor, partner, in-house] |

---

## Product Initiatives

Every initiative goes in exactly one of the four tables below based on its current lifecycle state. **These are always tables, never bulleted lists.** Every table uses the same four columns in the same order: Initiative, Brief Description, Status, Relevant Links. All four tables are mandatory on every Knowledge Base Overview, even if a table is empty (use a single placeholder row rather than deleting the table).

### Current

Initiatives actively being built right now.

| Initiative | Brief Description | Status | Relevant Links |
|-----------|-------------------|--------|----------------|
| [Name] | [One line. What it is, what problem it solves.] | [Status value from list below] | [Link to PRD, PIO (Product Initiative Overview), brief, or whatever canonical doc exists] |

Valid Status values for Current:

- **In Design** (active design work)
- **Refining** (requirements or scope being refined)
- **Implementing** (engineering build in flight)
- **QA** (in QA / testing)
- **Commercializing** (go-to-market prep in progress)
- **Releasing** (in the release process)

### Planned

Initiatives committed but not yet started.

| Initiative | Brief Description | Status | Relevant Links |
|-----------|-------------------|--------|----------------|
| [Name] | [One line] | [Status value from list below] | [Link to PRD, PIO, brief] |

Valid Status values for Planned:

- **Discovering** (early discovery work in flight)
- **Validating** (validating the approach, data, or hypothesis)
- **Awaiting Approval** (ready to move to Current pending sign-off)

### Backlog

Initiatives identified and scoped at least loosely, but not yet planned for a specific timeframe. Distinct from Discovery Backlog: these are initiatives the team knows it wants to do, just not yet. Discovery Backlog (further down) is for problems and questions that still need discovery research to become initiatives.

| Initiative | Brief Description | Status | Relevant Links |
|-----------|-------------------|--------|----------------|
| [Name] | [One line] | [Status value from list below] | [Link to brief, scoping doc, notes, or "None yet"] |

Valid Status values for Backlog:

- **Identified** (surfaced as a future initiative, not yet scoped)
- **Scoped** (scoped but waiting for capacity or priority)
- **On Hold** (was planned, paused for a specific reason)
- **Deprioritized** (active decision not to do this right now)

### Recently Released

Initiatives shipped in the last ~6 months. Keep this table for context and audit trail. Prune items older than 6 months on regular cleanup. If nothing has shipped in the last 6 months, keep the heading and table present with a single placeholder row ("None in the last ~6 months" across the row) rather than deleting the section.

| Initiative | Brief Description | Status | Relevant Links |
|-----------|-------------------|--------|----------------|
| [Name] | [One line] | [Released Month Year, e.g., "Feb 2026"] | [Link to PRD, PIO, brief, retro] |

---

## Discovery Backlog

Items in active discovery or waiting for discovery work. One table with a Status column covering the full discovery lifecycle.

| Item | Short Description | Linked Docs | Status |
|------|-------------------|-------------|--------|
| [Name] | [One line. The question being explored or hypothesis.] | [Link to research doc, prototype, discovery artifact] | [Status value from list below] |

Valid Status values (discovery lifecycle, roughly in order):

- **Pre-Discovery** (identified, waiting for capacity)
- **Kick-Off** (discovery work just starting)
- **Problem Framing** (scoping the problem)
- **Researching** (user or market research in flight)
- **Initial Prototyping** (early prototypes being built)
- **Discovery Workshops** (cross-functional workshops in progress)
- **Gathering Data** (data collection for validation)
- **Ideation** (divergent idea exploration)
- **Prototyping** (full prototyping phase)
- **Early Team Refinement** (handoff and refinement with delivery team)
- **Work Planning** (sizing, sequencing, and commitment prep)

```

---

## Style Notes for the Knowledge Base Overview

- This is a dashboard. Keep everything concise. One-liners, not paragraphs.
- Use tables (not bulleted lists) for Product Overview, Team, Key Stakeholders, Knowledgebase Integrity, Integrations, Product Initiatives, and Discovery Backlog.
- **Product Overview must always include all required rows** (Product Name(s), Product Modality, Status, Platform URL, Testing Environment URL, Point of Contact for Testing Environment Credentials, Integrated Partners, Backend or Third-Party Systems, Total Customers, Yearly Revenue / ARR, TAM, Relevant Competitors, Customer Journey Map Links, Service Blueprint Links, Core User Flows (Links)). Use "TBD" for unknown values; do not omit rows. Platform URL is the one exception where TBD is not acceptable.
- **Team must always include all seven rows**: VP, Dev Director, Dev Manager, Product Manager, Product Designers, Engineers, QA. Use "N/A" for any role that genuinely does not apply.
- **Key Stakeholders must always include all five roles**: Customer Success, Relationship Manager, Sales Lead, Compliance, Relevant Partner Stakeholders. Use "N/A" for any role that genuinely does not apply.
- Platform URL is non-optional. If the user cannot provide it, block KB generation and explain why: without it, no future audit can compare the KB to the live product.
- **Knowledgebase Integrity lives on this page, prominently, right after the Team & Stakeholders section.** It is the trust layer for the entire document. Anyone consulting the KB, human or machine, must see it before relying on the KB for a task.
- **Product Initiatives always uses four separate tables in this order: Current, Planned, Backlog, Recently Released.** All four are mandatory and must always be present on the Knowledge Base Overview, even if a table is empty. Each initiative lives in exactly one based on its current state. **Initiatives are always rendered as tables with the four columns Initiative, Brief Description, Status, Relevant Links. Never render initiatives as bullet lists, numbered lists, or prose paragraphs. The table format is non-negotiable.**
- Discovery Backlog is one table with a Status column covering the full discovery lifecycle.
- If a field is unknown, write "TBD" rather than leaving blank (except Platform URL).
- Do NOT add Last Updated, Last Audited, or Owned by fields. Confluence tracks edit history natively.
- No em dashes anywhere. Use periods, commas, colons, or parentheses. Hyphens between single words are fine.
- Link to the child page at the top or bottom: "For the full technical reference, see: [Product Name] Knowledgebase".
