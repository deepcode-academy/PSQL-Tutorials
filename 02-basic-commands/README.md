# Basic SQL Commands in PostgreSQL

- Topics:
  - `databases`, `tables`, `rows` va `columns` haqida tushuncha
  - Data types in `PostgreSQL`
  - Asosiy `SQL` buyruqlari: `CREATE DATABASE`, `CREATE TABLE`, `DROP`, `INSERT`, `SELECT`
  - Primary keys and constraints

> [!NOTE]
> `Databases`, `tables`, `rows` va `columns` — bu `SQL` va ma'lumotlar bazalari boshqaruvi tizimlarining asosiy tushunchalar hisoblanadi.

# `databases`, `tables`, `rows` VA `columns` HAQIDA TUSHUNCHA

1. Database
   - **Database** — bu bir joyda to'plangan va mantiqiy tartibda saqlangan ma'lumotlarning yig'indisi. Ma'lumotlar bazasi yordamida biz katta hajmdagi ma'lumotlarni `boshqarish`, `saqlash`, `yangilash` va ulardan samarali foydalanish imkoniyatiga ega bo'lamiz. 

   - **Masalan:** Kompaniya o'z mijozlari, mahsulotlari, buyurtmalari haqidagi ma'lumotlarni ma'lumotlar bazasida saqlaydi.

2. Table
   - **Table** — ma'lumotlarni `struktura` shaklida saqlash uchun ma'lumotlar bazasidagi asosiy obyektlardan biri. Har bir jadvalda columns(ustunlar) va rows(qatorlar) mavjud bo'lib, ularning yordamida ma'lumotlarni `tartiblangan` shaklda saqlash va ularni `tezkor topish` mumkin. 

     - **Masalan:** Talabalar jadvali quyidagi ma'lumotlarni o'zida saqlashi mumkin: `talabalar ID` raqami, `ism`, `familiya`, `yosh` va `guruh`.

3. Column
   - **Column** — jadvalda saqlanadigan `ma'lumotlarning turini` belgilaydigan element. Har bir ustun jadvaldagi ma'lumotlarga ma'lum bir tavsif beradi va unda ma'lum turdagi ma'lumotlar (`raqamlar`, `matn`, `sana` va h.k.) saqlanadi. 

   - **Masalan:** `ism`, `familiya` va `yosh` — bu talabalar jadvalidagi ustunlardir.

4. Row
    - **Row** — jadvaldagi ma'lumotlarning bir birlikdagi ko'rinishi bo'lib, har bir qator alohida bir yozuvni ifodalaydi. 
    - **Masalan:** talabalar jadvalida har bir qator bitta talaba haqida ma'lumot saqlaydi: talaba `ID` raqami, `ismi`, `yoshi` va `guruh`.

**Example:**

Agar bizda `students` nomli jadval bo'lsa, uning tarkibi quyidagicha bo'lishi mumkin:

|ID | Name        | Surname       | Age    | Class |
|---|-------------|---------------|--------|-------|
|1  | Ali         | Valiyev       | 20     | A1    |
|2  | Madina      | Ismailova     | 21     | A2    |
|3  | Bekzod      | Karimov       | 22     | A3    |

- **Database** - bunda bizning `"University"` nomli ma'lumotlar bazamiz bor deb tasavvur qilaylik.
- **Table** - `students` jadvali.
- **Columns** - `ID`, `Name`, `Surname`, `Age`, `Class`.
- **Rows** - har bir talaba haqida ma'lumotni ifodalovchi yozuvlar.

# DATA TYPES IN PostgreSQL

> [!NOTE]
> PostgreSQLda ma'lumot turlari (data types) ma'lumotlar bazasidagi ustunlar uchun qiymatlarni saqlash va ishlov berishning turli usullarini ta'minlaydi. Quyida PostgreSQLda ishlatiladigan asosiy ma'lumot turlari keltirilgan:

# Asosiy `SQL` buyruqlari: `CREATE DATABASE`, `CREATE TABLE`, `DROP`, `INSERT`, `SELECT`

## CREATE DATABASE
`CREATE DATABASE`: Yangi database yaratadi.
```sql
CREATE DATABASE database_name;
```

**Example:**
```sql
CREATE DATABASE School;
```

## CREATE TABLE

`CREATE TABLE`: Ma'lumotlar bazasi ichida yangi table(jadval) yaratadi.

```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
);
```
**Example:**
```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    Age INT,
    Grade VARCHAR(10)
);
```

|StudentID|Name| Age |Grade|
|---------|----|-----|-----|
|         |    |     |     | 


## DROP

`DROP`: Ma'lumotlar bazasini yoki jadvalni o‘chirib tashlaydi. Ushbu buyruqni ehtiyotkorlik bilan ishlating, chunki u barcha ma'lumotlarni o‘chiradi.

`Database`ni o‘chirish:
```sql
DROP DATABASE database_name;
```
`Table`ni o'chirish
```sql
DROP TABLE table_name;
```

**Example:**
```sql
DROP TABLE Students;
```

## INSERT

`INSERT`: Jadvalni elementlar bilan to'ldirish.

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

**Example:**

```sql
INSERT INTO Students (StudentID, Name, Age, Grade)
VALUES (1, 'John Doe', 20, 'A');
```

| StudentID | Name     | Age | Grade |
|-----------|----------|-----|-------|
| 1         | John Doe | 20  | A     | 

## SELECT

`SELECT`: Jadvaldan ma'lumotlarni chiqarib beradi. Maxsus ustunlarni yoki shartlarni belgilash mumkin.

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

**Example:**

```sql
SELECT Name, Age
FROM Students
WHERE Grade = 'A';
```

