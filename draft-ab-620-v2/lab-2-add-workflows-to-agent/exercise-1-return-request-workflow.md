---
lab:
  title: '2.1: Author a workflow that writes to Dataverse and sends confirmation email'
  description: In this exercise, you author a Copilot Studio workflow that creates a return-authorization record in Dataverse and sends the customer a confirmation email, then attach it to the agent as a tool.
  duration: 35 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Author a workflow that writes to Dataverse and sends confirmation email

In this lab, you extend the **Customer Support Rep Assistant** — an agent for internal customer service representatives at a company that sells products to consumers — with two workflows that let it take actions on behalf of the rep. The starting agent has identity, instructions, skills, memory, and safety already configured. By the end of this lab, you'll have two workflows attached to the agent and validated against a rep-framed scenario.

Skills describe **how** the agent should behave. Workflows let the agent **do** things — write to Dataverse, send email, call an API. In this exercise, you author a Copilot Studio workflow that creates a return-authorization row in the **Returns** Dataverse table, sends the customer a confirmation email, and attaches the flow to the agent so the orchestrator can call it during a chat.

You will complete the following tasks:

- Create a workflow with an agent-callable trigger and typed inputs.
- Add Dataverse and Outlook actions that produce a real side effect.
- Return typed outputs to the agent, publish the flow, and attach it to the agent as a tool.
- Test the flow end-to-end through the agent's **Preview** tab.

This exercise should take approximately **35** minutes to complete.

## Before you start

This exercise assumes you have a **Customer Support Rep Assistant** agent with instructions, skills, memory, and safety configured, plus an empty **Returns** Dataverse table in the preferred solution. You can reach this starting state by either:

- Completing [Lab 1](../lab-1-design-and-configure-agent/README.md), OR
- Following the setup instructions in [Exercise 0 — Set up the Lab 2 starter state](exercise-0-setup.md).

> [!NOTE]
> Task 9 uses Copilot Credits.

> [!NOTE]
> The confirmation email in Task 5 sends to whatever address the rep supplies at runtime. For testing, use your own tenant email as the customer email so you can see the message arrive. Change it later when you demo the workflow with real customer data.

## Task 1 — Create the workflow

Create a new Copilot Studio workflow with an agent-callable trigger. The **When an agent calls the flow** trigger is what makes this workflow discoverable as a tool the orchestrator can invoke during a chat.

1. In **Copilot Studio**, from the left navigation, select **Workflows** (also labeled **Flows** in some regions).

1. On the **Workflows** page, select **Create your first workflow** (if this is your first) or **+ New workflow** → **Workflow**.

    The flow designer opens with a **Start** node.

1. Select the **Start** node. In the trigger configuration panel on the right, set **Trigger type** to **When an agent calls the flow**.

    As soon as you select the trigger, a second node — **Respond to the agent** — is added at the end of the flow automatically. Together, the trigger and the response action are what make this a valid workflow.

## Task 2 — Rename and describe the flow

Rename the flow so the agent knows when to call it. The display name is a primary signal the orchestrator uses at tool-selection time, so keep it clear and action-oriented.

1. At the top of the designer, rename the flow to:

    ```text
    Create return authorization
    ```


## Task 3 — Define the trigger inputs

Define the four trigger inputs the agent will supply when calling the flow. Each input's name and description tell the agent what to map from the conversation into that field.

1. Select the **Start** node again. In the trigger panel, select **+ Add an input** and add each of the following.

    | Name | Type | Description |
    |---|---|---|
    | `customerEmail` | Text | The customer's email address, taken from the ticket or the rep's chat. |
    | `orderId` | Text | The order ID the return applies to. |
    | `reason` | Text | The customer's stated reason for the return. |
    | `condition` | Text | Physical condition of the item — for example, "unopened", "opened", or "defective". |

    > [!NOTE]
    > The designer doesn't have a **Required** checkbox on inputs. Every input is required by default. To make one optional, select the **...** menu on the input and choose **Make optional**. Leave all four as required.

## Task 4 — Add a step to create the Returns row

Add a step that inserts a row in the **Returns** Dataverse table using the values the agent passed in, plus a generated authorization number and two constants (`Status = Authorized`, `Expected refund = 0`).

**About dynamic content.** Everywhere you fill in a Dataverse field below, don't type the raw text — select the field, then pick the trigger input from the **dynamic content** picker on the right. The picker shows `customerEmail`, `orderId`, `reason`, and `condition` as available outputs from the **When an agent calls the flow** trigger. Selecting one inserts a token that resolves to the actual value at runtime.

1. Between the **Start** node and the **Respond to the agent** node, select the **+** to add a new step.

1. Search for and select the **Microsoft Dataverse** connector, then the **Add a new row** action.

