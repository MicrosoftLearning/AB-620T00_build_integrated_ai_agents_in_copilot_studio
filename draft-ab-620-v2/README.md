# AB-620 v2 labs

Hands-on lab content for **AB-620 v2 — Design and build integrated AI agent solutions in Microsoft Copilot Studio**, on the GitHub Copilot (GHCP) harness.

> **Testing this lab set? Start with [`TESTING-GUIDE.md`](TESTING-GUIDE.md).** It covers priorities, known gaps, and how the build plan, sample data, and provisioning docs fit together.

## Structure

- `lab-1-design-and-configure-agent/` — solution, agent, identity, skills, memory, model, safety, validation.
- `lab-2-add-workflows-to-agent/` — return-request workflow, refund-approval HITL workflow, run monitoring.
- `lab-3-integrate-knowledge-and-tools/` — Dataverse grounding, MCP-as-tool, connector-as-tool.
- `lab-4-configure-multi-agent-solution/` — Fulfillment agent as a connected specialist for warehouse operations (replacement shipments and SKU inventory).
- `lab-5-evaluate-publish-and-manage/` — evaluation, publish/deploy, monitoring, ALM.
- `sample-data/` — **shared** seed data used to build starter solution zips (Orders, Customer Records, Products, Returns schema, FAQ content). Referenced from the environment-setup docs — not consumed directly by learners.
- `environment-setup/` — **environment configuration and asset provisioning** for modular delivery. Starter solution zips (primary path), Dataverse manual provisioning (fallback), and SharePoint list fallback (last resort). See `environment-setup/README.md`.

## Sequential vs. modular delivery

- **Sequential (ILT).** Complete Lab 1 → 5 in order in the same environment. State persists across labs — no imports needed after Lab 1.
- **Modular (self-paced).** Each Lab N Exercise 1 imports `lab-N-starter.zip` from `environment-setup/starter-solutions/` and verifies baseline. If a zip isn't available, `environment-setup/dataverse-manual-provisioning.md` is the documented fallback.

## Publisher

Every solution component uses publisher **Support** (prefix `sup`). This is a neutral name chosen so the schema survives brand reskin at delivery time.
