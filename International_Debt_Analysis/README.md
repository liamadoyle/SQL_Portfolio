# Analysis of International Debt Data

## Project Overview

This project involves analyzing debt data provided by The World Bank, focusing on the debt owed by developing countries across various categories. The objective is to identify the number of distinct countries in the dataset, determine which country has the highest total debt, and find which country has the lowest amount of repayments. The analysis offers insights into global debt distribution and repayment patterns among developing nations.

## Data Description

The analysis is based on the following dataset:

**Table: `international_debt`**

| Column             | Data Type | Description                                          |
|--------------------|-----------|------------------------------------------------------|
| `country_name`     | VARCHAR   | Name of the country.                                 |
| `country_code`     | VARCHAR   | Code representing the country.                       |
| `indicator_name`   | VARCHAR   | Description of the debt indicator.                   |
| `indicator_code`   | VARCHAR   | Code representing the debt indicator.                |
| `debt`             | FLOAT     | Value of the debt indicator for the given country (in current US dollars). |

## Objectives

1. **Identify the Number of Distinct Countries:** Calculate how many unique countries are represented in the dataset.
2. **Determine the Country with the Highest Total Debt:** Identify which country owes the most debt.
3. **Find the Country with the Lowest Repayments:** Determine which country has made the lowest repayments.

## Key SQL Queries

### Identify the Number of Distinct Countries
This query calculates the number of unique countries in the dataset.

```sql
SELECT COUNT(DISTINCT country_name) AS distinct_countries
FROM international_debt;
```

### Determine the Country with the Highest Total Debt
This query identifies the country with the highest total debt.

```sql
SELECT country_name, SUM(debt) AS total_debt
FROM international_debt
GROUP BY country_name
ORDER BY total_debt DESC
LIMIT 1;
```

### Find the Country with the Lowest Repayments
This query finds the country with the lowest repayments.

```sql
SELECT country_name, SUM(debt) AS total_repayment
FROM international_debt
WHERE indicator_name LIKE '%repayment%'
GROUP BY country_name
ORDER BY total_repayment ASC
LIMIT 1;
```

## Results and Insights

- **Global Debt Distribution:** The analysis revealed the distribution of debt across different countries, highlighting significant debt holders among developing nations.
- **Repayment Patterns:** The data showed that certain countries had notably low repayment amounts, indicating potential financial challenges or strategic debt management.

## Conclusion

This analysis provides valuable insights into the debt landscape of developing countries. By understanding which countries hold the most debt and which have made the fewest repayments, policymakers and financial institutions can better assess global financial risks and opportunities.

## Technologies Used

- **SQL**: Used for data querying, filtering, grouping, and aggregation.
- **Jupyter Notebook**: Used to write and run SQL queries in an interactive environment.

## How to Run the Project

1. Clone the repository to your local machine.
2. Navigate to the `International_Debt_Data` folder.
3. Open the `.ipynb` file in Jupyter Notebook.
4. Run the notebook cells to see the SQL queries and results.

## Contact

For any questions or feedback, please reach out to me via [email](mailto:ld19rk@brocku.ca) or connect with me on [LinkedIn](https://www.linkedin.com/in/liam-doyle-6b88a12a4).

---

Thank you for exploring my SQL project!
