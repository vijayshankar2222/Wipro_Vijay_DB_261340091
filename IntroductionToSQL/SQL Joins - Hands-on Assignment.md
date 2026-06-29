# SQL Joins – Hands-on Assignment

## Aim

To understand and perform different types of SQL Joins such as **Natural Join, Equi Join, Self Join, Outer Join, Non-Equi Join, USING Clause, and ON Clause** using the EMP, DEPT, and SALGRADE tables.

---

## Question 1

### Natural Join

#### Question

Write a query for the HR department to produce the addresses of all the departments. Use the **EMP** and **DEPT** tables. Display **EMPNO, ENAME, SAL, DNAME, and LOC** using a **NATURAL JOIN**.

#### Explanation

A **Natural Join** automatically detects and joins two tables using all columns that share the same name and compatible data type — in this case, `DEPTNO`.
No explicit join condition needs to be written; Oracle SQL internally matches the common column and combines rows where values are equal.
This makes Natural Join a concise option when both tables share exactly one meaningful common column, though it can behave unexpectedly if multiple column names match.

#### SQL Query

```sql
SELECT EMPNO, ENAME, SAL, DNAME, LOC
FROM EMP
NATURAL JOIN DEPT;
```

#### Result

Each row in the output combines employee details from the EMP table with the corresponding department name and location from the DEPT table.
The join is performed automatically on `DEPTNO`, so only employees with a valid, matching department number appear in the result.
For example, an employee `SMITH` in `DEPTNO = 20` would appear alongside `DNAME = RESEARCH` and `LOC = DALLAS`.

---

## Question 2

### Equi Join

#### Question

Display the **JOB, MGR, SAL, COMM, and DNAME** of employees whose job is **SALESMAN**.

#### Explanation

An **Equi Join** explicitly connects the EMP and DEPT tables by matching equal values in the `DEPTNO` column using a `WHERE` clause condition (`E.DEPTNO = D.DEPTNO`).
Table aliases `E` and `D` are used to prefix column names, which clearly distinguishes columns from each table and avoids ambiguity errors.
The additional `AND E.JOB = 'SALESMAN'` condition further filters the joined result to return only rows where the employee's job title is SALESMAN.

#### SQL Query

```sql
SELECT E.JOB, E.MGR, E.SAL, E.COMM, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND E.JOB = 'SALESMAN';
```

#### Result

Only employees with `JOB = 'SALESMAN'` are returned, along with their manager number, salary, commission, and department name.
All salesman records in the EMP table typically belong to department 30, so `DNAME = SALES` is expected to appear for every returned row.
For example, employees like `ALLEN`, `WARD`, `MARTIN`, and `TURNER` would appear with their respective salary and commission values.

---

## Question 3

### Equi Join

#### Question

Display the **ENAME, JOB, DEPTNO, and DNAME** of employees who work in **DALLAS**.

#### Explanation

The EMP and DEPT tables are joined using the equality condition `E.DEPTNO = D.DEPTNO` to link each employee to their corresponding department record.
The additional filter `AND D.LOC = 'DALLAS'` restricts the result to only those departments whose location is Dallas, regardless of which department number it corresponds to.
This approach demonstrates how Equi Join can combine row filtering from both tables within a single WHERE clause.

#### SQL Query

```sql
SELECT E.ENAME, E.JOB, E.DEPTNO, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO
AND D.LOC = 'DALLAS';
```

#### Result

Only employees assigned to a department located in Dallas are returned, along with their job title, department number, and department name.
In the standard EMP/DEPT dataset, Dallas corresponds to `DEPTNO = 20` and `DNAME = RESEARCH`, so all returned rows will share these values.
Employees like `JONES`, `SCOTT`, `ADAMS`, and `FORD` who belong to department 20 are expected to appear in the output.

---

## Question 4

### Self Join

#### Question

Create a report to display employees' name and employee number along with their manager's name and manager number.

#### Explanation

A **Self Join** treats the EMP table as two separate logical instances — one representing employees (`E`) and the other representing managers (`M`) — and joins them within the same query.
The join condition `E.MGR = M.EMPNO` links each employee's manager reference to the corresponding manager's employee number in the second instance.
This technique is essential when a table contains a hierarchical relationship within itself, such as an employee-manager structure.

