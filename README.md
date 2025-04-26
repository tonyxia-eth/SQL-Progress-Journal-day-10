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
This query calculates the average salary for each year, rounded to two decimal places. The results are sorted in descending order of year.

Task 2: Salary of a Specific Player

SELECT year, salary
FROM salaries
WHERE player_id = (SELECT id FROM players WHERE first_name = 'Cal' AND last_name = 'Ripken')
ORDER BY year DESC;
This query retrieves the salary of 'Cal Ripken' in each year, sorted in descending order of year.

Task 3: Home Runs by Player

SELECT p.year, p.HR
FROM performances p
JOIN players pl ON p.player_id = pl.id
WHERE pl.first_name = 'Ken' AND pl.last_name = 'Griffey' AND pl.birth_year = 1969
ORDER BY p.year DESC;
This query returns the home runs (HR) hit by 'Ken Griffey Jr.' for each year, sorted by year.

Task 4: Player Salaries in 2001

SELECT pl.first_name, pl.last_name, s.salary
FROM salaries s
JOIN players pl ON s.player_id = pl.id
WHERE s.year = 2001
ORDER BY s.salary ASC, pl.first_name ASC, pl.last_name ASC, pl.id ASC
LIMIT 50;
This query retrieves the top 50 player salaries in 2001, ordered by salary, and then by player name and ID.

Task 5: Distinct Teams Played For by Satchel Paige

SELECT DISTINCT t.name
FROM players p
JOIN performances pf ON p.id = pf.player_id
JOIN teams t ON pf.team_id = t.id
WHERE p.first_name = 'Satchel' AND p.last_name = 'Paige';
This query finds the distinct teams that Satchel Paige played for during his career.

Task 6: Total Hits by Team in 2001

SELECT t.name, SUM(pf.H) AS "total hits"
FROM performances pf
JOIN teams t ON pf.team_id = t.id
WHERE pf.year = 2001
GROUP BY t.name
ORDER BY "total hits" DESC
LIMIT 5;
This query calculates the total hits for each team in 2001 and displays the top 5 teams with the most hits.

Task 7: Player with Maximum Salary

SELECT pl.first_name, pl.last_name
FROM players pl
JOIN salaries s ON pl.id = s.player_id
WHERE s.salary = (SELECT MAX(salary) FROM salaries);
This query returns the player(s) with the highest salary.

Task 8: Player with Maximum Home Runs in 2001

SELECT players.first_name, players.last_name, salaries.salary
FROM players
JOIN salaries ON players.id = salaries.player_id
JOIN performances ON players.id = performances.player_id
WHERE salaries.year = 2001 AND performances.year = 2001
AND performances.HR = (SELECT MAX(HR) FROM performances WHERE year = 2001);
This query identifies the player(s) who hit the most home runs in 2001, along with their salaries.

Task 9: Average Salary by Team in 2001

SELECT t.name, ROUND(AVG(s.salary), 2) AS "average salary"
FROM salaries s
JOIN teams t ON s.team_id = t.id
WHERE s.year = 2001
GROUP BY t.name
ORDER BY "average salary" ASC
LIMIT 5;
This query calculates the average salary by team in 2001, ordered by the lowest average salary.

Task 10: Players’ Salaries, Home Runs, and Year

SELECT pl.first_name, pl.last_name, s.salary, pf.HR, s.year
FROM players pl
JOIN salaries s ON pl.id = s.player_id
JOIN performances pf ON pl.id = pf.player_id AND s.year = pf.year
ORDER BY pl.id ASC, s.year DESC, pf.HR DESC, s.salary DESC;
This query shows the salary, home runs, and year for each player, ordered by player ID, year, home runs, and salary.

Task 11: Dollars per Hit in 2001

SELECT pl.first_name, pl.last_name, ROUND(s.salary * 1.0 / pf.H, 2) AS "dollars per hit"
FROM players pl
JOIN salaries s ON pl.id = s.player_id
JOIN performances pf ON pl.id = pf.player_id
WHERE s.year = 2001 AND pf.year = 2001 AND pf.H > 0
ORDER BY "dollars per hit" ASC, pl.first_name ASC, pl.last_name ASC
LIMIT 10;
This query calculates the "dollars per hit" for each player in 2001, sorted in ascending order.

Task 12: Players Who Excelled in Both Hits and RBIs in 2001

WITH per_hit AS (
    SELECT s.player_id
    FROM salaries s
    JOIN performances pf ON s.player_id = pf.player_id
    WHERE s.year = 2001 AND pf.year = 2001 AND pf.H > 0
    ORDER BY s.salary * 1.0 / pf.H ASC
    LIMIT 10
),
per_rbi AS (
    SELECT s.player_id
    FROM salaries s
    JOIN performances pf ON s.player_id = pf.player_id
    WHERE s.year = 2001 AND pf.year = 2001 AND pf.RBI > 0
    ORDER BY s.salary * 1.0 / pf.RBI ASC
    LIMIT 10
)
SELECT pl.first_name, pl.last_name
FROM players pl
WHERE pl.id IN (
    SELECT player_id FROM per_hit
    INTERSECT
    SELECT player_id FROM per_rbi
)
ORDER BY pl.last_name;
This query identifies players who excelled in both hits and RBIs in 2001 by intersecting two datasets — one for players with the most "dollars per hit" and one for players with the most "dollars per RBI".

Conclusion
This project significantly enhanced my SQL skills, particularly in areas like aggregation, filtering, and joins. I’ve gained a deeper understanding of how to efficiently extract and analyze data from relational databases, and I’m looking forward to applying these skills in future projects.
