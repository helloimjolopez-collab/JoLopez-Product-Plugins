# Interview Question Bank

This is the full question list organized by category. Ask questions one at a time or in small clusters (2-3 related questions). Don't dump the whole list - guide the conversation naturally.

## Validation Probing - Woven Throughout

As you ask these questions, listen for answers that contain unsubstantiated claims or vague quantifiers. When you hear them, probe gently using the patterns below. The goal is to understand the confidence level, not to interrogate.

**Trigger phrases to listen for:** "most users," "a lot of," "usually," "rarely," "almost all," "nobody uses," "everyone wants," "the majority," "a small percentage," "about X%," "I think," "I believe," "I heard that," "CS/Sales told me"

**Probing patterns:**

- *Quantifier probe:* "When you say 'most users' - do you have a rough sense of the actual split? Like 70/30? 90/10? And is that from Pendo data or more of a feel from working with the product?"
- *Source probe:* "That's a really useful insight. Is that something you've seen in the data, or more from your experience talking to customers?"
- *Secondhand probe:* "You mentioned CS told you that - do you know if there's a ticket theme or report behind it, or was it more conversational?"
- *Estimate probe:* "That number is helpful even as a rough estimate. Just so I can flag it accurately - would you say that's from a report or dashboard, or your best approximation?"

**Reassurance language (use generously):**

- "Totally fine if you don't have exact numbers - I just want to track what's confirmed vs. what we'll want to verify later."
- "That's a really common thing to not have data on. I'll flag it and tell you exactly how to check it later."
- "Your instinct here is valuable even without data. Let's note it and I'll suggest a way to confirm it."

Flag every unvalidated claim internally. Don't announce each flag - just keep the list running. You'll compile the full Validation Registry after the interview.

## 1. Product Identity & Overview

1. What's the product name?
2. Give me the one-liner - what does this product do in a sentence?
3. Who are the primary users? What are their actual job titles and what does their day look like?
4. Are there secondary users (people who use it occasionally or for admin purposes)?
5. What's the core value proposition - why do customers pick this over doing it manually or using a competitor?
6. What lifecycle stage is the product in? (new build, early growth, scaling, mature/stable, maintenance/sunset)
7. Is there a public-facing website or marketing page? URL?

## 2. Team & Ownership

8. Who is the Product Manager? (name + email/contact)
9. Who is the Product Designer? (name + email/contact)
10. Who is the Dev Director or Engineering Lead? (name + email/contact)
11. Who is the Dev Manager? (name + email/contact)
12. Who are the Key Stakeholders beyond the core product team? Ask about each role explicitly rather than leaving this open-ended. For any role that doesn't apply, have the user confirm "not applicable" rather than skipping it.
    - Customer Success lead (who owns CS for this product)
    - Relationship Manager (if applicable)
    - Sales Lead (or GTM counterpart)
    - Compliance / Legal contact (especially for regulated products)
    - Integration / Dev Partner contacts (e.g., a contact at a host system vendor such as the Rock Developer Partner)
    - Executive sponsor (the exec beyond the VP who influences product direction)
    - Any other stakeholder who has decision-making influence over this product
13. How big is the engineering team? Rough breakdown by discipline? (frontend, backend, QA, DevOps)
14. Is there a dedicated QA person/team?
15. Any contracted/agency team members?

## 3. Architecture & Technical Context

16. How would you describe the architecture? (API-first, monolith, microservices, serverless, hybrid)
17. What's the tech stack? (frontend framework, backend language/framework, database, hosting/cloud)
18. Are there integrated partner systems where your product is embedded as an integration? (e.g., your product is a plugin inside another platform)
    - If yes: what are the host systems? How does the integration work? (embedded UI, API-only, webhook)
19. Are there third-party integrations inside your product? (e.g., payment processing, email delivery, SMS, analytics, AI/ML services)
    - If yes: list them with what they do
20. What are the backend third-party systems? (screening vendors, data providers, compliance services, etc.)
    - Names, what they handle, relationship type (vendor, partner, in-house)
21. Any relevant infrastructure details? (CDN, message queues, caching layers, search engines)
22. Is there a public or internal API? API documentation link?

