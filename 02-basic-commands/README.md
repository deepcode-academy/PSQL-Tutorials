# 🐘 PostgreSQL ASOSLARI

# 🧩 2-DARS BASIC COMMANDS


## ✅ DATABASES, TABLES, COLUMNS VA ROWS HAQIDA TUSHUNCHA

### ❇️ DATABASE

📌 **Database** — bu katta ma'lumotlar ombori. U yerda turli xil ma'lumotlar tartib bilan saqlanadi. Katta ma'lumotlarni bir joyda saqlab, ularni tezda topish, o‘zgartirish va boshqarish uchun kerak.


### ❇️ TABLE

📌 **Table** — ma'lumotlarni ustunlar va qatorlarda tartibli saqlash va kerakli ma'lumotni tez topish uchun ishlatiladigan asosiy obyekt.

### ❇️ COLUMN

📌 **Column** — jadvaldagi ma'lumotlarning turini belgilovchi element bo‘lib, unda faqat ma'lum turdagi ma'lumotlar (masalan, raqam, matn, sana va boshqalar) saqlanadi.

### ❇️ ROW

📌 **Row** — bu jadvaldagi bitta ma'lumot to‘plami bo‘lib, u biror shaxs, buyum yoki hodisa haqida to‘liq ma'lumotni ifodalaydi.



📌 Agar bizda `students` nomli jadval bo'lsa, uning tarkibi quyidagicha bo'lishi mumkin:

|ID | Name        | Surname       | Age    | Class |
|---|-------------|---------------|--------|-------|
|1  | Ali         | Valiyev       | 20     | A1    |
|2  | Madina      | Ismailova     | 21     | A2    |
|3  | Bekzod      | Karimov       | 22     | A3    |

- **Database** - bunda bizning `"University"` nomli ma'lumotlar bazamiz bor deb tasavvur qilaylik.
- **Table** - `students` jadvali.
- **Columns** - `ID`, `Name`, `Surname`, `Age`, `Class`.
- **Rows** - har bir talaba haqida ma'lumotni ifodalovchi yozuvlar.

## ✅ DATA TYPES

📌 PostgreSQLda ma'lumot turlari (**data types**) ustunlarda saqlanadigan ma'lumotlarning turini belgilaydi va ular bilan qanday ishlash mumkinligini aniqlaydi.


### ❇️ NUMBER

📌 **NUMBER** — bu SQL ma'lumotlar bazasida sonlarni saqlash uchun ishlatiladigan asosiy ma'lumot turi hisoblanadi. NUMBER turlari yordamida butun sonlar, o‘nlik sonlar va boshqa raqamli qiymatlar saqlanadi.

#### ✳️ INTEGER

📌 **INTEGER (int, int4)** — bu 4 baytli butun son turi bo‘lib, −2,147,483,648 dan +2,147,483,647 gacha bo‘lgan sonlarni saqlaydi.

🎯 Oddiy ma’lumot saqlash uchun **example** jadvali yaratish

```sql
-- Example nomli jadval yaratilyapti
-- Bu jadvalda faqat bitta ustun bor: num
-- num ustuni INTEGER (butun son) tipida bo‘ladi
CREATE TABLE Example (
    num INTEGER
);
```

🎯 Talabalar haqidagi ma’lumotlarni saqlash uchun **students** jadvali yaratish

```sql
-- students nomli jadval yaratilyapti
CREATE TABLE students (
    
    -- id ustuni yaratilmoqda
    -- Har bir talabaning yagona (unique) identifikatori sifatida ishlatiladi
    -- INTEGER tipida bo‘ladi va PRIMARY KEY (asosiy kalit)
    id INTEGER PRIMARY KEY,
    
    -- name ustuni yaratilmoqda
    -- Talabaning ismini saqlaydi
    -- Maksimal uzunligi 50 belgidan iborat bo‘lishi mumkin
    name VARCHAR(50),
    
    -- age ustuni yaratilmoqda
    -- Talabaning yoshini saqlaydi
    -- INTEGER (butun son) tipida bo‘ladi
    age INTEGER
);
```

#### ✳️ BIGINT 

📌 **BIGINT (int8)** — bu 8 baytli butun son turi bo‘lib, juda katta butun sonlarni saqlash uchun ishlatiladi.

🎯 Oddiy **BIGINT** turidagi ustun yaratish uchun Example jadvali.

