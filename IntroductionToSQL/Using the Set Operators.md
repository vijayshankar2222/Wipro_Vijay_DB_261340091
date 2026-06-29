# Using the Set Operators – Hands-on Assignment

## Aim

To understand and implement SQL **Set Operators** such as **UNION** and **UNION ALL** for combining the results of multiple SQL queries. This assignment also demonstrates the use of aggregate functions and matrix reports using the **EMPLOYEES** table.

---

## Question 1

### Create a matrix query to display the job, the salary for that job based on department number, and the total salary for that job, for departments 10, 20, and 30. Give each column an appropriate heading.

#### Explanation

The **DECODE()** function evaluates each row's `department_id` and returns the `salary` value only when it matches the target department, otherwise it returns NULL.
The **SUM()** aggregate function wraps each DECODE expression so that salaries are totaled per department column across all rows sharing the same job.
The **GROUP BY job** clause groups the entire result by job title, producing one matrix row per unique job with department-wise and overall salary totals.

#### SQL Query

```sql
SELECT job,
       SUM(DECODE(department_id,10,salary)) AS Dept10,
       SUM(DECODE(department_id,20,salary)) AS Dept20,
       SUM(DECODE(department_id,30,salary)) AS Dept30,
       SUM(salary) AS Total
FROM employees
WHERE department_id IN (10,20,30)
GROUP BY job;
```

#### Result

Each row represents a unique job title with four salary columns: the total salary paid in department 10, 20, 30, and the combined total across all three.
If no employee with that job exists in a particular department, the corresponding cell displays NULL rather than zero.
For example, a `MANAGER` row might show NULL for Dept10, `14400` for Dept20, `8550` for Dept30, and `22950` as the Total.

---

## Question 2

### Using the SET operator, display the DEPTNO and total salary for each department, the JOB and total salary for each job, and the overall total salary.

#### Explanation

The **UNION** operator merges the result sets of three separate SELECT statements into one unified output, automatically removing any duplicate rows.
The first query groups by `department_id` to produce per-department salary totals; the second groups by `job_id` for per-job totals; the third computes the single grand total across all employees.
NULL placeholders are used in each query for columns that are not applicable, ensuring all three SELECT statements return the same number of columns with compatible data types for UNION to work.

#### SQL Query

```sql
SELECT TO_CHAR(department_id) AS Department,
       NULL AS Job,
       SUM(salary) AS Total_Salary
FROM employees
GROUP BY department_id
UNION
SELECT NULL,
       job_id,
       SUM(salary)
FROM employees
GROUP BY job_id
UNION
SELECT 'TOTAL',
       NULL,
       SUM(salary)
FROM employees;
```

#### Result

The combined output contains three logical sections: rows showing salary totals per department, rows showing salary totals per job, and a single `TOTAL` row representing the overall salary sum.
NULL values appear in the Department column for job-wise rows and in the Job column for department-wise rows, clearly separating the two dimensions of aggregation.
For example, one row might show `Department = 90, Job = NULL, Total_Salary = 58000`, while another shows `Department = NULL, Job = AD_PRES, Total_Salary = 24000`.

---

## Question 3

### Using the SET operator, display the JOB and DEPTNO of employees working in departments 20, 10, and 30 in the same order.

#### Explanation

The **UNION ALL** operator combines the results of all three SELECT statements sequentially without removing any duplicate entries, unlike UNION which deduplicates.
Each SELECT targets a single department using `WHERE department_id = <value>`, and the three queries are stacked in the exact order required: 20 first, then 10, then 30.
Since UNION ALL preserves the original order of each block, employees from department 20 always appear before department 10, and department 10 before department 30 in the final output.

#### SQL Query

```sql
SELECT job_id, department_id
FROM employees
WHERE department_id = 20
UNION ALL
SELECT job_id, department_id
FROM employees
WHERE department_id = 10
UNION ALL
SELECT job_id, department_id
FROM employees
WHERE department_id = 30;
```

#### Result

The output lists job IDs and department numbers in the specified sequence: all department 20 records first, followed by department 10, and finally department 30.
Unlike UNION, UNION ALL retains all rows including duplicates, so if two employees in the same department share the same job title, both rows appear in the result.
For example, the result begins with `MK_MAN, 20` and `MK_REP, 20` entries, then transitions to `AD_ASST, 10`, and concludes with the department 30 `SA_MAN` and `SA_REP` entries.

---

## Conclusion

This hands-on assignment demonstrates the use of SQL **Set Operators** for combining data from multiple queries across the following key concepts:

| Concept | Description |
|:--------|:------------|
| **UNION** | Combines result sets from multiple queries and removes duplicate rows |
| **UNION ALL** | Combines result sets and retains all rows including duplicates |
| **DECODE()** | Enables conditional column pivoting for matrix-style reports |
| **SUM()** | Aggregates salary values across grouped rows for summarized output |
| **NULL Placeholders** | Ensures column count and type compatibility across UNION queries |

These SQL techniques are widely used in report generation and data analysis within relational database systems.
