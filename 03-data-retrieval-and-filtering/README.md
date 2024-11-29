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


# PRACTICS