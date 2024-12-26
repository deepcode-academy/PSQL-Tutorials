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
WHERE id = 2;
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

- `age > 18` va `grade = 'A'` bo'lgan talabalarning `grade` ni `B` ga o'zgartiramiz:

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

| id | name    | age | grade |
|----|---------|-----|-------|
| 1  | Ali     | 20  | A     |
| 2  | Dilshod | 22  | B     |
| 3  | Shirin  | 19  | A     |
| 4  | Zafar   | 21  | C     |

