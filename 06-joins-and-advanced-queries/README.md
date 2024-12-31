# Joins and Advanced Queries

- Topics:
  - Types of joins: `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`
  - Combining tables with joins
  - Subqueries and nested queries

## Types of joins: INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN

> [!NOTE]
> `JOIN` operatori SQLda ikki yoki undan ortiq jadvallarni bog'lash va ular orasidagi ma'lumotlarni birlashtirish uchun ishlatiladi. JOIN yordamida ma'lumotlar o'rtasidagi bog'lanishlarni tahlil qilib, kerakli ma'lumotlarni olish mumkin.

| student_id | name   | group_id |
|------------|--------|----------|
| 1          | Ali    | 101      |
| 2          | Umid   | 102      |
| 3          | Hasan  | NULL     |
| 4          | Kamola | 103      |


| group_id | group_name  |
|----------|-------------|
| 101      | Matematika  |
| 102      | Fizika      |
| 103      | Informatika |
| 104      | Biologiya   |

## INNER JOIN

`INNER JOIN` faqat ikkala jadvalda mos keladigan ma'lumotlarni qaytaradi.

### Syntax

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```