---
lab:
  title: '3.1: Ground an agent in Dataverse knowledge'
  description: In this exercise, you attach two Dataverse tables to an agent as knowledge sources and reinforce grounding in its instructions so it can answer entity-specific questions without the user pasting the details.
  duration: 20 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Ground an agent in Dataverse knowledge

In this lab, you extend the **Customer Support Rep Assistant** — an agent for internal customer service representatives at a company that sells products to consumers — with knowledge sources, a Model Context Protocol (MCP) tool, and a connector-as-tool. The starting agent has identity, instructions, skills, memory, safety, and two workflows already in place, either from prior work you may have completed in earlier labs, or from the starter solution you import in the setup exercise. By the end of this lab, you'll have grounded the agent in structured Dataverse data plus a policy knowledge source, and connected two tool integrations that let it look up and act on external systems.

Skills describe *how* the agent should behave. **Knowledge** tells the agent *what is true right now*. In this exercise, you point the agent at two Dataverse tables so it can answer "who is this customer" and "what's the status of order 9876" without the rep having to paste details.

You will complete the following tasks:

- Attach the **Orders** Dataverse table as a knowledge source with a description that guides retrieval.
- Attach the **Customer Records** Dataverse table as a knowledge source.
- Reinforce grounding in the agent's instructions.
- Test grounded responses through the agent's **Preview** tab.

This exercise should take approximately **20** minutes to complete.

## Before you start

This exercise assumes you have a **Customer Support Rep Assistant** agent with two Dataverse tables available: **Orders** (with a seed row for order 9876) and **Customer Records** (with a seed row for `priya@contoso.com`). You can reach this starting state by either:

- Completing [Lab 2](../lab-2-add-workflows-to-agent/README.md), OR
- Following the setup instructions in [Exercise 0 — Set up the Lab 3 starter state](exercise-0-setup.md).

> [!NOTE]
> Task 4 uses Copilot Credits.

> [!NOTE]
> If Dataverse tables aren't available in your environment, you can point at SharePoint lists instead — see `../environment-setup/sharepoint-fallback.md`. Field names differ.

## Task 1 — Add the Orders table as knowledge

Attach the **Orders** Dataverse table as a knowledge source. The description you provide is what the orchestrator reads to decide when a rep question warrants an Orders lookup, so make it explicit about which fields live in the table and what kinds of questions it can answer.

1. Open the **Customer Support Rep Assistant** agent.

1. On the **Build** tab, in the components panel, select **+** next to **Knowledge**.

1. Choose **Dataverse tables**.

1. Select **Orders**. Confirm the schema preview.

1. Add a description the orchestrator uses to decide when to look here:

    ```text
    Customer orders — one row per order. Includes order ID, customer email, order total, and shipping status. Use this to look up order details when a rep provides an order ID or customer email.
    ```

1. Select **Add**.

## Task 2 — Add Customer Records as knowledge

Attach **Customer Records** as a second knowledge source, using the same flow as Task 1. Two well-described tables give the agent enough grounded structured data to answer most of the rep-facing scenarios in the rest of the labs.

1. Repeat the same flow with the **Customer Records** table.

1. Description:

    ```text
    Customer profile records — one row per customer. Includes preferred contact channel, loyalty tier, and support notes. Use this when a rep asks about a customer's history or preferences.
    ```

1. Select **Add**.

## Task 3 — Reinforce grounding in the instructions

Add a bullet to the agent's instructions that reinforces the grounding behavior. Knowledge-source descriptions do most of the tool-selection work, but a single line under Scope tells the model the rep expects grounded citations that name the source.

1. On the **Build** tab, in the **Instructions** area, find the **Scope** section. Add a bullet:

    ```text
    - When the rep gives an order ID or customer email, look up context in Orders and Customer Records before responding. Name the field(s) you used (for example, "per Orders.status").
    ```

1. Select **Save** below the Instructions area.

## Task 4 — Test in Preview

Test grounded responses through the agent's **Preview** tab. Two prompts — one that hits Orders, one that hits Customer Records — are enough to confirm the agent grounds and cites correctly.

> [!NOTE]
> This task uses Copilot Credits.

1. Select the **Preview** tab.

1. Enter the following prompt and select **Send**:

    ```text
    What can you tell me about order 9876?
    ```

    Confirm the response grounds in the Orders table (references order total, status, or ship-to). If the order doesn't exist, the agent should say so — not fabricate values.

1. Enter the following prompt and select **Send**:

    ```text
    What's Priya's preferred contact channel?
    ```

    Confirm the response grounds in Customer Records and names the field.

You've now grounded the agent in two Dataverse tables and reinforced the citation habit in its instructions — so the agent can answer "who is this customer" or "what's the status of order 9876" without the rep pasting the details, and can name the field it drew from. In the next exercise, you add an MCP server as a tool so the agent can search workplace context beyond structured tables.
