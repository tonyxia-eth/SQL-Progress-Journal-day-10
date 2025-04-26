CS50 SQL Project: Moneyball – Database Analysis of MLB Players, Teams, and Salaries
Overview
In this project, I worked with a dataset that contained information on Major League Baseball (MLB) players, teams, performances, and salaries. The focus was on developing and enhancing my skills in querying databases using SQL, with a particular emphasis on the following key concepts:

Aggregation

Filtering

Sorting

Joins

Grouping data

Through this project, I gained hands-on experience in extracting, aggregating, and organizing baseball data to generate insights. Below is a detailed breakdown of the tasks I completed, along with the SQL queries I used.

Key Concepts Covered
1. Aggregation Functions:
AVG(): Used to calculate the average value (e.g., average salary).

ROUND(): Used to round numeric results to a specified number of decimal places.

COUNT(), SUM(), MIN(), MAX(): Other common aggregation functions for summarizing data.

2. Grouping and Sorting Data:
GROUP BY: Used to group data by one or more columns (e.g., by year or team).

ORDER BY: Used to sort query results by specified columns (e.g., by team ID or year).

3. Filtering Data:
WHERE: Used to filter data based on conditions (e.g., filtering by salary or year).

4. Joins:
JOIN: Used to combine data from multiple tables (e.g., combining salaries with players' data).

Task-by-Task Breakdown
Task 1: Average Salary by Year


SELECT year, ROUND(AVG(salary), 2) AS "average salary"
FROM salaries
GROUP BY year
ORDER BY year DESC;
