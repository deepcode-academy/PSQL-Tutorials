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

# DATA TYPES IN `PostgreSQL`

> [!NOTE]
> PostgreSQLda ma'lumot turlari (data types) ma'lumotlar bazasidagi ustunlar uchun qiymatlarni saqlash va ishlov berishning turli usullarini ta'minlaydi. Quyida PostgreSQLda ishlatiladigan asosiy ma'lumot turlari keltirilgan:


1. Numeric Data Types(Raqamli ma'lumot turlari)

`INTEGER` (int, int4): 4 baytli butun sonlar (−2,147,483,648 dan +2,147,483,647 gacha).

**Example:**

```sql
CREATE TABLE Example (
    num INTEGER
);
```

`BIGINT` (int8): 8 baytli butun sonlar (−9,223,372,036,854,775,808 dan +9,223,372,036,854,775,807 gacha).

**Example:**

```sql
CREATE TABLE Example (
    big_num BIGINT
);
```

`SMALLINT` (int2): 2 baytli butun sonlar (−32,768 dan +32,767 gacha).

**Example:**

```sql
CREATE TABLE Example (
    small_num SMALLINT
);
```

`DECIMAL` yoki `NUMERIC`: Yuksak aniqlikka ega haqiqiy sonlar, masalan, valyutalarni saqlashda foydalidir.

**Sintaksis:** 

- `DECIMAL`(**precision**, **scale**)
  - **Precision:** Jami raqamlar soni.
  - **Scale:** Kasr qismidagi raqamlar soni.

**Example:**

```sql
CREATE TABLE Example (
    price DECIMAL(10, 2)
);
```

`REAL`: 4 baytli haqiqiy son (single precision floating point).

**Example:**

```sql
CREATE TABLE Example (
    value REAL
);
```

`DOUBLE PRECISION`: 8 baytli haqiqiy son (double precision floating point).

**Example:**

```sql
CREATE TABLE Example (
    value DOUBLE PRECISION
);
```

2.  Text Data Types(Matnli ma'lumot turlari)

`CHAR(n)`: Sabit uzunlikdagi matn. Agar matn uzunligi `n` dan qisqa bo'lsa, qolgan joylar bo'shliq bilan to'ldiriladi.

**Example:**

```sql
CREATE TABLE Example (
    code CHAR(5)
);
```

`VARCHAR(n)`: O'zgaruvchan uzunlikdagi matn. Maksimal uzunlik `n` bilan belgilangan.

**Example:**

```sql
CREATE TABLE Example (
    name VARCHAR(50)
);
```

`TEXT`: Cheksiz uzunlikdagi matn. Bu turingizdagi matnning uzunligi chegaralanmagan.

**Example:**

```sql
CREATE TABLE Example (
    description TEXT
);
```

3. Date and Time Data Types(Sana va vaqt ma'lumot turlari)

`DATE`: Faqat sana (yil, oy, kun).

**Example:**

```sql
CREATE TABLE Example (
    birth_date DATE
);
```

`TIME`: Faqat vaqt (soat, daqiqa, soniya).

**Example:**

```sql
CREATE TABLE Example (
    work_start TIME
);
```

`TIMESTAMP`: Sana va vaqt birgalikda (yil, oy, kun, soat, daqiqa, soniya).

**Example:**

```sql
CREATE TABLE Example (
    created_at TIMESTAMP
);
```

`TIMESTAMPTZ`: Sana va vaqt (soat mintaqasi bilan).

**Example:**

```sql
CREATE TABLE Example (
    event_time TIMESTAMPTZ
);
```

`INTERVAL`: Vaqt oralig'ini saqlash (masalan, 1 kun, 3 soat).

**Example:**

```sql
CREATE TABLE Example (
    duration INTERVAL
);
```

4. Boolean Data Type(Mantiqiy ma'lumot turlari )

`BOOLEAN`: `TRUE`, `FALSE` yoki `NULL` qiymatlarini saqlash.

**Example:**

```sql
CREATE TABLE Example (
    is_active BOOLEAN
);
```

5. Array Data Types(Ma'lumotlar to'plamlari)

`ARRAY`: Bir xil turdagi bir nechta qiymatlarni saqlash uchun array (massiv) turidan foydalanish mumkin.

**Example:**

```sql
CREATE TABLE Example (
    scores INTEGER[]
);
```

6. `JSON` va `JSONB` ma'lumot turlari (JSON Data Types)

`JSON`: JSON formatidagi ma'lumotlarni saqlash.

**Example:**

```sql
CREATE TABLE Example (
    data JSON
);
```

`JSONB`: `JSON` formatidagi ma'lumotlarni binary formatda saqlash (tezroq ishlash uchun).

**Example:**

```sql
CREATE TABLE Example (
    data JSONB
);
```

7. Yagona identifikatorlar (`UUID`)

`UUID`: Unikal identifikatorlarni saqlash uchun ishlatiladi. Bu tur, global darajada noyob bo'lgan identifikatorlarni yaratadi.

**Example:**

```sql
CREATE TABLE Example (
    user_id UUID
);
```

8. Binar ma'lumot turlari (Binary Data Types)

`BYTEA`: Binary (2-lik) ma'lumotlarni saqlash uchun ishlatiladi (masalan, rasm yoki fayl).

**Example:**

```sql
CREATE TABLE Example (
    image BYTEA
);
```

9. Ma'lumotlar turlari bilan bog'liq boshqa turdagi turlar

`SERIAL`: Avtomatik ravishda raqamlar yaratish uchun ishlatiladi (asosan asosiy kalit sifatida).

**Example:**

```sql
CREATE TABLE Example (
    id SERIAL PRIMARY KEY
);
```

`BIGSERIAL`: Katta avtomatik raqamlar yaratish uchun ishlatiladi.

**Example:**

```sql
CREATE TABLE Example (
    id BIGSERIAL PRIMARY KEY
);
```

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

# PRIMARY KEYS AND CONSTRAINTS

> [!NOTE]
> Ma'lumotlar bazasida asosiy kalit (primary key) va cheklovlar (constraints) ma'lumotlarning to'g'ri va tartibli saqlanishini ta'minlash uchun ishlatiladi.

### Primary key

Primary key jadvaldagi har bir qatorni yagona tarzda identifikatsiya qiladi. U shuni ta'minlaydiki, jadvaldagi ikkita qator bir xil asosiy kalit qiymatiga ega bo'lmaydi va asosiy kalit hech qachon `NULL` bo'lmaydi.

- Characteristics of a Primary Key:
  - **Uniqueness:** Har bir qator uchun asosiy kalit qiymati takrorlanmas bo'lishi kerak.
  - **Non-null:** Primary key ustunida `NULL` qiymatlari bo'lishi mumkin emas.
  - **Single-column or Composite:** Asosiy kalit bitta ustundan yoki bir nechta ustunlarning birikmasidan iborat bo'lishi mumkin.

**Example:**

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(100),
    Age INT
);
```

Bu yerda `StudentID` asosiy kalit bo'lib, har bir talabaning noyob `ID`ga ega bo'lishini ta'minlaydi.

**Composite Primary Key Example:**

```sql
CREATE TABLE Enrollments (
    StudentID INT,
    CourseID INT,
    PRIMARY KEY (StudentID, CourseID)
);
```

Bu jadvalda `StudentID` va `CourseID` birgalikda birikma asosiy kalit sifatida ishlatiladi, ya'ni har bir talaba bitta kursga faqat bir marta yozilishi mumkin.

### Constraints

> [!NOTE]
> `Constraints` jadval ustunlariga qo'llaniladigan qoidalar bo'lib, ular ma'lumotlarning `to'g'ri` va `tartibli` bo'lishini ta'minlaydi.

Types of Constraints:
1. NOT NULL

Ustunda `NULL` qiymat bo'lishiga ruxsat bermaydi.

**Example:**

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL
);
```




