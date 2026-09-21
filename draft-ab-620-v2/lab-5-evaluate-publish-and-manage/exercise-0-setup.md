---
lab:
  title: '5.0: Set up the starter state'
  description: In this exercise, you download and import the starter solution and confirm the starting state before you begin evaluating, publishing, and managing the agent.
  duration: 10 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Set up the starter state

This setup exercise prepares you to complete the rest of the lab. If you completed [the previous lab](../lab-4-configure-multi-agent-solution/README.md) in the same Copilot Studio environment, you already have the required starting state — skip this exercise and start with [Exercise 1](exercise-1-evaluate.md).

You will complete the following tasks:

- Download the starter solution package.
- Import the starter solution into your environment.
- Confirm the starting state before you begin evaluation.

This exercise should take approximately **10** minutes to complete.

## Before you start

To complete this exercise, you need access to Microsoft Copilot Studio in your environment.

## Task 1 — Download the starter solution package

The starter package contains the full multi-agent stack from prior labs in a single unmanaged solution.

<!-- TODO: The relative link below points to the shared starter-solutions folder in this repository. Replace with the public download URL once the zip is externally hosted. -->

1. Download the starter package: [lab-5-starter.zip](../environment-setup/starter-solutions/lab-5-starter.zip).

1. Note where the file downloaded to — you'll browse to it in the next task.

## Task 2 — Import the starter solution

1. In [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com`, from the **...** menu next to your username, select **Solutions**.

1. Select **Import solution**.

1. Browse to the `lab-5-starter.zip` file you just downloaded and select **Open**.

1. Select **Next**, then select **Import**. Wait 2–3 minutes for the import to complete.

1. When the import finishes, open the imported solution and select **Publish all customizations**.

1. Back on **Solutions**, hover **Customer Support Rep Assistant**, select the ellipsis (**...**), then select **Set as preferred solution**.

## Task 3 — Confirm the starting state

Take a minute to confirm everything is in place. If something's missing, fix it before continuing so the evaluation runs against the intended state.

1. The preferred solution is **Customer Support Rep Assistant**.

1. The orchestrator has: instructions, both skills (`product-troubleshooting` — with the triage-outcomes enrichment — and `faq-lookup`), memory enabled, safety configured, both Dataverse knowledge sources (Orders + Customer Records), the Work IQ (preview) MCP tool, the Outlook connector-as-tool, both return/refund workflows attached (**Create return authorization**, **Approve refund request**), and one connected agent.

1. **Fulfillment agent** is present and connected, with **Products** knowledge and the **Create shipment request** workflow attached.

1. The two return/refund workflows are attached to the **orchestrator** — they're same-domain as customer support and never moved to the specialist. The Fulfillment agent's only workflow is **Create shipment request**.

You're ready to evaluate the solution.
