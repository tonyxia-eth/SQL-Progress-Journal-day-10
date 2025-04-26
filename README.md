# ⚾ CS50 Moneyball SQL Project

## ✅ **Project Overview**
In this project, I worked with a dataset of Major League Baseball (MLB) players, teams, performances, and salaries.  
The project focused on developing skills in querying databases using SQL, covering essential concepts such as aggregation, filtering, sorting, joins, and grouping data.

---

## 🔑 **Key Concepts Covered**

### **Aggregation Functions:**
- **AVG()**: Calculating average values (e.g., average salary)
- **ROUND()**: Rounding numbers to a specified number of decimal places
- **COUNT(), SUM(), MIN(), MAX()**: Other common aggregation functions

### **Grouping and Sorting Data:**
- **GROUP BY**: Grouping data based on columns (e.g., by year or team)
- **ORDER BY**: Sorting results (e.g., by year or team ID)

### **Filtering Data:**
- **WHERE**: Filtering data based on conditions (e.g., filtering by year or salary)

### **Joins:**
- Combining data from multiple tables (e.g., combining salaries with players' data)

---

## 📝 **My Work Process: Task-by-Task Breakdown**

### ✅ **Task 1 – Average Salary by Year**
```sql
SELECT year, ROUND(AVG(salary), 2) AS "average salary" 
FROM salaries 
GROUP BY year 
ORDER BY year DESC;


SELECT year, salary 
FROM salaries 
WHERE player_id = (
    SELECT id 
    FROM players 
    WHERE first_name = 'Cal' AND last_name = 'Ripken'
) 
ORDER BY year DESC;

✅ Task 2 – Salary History of 'Cal Ripken'

SELECT year, salary 
FROM salaries 
WHERE player_id = (
    SELECT id 
    FROM players 
    WHERE first_name = 'Cal' AND last_name = 'Ripken'
) 
ORDER BY year DESC;

✅ Task 3 – Home Runs by 'Ken Griffey' (1969)

SELECT p.year, p.HR 
FROM performances p 
JOIN players pl ON p.player_id = pl.id 
WHERE pl.first_name = 'Ken' AND pl.last_name = 'Griffey' AND pl.birth_year = 1969 
ORDER BY p.year DESC;

✅ Task 4 – Salaries in 2001

SELECT pl.first_name, pl.last_name, s.salary 
FROM salaries s 
JOIN players pl ON s.player_id = pl.id 
WHERE s.year = 2001 
ORDER BY s.salary ASC, pl.first_name ASC, pl.last_name ASC, pl.id ASC 
LIMIT 50;

✅ Task 5 – Teams Satchel Paige Played For

SELECT DISTINCT t.name 
FROM players p 
JOIN performances pf ON p.id = pf.player_id 
JOIN teams t ON pf.team_id = t.id 
WHERE p.first_name = 'Satchel' AND p.last_name = 'Paige';

✅ Task 6 – Top 5 Teams by Total Hits (2001)

SELECT t.name, SUM(pf.H) AS "total hits" 
FROM performances pf 
JOIN teams t ON pf.team_id = t.id 
WHERE pf.year = 2001 
GROUP BY t.name 
ORDER BY "total hits" DESC 
LIMIT 5;

✅ Task 7 – Highest Paid Player in MLB History

SELECT pl.first_name, pl.last_name 
FROM players pl 
JOIN salaries s ON pl.id = s.player_id 
WHERE s.salary = (SELECT MAX(salary) FROM salaries);

✅ Task 8 – 2001 Home Run Leader and Their Salary

SELECT players.first_name, players.last_name, salaries.salary 
FROM players 
JOIN salaries ON players.id = salaries.player_id 
JOIN performances ON players.id = performances.player_id 
WHERE salaries.year = 2001 AND performances.year = 2001 
AND performances.HR = (SELECT MAX(HR) FROM performances WHERE year = 2001);

✅ Task 9 – Average Salary by Team (2001)

SELECT t.name, ROUND(AVG(s.salary), 2) AS "average salary" 
FROM salaries s 
JOIN teams t ON s.team_id = t.id 
WHERE s.year = 2001 
GROUP BY t.name 
ORDER BY "average salary" ASC 
LIMIT 5;

✅ Task 10 – Player Salaries and Home Runs (2001)

SELECT pl.first_name, pl.last_name, s.salary, pf.HR, s.year 
FROM players pl 
JOIN salaries s ON pl.id = s.player_id 
JOIN performances pf ON pl.id = pf.player_id AND s.year = pf.year 
ORDER BY pl.id ASC, s.year DESC, pf.HR DESC, s.salary DESC;

✅ Task 11 – Dollars Per Hit

SELECT pl.first_name, pl.last_name, 
ROUND(s.salary * 1.0 / pf.H, 2) AS "dollars per hit" 
FROM players pl 
JOIN salaries s ON pl.id = s.player_id 
JOIN performances pf ON pl.id = pf.player_id 
WHERE s.year = 2001 AND pf.year = 2001 AND pf.H > 0 
ORDER BY "dollars per hit" ASC, pl.first_name ASC, pl.last_name ASC 
LIMIT 10;

✅ Task 12 – Players with the Best Dollars Per Hit and RBI

WITH per_hit AS (
    SELECT s.player_id 
    FROM salaries s 
    JOIN performances pf ON s.player_id = pf.player_id 
    WHERE s.year = 2001 AND pf.year = 2001 AND pf.H > 0 
    ORDER BY s.salary * 1.0 / pf.H ASC 
    LIMIT 10
), per_rbi AS (
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
🧠 Key Takeaways 💡
SQL Concepts Practiced:

Aggregation functions: AVG(), SUM(), COUNT(), MAX(), MIN()

JOIN operations

Using subqueries for data filtering and extraction

Grouping and sorting data with GROUP BY and ORDER BY

Effective multi-table queries and value matching

🧰 Workflow Highlights
I used a repeatable command-line pattern to run and store each query:

cat .sql | sqlite3 moneyball.db > ".sql - results.txt"
For example:

cat 7.sql | sqlite3 moneyball.db > "7.sql - results.txt"
This approach allowed me to maintain consistency, efficiency, and organization in executing and saving query results.

🌱 Reflections
Although I wasn’t feeling 100% today, I managed to complete four tasks. Now, I feel more confident in handling JOINs, using subqueries, and formatting SQL results for professional use. The workflow I’ve developed for managing output files has become a key part of my daily process.
