# Advanced Data Retrieval with Functions and Aggregation

- Topics:
  - Using PostgreSQL functions `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`
  - Grouping data with `GROUP BY`
  - Filtering grouped data with `HAVING`

> [!NOTE]
> **PostgreSQL**da murakkab soʻrovlar va maʼlumotlarni samarali boshqarish `aggregation functions` hamda `advanced functions`ni ishlatishni talab qiladi.


# USING PostgreSQL FUNCTIONS `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`

1. Aggregate Functions
    - Agregatsiya funksiyalari bir necha qator ustida hisob-kitob olib boradi va bitta natija qaytaradi.

- `COUNT()`: Counts rows.
- `SUM()`: Calculates the total sum.
- `AVG()`: Calculates the average.
- `MAX()` and `MIN()`: Find the highest and lowest values.

```sql
SELECT department, COUNT(*) AS employee_count, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC;
```
- Bu soʻrov bo‘limlar bo‘yicha xodimlarni `guruhlab`, ularning sonini va `o‘rtacha ish haqini` hisoblaydi.

2. Filtering Aggregated Data
   - Agregatsiyalangan maʼlumotlarni filtrlash uchun `HAVING` operatoridan foydalaniladi.

**Example:**

```sql
SELECT department, SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 500000;
```
- Bu soʻrov umumiy ish haqi `500,000` dan yuqori boʻlgan bo‘limlarni chiqaradi.

3. Window Functions
   - Window functions operate on a set of rows related to the current row but do not reduce the result

**Example:**

```sql
SELECT 
    employee_id,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank
FROM employees;
```

- Bu soʻrov har bir bo‘limdagi ishchilarni ish haqi boʻyicha tartiblaydi.

4. Using Common Table Expressions `CTEs`
   - `CTE` murakkab soʻrovlarni soddalashtiradi va tushunishni osonlashtiradi.

**Example:**

```sql
WITH department_salaries AS (
    SELECT department, SUM(salary) AS total_salary
    FROM employees
    GROUP BY department
)
SELECT department
FROM department_salaries
WHERE total_salary > 500000;
```
- Bu misolda umumiy ish haqlari hisoblanib, saralangan bo‘limlar chiqariladi.

5. Advanced SQL Functions
   - **PostgreSQL**da maʼlumotlarni boshqarish uchun juda koʻp qurilgan funksiyalar mavjud.

- String Functions:
  - `CONCAT()`, `SUBSTRING()`, `LOWER()`, `UPPER()`

**Example:**

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```
