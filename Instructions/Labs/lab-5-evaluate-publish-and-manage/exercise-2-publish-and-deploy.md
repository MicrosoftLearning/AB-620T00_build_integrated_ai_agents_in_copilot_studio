---
lab:
  title: '5.2: Publish and deploy an agent to end-user surfaces'
  description: In this exercise, you publish the agent and deploy it to Microsoft Teams and Microsoft 365 Copilot.
  duration: 25 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Publish and deploy an agent to end-user surfaces

The agent is validated. Now get it into rep hands. In this exercise, you publish the agent and deploy it to Microsoft Teams and Microsoft 365 Copilot.

You will complete the following tasks:

- Publish the agent.
- Deploy the agent to Microsoft Teams and Microsoft 365 Copilot.
- Test the agent in Microsoft Teams.

This exercise should take approximately **25** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 1 — Evaluate an agent against a curated test set](exercise-1-evaluate.md) first. You should have your evaluation observations reviewed and any "fix now" changes applied.

> [!NOTE]
> Tasks 2 and 3 use Copilot Credits.

## Task 1 — Publish the agent

Publish the agent so its current configuration is live. Publish is a separate step from any channel deployment — you publish once, then deploy the published version to any surface you want.

1. Open the **Customer Support Rep Assistant** agent.

1. Select **Publish** in the top-right corner.

1. Select **Publish agent**.

1. Confirm and wait for publish to complete.

## Task 2 — Deploy to Microsoft Teams and Microsoft 365 Copilot

Add Microsoft Teams and Microsoft 365 Copilot as channels. Both surfaces are handled through the same combined **Teams + Microsoft 365** channel in Copilot Studio, so you deploy once and reach both.

1. On the publish confirmation popup, select **Add channels**. Alternatively, from the agent, select **Channels** from the components panel.

1. Select **Teams + Microsoft 365**.

1. Select **Microsoft 365 Copilot and Microsoft Teams**.

1. Select **Add channel**. Wait for publish to complete.

## Task 3 — Test the agent in Microsoft Teams

Test the deployed agent from inside Microsoft Teams. Opening the agent in the Teams web app is the fastest way to verify the deploy landed correctly and the rep-facing experience works end-to-end.

1. From the agent publishing confirmation popup, select **View in Teams + Microsoft 365**. If your browser prompts you to open the Microsoft Teams, select **Cancel**. On the Microsoft Teams page, select **Use the web app instead**, and then close any welcome messages.

    > [!NOTE]
    > The agent might open directly in Microsoft Copilot instead of the Teams web app. If this happens, in Copilot Studio, select **Teams + Microsoft 365** under **Channels**. Then, on the **Use and share** tab, select **View in Teams**.

1. A **Customer Support Rep Assistant** popup window is displayed. Select **Add** to install the agent in Teams.

1. From the install confirmation window, select **Open** twice to open the agent in Teams. A chat thread with the Customer Support Rep Assistant opens in Teams.

1. Enter and send the following prompt:

    ```text
    Give me a status recap for order 9876.
    ```

    Confirm the agent responds correctly in Teams.

You've published the agent and made it available to reps in Microsoft Teams and Microsoft 365 Copilot, and confirmed the deployed version responds correctly. In the next exercise, you tour the monitoring surfaces Copilot Studio provides for tracking how a published agent is being used.
