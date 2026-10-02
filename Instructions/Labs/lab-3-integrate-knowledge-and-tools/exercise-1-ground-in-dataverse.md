---
lab:
  title: '3.1: Connect an agent to Dataverse data'
  description: In this exercise, you add and configure Microsoft Dataverse tools that allow an agent to retrieve order and customer information. You then update the agent’s instructions and test the tools in Preview.
  duration: 20 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Connect an agent to Dataverse data

In this lab, you extend the **Customer Support Rep Assistant** — an agent for internal customer service representatives at a company that sells products to consumers — with Dataverse tools, a Model Context Protocol (MCP) tool, and a connector-as-tool. The starting agent has identity, instructions, skills, memory, safety, and two workflows already in place from the previous labs. By the end of this lab, you'll have grounded the agent in structured Dataverse data and connected two tool integrations that let it look up and act on external systems.

Skills describe *how* the agent should behave. **Knowledge** tells the agent *what is true right now*. In this exercise, you point the agent at two Dataverse tables so it can answer "who is this customer" and "what's the status of order 9876" without the rep having to paste details.

You will complete the following tasks:

- Add and configure a **Microsoft Dataverse** tool to retrieve order information from the **Orders** table.
- Add and configure a second **Microsoft Dataverse** tool to retrieve customer information from the **Customer Records** table.
- Update the agent's instructions to reinforce when each tool should be used.
- Test grounded responses through the agent's **Preview** tab.

This exercise should take approximately **20** minutes to complete.

## Before you start

This exercise assumes you have a **Customer Support Rep Assistant** agent with two Dataverse tables available: **Orders** (with a row for order 9876) and **Customer Records** (with a row for `priya@contoso.com`). To reach this starting state, complete Lab 2, including [Exercise 0 — Import the Dataverse tables and sample data](../lab-2-add-workflows-to-agent/exercise-0-setup.md).

> [!NOTE]
> Task 4 uses Copilot Credits.

## Task 1 — Add and configure the Orders tool

Add a Microsoft Dataverse tool that allows the **Customer Support Rep Assistant** to retrieve customer order information from the Orders table. The tool description helps the orchestrator determine when to use the tool, so clearly describe the available information and the questions the tool can answer.

1. Open the **Customer Support Rep Assistant** agent.

1. On the **Build** tab, in the components panel, select **+** next to **Tools**.

1. Select the **Connectors** tab, and then search for **Dataverse**.

1. Select the **List rows from selected environment** action.

1. Select **+ Add**.

1. In the **Tools** list, select **List rows from selected environment** to open its settings.

1. On the **Inputs** tab, configure the following inputs:

    | Input | Setting | Value |
    |---|---|---|
    | **Environment** | **Custom** | The environment that contains the **Orders** table |
    | **Table name** | **Custom** | The **Orders** table |

1. In **Description for AI**, enter:

    ```text
    Retrieves customer orders from the Orders table. Each row represents one order and includes the order ID, customer email, order total, and shipping status. Use this tool to look up order details when a customer support rep provides an order ID or customer email.
    ```

1. Select **Done**.

## Task 2 — Add and configure the Customer Records tool

Add a second Dataverse tool for the **Customer Records** table, using the same steps as Task 1. Two well-described tables give the agent enough structured data to answer most of the rep-facing scenarios in the rest of the labs.

1. Repeat the steps in Task 1, and select the **Customer Records** table instead of the **Orders** table.

1. In **Description for AI**, enter:

    ```text
    Retrieves customer profile records from the Customer Records table. Each row represents one customer and includes the preferred contact channel, loyalty tier, and support notes. Use this tool when a customer support rep asks about a customer's history or preferences.
    ```

1. Select **Done**.

## Task 3 — Reinforce grounding in the instructions

Add a bullet to the agent's instructions that reinforces the grounding behavior. Tool descriptions do most of the tool-selection work, but a single line under Scope tells the model that the rep expects answers that name their source.

1. On the **Build** tab, in the **Instructions** area, find the **Scope** section. Add a bullet:

    ```text
    - When the rep gives an order ID or customer email, look up context in Orders and Customer Records before responding. Name the field(s) you used (for example, "per Orders.status").
    ```

1. Select **Save**.

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
