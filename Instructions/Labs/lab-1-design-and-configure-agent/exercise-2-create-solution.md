---
lab:
  title: '1.2: Create a solution to manage agent components'
  description: In this exercise, you create a publisher and a solution to hold every component you build in this lab, then set the solution as your preferred solution.
  duration: 10 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft Power Platform
    - Microsoft Copilot Studio
---

# Create a solution to manage agent components

Every enterprise agent lives inside a **solution** — the container that packages the agent and its components together for application lifecycle management (ALM). The **publisher** attached to the solution stamps every component with a schema prefix that keeps assets uniquely identifiable.

In this exercise, you create a publisher and a solution for the Customer Support Rep Assistant, and set the solution as your preferred solution so components you build in the remaining exercises land inside it automatically.

You will complete the following tasks:

- Create the publisher and solution.
- Confirm the solution is set as preferred.

This exercise should take approximately **10** minutes to complete.

## Before you start

To complete this exercise, you need:

- To have completed [Exercise 1 — Create a Power Platform environment](exercise-1-create-environment.md) and to be able to access Copilot Studio in the environment you created.

## Task 1 — Create the publisher and solution

Create the solution together with its publisher and mark it as preferred. Creating the publisher inline (rather than reusing the default) lets you set the custom prefix — `sup` in this case — that stamps every component's schema name.

1. In Copilot Studio, in the left navigation, select the ellipsis (**...**) next to your account name and then select **Solutions**.

    > [!NOTE]
    > Selecting **Solutions** currently reroutes you to the Classic experience of Power Apps. When that happens, select the ellipsis (**...**) next to your account name again and select **Solutions** a second time to land on the Solutions list. Continue from there.

1. On the command bar, select **+ New solution**.

1. In the **New solution** panel, in the **Display name** field, enter `Customer Support Rep Assistant`. The **Name** field auto-populates — leave it as-is.

1. Next to the **Publisher** field, select **+ New publisher**.

1. In the **New publisher** panel, enter the following values:

    | Field | Value |
    |---|---|
    | Display name | `Support` |
    | Name | `Support` |
    | Prefix | `sup` |
    | Choice value prefix | Accept the default |

1. Select **Save**. You return to the **New solution** panel with **Support** selected as the publisher.

1. Leave **Version** at the default (`1.0.0.0`).

1. Select the **Set as your preferred solution** checkbox at the bottom of the panel.

1. Select **Create**.

## Task 2 — Confirm the solution is set as preferred

Verify the **Preferred** indicator on the solution row. Confirming this now prevents a common issue later, where components silently land in the wrong solution because the preferred flag was missed.

1. Select **Back to solutions** (←) to return to the **Solutions** list. In the **Solutions** list, locate **Customer Support Rep Assistant**. A **Preferred solution** label (or filled star) appears in the row.

   > [!NOTE]
   > Only one solution can be preferred at a time. If the indicator isn't visible, hover over the row, select the ellipsis (**...**), then select **Set preferred solution** and confirm.

You have now created the **Support** publisher with the `sup` prefix, packaged the **Customer Support Rep Assistant** solution around it, and set it as your preferred solution — so every component you author from here on lands in the right container with a consistent schema-name prefix. In the next exercise, you create the agent itself inside this solution and configure its identity, memory, and safety.