1. Configure the action:

    | Field | Value |
    |---|---|
    | Table name | Returns |
    | Authorization number | An expression that generates a unique ID. Click into the field, switch to the **Expression** tab in the value picker, paste `concat('RA-', formatDateTime(utcNow(), 'yyyyMMddHHmmss'))` (just the expression — nothing else), and select **Add** to commit. At runtime this produces values like `RA-20260907143012`. |
    | Customer email | Dynamic content: `customerEmail` from the trigger. |
    | Order ID | Dynamic content: `orderId` from the trigger. |
    | Reason | Dynamic content: `reason` from the trigger. |
    | Condition | Dynamic content: `condition` from the trigger. |
    | Status | `Authorized` |
    | Expected refund | `0` |

    > [!NOTE]
    > **Approved by** is left blank at this stage — a return is *authorized* here (Status = Authorized), then a *refund approval* fills in Approved by if a supervisor signs off. That's what the refund-approval flow in the next exercise does.

## Task 5 — Send the customer a confirmation email

Add a step that emails the customer confirming the return authorization. This produces a real side effect the rep can point to when the customer asks whether the request went through.

1. After the **Add a new row** action, select **+** to add another step.

1. Search for and select the **Office 365 Outlook** connector, then the **Send an email** action.

    > [!NOTE]
    > In some regions the action label reads **Send an email (V2)**; both invoke the same underlying operation. Pick whichever is available.

1. Configure the action:

    | Field | Value |
    |---|---|
    | To | Dynamic content: `customerEmail` from the trigger. |
    | Subject | Type `Return authorized: ` then insert dynamic content **Authorization number** from the previous **Add a new row** step's outputs. |
    | Body | A short confirmation referencing the authorization number, order ID, and reason. Keep the language neutral so it works for any brand. Use dynamic content for the three values. |

## Task 6 — Fill in the outputs to return to the agent

Fill in the two typed outputs the flow returns to the agent — the authorization number and the expected refund. The orchestrator receives these as tool outputs and folds them into its next reply to the rep.

1. Select the **Respond to the agent** node.

1. Select **+ Add an output** and add:

    | Name | Type | Value |
    |---|---|---|
    | `authorizationNumber` | Text | Dynamic content: **Authorization number** from the **Add a new row** step. |
    | `expectedRefund` | Number | Dynamic content: **Expected refund** from the **Add a new row** step. (This is `0` for now; you'll enrich it from a live Orders lookup in a later exercise.) |

    > [!NOTE]
    > The values above use the flow designer's **dynamic content** picker to reference outputs from earlier steps. If you edit the flow in the code view, you'll see expressions like `outputs('Add_a_new_row')?['body/sup_authorizationnumber']` — that's the underlying reference syntax. The picker builds these for you.

## Task 7 — Publish the flow

Publish the flow so the agent can call it. An unpublished flow doesn't appear in the tool picker.

1. Save the flow.

1. Select **Publish** at the top of the designer.

1. Wait for the confirmation. Return to the **Workflows** list and confirm **Create return authorization** shows status **Published**.

## Task 8 — Add the flow to the agent as a tool

Attach the published flow to the agent as a tool. Attaching is what turns a standalone flow into something the orchestrator can pick and invoke during a conversation.

1. In the left navigation, select **Agents**, then open the **Customer Support Rep Assistant**.

1. On the **Build** tab, in the components panel, select **+** next to **Tools**.

1. In the **Add tool** panel, select **Workflows**.

    Your **Create return authorization** flow appears in the list.

    > [!NOTE]
    > If it doesn't appear, confirm the flow is published (Task 7) and that the trigger and response action are the **When an agent calls the flow** trigger and **Respond to the agent** action. See [Add a workflow as a tool to an agent](https://learn.microsoft.com/microsoft-copilot-studio/workflows-experience/flow-agent) at `https://learn.microsoft.com/microsoft-copilot-studio/workflows-experience/flow-agent`.

1. Select the flow, then select **Add and configure**.

1. In the workflow details panel, review the display name, description, and the four input parameters. Confirm each maps to the trigger inputs you defined. Save.

## Task 9 — Test in Preview

Test the flow end-to-end from the agent's **Preview** tab. The agent picks a tool based on its instructions, skill descriptions, and tool descriptions combined — no instruction changes are needed here because the base instructions already cover return-authorization intent and the flow's description tells the orchestrator when to call it.

> [!NOTE]
> This task uses Copilot Credits.

> [!NOTE]
> The confirmation email is sent to whatever value the rep provides for `customerEmail`. For this test, use your own tenant email address so you can see the message arrive.

1. Select the **Preview** tab.

1. Enter the following prompt and select **Send**:

    ```text
    A customer, Priya, with email <your-tenant-email> is on order 9876. She says the blender is defective and wants to return it. Please authorize the return.
    ```

    Replace `<your-tenant-email>` with the email address of the tenant account you're using for the lab.

1. Confirm:
    - The agent calls **Create return authorization** and passes the four inputs.
    - The reply includes an authorization number (starts with `RA-`) and an expected refund of `0`.
    - A confirmation email arrives in the tenant inbox.
    - A new row exists in the **Returns** table with the values the agent passed.

You've authored a workflow, wired its inputs and outputs, published it, and attached it to the agent as a tool. In the next exercise, you layer a human-in-the-loop refund-approval flow on top.
