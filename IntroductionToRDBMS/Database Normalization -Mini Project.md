# Mini Project - Database Normalization (Practical)

## Aim

To understand and implement the concepts of **First Normal Form (1NF)**, **Second Normal Form (2NF)**, and **Third Normal Form (3NF)** by normalizing the given tables and writing the corresponding SQL queries.

---

# Question 1

## Normalize the Table into First Normal Form (1NF)

### Given Table

| Member_Id | First_Name | Last_Name | Hobbies |
|-----------|------------|-----------|---------|
| 101 | Jayson | Mark | Cricket, Swimming, Football |
| 102 | Ram | Ganesh | Swimming, Running, Music |
| 103 | Raj | Kishore | Dancing, Singing, Running |

## Answer

### Current Issue

The **Hobbies** column contains multiple values in a single field. This violates the rule of **First Normal Form (1NF)**, which requires every column to contain only one atomic (single) value.

### Table after Normalization (1NF)

| Member_Id | First_Name | Last_Name | Hobby |
|-----------|------------|-----------|--------|
| 101 | Jayson | Mark | Cricket |
| 101 | Jayson | Mark | Swimming |
| 101 | Jayson | Mark | Football |
| 102 | Ram | Ganesh | Swimming |
| 102 | Ram | Ganesh | Running |
| 102 | Ram | Ganesh | Music |
| 103 | Raj | Kishore | Dancing |
| 103 | Raj | Kishore | Singing |
| 103 | Raj | Kishore | Running |

### SQL Query Used

```sql
CREATE TABLE Member_Hobbies (
    Member_Id INT,
    First_Name VARCHAR(30),
    Last_Name VARCHAR(30),
    Hobby VARCHAR(30)
);

INSERT INTO Member_Hobbies VALUES
(101,'Jayson','Mark','Cricket'),
(101,'Jayson','Mark','Swimming'),
(101,'Jayson','Mark','Football'),
(102,'Ram','Ganesh','Swimming'),
(102,'Ram','Ganesh','Running'),
(102,'Ram','Ganesh','Music'),
(103,'Raj','Kishore','Dancing'),
(103,'Raj','Kishore','Singing'),
(103,'Raj','Kishore','Running');

SELECT * FROM Member_Hobbies;
```

### Result

- The **Hobbies** column has been split into individual rows, with each row now storing exactly one hobby per member.
- This eliminates the multi-valued attribute violation and ensures every cell in the table holds a single atomic value.
- The table now fully satisfies **First Normal Form (1NF)**, making it suitable for further normalization steps.

---

# Question 2

## Normalize the Table into Second Normal Form (2NF)

### Given Table

| EmpNo | Training | Training_Date | Dept |
|-------|----------|---------------|------|
| 101 | Oracle SQL | 12-Aug-2015 | TT |
| 101 | Java | 21-Aug-2015 | BU |
| 102 | Oracle SQL | 18-Sep-2014 | TT |

## Answer

### Current Issue

The table contains information about both **employees** and **training programs**.

The attribute **Dept** depends only on **EmpNo** and not on the complete information related to training. This creates a **Partial Dependency**, so the table should be divided into separate tables.

### Employee Table

| EmpNo | Dept |
|-------|------|
| 101 | TT |
| 102 | TT |

### Training Table

| EmpNo | Training | Training_Date |
|-------|----------|---------------|
| 101 | Oracle SQL | 12-Aug-2015 |
| 101 | Java | 21-Aug-2015 |
| 102 | Oracle SQL | 18-Sep-2014 |

### SQL Query Used

```sql
CREATE TABLE Employee (
    EmpNo INT PRIMARY KEY,
    Dept VARCHAR(20)
);

CREATE TABLE Training (
    EmpNo INT,
    Training VARCHAR(50),
    Training_Date DATE,
    FOREIGN KEY (EmpNo) REFERENCES Employee(EmpNo)
);

INSERT INTO Employee VALUES
(101,'TT'),
(102,'TT');

INSERT INTO Training VALUES
(101,'Oracle SQL','2015-08-12'),
(101,'Java','2015-08-21'),
(102,'Oracle SQL','2014-09-18');

SELECT * FROM Employee;

SELECT * FROM Training;
```

### Result

- The original table has been decomposed into two separate tables — **Employee** and **Training** — each storing only fully dependent attributes.
- The partial dependency of **Dept** on **EmpNo** alone has been eliminated by moving it to the **Employee** table where **EmpNo** is the sole primary key.
- Both tables now satisfy **Second Normal Form (2NF)**, and the **FOREIGN KEY** constraint maintains referential integrity between them.

---

# Question 3

## Normalize the Table into Third Normal Form (3NF)

### Given Table

| Member_Id | First_Name | Last_Name | Sports | Fees |
|-----------|------------|-----------|--------|------|
| 101 | Rajesh | Chand | Cricket | 100 |
| 102 | Jayesh | Raj | Hockey | 80 |
| 103 | Mark | Dorsson | Football | 90 |

## Answer

### Current Issue

The attribute **Fees** depends on **Sports** rather than directly on **Member_Id**.

This creates a **Transitive Dependency**, so the fee information should be stored separately.

### Member Table

| Member_Id | First_Name | Last_Name | Sports |
|-----------|------------|-----------|--------|
| 101 | Rajesh | Chand | Cricket |
| 102 | Jayesh | Raj | Hockey |
| 103 | Mark | Dorsson | Football |

### Sports_Fee Table

| Sports | Fees |
|---------|------|
| Cricket | 100 |
| Hockey | 80 |
| Football | 90 |

### SQL Query Used

```sql
CREATE TABLE Member (
    Member_Id INT PRIMARY KEY,
    First_Name VARCHAR(30),
    Last_Name VARCHAR(30),
    Sports VARCHAR(30)
);

CREATE TABLE Sports_Fee (
    Sports VARCHAR(30) PRIMARY KEY,
    Fees INT
);

INSERT INTO Member VALUES
(101,'Rajesh','Chand','Cricket'),
(102,'Jayesh','Raj','Hockey'),
(103,'Mark','Dorsson','Football');

INSERT INTO Sports_Fee VALUES
('Cricket',100),
('Hockey',80),
('Football',90);

SELECT * FROM Member;

SELECT * FROM Sports_Fee;
```

### Result

- The transitive dependency of **Fees** on **Sports** (instead of **Member_Id**) has been eliminated by separating it into the **Sports_Fee** table.
- The **Member** table now contains only attributes that are directly dependent on **Member_Id**, the primary key.
- Both tables now satisfy **Third Normal Form (3NF)**, ensuring no non-key attribute is transitively dependent on the primary key.

---

# Conclusion

This practical mini project demonstrates the process of database normalization.

- **1NF** removes repeating groups and multi-valued attributes.
- **2NF** removes partial dependencies by separating related data.
- **3NF** removes transitive dependencies by storing dependent information in separate tables.

Applying normalization improves data consistency, minimizes redundancy, and makes the database easier to maintain.

---
