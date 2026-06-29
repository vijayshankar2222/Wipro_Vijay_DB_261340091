# Restricting and Sorting Data – Hands-on Assignment

## Aim

To understand and implement SQL statements for **restricting**, **sorting**, **filtering**, and **formatting** data using clauses such as **WHERE, ORDER BY, DISTINCT, BETWEEN, IN, LIKE, IS NULL**, and **column aliases** on the **EMP** table.

---

## Question 1

### Display the ENAME, SAL, and COMM of employees who earn commission. Sort the records in descending order of Salary and Commission using the column's numeric position in the ORDER BY clause.

#### Explanation

The **WHERE** clause with `COMM IS NOT NULL` filters only those employees who have been assigned a commission value.
The **ORDER BY** clause uses numeric positions `2` and `3` to refer to the `SAL` and `COMM` columns respectively, avoiding column name repetition.
Using `DESC` on both positions ensures that records are sorted from the highest salary to the lowest, and within the same salary, from the highest commission downward.

#### SQL Query

```sql
SELECT ENAME, SAL, COMM
FROM EMP
WHERE COMM IS NOT NULL
ORDER BY 2 DESC, 3 DESC;
```

#### Result

Only employees with a non-NULL commission value appear in the output, excluding those with no commission.
Records are sorted primarily by **SAL** in descending order, and ties in salary are further sorted by **COMM** in descending order.
For example, an employee with `SAL = 1600` and `COMM = 300` would appear before one with `SAL = 1600` and `COMM = 100`.

---

## Question 2

### Display all unique job codes from the EMP table.

#### Explanation

The **DISTINCT** keyword is placed immediately after `SELECT` to instruct Oracle SQL to eliminate duplicate rows from the result set.
Without `DISTINCT`, the same job title would appear multiple times — once for each employee holding that role.
It scans all values in the `JOB` column and returns only one occurrence of each unique job code.

#### SQL Query

```sql
SELECT DISTINCT JOB
FROM EMP;
```

#### Result

Each job title present in the EMP table is displayed exactly once, regardless of how many employees hold that role.
Common values like `CLERK`, `MANAGER`, `SALESMAN`, `ANALYST`, and `PRESIDENT` appear as single entries.
The total number of rows returned equals the count of unique job types, not the total number of employees.

---

## Question 3

### Display employee number, employee name, job, and hire date with column aliases.

#### Explanation

Column aliases are assigned using the **AS** keyword followed by a descriptive label enclosed in double quotes.
Aliases replace the default column headers (`EMPNO`, `ENAME`, `JOB`, `HIREDATE`) with more readable, user-friendly names.
Double quotes are used here to preserve spaces and mixed-case formatting in the alias names.

#### SQL Query

```sql
SELECT EMPNO AS "Emp #",
       ENAME AS "Employee",
       JOB AS "Job",
       HIREDATE AS "Hire Date"
FROM EMP;
```

#### Result

The output displays four columns with the custom headers: **Emp #**, **Employee**, **Job**, and **Hire Date**.
All employee records are returned without any filtering, showing data for every row in the EMP table.
The alias names make the report more presentable and easier to interpret for end users.

---

## Question 4

### Display the employee name concatenated with the job title. Name the column "Employee and Title".

#### Explanation

The **concatenation operator (`||`)** joins two or more string values into a single output string within a SQL query.
A comma followed by a space (`', '`) is inserted between the name and job title to clearly separate the two values.
The result is aliased as `"Employee and Title"` to provide a meaningful column header in the output.

#### SQL Query

```sql
SELECT ENAME || ', ' || JOB AS "Employee and Title"
FROM EMP;
```

#### Result

Each row displays a single merged string combining the employee's name and job title in the format `ENAME, JOB`.
For example, the output for a record might appear as `SCOTT, ANALYST` or `KING, PRESIDENT`.
All employee records are displayed under the single column labeled **Employee and Title**.

---

## Question 5

### Display ENAME, JOB, HIREDATE, and MGR in a single output column separated by commas. Name the column THE_OUTPUT.