```sql
-- Example nomli jadval yaratilyapti
-- Bu jadvalda faqat bitta ustun bor: big_num
-- big_num ustuni BIGINT (katta butun son) tipida bo‘ladi
CREATE TABLE example (
    big_num BIGINT
);
```
🎯 Bank tranzaktsiyalarini saqlash uchun jadval yaratish

```sql
-- transactions nomli jadval yaratilyapti
CREATE TABLE transactions (
    
    -- transaction_id ustuni yaratilmoqda
    -- Har bir tranzaktsiyaga noyob raqam beriladi
    -- Katta qiymatlarni saqlash uchun BIGINT ishlatiladi
    transaction_id BIGINT PRIMARY KEY,
    
    -- amount ustuni yaratilmoqda
    -- Tranzaksiya summasini saqlaydi
    -- BIGINT ishlatilmoqda, chunki ba'zi hollarda juda katta summalar bo‘lishi mumkin
    amount BIGINT,
    
    -- description ustuni yaratilmoqda
    -- Tranzaksiya haqida qisqa izoh
    description VARCHAR(100)
);
```


#### ✳️ SMALLINT

📌 **SMALLINT (int2)** — bu 2 baytli butun son turi bo‘lib, kichik diapazondagi butun sonlarni saqlash uchun ishlatiladi.

🎯 Oddiy **SMALLINT** turidagi ustun yaratish uchun **example** jadvali:

```sql
-- Example nomli jadval yaratilyapti
-- Bu jadvalda faqat bitta ustun bor: small_num
-- small_num ustuni SMALLINT (kichik butun son) tipida bo‘ladi
CREATE TABLE example (
    small_num SMALLINT
);
```

🎯 Xodimlarning lavozim darajasini saqlash uchun.

```sql
-- employees nomli jadval yaratilyapti
CREATE TABLE employees (
    
    -- id ustuni: har bir xodimning ID raqami
    id INTEGER PRIMARY KEY,
    
    -- name ustuni: xodimning ismi
    name VARCHAR(50),
    
    -- level ustuni: xodimning lavozim darajasi
    -- Odatda bu 1 dan 10 gacha bo‘lgan kichik son bo‘ladi
    level SMALLINT
);
```

#### ✳️ DECIMAL OR NUMERIC

📌 **DECIMAL yoki NUMERIC** — bu aniq o‘nlik kasr sonlarni saqlash uchun ishlatiladigan ma'lumot turi. Bu turda aniqlik (precision) va kasr sonlar soni (scale) aniq belgilanadi.

🎯 **example** jadvali bilan amaliy misol

```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (
    
    -- price ustuni yaratilmoqda
    -- Mahsulot yoki xizmat narxini saqlash uchun ishlatiladi
    -- DECIMAL(8, 2) tipida bo‘lib, maksimal qiymat 999999.99 bo‘lishi mumkin
    price DECIMAL(8, 2)
);
```

- `DECIMAL`(**precision**, **scale**)
  - **Precision:** Jami raqamlar soni.
  - **Scale:** Kasr qismidagi raqamlar soni.


#### ✳️ REAL

📌 **REAL** — bu 4 baytli haqiqiy son (floating point) turi bo‘lib, o‘nlik kasr sonlarni taxminiy aniqlikda saqlaydi.


```sql
-- Example nomli jadval yaratilyapti
-- value ustuni REAL tipida, bu ustun haqiqiy sonlarni saqlaydi
CREATE TABLE example (
    value REAL
);
```

🎯 Ob-havo ma’lumotlari jadvali:

```sql
-- weather_data nomli jadval yaratilyapti
CREATE TABLE weather_data (
    
    -- id: har bir yozuv uchun ID
    id INTEGER PRIMARY KEY,
    
    -- temperature: harorat, REAL tipida
    temperature REAL,
    
    -- humidity: namlik foizi, REAL tipida
    humidity REAL
);
```

#### ✳️ DOUBLE PRECISION

