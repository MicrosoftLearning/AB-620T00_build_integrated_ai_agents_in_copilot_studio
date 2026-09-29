---
lab:
  title: '2.2: Add human-in-the-loop approval to a workflow'
  description: In this exercise, you author a second workflow that evaluates a refund amount, routes above-threshold requests to a supervisor via Approvals, and updates the Returns record based on the decision.
  duration: 30 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Add human-in-the-loop approval to a workflow

Not every refund should auto-approve. Refunds above a threshold need a supervisor to sign off. In this exercise, you author a **human-in-the-loop (HITL)** workflow that evaluates the requested refund amount, routes above-threshold requests to a supervisor via Approvals, and updates the **Returns** record based on the decision.

You will complete the following tasks:

- Author a second workflow with a threshold branch and an approval action.
- Update the Dataverse row based on the approval outcome.
- Return typed outputs to the agent, publish the flow, and attach it as a tool.
- Test both the above-threshold and below-threshold branches through the agent's **Preview** tab.

This exercise should take approximately **30** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 1 — Author a workflow that writes to Dataverse and sends confirmation email](exercise-1-return-request-workflow.md) first. You should have the **Create return authorization** workflow published and attached to the agent, and at least one **Returns** row created through **Preview**.

> [!NOTE]
> Task 9 uses Copilot Credits.

## Task 1 — Create the workflow

Create a second Copilot Studio workflow with an agent-callable trigger and give it a clear display name and description. Where the first workflow authorized returns, this one evaluates the refund step — route it above the threshold or auto-approve it below.

1. In Copilot Studio's left navigation, select **Workflows**.

1. Select **New workflow** → **Workflow**.

1. Select the **Start** node and set **Trigger type** to **When an agent calls the workflow**. A **Respond to the agent** node is added automatically.

1. Rename the flow to:

    ```text
    Approve refund request
    ```

## Task 2 — Define the trigger inputs

Define the three trigger inputs the agent supplies at call time — the authorization number, the requested refund amount, and the supervisor to route above-threshold requests to.

1. On the **When an agent calls the workflow** node, add three inputs:

    | Name | Type | Description |
    |---|---|---|
    | `authorizationNumber` | Text | The RA-... authorization number from an existing Returns row. |
    | `requestedAmount` | Number | The refund amount the rep is requesting for this return. |
    | `supervisorEmail` | Text | Email address of the supervisor who should approve above-threshold requests. |

## Task 3 — Look up the Returns row

Before you update anything, look up the Returns row by authorization number so later steps have the row ID to update.

1. Add a **Microsoft Dataverse → List rows** action after the **When an agent calls the workflow** node.

