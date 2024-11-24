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



# PRACTICS