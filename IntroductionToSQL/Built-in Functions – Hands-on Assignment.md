# Built-in Functions – Hands-on Assignment

## Aim

To understand and implement SQL **Built-in Functions** such as **Single Row Functions**, **Character Functions**, **Number Functions**, **Date Functions**, **Conversion Functions**, and **Conditional Functions** using the **EMPLOYEES** table.

---

## Question 1

### Write a query to display the current date. Label the column as Date.

#### Explanation

The **SYSDATE** function is a built-in Oracle SQL function that returns the current date and time from the database server.
It requires no arguments and is typically queried from the **DUAL** dummy table.
The result is formatted according to the database's default date format (e.g., `DD-MON-RR`).

#### SQL Query

```sql
SELECT SYSDATE AS "Date"
FROM dual;
```

#### Result

The query returns a single row containing the current system date.
The column is labeled **Date** as specified using the alias.
Output example: `29-JUN-26`.

---

## Question 2

### Display the employee number, last name, salary, and salary increased by 15.5%. Label the increased salary as New Salary.

#### Explanation

The salary is multiplied by **1.155** to calculate a 15.5% increase over the original value.
The **ROUND()** function is applied to round the result to the nearest whole number, removing decimals.
All four columns — employee ID, last name, original salary, and new salary — are displayed together.

#### SQL Query

```sql
SELECT employee_id,
       last_name,
       salary,
       ROUND(salary * 1.155) AS "New Salary"
FROM employees;
```

#### Result

Each row displays the employee's ID, last name, current salary, and the revised salary after applying the 15.5% hike.
The **New Salary** column shows a rounded integer value for clean presentation.
For example, an employee earning `10000` would show `11550` as their new salary.

---

## Question 3

### Modify the previous query to display the salary increase amount. Label the column Increase.

#### Explanation

An additional column **Increase** is added by subtracting the original salary from the new salary (`ROUND(salary * 1.155) - salary`).
This reveals exactly how much extra compensation each employee would receive after the 15.5% hike.
The **ROUND()** function ensures the computed increase is also a clean whole number.

#### SQL Query

```sql
SELECT employee_id,
       last_name,
       salary,
       ROUND(salary * 1.155) AS "New Salary",
       ROUND(salary * 1.155) - salary AS "Increase"
FROM employees;
```

#### Result

The output now contains five columns: employee ID, last name, original salary, new salary, and the increment amount.
The **Increase** column clearly shows the monetary difference due to the raise.
For example, an employee with salary `10000` would show an increase of `1550`.

---

## Question 4

### Display the employee names with the first letter in uppercase and remaining letters in lowercase. Also display the length of each name for employees whose names begin with J, A, or M.

#### Explanation

The **INITCAP()** function automatically capitalizes the first letter of each name and lowercases the rest.
The **LENGTH()** function counts the total number of characters in the last name.
The **WHERE** clause uses **SUBSTR()** and **UPPER()** to filter only employees whose last names start with J, A, or M.

#### SQL Query

```sql
SELECT INITCAP(last_name) AS Name,
       LENGTH(last_name) AS Length
FROM employees
WHERE UPPER(SUBSTR(last_name,1,1)) IN ('J','A','M')
ORDER BY last_name;
```

#### Result

Only employees whose last names begin with J, A, or M are displayed in the output.
Each name appears in proper case format (e.g., `Abel`, `James`, `Mourgos`).
The **Length** column shows the character count of each name, sorted alphabetically.

---

## Question 5

### Rewrite the previous query so that the user is prompted to enter the starting letter of the employee's last name.

#### Explanation

The **substitution variable** `&letter` allows the user to dynamically provide the starting letter at runtime.
The **UPPER()** function is applied to both the input and the column to make the search case-insensitive.
The **LIKE** operator with `%` wildcard matches all names that begin with the entered letter.

#### SQL Query

```sql
SELECT last_name
FROM employees
WHERE UPPER(last_name) LIKE UPPER('&letter') || '%';
```

#### Result

At runtime, the user is prompted with a dialog or input field to enter a single letter.
The query then filters and returns all employee last names that start with the entered character.
For example, entering `J` returns names like `Jones`, `Jain`, `Johnson`.

---

## Question 6

### Display the length of employment for each employee.

