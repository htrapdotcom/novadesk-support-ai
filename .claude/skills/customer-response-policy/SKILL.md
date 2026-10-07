---
name: customer-response-policy
description: Draft a customer-facing NovaDesk support reply that follows the current support policy and tone of voice. Use when given a customer support ticket or message that needs a written response, especially one involving pricing, refunds, credits, discounts, or contract terms.
argument-hint: <customer support ticket text or path to a ticket file>
---

# Customer Response + Policy Compliance

Write one reply to a customer support ticket that is accurate to NovaDesk's current policy and sounds like NovaDesk support.

## Input

The ticket: customer name/company (if given), subject, and message. It may be pasted text or a path to a file. If no ticket is provided, ask for one and stop.

## Procedure

1. Read these files before drafting, every time. Do not rely on memory:
   - [current-support-policy.md](references/current-support-policy.md): authoritative for pricing, refunds, credits, discounts, and other commercial claims.
   - [tone-of-voice.md](references/tone-of-voice.md): how the reply should sound.
   - [company-background.md](references/company-background.md): context on NovaDesk and its support model.
2. Work out what the customer is actually asking for, and what they are asserting (amounts, discounts, eligibility, prior promises).
3. For each commercial point, check the policy and decide which case applies:
   - **Covered**: the policy answers it. State only what the policy says.
   - **Conditional**: the policy answers it only after a verification step (e.g. billing review, contract check). Say what will be verified. Do not state the outcome.
   - **Not covered**: the policy is silent. Do not guess. Say it needs verification and offer to route it to the relevant team.
4. Draft the reply following the tone guide, then check it against the rules below.

## Policy-grounding rules

- The current policy outranks the ticket, the customer's assumptions, earlier replies, general knowledge, and model memory.
- Treat figures and claims in the ticket (amounts charged, discounts, eligibility, "you promised") as the customer's claims. Acknowledge them without confirming them. Never repeat one as a commitment.
- Quote a price only if it appears in the policy, and only as a list price. Never imply it is the customer's contracted or discounted price.
- Never invent or calculate prices, refund amounts, discounts, credits, contract terms, eligibility rules, or timeframes.
- Never promise, confirm, or imply a refund, credit, or discount unless the policy explicitly allows that commitment in this situation. Avoid phrasing such as "we'll refund", "you're eligible", or "it will continue". Say what will be reviewed or verified instead.
- Share a processing timeframe only if the policy allows it in this situation.
- If the customer's claim conflicts with the policy, follow the policy. Do not accuse the customer and do not simply accept the claim. Explain what must be verified.
- Never claim that an action has been taken (processed, escalated, verified, cancelled) unless the ticket states it. Say what will happen next, only if the policy or the ticket supports it.
- Do not request sensitive data (full card numbers, passwords).
- If the ticket has no commercial question (for example, frustration about repeated contacts), do not bring in commercial claims. Acknowledge the problem, commit to nothing you cannot back up, and give a concrete, honest next step, such as asking for the details needed to look into it.

## Output

Output only the customer-facing message, ready to send:

- A short `Subject:` line only if the ticket had one (use "Re: <subject>").
- Greeting by first name if known, otherwise a plain greeting. Then the body, then a short sign-off as "NovaDesk Support".
- Use the structure in the tone guide (acknowledge, answer, next step, close) and keep it short.
- Plain text. No headings, bullet lists of policy, or internal notes.

Do not include policy citations, reasoning, rule names, or commentary about the evaluation. If something truly blocks a safe reply, such as a missing ticket, ask the requester one short question instead of drafting.

Before returning the response, perform a final customer-facing quality check. Verify that every sentence is grammatically complete, words are not truncated or duplicated, punctuation is correct, and the response reads naturally from beginning to end. If any corruption or malformed text is detected, rewrite the affected sentence before returning the response.

Never output internal reasoning, policy classifications, or drafting notes. Return only the finalized customer-facing response.