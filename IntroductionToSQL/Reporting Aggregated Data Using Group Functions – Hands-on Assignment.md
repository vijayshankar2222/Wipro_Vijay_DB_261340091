# Reporting Aggregated Data Using Group Functions – Hands-on Assignment

## Aim

To understand and implement SQL **Group Functions** such as **MAX(), MIN(), SUM(), AVG(), COUNT(), GROUP BY, HAVING, ORDER BY**, and aggregate calculations using the **EMPLOYEES** table.

---

# Question 1

## Find the highest, lowest, total, and average salary of all employees. Label the columns as Maximum, Minimum, Sum, and Average.

### Explanation

Aggregate functions are used to perform calculations on multiple rows and return a single consolidated result. This query applies **MAX()**, **MIN()**, **SUM()**, and **AVG()** functions simultaneously on the salary column to extract key statistical values. The **ROUND()** function is also applied to the average salary to ensure the output is clean and without unnecessary decimal places.

### SQL Query

```sql
SELECT MAX(salary) AS Maximum,
       MIN(salary) AS Minimum,
       SUM(salary) AS Sum,
       ROUND(AVG(salary)) AS Average
FROM employees;
```

### Result

- The query returns a single row containing the highest, lowest, total, and average salary across all employees.
- Each column is properly labeled as **Maximum**, **Minimum**, **Sum**, and **Average** for clear identification.
- This demonstrates how multiple aggregate functions can be combined in a single SELECT statement for efficient data summarization.

---

# Question 2

## Round the results to the nearest whole number.

### Explanation

The **ROUND()** function eliminates decimal places and presents salary values as whole numbers for better readability. It is applied individually to each aggregate function — **MAX()**, **MIN()**, **SUM()**, and **AVG()** — to ensure uniform formatting across all columns. This is particularly useful when presenting financial data where fractional values are not meaningful.

### SQL Query

```sql
SELECT ROUND(MAX(salary)) AS Maximum,
       ROUND(MIN(salary)) AS Minimum,
       ROUND(SUM(salary)) AS Sum,
       ROUND(AVG(salary)) AS Average
FROM employees;
```

### Result

- All four salary calculations — Maximum, Minimum, Sum, and Average — are displayed as rounded whole numbers.
- The **ROUND()** function successfully removes decimal values, making the output cleaner and easier to interpret.
- This confirms that wrapping aggregate functions inside **ROUND()** applies rounding to the final computed result.

---

# Question 3

## Display the minimum, maximum, total, and average salary for each job type.

### Explanation

The **GROUP BY** clause organizes employees into separate groups based on their **job_id**, so that aggregate functions operate independently within each group. This allows a single query to produce salary statistics for every job type simultaneously rather than running separate queries for each role. The result provides a comprehensive salary breakdown across all departments and job categories within the organization.

### SQL Query

```sql
SELECT job_id,
       MIN(salary) AS Minimum,
       MAX(salary) AS Maximum,
       SUM(salary) AS Sum,
       ROUND(AVG(salary)) AS Average
FROM employees
GROUP BY job_id;
```

### Result

- The query groups all employees by their **job_id** and computes salary statistics for each group separately.
- Each row in the output represents a unique job type along with its corresponding Minimum, Maximum, Sum, and Average salary.
- This confirms that **GROUP BY** effectively partitions data, enabling aggregate functions to operate on each subset independently.

---

# Question 4

## Display the number of employees having the same job.

### Explanation

The **COUNT()** function is used here in combination with **GROUP BY** to count the number of employees belonging to each job category. Instead of counting individual rows manually, SQL automatically groups all employees by their **job_id** and returns the total count per group. This approach is highly efficient for understanding workforce distribution across different roles.

### SQL Query

```sql
SELECT job_id,
       COUNT(*) AS Number_of_People
FROM employees
GROUP BY job_id;
```

### Result

- The query returns each unique **job_id** along with the total number of employees assigned to that role.
- The **COUNT(\*)** function counts all rows within each group, including employees regardless of null values in other columns.
- This output helps identify which job roles have the highest and lowest number of employees in the organization.

---

# Question 5

## Determine the number of managers without listing them. Label the column as Number of Managers.

### Explanation

The **COUNT(DISTINCT manager_id)** function counts only the unique manager IDs present in the **manager_id** column, automatically ignoring any duplicate or null values. This ensures that even if a manager supervises multiple employees, they are counted only once in the final result. The use of **DISTINCT** is critical here to avoid overcounting managers who appear multiple times across different employee records.