#### Explanation

All four columns — `ENAME`, `JOB`, `HIREDATE`, and `MGR` — are concatenated into one string using the `||` operator.
Commas are placed between each value as literal string separators to clearly distinguish each piece of data.
The combined result is labeled `THE_OUTPUT` as a single output column alias without double quotes.

#### SQL Query

```sql
SELECT ENAME || ',' || JOB || ',' || HIREDATE || ',' || MGR AS THE_OUTPUT
FROM EMP;
```

#### Result

Each row produces a single string containing all four field values separated by commas, such as `SCOTT,ANALYST,19-APR-87,7566`.
The **THE_OUTPUT** column presents all the data in a compact, comma-delimited format within one field.
If `MGR` is NULL (e.g., for the President), the trailing comma will appear without a value at the end.

---

## Question 6

### Display the ENAME, JOB, and HIREDATE of employees named SCOTT or TURNER. Sort the records by hire date in ascending order.

#### Explanation

The **IN** operator checks if the `ENAME` value matches any name in the provided list, here `'SCOTT'` and `'TURNER'`.
Using `IN` is a cleaner and more readable alternative to writing multiple `OR` conditions in the `WHERE` clause.
The **ORDER BY HIREDATE ASC** clause sorts the filtered results chronologically from the earliest to the most recent hire date.

#### SQL Query

```sql
SELECT ENAME, JOB, HIREDATE
FROM EMP
WHERE ENAME IN ('SCOTT', 'TURNER')
ORDER BY HIREDATE ASC;
```

#### Result

Only two rows are returned — one for **SCOTT** and one for **TURNER** — along with their respective job titles and hire dates.
The records are sorted in ascending order of `HIREDATE`, so the earlier-hired employee appears first.
For example, if TURNER was hired before SCOTT, TURNER's record will appear in the first row.

---

## Question 7

### Display the ENAME and DEPTNO of employees working in departments 20 or 30. Sort the records alphabetically by employee name.

#### Explanation

The **IN (20, 30)** condition filters employees who belong to either department 20 or department 30.
This is equivalent to writing `DEPTNO = 20 OR DEPTNO = 30`, but the `IN` syntax is more concise and readable.
The **ORDER BY ENAME** clause sorts the filtered results alphabetically in ascending order (A to Z) by default.

#### SQL Query

```sql
SELECT ENAME, DEPTNO
FROM EMP
WHERE DEPTNO IN (20, 30)
ORDER BY ENAME;
```

#### Result

All employees belonging to department 20 or 30 are returned, with employees from other departments excluded.
The results are sorted alphabetically by employee name, making it easy to locate any specific employee.
For example, `ALLEN` (dept 30) would appear before `JONES` (dept 20) in the output.

---

## Question 8

### Display the employee name and salary of employees earning between 2000 and 3000 and working in departments 20 or 30. Use the aliases Employee and Monthly Salary.

#### Explanation

The **BETWEEN 2000 AND 3000** condition is inclusive, meaning employees earning exactly 2000 or exactly 3000 are also included.
The **AND DEPTNO IN (20, 30)** condition further narrows the result to only those working in the specified departments.
Column aliases `"Employee"` and `"Monthly Salary"` are applied to replace the default `ENAME` and `SAL` headers for better readability.

#### SQL Query

```sql
SELECT ENAME AS "Employee",
       SAL AS "Monthly Salary"
FROM EMP
WHERE SAL BETWEEN 2000 AND 3000
AND DEPTNO IN (20, 30);
```

#### Result

Only employees who satisfy both conditions — salary in range [2000, 3000] and department 20 or 30 — are shown.
The output uses the aliases **Employee** and **Monthly Salary** as column headers instead of the raw column names.
For example, `JONES` (dept 20, sal 2975) and `MARTIN` (dept 30, sal 2650) would be valid entries in the result.

---

## Question 9

### Display the employee name and hire date of all employees hired in 1981.

#### Explanation

