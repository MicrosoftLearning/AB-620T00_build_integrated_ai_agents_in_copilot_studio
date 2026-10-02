# Create the AB-620 Dataverse tables manually

Use this fallback only if you cannot import the additive solution in [Lab 2, Exercise 0](../lab-2-add-workflows-to-agent/exercise-0-setup.md). Creating all four tables by hand takes substantially longer than importing the solution. Keep the agent and **Customer Support Rep Assistant** solution you made in Lab 1; don't create a second agent or change its publisher.

## Before you start

Open the **Customer Support Rep Assistant** solution and confirm its publisher is **Support** with prefix `sup`. Make it your preferred solution. You need permission to create Dataverse tables in the environment. Use the exact table and column display names below so the later workflow steps can find the columns.

## Create each table in the solution

1. In Copilot Studio, select the ellipsis (**...**) next to your account name, select **Solutions**, and open **Customer Support Rep Assistant**.
1. Select **+ New** > **Table** > **Tables**, then **Start from blank**.
1. On the table card, select **...** > **Properties**. Enter the table's display name and description from the sections below. Keep the generated plural name, then select **Save**.
1. On the table card, select **...** > **View data**. Expand **New column**, select **Edit column**, set its display name to the specified *primary column*, and save it as text.
1. Use **+ New column** for the remaining columns. Leave **Required** off unless the interface requires a value. For single-line text, set **Format** to **Email** or **Text** as noted. For a choice, add the labels shown, keep the generated numeric values, leave multiple selection off, and set the default to **None**.
1. After saving the columns, return to the agent solution. Confirm the table is listed. If it isn't, use **+ Add existing** > **Table** and select it. Repeat for each of the four tables.

Check the column **Schema name** under **Advanced options** before saving. It must use the `sup_` prefix. Lab 2's return workflow and refund filter reference the **Authorization number** column. If a column has a different prefix or a structurally different name, correct it before proceeding.

### Returns

**Display name:** Returns. **Description:** Return authorizations created by the Customer Support Rep Assistant. **Primary column:** Authorization number (text).

| Additional column | Data type | Settings |
| --- | --- | --- |
| Customer email | Single line of text | Email format |
| Order ID | Single line of text | Text format |
| Reason | Single line of text | Text format; maximum 500 characters |
| Condition | Single line of text | Text format; maximum 200 characters |
| Expected refund | Whole number | No additional settings |
| Status | Choice | Authorized, Approved, Rejected, Escalated |
| Approved by | Single line of text | Email format |

The Returns table starts empty. Do not import a schema header file as records.

### Orders

**Display name:** Orders. **Description:** Customer orders. One row per order. **Primary column:** Order ID (text).

| Additional column | Data type | Settings |
| --- | --- | --- |
| Customer email | Single line of text | Email format |
| Customer name | Single line of text | Text format |
| Order date | Date only | No additional settings |
| SKU | Single line of text | Text format |
| Product name | Single line of text | Text format |
| Total | Currency | Use Decimal if Currency isn't available |
| Status | Single line of text | Text format |
| Ship to | Multiple lines of text | No additional settings |

### Customer Records

**Display name:** Customer Records. **Description:** Customer profile records. One row per customer. **Primary column:** Customer email (text).

| Additional column | Data type | Settings |
| --- | --- | --- |
| Customer name | Single line of text | Text format |
| Preferred channel | Choice | Email, Phone, SMS |
| Loyalty tier | Choice | Bronze, Silver, Gold |
| Notes | Multiple lines of text | No additional settings |

### Products

**Display name:** Products. **Description:** Product catalog. One row per SKU. **Primary column:** SKU (text).

| Additional column | Data type | Settings |
| --- | --- | --- |
| Product name | Single line of text | Text format |
| Category | Single line of text | Text format |
| Wattage | Whole number | No additional settings |
| Warranty months | Whole number | No additional settings |
| Compatibility notes | Multiple lines of text | No additional settings |

## Return to the sample-data import

After all four tables appear in **Customer Support Rep Assistant**, return to Task 3 in [Lab 2, Exercise 0](../lab-2-add-workflows-to-agent/exercise-0-setup.md). Import the three CSVs separately and complete the starting-state checks. Creating the schemas alone does not populate any rows.
