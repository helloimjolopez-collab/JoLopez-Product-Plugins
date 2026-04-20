# Interview Guide

The interview is the bridge between what the product shows you and what it hides. The goal is short: ask only what you cannot learn from clicking through the product.

## Principles

1. Ask per step, not once at the start. Questions tied to a specific step get specific answers. Blanket questions get vague ones.
2. Decide which questions to ask based on what you observed. If a step looks like a pure in-product action with no side effects, skip most of the lanes for that step.
3. Cap it at 3 to 4 questions per step unless the user volunteers more.
4. If the user does not know, mark the cell "Needs clarification" and move on. Do not probe.
5. Apply the writing style rules (see SKILL.md) when phrasing questions.

## Question bank

### Support Processes

Trigger: the step looks like it might involve a human on the company side.

- "During this step, does anyone from support, sales, or CS get involved? If so, what do they do?"
- "Is there a queue or manual review that happens here?"
- "Is a call scheduled or a meeting booked at this step?"

### Communications

Trigger: almost always worth asking once per flow. Skip for obvious in-session steps.

- "Does the user receive an email, SMS, push, or phone call during or right after this step?"
- "Does the company send any notification internally when this happens?"

### Backend Interactions

Trigger: the step looks like it might kick off a system process (create account, submit payment, trigger sync).

- "When the user does this, what happens on the backend? Any jobs that run, credentialing, syncs, webhooks?"
- "Is anything async? Does the user wait for something to complete before they can continue?"

### Third Parties / Vendors

Trigger: the step has visible hallmarks of a third party (redirect, branded widget, logo), or the user mentioned vendors during Phase 1.

- "Is any third party involved at this step? (Payment, identity, CRM, ChMS, etc.)"
- "Who owns what here? If the third party goes down, what breaks for the user?"

### Pain Points

Trigger: every step. Always worth asking, but keep it crisp.

- "Do you know of any friction, confusion, or drop-off here?"
- "Have users complained about anything at this step?"

### Emotion

Trigger: every step. Use the 5-point scale.

- "On a 1 to 5 scale where 1 is frustrated and 5 is delighted, how does the user typically feel at this step?"
- If the user is unsure: "If you had to guess, would they be above or below neutral here?"

### Opportunities (synthesized, not asked directly)

Do not ask "what's the opportunity here?" It produces hollow answers. Instead, derive opportunities from:

- Pain points the user named (opportunity = relieve that pain).
- Gaps you observed (e.g., no confirmation at a critical step).
- Emotion dips (low emotion with no pain point named means something is missing from the user's answer, ask follow-up).

## Heuristics for which questions to ask per step

| What you observed | Ask | Skip |
|---|---|---|
| Standard in-product click (navigate between screens) | Pain Points, Emotion | Backend, Communications, Third Parties, Support |
| Form submission (signup, payment, settings change) | Backend, Communications, Pain Points, Emotion, Third Parties | Support (unless review is obvious) |
| Payment or identity step | All lanes |  |
| Screen showing "check your email" or "confirmation sent" | Communications, Pain Points, Emotion | Backend (already implied) |
| External redirect (third party site, vendor page) | Third Parties, Pain Points, Emotion, Communications | Support |
| Success screen, terminal state | Communications, Pain Points, Emotion, Opportunities (synthesized) | Backend, Support, Third Parties |
| Waiting or async state (processing, pending) | Backend, Communications, Pain Points, Emotion | Third Parties (unless external) |

## Batching

Ask multiple questions in one turn when they share a topic. Example:

> "For step 3 (enters payment info): what happens on the backend when they submit? Do they get a confirmation email? Any third party involved? Known pain points?"

One answer captures the whole step. Do not ask 4 separate turns.

## When to stop interviewing

- You have enough to fill every cell that the lane config requires.
- The user is running out of energy or giving short "I don't know" answers in a row. At that point, fill the rest with "Needs clarification" tags and move to Phase 4.
