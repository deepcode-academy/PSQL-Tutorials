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

- Date/Time Functions:
  - `NOW()`, `AGE()`, `DATE_PART()`

**Example:**

```sql
SELECT employee_id, AGE(NOW(), hire_date) AS years_of_service
FROM employees;
```

- JSON Functions:
  - `JSONB_EXTRACT_PATH_TEXT()`, `JSON_AGG()`

**Example:**

```sql
SELECT JSON_AGG(department) AS departments
FROM employees;
```

6. Recursive Queries
   - Rekursiv soʻrovlar `hierarchical` yoki `tree-structured` maʼlumotlar bilan ishlash imkonini beradi.

**Example:**

```sql
WITH RECURSIVE employee_hierarchy AS (
    SELECT employee_id, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.employee_id, e.manager_id, eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.employee_id
)
SELECT * FROM employee_hierarchy;
```

Bu soʻrov `hierarchy of employees` va ularning darajasini shakllantiradi.

7. Combining Data with `JOIN` and `Aggregates`
   - `Aggregate Functions` bir nechta jadvallar bilan ishlashda ham foydali.

**Example:**

```sql
SELECT d.department_name, COUNT(e.employee_id) AS total_employees
FROM departments d
LEFT JOIN employees e ON d.department_id = e.department_id
GROUP BY d.department_name;
```

- Bu har bir bo‘limdagi ishchilar sonini, ishchi bo‘lmagan bo‘limlarni ham qo‘shib, chiqaradi.

# GROUPING DATA WITH `GROUP BY`

> [!NOTE]
> `GROUP BY` operatori **PostgreSQL**da ma’lumotlarni guruhlash uchun ishlatiladi. Bu operator ko‘pincha `Aggregate Functions` (`COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`) bilan birgalikda qo‘llaniladi

**Syntax:**

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

## SIMPLE EXAMPLE: GROUPING EMPLOYEES BY DEPARTMENT

**Table:** `employees`

| id | name    | department | salary |
|----|---------|------------|--------|
| 1  | Alice   | HR         | 5000   |
| 2  | Bob     | IT         | 7000   |
| 3  | Charlie | IT         | 6000   |
| 4  | Diana   | HR         | 5500   |
| 5  | Eve     | Sales      | 8000    |

**Goal:**

Har bir bo‘limdagi ishchilar sonini aniqlash.

**Query:**

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

**Result:**

| department | employee_count |
|------------|----------------|
| HR         | 2              |
| IT         | 2              |
| Sales      | 1              |

## USING AGGREGATE FUNCTIONS

- Har bir bo‘limning o‘rtacha maoshi

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

**Result:**

| department | average_salary |
|------------|----------------|
| HR         | 5250           |
| IT         | 6500           |
| Sales      | 8000           |

- Har bir bo‘limdagi eng katta maosh

```sql
SELECT department, MAX(salary) AS max_salary
FROM employees
GROUP BY department;
```

| department | average_salary |
|------------|----------------|
| HR         | 5500           |
| IT         | 7000           |
| Sales      | 8000           |

## GROUPING BY MULTIPLE COLUMNS

| id | region | product | revenue |
|----|--------|---------|---------|
| 1  | North  | Laptop  | 15000   |
| 2  | North  | Phone   | 20000   |
| 3  | South  | Laptop	 | 12000   | 
| 4  | South  | Phone   | 25000   | 
| 5  | East   | Laptop  | 10000   | 

**Goal:**

- Har bir mintaqa va mahsulot bo‘yicha umumiy daromadni hisoblash.

```sql
SELECT region, product, SUM(revenue) AS total_revenue
FROM sales
GROUP BY region, product;
```

| region | product  | 	total_revenue |
|--------|----------|----------------|
| North  | Laptop   | 	15000         |
| North  | Phone    | 	20000         |
| South  | Laptop   | 	12000         |
| South  | Phone    | 	25000         |
| East   | Laptop   | 	10000         |


## USING `HAVING` WITH `GROUP BY`

- Har bir bo‘limdagi ishchilar soni `1` dan katta bo‘lgan holatlar
- HAVING agregat funksiyalar natijasiga cheklov qo‘yish uchun ishlatiladi.

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

**Result:**

| department | employee_count |
|------------|----------------|
| HR         | 2              |
| IT         | 2              |

# FILTERING GROUPED DATA WITH `HAVING`

> [!NOTE]
> SQLda `HAVING` clause `GROUP BY` orqali guruhlangan maʼlumotlarni filtrlash uchun ishlatiladi. `WHERE` clausedan farqli o‘laroq, `HAVING` faqat guruhlash va agregatsiya (masalan, `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`)dan keyin qo‘llaniladi.

**Syntax:**

```sql
SELECT column1, column2, AGGREGATE_FUNCTION(column3)
FROM table_name
WHERE condition
GROUP BY column1, column2
HAVING aggregate_condition;
```

**Key Points:**

1. `WHERE` - guruhlashdan oldin qatorlarni filtrlash uchun ishlatiladi.
2. `GROUP BY` - qatorlarni belgilangan ustunlarga asoslangan holda guruhlash uchun ishlatiladi.
3. `HAVING` - guruhlangan maʼlumotlarni agregatsiya funksiyalariga asoslangan holda filtrlash uchun ishlatiladi.

**Example 1:** Filtering Sales Data

**Table:** Sales

| Salesperson | Region | Sales |
|-------------|--------|-------|
| Alice       | East   | 500   |
| Bob         | East	| 700   |
| Alice       | West   | 300   |
| Bob         | West   | 400   |
| Alice       | East   | 200   |
| Bob         | East   | 600   |

**Query:** Umumiy sotuv miqdori 1000 dan oshgan hududlarni ko‘rsating.

```sql
SELECT Region, SUM(Sales) AS Total_Sales
FROM Sales
GROUP BY Region
HAVING SUM(Sales) > 1000;
```
**Result:**

|Region| Total_Sales |
|------|-------------|
|East  | 2000        |


**Example 2:** Counting Employees in Departments

**Table:** Employees

| EmployeeID | Department | Salary |
|------------|------------|--------|
| 1          | IT         | 800    |
| 2          | HR         | 500    |
| 3          | IT         | 900    |
| 4          | Sales      | 600    |
| 5          | IT         | 700    |
| 6          | Sales      | 500    |

**Query:** 2 dan ortiq xodimga ega bo‘lgan bo‘limlarni ko‘rsating.

```sql
SELECT Department, COUNT(EmployeeID) AS Employee_Count
FROM Employees
GROUP BY Department
HAVING COUNT(EmployeeID) > 2;
```

**Result:**

| Department | Employee_Count |
|------------|----------------|
| IT         | 3              |

**Example 3:** Average Salary by Department

**Query:** O‘rtacha maosh 600 dan oshgan bo‘limlarni ko‘rsating.

```sql
SELECT Department, AVG(Salary) AS Average_Salary
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > 600;
```
**Result:**

| Department | Average_Salary |
|------------|----------------|
| IT         | 800            |