#### SQL Query

```sql
SELECT
    E.ENAME AS Employee,
    E.EMPNO AS "Emp#",
    M.ENAME AS Manager,
    M.EMPNO AS "Mgr#"
FROM EMP E
JOIN EMP M
ON E.MGR = M.EMPNO;
```

#### Result

Each row pairs an employee's name and number with the name and number of their direct manager from the same EMP table.
Employees who have a valid `MGR` value are included; however, the top-level employee (`KING`) is excluded because their `MGR` is NULL and has no matching row in the manager instance.
For example, `SMITH (7369)` would be paired with their manager `FORD (7902)` in the same output row.

---

## Question 5

### Left Outer Join

#### Question

Modify the previous query to display all employees including **KING**, who has no manager. Order the results by employee number.

#### Explanation

A **LEFT OUTER JOIN** returns all rows from the left table (employee instance `E`) regardless of whether a matching row exists in the right table (manager instance `M`).
For employees with no manager — such as `KING` whose `MGR` is NULL — the manager columns (`M.ENAME`, `M.EMPNO`) will display NULL in the result.
The `ORDER BY E.EMPNO` clause sorts the final output in ascending order of employee number, making the report organized and easier to navigate.

#### SQL Query

```sql
SELECT
    E.ENAME AS Employee,
    E.EMPNO AS "Emp#",
    M.ENAME AS Manager,
    M.EMPNO AS "Mgr#"
FROM EMP E
LEFT OUTER JOIN EMP M
ON E.MGR = M.EMPNO
ORDER BY E.EMPNO;
```

#### Result

All 14 employees (or however many exist in the EMP table) are returned, including `KING` who has no manager assigned.
For `KING`, the Manager and Mgr# columns display NULL since there is no matching row in the manager instance of the table.
The output is sorted by employee number in ascending order, with `SMITH (7369)` typically appearing first and `MILLER (7934)` last.

---

## Question 6

### Non-Equi Join

#### Question

First display the structure of the **SALGRADE** table. Then display the employee name, job, department name, salary, and salary grade.

#### Explanation

A **Non-Equi Join** uses a range-based condition (`BETWEEN ... AND ...`) instead of an equality (`=`) to match rows across tables.
The SALGRADE table defines salary bands using `LOSAL` and `HISAL` columns, and each employee's salary is matched against the band it falls within to determine their grade.
This query also performs a standard Equi Join between EMP and DEPT on `DEPTNO`, making it a three-table join combining both join types simultaneously.

#### SQL Query

```sql
DESC SALGRADE;
```

```sql
SELECT
    E.ENAME,
    E.JOB,
    D.DNAME,
    E.SAL,
    S.GRADE
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO
JOIN SALGRADE S
ON E.SAL BETWEEN S.LOSAL AND S.HISAL;
```

#### Result

Each row displays an employee's name, job title, department name, salary, and the salary grade assigned based on where their salary falls in the SALGRADE table.
The salary grade (1 through 5) is determined by the BETWEEN condition — an employee earning `800` would fall in Grade 1, while one earning `5000` would fall in Grade 5.
For example, `KING` with `SAL = 5000` would show `GRADE = 5` and `DNAME = ACCOUNTING`, while `SMITH` with `SAL = 800` would show `GRADE = 1` and `DNAME = RESEARCH`.

---

## Question 7

### Right Outer Join

#### Question

Display the **ENAME** and **DNAME** of all employees. Also display department names that do not have any employees.

#### Explanation

A **RIGHT OUTER JOIN** returns all rows from the right table (DEPT) regardless of whether a matching employee exists in the left table (EMP).
Departments that have no employees assigned to them will still appear in the result, with the `ENAME` column showing NULL for those rows.
This join type is useful for identifying "empty" departments or any right-table records that are unmatched in the left table.

#### SQL Query

