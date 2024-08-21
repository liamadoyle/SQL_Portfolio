# Analyzing Mental Health Risks Among International Students

## Project Overview

This project explores data from a Japanese international university's 2018 student survey to assess the mental health risks faced by international students. The analysis focuses on the relationship between social connectedness, acculturative stress, and depression, as well as the impact of the length of stay on these mental health outcomes. The project aims to replicate the study's findings, which suggest that international students are at higher risk for mental health difficulties.

## Data Description

The analysis is based on the following dataset:

**Table: `students`**

| Column      | Data Type | Description                                               |
|-------------|-----------|-----------------------------------------------------------|
| `id`        | VARCHAR   | Unique identifier for each student.                       |
| `stay`      | VARCHAR   | Length of stay in Japan (e.g., 1 year, 2 years).          |
| `inter_dom` | VARCHAR   | Student type—International (Inter) or Domestic (Dom).     |
| `todep`     | NUMERIC   | Total depression score (PHQ).                             |
| `tosc`      | NUMERIC   | Total social connectedness score (SCS).                   |
| `toas`      | NUMERIC   | Total acculturative stress score (AS).                    |

## Objectives

1. **Analyze the Impact of Length of Stay:** Determine how the length of stay impacts depression, social connectedness, and acculturative stress scores for international students.
2. **Correlation Analysis:** Calculate correlations between depression, social connectedness, and acculturative stress to understand their relationships.
3. **Regression Analysis:** Perform a regression analysis to identify the influence of social connectedness and acculturative stress on depression scores.

## Key SQL Queries

### View the Data

```sql
-- Run this code to view the data in students
SELECT * 
FROM students;
```

### Comparison of Average Scores for Depression, Social Connectedness, Acculturative Stress Across Length of Stays

```sql
SELECT stay,
	COUNT(*) AS count_int,
	ROUND(AVG(todep), 2) AS average_phq,
	ROUND(AVG(tosc), 2) AS average_scs,
	ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

### Correlation Analysis Between Depression, Social Connectedness, and Acculturative Stress

```sql
SELECT 
    ROUND(CORR(todep, tosc), 2) AS corr_depression_social_connectedness,
    ROUND(CORR(todep, toas), 2) AS corr_depression_acculturative_stress,
    ROUND(CORR(tosc, toas), 2) AS corr_social_connectedness_acculturative_stress
FROM 
    students
WHERE 
    inter_dom = 'Inter';
```

### Regression Analysis of Depression Scores

```sql
WITH regression_data AS (
    SELECT 
        todep,
        tosc,
        toas,
        AVG(todep) OVER () AS avg_dep,
        AVG(tosc) OVER () AS avg_scs,
        AVG(toas) OVER () AS avg_as
    FROM 
        students
    WHERE 
        inter_dom = 'Inter'
)
SELECT 
    SUM((tosc - avg_scs) * (todep - avg_dep)) / SUM(POWER(tosc - avg_scs, 2)) AS scs_coefficient,
    SUM((toas - avg_as) * (todep - avg_dep)) / SUM(POWER(toas - avg_as, 2)) AS as_coefficient
FROM 
    regression_data;
```

## Results and Insights

- **Length of Stay Impact:** Longer stays are associated with varying levels of depression, social connectedness, and acculturative stress among international students.
- **Correlation Insights:** Significant correlations were found between depression, social connectedness, and acculturative stress, indicating strong interdependencies.
- **Regression Analysis:** The regression analysis revealed how much of the variance in depression scores can be explained by social connectedness and acculturative stress.

## Conclusion

This analysis provides a deeper understanding of the mental health challenges faced by international students. The insights gained from correlation and regression analyses can help universities and policymakers develop better support systems for international students.

## Technologies Used

- **SQL**: Used for data querying, filtering, grouping, correlation, and regression analysis.
- **Jupyter Notebook**: Used to write and run SQL queries in an interactive environment.

## How to Run the Project

1. Clone the repository to your local machine.
2. Navigate to the `Student_Mental_Health` folder.
3. Open the `.ipynb` file in Jupyter Notebook.
4. Run the notebook cells to see the SQL queries and results.

## Contact

For any questions or feedback, please reach out to me via [email](mailto:ld19rk@brocku.ca) or connect with me on [LinkedIn](https://www.linkedin.com/in/liam-doyle-6b88a12a4).

---

Thank you for exploring my SQL project!
