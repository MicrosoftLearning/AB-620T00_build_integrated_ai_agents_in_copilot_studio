---
lab:
  title: '4.0: Set up the starter state'
  description: In this exercise, you download and import the starter solution and confirm the starting state before you begin creating the Fulfillment specialist agent.
  duration: 10 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Set up the starter state

This setup exercise prepares you to complete the rest of the lab. If you completed [the previous lab](../lab-3-integrate-knowledge-and-tools/README.md) in the same Copilot Studio environment, you already have the required starting state — skip this exercise and start with [Exercise 1](exercise-1-configure-fulfillment.md).

You will complete the following tasks:

- Download the starter solution package.
- Import the starter solution into your environment.
- Confirm the starting state before you begin creating the Fulfillment specialist.

This exercise should take approximately **10** minutes to complete.

## Before you start

To complete this exercise, you need access to Microsoft Copilot Studio in your environment.

> [!NOTE]
> If the starter package isn't available, you can hand-provision the **Products** Dataverse table using [../environment-setup/dataverse-manual-provisioning.md](../environment-setup/dataverse-manual-provisioning.md) and import seed rows from `../sample-data/products.csv`.

## Task 1 — Download the starter solution package

The starter package contains the orchestrator from prior labs plus the seeded **Products** Dataverse table.

<!-- TODO: The relative link below points to the shared starter-solutions folder in this repository. Replace with the public download URL once the zip is externally hosted. -->

1. Download the starter package: [lab-4-starter.zip](../environment-setup/starter-solutions/lab-4-starter.zip).

1. Note where the file downloaded to — you'll browse to it in the next task.

## Task 2 — Import the starter solution

1. In [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com`, from the **...** menu next to your username, select **Solutions**.

1. Select **Import solution**.

1. Browse to the `lab-4-starter.zip` file you just downloaded and select **Open**.

1. Select **Next**, then select **Import**. Wait 2–3 minutes.

1. When the import finishes, open the imported solution and select **Publish all customizations**.

1. Back on **Solutions**, hover **Customer Support Rep Assistant**, select the ellipsis (**...**), then select **Set as preferred solution**.

## Task 3 — Confirm the starting state

Take a minute to confirm everything is in place. If something's missing, fix it before continuing so the Fulfillment specialist you create in the next exercise builds on the right foundation.

1. The preferred solution is **Customer Support Rep Assistant**.

1. The orchestrator has: instructions, both skills (`product-troubleshooting`, `faq-lookup`), memory enabled, safety configured, both workflows attached as tools (**Create return authorization**, **Approve refund request**), both Dataverse knowledge sources (**Orders** + **Customer Records**), the **Work IQ (preview)** MCP tool, and the Outlook connector-as-tool (**Send ad-hoc email**).

1. The **Products** table exists and contains SKU **BLD-100** (Countertop Blender, 700W, 24-month warranty).

You're ready to create and configure the Fulfillment specialist.
