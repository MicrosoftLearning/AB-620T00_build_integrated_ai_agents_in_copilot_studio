---
lab:
  title: '1.1: Create a Power Platform environment'
  description: In this exercise, you create a Power Platform developer environment with Dataverse to host the Copilot Studio agent and every component you build in this lab.
  duration: 10 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft Power Platform
    - Microsoft Copilot Studio
---

# Create a Power Platform environment

Across this lab, you build the **Customer Support Rep Assistant** — an agent for internal customer service representatives at a company that sells products to consumers. Reps use the agent while working active support tickets to look up context, decide next steps, and package repeatable actions on the customer's behalf. By the end of this lab, you'll have a solution-packaged agent with clear identity and instructions, two reusable skills, and appropriate configurations.

A Power Platform **environment** is the top-level container that scopes users, data, and apps. Every Copilot Studio agent runs inside one, and every solution, Dataverse table, workflow, and connection lives inside that same boundary. Choosing the environment intentionally — and confirming Dataverse is available — sets the isolation and governance boundary for everything else in this lab.

In this exercise, you create a **Developer** environment with Dataverse in the Power Platform admin center and switch Copilot Studio into it, so the Customer Support Rep Assistant and its downstream components have a dedicated, isolated place to live.

You will complete the following tasks:

- Create a Developer environment with Dataverse.
- Confirm the environment is available in Copilot Studio.

This exercise should take approximately **10** minutes to complete.

## Before you start

To complete this exercise, you need:

- Access to Microsoft Copilot Studio in your environment.
- Permissions to create a Developer environment in the tenant.

## Task 1 — Create a Developer environment with Dataverse

Create the environment that will host your agent and its Dataverse data. Choosing **Developer** keeps this work isolated from any production or shared environments in the tenant, and adding Dataverse at creation time avoids a separate provisioning step later when workflows need tables to write to.

1. In a web browser, navigate to [Power Platform admin center](https://admin.powerplatform.microsoft.com/manage/environments) at `https://admin.powerplatform.microsoft.com/manage/environments` and sign in with your lab credentials.

1. If prompted to stay signed in, select **Yes**. Close any welcome or pop-up messages that appear.

1. On the **Environments** page, select **+ New** on the command bar.

1. In the **New environment** panel, enter the following values:

    | Field | Value |
    |---|---|
    | Name | *Your name* |
    | Type | **Developer** |
    | Macro Region Geography | **North America** (or the region closest to you) |
    | Add a Dataverse data store? | **Yes** |

    > [!NOTE]
    > The **Developer** environment type is free per user and is intended for individual maker work. It doesn't count against production capacity in the tenant.

1. Select **Next**. In the **Add Dataverse** step, enter the following values:

    | Field | Value |
    |---|---|
    | Language | **English (United States)** |
    | Currency | **USD ($)** |
    | Deploy sample apps and data | **No** |

1. Select **Save** and wait for the environment state to reach **Ready**. Use the **Refresh** button on the command bar to update the status.

    > [!NOTE]
    > Environment provisioning can take several minutes depending on tenant configuration. If provisioning fails with a capacity error, work with your tenant administrator to free capacity or use an existing Developer environment.

## Task 2 — Confirm the environment is available in Copilot Studio

Switch Copilot Studio into the environment you just created and confirm it's available for authoring. Copilot Studio defaults to the tenant's default environment, so this step ensures every component you build in the remaining exercises lands in your isolated environment — not the shared default.

1. In a new browser tab, navigate to [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/) at `https://copilotstudio.microsoft.com` and sign in with your lab credentials.

1. If prompted to stay signed in, select **Yes** to remain signed in.

1. If prompted with a **Get Started** screen, accept the default country or region and continue. Skip any welcome messages.

1. In the bottom-left corner, select the **environment selector** that displays the name of the currently selected environment, then select the environment you created in Task 1.

1. Confirm the environment name in the selector matches the environment you created. Every remaining exercise in this lab assumes you are working in this environment.

    > [!NOTE]
    > If Copilot Studio doesn't load in your new environment, capture your environment ID from the URL in the Power Platform admin center (a GUID such as `12345678-90ab-cdef-1234-567890abcdef`) and navigate directly to `https://copilotstudio.microsoft.com/environments/<your-environment-id>/home`.

You have now provisioned a Power Platform environment configured for Copilot Studio agent authoring — an isolated workspace where you'll build the **Customer Support Rep Assistant** agent and its supporting components throughout this lab. In the next exercise, you create the solution container that packages the agent and every component you add to it.
