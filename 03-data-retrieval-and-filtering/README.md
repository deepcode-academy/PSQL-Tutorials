# Data Retrieval and Filtering

- Topics:
  - `SELECT` statement with `WHERE` clause
  - Operators and conditions(`=`, `<`, `>`, `AND`, `OR`, `NOT`)
  - Sorting data with `ORDER BY`
  - Limiting results with `LIMIT`


# `SELECT` STATEMENT WITH `WHERE` CLAUSE

> [!NOTE]
> `SELECT` operatori va `WHERE` sharti yordamida `database`dan aniq shartlarga mos keladigan ma'lumotlarni tanlash mumkin.


## `SELECT` STATEMENT

`SELECT` SQLning eng asosiy operatorlaridan biri bo'lib, `database`dan ma'lumotlarni olish uchun ishlatiladi.

**Basic syntax:**

```sql
SELECT column1, column2, ...
FROM table_name;
```

`column1, column2, ...:` Qaysi ustunlarni tanlash kerakligini ko'rsatadi. Agar barcha ustunlarni tanlash kerak bo'lsa, `*` ishlatiladi.

`table_name:` Ma'lumot olingan jadvalning nomi.

## `WHERE` CLAUSE

`WHERE` ma'lum bir shartlarga asoslanib, qatorlarni filtrlaydi. Faqat `WHERE` shartiga mos keladigan qatorlar qaytariladi.

**Basic Examples:**

1. Malum bir shartga mos keladigan qatorlarni olish

Foydalanuvchilarning faqat 30 yoshdagilarini olish:

```sql
SELECT name, age
FROM users
WHERE age = 30;
```

2. Shartlar kombinatsiyasi `AND` va `OR`

`30` yoshdan katta va faqat `"Toshkent"` shahrida yashaydigan foydalanuvchilarni olish:

```sql
SELECT name, city, age
FROM users
WHERE age > 30 AND city = 'Toshkent';
```

3. Ma'lum bir oraliqda bo'lgan ma'lumotlarni olish

Yoshi `25` dan `40` gacha bo'lgan foydalanuvchilarni olish:

```sql
SELECT name, age
FROM users
WHERE age BETWEEN 25 AND 40;
```

4. Ma'lumotlarni o'xshashlik bilan qidirish `LIKE`

`"Ali"` bilan boshlanadigan ismlarni qidirish:

```sql
SELECT name
FROM users
WHERE name LIKE 'Ali%';
```

5. `NULL` qiymatlarni qidirish

Telefon raqami kiritilmagan foydalanuvchilarni olish:

```sql
SELECT name, phone
FROM users
WHERE phone IS NULL;
```

Additional Concepts

- `=`: Checks equality
- `>` or `<`: Checks greater than or less than.
- `!=` or `<>`: Checks inequality.
- `IN`: Checks if a value exists within a set of specified values.

**Example:** Using `IN`

Find users living in `"Tashkent"`, `"Samarkand"`, or `"Bukhara"`:
```sql
SELECT name, city
FROM users
WHERE city IN ('Tashkent', 'Samarkand', 'Bukhara');
```

# OPERATORS AND CONDITIONS `=`, `<`, `>`, `AND`, `OR`, `NOT`

> [!NOTE]
> `SQL operatorlari` va shartlari yordamida ma'lumotlarni `filtrlash`, `solishtirish` va `boshqarish` mumkin.

1. Equality Operators

- `=`: Checks if values are equal.

**Example:**

```sql
SELECT name, age
FROM users
WHERE age = 25;
```
**Explanation**: Returns users whose age is exactly 25.

- `!=` or `<>`: Checks if values are not equal.

**Example:**

```sql
SELECT name, age
FROM users
WHERE age != 25;
```

**Explanation**: Returns users whose age is not 25.

2. Comparison Operators

- `>`: Finds values greater than the specified value.

**Example:**

```sql
SELECT name, age
FROM users
WHERE age > 30;
```

- `<`: Finds values less than the specified value

**Example:**

```sql
SELECT name, age
FROM users
WHERE age < 18;
```

**Explanation**: Returns users younger than 18.

- `>=`: Finds values greater than or equal to the specified value.

**Example:**

```sql
SELECT name, age
FROM users
WHERE age >= 21;
```

- `<=`: Finds values less than or equal to the specified value.

**Example:**

```sql
SELECT name, age
FROM users
WHERE age <= 60;
```

3. Logical Operators

Mantiqiy operatorlar yordamida bir nechta shartlarni birlashtirish mumkin.

- `AND` Bir nechta shartlarning barchasi bajarilishini talab qiladi.

**Example:**

```sql
SELECT name, age, city
FROM users
WHERE age > 25 AND city = 'Tashkent';
```

4. Combining Conditions

`AND`, `OR`, va `NOT` operatorlari birgalikda ishlatilishi mumkin. Shartlarni o'qish oson bo'lishi uchun qavslar ishlatiladi.

**Example:**

