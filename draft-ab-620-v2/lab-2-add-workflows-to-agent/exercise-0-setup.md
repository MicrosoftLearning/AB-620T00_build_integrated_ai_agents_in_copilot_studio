---
lab:
  title: '2.0: Set up the starter state'
  description: In this exercise, you download and import a small set of Dataverse tables that the rest of the labs need, and confirm the starting state before authoring workflows.
  duration: 10 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Set up the starter state

In this setup exercise, you download and import a small set of Dataverse tables that the rest of the labs depend on but the labs don't teach you to build by hand — the **Returns**, **Orders**, **Customer Records**, and **Products** tables (seeded with the storyline data). This is the only starter import you'll need for the remaining labs. Every learner completes this setup regardless of whether they completed the prior lab in the same environment.

You will complete the following tasks:

- Download the components you need.
- Import the components you need.
- Confirm the starting state before you begin authoring workflows.

This exercise should take approximately **10** minutes to complete.

## Before you start

To complete this exercise, you need access to Microsoft Copilot Studio in your environment. Either the Lab 1 ending state — a **Customer Support Rep Assistant** solution containing the agent — from completing [the prior lab](../lab-1-design-and-configure-agent/README.md) in the same environment, or, if you're starting fresh at Lab 2, no prior state (Task 1 sets up everything for you).

> [!NOTE]
> If neither starter package is available, you can hand-provision the required Dataverse tables using the instructions in [../environment-setup/dataverse-manual-provisioning.md](../environment-setup/dataverse-manual-provisioning.md). This is a documented fallback — not the intended path.

## Task 1 — Download the components you need

Which package you download depends on how you got here. Both paths land in the same state and set you up for every remaining lab.

<!-- TODO: The relative links below point to the shared starter-solutions folder in this repository. Replace with the public download URLs once the zips are externally hosted. -->

**If you completed the prior lab in this same environment,** download [additive-components.zip](../environment-setup/starter-solutions/additive-components.zip). This package adds only the Dataverse tables the remaining labs need — it doesn't touch the agent you already built. It's a one-time import that covers every remaining lab.

**If you're starting fresh here** (you didn't complete the prior lab in this environment, or you're recovering from a broken state), download [lab-2-starter.zip](../environment-setup/starter-solutions/lab-2-starter.zip). This package contains the prior lab's ending state — publisher `Support` (prefix `sup`), the `Customer Support Rep Assistant` solution, and the agent — plus all of the Dataverse tables the remaining labs need.

Download only one package. Note where the file downloaded to — you'll browse to it in the next task.

## Task 2 — Import the components you need

1. In [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com`, from the **...** menu next to your username, select **Solutions**.

1. On the command bar, select **Import solution**.

1. Browse to the package you just downloaded (`additive-components.zip` or `lab-2-starter.zip`) and select **Open**.

1. Select **Next**, then select **Import**. Wait 2–3 minutes for the import to complete.

1. When the import finishes, open the **Customer Support Rep Assistant** solution.

1. On the command bar, select **Publish all customizations**.

1. Back on **Solutions**, hover the **Customer Support Rep Assistant** row, select the ellipsis (**...**), then select **Set as preferred solution**.

## Task 3 — Confirm the starting state

Take a minute to confirm everything is in place. If something's missing, fix it before continuing so the workflow you author has something to write to.

1. In **Solutions**, open **Customer Support Rep Assistant**. Confirm the publisher is **Support** and the prefix is **sup**.

1. Open the **Customer Support Rep Assistant** agent from inside the solution.

1. On the **Build** tab, confirm instructions are populated and the **Skills** section lists `product-troubleshooting` and `faq-lookup`. Confirm **Memory** is toggled on.

1. Back in the solution, confirm the **Returns** table exists. Open it and confirm the primary column is **Authorization number** plus seven other columns: **Customer email**, **Order ID**, **Reason**, **Condition**, **Expected refund**, **Status**, and **Approved by**. The table is empty — the first workflow you author writes the first row.

1. Confirm the other three Dataverse tables exist in the solution: **Orders** (contains order **9876** for `priya@contoso.com`), **Customer Records** (contains a row for `priya@contoso.com`), and **Products** (contains SKU **BLD-100**). Later labs use these; you don't touch them in this lab.

> [!NOTE]
> The first time you use a connector — **Office 365 Outlook**, **Microsoft Dataverse**, or **Approvals** — in this environment, Power Automate prompts you to sign in and asks for an optional **display name** for the connection. Leave the display name blank (Power Automate auto-names it) or enter something short like `Outlook - AB-620`. This is a one-time prompt per connector per user in a given environment. If it doesn't appear when you configure an action later in this lab, it's because the connection already exists — that's fine.

You're ready to author the return-request workflow.
