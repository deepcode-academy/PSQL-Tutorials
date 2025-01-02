# Joins and Advanced Queries

- Topics:
  - Types of joins: `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`
  - Combining tables with joins
  - Subqueries and nested queries

## Types of joins: INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN

> [!NOTE]
> `JOIN` operatori SQLda ikki yoki undan ortiq jadvallarni bog'lash va ular orasidagi ma'lumotlarni birlashtirish uchun ishlatiladi. JOIN yordamida ma'lumotlar o'rtasidagi bog'lanishlarni tahlil qilib, kerakli ma'lumotlarni olish mumkin.

`students` table

| student_id | name   | group_id |
|------------|--------|----------|
| 1          | Ali    | 101      |
| 2          | Umid   | 102      |
| 3          | Hasan  | NULL     |
| 4          | Kamola | 103      |

`groups` table

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

- Chap jadvaldagi barcha ma'lumotlarni qaytaradi va o'ng jadvaldan mos kelmaydigan joylarga `NULL` qo'shadi.

```sql
SELECT students.student_id, students.name, groups.group_name
FROM students
LEFT JOIN groups
ON students.group_id = groups.group_id;
```

## RIGHT JOIN

- O'ng jadvaldagi barcha ma'lumotlarni qaytaradi va chap jadvaldan mos kelmaydigan joylarga `NULL` qo'shadi.

```sql
SELECT students.student_id, students.name, groups.group_name
FROM students
RIGHT JOIN groups
ON students.group_id = groups.group_id;
```

## FULL JOIN

- Ikkala jadvaldagi barcha ma'lumotlarni qaytaradi. Mos kelmagan joylarga `NULL` qo'shiladi.

```sql
SELECT students.student_id, students.name, groups.group_name
FROM students
FULL JOIN groups
ON students.group_id = groups.group_id;
```

# Combining tables with joins

> [!NOTE]
> JOIN operatori yordamida ikki yoki undan ortiq jadvalni o'zaro bog'langan ustunlar (keys) orqali birlashtirish mumkin. Bu ma'lumotlar orasidagi mantiqiy bog'lanishni ko'rish imkonini beradi.


## INNER JOIN

- Bu `JOIN` faqat ikki jadvalda ham mos keladigan (bog'langan) ma'lumotlarni olib keladi.

```sql
SELECT students.name, groups.group_name
FROM students
INNER JOIN groups
ON students.group_id = groups.group_id;
```

## LEFT JOIN

- `LEFT JOIN` asosiy jadval (chapdagi jadval)dagi barcha malumotlarni oladi va o'ng jadvaldagi mos keladigan malumotlarni birlashtiradi. Agar mos kelmasa, `NULL` qiymatni qaytaradi.

```sql
SELECT students.name, groups.group_name
FROM students
LEFT JOIN groups
ON students.group_id = groups.group_id;
```

## RIGHT JOIN

- `RIGHT JOIN` o'ng jadvaldagi barcha malumotlarni oladi va chap jadvaldagi mos keladigan malumotlarni birlashtiradi. Agar mos kelmasa, `NULL` qiymatni qaytaradi.

```sql
SELECT students.name, groups.group_name
FROM students
RIGHT JOIN groups
ON students.group_id = groups.group_id;
```

## FULL OUTER JOIN

- `FULL OUTER JOIN` ikki jadvaldagi barcha malumotlarni oladi, mos kelmasa, `NULL` bilan to'ldiradi.

```sql
SELECT students.name, groups.group_name
FROM students
FULL OUTER JOIN groups
ON students.group_id = groups.group_id;
```

# Subqueries and nested queries

> [!NOTE]
> SQLda `subqueries` (ichki so'rovlar) yoki `nested queries` (ichama-ich so'rovlar) boshqa bir so'rov ichida yoziladigan so'rovlar hisoblanadi. Subquery asosiy so'rov uchun kerakli ma'lumotni olishga yordam beradi. Subqueries SQL buyruqlarining `SELECT`, `FROM`, yoki `WHERE` qismida ishlatilishi mumkin.

## Key Characteristics of Subqueries

1. Subquery har doim qavslar `( )` ichida yozilishi kerak.
2. Subquery `bitta qiymat`, `bitta qator` yoki `butun jadval`ni qaytarishi mumkin.
3. U quyidagi joylarda ishlatiladi:
   - `SELECT`: hisoblangan qiymatlarni olish uchun.
   - `WHERE`: filterlash shartlarini belgilash uchun.
   - `FROM`: vaqtinchalik jadval sifatida ishlatish uchun.
4. Subqueries turlari:
   - Uncorrelated Subqueries:
   - Correlated Subqueries:


# TASKS

1. `employees` table

| employee_id | name    | department_id | salary   | join_date   |
|-------------|---------|---------------|----------|-------------|
| 1           | Ali     | 1             | 1200.50  | 2020-05-15  |
| 2           | Vali    | 2             | 1500.00  | 2019-06-01  |
| 3           | Sodiq   | NULL          | 1100.75  | 2021-08-20  |
| 4           | Nozima  | 3             | 1300.00  | 2020-03-12  |
| 5           | Shavkat | 1             | 1250.00  | 2018-11-23  |
| 6           | Madina  | 4             | 1400.30  | 2022-02-10  |
| 7           | Jasur   | 2             | 1600.45  | 2019-12-01  |
| 8           | Karima  | 5             | 1350.00  | 2023-07-01  |
| 9           | Sarvar  | NULL          | 1000.00  | 2021-01-15  |
| 10          | Malika  | 3             | 1450.00  | 2022-10-25  |

2. `departments` table

| department_id | department_name | manager |
|---------------|-----------------|---------|
| 1             | HR              | Akbar   | 
| 2             | IT              | Dilshod | 
| 3             | Marketing       | Ravshan | 
| 4             | Finance         | Aziza   | 
| 5             | Operations      | Nigora  | 
| 6             | Logistics       | Kamol   |