**DOUBLE PRECISION** — bu 8 baytli aniqroq suzuvchi nuqtali haqiqiy sonlarni saqlash uchun ishlatiladigan ma'lumot turi, katta va kichik onlik sonlarni aniqlik bilan saqlaydi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- value ustuni yaratilmoqda
    -- Bu ustun DOUBLE PRECISION tipida bo‘ladi
    -- 8 baytli haqiqiy sonlarni (floating point) saqlaydi
    -- Katta aniqlikka ega kasr sonlarni saqlash uchun ishlatiladi
    value DOUBLE PRECISION
);
```

### ❇️ TEXT TYPE

#### ✳️ CHAR(N)

**CHAR(n)** — bu aniq uzunlikdagi matn turi, matn uzunligi n dan qisqa bo‘lsa, qolgan qismlar avtomatik bo‘sh joy (space) bilan to‘ldiriladi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- code ustuni yaratilmoqda
    -- CHAR(5) - bu ustun sabit (belgilangan) uzunlikdagi matn saqlaydi
    -- Har doim 5 ta belgi joy ajratiladi
    -- Agar kiritilgan matn 5 ta belgidan kam bo‘lsa, qolgan joylar bo‘sh joy (space) bilan to‘ldiriladi
    code CHAR(5)
);
```

#### ✳️ VARCHAR(N)

**VARCHAR(n)** — bu o‘zgaruvchan uzunlikdagi matn turi, matn uzunligi qancha bo‘lsa, shuncha joy egallaydi, lekin maksimal uzunlik n dan oshmaydi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- name ustuni yaratilmoqda
    -- VARCHAR(50) - bu ustun o‘zgaruvchan uzunlikdagi matn saqlaydi
    -- Maksimal uzunlik 50 ta belgi bilan cheklanadi
    -- Kiritilgan matn qancha uzunlikda bo‘lsa, shuncha joy egallaydi
    -- CHAR bilan farqi: bo‘sh joy bilan to‘ldirilmaydi, faqat kerakli joy egallanadi
    name VARCHAR(50)
);
```

#### ✳️ TEXT

**TEXT** - bu cheksiz uzunlikdagi matn saqlash uchun ishlatiladigan ma'lumotlar turi. Matn uzunligi chegaralanmagan, ya'ni istalgan uzunlikdagi yozuvlar saqlanishi mumkin. **VARCHAR(N)** dan farqli ravishda maksimal uzunlik ko‘rsatilmaydi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- description ustuni yaratilmoqda
    -- TEXT - bu ustun cheksiz uzunlikdagi matn saqlaydi
    -- Matn uzunligi oldindan belgilab qo‘yilmaydi
    -- Juda katta (katta hajmli) matnlar uchun qulay
    description TEXT
);
```

### ❇️ DATE AND TIME TYPES

#### ✳️ DATE

**DATE** - bu faqat sana (yil, oy, kun) ni saqlaydigan ma'lumotlar turi. DATE turi vaqtni (soat, daqiqa, soniya) o‘z ichiga olmaydi — faqatgina sana qismi saqlanadi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- birth_date ustuni yaratilmoqda
    -- DATE - bu ustun faqat sanani (yil, oy, kun) saqlaydi
    -- Vaqt qismi (soat, daqiqa, soniya) bu yerda mavjud emas
    birth_date DATE
);
```

#### ✳️ TIME

**TIME** — bu faqat vaqt (soat, daqiqa, soniya) ni saqlaydigan ma'lumotlar turi.
Bu turda sana (yil, oy, kun) saqlanmaydi — faqat kun ichidagi vaqt ifodalanadi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- work_start ustuni yaratilmoqda
    -- TIME - bu ustun faqat vaqtni (soat, daqiqa, soniya) saqlaydi
    -- Sana (yil, oy, kun) bu yerda mavjud emas
    -- Masalan: '08:30:00', '17:45:30' kabi qiymatlar saqlanadi
    work_start TIME
);
```

#### ✳️ TIMESTAMP

**TIMESTAMP** — bu sana va vaqtni birgalikda saqlovchi ma'lumotlar turi. Yil, oy, kun va soat, daqiqa, soniya birga saqlanadi. Ko‘pincha yozilgan vaqtni, o‘zgartirilgan vaqtni avtomatik yozib borishda ishlatiladi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- created_at ustuni yaratilmoqda
    -- TIMESTAMP - bu ustun sana va vaqtni birgalikda saqlaydi
    -- Sana: yil, oy, kun
    -- Vaqt: soat, daqiqa, soniya
    -- Masalan: '2025-07-13 22:30:15' kabi qiymat saqlanadi
    created_at TIMESTAMP
);
```

#### ✳️ TIMESTAMPTZ

**TIMESTAMPTZ** (Timestamp with Time Zone) — bu sana va vaqtni soat mintaqasi bilan birga saqlaydigan ma'lumotlar turi. Yil, oy, kun, soat, daqiqa, soniya va vaqt zonasi (UTC+05, UTC-03 va h.k.) birgalikda saqlanadi. Bu tur ko‘pincha global loyihalarda, foydalanuvchining aniq vaqtini belgilash uchun ishlatiladi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- event_time ustuni yaratilmoqda
    -- TIMESTAMPTZ - bu ustun sana, vaqt va vaqt mintaqasini birga saqlaydi
    -- Masalan: '2025-07-13 18:45:00+05:00'
    -- Bu vaqt UTC+5 mintaqasiga tegishli ekanligini bildiradi
    event_time TIMESTAMPTZ
);
```

