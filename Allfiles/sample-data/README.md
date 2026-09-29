# Sample data for AB-620 v2 labs

This folder contains the sample data that the labs use. The rows support the **Priya, order 9876, and SKU BLD-100** storyline that the test prompts follow from Lab 2 through Lab 5.

You import these files in [Lab 2, Exercise 0 — Set up the Dataverse tables](../../Instructions/Labs/lab-2-add-workflows-to-agent/exercise-0-setup.md). The column headings in each CSV file match the display names of the Dataverse columns you create in that exercise, so the columns map automatically when you import.

## Files

| File | Dataverse table | Primary column | Used in |
|---|---|---|---|
| `orders.csv` | Orders | Order ID | Lab 3 (Orders tool), Lab 4 (order 9876 lookup), Lab 5 (evaluation) |
| `customer-records.csv` | Customer Records | Customer email | Lab 3 (Customer Records tool) |
| `products.csv` | Products | SKU | Lab 4 (Fulfillment agent Products tool) |
| `faq-content.md` | Not applicable | Not applicable | Reference copy of the FAQ text that the Lab 1 `faq-lookup` skill answers from |

The **Returns** table has no sample data file. The **Create return authorization** workflow in Lab 2 adds its rows.

## Fictitious content

All customer names, email addresses, and orders in these files are fictitious. The email addresses are text values that the agent looks up. The labs don't send email to them.