The **BETWEEN** operator filters rows where `HIREDATE` falls within the given date range, inclusive of both boundary dates.
The dates `'01-JAN-81'` and `'31-DEC-81'` define the full calendar year 1981 in Oracle's default `DD-MON-YY` date format.
This approach captures all employees hired on any date throughout the entire year 1981.

#### SQL Query

```sql
SELECT ENAME, HIREDATE
FROM EMP
WHERE HIREDATE BETWEEN '01-JAN-81' AND '31-DEC-81';
```

#### Result

All employees whose hire date falls anywhere between January 1, 1981 and December 31, 1981 are displayed.
The output includes the employee's name and their exact hire date in the default Oracle date format.
Employees hired in other years (e.g., 1980 or 1987) are excluded from this result set.

---

## Question 10

### Display the ENAME and SAL of employees whose salary is greater than a user-specified value.

#### Explanation

The **ACCEPT** command prompts the user at runtime to input a salary threshold value, which is stored in the variable `AMT`.
The substitution variable `&AMT` is referenced in the `WHERE` clause to dynamically compare each employee's salary against the entered value.
This approach makes the query flexible and reusable without needing to hardcode a salary value in the SQL statement.

#### SQL Query

```sql
ACCEPT AMT PROMPT 'Enter Salary : '

SELECT ENAME, SAL
FROM EMP
WHERE SAL > &AMT;
```

#### Result

At runtime, the user is prompted with the message `Enter Salary :` and their input is used as the salary threshold.
Only employees whose `SAL` is strictly greater than the entered value are returned in the output.
For example, entering `2000` would display only employees earning more than 2000, such as `JONES (2975)` or `KING (5000)`.

---

## Question 11

### Display the employee name and job title of employees who do not have a manager.

#### Explanation

The **IS NULL** condition checks for rows where the `MGR` column contains no value (NULL), meaning no manager is assigned.
In the EMP table, typically only the top-level employee (the President) has a NULL manager field.
Using `= NULL` would not work in SQL — `IS NULL` is the correct syntax to test for the absence of a value.

#### SQL Query

```sql
SELECT ENAME, JOB
FROM EMP
WHERE MGR IS NULL;
```

#### Result

Only employees with no manager assigned — where `MGR` is NULL — are returned in the output.
In a standard EMP dataset, this typically returns a single record: `KING` with the job title `PRESIDENT`.
This query is useful for identifying the root node or top-level authority in an organizational hierarchy.

---

## Question 12

### Prompt the user for a Manager ID and display EMPNO, ENAME, SAL, and DEPTNO. Allow the user to sort the result by any selected column.

#### Explanation

Two **ACCEPT** commands are used — one to collect the Manager ID (`MID`) and another to collect the desired sort column name (`COL`).
The substitution variable `&MID` is used in the `WHERE` clause to filter employees reporting to the specified manager.
The substitution variable `&COL` is passed directly to the **ORDER BY** clause, allowing the user to dynamically choose the sort column at runtime.

#### SQL Query

```sql
ACCEPT MID PROMPT 'Enter Manager ID : '
ACCEPT COL PROMPT 'Enter Column Name : '

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE MGR = &MID
ORDER BY &COL;
```

#### Result

At runtime, the user first enters a Manager ID (e.g., `7839`) and then a column name (e.g., `SAL`) to sort by.
The query returns all employees who report to the entered manager, filtered by the `MGR` field.
Results are dynamically sorted by the user-chosen column, making the query highly flexible and interactive.

---

## Question 13

### Prompt the user for a Manager ID and generate EMPNO, ENAME, SAL, and DEPTNO for employees working under that manager. Allow sorting by a selected column.

#### Explanation

This query follows the same structure as Question 12, using **ACCEPT** to take both the Manager ID and sort column as runtime inputs.
The `WHERE MGR = &MID` condition isolates only those employees directly reporting to the specified manager.
The `ORDER BY &COL` clause then organizes the results based on the column name the user provides, enabling flexible output control.

#### SQL Query

