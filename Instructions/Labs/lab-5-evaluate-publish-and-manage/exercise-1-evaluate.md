---
lab:
  title: '5.1: Evaluate an agent against a curated test set'
  description: In this exercise, you run a curated multi-item test set against an agent using Copilot Studio's built-in evaluation feature, interpret the aggregate score and per-item outcomes, and decide what to fix now versus later.
  duration: 30 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Evaluate an agent against a curated test set

In this lab, you evaluate, publish, and monitor the full multi-agent **Customer Support Rep Assistant** solution. The starting state is a complete multi-agent solution — an orchestrator plus one connected specialist agent (**Fulfillment agent**), with all skills, memory, safety, workflows, Dataverse tools, MCP and connector tools, and routing configured in the previous labs. By the end of this lab, you'll have evaluated the agent, published it to a rep-facing channel, and reviewed its usage in monitoring.

Evaluation is the moment you stop trusting vibes and start trusting numbers. In this exercise, you run a **curated 15-item test set** against the Customer Support Rep Assistant using Copilot Studio's built-in evaluation feature and interpret the results.

You will complete the following tasks:

- Review the curated 15-item test set.
- Configure and run the evaluation.
- Review the aggregate score and per-item outcomes.
- Optionally compare a candidate model in Preview.
- Decide what to fix now versus later.

This exercise should take approximately **30** minutes to complete.

## Before you start

This exercise assumes you have the full multi-agent **Customer Support Rep Assistant** solution in place: the orchestrator plus the connected Fulfillment specialist, both return and refund workflows attached to the orchestrator, the Fulfillment agent's Products tool and shipment workflow attached, and all skills and tools configured. To reach this starting state, complete [Lab 4](../lab-4-configure-multi-agent-solution/exercise-1-configure-fulfillment.md).

You also need the [eval-test-set.csv](assets/eval-test-set.csv) file. If it opens in the browser, press **Ctrl+S** to save it to your computer.

> [!NOTE]
> This exercise is the single most Copilot-Credits-intensive activity in this lab. Run the 15-item evaluation once. The optional candidate-model comparison uses six Preview prompts and consumes additional credits; for ILT delivery, consider a shared demonstration.

## Task 1 — Review the test set

Open the curated test set and review its coverage. A useful evaluation set spans the shapes of prompt the agent handles — knowledge questions, workflow-invoking requests, delegation-triggering requests, and scope refusals — so a single run tells you which parts of the design are working and which need tuning.

1. Open the provided [`assets/eval-test-set.csv`](assets/eval-test-set.csv) file. It contains 15 rows covering a range of sample conversations that test different aspects of the agent's functionality:

    - Product-info questions (answered by the orchestrator directly from its Orders and Customer Records tools).
    - Return authorizations (handled by the orchestrator via the **Create return authorization** workflow).
    - Refund approvals above and below the $100 threshold (HITL path).
    - Replacement-shipment requests (delegated to the Fulfillment agent → **Create shipment request** workflow).
    - Combined return + replacement flows (multi-step: orchestrator authorizes the return, then delegates the shipment to Fulfillment).
    - Scope refusals (out-of-scope prompts the agent should refuse).
    - Ambiguous prompts to stress-test the priority rules.

1. Confirm each row has: `conversationNumber`, `question`, `response`.

## Task 2 — Create an evaluation and configure a test set

Create the evaluation and upload the test set. Copilot Studio runs each conversation in the set against the agent using the default **General quality** method, which scores relevance and completeness with a built-in judge model.

1. Open the **Customer Support Rep Assistant** agent.

1. From the top navigation menu within the agent, select **Evaluate**.

1. Select **+ Create your first evaluation**. If an evaluation already exists, select **+ New evaluation** instead. You're presented with multiple options for providing conversations for testing. For this exercise, you'll upload the CSV file containing the provided sample conversations to test against.

1. Upload `assets/eval-test-set.csv`.

1. Name the evaluation `End-to-end evaluation`.

1. The **General quality** evaluation method is selected as the current default method. In this mode, the evaluation uses AI to evaluate the agent against foundational quality standards like relevance and completeness.

1. Under **User profile** select **Manage**.

1. Select the **Select or add an account** dropdown then select **Add an account**.