```sql
SELECT E.ENAME, D.DNAME
FROM EMP E
RIGHT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

#### Result

All departments from the DEPT table are displayed, including those with no employees, while only employees with a matching department are shown from the EMP table.
Departments with employees display the employee name alongside the department name; departments without employees show NULL in the `ENAME` column.
In the standard dataset, `OPERATIONS (DEPTNO = 40)` has no employees, so it appears with `ENAME = NULL` in the output.

---

## Question 8

### Self Join with Outer Join

#### Question

Find the names and hire dates for employees who were hired before their managers, along with their managers' names and hire dates.

#### Explanation

The EMP table is self-joined with aliases `E` (employee) and `M` (manager), linked by the condition `E.MGR = M.EMPNO` to pair each employee with their manager.
A **LEFT OUTER JOIN** ensures all employees are initially considered, but the `WHERE M.HIREDATE > E.HIREDATE` condition then filters to retain only those hired earlier than their respective managers.
This comparison of hire dates across the two instances of the same table reveals which employees have more seniority than the person they report to.

#### SQL Query

```sql
SELECT
    E.ENAME AS Employee,
    E.HIREDATE AS Employee_HireDate,
    M.ENAME AS Manager,
    M.HIREDATE AS Manager_HireDate
FROM EMP E
LEFT OUTER JOIN EMP M
ON E.MGR = M.EMPNO
WHERE M.HIREDATE > E.HIREDATE;
```

#### Result

Only employees whose hire date is earlier than their manager's hire date appear in the output, along with both sets of hire dates for direct comparison.
The result confirms cases where an employee has served longer in the organization than the person currently managing them.
For example, if `BLAKE` was hired on `01-MAY-81` and their manager `KING` was hired on `17-NOV-81`, `BLAKE` would appear in this result set.

---

## Question 9

### USING Clause

#### Question

Display the **EMPNO, ENAME, DNAME, and LOC** of employees working as **CLERK** using the **USING** clause.

#### Explanation

The **USING** clause provides a shorthand join syntax where the common column (`DEPTNO`) is specified once in parentheses, and Oracle automatically matches equal values between the two tables.
Unlike the ON clause or Equi Join, the USING clause does not allow prefixing the join column with a table alias — `DEPTNO` must be referenced without a prefix in the query.
The `WHERE JOB = 'CLERK'` condition is applied after the join to filter the combined result to only clerk-level employees.

#### SQL Query

```sql
SELECT EMPNO, ENAME, DNAME, LOC
FROM EMP
JOIN DEPT
USING (DEPTNO)
WHERE JOB = 'CLERK';
```

#### Result

All employees with `JOB = 'CLERK'` are returned, each paired with their corresponding department name and location from the DEPT table.
The `DEPTNO` column is not explicitly listed in the SELECT but is used internally by the USING clause to perform the join.
For example, `SMITH` (clerk in RESEARCH, DALLAS) and `MILLER` (clerk in ACCOUNTING, NEW YORK) would both appear in the output.

---

## Question 10

### ON Clause

#### Question

Display the **ENAME, SAL, MGR, and DNAME** of employees whose salary is greater than **2000**.

#### Explanation

The **ON** clause explicitly defines the join condition between EMP and DEPT using `E.DEPTNO = D.DEPTNO`, giving full control over how the tables are connected.
Unlike the USING clause, the ON clause allows table aliases to prefix the join column, making it suitable for complex queries with multiple joins or self-joins.
The `WHERE E.SAL > 2000` condition is applied after the join to filter only those employees whose salary exceeds 2000.

#### SQL Query

```sql
SELECT E.ENAME, E.SAL, E.MGR, D.DNAME
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO
WHERE E.SAL > 2000;
```

#### Result

Only employees earning more than 2000 are displayed, along with their salary, manager number, and the name of their department.
Higher-salaried employees such as managers and analysts are expected to dominate this result set, while most clerks earning below 2000 are excluded.
For example, `JONES (SAL = 2975, DNAME = RESEARCH)` and `SCOTT (SAL = 3000, DNAME = RESEARCH)` would both appear in the output.

---

## Question 11

### LEFT OUTER JOIN

#### Question

Display the **EMPNO, ENAME, JOB, DEPTNO, DNAME, and LOC** using **LEFT OUTER JOIN**.

#### Explanation

A **LEFT OUTER JOIN** between EMP and DEPT returns every row from the EMP table (left table), whether or not a matching record exists in the DEPT table.
The join condition `ON E.DEPTNO = D.DEPTNO` links each employee to their department; if no match is found, the DEPT columns (`DNAME`, `LOC`) return NULL.
This is useful for ensuring no employee record is lost from the report even if their department data is missing or has been deleted.

#### SQL Query

```sql
SELECT
    E.EMPNO,
    E.ENAME,
    E.JOB,
    E.DEPTNO,
    D.DNAME,
    D.LOC
