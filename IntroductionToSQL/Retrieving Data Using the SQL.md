# Retrieving Data Using the SQL SELECT Statement – Hands-on Assignment

## Aim

To understand the basic SQL **SELECT** statement and retrieve data from database tables using commands such as **DESC**, **SELECT**, and **WHERE**.

---

# Question 1

## Determine the Structure of the DEPT Table and its Contents.

### Explanation

The **DESC** command reveals the internal structure of a table by listing all column names along with their respective data types and constraints. The **SELECT** statement is then used to fetch and display all the records stored within the table. Together, these commands provide a complete overview of both the schema and the data present in the table.

### SQL Query

#### Display the Structure of the DEPT Table

```sql
DESC DEPT;
```

#### Display the Contents of the DEPT Table

```sql
SELECT * FROM DEPT;
```

### Result

- The `DESC` command successfully displays the column names, data types, and constraints of the **DEPT** table.
- The `SELECT *` statement retrieves and displays all rows currently stored in the **DEPT** table.
- This confirms that the **DEPT** table is properly structured and contains valid department records.

---

# Question 2

## Determine the Structure of the EMP Table and its Contents.

### Explanation

The **DESC** command is applied to the **EMP** table to examine its structure, revealing all column names, data types, and whether each column allows null values. The **SELECT** statement is then executed to retrieve the complete set of employee records from the table. This process helps in understanding how employee data is organized within the database.

### SQL Query

#### Display the Structure of the EMP Table

```sql
DESC EMP;
```

#### Display the Contents of the EMP Table

```sql
SELECT * FROM EMP;
```

### Result

- The `DESC` command displays all columns of the **EMP** table along with their data types and nullability constraints.
- The `SELECT *` statement retrieves every employee record stored in the **EMP** table.
- This gives a full picture of the employee data, including fields such as EMPNO, ENAME, JOB, SAL, and DEPTNO.

---

# Question 3

## Display the ENAME and DEPTNO from the EMP table whose EMPNO is 7788.

### Explanation

The **WHERE** clause is used to apply a filter condition so that only rows matching the specified criteria are returned. In this query, only the employee with **EMPNO = 7788** is targeted, and specifically the **ENAME** and **DEPTNO** columns are selected. This demonstrates how SQL can retrieve selective columns for a specific record without fetching the entire table.

### SQL Query

```sql
SELECT ENAME, DEPTNO
FROM EMP
WHERE EMPNO = 7788;
```

### Result

- The query filters the **EMP** table and returns only the row where **EMPNO** equals **7788**.
- The output displays the **Employee Name (ENAME)** and the **Department Number (DEPTNO)** for that specific employee.
- This confirms that the **WHERE** clause successfully narrows down the result to a single targeted record.

---

# Conclusion

This hands-on assignment introduces the basic SQL **SELECT** statement and demonstrates how to retrieve information from database tables.

- **DESC** is used to view the table structure.
- **SELECT** retrieves records from a table.
- **SELECT \*** displays all columns.
- **WHERE** filters records based on a specified condition.

These commands form the foundation of SQL and are essential for querying data in relational database systems.
