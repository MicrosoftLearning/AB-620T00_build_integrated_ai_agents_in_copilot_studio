---
lab:
  title: '1.4: Build reusable behavior with skills'
  description: In this exercise, you author one skill from blank and upload a second from a provided SKILL.md file, then test both in Preview to confirm the orchestrator invokes each one when the situation matches.
  duration: 20 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Build reusable behavior with skills

Instructions define what the agent *is*. **Skills** define reusable *behavior* — a named, self-contained set of instructions the orchestrator can choose to apply when a situation matches. Attaching skills lets you keep the base instructions stable and package specific behaviors as independent components.

In this exercise, you extend the Customer Support Rep Assistant with two skills: one that walks reps through a triage tree for common product problems, and one that answers policy questions from a provided FAQ.

You will complete the following tasks:

- Author the `product-troubleshooting` skill from blank.
- Upload the `faq-lookup` skill from a provided `SKILL.md` file.
- Test both skills in Preview.

This exercise should take approximately **20** minutes to complete.

## Before you start

To complete this exercise, you need:

- To have completed [Exercise 3 — Create an agent and configure its identity](exercise-3-create-agent-and-identity.md), with the agent's instructions saved.
- The [faq-lookup.SKILL.md](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/raw/main/Instructions/Labs/lab-1-design-and-configure-agent/assets/faq-lookup.SKILL.md) file downloaded to your computer. If the file opens in the browser, press **Ctrl+S** to save it.

> [!NOTE]
> Task 3 uses Copilot Credits.

## Task 1 — Author the `product-troubleshooting` skill from blank

Author a skill directly in Copilot Studio to enable your agent to triage a customer's product problem into a category and next step.

1. On the agent's **Build** tab, in the components panel, select **Skills**.

1. In **Add skill**, select **Create from blank**.

1. Fill in the following values:

    | Field | Value |
    |---|---|
    | Name | `product-troubleshooting` |
    | Description | `Walks customer support reps through a short triage tree for common product problems (setup, connectivity, hardware, billing, account access) so they can narrow the customer's issue to a category and next step.` |

1. In the **Instructions** area for the skill, paste the following:

    ```markdown
    # Product troubleshooting

    Walks a customer support rep through a short triage tree for common product problems (setup, connectivity, hardware, billing, account access) so the rep can narrow the customer's issue to a category and one concrete next step.

    ## When to activate

    The rep says the customer is having a problem with a product and needs help identifying what's wrong.

    ## Behavior

    1. Ask the rep for the product category (device, subscription, or accessory) if you don't already know it.
    2. Ask one clarifying question at a time. Never ask more than one question in a turn.
    3. Narrow the problem to one of: setup, connectivity, hardware, billing, or account access.
    4. Recommend one concrete next step for the rep. For example, walk the customer through restarting the device, check the order record for a shipping delay, or escalate to hardware support if the device won't power on.
    5. If the triage points to something the rep needs external context for (an order lookup, a warranty check, an escalation), state that clearly and stop — do not fabricate the answer.

    ## Boundaries

    - Do not quote specific warranty periods, prices, or SLAs.
    - Do not resolve the ticket — this skill hands the rep to the next step, whatever it is.
    - Do not respond to the customer directly.
    - For general policy questions with no product problem behind them (return windows, warranty length, shipping timelines, order-status meanings), defer to the `faq-lookup` skill.
    ```

1. Select **Create**.

    The skill appears in the **Skills** section of the components panel.

## Task 2 — Upload the `faq-lookup` skill

Add the second skill by uploading a pre-built `SKILL.md` file. Uploading lets you reuse skills that have already been authored and version-controlled outside Copilot Studio.

1. Select the **Skills** section of the components panel. In the **Add skill** dialog, select **Upload a skill**.

1. Drag and drop the [`faq-lookup.SKILL.md`](assets/faq-lookup.SKILL.md) file you downloaded earlier onto the upload area (or select the area to browse to the file).

1. Confirm **faq-lookup** appears in the **Skills** list alongside **product-troubleshooting**.

1. Select **Save**.

## Task 3 — Test both skills in Preview

Validate that the orchestrator selects the right skill for each situation. Two rep-framed prompts — one policy-shaped, one troubleshooting-shaped — are enough to see each skill activate independently.

> [!NOTE]
> This task uses Copilot Credits.

1. Select the **Preview** tab.

1. Enter the following prompt and select **Send**:

    ```text
    Customer's asking how long they have to return an unopened item.
    ```

    Confirm the response comes from the **faq-lookup** skill (it references the Returns and refunds FAQ content or explicitly cites the skill).

1. Enter the following prompt and select **Send**:

    ```text
    Customer says their device won't turn on. Walk me through what to ask.
    ```

    Confirm the response comes from the **product-troubleshooting** skill — it asks one clarifying question and doesn't diagnose blind.

Each response should stay rep-framed and inside the skill's boundaries.

You have now extended the agent with two reusable skills — **product-troubleshooting** and **faq-lookup** — giving it structured behavior it can reach for whenever the rep's prompt matches either skill's purpose.
