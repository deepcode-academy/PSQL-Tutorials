# 🐘 07-DARS: INDEKSLAR VA PERFORMANCE OPTIMIZATSIYA (MUKAMMAL)

## 📋 MAVZU REJASI

- [Ma'lumotlar bazasi "Miyasi": Indeks nima?](#-malumotlar-bazasi-miyasi-indeks-nima)
- [Heads-up: Seq Scan vs Index Scan](#-seq-scan-vs-index-scan)
- [🌳 B-Tree: Standart va eng kuchli algoritm](#-b-tree-standart-va-eng-kuchli-algoritm)
- [🛠️ Indeks Turlari (Chuqur tahlil)](#-indeks-turlari-chuqur-tahlil)
  - [Composite (Murakkab) indekslar](#composite-murakkab-indekslar)
  - [Partial (Qisman) indekslar](#partial-qisman-indekslar)
  - [Expression (Ifodali) indekslar](#expression-ifodali-indekslar)
- [🔍 EXPLAIN ANALYZE-ni o'qish sirlari](#-explain-analyze-ni-oqish-sirlari)
- [🚀 Index-Only Scan va Covering Indexes](#-index-only-scan-va-covering-indexes)
- [⚠️ Indekslarning "Yomon" tomonlari](#-indekslarning-yomon-tomonlari)
- [🎓 Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz oddiy "Indeks yarating" degan buyruqdan ko'ra ko'prog'ini o'rganasiz. Biz PostgreSQL "kapoti" ostiga qaraymiz va so'rovlar qanday qilib millisekundlarda millionlab qatorlarni topishini tushunib olamiz.

---

## 🧠 MA'LUMOTLAR BAZASI "MIYASI": INDEKS NIMA?

### 📌 Heap nima?
Indeksiz har qanday jadval PostgreSQLda **Heap** (to'da) deb ataladi. Ma'lumotlar tartibsiz, qaysi qator bo'sh bo'lsa o'sha joyga yozib ketiladi.
Agar siz heap ichidan biror nima qidirsangiz, baza hamma qatorni o'qib chiqishga majbur. Bu katta kutubxonadan kitobni qidirib topish uchun hamma kitobning muqovasini o'qib chiqishga o'xshaydi.

### 📚 Indeks - Mundarija
**Indeks** — bu ma'lumotlarni tartiblangan holda saqlaydigan alohida jadvalcha. U asosan ikki narsani saqlaydi:
1. **Qiymat** (Masalan, foydalanuvchi ismi: "Ali")
2. **Pointer** (Bu qiymat asosiy jadvalning qaysi block/sahifasida ekanini ko'rsatuvchi manzil).

---

## 🐢 Seq Scan vs Index Scan 🐎

So'rov bajarganingizda PostgreSQL "Query Planner" (so'rovni rejalashtiruvchi) ikkita yo'ldan birini tanlaydi:

1. **Sequential Scan (Seq Scan):** Jadvalni boshidan oxirigacha o'qiydi.
   - *Qachon tanlaydi?* Jadval kichik bo'lsa yoki indeks bo'lmasa.
   
2. **Index Scan:** Avval indeksga qaraydi, manzilni oladi va to'ppa-to'g'ri o'sha qatorga boradi.
   - *Qachon tanlaydi?* Indeks bor bo'lsa va so'rov bir nechta qatorni qidirayotgan bo'lsa.

---

## 🌳 B-TREE: STANDART VA ENG KUCHLI ALGORITM

PostgreSQLda `CREATE INDEX` yozsangiz, u avtomatik ravishda **B-Tree** (Balanced Tree) yaratadi. 

### ⚙️ Qanday ishlaydi?
B-Tree ma'lumotlarni daraxt kabi shakllantiradi:
- **Root node (ildiz):** Eng tepada joylashgan qidirish yo'nalishini beruvchi tugun.
- **Leaf nodes (barglar):** Eng pastki qatlam, haqiqiy ma'lumot manzillarini saqlaydi.

**Nega tez?**
Agar sizda 1,000,000 dona ma'lumot bo'lsa, B-Tree yordamida kerakli qiymatni topish uchun bor-yo'g'i **20-22 ta** amaliyot kifoya! (Logarifmik algoritm).

---

## 🛠️ INDEKS TURLARI (CHUQUR TAHLIL)

### 1️⃣ Composite (Murakkab) Indekslar
Bir nechta ustun uchun bitta indeks.
```sql
CREATE INDEX idx_users_name_city ON users(last_name, first_name);
```
**Qoidasi:** Bu indeks "Telefon kitobi"ga o'xshaydi. Avval Familiya bo'yicha tartiblangan, keyin Ism bo'yicha. 
- Siz "Valiyev Ali"ni qidirishingiz mumkin (Tez).
- Siz faqat "Valiyev"ni qidirishingiz mumkin (Tez).
- **Lekin** Siz faqat "Ali"ni qidirib topolmaysiz (Baza kitobni boshidan oxirigacha varaqlashga majbur).

### 2️⃣ Partial (Qisman) Indekslar
Faqat ma'lum bir shartga javob beradigan qatorlar uchun indeks.
```sql
-- Faqat aktiv foydalanuvchilar uchun indeks (Joyni tejaydi!)
CREATE INDEX idx_active_users ON users(id) WHERE is_active = TRUE;
```
Bu indeks diskdan kam joy oladi va bazaga yuklamani kamaytiradi.

### 3️⃣ Expression (Ifodali) Indekslar
Funksiya natijasi bo'yicha indeks.
```sql
-- Agar siz doim LOWER(email) bilan qidirsangiz:
CREATE INDEX idx_users_lower_email ON users(LOWER(email));
```
Shunchaki `email`ga indeks qo'ysangiz, `WHERE LOWER(email) = ...` so'rovingiz indeksni ishlatmaydi!

---

## 🔍 EXPLAIN ANALYZE-NI O'QISH SIRLARI

So'rovni optimallashtirish uchun eng muhim qurol.

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE total_amount > 500000;
```

**Natijada nimalarga qarash kerak?**
1. **Planning Time:** So'rov rejasini o'ylashga ketgan vaqt.
2. **Execution Time:** Haqiqiy ishga ketgan vaqt (asosiy ko'rsatkich).
3. **Rows Removed by Filter:** Indeksiz qidirilganda natijaga to'g'ri kelmagan qancha qator o'qib chiqilgani (Qanchalik ko'p bo'lsa, shunchalik yomon).
4. **Actual Rows:** Haqiqiy qaytarilgan natijalar soni.

---

## 🚀 INDEX-ONLY SCAN VA COVERING INDEXES

Eng tezkor qidiruv usuli. 
Agar sizga kerakli hamma ma'lumot **Indeks**ning o'zida bo'lsa, PostgreSQL asosiy jadvalga (Heap) umuman bormaydi. Bu **Index Only Scan** deyiladi.

```sql
-- id va ism indeksda bo'lsa:
SELECT id, ism FROM foydalanuvchilar WHERE id = 100;
```

---

## ⚠️ INDEKSLARNING "YOMON" TOMONLARI

Indeks tekin emas! Har bir indeksning bahosi bor:
1. **INSERT/UPDATE sekinlashadi:** Har safar yangi ma'lumot qo'shilganda, baza nafaqat asosiy jadvalni, balki barcha indekslarni ham yangilab chiqishi kerak.
2. **Disk bloat:** Indekslar diskda joy egallaydi. Ba'zida indeks hajmi jadvalning o'zidan katta bo'lib ketadi.
3. **Write Amplification:** Juda ko'p indeks bo'lsa, bitta qatorni o'zgartirish o'nlab disk amallarini talab qiladi.

---

## 🎓 AMALIY MASHG'ULOT

### 📊 TEST JADVALI: "SHOP_PRODUCTS"

Tasavvur qiling, sizda **5,000,000** ta mahsuloti bor online do'kon bazasi bor.

**Shop_Products:**
```
┌────┬──────────────┬────────────┬─────────────┬──────────┬───────────┐
│ id │ code         │ name       │ category_id │ price    │ is_sale   │
├────┼──────────────┼────────────┼─────────────┼──────────┼───────────┤
│ 1  │ PR-001       │ iPhone 15  │ 10          │ 12,000   | TRUE      │
│ 2  │ pr-002       │ Laptop HP  │ 10          │ 5,000    │ FALSE     │
│ ...│ ...          │ ...        │ ...         │ ...      │ ...       │
└────┴──────────────┴────────────┴─────────────┴──────────┴───────────┘
```

---

### ✏️ Topshiriqlar (Sizning vazifangiz - tahlil qilish)

#### 1️⃣ Case-Insensitive Qidiruv
Sizning dasturingizda foydalanuvchilar mahsulot kodini `LOWER(code)` qilib qidirishadi (kichik harfda).
- **Vazifa:** Faqat bu holat uchun eng to'g'ri indeks turini tanlang va yaratish buyrug'ini yozing.
- **Nima uchun** oddiy `code` indeks bu yerda ish bermaydi?

#### 2️⃣ Qisman Indeks (Partial)
Do'konda faqat chegiruvdagi (`is_sale = TRUE`) mahsulotlar ro'yxati juda tez-tez ko'riladi. Barcha mahsulotlar 5 millionta, lekin chegiruvdagilar bor-yo'g'i 5,000 ta.
- **Vazifa:** Diskni tejaydigan va so'rovni maksimal tezlashtiradigan indeks yarating.

#### 3️⃣ EXPLAIN Tahlili
Quyidagi ikkita natijani solishtiring:
- Plan A: `Seq Scan on products (cost=0.00..105.00 rows=5000 width=32)`
- Plan B: `Index Scan using idx_name on products (cost=0.43..8.45 rows=1 width=32)`
- **Savol:** Qaysi biri tezroq? Natijadagi `cost` nima uchun Plan B da shunchalik past?

#### 4️⃣ Composite Indeks tartibi
Sizda `category_id` va `price` ustunlariga composite indeks bor: `CREATE INDEX idx_cat_price ON products(category_id, price)`.
- **Topshiriq:** Quyidagi so'rovlardan qaysi biri bu indeksni to'liq ishlatadi?
  A. `WHERE category_id = 5`
  B. `WHERE price > 1000`
  C. `WHERE category_id = 5 AND price > 1000`

#### 5️⃣ Maintenance
Jadvaldan juda ko'p ma'lumot o'chirildi va qo'shildi. Indekslar "shishib" (bloat) ketdi.
- **Topshiriq:** Indeksni butunlay o'chirmasdan, uni yangidan, ixcham qilib "qayta qurish" (rebuild) uchun qaysi buyruqni berish kerak?

---

## 🎯 DARS YAKUNLARI

✅ Siz tushunib yetdingiz:
- Indeks faqat "tezlashtirgich" emas, u alohida ma'lumot strukturasidir.
- B-Tree algoritmida qidirish logarifmik tezlikda bo'lishini.
- Har doim ham indeks yaxshi emasligini (Write overhead).
- EXPLAIN ANALYZE dasturchining eng yaqin do'stiligini.

### 📚 Keyingi darsda:

**08-DARS: Stored Procedures va Funksiyalar**
Biz faqat ma'lumot qidirishni emas, balki bazaning o'zi ichida qanday qilib mantiqiy algoritmlar yozishni o'rganamiz!
