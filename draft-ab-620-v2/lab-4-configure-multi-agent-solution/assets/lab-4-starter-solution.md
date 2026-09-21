# Lab 4 starter solution — import notes

For modular learners.

## What's inside

Everything in Lab 3 starter, plus:

- **Products** Dataverse table with seed rows (SKU BLD-100 plus five other SKUs from `../sample-data/products.csv`).

## Post-import checklist

1. Confirm the solution is preferred.
1. Confirm the orchestrator (Customer Support Rep Assistant) still has its knowledge, tools, workflows, and skills.
1. Confirm the **Products** Dataverse table exists and contains the seed rows (SKU BLD-100 plus five other SKUs from `../sample-data/products.csv`).

## Known gotchas

- Learners create the **Fulfillment agent** live in Exercise 1. It must be created inside the **Customer Support Rep Assistant** solution — connected-agent registration in Exercise 3 requires it to live in the same solution as the orchestrator.
- The **Create shipment request** workflow is not part of the starter zip. Learners author it live in Exercise 2.
- The two return/refund workflows (**Create return authorization**, **Approve refund request**) are attached to the orchestrator, not to the Fulfillment agent — they're customer-support scope, not warehouse scope. Don't reattach them.
