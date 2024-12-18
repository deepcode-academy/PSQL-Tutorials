# Data Manipulation

- Topics:
  - Updating records with `UPDATE`
  - Deleting records with `DELETE`
  - Managing data integrity with constraints (foreign keys, unique)
  - Transactions and rollback

> [!NOTE]
> PostgreSQLda ma'lumotlarni yangilash uchun `UPDATE` operatoridan foydalaniladi. Bu operator jadvaldagi mavjud malumotlarni yangilash uchun ishlatiladi.

## Syntax of UPDATE:

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

Bizda `students` degan jadval bor, quyidagi ustunlar bilan:

- **id**: Talaba ID raqami
- **name**: Talabaning ismi
- **age**: Talabaning yoshi
- **grade**: Talabaning darajasi

| id | name  | age | grade |
|----|-------|-----|-------|
| 1  | Ali   | 18  | A     |
| 2  | Vali  | 19  | B     |
| 3  | Komil | 17  | C     |

Endi, `id = 2` bo'lgan talabaning `grade` ustunini `A` ga o'zgartiramiz:

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

Agar bir vaqtning o'zida bir nechta ustunlarni yangilash kerak bo'lsa:

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