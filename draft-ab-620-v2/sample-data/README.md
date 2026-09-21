# Sample data for AB-620 v2 labs

This folder contains the shared seed data every lab depends on. The rows carry the **Priya + order 9876 + SKU BLD-100** storyline through Labs 1–5.

You never type these rows by hand. You either (a) get them via a starter solution zip that already contains the seed rows, or (b) import the CSVs into freshly-created Dataverse tables using **Get data → From text/CSV**. The environment-setup docs tell you which path applies.

## Files

| File | Used by | What it is |
|---|---|---|
| `orders.csv` | Lab 3 (Orders knowledge), Lab 4 (order 9876 SKU lookup), Lab 5 (eval set) | Customer orders keyed by `order_id` |
| `customer-records.csv` | Lab 3 (Customer Records knowledge) | Customer profile keyed by `customer_email` |
| `products.csv` | Lab 4 (Fulfillment agent knowledge) | Product catalog keyed by `sku` |
| `returns-schema.csv` | Lab 2 (Returns table) | **Headers only** — populated by the **Create return authorization** workflow at runtime |
| `faq-content.md` | Lab 1 Exercise 3 (FAQ-lookup skill) — for reference | Human-readable version of the FAQ text the skill answers from |

The Priya + order 9876 storyline is intentional: it's the through-line for every Preview test prompt in every lab.

## How the seed rows reach a lab environment

The three provisioning paths, in order of preference:

1. **Starter solution zip (primary path once zips exist).** Import `lab-N-starter.zip` from `../environment-setup/starter-solutions/`. Tables and seed rows land in one step. See [`../environment-setup/starter-solutions/README.md`](../environment-setup/starter-solutions/README.md).
2. **Dataverse manual provisioning (current fallback).** Follow [`../environment-setup/dataverse-manual-provisioning.md`](../environment-setup/dataverse-manual-provisioning.md) to create the tables, then use **Get data → From text/CSV** to import the CSVs from this folder. This is the path testers use today because the zips don't exist yet.
3. **SharePoint list fallback (last resort).** Follow [`../environment-setup/sharepoint-fallback.md`](../environment-setup/sharepoint-fallback.md). Only when Dataverse itself is blocked in the environment. Compromises the Lab 5 ALM story — see that doc for what to expect.

## Table names and primary keys

| CSV | Dataverse table name | Primary key |
|---|---|---|
| `orders.csv` | Orders | `order_id` |
| `customer-records.csv` | Customer Records | `customer_email` |
| `products.csv` | Products | `sku` |
| `returns-schema.csv` | Returns | `authorization_number` |

## Fictitious content

- Customer names and emails use approved fictitious content: `firstname@contoso.com` pattern (`priya@contoso.com`, etc.). This matches Microsoft's CELA-approved fictitious-content guidance.
- The emails are **Dataverse primary keys** — text tokens inside test prompts. Nothing gets sent to them.
- Do **not** use real customer PII in these files.
- If you add or change a storyline persona, keep the same pattern (`firstname@contoso.com`). Don't invent new domains.

