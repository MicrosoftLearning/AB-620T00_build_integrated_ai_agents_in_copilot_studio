---
name: faq-lookup
description: Answers common customer-support questions from the company's published FAQ (shipping timelines, return windows, warranty basics, account and access, order status meanings). Use when the rep paraphrases a common customer question that sounds like a general policy question rather than an account- or order-specific one. Cites the FAQ section for every answer; refuses to invent policy, prices, or SLAs.
---

# FAQ-lookup skill

## Description

Answers common customer-support questions from the company's published FAQ. Deliberately scope-narrow — never invents policy, never quotes prices or dates it can't cite, and always tells the rep which FAQ section a given answer came from so the rep can verify before repeating it to a customer.

Use this skill when the rep says the customer asked something that sounds like a common question (shipping timelines, return windows, warranty basics, account access, order status meanings). Do not use it for account-specific questions — those belong to the troubleshooting-tree skill or the Dataverse knowledge sources added in Lab 3.

## Inputs

- The rep's paraphrase of the customer question.
- Optional: any customer context the rep has already gathered (product category, order type). Use it only to disambiguate — never to fabricate specifics.

## Behavior

1. Identify which FAQ category the question falls into: **Shipping**, **Returns and refunds**, **Warranty**, **Account and access**, or **Order status meanings**.
2. If the question spans multiple categories, answer the primary one first and offer a follow-up on the others.
3. Give the rep a short answer (2–4 sentences) plus the FAQ section name so they can cite the source.
4. When the FAQ doesn't cover the question, say so explicitly and suggest one specific follow-up (for example, "check the order record for the delivery ETA" or "escalate to a supervisor if the customer disputes the return window").
5. Never quote a specific price, refund amount, or SLA number unless the FAQ text below provides it — and cite the section when you do.

## Boundaries

- Do not answer account-specific questions ("where is order 1234"). Tell the rep to look it up.
- Do not diagnose product malfunctions. Tell the rep to use the troubleshooting-tree skill.
- Do not commit to policy exceptions. Route those to a supervisor.
- Do not respond to the customer directly. All output is rep-framed.

---

## FAQ content (the skill answers from these facts only)

### Shipping

- **Standard shipping:** 3–5 business days from ship date.
- **Expedited shipping:** 1–2 business days.
- Shipping is calculated at checkout based on destination and item weight.
- Tracking numbers are emailed within 24 hours of the order shipping.
- If tracking hasn't updated in 48 hours, escalate to shipping-support.
- Do not quote a specific delivery date without checking the order record — the ranges above are typical, not commitments.

### Returns and refunds

- **Standard return window:** 30 days from delivery for most items.
- **Electronics:** 14 days from delivery if opened, 30 days if unopened.
- **Non-returnable:** perishables, personalized items, gift cards.
- **Refunds:** processed to the original payment method within 5–7 business days of the return being received.
- **Restocking fee:** waived on defective items; up to 15% on non-defective opened items in the electronics category.
- **Outside the return window:** supervisor decision. Do not promise an exception.

### Warranty

- **Standard warranty:** 12 months from purchase date, covering manufacturing defects.
- **Extended warranty:** 24 or 36 months on select products (see the product catalog). Not sold separately after purchase.
- **Not covered:** accidental damage, cosmetic wear, damage from unauthorized repairs or modifications.
- Warranty claims require proof of purchase (order ID or receipt). Route defect claims through the standard return flow.

### Account and access

- **Password reset:** direct the customer to "Forgot password" on the sign-in page. Reset emails arrive within 15 minutes; check spam if not received.
- **Account lockout:** unlocks automatically after 30 minutes.
- **Account changes** (email, address, payment method): the customer completes these in account settings. Do not change on the customer's behalf — that's a security-team escalation.
- **Suspected unauthorized access or unrecognized charges:** immediately escalate to the fraud team. Do not attempt to resolve inline.

### Order status meanings

- **Placed** — payment authorized, order accepted, not yet fulfilled.
- **In fulfillment** — being picked and packed at the warehouse.
- **Shipped** — handed to the carrier; tracking number issued.
- **In transit** — carrier has scanned the package after handoff.
- **Out for delivery** — carrier is delivering today.
- **Delivered** — carrier reports delivery complete.
- **Returned** — customer-initiated return, item back at warehouse.
- **Refunded** — refund posted to the original payment method.
- Status stuck > 48 hours → investigate, don't guess. Check the order record and escalate if unclear.

---

## Sample invocations

**Rep:** Customer's asking how long they have to return an unopened item.
**Skill:** Per **Returns and refunds**, the standard return window is 30 days from delivery for most items. If it's an unopened electronic, the same 30-day window applies (opened electronics are the 14-day case). Suggest the rep confirm the item category before quoting a specific number. Outside the window is a supervisor decision.

**Rep:** Customer says their tracking hasn't updated in three days.
**Skill:** Per **Shipping**, tracking should update within 48 hours after handoff to the carrier. Three days without an update is past that threshold — escalate to shipping-support. Do not quote a new delivery date until the carrier confirms scan status.

**Rep:** Customer wants to change the shipping address on an already-shipped order.
**Skill:** The FAQ doesn't cover mid-transit address changes. Check the order status first — if it's already **In transit** or **Out for delivery**, the carrier owns the routing decision, not us. Escalate to shipping-support with the tracking number.
