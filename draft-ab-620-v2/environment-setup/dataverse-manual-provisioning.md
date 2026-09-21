# Dataverse manual provisioning (fallback path)

Use this doc **only if** the starter solution zip isn't available for your lab and you need to hand-build the Dataverse tables. If the zip is available, use `starter-solutions/README.md` instead — this doc is the ~15-minutes-per-table fallback, not the primary path.

## Before you start

- Confirm your preferred solution is **Customer Support Rep Assistant** with publisher **Support** (prefix `sup`). If the publisher is different, either change it now or record the actual prefix and substitute it everywhere the tables below say `sup_`.
- All tables in this doc live **inside the preferred solution** so they travel with the agent across environments (Lab 5's ALM story depends on this).
- Column display names and logical names are important — the workflows and knowledge references in Labs 2–4 use the logical names verbatim. If a logical name comes out different in your environment, record it and substitute wherever it appears in the lab exercises.

## Tables to provision

| Table | Lab that needs it | This doc's section |
|---|---|---|
| **Returns** | Lab 2 | [Returns table](#returns-table) |
| **Orders** | Lab 3 | [Orders table](#orders-table) |
| **Customer Records** | Lab 3 | [Customer Records table](#customer-records-table) |
| **Products** | Lab 4 | [Products table](#products-table) |

Only build the tables you need for the lab you're about to start — you don't have to do all four upfront.

## Table-creation flow (common to all four)

Every table in this doc uses the same click-through. This section is written once; the per-table sections just list the fields.

### Create the shell and add columns

1. In **Copilot Studio** left navigation, select **Solutions**.
1. Select the **Customer Support Rep Assistant** solution to open it.
1. In the top command bar, select **+ New** → **Table** → **Tables**.
1. On the "Choose an option to create tables" page, select **Start from blank**.
1. From the table card, select the ellipses then select **Properties**.
1. In the Edit table form, fill in **Display name**, **Plural display name** (auto-filled — leave as-is), and **Description** then select **Save**.
1. From the table card, select the ellipses then select **View data**.
1. Expand the placeholder column titled **New column** then select **Edit column**. Fill in the primary column's **Display name** and **Description**. Leave the type as **Text**.
1. Select **+ New column**.
1. Fill in the fields from the per-table table below:
    - **Display name** — required. Copy verbatim from the per-table table.
    - **Data type** — required. Select from the dropdown.
    - **Format** — only appears for the `Single line of text` data type. Select `Text` (the default) unless the per-table table specifies otherwise (for example, `Email`).
    - **Required** — leave off.
1. **Choice-type columns.** When **Data type** is `Choice`, no Format dropdown appears. Instead:
    - Leave **Selecting multiple choices is allowed** unchecked (single-select).
    - In the **Choices** table, enter the labels from the per-table table under **Label**. Leave the auto-assigned **Value** integers (like `924,780,000`) as-is — the workflows read Labels, not Values.
    - Leave **Default choice** set to **None** unless the per-table table says otherwise.
1. **Confirm the schema name.** Expand **Advanced options** and confirm the **Schema name** field is pre-filled with the expected logical name (for example, `sup_Customeremail` for **Customer email**). Copilot Studio derives it by prepending the `sup_` prefix, stripping spaces, and **preserving the display name's original casing** — so `Customer email` becomes `sup_Customeremail` (capital `C`) and `Order ID` becomes `sup_OrderID`. Dataverse column references in workflows are case-insensitive, so the lab exercises will still resolve if you see the letters in a different case. If the schema name comes out structurally different (extra underscore, different prefix), record the actual value and substitute it wherever the exercise references the logical name. Leave **Visualization** set to **None**.
1. Select **Save and exit**.

Repeat for each column in the per-table section.

### Confirm it's in the solution

1. Return to **Solutions** → **Customer Support Rep Assistant**.
1. Confirm the new table appears under **Tables**. If not, use **+ Add existing** → **Table** → *(your table)* to bring it in.
1. Save.

### Common notes

- **Choice vs. `Single line of text`.** Any column marked with data type `Choice` in the tables below can fall back to `Single line of text` (Format: `Text`) if your Copilot Studio version won't let you create a local Choice inline. The workflows write plain strings either way. Choice is preferred for governance.
- **Email format.** Any column with data type `Single line of text` and Format `Email` can fall back to Format `Text` — the workflow stores an email address as a string either way. `Email` is preferred because it enables click-to-mail in model-driven apps.
- **Seed rows.** Sample rows for Orders, Customer Records, and Products live at `../sample-data/*.csv`. Import them via **Get data → From text/CSV** on each table after you've built the schema. Returns starts empty (the workflow writes the first row in Lab 2 Ex 2).

---

## Returns table

**Lab that needs it:** Lab 2.

**Shell.**

| Field | Value |
|---|---|
| Display name | `Returns` |
| Description | `Return authorizations created by the Customer Support Rep Assistant.` |
| Primary column display name | `Authorization number` |
| Primary column description | `RA-<timestamp> identifier written by the Create return authorization workflow.` |
| Expected logical name (table) | `sup_Return` |
| Expected logical name (primary column) | `sup_Authorizationnumber` |

**Additional columns.**

| Display name | Data type | Format | Choice values / notes | Expected logical name |
|---|---|---|---|---|
| `Customer email` | `Single line of text` | `Email` | — | `sup_Customeremail` |
| `Order ID` | `Single line of text` | `Text` | — | `sup_OrderID` |
| `Reason` | `Single line of text` | `Text` | Max length: 500 | `sup_Reason` |
| `Condition` | `Single line of text` | `Text` | Max length: 200 | `sup_Condition` |
| `Expected refund` | `Whole number` | — | — | `sup_Expectedrefund` |
| `Status` | `Choice` | — | Labels: `Authorized`, `Approved`, `Rejected`, `Escalated` | `sup_Status` |
| `Approved by` | `Single line of text` | `Email` | — | `sup_Approvedby` |

**Seed rows:** none. The workflow writes the first row in Lab 2 Ex 2.

---

## Orders table

**Lab that needs it:** Lab 3.

**Shell.**

| Field | Value |
|---|---|
| Display name | `Orders` |
| Description | `Customer orders. One row per order.` |
| Primary column display name | `Order ID` |
| Expected logical name (table) | `sup_Order` |
| Expected logical name (primary column) | `sup_OrderID` |

**Additional columns.**

| Display name | Data type | Format | Notes | Expected logical name |
|---|---|---|---|---|
| `Customer email` | `Single line of text` | `Email` | — | `sup_Customeremail` |
| `Customer name` | `Single line of text` | `Text` | — | `sup_Customername` |
| `Order date` | `Date only` | — | — | `sup_Orderdate` |
| `SKU` | `Single line of text` | `Text` | — | `sup_SKU` |
| `Product name` | `Single line of text` | `Text` | — | `sup_Productname` |
| `Total` | `Currency` | — | Fall back to `Decimal` if `Currency` is unavailable | `sup_Total` |
| `Status` | `Single line of text` | `Text` | — | `sup_Status` |
| `Ship to` | `Multiple lines of text` | — | — | `sup_Shipto` |

**Seed rows.** Import `../sample-data/orders.csv`. Confirm order **9876** exists — it's the storyline row for the rest of the course.

---

## Customer Records table

**Lab that needs it:** Lab 3.

**Shell.**

| Field | Value |
|---|---|
| Display name | `Customer Records` |
| Description | `Customer profile records. One row per customer.` |
| Primary column display name | `Customer email` |
| Expected logical name (table) | `sup_CustomerRecord` |
| Expected logical name (primary column) | `sup_Customeremail` |

**Additional columns.**

| Display name | Data type | Format | Choice values / notes | Expected logical name |
|---|---|---|---|---|
| `Customer name` | `Single line of text` | `Text` | — | `sup_Customername` |
| `Preferred channel` | `Choice` | — | Labels: `Email`, `Phone`, `SMS` | `sup_Preferredchannel` |
| `Loyalty tier` | `Choice` | — | Labels: `Bronze`, `Silver`, `Gold` | `sup_Loyaltytier` |
| `Notes` | `Multiple lines of text` | — | — | `sup_Notes` |

**Seed rows.** Import `../sample-data/customer-records.csv`. Confirm `priya@contoso.com` exists with preferred channel **Email** and loyalty tier **Gold**.

---

## Products table

**Lab that needs it:** Lab 4.

**Shell.**

| Field | Value |
|---|---|
| Display name | `Products` |
| Description | `Product catalog. One row per SKU.` |
| Primary column display name | `SKU` |
| Expected logical name (table) | `sup_Product` |
| Expected logical name (primary column) | `sup_SKU` |

**Additional columns.**

| Display name | Data type | Format | Notes | Expected logical name |
|---|---|---|---|---|
| `Product name` | `Single line of text` | `Text` | — | `sup_Productname` |
| `Category` | `Single line of text` | `Text` | — | `sup_Category` |
| `Wattage` | `Whole number` | — | — | `sup_Wattage` |
| `Warranty months` | `Whole number` | — | — | `sup_Warrantymonths` |
| `Compatibility notes` | `Multiple lines of text` | — | — | `sup_Compatibilitynotes` |

**Seed rows.** Import `../sample-data/products.csv`. Confirm SKU **BLD-100** (Countertop Blender, 700W, 24-month warranty) exists — it's on Priya's order 9876 and drives every product-info test prompt in Lab 4 and Lab 5.

---

## What if my logical names don't match?

Most common causes:

- **Different publisher / prefix.** Your environment's preferred solution has a different publisher (e.g. the default `cr_` prefix). Change the publisher on the solution to `Support` / `sup` before creating the tables, or record the actual prefix and substitute it wherever the lab exercises reference `sup_...`.
- **Different display-name normalization.** Copilot Studio strips spaces from the display name and preserves its casing to form the logical name. `Order ID` → `sup_OrderID`, `Customer email` → `sup_Customeremail`. Dataverse column references are case-insensitive, so casing differences don't break the lab workflows. If yours comes out structurally different (e.g. `sup_order_id` with an underscore, or a different prefix), record the actual value and substitute.

Either way — **record what you actually get** and paste it into the corresponding workflow / knowledge / connected-agent reference in the lab. Don't try to force-rename the logical name after the fact; that's a fresh set of headaches.