#### Explanation

The **MONTHS_BETWEEN()** function calculates the difference in months between two date values.
Here, it computes the duration from each employee's **hire_date** up to the current **SYSDATE**.
The result is ordered in ascending order to display employees with the least tenure first.

#### SQL Query

```sql
SELECT last_name,
       MONTHS_BETWEEN(SYSDATE, hire_date) AS MONTHS_WORKED
FROM employees
ORDER BY MONTHS_WORKED;
```

#### Result

Each row shows an employee's last name alongside the total number of months they have been employed.
The value returned is a decimal number (e.g., `245.67`) representing months including fractional days.
Results are sorted from the most recently hired employee to the longest-serving one.

---

## Question 7

### Create a report showing each employee's monthly salary and desired salary (Salary × 3). Label the column Dream Salaries.

#### Explanation

The **concatenation operator (`||`)** is used to combine text strings with column values into a single readable sentence.
Each sentence states the employee's name, their current monthly salary, and a desired salary calculated as three times the current value.
This format produces a human-readable narrative report from raw database data.

#### SQL Query

```sql
SELECT last_name || ' earns ' || salary ||
       ' monthly but wants ' || salary*3 AS "Dream Salaries"
FROM employees;
```

#### Result

Each row outputs a complete sentence in the format: `King earns 24000 monthly but wants 72000`.
The entire output is stored under a single column labeled **Dream Salaries**.
This demonstrates how string concatenation can be used to generate formatted text reports.

---

## Question 8

### Display the last name and salary. Format the salary to be 15 characters long and left-padded with the '$' symbol.

#### Explanation

The **LPAD()** function pads a string on the left side with a specified character until the total length reaches the given value.
Here, the salary is padded with `$` symbols to a total width of **15 characters**.
This creates a visual right-alignment effect, making salary values easier to compare in tabular output.

#### SQL Query

```sql
SELECT last_name,
       LPAD(salary,15,'$') AS SALARY
FROM employees;
```

#### Result

The **SALARY** column displays each value padded from the left with dollar signs to fill 15 characters.
For example, a salary of `24000` would appear as `$$$$$$$$$$24000`.
This formatting technique is commonly used in financial or payroll reporting for visual consistency.

---

## Question 9

### Display each employee's last name, hire date, and salary review date (the first Monday after six months of service).

#### Explanation

The **ADD_MONTHS()** function adds exactly six months to the employee's hire date.
The **NEXT_DAY()** function then finds the next occurrence of the specified weekday (Monday) after that calculated date.
Together, these two functions accurately determine each employee's first salary review eligibility date.

#### SQL Query

```sql
SELECT last_name,
       hire_date,
       NEXT_DAY(ADD_MONTHS(hire_date,6),'MONDAY') AS REVIEW
FROM employees;
```

#### Result

Each row shows the employee's name, original hire date, and their scheduled salary review date.
The **REVIEW** column always falls on a Monday, at least six months after the hire date.
For example, an employee hired on `17-JUN-03` would have a review date of the first Monday after `17-DEC-03`.

---

## Question 10

### Display the last name, hire date, and the day of the week on which the employee joined.

#### Explanation

The **TO_CHAR()** function converts a date value into a formatted string based on a specified format model.
Using the format `'Day'` returns the full name of the weekday (e.g., `Monday`, `Tuesday`).
The **ORDER BY** clause uses `'D'` format to sort results in day-of-week order (Sunday = 1 through Saturday = 7).

#### SQL Query

```sql
SELECT last_name,
       hire_date,
       TO_CHAR(hire_date,'Day') AS DAY
FROM employees
ORDER BY TO_CHAR(hire_date,'D');
```

#### Result

Each row displays the employee's last name, their hire date, and the full name of the day they were hired.
Results are sorted in chronological weekday order starting from Sunday through to Saturday.
For example, an employee hired on `17-JUN-03` would show `Tuesday` in the **DAY** column.

---

## Question 11

### Display employee names and commission amounts. If an employee does not receive commission, display "No Commission."

#### Explanation

The **NVL()** function substitutes a specified value whenever a column contains a **NULL**.
Since `commission_pct` stores NULL for non-commissioned employees, **TO_CHAR()** first converts the numeric value to a string for type consistency.
NVL then replaces any NULL string with the text `'No Commission'`.

