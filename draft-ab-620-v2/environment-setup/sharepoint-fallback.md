# SharePoint list fallback (last resort)

Use this doc **only if** Dataverse table creation is unavailable in your environment. It's a last-resort fallback that compromises part of the pedagogical arc — read the **What you give up** section before choosing this path.

## What you give up

Dataverse is the canonical Power Platform primitive for structured data inside a Copilot Studio solution, and Labs 3–5 lean on that. If you use SharePoint lists instead:

- **Lab 3 Ex 2 — Knowledge grounding.** Copilot Studio's `Add knowledge` picker treats Dataverse tables as a first-class source. SharePoint is available too, but through the Copilot connector — a slightly different UX flow. Learn the SharePoint version, and the muscle memory won't transfer cleanly to a real Dataverse-backed rollout.
- **Lab 5 Ex 5 — ALM story.** Dataverse tables live inside the preferred solution and travel with it across dev → test → prod in the pipeline exercise. SharePoint lists don't. Every environment you promote to needs the list manually recreated. The "solutions are the unit of movement" story you're teaching stops holding.
- **Realism.** Reps in the field would consume from a Dataverse table (or an equivalent enterprise system), not a SharePoint list. The scenario is more coherent when the labs mirror that.

If any of those matter for your delivery, do whatever it takes to unblock Dataverse (a different environment, a different tenant, a fresh Skillable request) before falling back to this doc.

## Lists to create

Create these in a SharePoint site your rep account can reach. Use identical display names to the Dataverse tables so any lab references still read sensibly.

### Returns list — Lab 2

| Column | Type |
|---|---|
| `Authorization number` (rename the default `Title` column) | Single line of text |
| `Customer email` | Single line of text |
| `Order ID` | Single line of text |
| `Reason` | Multiple lines of text (500 chars) |
| `Condition` | Single line of text |
| `Expected refund` | Number |
| `Status` | Choice: Authorized, Approved, Rejected, Escalated |
| `Approved by` | Single line of text |

No seed rows — the workflow writes the first row in Lab 2 Ex 2.

### Orders list — Lab 3

| Column | Type |
|---|---|
| `Order ID` (rename `Title`) | Single line of text |
| `Customer email` | Single line of text |
| `Customer name` | Single line of text |
| `Order date` | Date |
| `SKU` | Single line of text |
| `Product name` | Single line of text |
| `Total` | Number |
| `Status` | Single line of text |
| `Ship to` | Multiple lines of text |

Seed rows: import from `../sample-data/orders.csv`.

### Customer Records list — Lab 3

| Column | Type |
|---|---|
| `Customer email` (rename `Title`) | Single line of text |
| `Customer name` | Single line of text |
| `Preferred channel` | Choice: Email, Phone, SMS |
| `Loyalty tier` | Choice: Bronze, Silver, Gold |
| `Notes` | Multiple lines of text |

Seed rows: import from `../sample-data/customer-records.csv`.

### Products list — Lab 4

| Column | Type |
|---|---|
| `SKU` (rename `Title`) | Single line of text |
| `Product name` | Single line of text |
| `Category` | Single line of text |
| `Wattage` | Number |
| `Warranty months` | Number |
| `Compatibility notes` | Multiple lines of text |

Seed rows: import from `../sample-data/products.csv`.

## Adjustments in the lab exercises

Wherever a lab exercise says "Dataverse → Add a new row" or "Dataverse → Update a row" (Lab 2 workflows) or "Add knowledge → Dataverse tables" (Lab 3), use the SharePoint equivalent instead:

- Workflows: **SharePoint → Create item** / **Update item** / **Get item** targeting the list URL you noted.
- Knowledge: **Add knowledge → SharePoint** (via the Copilot connector) targeting the same site.

## What to note before starting the labs

- **Site URL and list names.** You'll paste these into workflow and knowledge references in Labs 2–4.
- **Column internal names.** SharePoint generates internal names by stripping spaces from display names on creation (subsequent renames don't update the internal name). If a column comes out with an internal name like `Customer_x0020_email`, that's fine — the workflow references it by internal name once created.
- **Publisher / solution boundary.** Your SharePoint lists live **outside** the `Customer Support Rep Assistant` solution. When you get to Lab 5 Ex 5, the pipeline promotion won't include them. You'll need to manually recreate the lists in each downstream environment and update environment variables to point at the new URLs.
