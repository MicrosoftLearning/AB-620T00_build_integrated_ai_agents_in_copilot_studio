# Lab 5 starter solution — import notes

For modular learners.

## What's inside

Everything in Lab 4 starter, plus:

- The Fulfillment agent registered as a connected agent on the orchestrator.
- Both return/refund workflows attached to the orchestrator (they never moved — they're same-domain as customer support).
- Enriched `product-troubleshooting` skill (delegation-aware).

## Post-import checklist

1. Confirm solution is preferred.
1. Verify the three agents are correctly wired (orchestrator + 2 specialists).
1. Verify the two return/refund workflows are attached to the orchestrator, and the **Create shipment request** workflow is attached to the Fulfillment agent.
1. Verify skills, memory, model, safety, knowledge, tools all intact.
1. Fallbacks:
    - Teams unavailable → M365 Copilot only (Exercise 2).
    - Security group unavailable → individual assignment (Exercise 2).
    - PP Pipeline unavailable → simulated export/import (Exercise 4).

## Known gotchas

- Environment variables in Exercise 4 assume the MCP URL and supervisor email were hard-coded in earlier work. If they were already parameterized upstream, adjust the environment-variable task to reference the existing variables instead of creating new ones.
- The full 15-item eval in Exercise 1 is a large credit expenditure — for ILT, consider a shared-instance demonstration.
