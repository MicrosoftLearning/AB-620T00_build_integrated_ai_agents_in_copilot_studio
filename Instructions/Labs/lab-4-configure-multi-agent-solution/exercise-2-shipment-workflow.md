---
lab:
  title: '4.2: Author a workflow and attach it to a specialist agent'
  description: In this exercise, you author a Copilot Studio workflow with an agent-callable trigger, add a connector action, return typed outputs, publish the flow, and attach it to a specialist agent as a tool.
  duration: 30 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Author a workflow and attach it to a specialist agent

The Fulfillment agent has a persona and knows about SKUs, but it can't actually *do* anything yet. In this exercise, you author a workflow that emails the warehouse team a structured shipment request and returns a shipment ID and ETA back to the agent, then attach the workflow to the Fulfillment agent as a tool. This is the same workflow-as-tool pattern from Lab 2, applied to a specialist agent instead of the orchestrator.

You will complete the following tasks:

- Create a workflow with an agent-callable trigger and typed inputs.
- Add an Outlook action that emails the warehouse team.
- Return a shipment ID and ETA to the agent, publish the flow, and attach it to the Fulfillment agent as a tool.

This exercise should take approximately **30** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 1 — Create and configure a specialist agent](exercise-1-configure-fulfillment.md) first. The Fulfillment agent should have instructions and the Products tool attached.

> [!NOTE]
> The shipment-request email sends to whatever warehouse address you supply at design time. For testing, use your own tenant email so you can see the message arrive.

## Task 1 — Create the workflow

Create a new Copilot Studio workflow with an agent-callable trigger. This is the same starting shape as the workflows in the prior lab — the **When an agent calls the workflow** trigger is what makes the workflow discoverable to any agent (orchestrator or specialist) as a tool.

1. In [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com`, from the left navigation, select **Workflows**.

1. On the **Workflows** page, select **+ New workflow** → **Workflow**.

    The flow designer opens with a **Start** node.

1. Select the **Start** node. In the trigger configuration panel on the right, set **Trigger type** to **When an agent calls the workflow**.

    A **Respond to the agent** node is added at the end of the flow automatically.

## Task 2 — Rename and describe the flow

Rename the flow so the specialist agent knows what it does. The display name is the primary signal at tool-selection time, so keep it clear and action-oriented.

1. At the top of the designer, rename the flow to:

    ```text
    Create shipment request
    ```

## Task 3 — Define the trigger inputs

Define the five trigger inputs the specialist supplies at call time. Two of them (`serialPrefix` and `notes`) are optional so the specialist can still call the flow when the rep hasn't given it those details.

1. Select the **When an agent calls the workflow** node. In the trigger panel, select **+ Add an input** and add each of the following.

    | Name | Type | Description |
    |---|---|---|
    | `customerEmail` | Text | The customer's email address — used only in the warehouse notification, not as a recipient. |
    | `sku` | Text | The SKU of the replacement product to ship. |
    | `quantity` | Number | The number of units to ship. Usually 1. |
    | `serialPrefix` | Text | Optional — a target serial-prefix range if the defective unit was from a specific batch and the replacement should come from a different one. |
    | `notes` | Text | Optional — free-text notes for the warehouse team. |

    > [!NOTE]
    > Mark `serialPrefix` and `notes` optional using the **...** menu on each input → **Make optional**. Leave the first three as required.

## Task 4 — Send the warehouse notification email

Add an Outlook action that sends the warehouse team a structured shipment-request notification. The email content is what a real warehouse team would need to fulfill the request — SKU, quantity, customer email, batch preference if the rep supplied one, and any notes.

1. Between the **Start** node and the **Respond to the agent** node, select the **+** to add a new step.

1. Search for and select the **Office 365 Outlook** connector, then the **Send an email** action.

1. Configure the action with the following settings:

    | Field | Value |
    |---|---|
    | To | Type your own tenant email address for testing. In a real deployment, this would be a shared warehouse mailbox — for example, `warehouse@contoso.com`. |
    | Subject | Type `Shipment request: ` insert the dynamic content **sku** from the trigger, then ` × `, then insert **quantity**. Result at runtime: `Shipment request: BLD-100 × 1`. |
    | Body | A short notification with the SKU, quantity, customer email, serial-prefix preference (if supplied), and notes (if supplied). Use dynamic content for each value. |

## Task 5 — Fill in the outputs to return to the agent

The workflow returns a mock shipment ID and an ETA so the agent has something to report back to the rep. In a real deployment, these would come from an inventory or shipping API.

1. Select the **Respond to the agent** node.

1. Select **+ Add an output** and add:

    | Name | Type | Value |
    |---|---|---|
    | `shipmentId` | Text | An expression that generates a unique ID. Click into the field, switch to the **Expression** mode in the value picker, paste `concat('SHIP-', formatDateTime(utcNow(), 'yyyyMMddHHmmss'))`, and select **Insert** to commit. |
    | `eta` | Text | An expression for a business-day ETA. Paste `formatDateTime(addDays(utcNow(), 5), 'yyyy-MM-dd')` in the **Expression** field. Returns a date five days out. |

## Task 6 — Publish the flow

Publish the flow so the specialist agent can call it. An unpublished flow doesn't appear in the tool picker.

1. Select **Save** to save the flow.

1. Select **Publish** at the top of the designer.

1. Wait for the confirmation. Return to the **Workflows** list and confirm **Create shipment request** shows status **Published**.

## Task 7 — Attach the workflow to the Fulfillment agent

Attach the published flow to the **Fulfillment agent** — not the orchestrator. Attaching to the specialist is the multi-agent architecture decision from the lab intro: warehouse actions live with the warehouse specialist, and the orchestrator delegates to it rather than owning the action itself.

1. In the left navigation, select **Agents**, then open the **Fulfillment agent** (not the orchestrator).

1. On the **Build** tab, in the components panel, select **+** next to **Tools**.

1. In the **Add tool** panel, select **Workflows**.

1. Select **Create shipment request**.

1. In the **Tools** list, select **Create shipment request** to open its details.

1. In the workflow details panel, review the display name and the five input parameters. Confirm each maps to the trigger inputs you defined.

1. Add the following description:

    ```text
    Creates a shipment request for a replacement product shipment. Use when a representative needs inventory shipped to a customer. Requires a SKU and quantity and can optionally include a serial-prefix preference and warehouse notes. Returns a shipment ID and estimated delivery date.
    ```

1. Select **Save**.

1. Publish the agent.

You've authored a workflow that emails the warehouse and returns structured outputs, then attached it to the Fulfillment specialist rather than the orchestrator. That's the workflow-attachment decision the design rule points to — the tool lives with the agent whose domain owns the action. In the next exercise, you connect the Fulfillment agent to the orchestrator and test the full multi-agent handoff.
