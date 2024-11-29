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

```postgresql
SELECT department, COUNT(*) AS employee_count, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC;
```




