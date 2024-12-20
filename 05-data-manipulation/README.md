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

1. Basic Update Example

- Bizda `students` degan jadval bor, quyidagi ustunlar bilan:

- **id**: Talaba ID raqami
- **name**: Talabaning ismi
- **age**: Talabaning yoshi
- **grade**: Talabaning darajasi

| id | name  | age | grade |
|----|-------|-----|-------|
| 1  | Ali   | 18  | A     |
| 2  | Vali  | 19  | B     |
| 3  | Komil | 17  | C     |

- Endi, `id = 2` bo'lgan talabaning `grade` ustunini `A` ga o'zgartiramiz:

```sql
UPDATE students
SET grade = 'A'
WHERE id = 2;
```

| id | name  | age | grade |
|----|-------|-----|-------|
| 1  | Ali   | 18  | A     |
| 2  | Vali  | 19  | A     |
| 3  | Komil | 17  | C     |


2. Updating Multiple Columns

- Agar bir vaqtning o'zida bir nechta ustunlarni yangilash kerak bo'lsa:

```sql
UPDATE students
SET age = 20, grade = 'B'
WHERE name = 'Komil';
```


| id | name  | age | grade |
|----|-------|-----|-------|
| 1  | Ali   | 18  | A     |
| 2  | Vali  | 19  | A     |
| 3  | Komil | 20  | B     |

3. Updating All Rows

- Agar jadvaldagi barcha qatorlarni yangilash kerak bo'lsa:

```sql
UPDATE students
SET grade = 'C';
```

| id | name  | age | grade |
|----|-------|-----|-------|
| 1  | Ali   | 18  | C     |
| 2  | Vali  | 19  | C     |
| 3  | Komil | 20  | C     |

4. Conditional Updates with `AND` and `OR`

- `age > 18` va `grade = 'A'` bo'lgan talabalarning `grade` ni `B` ga o'zgartiramiz:

```sql
UPDATE students
SET grade = 'B'
WHERE age > 18 AND grade = 'A';
```

5. Updating with Subqueries

- Boshqa jadval ma'lumotlaridan foydalanib qatorlarni yangilash mumkin.
- students jadvalidagi `grade` ustunini `grades` jadvalidan olib yangilaymiz:

| id | grade |
|----|-------|
| 1  | A     |
| 2  | B     |
| 3  | A     |


```sql
UPDATE students
SET grade = (SELECT grade FROM grades WHERE grades.id = students.id);
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