### SQL Query

```sql
SELECT COUNT(DISTINCT manager_id) AS "Number of Managers"
FROM employees;
```

### Result

- The query returns a single value representing the total count of unique managers in the organization.
- The **DISTINCT** keyword ensures that each manager is counted only once, regardless of how many employees they manage.
- The result is labeled as **Number of Managers** as specified, providing a clear and meaningful column header.

---

# Question 6

## Find the difference between the highest and lowest salaries. Label the column as DIFFERENCE.

### Explanation

This query calculates the salary range within the organization by subtracting the result of **MIN(salary)** from **MAX(salary)** in a single expression. The resulting value represents the spread between the highest-paid and lowest-paid employee, which is a key metric for understanding salary disparity. Labeling the output as **DIFFERENCE** ensures the column is self-descriptive and easy to interpret in reports.

### SQL Query

```sql
SELECT MAX(salary) - MIN(salary) AS DIFFERENCE
FROM employees;
```

### Result

- The query returns a single numeric value representing the gap between the highest and lowest salary in the **EMPLOYEES** table.
- The arithmetic expression **MAX(salary) - MIN(salary)** is evaluated after both aggregate functions compute their respective values.
- This result highlights the salary disparity within the organization and is labeled clearly as **DIFFERENCE**.

---

# Question 7

## Display the manager number and the salary of the lowest-paid employee for each manager. Exclude employees without managers and groups whose minimum salary is 2000 or less. Sort the result in descending order of salary.

### Explanation

This query combines multiple SQL clauses to produce a filtered and sorted summary of salary data per manager. The **WHERE** clause excludes employees with no assigned manager, while the **HAVING** clause filters out manager groups whose lowest salary falls at or below 2000 after grouping. Finally, the **ORDER BY** clause sorts the remaining results in descending order so that managers with the highest minimum salary appear at the top.

### SQL Query

```sql
SELECT manager_id,
       MIN(salary) AS Lowest_Salary
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY manager_id
HAVING MIN(salary) > 2000
ORDER BY Lowest_Salary DESC;
```

### Result

- The query returns each **manager_id** along with the lowest salary among the employees they manage, after applying all specified filters.
- Only managers whose lowest-paid employee earns more than **2000** are included, demonstrating the filtering power of the **HAVING** clause.
- The results are sorted in descending order of **Lowest_Salary**, so the manager with the highest minimum salary appears first.

---

# Question 8

## Display the total number of employees and the number of employees hired in 1980, 1981, and 1982.

### Explanation

This query uses conditional aggregation by combining **SUM()** with **CASE** statements to count employees hired in specific years within a single query. The **EXTRACT(YEAR FROM hire_date)** function retrieves the year portion from each employee's hire date, and the **CASE** expression assigns a value of 1 for matching years and 0 otherwise. This technique is more efficient than running separate queries for each year and produces a compact, cross-tabulated summary.

### SQL Query

```sql
SELECT COUNT(*) AS Total_Employees,
       SUM(CASE WHEN EXTRACT(YEAR FROM hire_date) = 1980 THEN 1 ELSE 0 END) AS "1980",
       SUM(CASE WHEN EXTRACT(YEAR FROM hire_date) = 1981 THEN 1 ELSE 0 END) AS "1981",
       SUM(CASE WHEN EXTRACT(YEAR FROM hire_date) = 1982 THEN 1 ELSE 0 END) AS "1982"
FROM employees;
```

### Result

- The query returns a single row displaying the total employee count alongside year-wise hiring counts for **1980**, **1981**, and **1982**.
- The **CASE** statements act as conditional counters, incrementing only when the hire year matches the specified value.
- This confirms that conditional aggregation is a powerful technique for generating pivot-style summaries directly within SQL.

---

# Conclusion

This hands-on assignment demonstrates the use of SQL **Aggregate Functions** for analyzing and summarizing data.

- **MAX()** returns the highest value.
- **MIN()** returns the lowest value.
- **SUM()** calculates the total value.
- **AVG()** computes the average value.
- **COUNT()** counts records.
- **GROUP BY** organizes data into groups.
- **HAVING** filters grouped records.
- **ORDER BY** sorts the final output.

These functions are essential for generating reports and performing data analysis in SQL databases.
