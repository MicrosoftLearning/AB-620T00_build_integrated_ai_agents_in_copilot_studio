---
lab:
  title: '4.3: Connect a specialist agent to the orchestrator and test end-to-end'
  description: In this exercise, you register a specialist agent as a connected agent on the orchestrator with a scoped delegation description, reinforce delegation in the orchestrator's instructions, enrich a skill with agent-agnostic triage outcomes, and run a multi-agent Preview test end-to-end.
  duration: 25 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Connect the Fulfillment agent, enrich the skill, and test end-to-end

Now you wire it up. The orchestrator gets a **connected agent** with a delegation description that tells it *when* to hand off to Fulfillment. The `product-troubleshooting` skill picks up a new triage-outcome branch — agent-agnostic, so the skill stays reusable — and the orchestrator's instructions carry the actual delegation logic. Then you run one multi-agent Preview prompt end-to-end.

You will complete the following tasks:

- Register the Fulfillment agent as a connected agent on the orchestrator.
- Reinforce delegation in the orchestrator's instructions.
- Enrich the `product-troubleshooting` skill with a replacement-shipment triage outcome.
- Test the full multi-agent handoff in Preview.

This exercise should take approximately **25** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 2 — Author a workflow and attach it to a specialist agent](exercise-2-shipment-workflow.md) first. The Fulfillment agent should have instructions, the Products tool, and the shipment workflow attached.

> [!NOTE]
> Task 4 uses Copilot Credits. A single Preview turn triggers the orchestrator, the connected specialist, and at least one workflow — expect a multi-model-call turn.

## Task 1 — Register the Fulfillment agent as a connected agent

Register the Fulfillment agent as a connected agent on the orchestrator with a scoped delegation description. The delegation description is what the orchestrator reads at tool-selection time to decide whether to hand off to the specialist — so it needs to name the specialist's domain and explicitly exclude domains that belong to other tools.

1. Open the **Customer Support Rep Assistant** (orchestrator).

1. On the **Build** tab, in the components panel, select **+** next to **Connected agents**.

1. In the picker, select **Fulfillment agent**.

1. Provide a descriptive **Description**. This is what the orchestrator reads to decide when to delegate.

    ```text
    Use the Fulfillment agent when the rep needs a replacement shipment scheduled or needs to check SKU inventory or serial-prefix availability for a defective unit. Fulfillment owns warehouse operations and shipment scheduling. Do NOT use for return authorizations, refund approvals, or customer-facing communications — those stay with the orchestrator.
    ```

1. Select **Connect** below the Agent details field.

## Task 2 — Reinforce delegation in the orchestrator instructions

The delegation description above is what the orchestrator reads at tool-selection time. A matching bullet in the orchestrator's own instructions makes the delegation habit explicit.

1. On the orchestrator's **Build** tab, in the **Instructions** area, find the **Scope** section. Add a bullet:

    ```text
    - When the rep needs a replacement shipment or a SKU inventory or serial-prefix check, delegate to the Fulfillment agent. Return authorization and refund approval stay with you — do those first, then delegate the shipment step.
    ```

1. Select **Save** on the top of the page.

## Task 3 — Enrich the `product-troubleshooting` skill

The `product-troubleshooting` skill walks the rep through symptoms but doesn't tell them what to do once a defect is confirmed. You add a triage outcome that includes the replacement-shipment step — described agent-agnostically so the skill stays reusable even if you attach it to the Fulfillment specialist later.

1. On the orchestrator's **Build** tab, in the components panel, open **Skills** and select **product-troubleshooting**.

