---
lab:
  title: '2.0: Set up the Dataverse tables'
  description: In this exercise, you create the Returns, Orders, Customer Records, and Products Dataverse tables in your solution and import the sample data that the remaining labs use.
  duration: 60 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Copilot Studio
---

# Set up the Dataverse tables

The workflows, Dataverse tools, and specialist agent that you build in the remaining labs read from and write to four Dataverse tables:

| Table | Where you use it | Starting data |
|---|---|---|
| **Returns** | Lab 2 workflows write return authorizations and refund outcomes to this table. | None |
| **Orders** | Lab 3 grounds the agent in order data. | Seven orders, including order **9876** for `priya@contoso.com` |
| **Customer Records** | Lab 3 grounds the agent in customer profiles. | Five customers, including `priya@contoso.com` |
| **Products** | Lab 4 grounds the Fulfillment agent in product data. | Six products, including SKU **BLD-100** |

In this exercise, you create all four tables inside the **Customer Support Rep Assistant** solution and import the sample rows. Because you create the tables inside the solution, they use the `sup` prefix and move with the agent when you promote the solution to other environments in Lab 5.

You will complete the following tasks:

- Download the sample data files.
- Create the Returns table.
- Create the Orders, Customer Records, and Products tables.
- Import the sample data.
- Confirm the starting state.

This exercise should take approximately **60** minutes to complete.

## Before you start

To complete this exercise, you need to have completed [Lab 1](../lab-1-design-and-configure-agent/exercise-1-create-environment.md) in the same environment. The **Customer Support Rep Assistant** solution, with the **Support** publisher (prefix `sup`), should be set as your preferred solution.

## Task 1 — Download the sample data files

Download the three files that contain the sample rows for the Orders, Customer Records, and Products tables. The Returns table starts empty, so it doesn't have a file.

