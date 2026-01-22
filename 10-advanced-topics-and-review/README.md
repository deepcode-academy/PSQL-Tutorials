# 🐘 10-DARS: MURAKKAB TOPIKLAR VA YAKUNIY REVIEW

## 📋 MAVZU REJASI

- [Window Functions - Murakkab hisobot tizimi](#-window-functions---murakkab-hisobot-tizimi)
- [CTE (Common Table Expressions) - WITH clause](#-cte-common-table-expressions)
- [Views va Materialized Views](#-views-va-materialized-views)
- [JSON va Array bilan ishlash](#-json-va-array-bilan-ishlash)
- [Time Series Data (Vaqt bo'yicha ma'lumotlar)](#-time-series-data)
- [O'tilganlarni yakuniy takrorlash](#-otilganlarni-yakuniy-takrorlash)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Bu oxirgi dars! Siz bu yerda PostgreSQLning eng kuchli va murakkab xususiyatlarini o'rganasiz. Ushbu bilimlar sizni junior dasturchidan middle darajasiga olib chiqadi. Real dunyo loyihalarida ko'p uchraydigan muammolarni hal qilish uchun kerakli qurollar qo'lga kiritasiz.

---

## 📊 TUSHUNTIRISH UCHUN JADVAL

Dars davomida quyidagi **Xodimlar** jadvalidan foydalanamiz:

```
┌────┬──────────┬──────────────┬─────────────┬──────────┬────────────┐
│ id │ ism      │ lavozim      │ bo'lim      │ maosh    │ ish_sana   │
├────┼──────────┼──────────────┼─────────────┼──────────┼────────────┤
│ 1  │ Ali      │ Developer    │ IT          │ 8,000    │ 2021-01-15 │
│ 2  │ Madina   │ Manager      │ HR          │ 12,000   │ 2020-03-20 │
│ 3  │ Bekzod   │ Senior Dev   │ IT          │ 15,000   │ 2019-07-01 │
│ 4  │ Dilnoza  │ Analyst      │ Analytics   │ 9,000    │ 2022-05-12 │
│ 5  │ Sardor   │ Junior Dev   │ IT          │ 5,000    │ 2023-09-30 │
│ 6  │ Zarina   │ Designer     │ Marketing   │ 7,500    │ 2021-11-10 │
│ 7  │ Komil    │ Accountant   │ Finance     │ 8,500    │ 2020-06-15 │
│ 8  │ Shahlo   │ Sales Rep    │ Sales       │ 6,000    │ 2022-02-01 │
└────┴──────────┴──────────────┴─────────────┴──────────┴────────────┘
```

---

## 🪟 WINDOW FUNCTIONS - MURAKKAB HISOBOT TIZIMI

**Window Function** — bu har bir qatorni o'chirmasdan, qo'shimcha hisob-kitoblar qilish imkonini beradi.

### 📌 ROW_NUMBER() - Raqamlash

Har bir qatorga tartib raqami berish:

```sql
SELECT 
    ism, 
    bo'lim, 
    maosh,
    ROW_NUMBER() OVER (ORDER BY maosh DESC) AS reyting
FROM xodimlar;
```

**Natija:**
```
   ism    │  bo'lim   │ maosh  │ reyting
──────────┼───────────┼────────┼─────────
 Bekzod   │ IT        │ 15,000 │ 1
 Madina   │ HR        │ 12,000 │ 2
 Dilnoza  │ Analytics │ 9,000  │ 3
```

### 📌 RANK() va DENSE_RANK()

Bir xil qiymatlar uchun bir xil reyting berish:

```sql
SELECT 
    ism, 
    maosh,
    RANK() OVER (ORDER BY maosh DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY maosh DESC) AS dense_rank
FROM xodimlar;
```

### 📌 PARTITION BY - Guruhlar ichida hisoblash

Har bir bo'lim ichida eng yuqori maoshli xodimni topish:

```sql
SELECT 
    ism, 
    bo'lim, 
    maosh,
    RANK() OVER (PARTITION BY bo'lim ORDER BY maosh DESC) AS bo'lim_reytingi
FROM xodimlar;
```

---

## 📝 CTE (COMMON TABLE EXPRESSIONS)

**CTE** — bu vaqtinchalik natija to'plami (WITH clause). Murakkab so'rovlarni qismlarga ajratish uchun ishlatiladi.

```sql
WITH yuqori_maoshlilar AS (
    SELECT ism, bo'lim, maosh
    FROM xodimlar
    WHERE maosh > 8000
)
SELECT 
    bo'lim,
    COUNT(*) AS xodimlar_soni,
    AVG(maosh) AS o'rtacha_maosh
FROM yuqori_maoshlilar
GROUP BY bo'lim;
```

### 🔁 Recursive CTE

O'z-o'zini chaqiruvchi CTE (masalan, iyerarxiya uchun):

```sql
WITH RECURSIVE sanash AS (
    SELECT 1 AS raqam
    UNION ALL
    SELECT raqam + 1 FROM sanash WHERE raqam < 10
)
SELECT * FROM sanash;
```

---

## 👁️ VIEWS VA MATERIALIZED VIEWS

### 📌 View (Ko'rinish)

Murakkab so'rovni oddiy jadval kabi ishlatish:

```sql
CREATE VIEW it_xodimlari AS
SELECT ism, lavozim, maosh
FROM xodimlar
WHERE bo'lim = 'IT';

-- Ishlatish:
SELECT * FROM it_xodimlari;
```

### 📌 Materialized View (Materializatsiyalangan ko'rinish)

View o'xshash, lekin natija saqlanadi (tezroq, lekin ma'lumot eskirishi mumkin):

```sql
CREATE MATERIALIZED VIEW bo'lim_statistikasi AS
SELECT 
    bo'lim,
    COUNT(*) AS xodimlar,
    AVG(maosh) AS o'rtacha
FROM xodimlar
GROUP BY bo'lim;

-- Ma'lumotni yangilash:
REFRESH MATERIALIZED VIEW bo'lim_statistikasi;
```

---

## 📦 JSON VA ARRAY BILAN ISHLASH

PostgreSQL JSON va massivlarni juda yaxshi qo'llab-quvvatlaydi.

### JSON misollar:

```sql
-- JSON ustunini yaratish
CREATE TABLE foydalanuvchi (
    id SERIAL PRIMARY KEY,
    ism VARCHAR(50),
    metadata JSONB
);

-- JSON qo'shish
INSERT INTO foydalanuvchi (ism, metadata) VALUES
('Ali', '{"yosh": 25, "shahar": "Toshkent", "hobby": ["futbol", "dasturlash"]}');

-- JSON ichidan qiymat olish
SELECT ism, metadata->>'shahar' AS shahar FROM foydalanuvchi;
```

### Array misollar:

```sql
-- Array ustuni
CREATE TABLE lavozimlar (
    id SERIAL PRIMARY KEY,
    ism VARCHAR(50),
    malakalar TEXT[]
);

INSERT INTO lavozimlar VALUES (1, 'Ali', ARRAY['Python', 'SQL', 'Docker']);

-- Array ichidan qidirish
SELECT * FROM lavozimlar WHERE 'SQL' = ANY(malakalar);
```

---

## 🎓 AMALIY MASHG'ULOT

### 📊 KENGAYTIRILGAN KOMPANIYA BAZASI

Amaliyot uchun katta jadval bilan ishlaymiz:

**Xodimlar (employees_full):**
```
┌────┬──────────┬──────────────┬─────────────┬──────────┬────────────┬──────────┐
│ id │ ism      │ lavozim      │ bo'lim      │ maosh    │ ish_sana   │ menejer_id│
├────┼──────────┼──────────────┼─────────────┼──────────┼────────────┼──────────┤
│ 1  │ Sardor   │ CEO          │ Executive   │ 50,000   │ 2015-01-01 │ NULL     │
│ 2  │ Madina   │ VP HR        │ HR          │ 30,000   │ 2016-03-15 │ 1        │
│ 3  │ Ali      │ Developer    │ IT          │ 12,000   │ 2020-06-20 │ 4        │
│ 4  │ Bekzod   │ IT Manager   │ IT          │ 25,000   │ 2017-08-10 │ 1        │
│ 5  │ Dilnoza  │ HR Specialist│ HR          │ 8,000    │ 2021-02-15 │ 2        │
│ 6  │ Komil    │ Senior Dev   │ IT          │ 18,000   │ 2018-11-01 │ 4        │
│ 7  │ Zarina   │ Designer     │ Marketing   │ 10,000   │ 2020-04-10 │ 8        │
│ 8  │ Shahlo   │ Marketing Dir│ Marketing   │ 22,000   │ 2017-05-20 │ 1        │
│ 9  │ Jasur    │ Junior Dev   │ IT          │ 6,000    │ 2023-01-10 │ 4        │
│ 10 │ Feruza   │ Accountant   │ Finance     │ 11,000   │ 2019-09-05 │ 1        │
└────┴──────────┴──────────────┴─────────────┴──────────┴────────────┴──────────┘
```

---

### ✏️ Topshiriqlar

#### 1️⃣ Window Functions
- Har bir bo'limdagi xodimlarni maosh bo'yicha tartiblang va ularning bo'lim ichidagi reytingini ko'rsating (`RANK`).
- Kompaniyadagi eng yuqori 3 ta maoshli xodimni toping (`ROW_NUMBER`).

#### 2️⃣ CTE (WITH clause)
- CTE yordamida IT bo'limidagi o'rtacha maoshni hisoblang va undan yuqori maosh olayotgan barcha xodimlarni ko'rsating.
- Recursive CTE bilan xodim va uning menejerini (va menejerning menjerini) ko'rsating (iyerarxiya).

#### 3️⃣ Views
- `yuqori_daromadlilar` nomli VIEW yarating (maoshi 15,000 dan yuqori xodimlar).
- Materialized View yaratib, har bir bo'limdagi xodimlar sonini va umumiy maosh xarajatlarini saqlang.

#### 4️⃣ JSON/Array (Qo'shimcha)
- Har bir xodimga `malakalar` nomli JSONB ustuni qo'shing va bir nechta xodimga malakalar to'plami kiriting.
- Malakalar ichida "PostgreSQL" borligini qidiradigan so'rov yozing.

#### 5️⃣ Yakuniy loyiha
- Barcha o'tilgan mavzularni birlashtiring: Window Functions, JOIN, GROUP BY, HAVING va CTEdan foydalanib, har bir menejer uchun uning jamoasidagi xodimlar soni va umumiy maosh xarajatlarini ko'rsatuvchi hisobot yarating.

---

## 🎯 O'TILGANLARNI YAKUNIY TAKRORLASH

### ✅ Barcha darslar bo'yicha tayyorlik:

**Dars 1-2:** PostgreSQL o'rnatish, CREATE TABLE, INSERT  
**Dars 3:** SELECT, WHERE, ORDER BY, LIMIT  
**Dars 4:** COUNT, SUM, AVG, GROUP BY, HAVING  
**Dars 5:** UPDATE, DELETE, Transactions  
**Dars 6:** INNER JOIN, LEFT JOIN, RIGHT JOIN, Subqueries  
**Dars 7:** Indexes, B-Tree, EXPLAIN ANALYZE  
**Dars 8:** Functions, Procedures, Triggers  
**Dars 9:** User Management, GRANT, REVOKE, RBAC  
**Dars 10:** Window Functions, CTE, Views, JSON

---

## 🎓 TABRIKLAYAMIZ!

Siz PostgreSQL kursini muvaffaqiyatli yakunladingiz! 🎉

**Keyingi qadamlar:**
1. Real loyihada PostgreSQL ishlatib ko'ring (masalan, Django yoki Node.js bilan).
2. PostgreSQL rasmiy dokumentatsiyasini o'qing.
3. GitHub'da Open Source loyihalarga hissa qo'shing.
4. Sertifikat olish uchun testdan o'ting (agar mavjud bo'lsa).

**Unutmang:** Eng yaxshi o'rganish usuli - amaliyot! Har kuni SQL yozib turing! 💪

---

## 📖 QO'SHIMCHA MANBALAR

- [PostgreSQL Official Documentation](https://www.postgresql.org/docs/)
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
- [SQL Practice: LeetCode Database Problems](https://leetcode.com/problemset/database/)
- [Mode Analytics SQL Tutorial](https://mode.com/sql-tutorial/)