FROM EMP E
LEFT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

#### Result

All employee records are returned regardless of whether their `DEPTNO` has a matching entry in the DEPT table.
For employees with a valid department match, all six columns — including `DNAME` and `LOC` — are populated with data from the DEPT table.
If an employee's `DEPTNO` has no matching department (e.g., orphaned records), the `DNAME` and `LOC` columns would display NULL for those rows.

---

## Question 12

### RIGHT OUTER JOIN

#### Question

Display the **ENAME** and **DNAME** of employees using **RIGHT OUTER JOIN**.

#### Explanation

A **RIGHT OUTER JOIN** prioritizes the right table (DEPT), returning all department records regardless of whether any employee is assigned to that department.
The join condition `ON E.DEPTNO = D.DEPTNO` connects matching employees to their departments, while unmatched departments still appear with NULL in the employee column.
This is conceptually the mirror image of a LEFT OUTER JOIN — here DEPT drives the result set instead of EMP.

#### SQL Query

```sql
SELECT E.ENAME, D.DNAME
FROM EMP E
RIGHT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

#### Result

Every department from the DEPT table appears in the output, including those with zero employees assigned to them.
For departments that have employees, each employee gets a separate row paired with the department name; for empty departments, `ENAME` displays NULL.
In the standard dataset, `DNAME = OPERATIONS` (dept 40) would appear with `ENAME = NULL` since no employees are assigned to that department.

---

## Question 13

### FULL OUTER JOIN

#### Question

Display the **EMPNO, DNAME, and LOC** using **FULL OUTER JOIN**.

#### Explanation

A **FULL OUTER JOIN** is a combination of LEFT and RIGHT OUTER JOINs — it returns all rows from both the EMP and DEPT tables, matched or unmatched.
Rows that have a matching `DEPTNO` in both tables are fully populated; rows from EMP with no matching DEPT show NULL for `DNAME` and `LOC`, and rows from DEPT with no employees show NULL for `EMPNO`.
This join ensures no data is omitted from either table, making it the most comprehensive join type for identifying gaps in both directions.

#### SQL Query

```sql
SELECT
    E.EMPNO,
    D.DNAME,
    D.LOC
FROM EMP E
FULL OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

#### Result

The output includes all employees with their department details, all departments with their employee numbers, and any unmatched records from either table with NULL in the missing columns.
Employees with valid department matches display all three columns fully populated; `DNAME = OPERATIONS` appears with `EMPNO = NULL` as it has no employees.
If any employee had an invalid `DEPTNO` not found in DEPT, their row would also appear with `DNAME = NULL` and `LOC = NULL` in the output.

---

## Conclusion

This practical assignment demonstrates the implementation of different SQL Join operations across the following join types:

| Join Type | Description |
|:----------|:------------|
| **Natural Join** | Automatically joins tables using all columns with matching names |
| **Equi Join** | Matches rows based on equal values of a common column using WHERE |
| **Self Join** | Joins a table to itself to represent hierarchical relationships |
| **Left Outer Join** | Returns all rows from the left table, with NULLs for unmatched right rows |
| **Right Outer Join** | Returns all rows from the right table, with NULLs for unmatched left rows |
| **Full Outer Join** | Returns all rows from both tables, with NULLs wherever no match exists |
| **Non-Equi Join** | Joins records using a range condition (`BETWEEN`) instead of equality |
| **USING Clause** | Simplifies join syntax when the common column name is identical in both tables |
| **ON Clause** | Explicitly defines a custom join condition with full alias support |

These joins are essential for retrieving related data from multiple tables efficiently in relational database management systems.