1. Edit the skill instructions. Replace the existing body with:

    ```markdown
    # Product troubleshooting

    Walks the customer support rep through a short triage tree for common product problems (setup, connectivity, hardware, billing, account access) so the rep can narrow the customer's issue to a category and one concrete next step.

    ## When to activate

    The rep says the customer is having a problem with a product and needs help identifying what's wrong.

    ## Behavior

    1. Ask the rep for the product category (device, subscription, or accessory) if you don't already know it.
    2. Ask one clarifying question at a time. Never ask more than one question in a turn.
    3. Narrow the problem to one of: setup, connectivity, hardware, billing, or account access.

    ## Triage outcomes

    - **Device does not power on (confirmed defect):**
      1. Confirm SKU via the Orders tool.
      2. Determine warranty status from the SKU record.
      3. If under warranty, initiate a return authorization.
      4. If under warranty and the customer wants a replacement, request a replacement shipment. Prefer a newer serial-prefix batch if the defect is batch-scoped.
      5. If out of warranty, provide self-service steps via the `faq-lookup` skill; no return.
    - **Damaged in transit:**
      1. Confirm the shipment date via the Orders tool.
      2. Initiate a return authorization — damaged-in-transit returns don't require a warranty check first.
      3. If the customer wants a replacement, request a replacement shipment.
    - **Wrong item received:**
      1. Confirm the ordered SKU vs. the received SKU with the rep.
      2. Initiate a return authorization; note in the return reason that it was a shipping mismatch.
      3. If the customer wants the correct item, request a replacement shipment for the ordered SKU.

    ## Boundaries

    - Do not quote specific warranty periods, prices, or SLAs — reference the Orders or Products lookup instead.
    - Do not resolve the ticket — this skill hands the rep to the next step, whatever it is.
    - Do not respond to the customer directly.
    - Defer pure policy questions (return windows, refund thresholds, return-shipping fees) to the `faq-lookup` skill.
    ```

1. Select **Save** below the skill body (appears as **Instructions**).

   > [!NOTE]
   > The skill body describes triage outcomes without naming which agent or tool executes each step. That's deliberate — the orchestrator's instructions (Task 2) and the connected-agent delegation description (Task 1) decide who does what. The skill stays reusable if you later attach it to the Fulfillment specialist.

## Task 4 — Test the end-to-end handoff in Preview

Run one multi-tool prompt that exercises the full multi-agent solution end-to-end. A single well-shaped prompt is enough to see the orchestrator handle the knowledge lookup and return authorization, delegate to the Fulfillment specialist for the shipment step, and summarize back to the rep.

> [!NOTE]
> This task uses Copilot Credits. Expect one orchestrator call, one specialist call, and at least two workflow invocations.

1. On the orchestrator, select the **Preview** tab.

1. Enter the following prompt and select **Send**:

    ```text
    Priya on order 9876 is returning her BLD-100 blender — the motor keeps shutting off after a few minutes. She'd like a replacement. Authorize the return for a $340 refund, and get her a replacement shipped. Use <your-tenant-email> for both the customer email and the warehouse notification for testing.
    ```

    Replace `<your-tenant-email>` with the email address of the tenant account you're using for the lab.

1. Expected:

    - Orchestrator looks up order **9876** with the **Orders** tool.
    - Orchestrator calls **Create return authorization** (workflow-as-tool on the orchestrator) — returns an authorization number.
    - Orchestrator delegates to the **Fulfillment agent** for the replacement shipment.
    - Fulfillment agent looks up the SKU with its **Products** tool (BLD-100 details, current serial prefix).
    - Fulfillment agent calls **Create shipment request** — returns a shipment ID and ETA.
    - Orchestrator summarizes back to the rep with the return authorization number and the shipment ID + ETA.
    - You receive two emails at the tenant address: the return-authorization confirmation (sent to the customer) and the warehouse shipment-request notification.

You've configured a single connected agent for a legitimately cross-functional domain, wired routing through both the delegation description and the orchestrator instructions, enriched a reusable skill with agent-agnostic triage outcomes, and validated the full multi-agent flow end-to-end. The Customer Support Rep Assistant now delegates warehouse operations correctly and keeps returns and refunds in its own lane.
