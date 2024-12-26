# Data Manipulation

- Topics:
  - Updating records with `UPDATE`
  - Deleting records with `DELETE`
  - Managing data integrity with constraints (foreign keys, unique)
  - Transactions and rollback

## Updating records with UPDATE

> [!NOTE]
> PostgreSQLda ma'lumotlarni yangilash uchun `UPDATE` operatoridan foydalaniladi. Bu operator jadvaldagi mavjud malumotlarni yangilash uchun ishlatiladi.

### Syntax of UPDATE:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

- **Key Points:**
  - `SET`: Qaysi ustunni yangi qiymat bilan yangilashni belgilaydi.
  - `WHERE`: Qaysi malumotni yangilashni aniqlash uchun ishlatiladi. Agar `WHERE` shartini qo'shmasangiz, jadvaldagi barcha malumotlar yangilanadi.
  - **Conditions**: Agar kerak bo'lsa, bir nechta shartlarni birlashtirish uchun AND, OR mantiqiy operatorlarini ishlatish mumkin.

| client_id | first_name | last_name  | date_of_birth | email               | phone         | address            | city       | country     | postal_code | registration_date | last_activity_date | balance | status |
|-----------|------------|------------|---------------|---------------------|---------------|--------------------|------------|-------------|-------------|-------------------|--------------------|---------|--------|
| 1         | Ali        | Karimov    | 1990-01-01    | ali.karimov@mail.uz | +99890123456  | Tashkent, Block 15 | Tashkent   | Uzbekistan  | 100100      | 2024-01-01        | 2024-12-01         | 150.50  | TRUE   |
| 2         | Dilnoza    | Tursunova  | 1985-05-12    | dilnoza@mail.uz     | +99890123457  | Samarkand, Block 7 | Samarkand  | Uzbekistan  | 140200      | 2024-02-15        | 2024-11-20         | 0.00    | FALSE  |
| 3         | Bekzod     | Rasulov    | 1992-03-22    | bekzod@mail.uz      | +99890123458  | Bukhara, Block 9   | Bukhara    | Uzbekistan  | 200300      | 2024-05-10        | 2024-10-30         | 250.75  | TRUE   |


1. Basic Update Example

- `id = 2` bo'lgan clientni `city` ustunini `Navoiy` ga o'zgartiramiz:

```sql
UPDATE clients
SET city = 'Navoiy'
WHERE client_id = 2;
```

2. Updating Multiple Columns

- Agar bir vaqtning o'zida bir nechta ustunlarni yangilash kerak bo'lsa:

```sql
UPDATE clients
SET city = 'Navoiy', balance = 250.75
WHERE first_name = 'Ali';
```

3. Updating All Rows

- Agar jadvaldagi barcha qatorlarni yangilash kerak bo'lsa:

```sql
UPDATE clients
SET city = 'Navoiy';
```

4. Conditional Updates with `AND` and `OR`

- `city = 'Toshkent'` va `status = TRUE` bo'lgan clientning `phone` ustunini `+998930850955` ga o'zgartiramiz:

```sql
UPDATE clients
SET phone = '+998930850955'
WHERE city = 'Toshkent' AND status = TRUE;
```

## Deleting records with DELETE

> [!NOTE]
> PostgreSQLda ma'lumotlarni o'chirish uchun `DELETE` operatoridan foydalaniladi.


### Syntax of the DELETE Statement

```sql
DELETE FROM table_name
WHERE condition;
```

- `table_name`: O'chirish amalga oshiriladigan jadval nomi.
- `condition`: Qaysi satrlarni o'chirish kerakligini belgilaydigan shart.

> [!CAUTION]
> Agar `WHERE` sharti ko'rsatilmasa, jadvaldagi barcha yozuvlar o'chiriladi. Bunda ehtiyot bo'lish kerak!

1.  Deleting specific records based on a condition

- Agar jadvaldan faqat aniq bir shartga mos malumotni o'chirish kerak bo'lsa, `WHERE` sharti qo'llaniladi.

```sql
DELETE FROM clients
WHERE client_id = 2;
```

2. Deleting all records

Agar barcha malumotlarni o'chirish kerak bo'lsa, `WHERE` sharti berilmaydi.

