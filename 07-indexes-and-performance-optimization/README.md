# 🐘 07-DARS: INDEKSLAR VA PERFORMANCE OPTIMIZATSIYA

## 📋 MAVZU REJASI

- [Indekslar tushunchasi (Nega so'rovlar sekin?)](#-indekslar-tushunchasi)
- [B-Tree indeksi - Standart tanlov](#-b-tree-indeksi)
- [Indeks yaratish turlari (Single, Composite, Unique)](#-indeks-yaratish-turlari)
- [EXPLAIN va EXPLAIN ANALYZE - So'rovni tahlil qilish](#-explain-va-explain-analyze)
- [Performance Optimizatsiya sirlari](#-performance-optimizatsiya-sirlari)
- [Indekslarning salbiy tomonlari (Over-indexing)](#-indekslarning-salbiy-tomonlari)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz quyidagilarni o'rganasiz:

✅ Millionlab ma'lumotlar ichidan qidirishni tezlashtirish  
✅ B-Tree va Hash indekslari farqini tushunish  
✅ `EXPLAIN` orqali bazaning ichki "miyasini" ko'rish  
✅ So'rovlar qayerda "tiqilib" qolayotganini aniqlash  
✅ Reallikda ma'lumotlar bazasini qanday optimallashtirish  
✅ Indekslarni to'g'ri boshqarish (yaratish va o'chirish)

---

## 📚 INDEKSLAR TUSHUNCHASI

### 📌 Nega bazamiz sekin ishlaydi?

Tasavvur qiling, sizda 1000 sahifalik kitob bor va siz undan "PostgreSQL" so'zini qidiryapsiz.
1. **Index'siz:** Har bir sahifani birma-bir o'qib chiqasiz (bu SQLda **Sequential Scan** deyiladi).
2. **Index'li:** Kitob oxiridagi mundarijaga qaraysiz va "P" harfidan "PostgreSQL" qaysi sahifada ekanini salkam 1 soniyada topasiz (bu **Index Scan**).

**Indeks** — bu bazadagi ma'lumotlarni tartiblangan holda saqlaydigan alohida struktura. U qidirish vaqtini 100-1000 barobar tezlashtiradi.

---

## 🌳 B-TREE INDEKSI - STANDART TANLOV

### 📌 B-Tree (Balanced Tree) nima?

PostgreSQLda eng ko'p ishlatiladigan indeks turi. U ma'lumotlarni daraxtsimon strukturada saqlaydi. 

**Afzalliklari:**
- `=`, `>`, `<`, `>=`, `<=`, `BETWEEN`, `IN`, `LIKE 'abc%'` amallarida juda tez ishlaydi.
- Ma'lumotlarni tartiblangan holda saqlaydi (ORDER BY uchun ideal).

### 💻 Hash Index haqida qisqacha
Faqat tenglik (`=`) uchun ishlaydi. B-Tree kabi keng qamrovli emas, shuning uchun PostgreSQLda odatda B-Tree ishlatish tavsiya etiladi.

---

## 🛠️ INDEKS YARATISH TURLARI

### 1️⃣ Oddiy Indeks (Single Column)
Bitta ustun bo'yicha qidirish uchun.

```sql
CREATE INDEX idx_users_email ON users(email);
```

### 2️⃣ Unikal Indeks (Unique Index)
Ustundagi ma'lumotlar krorlanmasligini ham ta'minlaydi.

```sql
CREATE UNIQUE INDEX idx_users_username ON users(username);
```
*(Eslatma: Primary Key yaratilganda PostgreSQL avtomatik unikal indeks yaratadi)*

### 3️⃣ Murakkab Indeks (Composite Index)
Ikki yoki undan ortiq ustun bo'yicha tez-tez birga qidirilsa ishlatiladi.

```sql
-- Familiya va Ism bo'yicha birga qidirish uchun
CREATE INDEX idx_users_full_name ON users(last_name, first_name);
```

> [!IMPORTANT]
> Composite indeksda ustunlar tartibi muhim! `(A, B)` indeksi `WHERE A=... AND B=...` uchun ishlaydi, lekin faqat `WHERE B=...` uchun har doim ham ishlamasligi mumkin.

---

## 🔍 EXPLAIN VA EXPLAIN ANALYZE

### 📌 So'rov rejasini tahlil qilish

Siz yozgan so'rov bazada qanday bajarilishini bilish uchun `EXPLAIN` buyrug'idan foydalanasiz.

- `EXPLAIN` — Faqat rejani (plan) ko'rsatadi (so'rovni bajarib o'tirmaydi).
- `EXPLAIN ANALYZE` — So'rovni bajaradi va **haqiqiy** qancha vaqt ketganini ko'rsatadi.

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'ali@test.com';
```

**Natijada ko'riladigan muhim terminlar:**
- **Seq Scan:** Jadval boshidan oxirigacha o'qilgan (Sekin ⚠️).
- **Index Scan:** Indeks ishlatilgan (Tez ✅).
- **Cost:** Bazaning taxminiy "mehnat" o'lchov birligi.
- **Execution Time:** Haqiqiy ketgan vaqt (ms da).

---

## ⚡ PERFORMANCE OPTIMIZATSIYA SIRLARI

### 💡 Indekslarni qachon ishlatish kerak?
1. Qidiruvda (`WHERE`) ko'p ishlatiladigan ustunlarga.
2. Jadvallarni bog'lashda (`JOIN`) ishlatiladigan Foreign Key'larga.
3. Saralashda (`ORDER BY`) ishtirok etadigan ustunlarga.

### ⚠️ Indekslarning salbiy tomonlari (Over-indexing)
Har bir yangi indeks:
- `INSERT`, `UPDATE`, `DELETE` amallarini **sekinlashtiradi** (chunki bazaga qo'shishda indeksni ham yangilab chiqish kerak).
- Diskda qo'shimcha joy egallaydi.

> [!TIP]
> Keraksiz yoki faqat bitta ma'lumot (masalan `jinsi`) bor ustunlarga indeks qo'yish foydasiz.

### 🧹 Maintenance (Xizmat ko'rsatish)
Vaqti-vaqti bilan bazadagi statistikani yangilab turish kerak, toki baza qaysi indeks yaxshi ekanini bilsin:

```sql
ANALYZE users; -- Statistika yangilash
VACUUM ANALYZE users; -- O'chirilgan joylarni tozalash va statistika yangilash
```

---

## 🎓 AMALIY MASHG'ULOT

### 📊 TEST MA'LUMOTLAR

Ushbu amaliyot uchun **1 000 000** ta qatori bor `foydalanuvchilar` jadvalini tasavvur qiling.

**Foydalanuvchilar (users):**
```
┌────┬───────────┬──────────────┬─────────────┬─────────────┬──────────┐
│ id │ ism       │ familiya     │ email       │ shahar      │ yosh     │
├────┼───────────┼──────────────┼─────────────┼─────────────┼──────────┤
│ 1  │ Ali       │ Valiyev      │ ali@...     │ Toshkent    │ 25       │
│ 2  │ Madina    │ Karimova     │ mad@...     │ Samarqand   │ 22       │
│ 3  │ ...       │ ...          │ ...         │ ...         │ ...      │
└────┴───────────┴──────────────┴─────────────┴─────────────┴──────────┘
```

---

### ✏️ Topshiriqlar

#### 1️⃣ Indeks yaratish asoslari
- Foydalanuvchilarning `email` ustuni bo'yicha tez qidirish uchun unikal indeks yarating.
- `shahar` va `yosh` ustunlari bo'yicha composite indeks yarating.

#### 2️⃣ Tahlil qilish (EXPLAIN)
- Quyidagi so'rovni `EXPLAIN ANALYZE` bilan tekshiring (indeksiz holatda):
  `SELECT * FROM users WHERE email = 'bek@test.uz';`
- Natijadagi `Seq Scan` ni toping va `Execution Time` ni yozib oling.

#### 3️⃣ Effektni tekshirish
- Email uchun indeks yarating va yana o'sha so'rovni `EXPLAIN ANALYZE` bilan tekshiring.
- `Index Scan` paydo bo'lganini va vaqt qanchaga qisqarganini ko'ring.

#### 4️⃣ Saralashni optimallashtirish
- Foydalanuvchilarni `yosh` bo'yicha tez saralash (`ORDER BY`) uchun mos indeks yarating.
- B-Tree indeksini ishlating.

#### 5️⃣ Indeksni o'chirish
- Yaratilgan `shahar_yosh` indeksini bazadan o'chirib tashlang.
- Keraksiz indekslar nima uchun bazaga yomon ta'sir qilishini izohlab bering.

#### 6️⃣ Maintenance mashqi
- Jadval kutilmaganda sekinlashib qolsa, statistikani yangilash uchun qaysi buyruqni berish kerakligini yozing.

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:

- [x] Indekslar bazani qanday tezlashtirishini
- [x] B-Tree indeksining ishlash prinsipi
- [x] Composite va Unique indekslar farqi
- [x] EXPLAIN ANALYZE yordamida "Slow Query"larni topish
- [x] Indekslarning bazaga salbiy ta'siri (Over-indexing)

### 📚 Keyingi darsda:

**08-DARS: PSQL Funksiyalari va Stored Procedurelar**
- SQLda o'z funksiyangizni yozish  
- PL/pgSQL tili asoslari  
- Ma'lumotlarni avtomatlashtirish  
- Triggerlar bilan ishlash

---

## 📖 QO'SHIMCHA MANBALAR

- [PostgreSQL Indexing Official Docs](https://www.postgresql.org/docs/current/indexes.html)
- [Use The Index, Luke! (SQL indexing guide)](https://use-the-index-luke.com/)
- [PostgreSQL EXPLAIN Explained](https://explain.depesz.com/)
