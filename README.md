# SQL Learning Journey: Moneyball Project (CS50) - day 10 - task completed

## Overview
In this project, I worked with a dataset of Major League Baseball (MLB) players, teams, performances, and salaries. The project focused on developing skills in querying databases using SQL, covering essential concepts such as **aggregation, filtering, sorting, joins**, and **grouping** data.

### Key Concepts Covered

1. **Aggregation Functions**:
   - `AVG()`: Calculating average values (e.g., average salary)
   - `ROUND()`: Rounding numbers to a specified number of decimal places
   - `COUNT()`, `SUM()`, `MIN()`, `MAX()`: Other common aggregation functions
   
2. **Grouping and Sorting Data**:
   - `GROUP BY`: Grouping data based on columns (e.g., by year or team)
   - `ORDER BY`: Sorting results (e.g., by year or team ID)
   
3. **Filtering Data**:
   - `WHERE`: Filtering data based on conditions (e.g., filtering by year or salary)
   
4. **Joins**:
   - Combining data from multiple tables (e.g., combining salaries with players’ data)

---

### My Work Process: Task-by-Task Breakdown

task 1 

SELECT year, ROUND(AVG(salary), 2) AS "average salary"
FROM salaries
GROUP BY year
ORDER BY year DESC;

Task 2

SELECT year, salary
FROM salaries
WHERE player_id = (
    SELECT id FROM players
    WHERE first_name = 'Cal' AND last_name = 'Ripken'
)
ORDER BY year DESC;

task 3

SELECT p.year, p.HR
FROM performances p
JOIN players pl ON p.player_id = pl.id
WHERE pl.first_name = 'Ken'
  AND pl.last_name = 'Griffey'
  AND pl.birth_year = 1969
ORDER BY p.year DESC;

task 4

SELECT pl.first_name, pl.last_name, s.salary
FROM salaries s
JOIN players pl ON s.player_id = pl.id
WHERE s.year = 2001
ORDER BY s.salary ASC, pl.first_name ASC, pl.last_name ASC, pl.id ASC
LIMIT 50;


task 5

SELECT DISTINCT t.name
FROM players p
JOIN performances pf ON p.id = pf.player_id
JOIN teams t ON pf.team_id = t.id
WHERE p.first_name = 'Satchel' AND p.last_name = 'Paige';

Task 6

SELECT t.name, SUM(pf.H) AS "total hits"
FROM performances pf
JOIN teams t ON pf.team_id = t.id
WHERE pf.year = 2001
GROUP BY t.name
ORDER BY "total hits" DESC
LIMIT 5;

task 7

SELECT pl.first_name, pl.last_name
FROM players pl
JOIN salaries s ON pl.id = s.player_id
WHERE s.salary = (SELECT MAX(salary) FROM salaries);


task 8

SELECT players.first_name, players.last_name, salaries.salary
FROM players
JOIN salaries ON players.id = salaries.player_id
JOIN performances ON players.id = performances.player_id
WHERE salaries.year = 2001
  AND performances.year = 2001
  AND performances.HR = (
      SELECT MAX(HR)
      FROM performances
      WHERE year = 2001
  );


task 9

SELECT t.name, ROUND(AVG(s.salary), 2) AS "average salary"
FROM salaries s
JOIN teams t ON s.team_id = t.id
WHERE s.year = 2001
GROUP BY t.name
ORDER BY "average salary" ASC
LIMIT 5;


task 10

SELECT pl.first_name, pl.last_name, s.salary, pf.HR, s.year
FROM players pl
JOIN salaries s ON pl.id = s.player_id
JOIN performances pf ON pl.id = pf.player_id AND s.year = pf.year
ORDER BY pl.id ASC, s.year DESC, pf.HR DESC, s.salary DESC;


task 11

SELECT pl.first_name, pl.last_name,
       ROUND(s.salary * 1.0 / pf.H, 2) AS "dollars per hit"
FROM players pl
JOIN salaries s ON pl.id = s.player_id
JOIN performances pf ON pl.id = pf.player_id
WHERE s.year = 2001 AND pf.year = 2001 AND pf.H > 0
ORDER BY "dollars per hit" ASC, pl.first_name ASC, pl.last_name ASC
LIMIT 10;


task 12


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

