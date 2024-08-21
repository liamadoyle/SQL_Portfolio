# EV Charging Station Usage Analysis for Apartment Buildings

## Project Overview

With the growing popularity of electric vehicles (EVs), apartment buildings are increasingly retrofitting their parking garages to include shared charging stations. However, with rising demand, competition for these stations can become an issue. This project analyzes EV charging habits to help apartment building managers better understand usage patterns and optimize the availability of shared charging stations for their tenants.

## Data Description

The analysis is based on the following dataset:

**Table: `charging_sessions`**

| Column               | Data Type | Description                                         |
|----------------------|-----------|-----------------------------------------------------|
| `garage_id`          | VARCHAR   | Identifier for the garage/building.                 |
| `user_id`            | VARCHAR   | Identifier for the individual user.                 |
| `user_type`          | VARCHAR   | Indicates whether the station is Shared or Private. |
| `start_plugin`       | DATETIME  | Date and time the session started.                  |
| `start_plugin_hour`  | NUMERIC   | The hour (in military time) the session started.    |
| `end_plugout`        | DATETIME  | Date and time the session ended.                    |
| `end_plugin_hour`    | NUMERIC   | The hour (in military time) the session ended.      |
| `duration_hours`     | NUMERIC   | Length of the session, in hours.                    |
| `el_kwh`             | NUMERIC   | Amount of electricity used (in Kilowatt hours).     |
| `month_plugin`       | VARCHAR   | Month the session started.                          |
| `weekdays_plugin`    | VARCHAR   | Day of the week the session started.                |

## Objectives

1. **Analyze Charging Duration and Usage Patterns:** Examine session duration, electricity consumption, and peak usage times to identify trends in charging habits.
2. **Compare Shared vs. Private Usage:** Investigate differences between shared and private charging stations to understand user behavior.
3. **Identify Peak Demand Periods:** Determine the times of day and days of the week when charging demand is highest.

## Key SQL Queries

### Examining User Information

```sql
SELECT garage_id, COUNT(DISTINCT user_id) AS num_unique_users
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY garage_id
ORDER BY num_unique_users DESC;
```

```sql
SELECT weekdays_plugin, start_plugin_hour, COUNT(user_id) AS num_charging_sessions
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY weekdays_plugin, start_plugin_hour
ORDER BY num_charging_sessions DESC
LIMIT 10;
```

```sql
SELECT user_id, AVG(duration_hours) AS avg_charging_duration
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY user_id
HAVING AVG(duration_hours) > 10
ORDER BY avg_charging_duration DESC;
```
### Analyze Charging Duration and Usage Patterns

```sql
SELECT
    user_type,
    AVG(duration_hours) AS avg_duration,
    AVG(el_kwh) AS avg_consumption
FROM
    charging_sessions
GROUP BY
    user_type;
```

### Compare Shared vs. Private Usage

```sql
SELECT
    user_type,
    COUNT(*) AS total_sessions,
    SUM(el_kwh) AS total_consumption
FROM
    charging_sessions
GROUP BY
    user_type;
```

### Identify Peak Demand Periods

```sql
SELECT
    weekdays_plugin,
    start_plugin_hour,
    COUNT(*) AS session_count
FROM
    charging_sessions
GROUP BY
    weekdays_plugin, start_plugin_hour
ORDER BY
    session_count DESC;
```

## Results and Insights

- **Usage Patterns:** The analysis revealed that shared charging stations have a higher average usage duration and electricity consumption compared to private stations.
- **Peak Demand:** Peak usage occurs during weekday evenings, highlighting the need for more charging availability during these times.
- **Optimization Opportunities:** Building managers can use this data to optimize the availability and distribution of charging stations, reducing tenant frustration and improving overall satisfaction.

## Conclusion

The analysis provides actionable insights for optimizing EV charging station usage in apartment buildings. By understanding peak demand periods and usage patterns, building managers can make informed decisions on how to better allocate charging resources.

## Technologies Used

- **SQL**: Used for data querying, filtering, grouping, and aggregation.
- **Jupyter Notebook**: Used to write and run SQL queries in an interactive environment.

## How to Run the Project

1. Clone the repository to your local machine.
2. Navigate to the `EV_Charging_Station_Usage` folder.
3. Open the `.ipynb` file in Jupyter Notebook.
4. Run the notebook cells to see the SQL queries and results.

## Contact

For any questions or feedback, please reach out to me via [email](mailto:ld19rk@brocku.ca) or connect with me on [LinkedIn](https://www.linkedin.com/in/liam-doyle-6b88a12a4).

---

Thank you for exploring my SQL project!
