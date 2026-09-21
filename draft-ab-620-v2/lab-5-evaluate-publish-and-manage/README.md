# Lab 5 — Evaluate, publish, and manage

**Course:** AB-620 v2 — Design and build integrated AI agent solutions in Microsoft Copilot Studio
**Harness:** GitHub Copilot (GHCP)
**Estimated duration:** 90–105 minutes

---

## Scenario recap

The Customer Support Rep Assistant is functionally complete: base agent, workflows, knowledge, tools, connected agents. Now you have to prove it works, get it into rep hands, keep an eye on it, and move it through environments the way any production Power Platform solution needs to move. This lab covers evaluation, publish/deploy, monitoring, and ALM.

**User of the agent:** internal customer service rep. **Beneficiary:** external customer.

---

## Lab objectives

By the end of this lab you can:

- Evaluate the agent against a curated test set and interpret results.
- Publish the agent and deploy to Microsoft Teams and Microsoft 365 Copilot.
- Monitor agent KPIs, Copilot Credits burn, and transcripts.
- Package the solution with environment variables and move it through a Power Platform Pipeline (dev → test → prod).
- Evaluate an alternative AI model candidate and validate post-migration behavior.

## OD subtask coverage (Lab 5)

- Evaluate agent behavior
- Publish an agent
- Deploy an agent to Microsoft Teams and to Microsoft 365 Copilot
- Manage security via security groups
- Monitor an agent
- Add existing agents to a solution
- Configure environment variables
- Use a Power Platform Pipeline to move a solution through dev → test → prod
- Evaluate AI model candidates

**OD adjustment:** "Create a solution" happened earlier in the course. This lab focuses on adding *existing* components to a solution and moving the solution across environments.

---

## Prerequisites

- Completed Lab 4, **or** import the Lab 5 starter (`assets/lab-5-starter-solution.md`).
- Microsoft Teams and Microsoft 365 Copilot licensing in the environment.
- An Entra security group to scope agent access. If unavailable, fallback to individual assignment.
- A test/prod environment reachable via a Power Platform Pipeline. Fallback: simulated export/import.
- All Dataverse tables from earlier work present in the environment (Returns, Orders, Customer Records, Products). Modular learners get these from the Lab 5 starter zip in Exercise 0. Fallback provisioning paths live in `../environment-setup/`. The evaluation CSV (`assets/eval-test-set.csv`) references order **9876**, customer **priya@contoso.com**, and SKU **BLD-100** — all defined in the shared seed data.

---

## Cost note — Copilot Credits consumption

| Step | What consumes credits | Profile |
|---|---|---|
| Exercise 0 — Set up starter state | None | Zero |
| Exercise 1 — Evaluation run (15 items) | Model call + tool calls per item, potentially per agent | **Very High** — flag |
| Exercise 2 — Publish + deploy smoke test | Model call | Moderate (2–3 prompts) |
| Exercise 3 — Monitoring | None (read-only) | Zero |
| Exercise 4 — Pipeline + model candidate re-evaluation | Second eval run | **Very High** — flag |

**Flags:**

- **Exercise 1** — 15 test items × orchestrator + specialist calls per item. This is the single most expensive activity in this lab. Do not re-run casually.
- **Exercise 4** — a second, smaller eval run to validate the model candidate. Use a **subset** of the test set (5–8 items) unless credits allow.

---

## Exercises

1. [Exercise 0 — Set up the Lab 5 starter state](exercise-0-setup.md)
1. [Exercise 1 — Evaluate an agent against a curated test set](exercise-1-evaluate.md)
1. [Exercise 2 — Publish and deploy an agent to end-user surfaces](exercise-2-publish-and-deploy.md)
1. [Exercise 3 — Monitor KPIs, credits, and transcripts for a published agent](exercise-3-monitor.md)
1. [Exercise 4 — Apply ALM — environment variables, pipelines, and evaluating a candidate model](exercise-4-alm-and-model-candidates.md)

---

## End-state check

- Evaluation results stored and reviewed.
- Agent published; Teams channel and M365 Copilot surface both work end-to-end.
- Security group governs access.
- Monitoring dashboards accessible; the learner can point at KPIs, credit burn, and transcripts.
- Solution exported and imported into a test environment via a Power Platform Pipeline (or simulated fallback).
- Environment variables are used for at least one endpoint reference (Work IQ MCP URL is a natural candidate).
- Model candidate evaluated on a subset; decision documented.

---

## Instructor notes

- **Fallback: Teams unavailable → M365 Copilot only.** If both unavailable → Preview canvas only.
- **Fallback: security group → individual assignment.**
- **Fallback: PP Pipeline → simulated export/import** (`.zip` file, manual import into a second environment).
- **Cost visibility.** This is the credit-heaviest lab. Consider running the second eval (final exercise) as a demonstration on a shared instructor account for ILT.

---

## Sources cross-checked

- Copilot Studio Deep Dive Deck — evaluation, publish, monitor.
- Microsoft Learn — Publish and deploy, Analytics, Solution export/import, Power Platform Pipelines.
- Deep Dive Deck — model catalog and model candidate evaluation.
