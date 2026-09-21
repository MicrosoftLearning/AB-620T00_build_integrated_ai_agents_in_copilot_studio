---
lab:
  title: '4.1: Create and configure a specialist agent'
  description: In this exercise, you create a new agent inside an existing Copilot Studio solution, write scoped instructions that keep it focused on a single job, and attach a Dataverse table as a knowledge source.
  duration: 20 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Create and configure the Fulfillment specialist agent

In this lab, you extend the **Customer Support Rep Assistant** — an agent for internal customer service representatives at a company that sells products to consumers — into a multi-agent solution by adding a **Fulfillment agent** as a connected specialist. Fulfillment owns warehouse operations — SKU inventory checks and replacement shipments — which is a legitimately different domain than customer support. The starting orchestrator has identity, instructions, skills, memory, safety, workflows, knowledge sources, an MCP tool, and a connector-as-tool already configured. In this lab, you create the Fulfillment agent from scratch, configure it as a specialist, author a shipment-request workflow, and connect Fulfillment to the orchestrator with clear routing.

In this exercise, you create the Fulfillment agent and configure it as a real specialist by giving it a scoped persona in its instructions and grounding it in the Products catalog. In the exercises that follow, you'll extend the agent and connect it to the orchestrator agent.

You will complete the following tasks:

- Create the Fulfillment agent inside the **Customer Support Rep Assistant** solution.
- Write scoped instructions that keep the agent focused within its warehouse-operations role.
- Attach the Products Dataverse table as knowledge with a description that guides retrieval.

This exercise should take approximately **20** minutes to complete.

## Before you start

This exercise assumes you have the **Customer Support Rep Assistant** orchestrator in your preferred solution, plus the seeded **Products** Dataverse table. You can reach this starting state by either:

- Completing [Lab 3](../lab-3-integrate-knowledge-and-tools/README.md), OR
- Following the setup instructions in [Exercise 0 — Set up the Lab 4 starter state](exercise-0-setup.md).

## Task 1 — Create the Fulfillment agent

Create the Fulfillment agent inside the **Customer Support Rep Assistant** solution. Keeping it in the same solution as the orchestrator matters because the connected-agent registration you'll perform in a later exercise requires both agents to live together.

1. In [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com`, confirm your preferred solution is **Customer Support Rep Assistant**.

1. In the left navigation, select **Agents**, then select **+ New agent**.

1. Select **Skip to configure** so you can set the values directly instead of walking through the guided setup.

1. Give the agent these values:

    - **Name:** `Fulfillment agent`

## Task 2 — Write scoped instructions

Provide the Fulfillment agent with clear instructions. Because this agent may be invoked in other ways beyond the Customer Support Rep agent, you'll use caller-agnostic instructions.

1. On the **Build** tab, in the **Instructions** area, paste:

    ```text
    ## Role
    You are the fulfillment specialist for the warehouse operations team. You answer SKU-level inventory questions and schedule replacement shipments when asked to.

    ## Scope
    - Answer inventory and serial-prefix questions grounded in the Products catalog.
    - Schedule replacement shipments through the Create shipment request tool when the caller requests a replacement.
    - Always name the SKU, quantity, and expected shipment ETA in your reply.

    ## Boundaries
    - Do not authorize returns, approve refunds, or send customer-facing email — those are out of scope for this specialist.
    - Do not answer general product-spec questions unless they're tied to an inventory or shipment request.
    - Do not respond to end customers directly — return your structured answer to the caller (which may be another agent or an internal user).
    ```

1. Select **Save** below the Instructions area.

## Task 3 — Attach the Products table as knowledge

Add the Products table as a knowledge source. This enables the Fulfillment agent to know what SKUs exist, their serial-prefix ranges, and inventory hints.

1. On the **Build** tab, in the components panel, select **+** next to **Knowledge**.

1. Choose **Dataverse tables**.

1. Select **Products**. Confirm the schema preview shows columns for SKU, product name, wattage, warranty length, and current serial prefix.

1. Add a description the specialist reads to decide when to use this knowledge source:

    ```text
    Product catalog — one row per SKU. Includes SKU, product name, specs, warranty length, and the current serial-prefix range shipping from the warehouse. Use this to look up SKU details when the rep names a product, and to identify a newer serial-prefix batch when asked to ship a replacement for a defective unit.
    ```

1. Select **Add**.

You've configured the Fulfillment agent's identity and grounding. It knows what it's for, what it isn't for, and where its facts live. In the next exercise, you give it the tool it needs to actually schedule a shipment.