## 4. Environments & Access

23. What's the production URL?
24. Is there a staging or test environment? URL?
25. **Platform URL for audits (REQUIRED, non-optional):** What single URL should any future human or machine auditor use to explore this product? This becomes the Knowledge Base Overview's Platform URL field. If the user cannot provide this, block KB generation and explain why: without a Platform URL, no future audit (human or automated) can compare the KB to the live product. If the user names multiple candidate URLs (e.g., an admin view and a user view), ask them to pick the one that should be the canonical reference environment.
26. Can you give me credentials to access the test environment so I can explore it? (email + password)
27. Any other environment links? (admin panels, internal tools, API playgrounds)
28. Are there Figma files for current designs or the design system? Links? (Goes into the Knowledge Base Overview Product Overview: Figma Links.)
28a. **Pendo Dashboard link?** Even if usage is limited, capture the product's Pendo space or dashboard URL. (Goes into the Knowledge Base Overview Product Overview: Pendo Dashboard.)
29. Where does the team track work? (Jira, Linear, Asana, Productboard, etc.) Can I get a link?
30. Is there a UX backlog or product backlog I should look at? Link?
31. Any existing documentation I should read first? (PRDs, specs, wiki pages, architecture docs)
32. Do you have any customer journey maps or service blueprints for this product? (These map the end-to-end experience beyond just the product UI - including touchpoints like onboarding, support, billing, renewal, etc.) If yes, links?
33. Any other relevant resources or links?

## 5. Business Context

32. Approximately how many total customers does the product have?
    - *Validation probe:* "Is that from a dashboard like Power BI, or a rough number? Either is fine - I'll note the confidence level."
33. What's the approximate yearly revenue (or ARR)?
    - *Validation probe:* "Same question - is that from financials or a ballpark? Totally OK if it's a ballpark."
34. What's the pricing model? (per-seat, per-transaction, tiered plans, enterprise quotes, etc.)
35. Who are the key stakeholders outside the product team? (sales leaders, CS leaders, executives)
    - Names and roles if possible
36. What's the current product status? (active development, feature freeze, maintenance, sunsetting)

## 6. Competitive Landscape