```sql
ACCEPT MID PROMPT 'Enter Manager ID : '
ACCEPT COL PROMPT 'Enter Column Name : '

SELECT EMPNO, ENAME, SAL, DEPTNO
FROM EMP
WHERE MGR = &MID
ORDER BY &COL;
```

#### Result

The output shows employee details — number, name, salary, and department — for all staff under the specified manager.
Entering a Manager ID like `7698` would return employees such as `ALLEN`, `WARD`, and `MARTIN` who report to that manager.
The sort order changes based on the column name provided, for instance entering `ENAME` would display results in alphabetical order.

---

## Question 14

### Display all employee names in which the third letter is 'A'.

#### Explanation

The **LIKE** operator is used for pattern matching, where each underscore (`_`) represents exactly one unknown character.
Two underscores `__` are placed before `A` to skip the first and second characters and match `A` specifically at the third position.
The `%` wildcard after `A` allows any number of characters to follow, ensuring names of all lengths are captured.

#### SQL Query

```sql
SELECT ENAME
FROM EMP
WHERE ENAME LIKE '__A%';
```

#### Result

All employee names where the character at position 3 is exactly `'A'` are returned in the output.
For example, names like `BLAKE` (B-L-A) and `CLARK` (C-L-A) would match this pattern.
Names where `A` appears at any other position (first, second, or fourth) are excluded from the results.

---

## Question 15

### Display the names of employees who have both 'A' and 'S' in their names.

#### Explanation

Two separate **LIKE** conditions are combined using the **AND** operator to ensure both letters must be present in the name.
The pattern `'%A%'` matches any name containing the letter `A` at any position, and similarly `'%S%'` for the letter `S`.
Both conditions must be true simultaneously — an employee name must contain both `A` and `S` to appear in the result.

#### SQL Query

```sql
SELECT ENAME
FROM EMP
WHERE ENAME LIKE '%A%'
AND ENAME LIKE '%S%';
```

#### Result

Only employees whose names contain both the letter `A` and the letter `S` (in any position) are returned.
For example, `JAMES` (contains both A and S) would be included, while `CLARK` (no S) would be excluded.
The matching is case-sensitive in Oracle by default, so both letters are expected to be uppercase as stored in the EMP table.

---

## Question 16

### Display the ENAME, JOB, and SAL of employees whose job is CLERK and whose salary is either 800, 950, or 1300.

#### Explanation

The `WHERE JOB = 'CLERK'` condition filters only employees holding the CLERK job title.
The **IN (800, 950, 1300)** operator then further restricts results to only those clerks earning one of the three specified salary values.
Both conditions are combined with **AND**, meaning an employee must satisfy both — be a CLERK and have a matching salary — to appear in the result.

#### SQL Query

```sql
SELECT ENAME, JOB, SAL
FROM EMP
WHERE JOB = 'CLERK'
AND SAL IN (800, 950, 1300);
```

#### Result

Only employees with `JOB = 'CLERK'` and a salary of exactly 800, 950, or 1300 are displayed.
Clerks with other salary values (e.g., 1100) are excluded even though they match the job condition.
For example, `SMITH (800)` and `JAMES (950)` would be valid entries returned by this query.

---

## Conclusion

This hands-on assignment demonstrates the use of SQL statements for restricting and sorting records across the following key concepts:

| Clause / Feature | Purpose |
|:-----------------|:--------|
| **WHERE** | Filters records based on one or more conditions |
| **ORDER BY** | Sorts query results in ascending or descending order |
| **DISTINCT** | Removes duplicate values from the result set |
| **BETWEEN** | Filters values within a specified inclusive range |
| **IN** | Checks whether a value matches any item in a list |
| **LIKE** | Performs pattern-based string matching |
| **IS NULL** | Identifies rows where a column has no assigned value |
| **Column Aliases** | Replaces default column names for improved readability |
| **ACCEPT** | Accepts dynamic user input at query runtime |

These SQL features are essential for retrieving accurate, organized, and user-friendly data from relational databases in real-world applications.