#### ✳️ INTERVAL

**INTERVAL** — bu vaqt oralig‘ini (davrni) saqlovchi ma'lumotlar turi. Masalan: 1 kun, 3 soat, 2 oy 15 kun, 1 yil 5 oy 10 kun 4 soat 30 daqiqa kabi qiymatlar saqlanishi mumkin. Bu tur vaqtlar orasidagi farqni ifodalash yoki vaqt ustiga qo‘shish/ayirishda ishlatiladi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- duration ustuni yaratilmoqda
    -- INTERVAL - bu ustun vaqt oralig‘ini saqlaydi
    -- Masalan: '1 day', '3 hours', '2 months 15 days' kabi qiymatlar
    duration INTERVAL
);
```

### ❇️ BOOLEAN TYPE

#### ✳️ BOOLEAN

**BOOLEAN** — bu mantiqiy (true/false) qiymatlarni saqlovchi ma'lumotlar turi. Faqatgina TRUE, FALSE yoki NULL qiymatlarni qabul qiladi. Ko‘pincha holatni bildiruvchi ustunlarda ishlatiladi (masalan: foydalanuvchi faolmi yoki yo‘qmi).


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- is_active ustuni yaratilmoqda
    -- BOOLEAN - bu ustun TRUE yoki FALSE qiymatlarini saqlaydi
    -- TRUE - agar holat faol bo‘lsa
    -- FALSE - agar holat faol bo‘lmasa
    -- NULL - agar holat noma’lum bo‘lsa
    is_active BOOLEAN
);
```

### ❇️ ARRAY TYPE

#### ✳️ ARRAY

**ARRAY** — bu bir xil turdagi bir nechta qiymatlarni bitta ustun ichida saqlash imkonini beruvchi massiv (array) ma'lumot turi. Massivdagi barcha elementlar bir xil ma'lumot turiga ega bo‘lishi kerak (masalan: INTEGER[], TEXT[]).


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- scores ustuni yaratilmoqda
    -- INTEGER[] - bu ustun butun sonlardan iborat massivni saqlaydi
    -- Masalan: {90, 85, 78} kabi qiymatlar
    -- Har bir qiymat massiv ichida, bir xil turda bo'lishi shart
    scores INTEGER[]
);
```

### ❇️ JSON AND JSONB TYPES

#### ✳️ JSON

**JSON** — bu JSON (JavaScript Object Notation) formatidagi tuzilgan ma'lumotlarni saqlovchi ma'lumot turi. Strukturaviy (nested) ma'lumotlarni, juftlik ko‘rinishidagi (kalit:qiymat) yozuvlarni saqlash uchun ishlatiladi. PostgreSQL JSON ustunlarida ma'lumotlarni o‘qish, izlash va o‘zgartirish uchun maxsus funksiyalarni qo‘llab-quvvatlaydi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- data ustuni yaratilmoqda
    -- JSON - bu ustun JSON formatidagi strukturalangan ma'lumotni saqlaydi
    -- Masalan: {'name': 'Umid', 'age': 25} kabi
    -- Kalit-qiymat juftliklari ko‘rinishida saqlanadi
    data JSON
);
```

#### ✳️ JSONB

