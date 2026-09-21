# Lab 3 starter solution — import notes

For modular learners.

## What's inside

Everything in the Lab 2 starter, plus:

- Two workflows from Lab 2: **Create return authorization**, **Approve refund request**. (Publisher prefix `sup_` is applied to the schema names automatically; display names are action-oriented.)
- Dataverse **Orders** and **Customer Records** tables (schema + minimal seed rows — Priya as customer, order 9876 in Orders).

## Post-import checklist

1. Confirm the solution is preferred.
1. Verify the two workflows show as tools on the agent.
1. Confirm the Orders and Customer Records tables exist and contain seed rows.
1. Fallback: if Dataverse tables are missing, provision SharePoint lists per Lab 3 README instructor notes and update workflow references.

## Known gotchas

- The Work IQ (preview) MCP tool is not part of the starter solution zip. Learners add it live in Exercise 2. Its availability depends on tenant admin having enabled Work IQ for the tenant (see [Enable your tenant for Work IQ](https://learn.microsoft.com/microsoft-365/copilot/extensibility/work-iq/enable-work-iq)). Ex 2 Task 1 walks the learner through seeding two calendar events on the tenant account so the Preview tests have retrievable context.
