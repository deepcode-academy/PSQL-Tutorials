# 🐘 04-DARS: FUNKSIYALAR VA AGREGAT OPERATSIYALAR

## 📋 MAVZU REJASI

- [Agregat funksiyalar asoslari](#-agregat-funksiyalar-asoslari)
- [COUNT - Sanash](#-count---sanash)
- [SUM - Yig'indi](#-sum---yigindi)
- [AVG - O'rtacha](#-avg---ortacha)
- [MIN va MAX - Eng kichik va katta](#-min-va-max---eng-kichik-va-katta)
- [GROUP BY - Guruhlash](#-group-by---guruhlash)
- [HAVING - Guruhlangan ma'lumotlarni filtrlash](#-having---guruhlangan-malumotlarni-filtrlash)
- [String funksiyalari](#-string-funksiyalari)
- [Sana va vaqt funksiyalari](#-sana-va-vaqt-funksiyalari)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz quyidagilarni o'rganasiz:

✅ Agregat funksiyalar (COUNT, SUM, AVG, MIN, MAX)  
✅ GROUP BY bilan ma'lumotlarni guruhlash  
✅ HAVING bilan filtrlash  
✅ String funksiyalari (CONCAT, UPPER, LOWER, LENGTH)  
✅ Sana/vaqt funksiyalari (NOW, AGE, DATE_PART)  
✅ Matematik funksiyalar  
✅ Real proyektlarda qo'llash

---

## 📊 AGREGAT FUNKSIYALAR ASOSLARI

### 📌 Agregat funksiya nima?

**Agregat funksiya** — bir nechta qatorlardan **bitta natija** qaytaradigan funksiya.

**Real hayotdan misol:**

Tasavvur qiling, do'konda oylik hisobot:
- 📊 Nechta mahsulot sotildi? → **COUNT**
- 💰 Umumiy daromad qancha? → **SUM**
- 📈 O'rtacha sotish narxi? → **AVG**
- 🏆 Eng qimmat mahsulot? → **MAX**
- 💸 Eng arzon mahsulot? → **MIN**

### 🎯 Asosiy Agregat Funksiyalar

| Funksiya | Ma'nosi | Ishlatish |
|----------|---------|-----------|
| `COUNT()` | Sanash | Qatorlar sonini hisoblash |
| `SUM()` | Yig'indi | Raqamlarni qo'shish |
| `AVG()` | O'rtacha | O'rtacha qiymat |
| `MIN()` | Minimum | Eng kichik qiymat |
| `MAX()` | Maksimum | Eng katta qiymat |

### 💻 Test Ma'lumotlar

Keling, real do'kon example bilan ishlaylik:

```sql
-- Test jadvali
CREATE TABLE sotuvlar (
    id SERIAL PRIMARY KEY,
    mahsulot VARCHAR(100),
    kategoriya VARCHAR(50),
    narx DECIMAL(10, 2),
    soni INTEGER,
    sana DATE
);

-- Ma'lumot qo'shish
INSERT INTO sotuvlar (mahsulot, kategoriya, narx, soni, sana) VALUES
    ('Laptop HP', 'Elektronika', 5000000, 2, '2026-01-15'),
    ('iPhone 15', 'Elektronika', 12000000, 1, '2026-01-16'),
    ('Samsung TV', 'Elektronika', 3500000, 3, '2026-01-17'),
    ('Nike Shoes', 'Kiyim', 750000, 5, '2026-01-15'),
    ('Adidas T-shirt', 'Kiyim', 150000, 10, '2026-01-16'),
    ('Levi Jeans', 'Kiyim', 400000, 7, '2026-01-18'),
    ('PostgreSQL Book', 'Kitob', 250000, 4, '2026-01-19'),
    ('Python Book', 'Kitob', 300000, 6, '2026-01-20');
```

**Yaratilgan jadval:**

```
┌────┬──────────────────┬─────────────┬───────────┬──────┬────────────┐
│ id │ mahsulot         │ kategoriya  │ narx      │ soni │ sana       │
├────┼──────────────────┼─────────────┼───────────┼──────┼────────────┤
│ 1  │ Laptop HP        │ Elektronika │ 5000000   │ 2    │ 2026-01-15 │
│ 2  │ iPhone 15        │ Elektronika │ 12000000  │ 1    │ 2026-01-16 │
│ 3  │ Samsung TV       │ Elektronika │ 3500000   │ 3    │ 2026-01-17 │
│ 4  │ Nike Shoes       │ Kiyim       │ 750000    │ 5    │ 2026-01-15 │
│ 5  │ Adidas T-shirt   │ Kiyim       │ 150000    │ 10   │ 2026-01-16 │
│ 6  │ Levi Jeans       │ Kiyim       │ 400000    │ 7    │ 2026-01-18 │
│ 7  │ PostgreSQL Book  │ Kitob       │ 250000    │ 4    │ 2026-01-19 │
│ 8  │ Python Book      │ Kitob       │ 300000    │ 6    │ 2026-01-20 │
└────┴──────────────────┴─────────────┴───────────┴──────┴────────────┘
```

---

## 🔢 COUNT - SANASH

### 📌 COUNT() nima?

**COUNT()** — qatorlar **sonini** hisoblaydi.

### 💻 Sintaksis

```sql
-- Barcha qatorlar
COUNT(*)

-- NULL bo'lmaganlar
COUNT(column_name)

-- Noyob qiymatlar
COUNT(DISTINCT column_name)
```

### 🧪 Misollar

#### 1️⃣ Barcha qatorlarni sanash

```sql
-- Jami nechta sotuv bo'lgan?
SELECT COUNT(*) AS "Jami sotuvlar" 
FROM sotuvlar;
```

**Natija:**
```
 Jami sotuvlar
---------------
            8
```

#### 2️⃣ Shartli sanash

```sql
-- Elektronika kategoriyasida nechta mahsulot?
SELECT COUNT(*) AS "Elektronika mahsulotlari" 
FROM sotuvlar 
WHERE kategoriya = 'Elektronika';
```

**Natija:**
```
 Elektronika mahsulotlari
--------------------------
                        3
```

#### 3️⃣ DISTINCT bilan

```sql
-- Necha xil kategoriya bor?
SELECT COUNT(DISTINCT kategoriya) AS "Kategoriyalar soni" 
FROM sotuvlar;
```

**Natija:**
```
 Kategoriyalar soni
--------------------
                  3
```

```sql
-- Har bir kategoriyada nechta mahsulot?
SELECT 
    kategoriya,
    COUNT(*) AS "Mahsulotlar soni"
FROM sotuvlar
GROUP BY kategoriya
ORDER BY COUNT(*) DESC;
```

**Natija:**
```
 kategoriya  | Mahsulotlar soni
-------------+------------------
 Elektronika |                3
 Kiyim       |                3
 Kitob       |                2
```

---

## ➕ SUM - YIG'INDI

### 📌 SUM() nima?

**SUM()** — raqamli qiymatlarning **yig'indisini** hisoblaydi.

### 💻 Misollar

#### 1️⃣ Umumiy daromad

```sql
-- Barcha sotuvlarning umumiy qiymati
SELECT SUM(narx * soni) AS "Umumiy daromad" 
FROM sotuvlar;
```

**Natija:**
```
 Umumiy daromad
----------------
      35400000
```

**Tushuntirish:**
```
Laptop HP:        5,000,000 × 2  = 10,000,000
iPhone 15:       12,000,000 × 1  = 12,000,000
Samsung TV:       3,500,000 × 3  = 10,500,000
Nike Shoes:         750,000 × 5  =  3,750,000
Adidas T-shirt:     150,000 × 10 =  1,500,000
Levi Jeans:         400,000 × 7  =  2,800,000
PostgreSQL Book:    250,000 × 4  =  1,000,000
Python Book:        300,000 × 6  =  1,800,000
                                  -----------
                                   43,350,000 ← Xato! Qarang!
```

**To'g'ri hisoblash:**

```sql
SELECT 
    mahsulot,
    narx,
    soni,
    narx * soni AS "Summa"
FROM sotuvlar;
```

#### 2️⃣ Kategoriya bo'yicha daromad

```sql
-- Har bir kategoriyaning daromadi
SELECT 
    kategoriya,
    SUM(narx * soni) AS "Kategoriya daromadi"
FROM sotuvlar
GROUP BY kategoriya
ORDER BY "Kategoriya daromadi" DESC;
```

**Natija:**
```
 kategoriya  | Kategoriya daromadi
-------------+---------------------
 Elektronika |            32500000
 Kiyim       |             8050000
 Kitob       |             2800000
```

#### 3️⃣ Jami sotilgan mahsulotlar

```sql
-- Nechta dona mahsulot sotilgan?
SELECT SUM(soni) AS "Jami sotilgan donalar" 
FROM sotuvlar;
```

**Natija:**
```
 Jami sotilgan donalar
-----------------------
                    38
```

---

## 📊 AVG - O'RTACHA

### 📌 AVG() nima?

**AVG()** — **o'rtacha qiymat**ni hisoblaydi.

### 💻 Misollar

#### 1️⃣ O'rtacha narx

```sql
-- Mahsulotlarning o'rtacha narxi
SELECT 
    ROUND(AVG(narx), 2) AS "O'rtacha narx"
FROM sotuvlar;
```

**Natija:**
```
 O'rtacha narx
---------------
    2793750.00
```

#### 2️⃣ Kategoriya bo'yicha o'rtacha

```sql
-- Har bir kategoriyaning o'rtacha narxi
SELECT 
    kategoriya,
    ROUND(AVG(narx), 2) AS "O'rtacha narx",
    ROUND(AVG(soni), 2) AS "O'rtacha soni"
FROM sotuvlar
GROUP BY kategoriya
ORDER BY "O'rtacha narx" DESC;
```

**Natija:**
```
 kategoriya  | O'rtacha narx | O'rtacha soni
-------------+---------------+---------------
 Elektronika |    6833333.33 |          2.00
 Kiyim       |     433333.33 |          7.33
 Kitob       |     275000.00 |          5.00
```

#### 3️⃣ MIN, MAX va AVG birga

```sql
-- Statistika
SELECT 
    kategoriya,
    COUNT(*) AS "Mahsulotlar",
    MIN(narx) AS "Min narx",
    ROUND(AVG(narx), 0) AS "O'rtacha",
    MAX(narx) AS "Max narx"
FROM sotuvlar
GROUP BY kategoriya;
```

**Natija:**
```
 kategoriya  | Mahsulotlar | Min narx  | O'rtacha | Max narx
-------------+-------------+-----------+----------+-----------
 Elektronika |           3 |  3500000  |  6833333 | 12000000
 Kiyim       |           3 |   150000  |   433333 |   750000
 Kitob       |           2 |   250000  |   275000 |   300000
```

---

## ⬆️⬇️ MIN VA MAX - ENG KICHIK VA KATTA

### 📌 MIN() va MAX() nima?

- **MIN()** — eng **kichik** qiymat
- **MAX()** — eng **katta** qiymat

### 💻 Misollar

#### 1️⃣ Eng arzon va qimmat

```sql
-- Eng arzon mahsulot
SELECT 
    mahsulot,
    narx
FROM sotuvlar
WHERE narx = (SELECT MIN(narx) FROM sotuvlar);
```

**Natija:**
```
    mahsulot     |  narx
-----------------+--------
 Adidas T-shirt  | 150000
```

```sql
-- Eng qimmat mahsulot
SELECT 
    mahsulot,
    narx
FROM sotuvlar
WHERE narx = (SELECT MAX(narx) FROM sotuvlar);
```

**Natija:**
```
  mahsulot  |   narx
------------+-----------
 iPhone 15  | 12000000
```

#### 2️⃣ Har kategoriyada eng qimmat

```sql
-- Har bir kategoriyada eng qimmat mahsulot
SELECT 
    kategoriya,
    MAX(narx) AS "Eng qimmat mahsulot narxi"
FROM sotuvlar
GROUP BY kategoriya;
```

**Natija:**
```
 kategoriya  | Eng qimmat mahsulot narxi
-------------+---------------------------
 Elektronika |                 12000000
 Kiyim       |                   750000
 Kitob       |                   300000
```

---

## 📦 GROUP BY - GURUHLASH

### 📌 GROUP BY nima?

**GROUP BY** — ma'lumotlarni **guruhlar**ga ajratadi va har bir guruh uchun hisob-kitob qiladi.

### 💻 Sintaksis

```sql
SELECT 
    column1,
    AGGREGATE_FUNCTION(column2)
FROM table_name
GROUP BY column1;
```

### 🧪 Misollar

#### 1️⃣ Oddiy guruhlash

```sql
-- Har bir kategoriyada nechta mahsulot?
SELECT 
    kategoriya,
    COUNT(*) AS "Soni"
FROM sotuvlar
GROUP BY kategoriya;
```

**Natija:**
```
 kategoriya  | Soni
-------------+------
 Elektronika |    3
 Kiyim       |    3
 Kitob       |    2
```

#### 2️⃣ Bir nechta agregat funksiya

```sql
-- To'liq statistika
SELECT 
    kategoriya,
    COUNT(*) AS "Mahsulotlar soni",
    SUM(soni) AS "Jami sotilgan",
    SUM(narx * soni) AS "Daromad",
    ROUND(AVG(narx), 0) AS "O'rtacha narx"
FROM sotuvlar
GROUP BY kategoriya
ORDER BY "Daromad" DESC;
```

**Natija:**
```
 kategoriya  | Mahsulotlar soni | Jami sotilgan |  Daromad  | O'rtacha narx
-------------+------------------+---------------+-----------+---------------
 Elektronika |                3 |             6 | 32500000  |       6833333
 Kiyim       |                3 |            22 |  8050000  |        433333
 Kitob       |                2 |            10 |  2800000  |        275000
```

#### 3️⃣ Bir nechta ustun bo'yicha

```sql
-- Kategoriya va sana bo'yicha
SELECT 
    kategoriya,
    sana,
    COUNT(*) AS "Sotuvlar soni",
    SUM(narx * soni) AS "Kun daromadi"
FROM sotuvlar
GROUP BY kategoriya, sana
ORDER BY sana, kategoriya;
```

**Natija:**
```
 kategoriya  |    sana    | Sotuvlar soni | Kun daromadi
-------------+------------+---------------+--------------
 Elektronika | 2026-01-15 |             1 |     10000000
 Kiyim       | 2026-01-15 |             1 |      3750000
 Elektronika | 2026-01-16 |             1 |     12000000
 Kiyim       | 2026-01-16 |             1 |      1500000
 Elektronika | 2026-01-17 |             1 |     10500000
 Kiyim       | 2026-01-18 |             1 |      2800000
 Kitob       | 2026-01-19 |             1 |      1000000
 Kitob       | 2026-01-20 |             1 |      1800000
```

---

## 🔍 HAVING - GURUHLANGAN MA'LUMOTLARNI FILTRLASH

### 📌 HAVING nima?

**HAVING** — `GROUP BY` dan **keyin** filtrlash uchun ishlatiladi.

**WHERE vs HAVING:**

| WHERE | HAVING |
|-------|--------|
| GROUP BY **dan oldin** | GROUP BY **dan keyin** |
| Oddiy qatorlarni filtrlaydi | Guruhlangan natijalarni filtrlaydi |
| Agregat funksiyalarsiz | Agregat funksiyalar bilan |

### 💻 Misollar

#### 1️⃣ Daromad bo'yicha filtrlash

```sql
-- Daromadi 5,000,000 dan yuqori kategoriyalar
SELECT 
    kategoriya,
    SUM(narx * soni) AS "Daromad"
FROM sotuvlar
GROUP BY kategoriya
HAVING SUM(narx * soni) > 5000000
ORDER BY "Daromad" DESC;
```

**Natija:**
```
 kategoriya  |  Daromad
-------------+-----------
 Elektronika | 32500000
 Kiyim       |  8050000
```

#### 2️⃣ 2+ mahsulot bo'lgan kategoriyalar

```sql
-- Kamida 3 ta mahsulot bo'lgan kategoriyalar
SELECT 
    kategoriya,
    COUNT(*) AS "Mahsulotlar soni",
    SUM(narx * soni) AS "Daromad"
FROM sotuvlar
GROUP BY kategoriya
HAVING COUNT(*) >= 3;
```

**Natija:**
```
 kategoriya  | Mahsulotlar soni |  Daromad
-------------+------------------+-----------
 Elektronika |                3 | 32500000
 Kiyim       |                3 |  8050000
```

#### 3️⃣ WHERE va HAVING birga

```sql
-- 2026-01-16 dan keyin, daromadi 2,000,000 dan yuqori
SELECT 
    kategoriya,
    COUNT(*) AS "Mahsulotlar",
    SUM(narx * soni) AS "Daromad"
FROM sotuvlar
WHERE sana > '2026-01-16'
GROUP BY kategoriya
HAVING SUM(narx * soni) > 2000000
ORDER BY "Daromad" DESC;
```

**Natija:**
```
 kategoriya  | Mahsulotlar |  Daromad
-------------+-------------+-----------
 Elektronika |           1 | 10500000
 Kiyim       |           1 |  2800000
 Kitob       |           2 |  2800000
```

---

## 🔤 STRING FUNKSIYALARI

### 📌 Asosiy String Funksiyalari

| Funksiya | Ma'nosi | Misol |
|----------|---------|-------|
| `CONCAT()` | Birlashtirish | `'Ali' + ' ' + 'Valiyev'` |
| `UPPER()` | Katta harfga | `'hello' → 'HELLO'` |
| `LOWER()` | Kichik harfga | `'HELLO' → 'hello'` |
| `LENGTH()` | Uzunlik | `'Hello' → 5` |
| `SUBSTRING()` | Qism olish | `'Hello' → 'ell'` |
| `TRIM()` | Bo'sh joyni olib tashlash | `' Hello ' → 'Hello'` |
| `REPLACE()` | Almashtirish | `'Hello' → 'Hallo'` |

### 💻 Misollar

#### 1️⃣ CONCAT - Birlashtirish

```sql
-- Mahsulot va kategoriyani birlashtirish
SELECT 
    CONCAT(mahsulot, ' (', kategoriya, ')') AS "To'liq nom",
    narx
FROM sotuvlar
LIMIT 5;
```

**Natija:**
```
        To'liq nom         |   narx
---------------------------+-----------
 Laptop HP (Elektronika)   |  5000000
 iPhone 15 (Elektronika)   | 12000000
 Samsung TV (Elektronika)  |  3500000
 Nike Shoes (Kiyim)        |   750000
 Adidas T-shirt (Kiyim)    |   150000
```

#### 2️⃣ UPPER va LOWER

```sql
-- Katta va kichik harflar
SELECT 
    mahsulot,
    UPPER(mahsulot) AS "KATTA HARF",
    LOWER(mahsulot) AS "kichik harf"
FROM sotuvlar
WHERE kategoriya = 'Kitob';
```

**Natija:**
```
     mahsulot      |    KATTA HARF     |   kichik harf
-------------------+-------------------+-------------------
 PostgreSQL Book   | POSTGRESQL BOOK   | postgresql book
 Python Book       | PYTHON BOOK       | python book
```

#### 3️⃣ LENGTH - Uzunlik

```sql
-- Mahsulot nomining uzunligi
SELECT 
    mahsulot,
    LENGTH(mahsulot) AS "Nom uzunligi"
FROM sotuvlar
ORDER BY LENGTH(mahsulot) DESC;
```

#### 4️⃣ SUBSTRING - Qism olish

```sql
-- Birinchi 10 belgini olish
SELECT 
    mahsulot,
    SUBSTRING(mahsulot FROM 1 FOR 10) AS "Qisqa nom"
FROM sotuvlar;
```

**Natija:**
```
     mahsulot      | Qisqa nom
-------------------+------------
 Laptop HP         | Laptop HP
 iPhone 15         | iPhone 15
 Samsung TV        | Samsung TV
 Nike Shoes        | Nike Shoes
 Adidas T-shirt    | Adidas T-s
```

#### 5️⃣ REPLACE - Almashtirish

```sql
-- 'Book' so'zini 'Kitob'ga almashtirish
SELECT 
    mahsulot,
    REPLACE(mahsulot, 'Book', 'Kitob') AS "O'zbekcha"
FROM sotuvlar
WHERE mahsulot LIKE '%Book%';
```

**Natija:**
```
     mahsulot      |     O'zbekcha
-------------------+-------------------
 PostgreSQL Book   | PostgreSQL Kitob
 Python Book       | Python Kitob
```

---

## 📅 SANA VA VAQT FUNKSIYALARI

### 📌 Asosiy Funksiyalar

| Funksiya | Ma'nosi | Misol |
|----------|---------|-------|
| `NOW()` | Hozirgi vaqt | `2026-01-22 23:45:00` |
| `CURRENT_DATE` | Bugungi sana | `2026-01-22` |
| `CURRENT_TIME` | Hozirgi vaqt | `23:45:00` |
| `AGE()` | Farq hisoblash | `25 years 3 mons` |
| `DATE_PART()` | Qismini olish | `2026` |
| `EXTRACT()` | Qiymat olish | `1` |

### 💻 Misollar

#### 1️⃣ Hozirgi vaqt

```sql
-- Bugun va hozir
SELECT 
    CURRENT_DATE AS "Bugun",
    CURRENT_TIME AS "Hozir",
    NOW() AS "Bugun va hozir";
```

**Natija:**
```
   Bugun    |    Hozir     |    Bugun va hozir
------------+--------------+----------------------
 2026-01-22 | 23:45:30     | 2026-01-22 23:45:30
```

#### 2️⃣ Necha kun oldin sotilgan?

```sql
-- Mahsulot qachon sotilgan va necha kun bo'ldi?
SELECT 
    mahsulot,
    sana AS "Sotilgan sana",
    CURRENT_DATE - sana AS "Necha kun oldin"
FROM sotuvlar
ORDER BY sana DESC;
```

**Natija:**
```
     mahsulot      | Sotilgan sana | Necha kun oldin
-------------------+---------------+-----------------
 Python Book       | 2026-01-20    |               2
 PostgreSQL Book   | 2026-01-19    |               3
 Levi Jeans        | 2026-01-18    |               4
 Samsung TV        | 2026-01-17    |               5
```

#### 3️⃣ AGE - Aniq farq

```sql
-- Aniq vaqt farqi
SELECT 
    mahsulot,
    sana,
    AGE(CURRENT_DATE, sana) AS "Vaqt farqi"
FROM sotuvlar
WHERE kategoriya = 'Elektronika';
```

**Natija:**
```
   mahsulot   |    sana    |  Vaqt farqi
--------------+------------+--------------
 Laptop HP    | 2026-01-15 | 7 days
 iPhone 15    | 2026-01-16 | 6 days
 Samsung TV   | 2026-01-17 | 5 days
```

#### 4️⃣ DATE_PART - Qismini olish

```sql
-- Yil, oy, kunni ajratish
SELECT 
    mahsulot,
    sana,
    DATE_PART('year', sana) AS "Yil",
    DATE_PART('month', sana) AS "Oy",
    DATE_PART('day', sana) AS "Kun"
FROM sotuvlar
LIMIT 5;
```

**Natija:**
```
     mahsulot      |    sana    | Yil  | Oy | Kun
-------------------+------------+------+----+-----
 Laptop HP         | 2026-01-15 | 2026 |  1 |  15
 iPhone 15         | 2026-01-16 | 2026 |  1 |  16
 Samsung TV        | 2026-01-17 | 2026 |  1 |  17
```

#### 5️⃣ Hafta bo'yicha guruhlash

```sql
-- Hafta injidan sanash (Yakshanba = 0)
SELECT 
    DATE_PART('dow', sana) AS "Hafta kuni",
    CASE DATE_PART('dow', sana)
        WHEN 0 THEN 'Yakshanba'
        WHEN 1 THEN 'Dushanba'
        WHEN 2 THEN 'Seshanba'
        WHEN 3 THEN 'Chorshanba'
        WHEN 4 THEN 'Payshanba'
        WHEN 5 THEN 'Juma'
        WHEN 6 THEN 'Shanba'
    END AS "Kun nomi",
    COUNT(*) AS "Sotuvlar soni"
FROM sotuvlar
GROUP BY DATE_PART('dow', sana)
ORDER BY DATE_PART('dow', sana);
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
│ 2  │ Madina  │ Karimova  │ Frontend Developer  │ IT        │ 7500000  │ 2021-03-20         │ Toshkent  │
│ 3  │ Bekzod  │ Tursunov  │ Senior Developer    │ IT        │ 12000000 │ 2019-07-01         │ Samarqand │
│ 4  │ Dilnoza │ Rahimova  │ Marketing Manager   │ Marketing │ 7000000  │ 2022-05-12         │ Buxoro    │
│ 5  │ Sardor  │ Ahmedov   │ Junior Developer    │ IT        │ 4500000  │ 2023-09-30         │ Toshkent  │
│ 6  │ Zarina  │ Yusupova  │ Accountant          │ Finance   │ 6000000  │ 2021-11-10         │ Toshkent  │
│ 7  │ Abbos   │ Rustamov  │ Data Analyst        │ IT        │ 7500000  │ 2020-06-15         │ Farg'ona  │
│ 8  │ Feruza  │ Qodirova  │ Sales Manager       │ Sales     │ 6500000  │ 2022-02-01         │ Toshkent  │
│ 9  │ Jasur   │ Mirzayev  │ HR Manager          │ HR        │ 5500000  │ 2020-03-15         │ Toshkent  │
│ 10 │ Kamola  │ Ergasheva │ PR Manager          │ Marketing │ 6000000  │ 2021-08-20         │ Samarqand │
└────┴─────────┴───────────┴─────────────────────┴───────────┴──────────┴────────────────────┴───────────┘
```

> **💡 Eslatma:** Yuqoridagi jadval sizning `xodimlar` database'ingizda mavjud. Topshiriqlarni bajarish uchun bu ma'lumotlardan foydalaning.

### ✏️ Topshiriqlar

#### Topshiriq 1: COUNT - Asosiy

1. Jami nechta xodim bor?
2. IT bo'limida nechta xodim bor?
3. Toshkentda yashaydigan xodimlar soni?

---

#### Topshiriq 2: SUM va AVG

1. Barcha xodimlarning umumiy maoshi qancha?
2. O'rtacha maosh qancha?
3. IT bo'limining umumiy va o'rtacha maoshi?

---

#### Topshiriq 3: MIN va MAX

1. Eng kam va eng yuqori maosh qancha?
2. Qaysi xodim eng ko'p maosh oladi?
3. Har bir bo'limdagi eng yuqori maosh?

---

#### Topshiriq 4: GROUP BY

1. Har bir bo'limda nechta xodim bor?
2. Har bir shaharda nechta xodim bor?
3. Bo'lim bo'yicha to'liq statistika (soni, umumiy maosh, o'rtacha)?

---

#### Topshiriq 5: HAVING

1. Umumiy maoshi 10,000,000 dan yuqori bo'lgan bo'limlar?
2. 2+ xodim bo'lgan bo'limlar?
3. O'rtacha maoshi 7,000,000 dan yuqori bo'limlar?

---

#### Topshiriq 6: String Funksiyalari

1. Xodimlarning to'liq ismi (ISM + FAMILIYA)?
2. Email yarating (ism.familiya@company.uz)?
3. Lavozimi "Manager" so'zi bilan tugaydiganlar?

---

#### Topshiriq 7: Sana Funksiyalari

1. Har bir xodim necha yil ishlagan?
2. 2021-yilda ishga kirganlar?
3. 3+ yil ishlagan xodimlar?

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:

- [x] Agregat funksiyalar (COUNT, SUM, AVG, MIN, MAX)
- [x] GROUP BY bilan ma'lumotlarni guruhlash
- [x] HAVING bilan guruhlangan ma'lumotlarni filtrlash
- [x] WHERE va HAVING farqi
- [x] String funksiyalari (CONCAT, UPPER, LOWER, LENGTH)
- [x] Sana/vaqt funksiyalari (NOW, AGE, DATE_PART)
- [x] Real proyektlarda qo'llash

### 📚 Keyingi darsda:

**05-DARS: Ma'lumotlarni Boshqarish va O'zgartirish**
- UPDATE bilan ma'lumotlarni yangilash
- DELETE bilan o'chirish
- Transaction'lar (BEGIN, COMMIT, ROLLBACK)
- Xavfsiz ma'lumot boshqarish
- Best practices

---

## 📖 QO'SHIMCHA MANBALAR

### 💡 Maslahatlar

1. **Agregat funksiyalar:**
   - NULL qiymatlarni hisobga olmaydi (COUNT stidan tashqari)
   - ROUND() bilan o'nlik sonni cheklash yaxshi
   - DISTINCT bilan noyob qiymatlarni sanash

2. **GROUP BY:**
   - SELECT'dagi barcha ustunlar GROUP BY'da bo'lishi kerak
   - Yoki agregat funksiya ichida bo'lishi kerak

3. **HAVING vs WHERE:**
   - WHERE: GROUP BY dan **oldin**
   - HAVING: GROUP BY dan **keyin**

4. **Performance:**
   - WHERE'ni iloji boricha ishlatish (HAVING'dan tez)
   - Index yaratish tegishli ustunlarga
   - EXPLAIN ANALYZE bilan tekshirish

### 🔗 Foydali Resurslar

- [PostgreSQL Aggregate Functions](https://www.postgresql.org/docs/current/functions-aggregate.html)
- [PostgreSQL String Functions](https://www.postgresql.org/docs/current/functions-string.html)
- [PostgreSQL Date/Time Functions](https://www.postgresql.org/docs/current/functions-datetime.html)