**JSONB** — bu JSON formatidagi ma'lumotlarni binary (ikkilik) formatda saqlovchi ma'lumot turi. JSON turiga o‘xshaydi, lekin JSONB tezroq ishlaydi, indekslash imkoniyatiga ega va tartiblanmagan (unordered) ko‘rinishda saqlanadi. Katta hajmdagi va murakkab tuzilgan JSON ma'lumotlar bilan samarali ishlash uchun tavsiya etiladi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- data ustuni yaratilmoqda
    -- JSONB - bu ustun JSON formatidagi ma'lumotni binary ko‘rinishda saqlaydi
    -- JSON ga nisbatan ko‘proq imkoniyat va tezlik taqdim etadi
    -- Masalan: {'product': 'laptop', 'price': 1500} kabi ma'lumotlar
    data JSONB
);
```

### ❇️ UUID TYPE

#### ✳️ UUID

**UUID (Universally Unique Identifier)** — bu butun dunyo bo‘yicha yagona (noyob) identifikatorlarni saqlash uchun ishlatiladigan ma'lumot turi. Ko‘pincha foydalanuvchi ID, mahsulot ID yoki boshqa unikal elementlarni aniqlashda ishlatiladi. UUID qiymatlari 128-bitli bo‘ladi va tasodifiy yoki belgilangan algoritm asosida yaratiladi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- user_id ustuni yaratilmoqda
    -- UUID - bu ustun noyob identifikatorlarni saqlaydi
    -- Masalan: 'a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11'
    -- Har bir qiymat global darajada noyob bo‘ladi
    user_id UUID
);
```

### ❇️ BINARY DATA TYPES

#### ✳️ BYTEA

**BYTEA (Byte Array)** — bu binary (ikkilik) ma'lumotlarni saqlash uchun ishlatiladigan ma'lumot turi. Masalan, rasmlar, video, audio, fayllar, shifrlangan ma'lumotlar yoki boshqa raqamli kontent saqlashda ishlatiladi. Saqlangan ma'lumotlar ikkilamchi (2-lik) ko‘rinishda bazaga yoziladi.


```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- image ustuni yaratilmoqda
    -- BYTEA - bu ustun binary (ikkilik) ma'lumotlarni saqlaydi
    -- Masalan: rasm, fayl, video, audio va h.k.
    -- Ma'lumotlar 2-lik ko‘rinishda saqlanadi
    image BYTEA
);
```

### ❇️ SERIAL AND BIGSERIAL TYPES

#### ✳️ SERIAL

**SERIAL** — bu avtomatik ravishda o‘sib boruvchi butun son qiymatlarni yaratish uchun ishlatiladigan ma'lumot turi. Ko‘pincha asosiy kalit (primary key) sifatida ishlatiladi. Har bir yangi yozuv qo‘shilganda qiymat avtomatik tarzda 1 taga ortadi. SERIAL bu INTEGER ustuniga AUTO INCREMENT xususiyatini qo‘shadi.

```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- id ustuni yaratilmoqda
    -- SERIAL - bu ustun avtomatik ravishda raqam hosil qiladi
    -- Har bir yangi yozuv uchun qiymat avtomatik 1 taga ortadi
    -- PRIMARY KEY - bu ustun asosiy kalit (noyob va NULL bo‘lmaydi)
    id SERIAL PRIMARY KEY
);
```

#### ✳️ BIGSERIAL

**BIGSERIAL** — bu katta hajmdagi avtomatik o‘suvchi butun sonlarni yaratish uchun ishlatiladigan ma'lumot turi. SERIAL turiga o‘xshaydi, lekin katta raqamlar (64-bitgacha) bilan ishlash imkonini beradi. Ko‘pincha millionlab yozuvlar yoki katta hajmli bazalar uchun asosiy kalit (primary key) sifatida qo‘llaniladi.

```sql
-- Example nomli jadval yaratilyapti
CREATE TABLE example (

    -- id ustuni yaratilmoqda
    -- BIGSERIAL - bu ustun avtomatik tarzda katta raqamlar yaratadi
    -- Har bir yangi yozuv uchun qiymat avtomatik 1 taga ortadi
    -- PRIMARY KEY - bu ustun asosiy kalit hisoblanadi
    -- Katta hajmli ma'lumotlar bazalari uchun mos
    id BIGSERIAL PRIMARY KEY
);
```

## ✅ ASOSIY SQL BUYRUQLARI: CREATE DATABASE, CREATE TABLE, DROP, INSERT, SELECT

`\h` - SQL dagi buyruqlarni chiqarish

`\?`  - PSQL ni ishlatish uchun buyruqlar

`\q` - dasturdan chiqish

`\l` - database larni ko'rish

`\! cls` - terminalni tozalash

`\c database_name` - ma'lumotlar bazasiga ulanish

