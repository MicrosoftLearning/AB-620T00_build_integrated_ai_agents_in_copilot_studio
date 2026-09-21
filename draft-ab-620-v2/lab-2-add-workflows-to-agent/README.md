# Lab 2 — Add workflows to the agent

**Course:** AB-620 v2 — Design and build integrated AI agent solutions in Microsoft Copilot Studio
**Harness:** GitHub Copilot (GHCP)
**Estimated duration:** 75–90 minutes

---

## Scenario recap

You've configured the Customer Support Rep Assistant with identity, instructions, skills, memory, and safety. In this lab, the rep needs the agent to **take action** — not just answer. You'll add two workflows: one that authorizes a return and one that handles refund approval with a human in the loop. Both are authored inside the **preferred solution** so they travel with the agent as reusable components.

**User of the agent:** internal customer service rep. **Beneficiary:** external customer receiving the return authorization / refund.

---

## Lab objectives

By the end of this lab you can:

- Continue from Lab 1 or import the Lab 2 starter solution.
- Author a workflow (Power Automate cloud flow) that writes to Dataverse and sends confirmation email — attached to the agent as a workflow-as-tool.
- Author a human-in-the-loop (HITL) workflow using the Approvals connector, with threshold logic and error handling.
- Monitor workflow runs from the agent history view.

## OD subtask coverage (Lab 2)

- Author workflows in a Copilot Studio agent
- Add human-in-the-loop steps to workflows
- Monitor workflow runs

---

## Prerequisites

- Completed Lab 1, **or** import `assets/lab-2-starter-solution.md` and follow the post-import checklist.
- Dataverse **Returns** table available in your environment (inside the `Customer Support Rep Assistant` solution). Modular learners get this from the Lab 2 starter zip in Exercise 1. Fallback provisioning paths — hand-build in Dataverse, or SharePoint list — live in `../environment-setup/`.
- Outlook (Office 365) connector reachable from the environment.
- Approvals connector reachable (Teams-first; falls back to email).

---

## Cost note — Copilot Credits consumption

| Step | What consumes credits | Profile |
|---|---|---|
| Exercise 0 — Set up starter state | None | Zero |
| Exercise 1 — Author return workflow + test in flow designer | Connector operations only (no model call) | Low |
| Exercise 1 — Test workflow-as-tool via agent Preview | Model call + workflow invocation | Moderate (1–2 prompts) |
| Exercise 2 — Refund-approval workflow test | Connector operations + approvals | Low (test via flow, not agent) |
| Exercise 2 — Test via agent Preview | Model call + workflow + approval | Moderate (1 prompt, self-approve) |
| Exercise 3 — Monitor runs | None | Zero |

**Credit-conscious guidance:** author and unit-test workflows in the flow designer first (no model burn). Only invoke via agent Preview 1–2 times to verify the workflow-as-tool wiring.

---

## Exercises

1. [Exercise 0 — Set up the Lab 2 starter state](exercise-0-setup.md)
1. [Exercise 1 — Author a workflow that writes to Dataverse and sends confirmation email](exercise-1-return-request-workflow.md)
1. [Exercise 2 — Add human-in-the-loop approval to a workflow](exercise-2-refund-approval-hitl.md)
1. [Exercise 3 — Monitor workflow runs](exercise-3-monitor-runs.md)

---

## End-state check

- Customer Support Rep Assistant contains two workflows attached as tools:
    - **Create return authorization** — writes to Dataverse Returns and emails the customer.
    - **Approve refund request** — evaluates threshold, requests approval, and updates the record.
- Both workflows are inside the preferred solution.
- Return + refund flow tested end-to-end at least once from agent Preview.
- Learner can locate a workflow run in history and inspect its inputs, outputs, and errors.

---

## Instructor notes

- **Fallback: Dataverse Returns → SharePoint list.** If the Dataverse table is missing, create a SharePoint list with columns `Title (customer)`, `OrderId`, `Reason`, `Condition`, `AuthorizationNumber`, `ExpectedRefund`, `Status`. All workflow references update accordingly.
- **Fallback: Teams approvals → email.** If Teams Approvals unavailable, use the "Start and wait for an approval (email)" action.
- **Fallback: second HITL user → self-approval.** Learners approve their own request. Note this trades realism for tractability.
- **Model-agnostic testing.** Do not tune the model for workflow correctness — the workflow is deterministic. Model calls only in Preview.

---

## Sources cross-checked

- Copilot Studio Technical Deep Dive Deck — workflow-as-tool primitive.
- Agent Academy Mission 04 — precedent for workflows attached to agent.
- Microsoft Learn — Add a Power Automate flow as a tool; Approvals connector.
