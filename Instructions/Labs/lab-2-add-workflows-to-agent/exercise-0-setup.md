---
lab:
    title: '2.0: Import the Dataverse tables and sample data'
    description: Import four Dataverse table definitions into your existing agent environment, then import the sample records used in the remaining labs.
    duration: 30 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Import the Dataverse tables and sample data

The workflows, Dataverse tools, and specialist agent that you build in the remaining labs use four Dataverse tables:

| Table | Where you use it | Starting data |
| --- | --- | --- |
| **Returns** | Lab 2 workflows write return authorizations and refund outcomes to this table. | None |
| **Orders** | Lab 3 grounds the agent in order data. | Seven orders, including order **9876** for `priya@contoso.com` |
| **Customer Records** | Lab 3 grounds the agent in customer profiles. | Five customers, including `priya@contoso.com` |
| **Products** | Lab 4 grounds the Fulfillment agent in product data. | Six products, including SKU **BLD-100** |

In this exercise, you import the four table definitions from a separate additive solution, add them to your existing **Customer Support Rep Assistant** solution, and import the sample rows. The additive solution doesn't replace the agent you built in Lab 1. The solution ZIP contains table definitions, **not** the sample records.

You will complete the following tasks:

- Download the additive solution and sample data files.
- Import the tables and add them to your agent solution.
- Import the sample data.
- Confirm the starting state.

Allow approximately **30 minutes** for this setup. Import times vary by environment; the manual table-creation fallback takes longer.

## Before you start

Complete [Lab 1](../lab-1-design-and-configure-agent/exercise-1-create-environment.md) in this same environment first. The **Customer Support Rep Assistant** solution, with the **Support** publisher (prefix `sup`), should contain your agent and be set as your preferred solution. You also need permission to import a solution and data into this environment.

> [!NOTE]
> If all four tables already exist, skip the solution import and check their rows in Task 3. If only some tables exist, ask your instructor before importing another copy. If the package can't be used, follow the [manual table-creation fallback](../environment-setup/dataverse-manual-provisioning.md), then return to Task 3. Learners who haven't completed Lab 1 need to do so first; this additive package does not contain the agent.

## Task 1 — Download the additive solution and sample data

Download the table definitions and the three files that supply the sample rows. The Returns table starts empty.

1. Download [additive-components.zip](../environment-setup/starter-solutions/additive-components.zip). Save the ZIP without extracting or modifying it.

1. Download each of the following CSV files to your computer. If a file opens in the browser instead of downloading, press **Ctrl+S** to save it.

    - [orders.csv](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/raw/main/Allfiles/sample-data/orders.csv)
    - [customer-records.csv](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/raw/main/Allfiles/sample-data/customer-records.csv)
    - [products.csv](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/raw/main/Allfiles/sample-data/products.csv)

1. Note where you saved the ZIP and CSV files. You'll use them in Tasks 2 and 3.

## Task 2 — Import the four table definitions

Import the separate **AB-620 Additive Components** solution into the environment where you built your agent. Then add the imported tables to your agent's solution so they can be included when you promote that solution later.

1. In Copilot Studio, select the ellipsis (**...**) next to your account name, and then select **Solutions**.

    > [!NOTE]
    > If **Solutions** reroutes you to the classic Power Apps experience, select the ellipsis (**...**) next to your account name and select **Solutions** again.

1. On the command bar, select **Import solution**, choose the ZIP from Task 1, and select **Next** > **Import**. Wait for the import to complete.

1. In **Solutions**, confirm **AB-620 Additive Components** is present. It provides the four tables in a separate solution; it does not replace **Customer Support Rep Assistant**.

1. Open **Customer Support Rep Assistant**, select **+ Add existing** > **Table**, and select **Returns**, **Orders**, **Customer Records**, and **Products**. Select **Next**, choose **Include all objects** for these new tables, and select **Add**. Confirm the tables appear in your agent solution. Their columns use the `sup` publisher prefix.

1. Set **Customer Support Rep Assistant** as your preferred solution if it isn't already. On the **Solutions** page, open the solution's ellipsis (**...**) menu and select **Set preferred solution**.

## Task 3 — Import the sample data

Import the sample rows into the existing Orders, Customer Records, and Products tables. A solution import doesn't carry their records. If Skillable or an instructor already provided all the example rows in your environment, do **not** import the CSVs again.

1. Open the **Orders** table and check whether order **9876** for `priya@contoso.com` is already present. Check **Customer Records** for `priya@contoso.com` and **Products** for SKU **BLD-100**. If a table has only *some* sample rows, ask your instructor before importing the whole file so you don't create duplicates.

1. For any table missing **all** its sample rows, open the matching table and select **Import** > **Import data from Excel**. If prompted to select a table, select the existing table and then **Next**. If this command isn't available, in Power Apps select **Tables** > **Import** > **Import data** > **Text/CSV** and choose the same existing table.

1. Select **Upload** and select the matching CSV that you downloaded in Task 1: `orders.csv` for **Orders**, `customer-records.csv` for **Customer Records**, or `products.csv` for **Products**.

1. Review the mappings before importing. The CSV headers match the table column display names, but confirm that each source column maps to its intended column. Check **Order ID**, **Customer email**, **SKU**, the date and currency fields, and the **Preferred channel** and **Loyalty tier** choices. If mapping shows an error, select **Map columns**, correct it, and select **Save changes**.

1. When **Mapping status** shows **Mapping was successful**, select **Import** and wait for it to finish. Repeat for each table that is entirely missing its sample rows. Do not import the header-only Returns schema file as data.

## Task 4 — Confirm the starting state

Confirm that everything is in place before you author workflows. If something's missing, fix it now so that the workflows you author have tables to read from and write to.

1. In the **Customer Support Rep Assistant** solution, select **Publish all customizations**.

1. Open the **Returns** table. Confirm that the primary column is **Authorization number** and that the table has seven other columns: **Customer email**, **Order ID**, **Reason**, **Condition**, **Expected refund**, **Status**, and **Approved by**. It starts with no rows. The first workflow you author writes the first row.

1. Open the **Orders** table and confirm it has seven sample rows, including order **9876** for `priya@contoso.com`.

1. Open **Customer Records** and confirm it has five sample rows, including `priya@contoso.com` with preferred channel **Email** and loyalty tier **Gold**.

1. Open **Products** and confirm it has six sample rows, including SKU **BLD-100** (**Countertop Blender (700W)**, 24-month warranty).

> [!NOTE]
> The first time you use a connector (**Office 365 Outlook**, **Microsoft Dataverse**, or **Approvals**) in this environment, Power Automate prompts you to sign in and asks for an optional **display name** for the connection. Leave the display name blank so that Power Automate names it, or enter a short name like `Outlook - AB-620`. This prompt appears once per connector for each user in an environment. If it doesn't appear when you configure an action later in this lab, the connection already exists.

You've imported the table definitions and sample data that the remaining labs use. In the next exercise, you author the return-request workflow.
