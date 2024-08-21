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
3. **Focus on Wholesale Orders:** Filter the data to include only wholesale orders, as this analysis is focused on the wholesale segment of the business.

## Key SQL Queries

### Calculate Net Revenue by Product Line
This query calculates the net revenue for each product line by subtracting payment fees from the total order value. The data is grouped by product line, month, and warehouse, focusing on wholesale orders only.

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

## Results and Insights

- **Revenue Trends:** The analysis identified significant revenue trends across different product lines, months, and warehouses.
- **Warehouse Performance:** Some warehouses consistently outperformed others, highlighting opportunities for resource allocation and strategy adjustment.
- **Product Line Analysis:** Certain product lines generated higher revenue, suggesting potential focus areas for marketing and inventory management.

## Conclusion

The results of this analysis provide actionable insights into the performance of different warehouses and product lines, enabling the company's board of directors to make data-driven decisions. The focus on wholesale orders ensures that the analysis aligns with the company’s strategic goals.

## Technologies Used

- **SQL**: Used for data querying, filtering, grouping, and aggregation.
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
