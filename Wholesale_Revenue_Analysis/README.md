# Wholesale Revenue Analysis for Motorcycle Parts Company

## Project Overview

This project involves analyzing sales data for a company that sells motorcycle parts through three warehouses. The objective is to calculate the net revenue by product line, grouped by month and warehouse, focusing solely on wholesale orders. The analysis provides valuable insights into revenue trends across different warehouses and product lines over time, helping the company's board of directors make informed decisions.

## Data Description

The analysis is based on the following dataset:

**Table: `sales`**

| Column         | Data Type | Description                                           |
|----------------|-----------|-------------------------------------------------------|
| `order_number` | VARCHAR   | Unique order number.                                  |
| `date`         | DATE      | Date of the order, from June to August 2021.          |
| `warehouse`    | VARCHAR   | The warehouse that the order was made from—North, Central, or West. |
| `client_type`  | VARCHAR   | Whether the order was Retail or Wholesale.            |
| `product_line` | VARCHAR   | Type of product ordered.                              |
| `quantity`     | INT       | Number of products ordered.                           |
| `unit_price`   | FLOAT     | Price per product (in dollars).                       |
| `total`        | FLOAT     | Total price of the order (in dollars).                |
| `payment`      | VARCHAR   | Payment method—Credit card, Transfer, or Cash.        |
| `payment_fee`  | FLOAT     | Percentage of total charged due to the payment method.|

## Objectives

1. **Calculate Net Revenue:** Determine the net revenue for each product line by subtracting payment fees from the total order value.
2. **Group Data by Month and Warehouse:** Aggregate the net revenue by product line, grouped by month and warehouse, to identify trends over time.
3. **Analyze Revenue Trends Using Advanced SQL Techniques:** Incorporate advanced SQL queries such as rolling averages, year-over-year growth analysis, and common table expressions (CTEs) to gain deeper insights into revenue trends.

## Key SQL Queries

### Calculate Net Revenue by Product Line

```sql
SELECT product_line, 
	CASE
		WHEN EXTRACT(MONTH FROM date) = 6 THEN 'June'
		WHEN EXTRACT(MONTH FROM date) = 7 THEN 'July'
		WHEN EXTRACT(MONTH FROM date) = 8 THEN 'August'
		ELSE 'Other'
	END AS month,
	warehouse, 
	SUM(total - payment_fee) AS net_revenue
FROM sales
WHERE client_type = 'Wholesale'
GROUP BY product_line, month, warehouse
ORDER BY product_line, month, net_revenue DESC
```
### Calculate Rolling Average Revenue Over Three Months

```sql
WITH monthly_revenue AS (
    SELECT 
        EXTRACT(MONTH FROM date) AS month,
        EXTRACT(YEAR FROM date) AS year,
        SUM(total - (total * payment_fee / 100)) AS net_revenue
    FROM sales
    WHERE client_type = 'Wholesale'
    GROUP BY year, month
)
SELECT
    year,
    month,
    AVG(net_revenue) OVER (ORDER BY year, month ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS rolling_avg_revenue
FROM monthly_revenue
ORDER BY year, month;
```
### Analyze Year-Over-Year Growth by Product Line

```sql
WITH yearly_revenue AS (
    SELECT 
        product_line,
        EXTRACT(YEAR FROM date) AS year,
        SUM(total - (total * payment_fee / 100)) AS net_revenue
    FROM sales
    WHERE client_type = 'Wholesale'
    GROUP BY product_line, year
)
SELECT
    product_line,
    year,
    net_revenue,
    LAG(net_revenue, 1) OVER (PARTITION BY product_line ORDER BY year) AS previous_year_revenue,
    ROUND((net_revenue - LAG(net_revenue, 1) OVER (PARTITION BY product_line ORDER BY year)) / LAG(net_revenue, 1) OVER (PARTITION BY product_line ORDER BY year) * 100, 2) AS yoy_growth
FROM yearly_revenue
ORDER BY product_line, year;
```

### Identify Top 3 Warehouses by Total Revenue Using a Common Table Expression (CTE)

```sql
WITH warehouse_revenue AS (
    SELECT
        warehouse,
        SUM(total - (total * payment_fee / 100)) AS total_revenue
    FROM sales
    WHERE client_type = 'Wholesale'
    GROUP BY warehouse
)
SELECT
    warehouse,
    total_revenue
FROM warehouse_revenue
ORDER BY total_revenue DESC
LIMIT 3;
```

## Results and Insights

- **Revenue Trends:** The analysis identified significant revenue trends across different product lines, months, and warehouses.
- **Advanced Analysis:** The use of rolling averages, year-over-year growth analysis, and CTEs provided deeper insights into revenue trends, allowing for a more nuanced understanding of business performance.
- **Warehouse Performance:** Some warehouses consistently outperformed others, highlighting opportunities for resource allocation and strategy adjustment.
- **Product Line Analysis:** Certain product lines generated higher revenue, suggesting potential focus areas for marketing and inventory management.

## Conclusion

The results of this analysis provide actionable insights into the performance of different warehouses and product lines, enabling the company's board of directors to make data-driven decisions. The focus on wholesale orders and the use of advanced SQL techniques ensures that the analysis aligns with the company’s strategic goals.

## Technologies Used

- **SQL**: Used for data querying, filtering, grouping, and advanced analysis with window functions, CTEs, and set theory.
- **Jupyter Notebook**: Used to write and run SQL queries in an interactive environment.

## How to Run the Project

1. Clone the repository to your local machine.
2. Navigate to the `Wholesale_Revenue_Analysis` folder.
3. Open the `.ipynb` file in Jupyter Notebook.
4. Run the notebook cells to see the SQL queries and results.

## Contact

For any questions or feedback, please reach out to me via [email](mailto:ld19rk@brocku.ca) or connect with me on [LinkedIn](https://www.linkedin.com/in/liam-doyle-6b88a12a4).

---

Thank you for exploring my SQL project!