`\dt` - jadvalni ko'rish

`\d table_name` - jadvalga ulanish

## CREATE DATABASE
`CREATE DATABASE`: Yangi database yaratadi.

`\c database_name;` - databasega ulanish

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

`\dt` - yaratilgan jadvalni ko'rish

`\d table_name;` - jadvalga ulanish


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

2. UNIQUE

Ustundagi barcha qiymatlar takrorlanmas bo'lishini ta'minlaydi.

**Example:**

```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    SKU VARCHAR(50) UNIQUE
);
```

3. PRIMARY KEY

U `NOT NULL` va `UNIQUE` qoidalarini birlashtirib, har bir qatorni noyob identifikatsiyalaydi.

4. FOREIGN KEY

Jadvalni boshqa jadval bilan bog'laydi, boshqa jadvalning asosiy kalitiga ishora qiladi.

**Example:**

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

5. CHECK

Ustundagi qiymatlarning ma'lum bir shartga mos kelishini tekshiradi.

**Example:**

```sql
CREATE TABLE Accounts (
    AccountID INT PRIMARY KEY,
    Balance DECIMAL(10, 2) CHECK (Balance >= 0)
);
```

6. DEFAULT

Agar kiritishda qiymat berilmasa, ustun uchun `standart` qiymatni belgilaydi.

**Example:**

```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY,
    RegistrationDate DATE DEFAULT CURRENT_DATE
);
```

# PRACTICE

1. Create `library_system` database

- `library_system` nomli ma’lumotlar bazasini yarating.
- `library_system` bazasida `books` nomli jadval yarating va quyidagi ustunlarni qo‘shing:
  - `id` (integer, primary key)
  - `title` (varchar, maksimal 200 ta belgi)
  - `author` (varchar, maksimal 100 ta belgi)
  - `published_year` (integer)
- `books` jadvaliga quyidagi ma’lumotlarni kiriting:

| ID | title                    | author          | published_year |
|----|--------------------------|-----------------|----------------|
| 1  | "1984"                   | "George Orwell" | 1949           |
| 2  | "To Kill a Mockingbird"  | "Harper Lee"    | 1960           |

- `books` jadvalidan barcha ma’lumotlarni oling.
- 1950-yildan keyin nashr etilgan kitoblarning faqat `title` va `author` ustunlarini oling.
- `books` jadvalini o‘chirib tashlang.
- `library_system` ma’lumotlar bazasini o‘chirib tashlang (ehtiyotkorlik bilan foydalaning!).

2. Primary Key bilan jadval yaratish

- `students` nomli jadval yarating. U quyidagi ustunlardan iborat bo‘lsin:
  - `student_id` (integer, primary key)
  - `name` (varchar, 100 ta belgi)
  - `age` (integer)

3. `Composite Primary Key` yaratish

- `enrollments` nomli jadval yarating. U quyidagi ustunlardan iborat bo‘lsin:
  - `student_id` (integer)
  - `course_id` (integer)
  - `enrollment_date` (date)
- `student_id` va `course_id` ustunlarini birgalikda `composite primary key` sifatida belgilang

4. `NOT NULL Constraint` qo‘shish

- `teachers` nomli jadval yarating. U quyidagi ustunlardan iborat bo‘lsin:
  - `teacher_id` (integer, primary key)
  - `name` (varchar, 100 ta belgi, null bo‘lishi mumkin emas)
  - `subject` (varchar, 50 ta belgi)

5. `UNIQUE Constraint` qo‘shish

- `courses` nomli jadval yarating. U quyidagi ustunlardan iborat bo‘lsin:
  - `course_id` (integer, primary key)
  - `course_name` (varchar, 100 ta belgi, har bir nom noyob bo‘lishi kerak)

6. `FOREIGN KEY Constraint` qo‘shish

- `classes` nomli jadval yarating. U quyidagi ustunlardan iborat bo‘lsin:
  - `class_id` (integer, primary key)
  - `teacher_id` (integer, teachers jadvalidagi teacher_id ustuniga bog‘langan bo‘lishi kerak)

7. `CHECK Constraint` qo‘shish

- `grades` nomli jadval yarating. U quyidagi ustunlardan iborat bo‘lsin:
    - `grade_id` (integer, primary key)
    - `student_id` (integer)
    - `grade` (integer, qiymati 0 va 100 oralig‘ida bo‘lishi kerak)