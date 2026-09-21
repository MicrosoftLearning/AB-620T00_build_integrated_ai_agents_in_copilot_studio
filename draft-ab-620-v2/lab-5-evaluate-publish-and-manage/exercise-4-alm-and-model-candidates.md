---
lab:
  title: '5.4: Apply ALM — environment variables, pipelines, and evaluating a candidate model'
  description: In this exercise, you configure environment variables, promote the solution through a Power Platform Pipeline (dev → test → prod), and run a small candidate-model evaluation as part of ongoing ALM.
  duration: 35 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
    - Power Platform ALM
---

# Apply ALM — environment variables, pipelines, and evaluating a candidate model

This exercise closes the loop: package the solution for movement across environments using **environment variables**, promote it through a **Power Platform Pipeline** (dev → test → prod), then run a small candidate-model evaluation as part of ongoing ALM and validate that the promoted agent still behaves.

You will complete the following tasks:

- Add all existing components to the solution and confirm the inventory.
- Configure environment variables for the endpoints most likely to change across environments.
- Promote the solution through a Power Platform Pipeline to test, then prod.
- Evaluate a candidate model against a subset of the earlier test set.
- Validate post-migration behavior in the prod environment.

This exercise should take approximately **35** minutes to complete.

## Before you start

This exercise builds directly on the previous exercise in this lab. Complete [Exercise 3 — Monitor KPIs, credits, and transcripts for a published agent](exercise-3-monitor.md) first.

> [!NOTE]
> This exercise assumes three Dataverse-enabled Power Platform environments (dev, test, prod) are already provisioned for you, with **test** and **prod** enabled as Managed Environments, and a Power Platform Pipeline (dev → test → prod) already configured. Your dev environment is the one you've been working in for the previous labs.

> [!NOTE]
> Task 4 uses Copilot Credits — this is your second evaluation run in this lab. Keep the subset small.

## Task 1 — Add existing components to the solution

Add every remaining component to the preferred solution and confirm the inventory. A stray connector reference or workflow left outside the solution won't move with the pipeline in Task 3, so this audit is what makes the promotion clean.

1. Open the **Customer Support Rep Assistant** solution.

1. Confirm both agents (orchestrator + Fulfillment specialist), all three workflows (both return/refund on orchestrator + Create shipment request on Fulfillment), all skills, all tool registrations, and all three Dataverse knowledge tables are inside the solution.

1. On the main **Agent** component (and on each workflow), open the command menu (**⋮**), select **Advanced**, and then select **Add required objects**. This automatically pulls in every dependency the component references — connection references, environment variables, tables, and child solutions — so nothing gets left behind at export.

    > [!NOTE]
    > If anything was created outside the preferred solution (a stray connector reference, for example), you can also select **+ Add existing** on the solution's command bar to bring specific components in one at a time.

## Task 2 — Configure environment variables

Configure environment variables for the endpoints and identifiers most likely to change across environments. Environment variables let you promote the same solution to test and prod without hand-editing the workflow or MCP registration each time.

1. In the solution, select **+ New** → **More** → **Environment variable**.

1. Create at least two environment variables:

    | Display name | Data type | Used for |
    |---|---|---|
    | `Workplace search MCP URL` | Text | Endpoint URL used by the workplace-search MCP tool. |
    | `Default supervisor email` | Text | Default supervisor address used by the **Approve refund request** workflow. |

1. Provide a **current value** for each variable in the dev environment.

1. Update the workflow and MCP tool registration to reference the environment variables instead of hard-coded values.

1. Save the solution.

    > [!NOTE]
    > Environment variables are read-only inside Copilot Studio — makers change the values in Power Apps, and the agent must be **republished** any time a value changes for the change to take effect at runtime. Secret-type variables are the one exception (they're retrieved live).

## Task 3 — Promote through a Power Platform Pipeline

Promote the solution through a Power Platform Pipeline to test, and then prod. Providing environment-specific values for the environment variables at each stage is what keeps the promoted agent pointing at the right endpoints per environment.

1. Open **Power Platform Pipelines** from the maker portal.

1. Select or create a pipeline linking dev → test → prod.

1. Deploy the **Customer Support Rep Assistant** solution to **test**.

1. When prompted, provide **test-environment values** for the environment variables (a different MCP URL and supervisor email).

    > [!NOTE]
    > On import you're also prompted to map each **connection reference** (Outlook, Dataverse, the workplace-search MCP connector) to a real connection in the target environment — pick an existing one or create a new one on the spot. This is the most common ALM friction point, and it's expected.

1. After the deployment completes, open the agent in the test environment and send:

    ```text
    Give me a one-line status.
    ```

    Confirm the agent responds and the environment variables resolved correctly.

    > [!NOTE]
    > If a Power Platform Pipeline isn't available, export the solution as a managed `.zip`, import it into a second environment, and supply the environment variable values on import. Document the differences vs. a real pipeline.

1. Repeat the deploy for **prod**.

    > [!NOTE]
    > A handful of settings don't flow through the solution and need to be re-applied in each target environment after import: **channel deployments** (the Teams + Microsoft 365 Copilot deployment you did in Exercise 2), **sharing** (the security group), manual authentication settings, and Application Insights settings. Redo Exercise 2's publish + deploy steps in test and prod after the pipeline promotion completes.

## Task 4 — Evaluate a candidate model

Copilot Studio lets you route generative work through different models, and part of ALM is periodically re-evaluating whether your current default is still the best fit.

> [!NOTE]
> This task uses Copilot Credits.

1. In the test environment, on the orchestrator's **Build** tab, open the model selector and pick a **candidate** model — a different model than the one currently powering the agent.

1. Run a **subset** of the earlier evaluation (5–8 items from `assets/eval-test-set.csv`) against the candidate. Keep the subset small.

1. Compare results against the baseline in `assets/eval-notes.md`.

1. Decide: keep the original model, switch to the candidate, or defer. Document the decision and rationale.

    > [!NOTE]
    > If you decide to adopt the candidate model, change the model on the orchestrator in **dev** and re-promote through the pipeline. Model choice is an agent property that flows with the solution, so a model change made directly in test would be overwritten by the next dev → test deploy.

## Task 5 — Validate post-migration behavior

Validate the promoted agent in the prod environment with a small set of Preview prompts. Post-migration validation is a smaller, cheaper version of the full evaluation from Exercise 1 — enough to confirm the agent still behaves after the environment variables and model choice resolved to their prod values.

1. Return to the **prod** environment.

1. Ensure the model choice matches your decision from Task 4 (roll forward through the pipeline if adopting the candidate).

1. Send a short validation set from Preview or the Teams surface — 2–3 prompts covering a return authorization, a replacement-shipment delegation to Fulfillment, and a scope refusal.

    Confirm behavior matches expectations.

You've now completed the full lifecycle: designed, built, integrated knowledge and tools, added connected agents, evaluated, published, monitored, and promoted through environments — with ALM habits in place for ongoing operation.
