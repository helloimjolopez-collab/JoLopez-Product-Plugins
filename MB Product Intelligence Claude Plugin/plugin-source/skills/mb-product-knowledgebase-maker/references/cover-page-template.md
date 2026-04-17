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

| | |
|---|---|
| **Product Name(s)** | [Official product name(s) and any internal codenames. Example: "Protect My Ministry (internally: Horizon)"] |
| **Platform URL (test/reference env)** | [REQUIRED, non-optional. The single URL any future human or machine auditor should use to explore this product. Must be a real clickable link, not bare text, not a placeholder, not "TBD".] |
| **VP** | [Name] |
| **Dev Director** | [Name] |
| **Dev Manager** | [Name] |
| **Product Manager** | [Name] |
| **Product Designer** | [Name] |
| **Devs** | [Names and affiliations. Example: "Falgun Gachi (Hexaware), Divyang Patel (Hexaware), Donnacha Connolly"] |
| **QA** | [Names and affiliations] |
| **Product Modality** | [How the product is deployed. Example: "Standalone with integrations available", "Embedded-only", "SaaS multi-tenant"] |
| **Integrated Partners** | [Host systems the product integrates into. Example: "Planning Center, Rock CRM, Ministry Platform"] |
| **Environments** | [Links to production, staging, test environments] |
| **Core User Flows (links)** | [Links to the flow artifacts themselves, for example interactive HTML flow maps, Figma flow pages, Lucid charts. One link per flow.] |
| **Customer Journey Maps (links)** | [Direct links to journey map artifacts. If none exist, write "TBD, not yet created"] |
| **Service Blueprints (links)** | [Direct links to service blueprint artifacts. If none exist, write "TBD, not yet created"] |
| **Pendo Dashboard (link)** | [Direct link to the product's Pendo dashboard or space] |
| **Figma Links** | [Direct link to Figma project] |
| **Other Relevant Resources** | [Design system, API docs, any other reference links. One per line or comma separated.] |
| **Main Personas** | [Short list of the primary and secondary personas for this product, names and one-line role each. No deep detail here. Example: "Sarah, Volunteer Coordinator (primary); Pastor Mike, Campus Pastor (secondary); Admin Andy, IT Contact (tertiary)". Full persona detail lives in Knowledgebase Section 6.] |
| **Status** | [Current lifecycle status. Example: "Live, Active Development", "Live, Maintenance", "Beta", "Sunset"] |

### Key Stakeholders

Break stakeholders out by role, not as free text. For any role that does not apply, write "N/A".

| Role | Name | Contact |
|------|------|---------|
| Customer Success lead | [Name] | [Email] |
| Relationship Manager | [Name or "N/A"] | [Email] |
| Sales Lead (or GTM counterpart) | [Name] | [Email] |
| Compliance / Legal contact | [Name or "N/A"] | [Email] |
| Integration / Dev Partner contacts | [Name and org, e.g., "Dmitri Volkov (Rock Developer Partner)"] | [Email] |
| Executive sponsor (beyond VP) | [Name or "N/A"] | [Email] |
| Other | [Name, role, why relevant] | [Email] |

---

## Business Context

| Metric | Value |
|--------|-------|
| Total Customers | [Number or approximate, e.g., "~2,500"] |
| Yearly Revenue / ARR | [Amount or range, e.g., "~$X.XM ARR"] |
| Pricing Model | [Per-seat, usage-based, tiered plans, enterprise quotes] |
| Relevant Competitors | [Comma-separated list of primary competitors] |
| Key Differentiator | [One sentence on what makes this product distinctive in its market] |
| TAM | [Total Addressable Market description, e.g., "US churches and faith-based organizations"] |

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

### Recently Complete

Initiatives shipped in the last ~6 months. Keep this table for context and audit trail. Prune items older than 6 months on regular cleanup.

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

---

## KB Integrity

This table is the single source of truth for everything incomplete, unvalidated, flagged, or needing a human decision in this KB (both the Knowledge Base Overview and the Knowledgebase). Every row must have all columns filled in. Treat each row as a to-do that affects how much a reader can trust this document.

The KB Assistant skill uses this table as the trust layer for any task the user brings. If a user is about to prototype, plan, or make a decision that touches one of these rows, the assistant flags it before the user acts.

| # | Item | What's Missing / Assumed / Incomplete | Why It Matters | How to Validate | Who | When | Status |
|---|------|--------------------------------------|----------------|-----------------|-----|------|--------|
| 1 | [Short name for the item, e.g., "Invite vs manual order split"] | [What specifically is unresolved, e.g., "Assumed 90/10 invite-to-manual split. No data source confirmed."] | [What decision or design depends on this being right. What breaks if it is wrong.] | [Concrete action, not just a method label. E.g., "Pull Pendo report: Orders > filter Order Type = Invite vs Manual > last 90 days > compare counts."] | [Specific person or role] | [Next action date or cadence, e.g., "By 2026-05-15" or "Before next discovery sprint"] | [Status value from list below] |

Valid Status values:

- **Not Started** (acknowledged but no one is working on it yet)
- **In Progress** (actively being validated)
- **Blocked** (cannot progress without something else, name the blocker in the Item column)
- **Validated** (confirmed, keep the row for audit trail until cleanup)
- **Resolved** (integrated into the document; the row can be removed on the next cleanup pass)
```

---

## Style Notes for the Knowledge Base Overview

- This is a dashboard. Keep everything concise. One-liners, not paragraphs.
- Use tables (not bulleted lists) for Product Initiatives, Discovery Backlog, KB Integrity, Integrations, and Key Stakeholders.
- Platform URL is non-optional. If the user cannot provide it, block KB generation and explain why: without it, no future audit can compare the KB to the live product.
- Break Key Stakeholders out by role. Free-text stakeholder lists go stale first.
- Product Initiatives uses three separate tables (In Progress, Planned, Recently Complete). Each initiative lives in exactly one based on its current state.
- Discovery Backlog is one table with a Status column covering the full discovery lifecycle.
- KB Integrity is the one place where unvalidated data, flagged gaps, and maintenance to-dos live. It is the trust layer for the entire document.
- If a field is unknown, write "TBD" rather than leaving blank. Platform URL is the one exception where TBD is not acceptable.
- Do NOT add Last Updated, Last Audited, or Owned by fields. Confluence tracks edit history natively.
- No em dashes anywhere. Use periods, commas, colons, or parentheses. Hyphens between single words are fine.
- Link to the child page at the top or bottom: "For the full technical reference, see: [Product Name] Knowledgebase".
