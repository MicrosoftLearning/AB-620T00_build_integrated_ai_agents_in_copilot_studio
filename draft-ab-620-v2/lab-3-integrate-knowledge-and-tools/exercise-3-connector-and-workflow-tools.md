---
lab:
  title: '3.3: Add a connector operation as an agent tool and audit the tool inventory'
  description: In this exercise, you attach a connector operation as a first-class agent tool, audit the full tool inventory now attached to the agent, and run a multi-tool Preview test that exercises knowledge, workflows, an MCP server, and a connector in a single turn.
  duration: 25 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Add a connector operation as an agent tool and audit the tool inventory

The rep also needs to send ad-hoc email — not just the templated confirmation baked into the return-authorization flow. A **connector-as-tool** exposes a single connector operation as a first-class tool the agent can pick. In this exercise, you attach the Outlook **Send an email** operation, then step back and audit the full tool inventory — the return and refund flows, the MCP tool from the prior exercise, and the connector-as-tool from this exercise — with a multi-tool test.

You will complete the following tasks:

- Attach the Outlook **Send an email** connector operation as a tool.
- Audit the tool inventory and tune the workflow tool descriptions so the agent can distinguish them from the new connector tool.
- Run a multi-tool test through the agent's **Preview** tab.

This exercise should take approximately **25** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 2 — Add an MCP server as an agent tool](exercise-2-mcp-as-tool.md) first. The **Work IQ (preview)** MCP tool should be registered and tested, with both seed calendar events (`Returns policy update` and `BLD-100 motor defect confirmed`) on the tenant account's calendar.

> [!NOTE]
> Task 3 uses Copilot Credits. A single multi-tool prompt can trigger 3–4 tool invocations. Run once.

## Task 1 — Add the Outlook connector action as a tool

Attach the Outlook **Send an email** operation as a connector-based tool. A connector-as-tool exposes a single, well-defined operation with a description you control — which is exactly what you want for a lower-frequency, agent-picked action like ad-hoc email that shouldn't overlap with the templated confirmation from the return workflow.

1. On the agent's **Build** tab, in the components panel, select **+** next to **Tools**.

1. Select **Connector**.

1. Find and select **Office 365 Outlook**, then choose the **Send an email** action.

    > [!NOTE]
    > In some regions the action label reads **Send an email (V2)**; both invoke the same underlying operation.

1. Configure:

    | Field | Value |
    |---|---|
    | Tool name | `Send ad-hoc email` |
    | Description | `Send a one-off email to a customer. Use only when the request is outside the scope of a return or refund workflow — do not use to send return confirmations (those are sent by Create return authorization).` |

1. Select **Save** below the tool configuration.

## Task 2 — Audit the tool inventory

Audit the four tools and two skills now attached to the agent. Reviewing the full inventory as one picture — especially how the workflow descriptions differ from the connector tool's description you just added — is what gives the orchestrator clean signals to pick the right tool for each shape of rep request in Task 3.

1. In the **Tools** section of the components panel, confirm the tool inventory:

    - **Create return authorization** (workflow-as-tool)
    - **Approve refund request** (workflow-as-tool)
    - **Work IQ (preview)** (MCP-as-tool)
    - **Send ad-hoc email** (connector-as-tool)

1. In the **Skills** section, confirm both skills from the first lab still appear: **product-troubleshooting** and **faq-lookup**.

1. Open **Create return authorization** and **Approve refund request** in turn and reread the description on each. Notice how each names a specific structured outcome (write a return record and send a confirmation email; evaluate a refund threshold and route for approval) — that specificity is what tells the orchestrator to pick the workflow instead of the more open-ended **Send ad-hoc email** tool when the rep request matches the workflow's shape.

## Task 3 — Multi-tool test in Preview

Run a single multi-tool prompt end-to-end. One well-shaped prompt is enough to exercise the full inventory — knowledge lookup, MCP retrieval, workflow invocation, and connector operation — in the same conversational turn.

> [!NOTE]
> This task uses Copilot Credits. A single prompt may trigger 3–4 tool calls.

1. Select the **Preview** tab.

1. Enter the following prompt and select **Send**:

    ```text
    Priya on order 9876 wants to return a defective BLD-100 blender — she says the motor keeps shutting off. Check my calendar for anything relevant to this SKU or our refund policy, then create the return authorization and email her a summary at <your-tenant-email>.
    ```

    Replace `<your-tenant-email>` with the email address of the tenant account you're using for the lab.

1. Expected behavior:
    - Agent looks up order 9876 in **Orders** knowledge.
    - Agent calls **Work IQ (preview)** to retrieve the `BLD-100 motor defect confirmed` calendar event, and either in the same turn or a follow-up turn, the `Returns policy update` event.
    - Agent calls **Create return authorization** to authorize the return.
    - Agent calls **Send ad-hoc email** to send Priya the summary.

    If the agent skips **Send ad-hoc email** and relies on the workflow's built-in confirmation email, note the deviation — a later exercise revisits the `product-troubleshooting` skill to give the agent stronger tool-selection guidance. If the agent skips the **Work IQ** check, tighten the server-level description you set in Ex 2 Task 3 so it more directly matches prompts about product defects and policy dates.

You've now built the full tool inventory for the rep-facing agent — two workflow-as-tools, one MCP-as-tool over workplace context, and one connector-as-tool for ad-hoc email — and confirmed the orchestrator can pick the right tool for each shape of rep request.