#### SQL Query

```sql
SELECT last_name,
       NVL(TO_CHAR(commission_pct),'No Commission') AS COMM
FROM employees;
```

#### Result

Employees who earn a commission will display their commission percentage value (e.g., `0.2`, `0.35`).
Employees with no commission will display the text **No Commission** instead of a blank or NULL.
This makes the output more readable and user-friendly for HR or payroll reporting.

---

## Question 12

### Display the first eight characters of employee names and represent their salaries using asterisks.

#### Explanation

The **SUBSTR()** function extracts only the first 8 characters of the last name to standardize name display width.
The **RPAD()** function builds an asterisk bar where the number of `*` symbols represents `salary / 1000`, creating a visual chart.
The **TRUNC()** function ensures the division result is an integer before being used as the padding length.

#### SQL Query

```sql
SELECT RPAD(SUBSTR(last_name,1,8),8,' ') AS LAST_NAME,
       RPAD('*',TRUNC(salary/1000),'*') AS EMPLOYEES_AND_THEIR_SALARIES
FROM employees
ORDER BY salary DESC;
```

#### Result

The **LAST_NAME** column shows names truncated and padded to exactly 8 characters for uniform alignment.
The **EMPLOYEES_AND_THEIR_SALARIES** column displays a bar of asterisks proportional to the salary (1 star per 1000 units).
Results are sorted from highest to lowest salary, creating a descending visual salary chart.

---

## Question 13

### Using the DECODE function, display the grade of employees based on their JOB_ID.

#### Explanation

The **DECODE()** function works like a lookup table — it compares the `job_id` against a list of known values and returns the mapped grade.
Each matched job ID (`AD_PRES`, `ST_MAN`, `SA_REP`, `ST_CLERK`) is assigned a letter grade from A to D.
Any job ID that does not match the listed values receives a default grade of `'0'`.

#### SQL Query

```sql
SELECT last_name,
       job_id,
       DECODE(job_id,
              'AD_PRES','A',
              'ST_MAN','B',
              'SA_REP','C',
              'ST_CLERK','D',
              '0') AS GRADE
FROM employees;
```

#### Result

Each employee row now includes a **GRADE** column alongside their name and job ID.
Employees with matching job IDs receive grades A through D based on the mapping defined in DECODE.
Employees whose job IDs are not listed in the DECODE receive a grade of `0`.

---

## Question 14

### Rewrite the previous query using the CASE statement.

#### Explanation

The **CASE** expression is the ANSI SQL-standard alternative to Oracle's proprietary **DECODE** function.
It evaluates each `WHEN` condition against `job_id` and returns the corresponding `THEN` value upon a match.
The `ELSE` clause handles unmatched cases, just like the default value in DECODE, and the `END` keyword closes the CASE block.

#### SQL Query

```sql
SELECT last_name,
       job_id,
       CASE job_id
           WHEN 'AD_PRES' THEN 'A'
           WHEN 'ST_MAN' THEN 'B'
           WHEN 'SA_REP' THEN 'C'
           WHEN 'ST_CLERK' THEN 'D'
           ELSE '0'
       END AS GRADE
FROM employees;
```

#### Result

The output is identical to the DECODE version — each employee displays their name, job ID, and assigned grade.
The **CASE** expression is generally preferred over DECODE for its improved readability and ANSI SQL portability.
Both queries produce the same grading result, confirming that CASE and DECODE are functionally equivalent here.

---

## Conclusion

This hands-on assignment demonstrates the practical use of SQL **Built-in Functions** across the following categories:

| Category | Functions Used |
|:---------|:---------------|
| **Character Functions** | `INITCAP`, `LENGTH`, `SUBSTR`, `LPAD`, `RPAD` |
| **Number Functions** | `ROUND`, `TRUNC` |
| **Date Functions** | `SYSDATE`, `ADD_MONTHS`, `NEXT_DAY`, `MONTHS_BETWEEN` |
| **Conversion Functions** | `TO_CHAR` |
| **Null Handling Functions** | `NVL` |
| **Conditional Functions** | `DECODE`, `CASE` |

These built-in functions simplify data formatting, calculations, and reporting — making SQL queries more efficient, readable, and user-friendly in real-world database applications.
