# Exploring the Golden Age of Video Games

## Project Overview

In this project, I analyzed video game critic and user scores, along with sales data for the top 400 games released since 1977. The goal was to identify the "golden age" of video games by determining which release years were most favored by users and critics. Additionally, the project explored the business side of gaming by examining sales data. The analysis involved joining datasets, filtering, grouping, and ordering data to uncover key trends in the gaming industry.

## Data Description

The analysis is based on data from two main tables:

**Table 1: `game_sales`**

| Column    | Data Type | Description                          |
|-----------|-----------|--------------------------------------|
| `name`    | VARCHAR   | Name of the video game.              |
| `platform`| VARCHAR   | Gaming platform.                     |
| `publisher`| VARCHAR  | Game publisher.                      |
| `developer`| VARCHAR  | Game developer.                      |
| `games_sold`| FLOAT   | Number of copies sold (in millions). |
| `year`    | INT       | Release year.                        |

**Table 2: `reviews`**

| Column        | Data Type | Description                         |
|---------------|-----------|-------------------------------------|
| `name`        | VARCHAR   | Name of the video game.             |
| `critic_score`| FLOAT     | Critic score according to Metacritic. |
| `user_score`  | FLOAT     | User score according to Metacritic.  |

Additional tables used in the analysis include `users_avg_year_rating` and `critics_avg_year_rating`.

## Objectives

1. **Identify Best-Selling Games:** Analyze sales data to identify the top-selling games.
2. **Find Top Years by Critic Score:** Determine which years had the highest average critic scores for games released that year.
3. **Identify the Golden Age:** Compare critic and user ratings across years to identify the "golden age" of video games.

## Key SQL Queries

### Best-Selling Games
This query retrieves the top 10 best-selling games.

```sql
-- best_selling_games
SELECT * 
FROM game_sales 
ORDER BY games_sold DESC 
LIMIT 10;
```

### Top Years by Critic Score
This query identifies the top 10 years with the highest average critic scores for games released that year.

```sql
-- critics_top_ten_years
SELECT g.year, COUNT(DISTINCT g.name) AS num_games, ROUND(AVG(r.critic_score), 2) AS avg_critic_score
FROM game_sales AS g
INNER JOIN reviews AS r
ON g.name = r.name
GROUP BY g.year
HAVING COUNT(DISTINCT g.name) >= 4
ORDER BY avg_critic_score DESC
LIMIT 10;
```

### Identifying the Golden Age
This query compares critic and user ratings to find the years that could be considered the "golden age" of video games.

```sql
-- golden_years
SELECT u.year, u.num_games, avg_critic_score, avg_user_score, SUM(avg_critic_score - avg_user_score) AS diff
FROM users_avg_year_rating AS u
INNER JOIN critics_avg_year_rating AS c
ON u.year = c.year
WHERE avg_critic_score > 9 
    OR avg_user_score > 9
GROUP BY u.year, avg_critic_score, avg_user_score
ORDER BY u.year ASC;
```

## Results and Insights

- **Best-Selling Games:** The top-selling games were identified, providing insights into the commercial success of key titles.
- **Top Years by Critic Score:** Several years stood out with exceptionally high average critic scores, suggesting these could be considered part of the "golden age."
- **Golden Age Identification:** By analyzing the differences between critic and user ratings, specific years were identified as the "golden age" of video games, with exceptionally high ratings from both groups.

## Conclusion

This analysis provides a comprehensive look into the evolution of video games, highlighting the best-selling games, top-rated years, and identifying potential "golden age" periods. The insights gained can be valuable for understanding trends in the gaming industry and the factors that contribute to the success of video games.

## Technologies Used

- **SQL**: Used for data querying, filtering, grouping, and aggregation.
- **Jupyter Notebook**: Used to write and run SQL queries in an interactive environment.

## How to Run the Project

1. Clone the repository to your local machine.
2. Navigate to the `Golden_Age_Games` folder.
3. Open the `.ipynb` file in Jupyter Notebook.
4. Run the notebook cells to see the SQL queries and results.

## Contact

For any questions or feedback, please reach out to me via [email](mailto:ld19rk@brocku.ca) or connect with me on [LinkedIn](https://www.linkedin.com/in/liam-doyle-6b88a12a4).

---

Thank you for exploring my SQL project!
