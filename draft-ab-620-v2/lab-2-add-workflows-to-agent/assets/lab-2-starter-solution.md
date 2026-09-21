# Lab 2 starter solution — import notes

This asset describes the **Lab 2 starter solution zip** for modular delivery. Sequential learners who completed Lab 1 don't need this.

## What's inside

- Publisher: **Support** (prefix `sup`).
- Solution name: **Customer Support Rep Assistant**.
- Copilot Studio agent: **Customer Support Rep Assistant** on the GHCP harness with:
    - Identity + instructions authored during prior work.
    - Two skills: `product-troubleshooting`, `faq-lookup`.
    - Memory enabled (per-user, Microsoft-managed).
    - Safety & access set to **Authenticate with Microsoft** (with **User feedback** on).
- Dataverse **Returns** table (schema only — no seed rows).

## Post-import checklist

1. Confirm the solution is the **preferred solution**.
1. Open the agent and verify all five components above.
1. Send one Preview prompt (`Give me a one-line status`) and confirm a coherent reply.
1. If the Dataverse Returns table isn't provisioned in this environment, fall back to a SharePoint list per Lab 2 README instructor notes.

## Known gotchas

- Publisher prefix `sup_` is baked into the Returns table column names. Do not rebrand the publisher after import — schema updates would be manual.
- The starter agent does not include any workflows, tools, connected agents, or knowledge — those are Labs 2, 3, and 4 subject matter.
