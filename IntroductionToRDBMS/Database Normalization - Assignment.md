# DBMS Normalization - Hands-on Assignment Solution

## Question 1

### Observe the structure of the given EMPLOYEE table and suggest whether it can be further normalized.

**Table Structure**

`EMPNO, ENAME, SAL, DEPTNO, DNAME, LOC`

### Answer

### Current Normal Form: Second Normal Form (2NF)

### Explanation

- `EMPNO` is the **Primary Key** of the table, and attributes `ENAME`, `SAL`, and `DEPTNO` are directly and fully dependent on it.
- However, `DNAME` and `LOC` are dependent on `DEPTNO` rather than on `EMPNO`, which means they are indirectly linked to the primary key through another non-key attribute.
- This indirect dependency is called a **Transitive Dependency**, and its presence means the table does not satisfy the requirements of **Third Normal Form (3NF)**.

### Converting the Table into 3NF

To remove the transitive dependency, the department-related information should be stored in a separate table.

#### EMPLOYEE Table

| EMPNO | ENAME | SAL | DEPTNO |
|-------:|-------|----:|-------:|
| 101 | Ram | 30000 | 10 |

#### DEPARTMENT Table

| DEPTNO | DNAME | LOC |
|-------:|-------|------|
| 10 | Sales | Delhi |

### Result

- After decomposition, the **EMPLOYEE** table contains only attributes that are directly dependent on `EMPNO`, the primary key.
- The **DEPARTMENT** table stores `DNAME` and `LOC` which are fully dependent on `DEPTNO`, its own primary key.
- The transitive dependency has been completely removed, and both tables now satisfy **Third Normal Form (3NF)**.

---

## Question 2

### Observe the structure of the given STUDENT table and suggest whether it can be further normalized.

**Table Structure**

`ROLLNO, NAME, AGE, EXAM, MARKS, GRADE`

### Answer

### Current Normal Form: Second Normal Form (2NF)

### Explanation

- `ROLLNO` acts as the **Primary Key**, and attributes such as `NAME`, `AGE`, `EXAM`, and `MARKS` are associated with it directly.
- However, `GRADE` is determined by the value of `MARKS` rather than by `ROLLNO` directly, creating an indirect dependency through a non-key attribute.
- This indirect relationship is a **Transitive Dependency**, which prevents the table from being promoted to **Third Normal Form (3NF)**.

### Converting the Table into 3NF

Separate the student, result, and grade information into different tables.

#### STUDENT Table

| ROLLNO | NAME | AGE |
|-------:|------|----:|
| 1 | Aaliya | 20 |

#### RESULT Table

| ROLLNO | EXAM | MARKS |
|-------:|------|------:|
| 1 | DBMS | 95 |

#### GRADE Table

| MARKS | GRADE |
|------:|-------|
| 95 | A+ |

### Result

- The original table has been split into three tables — **STUDENT**, **RESULT**, and **GRADE** — each storing only directly dependent attributes.
- The transitive dependency of `GRADE` on `MARKS` (instead of `ROLLNO`) has been fully eliminated by placing it in a dedicated **GRADE** table.
- Every non-key attribute in each table now depends solely on its own primary key, confirming that the design satisfies **Third Normal Form (3NF)**.

---

## Question 3

### Observe the structure of the given EMPLOYEE table and identify the problem. Also suggest how to solve it.

**Table Structure**

`EMPNO, PROJECT_NO, NO_OF_DAYS, CUSTOMERNAME`

**Note:** `EMPNO` and `PROJECT_NO` together form a **Composite Primary Key**.

### Answer

### Problem Identified: Partial Dependency

### Explanation

- The composite primary key consists of `EMPNO` and `PROJECT_NO`, and `NO_OF_DAYS` correctly depends on both attributes of this composite key.
- However, `CUSTOMERNAME` depends only on `PROJECT_NO` and not on the full combination of `EMPNO` and `PROJECT_NO`, creating an incomplete dependency.
- Since a non-key attribute depends on only a part of the composite primary key, the table contains a **Partial Dependency** and is therefore only in **First Normal Form (1NF)**.

### Converting the Table into 2NF

Split the table into two separate tables to eliminate the partial dependency.

#### EMPLOYEE_PROJECT Table

| EMPNO | PROJECT_NO | NO_OF_DAYS |
|-------:|------------|-----------:|
| 101 | P101 | 20 |

#### PROJECT Table

| PROJECT_NO | CUSTOMERNAME |
|------------|--------------|
| P101 | ABC Ltd |

### Result

- The table has been decomposed into **EMPLOYEE_PROJECT** and **PROJECT**, each holding only fully dependent attributes.
- The **EMPLOYEE_PROJECT** table retains `NO_OF_DAYS` which correctly depends on the full composite key of `EMPNO` and `PROJECT_NO`.
- The partial dependency of `CUSTOMERNAME` on only `PROJECT_NO` has been eliminated, and the design now satisfies **Second Normal Form (2NF)**.

---

# Conclusion

The given tables contain either **Transitive Dependency** or **Partial Dependency**, which prevent them from reaching higher normal forms. By separating related data into appropriate tables, redundancy is reduced, data consistency is improved, and the database follows proper normalization principles.
