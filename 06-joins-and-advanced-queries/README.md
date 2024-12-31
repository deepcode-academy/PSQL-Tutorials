# Joins and Advanced Queries

- Topics:
  - Types of joins: `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`
  - Combining tables with joins
  - Subqueries and nested queries

## Types of joins: INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN

> [!NOTE]
> `JOIN` operatori SQLda ikki yoki undan ortiq jadvallarni bog'lash va ular orasidagi ma'lumotlarni birlashtirish uchun ishlatiladi. JOIN yordamida ma'lumotlar o'rtasidagi bog'lanishlarni tahlil qilib, kerakli ma'lumotlarni olish mumkin.

- students table

| student_id | name   | group_id |
|------------|--------|----------|
| 1          | Ali    | 101      |
| 2          | Umid   | 102      |
| 3          | Hasan  | NULL     |
| 4          | Kamola | 103      |

- group table

| group_id | group_name  |
|----------|-------------|
| 101      | Matematika  |
| 102      | Fizika      |
| 103      | Informatika |
| 104      | Biologiya   |

## INNER JOIN

- Faqat ikkala jadvalda mos keladigan ma'lumotlarni qaytaradi.

```sql
SELECT students.student_id, students.name, groups.group_name
FROM students
INNER JOIN groups
ON students.group_id = groups.group_id;
```

## LEFT JOIN

- Chap jadvaldagi barcha ma'lumotlarni qaytaradi va o'ng jadvaldan mos kelmaydigan joylarga NULL qo'shadi.

```sql
SELECT students.student_id, students.name, groups.group_name
FROM students
LEFT JOIN groups
ON students.group_id = groups.group_id;
```

## RIGHT JOIN

- O'ng jadvaldagi barcha ma'lumotlarni qaytaradi va chap jadvaldan mos kelmaydigan joylarga NULL qo'shadi.

```sql
SELECT students.student_id, students.name, groups.group_name
FROM students
RIGHT JOIN groups
ON students.group_id = groups.group_id;
```