1. Sign in to the tenant account you're using to complete the lab. The evaluation will run the tests and invoke the agent using your account.

1. Select **Save** to save the profile and connection.

1. Select **Evaluate** to run the evaluation using the test set. The evaluation processes each conversation in the test set. This process might take several minutes.

## Task 3 — Review results

Review the results Copilot Studio surfaces once the run completes. The aggregate score gives you the headline; the per-item outcomes are where the interesting patterns live.

1. After the run completes, review the evaluation results. If you navigated away, return to **Evaluate** and select the completed run.

1. Review the aggregate score.

## (Optional) Task 4 — Compare a candidate model in Preview

Use a few read-only questions from the test set to see how a different model responds. This is a **qualitative comparison**, not a second scored evaluation or approval to change the published agent. Skip this task if your environment doesn't offer another model permitted by your organization's policies. For the available controls, see [Select a model for an agent](https://learn.microsoft.com/microsoft-copilot-studio/agents-experience/authoring-select-agent-model).

1. On the agent's **Build** tab, record the currently selected **Model** in `assets/eval-notes.md`. Don't change it yet.

1. In **Preview**, ask these questions from the test set in separate new conversations, and record the responses in your notes:

    - "What is the wattage of the blender on order 9876?"
    - "What's the best mutual fund for retirement?"
    - "What are our return shipping options?"

    These questions should retrieve information or test a scope refusal. Don't use return, refund, shipment, or email prompts for this comparison because those can change data or send messages.

1. Return to **Build**, select **Model**, choose a different model available to you, and select **Save**. If no suitable candidate is available, leave the original model selected and skip the rest of this task.

1. In **Preview**, ask the *same three questions* again in separate new conversations. Compare factual accuracy, source use, and scope refusal with the original responses. Check the activity trace for unexpected tool calls; if an action tool runs, stop and investigate before any further testing. Record the candidate model name, observations, and whether you would keep the baseline or investigate the candidate further in `assets/eval-notes.md`.

1. Before continuing, return to **Build**, restore the original model, and select **Save**. Confirm that the original model is selected so Exercise 2 doesn't publish your trial candidate.

> [!NOTE]
> Model availability depends on your environment and administrator settings. This small Preview check doesn't establish which model is safe to deploy; a production model change needs a repeatable evaluation and release review.

## (Optional) Task 5 — Improve the agents based on evaluation results

1. Drill into each item and look for opportunities to refine the agent based on evaluation results:

    - **Delegation to Fulfillment fired when it shouldn't have** — the delegation description is too broad. Tighten the wording of the **delegation description** on the Fulfillment connected-agent registration on the orchestrator. This is the description you write when you register the Fulfillment agent as a connected agent (see [Lab 4 Exercise 3 Task 1](../lab-4-configure-multi-agent-solution/exercise-3-connect-and-test.md) for the field's location).
    - **Delegation to Fulfillment failed to fire when it should have** — the routing signal is too weak. Tighten the delegation bullet in the orchestrator's **Instructions** under the Scope section. This is the bullet added to reinforce delegation in the orchestrator's own instructions (see [Lab 4 Exercise 3 Task 2](../lab-4-configure-multi-agent-solution/exercise-3-connect-and-test.md) for where this bullet lives).
    - **Workflow was called with malformed inputs** — tighten the workflow's tool description and the corresponding **Scope** bullet in the calling agent's instructions so the agent knows which fields to populate.
    - **Scope refusals fired when they shouldn't have — or didn't fire when they should** — revisit the orchestrator's **Boundaries** section and its safety configuration.

1. Log observations in `assets/eval-notes.md`. Keep your baseline and any optional candidate-model observations for future tuning.

1. Sort observations into:

    - **Fix now** — instruction wording, skill enrichment, or tool description tweaks. Cheap to fix, high impact.
    - **Fix later** — deeper architecture questions (should a specialist be split, should a new tool be added).

1. Apply the "Fix now" changes. Don't re-run the full evaluation — validate against 2–3 targeted Preview prompts instead.

You've run a multi-item evaluation against the full multi-agent solution, reviewed both the aggregate score and the per-item outcomes, and separated the cheap tuning changes from the deeper architecture questions. If you tried a candidate model, you've restored the original model before publishing. In the next exercise, you publish the agent and deploy it to end-user surfaces so reps can use it.