1. Download each of the following files to your computer. If a file opens in the browser instead of downloading, press **Ctrl+S** to save it.

    - [orders.csv](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/raw/main/Allfiles/sample-data/orders.csv)
    - [customer-records.csv](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/raw/main/Allfiles/sample-data/customer-records.csv)
    - [products.csv](https://github.com/MicrosoftLearning/AB-620T00_build_integrated_ai_agents_in_copilot_studio/raw/main/Allfiles/sample-data/products.csv)

1. Note the folder where you saved the files. You browse to it in Task 4.

## Task 2 — Create the Returns table

Every table in this exercise follows the same steps: create the table, rename its primary column, and then add the remaining columns. You build the Returns table first, and then repeat the same steps for the other three tables in Task 3.

1. In Copilot Studio, select the ellipsis (**...**) next to your account name, and then select **Solutions**.

    > [!NOTE]
    > If **Solutions** reroutes you to the classic Power Apps experience, select the ellipsis (**...**) next to your account name and select **Solutions** again.

1. Open the **Customer Support Rep Assistant** solution.

1. On the command bar, select **+ New** > **Table** > **Tables**.

1. On the **Choose an option to create tables** page, select **Start from blank**.

1. On the table card, select the ellipsis (**...**), and then select **Properties**.

1. In the **Edit table** panel, enter the following values, leave **Plural display name** as it's generated, and then select **Save**.

    | Field | Value |
    |---|---|
    | Display name | `Returns` |
    | Description | `Return authorizations created by the Customer Support Rep Assistant.` |

1. On the table card, select the ellipsis (**...**), and then select **View data**.

1. Expand the placeholder column named **New column**, and then select **Edit column**. Change **Display name** to `Authorization number`, leave the data type as **Text**, and then save the column.

1. Select **+ New column** and add each of the following columns. For each column, enter the **Display name**, select the **Data type**, set the options listed, leave **Required** off, and then select **Save and exit**.

    | Display name | Data type | Options |
    |---|---|---|
    | `Customer email` | Single line of text | **Format**: Email |
    | `Order ID` | Single line of text | **Format**: Text |
    | `Reason` | Single line of text | **Format**: Text. Under **Advanced options**, set **Maximum character count** to `500`. |
    | `Condition` | Single line of text | **Format**: Text. Under **Advanced options**, set **Maximum character count** to `200`. |
    | `Expected refund` | Whole number | None |
    | `Status` | Choice | Choices: `Authorized`, `Approved`, `Rejected`, `Escalated` |
    | `Approved by` | Single line of text | **Format**: Email |

    > [!NOTE]
    > For a **Choice** column, enter each label in the **Choices** list and keep the numeric values that Dataverse generates. Leave **Selecting multiple choices is allowed** cleared and leave **Default choice** set to **None**.

    > [!TIP]
    > Before you save a column, you can expand **Advanced options** to see the **Schema name** that Dataverse generates, such as `sup_Customeremail`. The workflows in later exercises use these names. If a schema name doesn't start with `sup_`, confirm that **Customer Support Rep Assistant** is your preferred solution before you continue.

1. Return to the **Customer Support Rep Assistant** solution and confirm that **Returns** appears in the list of objects. If it doesn't, select **Add existing** > **Table**, select **Returns**, and then add it to the solution.

## Task 3 — Create the Orders, Customer Records, and Products tables

Repeat the steps in Task 2 for each of the following tables. Use the display name and description for the table, rename the primary column, and then add the listed columns.

### Orders table

| Field | Value |
|---|---|
| Display name | `Orders` |
| Description | `Customer orders. One row per order.` |
| Primary column display name | `Order ID` |

| Display name | Data type | Options |
|---|---|---|
| `Customer email` | Single line of text | **Format**: Email |
| `Customer name` | Single line of text | **Format**: Text |
| `Order date` | Date only | None |
| `SKU` | Single line of text | **Format**: Text |
| `Product name` | Single line of text | **Format**: Text |
| `Total` | Currency | None. If **Currency** isn't available, select **Decimal**. |
| `Status` | Single line of text | **Format**: Text |
| `Ship to` | Multiple lines of text | None |

### Customer Records table

| Field | Value |
|---|---|
| Display name | `Customer Records` |
| Description | `Customer profile records. One row per customer.` |
| Primary column display name | `Customer email` |

| Display name | Data type | Options |
|---|---|---|
| `Customer name` | Single line of text | **Format**: Text |
| `Preferred channel` | Choice | Choices: `Email`, `Phone`, `SMS` |
| `Loyalty tier` | Choice | Choices: `Bronze`, `Silver`, `Gold` |
| `Notes` | Multiple lines of text | None |

### Products table

| Field | Value |
|---|---|
| Display name | `Products` |
| Description | `Product catalog. One row per SKU.` |
| Primary column display name | `SKU` |

| Display name | Data type | Options |
|---|---|---|
| `Product name` | Single line of text | **Format**: Text |
| `Category` | Single line of text | **Format**: Text |
| `Wattage` | Whole number | None |
| `Warranty months` | Whole number | None |
| `Compatibility notes` | Multiple lines of text | None |

## Task 4 — Import the sample data

Import the sample rows into the Orders, Customer Records, and Products tables. The column headings in each file match the column display names you created, so the columns map automatically.

1. In the **Customer Support Rep Assistant** solution, open the **Orders** table.

1. On the command bar, select **Import** > **Import data from Excel**. If you're prompted to select tables, select **Orders**, and then select **Next**.

1. Select **Upload**, browse to the `orders.csv` file that you downloaded, and then select **Open**.

1. When **Mapping status** shows **Mapping was successful**, select **Import**.

    > [!NOTE]
    > If the mapping status shows a warning or an error, select **Map columns**, map each column in the file to the column with the same name, and then select **Save changes**.

1. Wait for the import to finish. The total number of inserted rows appears when it's done.

1. Repeat these steps to import `customer-records.csv` into the **Customer Records** table and `products.csv` into the **Products** table.

## Task 5 — Confirm the starting state

Confirm that everything is in place before you author workflows. If something's missing, fix it now so that the workflows you author have tables to read from and write to.

1. In the **Customer Support Rep Assistant** solution, on the command bar, select **Publish all customizations**.

1. Open the **Returns** table. Confirm that the primary column is **Authorization number** and that the table has seven other columns: **Customer email**, **Order ID**, **Reason**, **Condition**, **Expected refund**, **Status**, and **Approved by**. The table has no rows. The first workflow you author writes the first row.

1. Open the **Orders** table and confirm that it contains order **9876** for `priya@contoso.com`.

1. Open the **Customer Records** table and confirm that it contains a row for `priya@contoso.com` with preferred channel **Email** and loyalty tier **Gold**.

1. Open the **Products** table and confirm that it contains SKU **BLD-100** (**Countertop Blender (700W)**, 24-month warranty).

> [!NOTE]
> The first time you use a connector (**Office 365 Outlook**, **Microsoft Dataverse**, or **Approvals**) in this environment, Power Automate prompts you to sign in and asks for an optional **display name** for the connection. Leave the display name blank so that Power Automate names it, or enter a short name like `Outlook - AB-620`. This prompt appears once per connector for each user in an environment. If it doesn't appear when you configure an action later in this lab, the connection already exists.

You've created the four Dataverse tables and imported the sample data that the remaining labs use. In the next exercise, you author the return-request workflow.