37. Who are the 3-5 primary competitors?
38. What makes your product different from them? The key differentiator?
39. What's the estimated Total Addressable Market (TAM)?
40. Any competitive intelligence notes? (features they have that you don't, pricing differences, market positioning)

## 7. Current Functionality (what works today - factual, not editorial)

41. What are the core user flows, the 3 to 5 things users do most often?
    - Walk me through each one at a high level.
    - **For each flow, ask: is there an existing artifact (Figma flow page, Lucid chart, interactive HTML flow map, or similar)?** Capture the link per flow. These links are required on the Knowledge Base Overview Product Overview ("Core User Flows (links)") and on each flow's subheader table in Knowledgebase Section 4.
    - **If a flow has no artifact yet**, flag it as a Knowledgebase Integrity row: "Flow [name] has no artifact link" with Status "Open" and a proposed Who and When.
    - *Validation probe:* "When you say these are the flows users do most, is that from Pendo analytics showing the top flows, or from your understanding of the product? Both are useful, I just want to note the source."
42. How is the product organized? (what's in the sidebar/nav, how do users get around)
43. Are there any features that work completely differently from how they look? (e.g., a button that says "Create" but actually just searches)

## 8. Current Challenges (separate from functionality - what's broken or frustrating)

44. What are the known UX pain points or things users complain about?
    - *Validation probe:* "Are these from user feedback directly, support tickets, or your own observation? If there are ticket themes in the support system, that's great data."
45. Any features that exist but are broken, deprecated, half-built, or confusing?
46. What technical debt is causing the most problems?
47. What are the most common support tickets or customer complaints?
    - *Validation probe:* "Is there a ticket report or CSAT data behind this, or is it more of a sense from talking to the CS team?"
48. Are there internal team frustrations - things that slow you down or cause friction?
49. Are there architectural constraints that limit what you can build or how fast you can ship?

## 9. Product Initiatives (In Progress, Planned, Recently Completed)

All three tables live on the Knowledge Base Overview. Each initiative lives in exactly one based on its current state. Capture name, short description, linked docs (PRD, PIO, brief), and a status value from the allowed list for its table.

50. **In Progress.** What initiatives are actively being built right now? For each, capture:
    - Name
    - One-line description of what it is and what problem it solves
    - Linked docs (PRD, PIO, brief, wiki page)
    - Status (pick one): **In Design**, **Refining**, **Implementing**, **QA**, **Commercializing**, **Releasing**

51. **Planned.** What initiatives are committed but have not started yet? For each, capture:
    - Name
    - One-line description
    - Linked docs
    - Status (pick one): **Discovering**, **Validating**, **Awaiting Approval**

52. **Recently Completed.** What has shipped in the last ~6 months? For each, capture:
    - Name
    - One-line description
    - Linked docs (PRD, PIO, retro if any)
    - Released month/year (e.g., "Feb 2026")

## 10. Discovery Backlog

All discovery items live on the Knowledge Base Overview Discovery Backlog table. One table, one row per item.

53. For each item in discovery or waiting for discovery, capture:
    - Name
    - Short description (the question being explored or the hypothesis)
    - Linked docs (research doc, prototype, discovery artifact)
    - Status (pick one from the discovery lifecycle): **Pre-Discovery**, **Kick-Off**, **Problem Framing**, **Researching**, **Initial Prototyping**, **Discovery Workshops**, **Gathering Data**, **Ideation**, **Prototyping**, **Early Team Refinement**, **Work Planning**

54. Are there any known upcoming asks from stakeholders, customers, or sales that should be in Discovery but are not yet captured?

55. What are the current challenges or blockers the team is facing? (These may be candidates for Known Risks & Dependencies in the Knowledgebase Section 8, or Knowledgebase Integrity rows on the Knowledge Base Overview.)

## 11. Personas

57. Do you have existing user personas for this product? Where are they documented?
58. Are the existing personas still accurate, or have things shifted?
59. **Main Personas (for the Knowledge Base Overview Product Overview "Main Personas" row):** Even before building full personas, give me the short list: name and one-line role per primary, secondary, and tertiary persona. Example: "Sarah, Volunteer Coordinator (primary); Pastor Mike, Campus Pastor (secondary)". This goes into the Knowledge Base Overview Product Overview table. Full per-persona detail goes into Knowledgebase Section 6 (Personas) or a separate persona doc built with the Persona Maker skill.
60. If no full personas exist: would you like to build them now as part of this process, or later?
    - If now: transition to the MB Persona Maker skill (see below).
    - If later: note this as a Knowledgebase Integrity row on the Knowledge Base Overview.

**If the user wants to build personas:** Tell them about the MB Persona Maker skill. It's a companion skill that guides a deep persona creation process - interviewing for user types, goals, frustrations, workflows, and critically, identifying data gaps and giving them a full validation guide for gathering the research they need. The persona output integrates directly into this knowledgebase.

## Follow-Up Questions

These aren't in a fixed order - use them when they naturally come up:

- "You mentioned [X] - can you tell me more about how that works?"
- "When you say [term], what exactly does that mean in your product's context?"
- "Is that the same as [related concept] or different?"
- "How does that interact with [other thing they mentioned]?"
- "If a brand new user sat down at this product, what would confuse them first?"
- "What's the thing about this product that takes the longest to explain to new team members?"
- "Is there anything I haven't asked about that you think is important?"

## Validation-Specific Follow-Ups

Use these when you hear a claim that needs a confidence check:

- "Do you have a rough sense of the actual numbers behind that, or is it more of an instinct?"
- "Is that something you've seen in Pendo or a report, or more from your day-to-day experience?"
- "When you say [quantifier], what's your confidence level - like, pretty sure, or more of a hypothesis?"
- "Has anyone on the team looked into that formally, or is it still an open question?"
- "Would your CS team agree with that, or might they have a different take?"
- "If we pulled the data on that from Pendo or Power BI, do you think it would match what you're describing?"
- "That's helpful context. Is there a specific incident or conversation that shaped that view, or is it a pattern you've noticed over time?"