```sql
SELECT name, age, city
FROM users
WHERE (age > 30 AND city = 'Tashkent') OR NOT age = 25;
```

**Explanation:**

- `30` yoshdan katta va `"Toshkent"` shahrida yashovchilar yoki
- `25` yoshda bo'lmagan foydalanuvchilarni qaytaradi.

5. Additional Operators

`BETWEEN`: Ma'lum bir oraliqdagi qiymatlarni tanlash.

**Example:**

```sql
SELECT name, age
FROM users
WHERE age BETWEEN 20 AND 30;
```
**Explanation:**

- `20` dan `30` yoshgacha bo'lgan foydalanuvchilarni qaytaradi.

`IN`: Bir nechta qiymatlar ro'yxatiga mos kelishini tekshiradi.

```sql
SELECT name, city
FROM users
WHERE city IN ('Tashkent', 'Samarkand', 'Bukhara');
```
**Explanation:**

- Foydalanuvchilar "Toshkent", "Samarqand" yoki "Buxoro" shahrida yashashi kerak.

`LIKE`: Qismiy moslikni tekshiradi (for string).

- `%`: Belgilar ketma-ketligini ifodalaydi.
- `_`: Bitta belgini bildiradi.

```sql
SELECT name
FROM users
WHERE name LIKE 'A%';
```
**Explanation:**

Ismi `"A"` harfi bilan boshlanadigan foydalanuvchilarni qaytaradi.

# SORTING DATA WITH `ORDER BY`

