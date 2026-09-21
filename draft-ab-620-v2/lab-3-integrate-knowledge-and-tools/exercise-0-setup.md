---
lab:
  title: '3.0: Set up the starter state'
  description: In this exercise, you download and import the starter solution and confirm the starting state before you begin adding knowledge sources and tools.
  duration: 10 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Set up the starter state

This setup exercise prepares you to complete the rest of the lab. If you completed [the previous lab](../lab-2-add-workflows-to-agent/README.md) in the same Copilot Studio environment, you already have the required starting state — skip this exercise and start with [Exercise 1](exercise-1-ground-in-dataverse.md).

You will complete the following tasks:

- Download the starter solution package.
- Import the starter solution into your environment.
- Confirm the starting state before you begin adding knowledge and tools.

This exercise should take approximately **10** minutes to complete.

## Before you start

To complete this exercise, you need access to Microsoft Copilot Studio in your environment.

> [!NOTE]
> If the starter package isn't available, you can hand-provision the **Orders** and **Customer Records** Dataverse tables using the instructions in [../environment-setup/dataverse-manual-provisioning.md](../environment-setup/dataverse-manual-provisioning.md), then import seed rows from `../sample-data/orders.csv` and `../sample-data/customer-records.csv`.

## Task 1 — Download the starter solution package

The starter package contains the agent from prior work (identity, instructions, skills, memory, safety), both workflows (**Create return authorization**, **Approve refund request**) attached to the agent, and the seeded **Orders**, **Customer Records**, and **Products** Dataverse tables plus the empty **Returns** table.

<!-- TODO: The relative link below points to the shared starter-solutions folder in this repository. Replace with the public download URL once the zip is externally hosted. -->

1. Download the starter package: [lab-3-starter.zip](../environment-setup/starter-solutions/lab-3-starter.zip).

1. Note where the file downloaded to — you'll browse to it in the next task.

## Task 2 — Import the starter solution

1. In [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com`, from the **...** menu next to your username, select **Solutions**.

1. Select **Import solution**.

1. Browse to the `lab-3-starter.zip` file you just downloaded and select **Open**.

1. Select **Next**, then select **Import**. Wait 2–3 minutes.

1. When the import finishes, open the imported solution and select **Publish all customizations**.

1. Back on **Solutions**, hover **Customer Support Rep Assistant**, select the ellipsis (**...**), then select **Set as preferred solution**.

## Task 3 — Confirm the starting state

Take a minute to confirm everything is in place. If something's missing, fix it before continuing so the knowledge sources and tools you add in later exercises build on the right foundation.

1. The preferred solution is **Customer Support Rep Assistant** (publisher **Support**, prefix **sup**).

1. The agent has instructions, both skills (`product-troubleshooting`, `faq-lookup`), and memory enabled.

1. Two workflows are in the solution: **Create return authorization** and **Approve refund request**. Both are attached to the agent as tools.

1. The **Returns** table exists with its columns (empty or with one test row from prior work).

1. The **Orders** table exists and contains order **9876** for `priya@contoso.com`.

1. The **Customer Records** table exists and contains a row for `priya@contoso.com` with preferred channel **Email** and loyalty tier **Gold**.

You're ready to add knowledge sources and tools.
