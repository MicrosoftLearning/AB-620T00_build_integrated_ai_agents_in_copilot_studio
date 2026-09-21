# Lab 3 — Integrate knowledge and tools

**Course:** AB-620 v2 — Design and build integrated AI agent solutions in Microsoft Copilot Studio
**Harness:** GitHub Copilot (GHCP)
**Estimated duration:** 75–95 minutes

---

## Scenario recap

The rep now has workflows for return and refund. But the workflows fire on data the rep types in — the agent still doesn't **know** what an order is, who the customer is, or where to look up policy. In this lab you ground the agent in Dataverse and add MCP-based and connector-based tools so the agent can gather context on its own before acting.

**User of the agent:** internal customer service rep. **Beneficiary:** external customer whose orders and records are being looked up.

---

## Lab objectives

By the end of this lab you can:

- Ground the agent in Dataverse tables (Orders, Customer Records) as knowledge.
- Add a Model Context Protocol (MCP) server as a tool.
- Add a connector as a tool (Outlook) and expose Lab 2 workflows to the agent as tools.

## OD subtask coverage (Lab 3)

- Ground an agent in enterprise data
- Add an MCP server as a tool
- Add a connector as a tool
- Add a workflow as a tool (revisited from Lab 2 in the context of the tool inventory)

---

## Prerequisites

- Completed Lab 2, **or** import the Lab 3 starter solution (`assets/lab-3-starter-solution.md`).
- Dataverse **Orders** and **Customer Records** tables inside the `Customer Support Rep Assistant` solution, seeded from `../sample-data/orders.csv` and `../sample-data/customer-records.csv`. Modular learners get these from the Lab 3 starter zip in Exercise 1. Fallback provisioning paths live in `../environment-setup/`.
- A tenant admin has enabled **Work IQ (preview)** for the tenant. Fallback: any other built-in MCP server in the Add-a-tool picker (adjust the Ex 2 Task 1 seed step accordingly).
- Outlook (Office 365) connector reachable.

---

## Cost note — Copilot Credits consumption

| Step | What consumes credits | Profile |
|---|---|---|
| Exercise 0 — Set up starter state | None | Zero |
| Exercise 1 — Knowledge grounding + 1–2 test prompts | Model call + Dataverse read | Low |
| Exercise 2 — MCP-as-tool test | Model call + MCP call | Moderate |
| Exercise 3 — Multi-tool orchestration test | Model call + multiple tool invocations per prompt | **High** — flag |

**Flag: Exercise 3** deliberately exercises multiple tools in a single agent turn (Outlook + prior-work workflow). Cap validation at 1–2 prompts. Each prompt may trigger 3–4 tool invocations.

---

## Exercises

1. [Exercise 0 — Set up the Lab 3 starter state](exercise-0-setup.md)
1. [Exercise 1 — Ground an agent in Dataverse knowledge](exercise-1-ground-in-dataverse.md)
1. [Exercise 2 — Add an MCP server as an agent tool](exercise-2-mcp-as-tool.md)
1. [Exercise 3 — Add a connector operation as an agent tool and audit the tool inventory](exercise-3-connector-and-workflow-tools.md)

---

## End-state check

- Agent has knowledge sources: **Orders** and **Customer Records**.
- Tool inventory contains: at least one MCP tool, Outlook connector-as-tool, and both Lab 2 workflows.
- Multi-tool test prompt executes end-to-end from Preview.

---

## Instructor notes

- **Fallback: Dataverse Orders / Customer Records → SharePoint lists.**
- **Fallback: Work IQ (preview) → any other built-in MCP server in the Add-a-tool picker** (adjust the Ex 2 Task 1 seed step accordingly). All serve the "external retrieval via MCP" OD objective; only the seed content and sample prompts change.
- **Tool selection determinism.** The agent won't always pick the "right" tool. Frame this as an expected behavior to be managed with instructions (Lab 4 revisits the `product-troubleshooting` skill precisely to address this).
- **High-cost exercise.** Watch credit burn during Exercise 3 validation — multi-tool prompts can trigger 3–4 tool invocations each.

---

## Sources cross-checked

- Copilot Studio Deep Dive Deck — knowledge, MCP-as-tool, connector-as-tool primitives.
- Agent Academy Mission 04 — tool orchestration precedent.
- Microsoft Learn — Add knowledge; Add tools; MCP overview.
