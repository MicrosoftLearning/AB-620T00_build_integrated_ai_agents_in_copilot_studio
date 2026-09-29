# Customer support FAQ (reference content)

This is the reference FAQ text that the `faq-lookup` skill (Lab 1 Exercise 3) is designed to answer from. In a real deployment, this content would live in a Dataverse table, a SharePoint page, or an MCP-served knowledge index. For the labs, the content is embedded inline in the skill's instructions — this file exists as the human-readable copy so instructors and reviewers can see exactly what the skill knows.

The skill deliberately never quotes a specific number (dollar figure, day count) unless the FAQ text below states it and the skill cites the section by name.

---

## Shipping

**Standard shipping** takes 3–5 business days from the ship date. **Expedited shipping** takes 1–2 business days. Shipping is calculated at checkout based on destination and item weight. Tracking numbers are emailed within 24 hours of the order shipping.

**Order not arrived by the estimated date?** Confirm the ship date and tracking number first, then check the carrier's tracking page. If tracking hasn't updated in 48 hours, escalate to shipping-support.

**Do not** quote a specific delivery date to the customer without checking the order record — dates in this FAQ are typical ranges, not commitments.

## Returns and refunds

**Standard return window:** 30 days from delivery for most items.

**Electronics:** 14 days from delivery if opened, 30 days if unopened.

**Non-returnable:** perishables, personalized items, gift cards.

**Refunds** are processed to the original payment method within 5–7 business days of the return being received.

**Restocking fee:** waived on defective items; up to 15% on non-defective opened items in the electronics category.

**Outside the return window?** That's a supervisor decision. Do not promise an exception.

## Warranty

**Standard warranty:** 12 months from purchase date, covering manufacturing defects.

**Extended warranty:** 24 or 36 months available on select products (see product page or the product catalog). Extended warranty is not sold separately after purchase — it's included with the product SKU.

**Not covered:** accidental damage, cosmetic wear, damage from unauthorized repairs or modifications.

**Warranty claims** require proof of purchase (order ID or receipt). For defects, direct the customer to start a return under the standard return flow — refunds and replacements route through the same workflow.

## Account and access

**Password reset:** direct the customer to the "Forgot password" link on the sign-in page. Reset emails arrive within 15 minutes; check spam if not received.

**Locked out after multiple attempts:** the account unlocks automatically after 30 minutes.

**Account changes (email, address, payment method):** the customer completes these in their account settings. Do not change account details on the customer's behalf — that's a security-team escalation.

**Suspected unauthorized access or unrecognized charges:** immediately escalate to the fraud team. Do not attempt to resolve inline.

## Order status meanings

- **Placed** — payment authorized, order accepted, not yet fulfilled.
- **In fulfillment** — being picked and packed at the warehouse.
- **Shipped** — handed to the carrier; tracking number issued.
- **In transit** — carrier has scanned the package after handoff.
- **Out for delivery** — carrier is delivering today.
- **Delivered** — carrier reports delivery complete.
- **Returned** — customer-initiated return, item back at warehouse.
- **Refunded** — refund posted to the original payment method.

If a status has been stuck for more than 48 hours, treat that as a signal to investigate — not to guess. Check the order record and, if unclear, escalate.
