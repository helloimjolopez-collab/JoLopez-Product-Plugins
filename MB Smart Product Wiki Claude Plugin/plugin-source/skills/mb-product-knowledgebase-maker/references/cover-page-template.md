# Product Knowledge Base Overview - Page Template

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

This is the trust layer for the entire knowledgebase. Every unvalidated claim, missing data point, unresolved assumption, and flagged gap lives here. It lives on the Knowledge Base Overview (this page) so that anyone consulting the KB, human or machine, sees the open questions and assumptions before relying on the KB for a task (prototyping, prioritisation, writing a PRD, making a decision).

Every row must have all columns filled in. If you cannot fill in a column, that is itself a gap to flag.

| Name | Description | Urgency / Importance | Assigned To | Status | Date Flagged | Target Resolution Date |
|------|-------------|----------------------|-------------|--------|--------------|------------------------|
| [Short name for the item, e.g., "Invite vs manual order split"] | [What specifically is unresolved, plus the concrete action to validate it. E.g., "Assumed 90/10 invite-to-manual split. No data source confirmed. Validate via Pendo report: Orders > Order Type = Invite vs Manual > last 90 days."] | [How much this matters. Use a simple ladder: **Critical** (decisions today depend on it), **High** (decisions this quarter depend on it), **Medium** (good to resolve, not blocking), **Low** (housekeeping). Include a one-line reason.] | [Specific person or role responsible for gathering this data. Not "TBD" or "someone".] | [Status value from list below] | [YYYY-MM-DD when first flagged] | [YYYY-MM-DD target for resolution, or cadence like "Before next discovery sprint"] |

Valid Status values:

- **Open** (acknowledged, no one assigned or started yet)
- **Planned** (scheduled, owner and target date set, not yet in flight)
- **In Progress** (actively being validated)
- **Resolved** (integrated into the document; the row can be removed on the next cleanup pass)

The MB Product Knowledgebase Assistant skill reads this table on every load. When a user is about to start a task (prototype, PRD, sprint planning, prioritisation), the Assistant proactively surfaces any rows that could affect the reliability of that task.

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

Every initiative goes in exactly one of the three tables below based on its current lifecycle state. Use tables, not bulleted lists.

### In Progress

Initiatives actively being built right now.

| Initiative | Short Description | Linked Docs | Status |
|-----------|-------------------|-------------|--------|
| [Name] | [One line. What it is, what problem it solves.] | [Link to PRD, PIO (Product Initiative Overview), brief, or whatever canonical doc exists] | [Status value from list below] |

Valid Status values for In Progress:

- **In Design** (active design work)
- **Refining** (requirements or scope being refined)
- **Implementing** (engineering build in flight)
- **QA** (in QA / testing)
- **Commercializing** (go-to-market prep in progress)
- **Releasing** (in the release process)

### Planned

Initiatives committed but not yet started.

| Initiative | Short Description | Linked Docs | Status |
|-----------|-------------------|-------------|--------|
| [Name] | [One line] | [Link to PRD, PIO, brief] | [Status value from list below] |

Valid Status values for Planned:

- **Discovering** (early discovery work in flight)
- **Validating** (validating the approach, data, or hypothesis)
- **Awaiting Approval** (ready to move to In Progress pending sign-off)

### Recently Released

Initiatives shipped in the last ~6 months. **This subsection is mandatory on every Knowledge Base Overview.** Keep this table for context and audit trail. Prune items older than 6 months on regular cleanup. If nothing has shipped in the last 6 months, keep the heading and table present with a single placeholder row ("None in the last ~6 months" across the row) rather than deleting the section.

| Initiative | Short Description | Linked Docs | Released |
|-----------|-------------------|-------------|----------|
| [Name] | [One line] | [Link to PRD, PIO, brief, retro] | [Month Year shipped, e.g., "Feb 2026"] |

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
- **Product Initiatives always uses three separate tables in this order: In Progress, Planned, Recently Released.** All three are mandatory and must always be present on the Knowledge Base Overview, even if a table is empty. Each initiative lives in exactly one based on its current state.
- Discovery Backlog is one table with a Status column covering the full discovery lifecycle.
- If a field is unknown, write "TBD" rather than leaving blank (except Platform URL).
- Do NOT add Last Updated, Last Audited, or Owned by fields. Confluence tracks edit history natively.
- No em dashes anywhere. Use periods, commas, colons, or parentheses. Hyphens between single words are fine.
- Link to the child page at the top or bottom: "For the full technical reference, see: [Product Name] Knowledgebase".