```sql
DELETE FROM clients;
```

## Managing data integrity with constraints (foreign keys, unique)

### Foreign Key

- Foreign key bir jadvaldagi ustun boshqa jadvaldagi ustunga bog'liq bo'lishini ta'minlaydi. Bu jadvallarni bir biriga bog'lash uchun ishlatiladi ishlatiladi.

### Customers Table

| customer_id | name            | email                |
|-------------|-----------------|----------------------|
| 1           | Anvar Aliyev    | aliyev@example.com   |
| 2           | Nodira Karimova | karimova@example.com |

### Orders Table

| order_id | customer_id | order_date  | amount  |
|----------|-------------|-------------|---------|
| 1        | 1           | 2024-12-25  | 150.00  |
| 2        | 2           | 2024-12-26  | 200.50  |


```sql
-- Creating the Customers table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY, -- Primary key
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL -- Email must be unique
);

-- Creating the Orders table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY, -- Primary key
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    amount NUMERIC(10, 2) NOT NULL,
    CONSTRAINT fk_customer FOREIGN KEY (customer_id)
    REFERENCES customers (customer_id) ON DELETE CASCADE
);
```

# PRACTICS

1. Sizda `students` nomli jadval mavjud. Jadvalda quyidagi ustunlar bor:
   - **id (integer)** - talabalar id raqamlari (PRIMARY KEY)
   - **name (varchar)** - talabaning ismi
   - **age (integer)** - talabaning yoshi
   - **grade (varchar)** - talabaning bahosi (masalan, A, B, C, D)
   - **city (varchar)** - talabaning yashash shahri

### Data

| id | name    | age | grade | city      |
|----|---------|-----|-------|-----------|
| 1  | Ali     | 19  | B     | Tashkent  |
| 2  | Salim   | 18  | C     | Samarkand |
| 3  | Malika  | 20  | A     | Bukhara   |
| 4  | Shoxruh | 21  | D     | Tashkent  |
| 5  | Laylo   | 22  | C     | Nukus     |

### Tasks

- `Tashkent` shahrida yashovchi barcha talabalarni toping va ularning `grade` ustunini `B` ga o'zgartiring.
- Yoshi `20` yoki undan katta bo'lgan talabalarni toping va ularning yashash shahrini `Tashkent` ga o'zgartiring.
- `grade` ustuni `C` bo'lgan barcha talabalar uchun `age` ni `1` yoshga oshiring.


2. Sizda `employees` nomli jadval bor. Jadvalda quyidagi ustunlar mavjud:
   - **id (integer)** - xodimning id raqami (asosiy kalit, PRIMARY KEY)
   - **name (varchar)** - xodimning ismi
   - **department (varchar)** - xodim ishlaydigan bo'lim (masalan, HR, IT, Sales)
   - **salary (integer)** - xodimning oyligi
   - **experience (integer)** - xodimning ish tajribasi (yillarda)
   - **city (varchar)** - xodim yashayotgan shahar

| id | name     | department | salary | experience | city       |
|----|----------|------------|--------|------------|------------|
| 1  | Nodir    | IT         | 1200   | 3          | Tashkent   |
| 2  | Malika   | HR         | 900    | 5          | Samarkand  |
| 3  | Shoxruh  | Sales      | 1500   | 7          | Bukhara    |
| 4  | Laylo    | IT         | 1000   | 2          | Nukus      |
| 5  | Kamol    | HR         | 850    | 6          | Tashkent   |
| 6  | Saida    | Sales      | 1100   | 4          | Andijan    |

### Tasks
- Toshkent shahridagi barcha xodimlarning oyliklarini `10%` ga oshiring.
- Tajribasi `5` yildan oshgan xodimlarni toping va ularning bo'limini `"Senior"` qilib yangilang.
- Oyligi `1000` dan kam bo'lgan barcha xodimlarni toping va ularning oyligini `950` ga o'zgartiring.
- `"Sales"` bo'limida ishlaydigan xodimlarning shaharlarini `"Tashkent"` qilib yangilang.
- `"IT"` bo'limida ishlaydigan, tajribasi `3` yildan kam bo'lgan xodimlarning oyliklarini `20%` ga oshiring.

