---
lab:
  title: '1.3: Create an agent and configure its identity, memory, and safety'
  description: In this exercise, you create a new agent on the GitHub Copilot harness, author natural-language instructions that define its role and tone, enable memory, confirm the default safety and access settings, and validate the behavior in Preview.
  duration: 30 minutes
  level: 200
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Create an agent and configure its identity, memory, and safety

**Instructions** act as the behavior contract that shapes every response for an agent. **Memory** carries user-specific context — a rep's preferences and cues from prior tickets — from one chat to the next. **Safety and access** settings determine who can use the agent.

In this exercise, you create the base Customer Support Rep Assistant agent inside the solution you built earlier, give it a name reps will recognize, author instructions that guide it to be patient, precise, in-scope, and clear about when to escalate, enable memory, confirm authenticated-access defaults, and validate the instructions with rep-framed prompts in Preview.

You will complete the following tasks:

- Create the agent and set its name.
- Author the natural-language instructions.
- Enable memory.
- Review safety and access settings.
- Validate the agent in Preview.

This exercise should take approximately **30** minutes to complete.

## Before you start

To complete this exercise, you need:

- To have completed [Exercise 2 — Create a solution to manage agent components](exercise-2-create-solution.md), with the **Customer Support Rep Assistant** solution created and set as your preferred solution.

> [!NOTE]
> Task 5 uses Copilot Credits.

## Task 1 — Create the agent and set its name

Create the agent shell inside your preferred solution and give it a display name reps will recognize. Skipping the conversational authoring option keeps setup predictable across every learner running this lab.

1. On the Copilot Studio **Home** page, select the **Agent (GitHub Copilot)** tile to create a new agent on the GitHub Copilot harness.

    > [!NOTE]
    > If Copilot Studio prompts you with **Describe your agent**, select **Skip to configure**. Manual configuration is predictable for lab purposes and avoids credit consumption during setup.

1. At the top of the **Build** tab, select the agent name field, which typically has a placeholder like `Untitled Agent` and replace the placeholder name with:

    ```text
    Customer Support Rep Assistant
    ```

    > [!NOTE]
    > The name auto-saves as you type. The field allows up to 42 characters and can't include angle brackets (`<` and `>`).

## Task 2 — Author the natural-language instructions

Author the instructions that define what the agent is. Instructions are the primary way to shape behavior on the GitHub Copilot harness — they set role, tone, scope, escalation, and safety guardrails, and stay stable as you add skills, tools, and workflows on top.

1. In the **Instructions** area on the **Build** tab, paste the following:

    ```text
    You are the Customer Support Rep Assistant. You help internal customer service representatives handle support tickets for customers. You do not talk to customers directly.

    Role and audience
    - Your user is always an internal customer service representative working an active support ticket.
    - You give the rep information, next steps, and packaged actions they can review and take on the customer's behalf.
    - Respond to the rep in second person ("the customer asked...", "you can offer..."). Never respond as if you were talking to the customer.

    Tone and response style
    - Be patient and precise. Prefer short, structured answers over long explanations.
    - Use numbered steps when you walk the rep through a diagnostic or action sequence.
    - When you cite a source, name it explicitly (for example, "per the FAQ" or "per the order record").
    - Do not invent order details, product specifications, return policies, or refund amounts. If you don't have the information, say so and describe what the rep can look up or ask.

    Scope
    - You assist only with the company's products, customer orders, and returns and refunds.
    - Politely decline out-of-scope requests (unrelated tech support, personal advice, non-company products) and offer the rep a suggested next step where you can.
    - Do not provide legal, financial, medical, or HR advice.

    Escalation and edge cases
    - If the case involves suspected fraud, threats, safety, or account compromise, do not attempt to resolve it. Recommend the rep escalate to a supervisor and summarize the case in 3–5 bullets the rep can hand off.
    - If the customer request is outside published policy (for example, a return outside the return window), do not promise an exception. Describe the standard policy and note that any exception is a supervisor decision.
    - If you cannot answer with available information, state that clearly and suggest one specific follow-up the rep can take.

    Security and compliance
    - Do not ask the rep for the customer's password, one-time passcode, full payment card number, or other sensitive credentials.
    - Do not repeat or store any sensitive credential the rep pastes into the conversation.
    ```

1. In the upper-right corner of the page, select the **Save** (disk) icon.

    > [!NOTE]
    > The Instructions section has its own **Save** control (separate from auto-save on the name field). If you leave the Build tab without saving, changes are lost.

## Task 3 — Enable memory

Enable memory so the agent remembers signals shared during a chat and applies them on later interactions. Memory is a single toggle in the components panel — the platform handles the storage and per-user scoping for you.

1. On the agent's **Build** tab, in the components panel, locate the **Memory** section.

1. Toggle **Memory** on.

1. Confirm the toggle stays on and no additional configuration surface appears — memory has no scope, allow-list, or maker-visible view of user memories in the current UI.

> [!NOTE]
> Memory takes effect across future sessions, so you won't see its effect in the Preview validation later in this exercise. Memory manifests as the agent recalling preferences or context the same rep shared in earlier sessions.

## Task 4 — Review safety and access settings

Confirm the agent's safety and access settings. Rep-facing internal agents live behind authenticated access — Copilot Studio configures this for you by default, so this task is a quick review rather than a configuration change.

1. On the agent's **Build** tab, open **Settings** from the ellipsis (**...**) menu.

1. Select the **Safety & access** tab.

1. Under **Authentication**, confirm the dropdown is set to **Authenticate with Microsoft**. This is the default and is what you want — internal reps should always sign in.

    > [!NOTE]
    > **No authentication** is available in the dropdown but isn't appropriate for an internal rep agent. Leave the default. The **Web channel security** setting isn't applicable until you publish the agent — you'll return to it in a later lab.

1. Close the Settings panel.

## Task 5 — Validate the agent in Preview

Run a short set of rep-framed prompts in Preview to validate that the identity and instructions are working together. Three prompts cover tone, scope refusal, and escalation — the behavior contract you just authored.

> [!NOTE]
> This task uses Copilot Credits.

1. Select the **Preview** tab.

    > [!NOTE]
    > The **End user preview** toggle at the top of the Preview tab is off by default. This allows you to view maker-only details while testing.

1. Test **tone and rep framing**. Enter the following prompt and select **Send**:

    ```text
    A customer just called saying their order hasn't arrived. I don't have the order number in front of me yet. What should I ask them first?
    ```

    Expected: the response speaks to **you as the rep**, suggests one focused question, doesn't invent an order status, and stays patient and precise.

1. Test **scope refusal**. Enter the following prompt and select **Send**:

    ```text
    Customer is asking about their tax return. Can you help?
    ```

    Expected: a polite refusal (out of scope per the instructions), with a suggested next step for the rep.

1. Test **escalation**. Enter the following prompt and select **Send**:

    ```text
    Customer says someone else placed the order using their card. What do I do?
    ```

    Expected: the agent does not attempt to resolve the case; it recommends escalation to a supervisor with a 3–5 bullet summary.

1. If any prompt doesn't behave as expected, revisit the instructions in Task 2 and refine.

You have now created the **Customer Support Rep Assistant** agent with identity, memory, and safety configured, and validated that the instructions shape every response the way you designed them. In the next exercise, you package reusable behavior as skills the orchestrator can invoke when a situation matches.
