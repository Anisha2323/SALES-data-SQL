# Sales Data Analysis using SQL & Tableau

A sales analytics project using **MySQL and Tableau** to analyze revenue, sales quantity, customers, products, markets, and profitability for an India-based hardware sales dataset.

> **Project note:** This repository is based on a public learning project/tutorial. The repository has been reorganized for portfolio use, with the SQL analysis questions and queries retained and corrected for readability and syntax. Any original project attribution and references should be preserved when publishing.

## Project Overview

The objective is to transform raw sales data into an analysis-ready data model and use SQL and Tableau to answer common business questions around revenue, customers, products, markets, and profit.

### Business Problem

The sales team needs a dashboard that helps them understand:

- Revenue performance across cities and markets
- Revenue trends across years and months
- Top customers by revenue and sales quantity
- Top products by revenue
- Net profit and profit margin by market

## Tools & Technologies

- **MySQL** — data storage, querying, joins, aggregation, and analysis
- **Tableau** — data modeling, visualization, dashboards, and business analysis
- **Excel** — source-data inspection / alternative data source
- **SQL** — filtering, joins, `DISTINCT`, `COUNT`, `SUM`, and date-based analysis

## Dataset

The database contains the following main tables:

- `customers` — customer code, customer name, and customer type
- `date` — date, year, month, and calendar attributes
- `markets` — market code, market name, and related market information
- `products` — product information
- `transactions` — sales transactions, quantities, sales amounts, currency, profit margin, profit, and cost price

The SQL database dump is available in [`database/sales_database.sql`](database/sales_database.sql).

## Data Model

The project uses a sales fact table connected to supporting dimension tables. The resulting model is used to support analysis in Tableau.

![Star Schema](images/star_schema.png)

## Project Workflow

![Data Flow](images/data_flow.jpg)

The overall workflow is:

1. Load the sales database.
2. Explore and validate the tables using SQL.
3. Join transaction data with the date and other dimension tables when required.
4. Prepare the data model for analysis.
5. Connect Tableau to the data.
6. Build revenue and profit dashboards.
7. Use the dashboards to identify trends and business insights.

## SQL Analysis — Questions & Queries

The following questions were used for SQL-based exploration of the sales database.

### 1. Show all customer records

```sql
SELECT *
FROM customers;
```

### 2. Show the total number of customers

```sql
SELECT COUNT(*) AS total_customers
FROM customers;
```

### 3. Show transactions for the Chennai market

The market code for Chennai in this dataset is `Mark001`.

```sql
SELECT *
FROM transactions
WHERE market_code = 'Mark001';
```

### 4. Show distinct product codes sold in Chennai

```sql
SELECT DISTINCT product_code
FROM transactions
WHERE market_code = 'Mark001';
```

### 5. Show transactions where the currency is US dollars

The source dump contains carriage-return characters in some currency values, so `TRIM()` is used to make the filter robust.

```sql
SELECT *
FROM transactions
WHERE TRIM(currency) = 'USD';
```

### 6. Show transactions from 2020 by joining the transaction and date tables

```sql
SELECT t.*, d.*
FROM transactions AS t
INNER JOIN date AS d
    ON t.order_date = d.date
WHERE d.year = 2020;
```

### 7. Calculate total sales amount for 2020

Because the dataset contains both INR and USD records and does not provide an exchange-rate conversion table, this query reports the sum of the recorded `sales_amount` values rather than converting them into a common currency.

```sql
SELECT SUM(t.sales_amount) AS total_sales_amount
FROM transactions AS t
INNER JOIN date AS d
    ON t.order_date = d.date
WHERE d.year = 2020
  AND TRIM(t.currency) IN ('INR', 'USD');
```

### 8. Calculate total sales amount for January 2020

```sql
SELECT SUM(t.sales_amount) AS total_sales_amount
FROM transactions AS t
INNER JOIN date AS d
    ON t.order_date = d.date
WHERE d.year = 2020
  AND d.month_name = 'January'
  AND TRIM(t.currency) IN ('INR', 'USD');
```

### 9. Calculate total sales amount for Chennai in 2020

```sql
SELECT SUM(t.sales_amount) AS total_sales_amount
FROM transactions AS t
INNER JOIN date AS d
    ON t.order_date = d.date
WHERE d.year = 2020
  AND t.market_code = 'Mark001';
```

## Tableau Dashboards

The Tableau workbook is available in [`tableau/Sales_Insights_Tableau.twbx`](tableau/Sales_Insights_Tableau.twbx).

### Revenue Analysis

![Revenue Dashboard](images/revenue_dashboard.png)

### Profit Analysis

![Profit Dashboard](images/profit_dashboard.png)

## Key Analysis Areas

The dashboards focus on:

- Revenue by city / market
- Revenue trends over time
- Customer-level revenue and sales quantity
- Product-level revenue
- Market-level profit and profit margin

## Project Structure

```text
sales-data-sql-tableau-analysis/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── database/
│   └── sales_database.sql
│
├── tableau/
│   └── Sales_Insights_Tableau.twbx
│
└── images/
    ├── data_flow.jpg
    ├── star_schema.png
    ├── revenue_dashboard.png
    └── profit_dashboard.png
```

## How to Run

### SQL

1. Install MySQL.
2. Create/import the database using `database/sales_database.sql`.
3. Open the `sales` database.
4. Run the SQL queries from the README or your SQL client.

### Tableau

1. Install Tableau Desktop or Tableau Public.
2. Open `tableau/Sales_Insights_Tableau.twbx`.
3. If Tableau asks for the original data connection, reconnect it to the local MySQL database or the packaged data source as appropriate.
4. Open the Revenue Analysis and Profit Analysis dashboards.

## References

The project was developed as a learning project using publicly available resources covering Tableau, SQL, OLTP/OLAP concepts, and star-schema data modeling. When publishing or modifying this repository, retain the original project's attribution where applicable.

## Portfolio Note

For resume/interview use, be prepared to explain the database schema, SQL queries, joins, revenue/profit calculations, Tableau data model, dashboard KPIs, and the business insights obtained from the visualizations.
