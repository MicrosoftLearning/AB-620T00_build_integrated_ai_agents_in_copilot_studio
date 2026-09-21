---
lab:
  title: '2.3: Monitor workflow runs'
  description: In this exercise, you inspect the run history for the two workflows you authored so you can trace exactly what the agent called, what inputs it passed, what the workflow did, and what came back.
  duration: 15 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Monitor workflow runs

Workflows fail. Connectors throttle. Approvals time out. You need to be able to answer *"what did the agent do, and what happened next"* — and you need to answer it from inside the maker experience, not from a separate portal. In this exercise, you inspect the run history for the two workflows you authored earlier in this lab.

You will complete the following tasks:

- Inspect a successful run of the return-authorization flow.
- Inspect a human-in-the-loop run of the refund-approval flow.
- Force a failure and read the error surface in the run history.

This exercise should take approximately **15** minutes to complete.

## Before you start

This exercise builds on the previous two exercises in this lab. Complete [Exercise 1](exercise-1-return-request-workflow.md) and [Exercise 2](exercise-2-refund-approval-hitl.md) first. You should have both **Create return authorization** and **Approve refund request** published, attached to the agent, and each with at least one successful run.

> [!NOTE]
> Task 3 uses Copilot Credits.

## Task 1 — Inspect a successful run of the return-authorization flow

Open the return-authorization flow's run history and walk through a successful run step by step. This is the full record Copilot Studio surfaces when things go right — the inputs the agent passed, the payload written to Dataverse, the email sent, and the outputs returned.

1. In the left navigation, select **Workflows**.

1. Open **Create return authorization**.

1. On the flow's page, select **Run history** (or the runs tab).

1. Open the most recent successful run.

1. Review each step:
    - **When an agent calls the flow** — the four input values the agent passed in.
    - **Add a new row** — the exact Dataverse payload written.
    - **Send an email** — the message body and recipient.
    - **Respond to the agent** — the outputs returned.

1. Confirm the same authorization number appears in the Dataverse row, the email subject, and the outputs.

## Task 2 — Inspect a HITL run of the refund-approval flow

Open the refund-approval flow's run history and inspect a human-in-the-loop run end-to-end. Approval outcomes surface as their own step, and the branch that fired downstream from the Switch is visible in the same view.

1. Open **Approve refund request** → **Run history**.

1. Open the above-threshold approval run from the previous exercise.

1. Review:
    - Which branch of the **Condition** fired.
    - The **Start and wait for an approval** step and its outcome.
    - Which **Update a row** action fired based on the outcome.
    - The **Respond to the agent** outputs.

## Task 3 — Force a failure and inspect

Deliberately trigger a failure and inspect what the run history surfaces. Seeing a failed run now is the fastest way to know what error information you'll have when a real failure lands in production.

> [!NOTE]
> This task uses Copilot Credits.

1. Return to the agent's **Preview** tab and send:

    ```text
    Please start a return for a customer. No order number yet.
    ```

1. Depending on how the orchestrator handles missing inputs, the agent either refuses to call the flow (because `orderId` is required) or calls it with a blank `orderId`.

1. Open **Create return authorization** → **Run history**.
    - If a new run appears and failed, open it. Identify which step failed and why. Read the error surface.
    - If no new run appears, the agent refused to call the flow. That's a valid outcome — it means the orchestrator respected the required-input contract.

1. Note which of the two you saw. Both are useful signals: agent-side input validation is generally more forgiving than flow-side; the run history is where you find out which one caught it.

You can now trace any run of either flow end-to-end. That's the observability foundation for evaluation and monitoring later.