> [!NOTE]
> `ORDER BY` operatori ma'lumotlarni saralashda ishlatiladi. Bu operator ma'lumotlarni **ascending**(o'sish tartibida) yoki **descending**(kamayish tartibida) tartiblash imkoniyatini beradi.

## GENERAL SYNTAX

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column_name [ASC | DESC];
```
- `ASC`: Ma'lumotlarni o'sish tartibida saralaydi (standart qiymat).
- `DESC`: Ma'lumotlarni kamayish tartibida saralaydi.

## EXAMPLES WITH EXPLANATION

1. Simple Sorting
   - Aytaylik, `students` jadvalimiz bor va unda quyidagi ma'lumotlar saqlanadi:

| id | name      | score |
|----|-----------|-------|
| 1  | Ali       | 85    |
| 2  | Umid      | 92    |
| 3  | Bekzod    | 74    |
| 4  | Shirin    | 88    |

- O'sish tartibida saralash
  - Quyidagi `SQL` so'rovi ma'lumotlarni `score` ustuniga qarab o'sish tartibida saralaydi:

```sql
SELECT * 
FROM students 
ORDER BY score ASC;
```
**Result**:

| id | name   | score |
|----|--------|-------|
| 3  | Bekzod | 74    |
| 1  | Ali    | 85    |
| 4  | Shirin | 88    |
| 2  | Umid   | 92    |

- Kamayish tartibida saralash 
  - Quyidagi so'rov ma'lumotlarni `score` ustuniga qarab kamayish tartibida saralaydi:

```sql
SELECT * 
FROM students 
ORDER BY score DESC;
```

**Result**:

| id | name   | score |
|----|--------|-------|
| 2  | Umid   | 92    |
| 4  | Shirin | 88    |
| 1  | Ali    | 85    |
| 3  | Bekzod | 74    |


2. Sorting by Multiple Columns 
   - Agar bir nechta ustunlar bo'yicha saralash kerak bo'lsa, ular ketma-ket yoziladi.

**Example:**

`name` ustuni bo'yicha alfavit tartibida, agar ism bir xil bo'lsa, `score` kamayish tartibida saralanadi:

```sql
SELECT * 
FROM students 
ORDER BY name ASC, score DESC;
```
| id | name   | score |
|----|--------|-------|
| 1  | Ali    | 85    |
| 3  | Bekzod | 74    |
| 4  | Shirin | 88    |
| 2  | Umid   | 92    |


3. Limiting Results with Sorting
   - Agar faqat eng yuqori `3` ta natijani olish kerak bo'lsa:

**Example:**

```sql
SELECT * 
FROM students 
ORDER BY score DESC 
LIMIT 3;
```

| id | name   | score |
|----|--------|-------|
| 2  | Umid   | 92    |
| 4  | Shirin | 88    |
| 1  | Ali    | 85    |

4. Handling NULL Values
   - Agar jadvalda `NULL` qiymatlar bo'lsa, PostgreSQL ularni saralashda oxirida yoki boshida joylashtiradi. Buni boshqarish uchun `NULLS FIRST` yoki `NULLS LAST` ishlatiladi.

**Example:**

| id | name      | score  |
|----|-----------|--------|
| 1  | Ali       | 85     |
| 2  | Umid      | 92     |
| 3  | Bekzod    | NULL   |
| 4  | Shirin    | 88     |

Saralashda `NULL` qiymatlarni boshiga qo'yish:

```sql
SELECT * 
FROM students 
ORDER BY score ASC NULLS FIRST;
```
| id | name   | score |
|----|--------|-------|
| 3  | Bekzod | NULL  |
| 1  | Ali    | 85    |
| 4  | Shirin | 88    |
| 2  | Umid   | 92    |

# LIMITING RESULTS WITH `LIMIT`

> [!NOTE]
> `LIMIT` operatori maʼlumotlarni qaytarishda maʼlum bir son bilan cheklash uchun ishlatiladi. Bu, ayniqsa, ko‘p sonli natijalar bilan ishlaganda muhimdir, chunki `LIMIT` operatori faqat kerakli miqdordagi yozuvlarni qaytarishga imkon beradi.

`LIMIT` Syntax

```sql
SELECT column1, column2, ...
FROM table_name
LIMIT number_of_rows;
```

`number_of_rows`: Natijalar orasidan nechta yozuvni qaytarish kerakligini belgilaydi.

1. Fetching the First `5 Rows`
   - Tasavvur qilaylik, bizda `students` jadvali bor, va u quyidagi ustunlarni o‘z ichiga oladi: `id`, `name`, va `score`.

**Example:**

```sql
SELECT id, name, score
FROM students
LIMIT 5;
```
- Bu so‘rov `students` jadvalidan birinchi `5` ta `row`ni qaytaradi.

2. Using `LIMIT` with Sorting
   - Birinchi eng yuqori baholangan `3` nafar talabani ko‘rish uchun:

```sql
SELECT id, name, score
FROM students
ORDER BY score DESC
LIMIT 3;
```

- `ORDER BY score DESC`: Yozuvlarni eng yuqori bahodan pastga qarab tartiblaydi.
- `LIMIT 3`: Faqat `3` ta natijani qaytaradi.

3. Combining `LIMIT` and `OFFSET`
   - `OFFSET` operatori natijalardagi maʼlum bir sonli yozuvlarni o‘tkazib yuborishga imkon beradi.
   - Masalan, 6-`row`dan boshlab keyingi 5 ta `row`ni olish uchun:

```sql
SELECT id, name, score
FROM students
ORDER BY score DESC
LIMIT 5 OFFSET 5;
```

- `LIMIT 5`: Faqat `5` ta `row`ni qaytaradi.
- `OFFSET 5`: Birinchi `5` ta `row`ni o‘tkazib yuboradi.

4. Using `LIMIT` with `WHERE`
   - Faqat bahosi `80` dan katta bo‘lgan talabalarning birinchi `3` tasini olish:

```sql
SELECT id, name, score
FROM students
WHERE score > 80
ORDER BY score DESC
LIMIT 3;
```

5. `LIMIT` with `Aggregate Functions`
   - Birinchi 10 ta talabaning o‘rtacha bahosini hisoblash:

```sql
SELECT AVG(score) AS average_score
FROM (
    SELECT score
    FROM students
    ORDER BY score DESC
    LIMIT 10
) AS top_students;
```

- Bu so‘rov eng yuqori baholangan `10` talabaning `o‘rtacha` bahosini qaytaradi.


# PRACTICS

Here are some tasks based on the `SELECT` statement with the `WHERE` clause, along with a sample database structure:

## SAMPLE TABLE: `employees`

| id | name    | department | salary | hire_data   |
|----|---------|------------|--------|-------------|
| 1  | Alice   | HR         | 500    | 2021-01-15  |
| 2  | Bob     | IT         | 7000   | 2020-03-20  |
| 3  | Charlie | IT         | 6500   | 2019-07-01  |
| 4  | Diana   | Marketing  | 6000   | 2022-05-12  |
| 5  | Edward  | HR         | 5500   | 2020-09-30  |

## TASKS

1. Basic Filtering
   - `IT` bo‘limida ishlaydigan xodimlarni toping.
   - **Expected Output:** Rows with `Bob` and `Charlie`.

2. Filtering with `Numbers`
   - Maoshi `6000` dan katta bo‘lgan xodimlarni toping.
   - **Expected Output:** Rows with `Bob` and `Charlie`.

3. Filtering with `Dates`
   - 2020-yilning 1-yanvaridan keyin ishga qabul qilingan xodimlarni toping.
   - **Expected Output:** Rows with `Alice`, `Diana`, and `Edward`.

4. Combining Conditions
   - `HR` bo‘limida ishlaydigan va maoshi `6000` dan kam bo‘lgan xodimlarni toping.
   - **Expected Output:** Rows with `Alice` and `Edward`.

5. Using `OR` in Conditions
   - `HR` yoki `Marketing` bo‘limida ishlaydigan xodimlarni toping.
   - **Expected Output:** Rows with `Alice`, `Diana`, and `Edward`.

6. Negating Conditions
   - l
   - Expected Output: Rows with Alice, Diana, and Edward.

7. Using Wildcards `LIKE`
   - l
   - Expected Output: Row with Alice.

8. Using `IN` Clause
   - l
   - Expected Output: Rows with Alice, Bob, Charlie, and Edward.

