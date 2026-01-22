# 🐘 03-DARS: MA'LUMOTLARNI QIDIRISH VA FILTRLASH

## 📋 MAVZU REJASI

- [SELECT statement asoslari](#-select-statement-asoslari)
- [WHERE clause bilan filtrlash](#-where-clause-bilan-filtrlash)
- [Operatorlar va shartlar](#-operatorlar-va-shartlar)
- [ORDER BY bilan saralash](#-order-by-bilan-saralash)
- [LIMIT va OFFSET](#-limit-va-offset)
- [Pattern Matching - LIKE va ILIKE](#-pattern-matching---like-va-ilike)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz quyidagilarni o'rganasiz:

✅ SELECT operatori va uning barcha imkoniyatlari  
✅ WHERE sharti bilan ma'lumotlarni filtrlash  
✅ Har xil operatorlar (=, <, >, AND, OR, NOT)  
✅ ORDER BY bilan ma'lumotlarni saralash  
✅ LIMIT va OFFSET orqali natijalarni cheklash  
✅ LIKE, BETWEEN, IN kabi maxsus operatorlar  
✅ Real loyihalarda qo'llash

---

## 📊 SELECT STATEMENT ASOSLARI

### 📌 SELECT nima?

**SELECT** — bu PostgreSQL'ning **eng muhim** va **eng ko'p ishlatiladigan** buyrug'i. U database'dan ma'lumotlarni o'qish uchun ishlatiladi.

**Real hayotdan misol:**

Tasavvur qiling, kutubxonadagi kitoblar katalogidan qidirish:
- 📚 Barcha kitoblarni ko'rish
- 🔍 Muayyan muallifning kitoblarini topish
- 📖 2020-yildan keyin chiqgan kitoblarni ko'rish
- ⭐ Eng mashhur kitoblarni saralash

### 💻 Asosiy Sintaksis

```sql
-- 1. Barcha ustunlarni tanlash
SELECT * FROM table_name;

-- 2. Aniq ustunlarni tanlash
SELECT column1, column2, column3 FROM table_name;

-- 3. Noyob qiymatlarni tanlash
SELECT DISTINCT column_name FROM table_name;

-- 4. Ustunlarga nom berish (Alias)
SELECT column_name AS yangi_nom FROM table_name;
```

### 🧪 Amaliy Misollar

```sql
-- Test uchun jadval yaratish
CREATE TABLE talabalar (
    id SERIAL PRIMARY KEY,
    ism VARCHAR(50),
    familiya VARCHAR(50),
    yosh INTEGER,
    kurs INTEGER,
    fakultet VARCHAR(100),
    stipendiya DECIMAL(10, 2),
    shahar VARCHAR(50),
    kirish_yili INTEGER,
    email VARCHAR(100)
);

-- Ma'lumot qo'shish
INSERT INTO talabalar (ism, familiya, yosh, kurs, fakultet, stipendiya, shahar, kirish_yili, email) VALUES
    ('Ali', 'Valiyev', 20, 2, 'Dasturlash', 800000, 'Toshkent', 2023, 'ali@university.uz'),
    ('Madina', 'Karimova', 19, 1, 'Iqtisodiyot', 700000, 'Samarqand', 2024, 'madina@university.uz'),
    ('Bekzod', 'Tursunov', 22, 4, 'Dasturlash', 1000000, 'Toshkent', 2021, 'bekzod@university.uz'),
    ('Dilnoza', 'Rahimova', 21, 3, 'Tibbiyot', 900000, 'Buxoro', 2022, 'dilnoza@university.uz'),
    ('Sardor', 'Ahmedov', 20, 2, 'Iqtisodiyot', 750000, 'Toshkent', 2023, 'sardor@university.uz'),
    ('Zarina', 'Yusupova', 19, 1, 'Tibbiyot', 850000, 'Samarqand', 2024, 'zarina@university.uz'),
    ('Abbos', 'Rustamov', 23, 4, 'Dasturlash', 950000, 'Farg\'ona', 2021, 'abbos@university.uz'),
    ('Feruza', 'Qodirova', 21, 3, 'Iqtisodiyot', 800000, 'Toshkent', 2022, 'feruza@university.uz');
```

#### 1️⃣ Barcha ma'lumotlarni olish

```sql
-- Barcha talabalarni ko'rish
SELECT * FROM talabalar;
```

**Natija:**
```
 id |   ism   | familiya  | yosh | kurs |   fakultet   | stipendiya |  shahar   | kirish_yili |        email
----+---------+-----------+------+------+--------------+------------+-----------+-------------+---------------------
  1 | Ali     | Valiyev   |   20 |    2 | Dasturlash   |  800000.00 | Toshkent  |        2023 | ali@university.uz
  2 | Madina  | Karimova  |   19 |    1 | Iqtisodiyot  |  700000.00 | Samarqand |        2024 | madina@university.uz
  3 | Bekzod  | Tursunov  |   22 |    4 | Dasturlash   | 1000000.00 | Toshkent  |        2021 | bekzod@university.uz
  ...
```

#### 2️⃣ Aniq ustunlarni tanlash

```sql
-- Faqat ism, familiya va fakultetni ko'rish
SELECT ism, familiya, fakultet FROM talabalar;
```

**Natija:**
```
   ism   | familiya  |   fakultet
---------+-----------+--------------
 Ali     | Valiyev   | Dasturlash
 Madina  | Karimova  | Iqtisodiyot
 Bekzod  | Tursunov  | Dasturlash
 ...
```

#### 3️⃣ DISTINCT - Noyob qiymatlar

```sql
-- Qaysi fakultetlar bor?
SELECT DISTINCT fakultet FROM talabalar;
```

**Natija:**
```
   fakultet
--------------
 Dasturlash
 Iqtisodiyot
 Tibbiyot
```

```sql
-- Qaysi shaharlardan talabalar bor?
SELECT DISTINCT shahar FROM talabalar;
```

**Natija:**
```
  shahar
-----------
 Toshkent
 Samarqand
 Buxoro
 Farg'ona
```

#### 4️⃣ Column Aliases - Nom berish

```sql
-- Ustunlarga o'zbek nomlari berish
SELECT 
    ism AS "Ismi",
    familiya AS "Familiyasi",
    fakultet AS "Ta'lim yo'nalishi",
    stipendiya AS "Oylik stipendiya"
FROM talabalar;
```

**Natija:**
```
  Ismi  | Familiyasi | Ta'lim yo'nalishi | Oylik stipendiya
--------+------------+-------------------+------------------
 Ali    | Valiyev    | Dasturlash        |       800000.00
 Madina | Karimova   | Iqtisodiyot       |       700000.00
 ...
```

#### 5️⃣ Hisob-kitoblar

```sql
-- Stipendiyani dollarda ko'rish (1$ = 12,500 so'm)
SELECT 
    ism,
    familiya,
    stipendiya AS "So'mda",
    ROUND(stipendiya / 12500, 2) AS "Dollarda",
    fakultet
FROM talabalar;
```

**Natija:**
```
   ism   | familiya  |   So'mda   | Dollarda |   fakultet
---------+-----------+------------+----------+--------------
 Ali     | Valiyev   |  800000.00 |    64.00 | Dasturlash
 Madina  | Karimova  |  700000.00 |    56.00 | Iqtisodiyot
 Bekzod  | Tursunov  | 1000000.00 |    80.00 | Dasturlash
 ...
```

---

## 🔍 WHERE CLAUSE BILAN FILTRLASH

### 📌 WHERE nima?

**WHERE** — bu shartli filtrlash. Faqat **shartga mos kelgan** qatorlarni qaytaradi.

### 💻 Asosiy Sintaksis

```sql
SELECT columns
FROM table_name
WHERE condition;
```

### 🧪 Amaliy Misollar

#### 1️⃣ Oddiy shart

```sql
-- Faqat Dasturlash fakultetidagi talabalar
SELECT ism, familiya, fakultet 
FROM talabalar 
WHERE fakultet = 'Dasturlash';
```

**Natija:**
```
   ism   | familiya |  fakultet
---------+----------+------------
 Ali     | Valiyev  | Dasturlash
 Bekzod  | Tursunov | Dasturlash
 Abbos   | Rustamov | Dasturlash
```

#### 2️⃣ Raqamli shart

```sql
-- 20 yoshdan katta talabalar
SELECT ism, familiya, yosh 
FROM talabalar 
WHERE yosh > 20;
```

**Natija:**
```
   ism   | familiya  | yosh
---------+-----------+------
 Bekzod  | Tursunov  |   22
 Dilnoza | Rahimova  |   21
 Abbos   | Rustamov  |   23
 Feruza  | Qodirova  |   21
```

```sql
-- Stipendiyasi 800,000 va undan yuqori bo'lgan talabalar
SELECT ism, familiya, stipendiya 
FROM talabalar 
WHERE stipendiya >= 800000
ORDER BY stipendiya DESC;
```

**Natija:**
```
   ism   | familiya  | stipendiya
---------+-----------+------------
 Bekzod  | Tursunov  | 1000000.00
 Abbos   | Rustamov  |  950000.00
 Dilnoza | Rahimova  |  900000.00
 Zarina  | Yusupova  |  850000.00
 Ali     | Valiyev   |  800000.00
 Feruza  | Qodirova  |  800000.00
```

---

## ⚙️ OPERATORLAR VA SHARTLAR

### 🔢 Solishtirish Operatorlari

| Operator | Ma'nosi | Misol |
|----------|---------|-------|
| `=` | Teng | `yosh = 20` |
| `!=` yoki `<>` | Teng emas | `yosh <> 20` |
| `>` | Katta | `yosh > 20` |
| `<` | Kichik | `stipendiya < 800000` |
| `>=` | Katta yoki teng | `kurs >= 3` |
| `<=` | Kichik yoki teng | `yosh <= 21` |

### 🧪 Misollar

```sql
-- 1. Teng emas
SELECT ism, familiya, shahar 
FROM talabalar 
WHERE shahar != 'Toshkent';
```

```sql
-- 2. Kichik yoki teng
SELECT ism, familiya, kurs 
FROM talabalar 
WHERE kurs <= 2;
```

**Natija:**
```
   ism   | familiya | kurs
---------+----------+------
 Ali     | Valiyev  |    2
 Madina  | Karimova |    1
 Sardor  | Ahmedov  |    2
 Zarina  | Yusupova |    1
```

### 🔗 Mantiqiy Operatorlar

#### 1️⃣ AND - VA

**Barcha shartlar** bajarilishi kerak.

```sql
-- Toshkentlik VA Dasturlash fakultetidagi talabalar
SELECT ism, familiya, shahar, fakultet 
FROM talabalar 
WHERE shahar = 'Toshkent' AND fakultet = 'Dasturlash';
```

**Natija:**
```
   ism   | familiya |  shahar  |  fakultet
---------+----------+----------+------------
 Ali     | Valiyev  | Toshkent | Dasturlash
 Bekzod  | Tursunov | Toshkent | Dasturlash
```

```sql
-- 3-kursda VA stipendiyasi 800,000 dan yuqori
SELECT ism, familiya, kurs, stipendiya 
FROM talabalar 
WHERE kurs = 3 AND stipendiya > 800000;
```

**Natija:**
```
   ism   | familiya | kurs | stipendiya
---------+----------+------+------------
 Dilnoza | Rahimova |    3 |  900000.00
```

#### 2️⃣ OR - YOKI

**Kamida bitta shart** bajarilsa kifoya.

```sql
-- Dasturlash YOKI Tibbiyot fakultetidagi talabalar
SELECT ism, familiya, fakultet 
FROM talabalar 
WHERE fakultet = 'Dasturlash' OR fakultet = 'Tibbiyot';
```

**Natija:**
```
   ism   | familiya |  fakultet
---------+----------+------------
 Ali     | Valiyev  | Dasturlash
 Bekzod  | Tursunov | Dasturlash
 Dilnoza | Rahimova | Tibbiyot
 Zarina  | Yusupova | Tibbiyot
 Abbos   | Rustamov | Dasturlash
```

```sql
-- Toshkentlik YOKI stipendiyasi 900,000 dan yuqori
SELECT ism, familiya, shahar, stipendiya 
FROM talabalar 
WHERE shahar = 'Toshkent' OR stipendiya >= 900000;
```

#### 3️⃣ NOT - EMAS

**Shartni inkor qiladi**.

```sql
-- Toshkentlik EMAS talabalar
SELECT ism, familiya, shahar 
FROM talabalar 
WHERE NOT shahar = 'Toshkent';

-- Yoki
SELECT ism, familiya, shahar 
FROM talabalar 
WHERE shahar <> 'Toshkent';
```

**Natija:**
```
   ism   | familiya  |  shahar
---------+-----------+-----------
 Madina  | Karimova  | Samarqand
 Dilnoza | Rahimova  | Buxoro
 Zarina  | Yusupova  | Samarqand
 Abbos   | Rustamov  | Farg'ona
```

#### 4️⃣ Kombinatsiya

```sql
-- (Dasturlash fakulteti VA Toshkentlik) YOKI stipendiya 1,000,000
SELECT ism, familiya, shahar, fakultet, stipendiya 
FROM talabalar 
WHERE (fakultet = 'Dasturlash' AND shahar = 'Toshkent') 
   OR stipendiya = 1000000;
```

**Natija:**
```
   ism   | familiya |  shahar  |  fakultet  | stipendiya
---------+----------+----------+------------+------------
 Ali     | Valiyev  | Toshkent | Dasturlash |  800000.00
 Bekzod  | Tursunov | Toshkent | Dasturlash | 1000000.00
```

### 📦 Maxsus Operatorlar

#### 1️⃣ BETWEEN - Oraliq

```sql
-- Yoshi 19 dan 21 gacha (ikkalasi ham kiritilgan)
SELECT ism, familiya, yosh 
FROM talabalar 
WHERE yosh BETWEEN 19 AND 21;
```

**Natija:**
```
   ism   | familiya  | yosh
---------+-----------+------
 Ali     | Valiyev   |   20
 Madina  | Karimova  |   19
 Dilnoza | Rahimova  |   21
 Sardor  | Ahmedov   |   20
 Zarina  | Yusupova  |   19
 Feruza  | Qodirova  |   21
```

```sql
-- Stipendiyasi 750,000 dan 900,000 gacha
SELECT ism, familiya, stipendiya 
FROM talabalar 
WHERE stipendiya BETWEEN 750000 AND 900000
ORDER BY stipendiya;
```

#### 2️⃣ IN - Ro'yxatda bormi?

```sql
-- Toshkent, Samarqand yoki Buxorolik talabalar
SELECT ism, familiya, shahar 
FROM talabalar 
WHERE shahar IN ('Toshkent', 'Samarqand', 'Buxoro');
```

**Natija:**
```
   ism   | familiya  |  shahar
---------+-----------+-----------
 Ali     | Valiyev   | Toshkent
 Madina  | Karimova  | Samarqand
 Bekzod  | Tursunov  | Toshkent
 Dilnoza | Rahimova  | Buxoro
 Sardor  | Ahmedov   | Toshkent
 Zarina  | Yusupova  | Samarqand
 Feruza  | Qodirova  | Toshkent
```

```sql
-- NOT IN - Ro'yxatda yo'q
SELECT ism, familiya, shahar 
FROM talabalar 
WHERE shahar NOT IN ('Toshkent', 'Samarqand');
```

**Natija:**
```
   ism   | familiya  |  shahar
---------+-----------+-----------
 Dilnoza | Rahimova  | Buxoro
 Abbos   | Rustamov  | Farg'ona
```

#### 3️⃣ IS NULL / IS NOT NULL

```sql
-- Test uchun: ba'zi talabalar uchun email yo'q
UPDATE talabalar SET email = NULL WHERE id IN (3, 5);

-- Email manzili yo'q talabalar
SELECT ism, familiya, email 
FROM talabalar 
WHERE email IS NULL;
```

**Natija:**
```
   ism   | familiya | email
---------+----------+-------
 Bekzod  | Tursunov | NULL
 Sardor  | Ahmedov  | NULL
```

```sql
-- Email manzili bor talabalar
SELECT ism, familiya, email 
FROM talabalar 
WHERE email IS NOT NULL;
```

---

## 🔤 PATTERN MATCHING - LIKE VA ILIKE

### 📌 LIKE operatori

**LIKE** — matnli qidirish uchun (case-sensitive).

**Wildcard belgilar:**
- `%` — Istalgan miqdordagi belgini anglatadi (0 yoki ko'proq)
- `_` — Faqat **bitta** belgini anglatadi

### 🧪 Misollar

```sql
-- 1. Ismi 'A' harfi bilan boshlanadigan talabalar
SELECT ism, familiya 
FROM talabalar 
WHERE ism LIKE 'A%';
```

**Natija:**
```
  ism   | familiya
--------+----------
 Ali    | Valiyev
 Abbos  | Rustamov
```

```sql
-- 2. Ismi 'a' bilan tugaydigan talabalar
SELECT ism, familiya 
FROM talabalar 
WHERE ism LIKE '%a';
```

**Natija:**
```
   ism   | familiya
---------+-----------
 Madina  | Karimova
 Dilnoza | Rahimova
 Zarina  | Yusupova
 Feruza  | Qodirova
```

```sql
-- 3. Ismida 'ar' bo'lgan talabalar
SELECT ism, familiya 
FROM talabalar 
WHERE ism LIKE '%ar%';
```

**Natija:**
```
  ism   | familiya
--------+----------
 Sardor | Ahmedov
 Zarina | Yusupova
```

```sql
-- 4. Ismi 4 harfli talabalar (aniq 4 ta belgili)
SELECT ism, familiya 
FROM talabalar 
WHERE ism LIKE '____';  -- 4 ta underscore
```

**Natija:**
```
 ism | familiya
-----+----------
 (bo'sh, chunki barcha ismlar 4 harfdan ko'p)
```

### 📌 ILIKE operatori

**ILIKE** — LIKE kabi, lekin **case-insensitive** (katta-kichik harfni hisobga olmaydi).

```sql
-- 'a' bilan boshlanadigan (katta yoki kichik)
SELECT ism, familiya 
FROM talabalar 
WHERE ism ILIKE 'a%';
```

**Natija:**
```
  ism   | familiya
--------+----------
 Ali    | Valiyev
 Abbos  | Rustamov
```

```sql
-- Email manzili 'UNIVERSITY' so'zini o'z ichiga olgan
SELECT ism, email 
FROM talabalar 
WHERE email ILIKE '%university%';
```

### 📌 Regex Pattern (Ilg'or)

```sql
-- Ismi 'S' yoki 'F' bilan boshlanadigan
SELECT ism, familiya 
FROM talabalar 
WHERE ism ~ '^[SF]';
```

**Natija:**
```
  ism   | familiya
--------+-----------
 Sardor | Ahmedov
 Feruza | Qodirova
```

---

## 📊 ORDER BY BILAN SARALASH

### 📌 ORDER BY nima?

**ORDER BY** — natijalarni **saralash** (tartiblash) uchun ishlatiladi.

- **ASC** — Ascending (o'sish tartibida) - **default**
- **DESC** — Descending (kamayish tartibida)

### 💻 Sintaksis

```sql
SELECT columns
FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC];
```

### 🧪 Misollar

#### 1️⃣ Oddiy saralash

```sql
-- Ismga qarab alifbo tartibida
SELECT ism, familiya 
FROM talabalar 
ORDER BY ism ASC;
```

**Natija:**
```
   ism   | familiya
---------+-----------
 Abbos   | Rustamov
 Ali     | Valiyev
 Bekzod  | Tursunov
 Dilnoza | Rahimova
 Feruza  | Qodirova
 Madina  | Karimova
 Sardor  | Ahmedov
 Zarina  | Yusupova
```

```sql
-- Yoshga qarab kamayish tartibida (eng kattadan kichikka)
SELECT ism, familiya, yosh 
FROM talabalar 
ORDER BY yosh DESC;
```

**Natija:**
```
   ism   | familiya  | yosh
---------+-----------+------
 Abbos   | Rustamov  |   23
 Bekzod  | Tursunov  |   22
 Dilnoza | Rahimova  |   21
 Feruza  | Qodirova  |   21
 Ali     | Valiyev   |   20
 Sardor  | Ahmedov   |   20
 Madina  | Karimova  |   19
 Zarina  | Yusupova  |   19
```

#### 2️⃣ Bir nechta ustun bo'yicha

```sql
-- Avval fakultet (alifbo), keyin stipendiya (kamayish)
SELECT ism, familiya, fakultet, stipendiya 
FROM talabalar 
ORDER BY fakultet ASC, stipendiya DESC;
```

**Natija:**
```
   ism   | familiya  |   fakultet   | stipendiya
---------+-----------+--------------+------------
 Bekzod  | Tursunov  | Dasturlash   | 1000000.00
 Abbos   | Rustamov  | Dasturlash   |  950000.00
 Ali     | Valiyev   | Dasturlash   |  800000.00
 Zarina  | Yusupova  | Tibbiyot     |  850000.00
 Dilnoza | Rahimova  | Tibbiyot     |  900000.00
 Feruza  | Qodirova  | Iqtisodiyot  |  800000.00
 Sardor  | Ahmedov   | Iqtisodiyot  |  750000.00
 Madina  | Karimova  | Iqtisodiyot  |  700000.00
```

#### 3️⃣ Hisoblangan ustun bo'yicha

```sql
-- Stipendiyaga ko'ra eng boydan kambag'alga
SELECT 
    ism, 
    familiya, 
    stipendiya,
    stipendiya * 12 AS "Yillik stipendiya"
FROM talabalar 
ORDER BY "Yillik stipendiya" DESC;
```

#### 4️⃣ NULL qiymatlar bilan

```sql
-- NULL qiymatlarni boshida ko'rsatish
SELECT ism, familiya, email 
FROM talabalar 
ORDER BY email ASC NULLS FIRST;
```

**Natija:**
```
   ism   | familiya |        email
---------+----------+---------------------
 Bekzod  | Tursunov | NULL
 Sardor  | Ahmedov  | NULL
 Abbos   | Rustamov | abbos@university.uz
 Ali     | Valiyev  | ali@university.uz
 ...
```

```sql
-- NULL qiymatlarni oxirida ko'rsatish
SELECT ism, familiya, email 
FROM talabalar 
ORDER BY email ASC NULLS LAST;
```

---

## 📉 LIMIT VA OFFSET

### 📌 LIMIT nima?

**LIMIT** — natijalarni **cheklash** uchun (faqat birinchi N ta qator).

**Qachon ishlatiladi:**
- ✅ Pagination (sahifalash)
- ✅ Top 10, Top 5 ko'rsatish
- ✅ Preview (dastlabki ko'rinish)
- ✅ Performance optimization

### 💻 Sintaksis

```sql
SELECT columns
FROM table_name
ORDER BY column
LIMIT n;

-- OFFSET bilan
SELECT columns
FROM table_name
ORDER BY column
LIMIT n OFFSET m;
```

### 🧪 Misollar

#### 1️⃣ LIMIT - Birinchi N ta

```sql
-- Eng yuqori 3 ta stipendiya oluvchi
SELECT ism, familiya, stipendiya 
FROM talabalar 
ORDER BY stipendiya DESC 
LIMIT 3;
```

**Natija:**
```
   ism   | familiya  | stipendiya
---------+-----------+------------
 Bekzod  | Tursunov  | 1000000.00
 Abbos   | Rustamov  |  950000.00
 Dilnoza | Rahimova  |  900000.00
```

```sql
-- Eng yosh 5 ta talaba
SELECT ism, familiya, yosh 
FROM talabalar 
ORDER BY yosh ASC 
LIMIT 5;
```

#### 2️⃣ OFFSET - O'tkazib yuborish

```sql
-- 4-qatordan boshlab 3 ta talaba
SELECT ism, familiya, stipendiya 
FROM talabalar 
ORDER BY stipendiya DESC 
LIMIT 3 OFFSET 3;
```

**Natija:**
```
   ism   | familiya | stipendiya
---------+----------+------------
 Zarina  | Yusupova |  850000.00
 Ali     | Valiyev  |  800000.00
 Feruza  | Qodirova |  800000.00
```

**Tushuntirish:**
- `LIMIT 3` — 3 ta qator
- `OFFSET 3` — Birinchi 3 tasini o'tkazib yuborish

#### 3️⃣ Pagination (Sahifalash)

```sql
-- 1-sahifa (birinchi 3 ta)
SELECT ism, familiya FROM talabalar 
ORDER BY id 
LIMIT 3 OFFSET 0;

-- 2-sahifa (keyingi 3 ta)
SELECT ism, familiya FROM talabalar 
ORDER BY id 
LIMIT 3 OFFSET 3;

-- 3-sahifa (keyingi 3 ta)
SELECT ism, familiya FROM talabalar 
ORDER BY id 
LIMIT 3 OFFSET 6;
```

**Formula:**
```
OFFSET = (page_number - 1) * page_size
LIMIT = page_size

Misol:
- Sahifa №3, har sahifada 5 ta:
  OFFSET = (3 - 1) * 5 = 10
  LIMIT = 5
```

#### 4️⃣ Real proyekt misoli

```sql
-- Blog platformasi: Birinchi 10 ta maqola
SELECT 
    title, 
    author, 
    published_date,
    views
FROM articles 
WHERE status = 'published'
ORDER BY published_date DESC 
LIMIT 10;

-- E-commerce: Eng mashhur 20 ta mahsulot
SELECT 
    product_name,
    price,
    rating,
    sales_count
FROM products 
WHERE in_stock = true
ORDER BY sales_count DESC, rating DESC 
LIMIT 20;
```

---


## 🎓 AMALIY MASHG'ULOT

### 📊 XODIMLAR JADVALI

Quyidagi jadval bilan ishlang. Topshiriqlarni bajarish uchun ushbu ma'lumotlardan foydalaning:

```
┌────┬─────────┬───────────┬─────────────────────┬───────────┬──────────┬────────────────────┬───────────┐
│ id │ ism     │ familiya  │ lavozim             │ bo'lim    │ maosh    │ ish_boshlagan_sana │ shahar    │
├────┼─────────┼───────────┼─────────────────────┼───────────┼──────────┼────────────────────┼───────────┤
│ 1  │ Ali     │ Valiyev   │ Backend Developer   │ IT        │ 8000000  │ 2021-01-15         │ Toshkent  │
│ 2  │ Madina  │ Karimova  │ HR Manager          │ HR        │ 5500000  │ 2020-03-20         │ Toshkent  │
│ 3  │ Bekzod  │ Tursunov  │ Senior Developer    │ IT        │ 12000000 │ 2019-07-01         │ Samarqand │
│ 4  │ Dilnoza │ Rahimova  │ Marketing Manager   │ Marketing │ 7000000  │ 2022-05-12         │ Buxoro    │
│ 5  │ Sardor  │ Ahmedov   │ Junior Developer    │ IT        │ 4500000  │ 2023-09-30         │ Toshkent  │
│ 6  │ Zarina  │ Yusupova  │ Accountant          │ Finance   │ 6000000  │ 2021-11-10         │ Toshkent  │
│ 7  │ Abbos   │ Rustamov  │ Data Analyst        │ IT        │ 7500000  │ 2020-06-15         │ Farg'ona  │
│ 8  │ Feruza  │ Qodirova  │ Sales Manager       │ Sales     │ 6500000  │ 2022-02-01         │ Toshkent  │
└────┴─────────┴───────────┴─────────────────────┴───────────┴──────────┴────────────────────┴───────────┘
```

> **💡 Eslatma:** Yuqoridagi jadval sizning `xodimlar` database'ingizda mavjud. Topshiriqlarni bajarish uchun bu ma'lumotlardan foydalaning.

---

### ✏️ Topshiriqlar

#### 1️⃣ Asosiy Filtrlash

**Topshiriq:** IT bo'limida ishlaydigan barcha xodimlarni toping.

---

#### 2️⃣ Raqamli Filtrlash

**Topshiriq:** Maoshi 7,000,000 so'mdan yuqori bo'lgan xodimlarni toping va maosh bo'yicha kamayish tartibida saralang.

---

#### 3️⃣ Sana bilan Filtrlash

**Topshiriq:** 2021-yilning 1-yanvaridan keyin ishga qabul qilingan xodimlarni toping.

---

#### 4️⃣ AND Operatori

**Topshiriq:** Toshkentda yashaydigan va maoshi 6,000,000 dan yuqori bo'lgan xodimlarni toping.

---

#### 5️⃣ OR Operatori

**Topshiriq:** IT yoki Marketing bo'limida ishlaydigan xodimlarni toping.

---

#### 6️⃣ NOT Operatori

**Topshiriq:** Toshkentda YASHAMAYDIGAN xodimlarni toping.

---

#### 7️⃣ LIKE Operatori

**Topshiriq:** Lavozimi "Manager" so'zi bilan tugaydigan xodimlarni toping.

---

#### 8️⃣ BETWEEN Operatori

**Topshiriq:** Maoshi 5,000,000 dan 8,000,000 gacha bo'lgan xodimlarni toping.

---

#### 9️⃣ TOP 3

**Topshiriq:** Eng yuqori maoshli 3 nafar xodimni toping.

---

#### 🔟 Murakkab So'rov

**Topshiriq:** IT bo'limida ishlaydigan, maoshi 5,000,000 dan yuqori bo'lgan va Toshkentda yashaydigan xodimlarni lavozim bo'yicha alifbo tartibida ko'rsating.

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:

- [x] SELECT statement va uning barcha imkoniyatlari
- [x] WHERE clause bilan ma'lumotlarni filtrlash
- [x] Solishtirish operatorlari (=, <, >, <=, >=, <>)
- [x] Mantiqiy operatorlar (AND, OR, NOT)
- [x] Maxsus operatorlar (BETWEEN, IN, LIKE, IS NULL)
- [x] ORDER BY bilan saralash (ASC, DESC, NULLS FIRST/LAST)
- [x] LIMIT va OFFSET bilan sahifalash
- [x] Pattern matching (LIKE, ILIKE)

### 📚 Keyingi darsda:

**04-DARS: Funksiyalar va Agregat Operatsiyalar**
- COUNT, SUM, AVG, MIN, MAX
- GROUP BY va HAVING
- String funksiyalari
- Sana-vaqt funksiyalari
- CASE WHEN shartli ifodalar

---

## 📖 QO'SHIMCHA RESURSLAR

### 🔗 Foydali Havolalar

- [PostgreSQL WHERE Documentation](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-WHERE)
- [PostgreSQL Pattern Matching](https://www.postgresql.org/docs/current/functions-matching.html)
- [SQL Style Guide](https://www.sqlstyle.guide/)

### 💡 Maslahatlar

1. **Performance uchun:**
   - `WHERE` shartlarini birinchi o'ringa qo'ying
   - Index yaratish haqida o'ylang
   - `SELECT *` o'rniga aniq ustunlarni tanlang

2. **Best Practices:**
   - Kodni o'qilishi oson qilish uchun formatlang
   - Izoh (comment) yozing
   - Consistentlik (bir xillik) saqlang

3. **Debugging:**
   - Murakkab so'rovlarni qismlarga bo'ling
   - `LIMIT 10` bilan dastlab test qiling
   - `EXPLAIN ANALYZE` bilan performance tekshiring
