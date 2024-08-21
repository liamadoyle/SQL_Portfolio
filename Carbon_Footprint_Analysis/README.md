# Analysis of Product Carbon Footprints Across Industries

## Project Overview

This project involves analyzing product carbon footprints (PCFs) across various industries using data provided by multiple companies. The objective is to examine the greenhouse gas emissions associated with different stages of product production, such as upstream, operations, and downstream processes. By analyzing this data, we can identify which industries contribute the most to global emissions and suggest opportunities for reducing environmental impact.

## Data Description

The analysis is based on the following dataset:

**Table: `product_emissions`**

| Column                    | Data Type | Description                                                    |
|---------------------------|-----------|----------------------------------------------------------------|
| `id`                      | VARCHAR   | Unique identifier for each record.                             |
| `year`                    | INT       | Year of the product's emission data.                           |
| `product_name`            | VARCHAR   | Name of the product.                                           |
| `company`                 | VARCHAR   | Name of the company that produced the product.                 |
| `country`                 | VARCHAR   | Country where the product was produced.                        |
| `industry_group`          | VARCHAR   | Industry category of the product.                              |
| `weight_kg`               | NUMERIC   | Weight of the product (in kilograms).                          |
| `carbon_footprint_pcf`    | NUMERIC   | Carbon footprint of the product (in CO2 equivalent).           |
| `upstream_percent_total_pcf` | VARCHAR | Percentage of the total PCF attributed to upstream processes.  |
| `operations_percent_total_pcf` | VARCHAR | Percentage of the total PCF attributed to operational processes. |
| `downstream_percent_total_pcf` | VARCHAR | Percentage of the total PCF attributed to downstream processes.|

## Objectives

1. **Examine Carbon Footprints Across Industries:** Analyze the total carbon footprints of products across different industries.
2. **Identify Key Contributors:** Determine which industries and companies contribute the most to global emissions.
3. **Analyze Emissions by Production Stage:** Break down emissions by upstream, operational, and downstream processes to identify key areas for improvement.

## Key SQL Queries

### Calculate Total Industry Footprint for 2017

```sql
SELECT industry_group, COUNT(DISTINCT company) AS num_companies, ROUND(SUM(carbon_footprint_pcf), 1) AS total_industry_footprint
FROM product_emissions
WHERE year = '2017'
GROUP BY industry_group
ORDER BY total_industry_footprint DESC;
```

### Identify Key Contributors

```sql
SELECT
    company,
    industry_group,
    SUM(carbon_footprint_pcf) AS total_carbon_footprint
FROM
    product_emissions
GROUP BY
    company, industry_group
ORDER BY
    total_carbon_footprint DESC
LIMIT 10;
```

### Analyze Emissions by Production Stage

```sql
SELECT
    industry_group,
    AVG(CAST(upstream_percent_total_pcf AS NUMERIC)) AS avg_upstream_percent,
    AVG(CAST(operations_percent_total_pcf AS NUMERIC)) AS avg_operations_percent,
    AVG(CAST(downstream_percent_total_pcf AS NUMERIC)) AS avg_downstream_percent
FROM
    product_emissions
GROUP BY
    industry_group;
```

## Results and Insights

- **Industry Impact:** The analysis revealed significant differences in carbon footprints across industries, with certain sectors contributing disproportionately to global emissions.
- **Production Stage Analysis:** Upstream and downstream processes were identified as major contributors to carbon footprints, highlighting opportunities for companies to target these areas for emission reductions.

## Conclusion

This analysis provides a detailed understanding of the carbon footprint of products across industries, helping companies and policymakers identify areas where they can focus efforts to reduce greenhouse gas emissions. By targeting specific stages of production, there is significant potential to lower overall environmental impact.

## Technologies Used

- **SQL**: Used for data querying, filtering, grouping, and aggregation.
- **Jupyter Notebook**: Used to write and run SQL queries in an interactive environment.

## How to Run the Project

1. Clone the repository to your local machine.
2. Navigate to the `Product_Carbon_Footprint` folder.
3. Open the `.ipynb` file in Jupyter Notebook.
4. Run the notebook cells to see the SQL queries and results.

## Contact

For any questions or feedback, please reach out to me via [email](mailto:ld19rk@brocku.ca) or connect with me on [LinkedIn](https://www.linkedin.com/in/liam-doyle-6b88a12a4).

---

Thank you for exploring my SQL project!
