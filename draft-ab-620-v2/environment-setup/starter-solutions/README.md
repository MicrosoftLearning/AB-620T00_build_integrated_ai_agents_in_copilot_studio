# Starter solution zips

**Status as of 2026-09-06: pending — the zip files themselves haven't been produced yet.** Until they are, use the click-by-click fallback in `../dataverse-manual-provisioning.md`. This README documents the intended shape so we're ready when someone with a working Copilot Studio env exports them.

---

## Intended shape

Three managed solution zips, one per lab, each cumulative:

| Zip | Contents | Consumed by |
|---|---|---|
| `lab-2-starter.zip` | Publisher `Support` (prefix `sup`) + solution `Customer Support Rep Assistant` + Lab 1 agent (identity, instructions, skills, memory, model, safety) + **Returns** Dataverse table (schema only) | Lab 2 Ex 1 |
| `lab-3-starter.zip` | Everything in `lab-2-starter.zip` + **Create return authorization** and **Approve refund request** workflows + **Orders** and **Customer Records** Dataverse tables + seed rows from `../../sample-data/orders.csv` and `../../sample-data/customer-records.csv` | Lab 3 Ex 1 |
| `lab-4-starter.zip` | Everything in `lab-3-starter.zip` + **Products** Dataverse table + seed rows from `../../sample-data/products.csv` | Lab 4 Ex 0 |
| `lab-5-starter.zip` | Everything in `lab-4-starter.zip` + Fulfillment agent connected and configured (Products knowledge + Create shipment request workflow attached) + enriched product-troubleshooting skill | Lab 5 Ex 1 |

Each zip is a **managed** solution unless the trainer needs to edit the imported components inline for a demo (in which case, export as **unmanaged** for that specific delivery).

## Import path (this is what the lab exercises point at)

```
1. Copilot Studio → Solutions → Import solution
2. Browse to Allfiles\Lab0N\lab-N-starter.zip
3. Import → wait ~2 minutes
4. Set the imported solution as your preferred solution
5. Publish all customizations
```

That's the entire modular-learner setup path per lab. Same shape as [PL-400 Lab 01](https://github.com/MicrosoftLearning/PL-400_Microsoft-Power-Platform-Developer/blob/master/Instructions/Labs/LAB%5BPL-400%5D_Lab01_ImportSolution.md).

## How to produce the zips (pre-release TODO)

Someone with a working Copilot Studio developer environment does this once per lab, then commits the resulting `.zip` file:

1. Complete all exercises in the prior lab end-to-end so the environment holds the correct end-state.
1. In Copilot Studio, open **Solutions** → **Customer Support Rep Assistant** → ellipsis (**...**) → **Export solution**.
1. Choose **Managed** (or Unmanaged for editable demos).
1. Wait for the export. Download the zip.
1. Rename to `lab-N-starter.zip` and commit to `AB-620 Labs/Allfiles/Lab0N/lab-N-starter.zip` (matching the PL-400 folder pattern).
1. Verify by importing into a clean second environment and running through the next lab's exercises.

## Seed data source of record

Seed rows for Orders, Customer Records, and Products are the CSVs in `../../sample-data/`. Whoever produces the zips should import those CSVs before exporting so the seed data travels with the solution.

## Naming and prefix note

If your Copilot Studio environment defaults to a different publisher (e.g. the tenant's default publisher), **change it to `Support` / `sup` before creating any components** — otherwise the exported solution will use logical names like `cr_customeremail` instead of `sup_customeremail`, and every workflow / knowledge reference in Labs 2–5 will need to be updated.
