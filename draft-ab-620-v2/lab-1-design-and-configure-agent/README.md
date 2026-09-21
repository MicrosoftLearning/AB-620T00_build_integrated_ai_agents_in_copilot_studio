# Lab 1 — Design and configure an agent for the GitHub Copilot harness

**Course:** AB-620 v2 — Design and build integrated AI agent solutions in Microsoft Copilot Studio
**Harness:** GitHub Copilot (GHCP)
**Estimated duration:** 70–90 minutes

---

## Scenario

Contoso's customer service organization is standing up an AI agent to help internal reps handle support tickets faster. External customers reach out through the usual channels; behind the scenes, the **rep** uses the **Customer Support Rep Assistant** to look up context, decide next steps, and package repeatable actions. In this lab you author the base agent.

> **Scenario roles**
> - **User of the agent:** internal customer service representative (employee)
> - **Beneficiary:** external customer receiving support
> - The external customer never interacts with the agent directly.

> Product / company names in this lab are neutral placeholders. When the scenario brand is finalized, the solution schema (`sup_` prefix) survives a reskin.

---

## Lab objectives

By the end of this lab you can:

- Create a Power Platform Developer environment with Dataverse.
- Create a solution and set it as your preferred solution.
- Create an agent on the GitHub Copilot harness, configure its identity and natural-language instructions, enable memory, confirm safety and access, and validate the instructions in Preview.
- Author a reusable skill from blank and upload a pre-built skill from a `SKILL.md`, then validate skill activation in Preview.

## OD subtask coverage (Lab 1)

- Create a Power Platform environment (Exercise 1).
- Create a solution (Exercise 2) — pulled forward from Lab 5 per the 2026-08-27 direction.
- Plan reusable agent components (Exercises 3, 4)
- Create a new skill (Exercise 4)
- Upload a skill (Exercise 4)
- Enable and manage memory for user-specific context (Exercise 3)
- Review and confirm safety and access settings (Exercise 3)

---

## Prerequisites

- Microsoft Copilot Studio access with the **New experience** toggle enabled (GHCP authoring surface).
- Access to the Power Platform admin center with permissions to create a Developer environment.
- Skillable Valorem lab environment provisioned for AB-620 v2, or a Microsoft 365 tenant with equivalent licensing.
- Provided assets in `assets/`:
    - `faq-lookup.SKILL.md` — for Exercise 4. Includes the FAQ content the skill answers from.

No starter solution required — this lab is designed to be run on a fresh Microsoft 365 tenant.

---

## Cost note — Copilot Credits consumption

Agents on the GitHub Copilot harness use **consumption-based billing (Copilot Credits)**. In Lab 1:

| Step | What consumes credits | Profile |
|---|---|---|
| Exercise 1 — Environment provisioning | None | Zero |
| Exercise 2 — Solution setup | None | Zero |
| Exercise 3 — Instruction validation in Preview | Model call | Moderate (3 prompts) |
| Exercise 4 — Skill test in Preview | Model call + skill invocation | Low (2 prompts) |

**Free:** all Build-tab and Settings configuration. Only Preview runtime consumes credits. Keep validation to 1–3 prompts per exercise to control burn.

---

## Exercises

1. [Exercise 1 — Create a Power Platform environment](exercise-1-create-environment.md)
2. [Exercise 2 — Create a solution to manage agent components](exercise-2-create-solution.md)
3. [Exercise 3 — Create an agent and configure its identity, memory, and safety](exercise-3-create-agent-and-identity.md)
4. [Exercise 4 — Build reusable behavior with skills](exercise-4-build-skills.md)

---

## End-state check

- Power Platform Developer environment created with Dataverse; selected as the active environment in Copilot Studio.
- Customer Support Rep Assistant solution created with `Support` publisher (prefix `sup`), set as preferred.
- Customer Support Rep Assistant agent inside that solution, with identity + instructions authored, memory enabled, safety and access confirmed, and instructions validated in Preview.
- Two skills attached (troubleshooting-tree from blank + FAQ-lookup uploaded), with skill activation validated in Preview.

---

## Instructor notes

- **Delivery mode.** ILT delivery uses the bundled Skillable Valorem instance; saves persist multi-day. Modular self-paced delivery is supported by starter solution zips for later labs that represent the end-state of this lab.
- **Environment provisioning latency.** Task 1 in Exercise 1 can take several minutes on shared tenants. Warn learners at the start of the lab and consider having them start provisioning before the introductory content wraps.
- **New experience toggle** may default to off in some Skillable provisions. Enable it on the Home page before Exercise 3.
- **Preview availability.** If blocked, fall back to inspection-based validation per the per-exercise callouts.
- **Cost visibility.** Point learners at the Cost note before Preview steps.
- **Lab 5 OD coverage adjustment.** "Create a solution" moves from Lab 5 to Lab 1 Exercise 2. Lab 5 continues to own the remaining ALM subtasks (add existing components, environment variables, pipelines, dev→test→prod promotion).

---

## Sources cross-checked

- Copilot Studio Technical Deep Dive Deck — terminology + GHCP component model.
- Agent Academy Recruit NextGen, Missions 03 and 04 — format precedent for solution + agent creation.
- Microsoft Learn — Configure agent details and instructions; Build tab overview; Solution export/import.