1. On the **List rows** action, configure:

    | Field | Value |
    |---|---|
    | Table name | Returns |
    | Filter rows | `sup_authorizationnumber eq '` then dynamic content **authorizationNumber** then `'`. (Use the expression editor if the filter builder doesn't accept mixed content.) |
    | Row count | `1` |

## Task 4 — Add the threshold condition

Add a Condition action that branches on the requested refund amount. Refunds up to $100 auto-approve; above $100 route to a supervisor.

1. Add an **If/Else** condition action after **List rows**.

1. Configure the condition with **requestedAmount** (dynamic content) as the **Property**, **Greater** as the **Operator**, and `100` as the **Value**.

1. Two branches appear: **If** for refunds above $100 and **Else** for refunds of $100 or less.

## Task 5 — Above-threshold branch: request approval

Configure the above-threshold branch. When the requested amount exceeds the threshold, the flow sends the supervisor an approval request through Teams (with email fallback) and updates the Returns row based on the approval outcome.

In the **If** branch:

1. Add an **Approvals → Start and wait for an approval** action.

1. In the **Start and wait for an approval** action, add a connection, and then configure:

    | Field | Value |
    |---|---|
    | Approval type | **Approve/Reject — First to respond** |
    | Title | `Refund approval needed: ` + dynamic content **authorizationNumber** |
    | Assigned to | Dynamic content: **supervisorEmail** |
    | Details | Short summary referencing the authorization number and requested amount. |

    > [!NOTE]
    > Approvals uses Microsoft Teams by default and falls back to email when the assignee doesn't have Teams. If you don't have a second user available for testing, use your own email as the supervisor address and approve your own request.

1. After the approval action, add a **Switch** on the approval's **Outcome** output. In the **On** field, select **Outcome** from the Start and wait for an approval action, then configure:

    - **Approve** → **Update a row** (Microsoft Dataverse) with `Status = Approved`, `Expected refund = requestedAmount`, `Approved by = supervisorEmail`.
    - **Reject** → **Update a row** (Microsoft Dataverse) with `Status = Rejected`, `Expected refund = 0`.
    - **Default** (timeout or other outcome) → **Update a row** (Microsoft Dataverse) with `Status = Escalated`.

    In each case, use a **Microsoft Dataverse → Update a row** action targeting the **Returns** table, with **Row ID** set to the first row's ID from the **List rows** step.

## Task 6 — Below-threshold branch: auto-approve

Configure the below-threshold branch. When the requested amount is at or below the threshold, the flow auto-approves and updates the Returns row without human involvement.

In the **Else** branch:

1. Add a **Microsoft Dataverse → Update a row** action targeting the **Returns** table.

1. Configure:

    | Field | Value |
    |---|---|
    | Row ID | First row from **List rows**. |
    | Status | `Approved` |
    | Expected refund | Dynamic content: **requestedAmount**. |
    | Approved by | `auto-approved` (or leave blank — up to you). |

## Task 7 — Return outputs to the agent

Return typed outputs to the agent so it can summarize the outcome back to the rep. Every branch of the condition must feed one of these outputs so the agent always has an answer to report.

1. Delete the **Respond to the agent** action that was added automatically when you set the trigger.

1. At the end of each **Switch** case and at the end of the **Else** branch, add a **Respond to the agent** action.

1. In each **Respond to the agent** action, add the following outputs:

    | Name | Type | Value |
    |---|---|---|
    | `outcome` | Text | `Approved`, `Rejected`, or `Escalated` as a fixed value in each branch. |
    | `finalRefundAmount` | Number | `requestedAmount` on the approve and auto-approve branches, `0` on the reject and default branches. |
    | `notes` | Text | A short human-readable summary the agent can echo. |

    > [!NOTE]
    > In practice, you might place a separate **Respond to the agent** action at the end of each Switch case with different output values. That's supported — see [Modify an existing flow to use with an agent](https://learn.microsoft.com/microsoft-copilot-studio/flow-modify-use-with-agent) at `https://learn.microsoft.com/microsoft-copilot-studio/flow-modify-use-with-agent` — as long as every branch has one.

## Task 8 — Publish and attach to the agent

Save, publish, and attach the flow to the agent — the same publish-then-attach pattern as the previous exercise. This makes the approval flow available to the orchestrator at the next Preview turn.

1. **Save** the flow, then **Publish** it.

1. In **Agents**, open **Customer Support Rep Assistant**.

1. On the **Build** tab, select **+** next to **Tools** → **Workflows** → **Approve refund request**.

1. Select the workflow and confirm the input parameters are correct.

1. Add the following description:

    ```text
    Evaluates the requested refund amount for an authorized return, routes above-threshold requests to a supervisor for approval, and updates the Returns record with the outcome. Call this when the rep asks to process, approve, or resolve a refund.
    ```

1. Select **Save**.

## Task 9 — Test in Preview

Test both branches through the agent's **Preview** tab, starting with the above-threshold path so you can see the approval request arrive. A below-threshold spot-check afterward confirms the auto-approve branch fires too.

> [!NOTE]
> This task uses Copilot Credits.

> [!NOTE]
> When testing from the agent's **Preview** tab, Dataverse actions might return **401 Unauthorized**, or **Start and wait for an approval** might return **HTTP 504**. If either issue occurs, use **Dataverse MCP Server** to update the matching **Returns** row and configure the workflow as follows:
>
> ```text
> When an agent calls the workflow
> └── If requestedAmount is greater than 100
>     ├── If
>     │   ├── Create an approval
>     │   └── Respond to the agent: Pending
>     └── Else
>         └── Respond to the agent: Approved
> ```
>
> Both **Respond to the agent** actions must use the same output schema:
>
> - `outcome`: Text
> - `finalRefundAmount`: Number
> - `notes`: Text
>
> Update the agent instructions to call the workflow and then use **Dataverse MCP Server** to update the matching row to **Pending** for above-threshold requests or **Approved** for below-threshold requests.

1. Note the authorization number from an existing Returns row (the one you created in the previous exercise's Preview test).

1. In the **Preview** tab, send:

    ```text
    For return <paste RA number>, the customer is requesting a $250 refund. The supervisor is <your-tenant-email>. Please process the refund.
    ```

    Replace `<paste RA number>` with the actual authorization number from the **Returns** table and `<your-tenant-email>` with your tenant email address.

1. Confirm:
    - The above-threshold path fires.
    - You receive an approval request in Teams or by email at the supervisor address.
    - Approving updates the Returns row to `Approved` with `Expected refund = 250`.
    - The agent's reply summarizes the outcome.

1. (Optional) Repeat with `requestedAmount = 40` to confirm the below-threshold path auto-approves without an approval request.

You've added a second workflow with a threshold branch, an approval action, and outcome-aware Dataverse updates — all triggered from the same rep-side conversation. In the next exercise, you inspect the run history for both flows so you can answer "what did the agent do, and what happened next" from inside Copilot Studio.
