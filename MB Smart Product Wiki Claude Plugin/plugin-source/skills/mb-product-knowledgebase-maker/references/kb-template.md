# Knowledgebase - Page Template

This template is for the **child Confluence page** for a product. In Confluence it is named "[Product Name] Knowledgebase" and lives nested inside the parent "[Product Name] Knowledge Base Overview" page. See `references/cover-page-template.md` for the Knowledge Base Overview template.

The Knowledge Base Overview holds everything operational and business: team, stakeholders, initiatives, discovery backlog, KB integrity, integrations, business context, environments. The Knowledgebase holds the technical and product-internal depth: architecture, features, user flows, current challenges, personas, terminology, and risks.

The two pages must not duplicate each other. If a topic that lives on the Knowledge Base Overview is relevant for context inside the Knowledgebase, link to the Knowledge Base Overview rather than copying content.

The Knowledgebase should be thorough enough that a new team member can read it cold and understand how the product actually works.

Writing style: no em dashes. Use periods, commas, colons, or parentheses. Hyphens between single words (for example "data-informed") are fine. No formulaic contrast phrasing. Keep prose plain and direct.

---

## Template Structure

```markdown
# [Product Name] Knowledgebase

---

## Human Review Status

This block tracks whether a human has done a full end-to-end review of this document. AI-generated and AI-assisted knowledgebases can look polished without actually being correct; a human has to read the whole thing before it can be trusted as authoritative. This is distinct from the Knowledgebase Integrity table on the Knowledge Base Overview: Knowledgebase Integrity tracks specific unresolved gaps, this tracks whether a person has actually sat down and read the document end to end.

| | |
|---|---|
| **Fully reviewed by a human end-to-end?** | [Yes / No] |
| **Last full review date** | [YYYY-MM-DD, or "Never"] |
| **Reviewer name** | [Name of the person who did the full review] |
| **Notes from last review** | [Any caveats, corrections, or open questions the reviewer wanted to flag] |

If "Fully reviewed" is No, every consumer of this document should treat it as a working draft, not an authoritative source.

---

## 1. Product Overview

[2 to 3 paragraphs covering: what the product does, who it serves, core value proposition, lifecycle stage. Narrative form.]

### Primary Use Case

[How most users interact with the product. The 80 percent use case. Include integration context if relevant.]

### Core Features

[Bullet list of the main capabilities.]

---

## 2. Platform Architecture

### Architecture Overview

[API-first? Monolith? Microservices? How the system is structured and why it matters for understanding the product.]

### Tech Stack

- Frontend: [framework, language]
- Backend: [framework, language]
- Database: [type, name]
- Hosting: [cloud provider, infrastructure notes]
- Other: [CDN, queuing, caching, search, etc.]

### Data Model: [Key Entity Name]

[Describe the core data model. The main entity (user, order, project, etc.) and how it flows through the system. Include nuances like: where records originate, how they sync, what happens on creation or deletion, and any gotchas about persistence or commit points.]

### Embedded / Integration Architecture

[If the product integrates with host systems, describe: how the embedding works, what data flows where, what is shared vs independent. For the full list of integration partners and backend vendors, see the Knowledge Base Overview page.]

---

## 3. Features & Functionality

This section is the feature inventory. For step-by-step user paths, see Section 4 Core User Flows. For what is broken or missing, see Section 5 Current Challenges.

### Information Architecture

[Describe the product's navigation structure. Sidebar items, top-level sections, settings location.]

### [Feature Area 1] (example: Dashboard)

[What the user sees, key metrics or cards, available actions, filters.]

### [Feature Area 2] (example: Orders)

[Description of this area, what it contains, key features.]

[Continue for each major feature area of the product.]

### Settings & Configuration

[What can be configured, default values, admin-only features, notification settings.]

---

## 4. Core User Flows

This section is primarily for machine consumption, not human reading. Humans looking at user flows should open the linked flow artifact directly (Figma flow, Lucid chart, interactive HTML flow map). This section exists so an LLM, auditor, or automated process can compare the live product against documented flows without needing to interpret a visual artifact.

**Every flow in this section must include a link to the actual flow artifact in the subheader table.** A text-only description is not sufficient. If a flow has no artifact link, flag it in the Knowledge Base Overview Knowledgebase Integrity table with Status "Open" so someone creates one.

### Flow 1: [Flow Name]

| | |
|---|---|
| **Flow artifact (REQUIRED)** | [Link to Figma flow page, Lucid chart, interactive HTML flow map, or whatever canonical artifact represents this flow. This link is mandatory.] |
| **Trigger** | [How the user starts this flow] |
| **Commit Point** | [When the action becomes permanent. What gets created or saved and at what moment.] |
| **Last verified against live product** | [YYYY-MM-DD] |

**Steps (textual representation for machine comparison):**

1. [Step description. What the user sees, what they do, what fields, what buttons, what options.]
2. [Next step.]
3. [Continue.]

**Post-Flow:** [What happens after. Confirmations, redirects, success state, where the user lands.]

**Known Pain Points:**

- [Issue 1]
- [Issue 2]

### Flow 2: [Flow Name]

| | |
|---|---|
| **Flow artifact (REQUIRED)** | [Link] |
| **Trigger** | [...] |
| **Commit Point** | [...] |
| **Last verified against live product** | [YYYY-MM-DD] |

**Steps:**

1. [...]

[Continue for each core flow.]

---

## 5. Current Challenges

This section describes what is wrong, broken, frustrating, or missing. Keeping it separate from Features & Functionality keeps both sections cleaner.

Organize by category: UX Issues, Technical Debt, Missing Capabilities, Customer Pain Points, Team / Process Challenges.

### UX & Usability Issues

1. **[Issue Title]** - [Description. Who it impacts. Why it matters.]
2. **[Issue Title]** - [Description.]

### Technical Debt & Constraints

1. **[Issue Title]** - [Description.]

### Missing Capabilities

1. **[Issue Title]** - [Description.]

### Customer-Reported Issues

1. **[Issue Title]** - [Description.]

---

## 6. Personas

If personas were built using the MB Persona Maker skill, link to the full persona document here. Otherwise include a short summary per persona.

### Primary Persona

[Name, role, goals, frustrations, key workflows. Or link to full persona doc.]

### Secondary Persona

[Same structure.]

### Tertiary Persona (if applicable)

[Same structure.]

### Persona Data Gaps

[Any persona attributes based on assumptions rather than validated data. These should also appear as rows in the Knowledge Base Overview Knowledgebase Integrity table with Status values so they roll up into the document's trust layer.]

---

## 7. Terminology & Mental Models

A glossary of product-specific terms and how the team actually talks about the product. One of the most valuable sections for onboarding. Every product has internal jargon that is not obvious to newcomers.

| Term | Meaning | NOT This |
|------|---------|----------|
| [term] | [what the team means] | [common misconception or alternative term] |

---

## 8. Known Risks & Dependencies

Risks and dependencies that could affect the product: regulatory and compliance exposure, single-vendor dependencies, key person risk, integration fragility, and anything else the team would want a new person to be aware of before they start making changes.

| Risk / Dependency | Description | Owner |
|-------------------|-------------|-------|
| [e.g., Compliance exposure] | [What the risk is and why it matters. E.g., "FCRA governs background screening disclosures; any change to consent flow must be reviewed by Legal."] | [Who monitors this] |
| [e.g., Vendor concentration] | [E.g., "Background screening is single-sourced through Digital Delve. No failover vendor. Contract renews [date]."] | [Who owns the relationship] |
| [e.g., Key person risk] | [E.g., "Team is Hexaware-heavy; two devs hold most of the codebase knowledge. Succession plan needed."] | [Who] |

Categories to consider:

- **Compliance / regulatory exposure** (FCRA, HIPAA, PCI, SOC2, GDPR, CCPA, etc.)
- **Vendor concentration** (single-vendor dependencies, contracts at risk, replacement costs)
- **Key person risk** (knowledge held by one or two people, succession gaps)
- **Integration dependency** (host systems, partner APIs, fragile webhooks)
- **Data / migration risk** (legacy data formats, bulk migration debts)
- **Technical debt with strategic impact** (specific debt items that would delay or block strategic work)
```

---

## Style Notes

- Clear, direct prose. No corporate fluff.
- Tables for structured data. Prose for context and narrative.
- Section 4 Core User Flows **must** include a link to the flow artifact per flow. Text-only is not acceptable. The text steps are for machine comparison, the artifact link is for humans.
- Section 6 Personas: a short summary is fine. The full persona doc lives wherever the Persona Maker skill placed it, linked from this section.
- Do **not** include Last Updated, Last Audited, or Owned by here. Confluence tracks edit history natively. The Human Review Status block at the top of the Knowledgebase is the one exception, because "has a human actually read this" is not something Confluence tracks.
- Do **not** duplicate content that lives on the Knowledge Base Overview. If you find yourself writing about Team & Stakeholders, Initiatives, Discovery, Knowledgebase Integrity, Integrations, or Business Context, stop and put it on the Knowledge Base Overview instead.
- No em dashes anywhere. Use periods, commas, colons, or parentheses. Hyphens between single words are fine.
- The document should be thorough enough that a new team member can read it cold and understand the product.
