# Lab 4 — Configure a multi-agent solution

**Course:** AB-620 v2 — Design and build integrated AI agent solutions in Microsoft Copilot Studio
**Harness:** GitHub Copilot (GHCP)
**Estimated duration:** 85–100 minutes

---

## Scenario recap

Customer Support Rep Assistant now has knowledge, workflows, and tools. In this lab you add a **Fulfillment agent** as a connected specialist that owns warehouse operations — SKU inventory checks and replacement shipments — because that's a legitimately different domain from customer support and belongs to a different team. Product-info and return/refund work stay on the orchestrator. You configure the Fulfillment specialist, give it a shipment-request workflow, connect it to the orchestrator, and validate the multi-agent handoff end-to-end.

**User of the agent:** internal customer service rep. **Beneficiary:** external customer. **Cross-functional participant:** warehouse operations team.

---

## Lab objectives

By the end of this lab you can:

- Configure a specialist agent's instructions and knowledge.
- Author a workflow and attach it to a specialist agent (not the orchestrator).
- Register a connected agent on the orchestrator with a scoped delegation description.
- Enrich a reusable skill with agent-agnostic triage outcomes that survive the multi-agent split.

## OD subtask coverage (Lab 4)

- Add connected agents
- Reuse existing skills across agents (skill enrichment in the final exercise of this lab)

---

## Prerequisites

- Completed Lab 3, **or** import the Lab 4 starter (`assets/lab-4-starter-solution.md`).
- Dataverse **Products** table inside the `Customer Support Rep Assistant` solution, seeded from `../sample-data/products.csv`. Modular learners get this from the Lab 4 starter zip in Exercise 0. Fallback provisioning paths live in `../environment-setup/`.

---

## Cost note — Copilot Credits consumption

| Step | What consumes credits | Profile |
|---|---|---|
| Exercise 0 — Set up starter state | None | Zero |
| Exercise 1 — Configure the Fulfillment specialist | None (no Preview call) | Zero |
| Exercise 2 — Author + attach the shipment workflow | None (workflow authoring; no Preview) | Zero |
| Exercise 3 — Connect + enrich + multi-agent Preview test | Orchestrator + specialist model calls + two workflow invocations (return + shipment) | Moderate — the credit-heaviest single turn in the lab |

**Flag: Exercise 3.** One multi-agent Preview turn now costs at minimum 2 model calls (orchestrator + Fulfillment) plus 2 workflow invocations. Cap validation at the one prompt provided.

---

## Exercises

1. [Exercise 0 — Set up the Lab 4 starter state](exercise-0-setup.md)
1. [Exercise 1 — Configure the Fulfillment specialist agent](exercise-1-configure-fulfillment.md)
1. [Exercise 2 — Author the Create shipment request workflow and attach it to the Fulfillment agent](exercise-2-shipment-workflow.md)
1. [Exercise 3 — Connect the Fulfillment agent, enrich the skill, and test end-to-end](exercise-3-connect-and-test.md)

---

## End-state check

- Fulfillment agent configured with instructions and Products knowledge.
- **Create shipment request** workflow authored, published, and attached to the Fulfillment agent (not the orchestrator).
- Fulfillment registered as a connected agent on Customer Support Rep Assistant with a scoped delegation description.
- `product-troubleshooting` skill enriched with agent-agnostic triage outcomes that include a replacement-shipment step.
- End-to-end multi-agent handoff test passes: orchestrator authorizes the return, delegates to Fulfillment for the replacement shipment, and reports both back to the rep.

---

## Instructor notes

- **Why a Fulfillment specialist?** The design rationale — warehouse operations is a legitimately different domain from customer support and earns its own agent, while return/refund work is same-domain and stays on the orchestrator — is taught in the course training content, not in the lab itself. If a learner asks in the moment, the short answer is: connected agents are for cross-functional work (different team, different domain, different tools). Everything else should stay on the orchestrator.
- **Return/refund workflows stay on the orchestrator.** Do not move them to Fulfillment — they're same-domain as the customer-support orchestrator. Fulfillment owns only warehouse-operations tools.
- **Agent-agnostic skills.** The enriched `product-troubleshooting` skill in Ex 3 uses triage outcomes without naming which agent or tool executes each step. The orchestrator's instructions do the routing. Reinforce this pattern — it's what makes the skill reusable if a learner later attaches it to the Fulfillment specialist too.
- **Handoff cost.** Emphasize the credit multiplier of multi-agent turns to learners before the Ex 3 Preview step.

---

## Sources cross-checked

- Copilot Studio Deep Dive Deck — the component model and the *choose the smallest component* design rule.
- Agent Academy Mission 04 — precedent for delegation and handoff.
- Microsoft Learn — [Connected agents overview](https://learn.microsoft.com/microsoft-copilot-studio/authoring-connected-agents).
