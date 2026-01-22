# 🐘 05-DARS: MA'LUMOTLARNI O'ZGARTIRISH VA TRANZAKSIYALAR

## 📋 MAVZU REJASI

- [UPDATE - Ma'lumotlarni yangilash](#-update---malumotlarni-yangilash)
- [DELETE - Ma'lumotlarni o'chirish](#-delete---malumotlarni-ochirish)
- [TRUNCATE - Jadvalni tozalash](#-truncate---jadvalni-tozalash)
- [RETURNING - Natijani qaytarish](#-returning---natijani-qaytarish)
- [Tranzaksiyalar (BEGIN, COMMIT, ROLLBACK)](#-tranzaksiyalar-begin-commit-rollback)
- [Ma'lumotlar yaxlitligi (Data Integrity)](#-malumotlar-yaxlitligi-data-integrity)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz quyidagilarni o'rganasiz:

✅ Mavjud ma'lumotlarni xavfsiz yangilash (`UPDATE`)  
✅ Ma'lumotlarni shart asosida o'chirish (`DELETE`)  
✅ Tranzaksiyalar yordamida ma'lumotlar xavfsizligini ta'minlash  
✅ Xatolik yuz berganda o'zgarishlarni bekor qilish (`ROLLBACK`)  
✅ O'zgartirilgan ma'lumotlarni darhol ko'rish (`RETURNING`)  
✅ Real proyektlarda ma'lumotlarni boshqarish strategiyalari

---

## 🔄 UPDATE - MA'LUMOTLARNI YANGILASH

### 📌 UPDATE nima?

**UPDATE** — jadvaldagi mavjud qatorlarning qiymatlarini o'zgartirish uchun ishlatiladi.

### 💻 Sintaksis

```sql
UPDATE table_name
SET column1 = value1, 
    column2 = value2
WHERE condition;
```

> [!CAUTION]
> Agar `WHERE` shartini yozishni unutsangiz, jadvaldagi **BARCHA** qatorlar yangilanib ketadi!

### 🧪 Misollar

#### 1️⃣ Bitta ustunni yangilash

```sql
-- IDsi 1 bo'lgan mijozning ismini o'zgartirish
UPDATE mijozlar
SET ism = 'Akmal'
WHERE id = 1;
```

#### 2️⃣ Bir nechta ustunni yangilash

```sql
-- Mahsulot narxi va sonini yangilash
UPDATE mahsulotlar
SET narx = 5500000,
    stock_quantity = 15
WHERE slug = 'laptop-hp';
```

#### 3️⃣ Hisob-kitob bilan yangilash

```sql
-- Barcha IT bo'limi xodimlarining maoshini 10% ga oshirish
UPDATE xodimlar
SET maosh = maosh * 1.10
WHERE bo'lim = 'IT';
```

---

## 🗑️ DELETE - MA'LUMOTLARNI O'CHIRISH

### 📌 DELETE nima?

**DELETE** — jadvaldan qatorlarni o'chirib tashlaydi.

### 💻 Sintaksis

```sql
DELETE FROM table_name
WHERE condition;
```

> [!WARNING]
> `WHERE` shartisiz `DELETE` buyrug'i jadvaldagi barcha ma'lumotlarni o'chirib yuboradi, lekin jadvalning o'zi (strukturasi) qoladi.

### 🧪 Misollar

#### 1️⃣ Shart bo'yicha o'chirish

```sql
-- Faol bo'lmagan foydalanuvchilarni o'chirish
DELETE FROM foydalanuvchilar
WHERE is_active = FALSE;
```

#### 2️⃣ Murakkab shart bilan o'chirish

```sql
-- Bekor qilingan va 30 kundan eski buyurtmalarni o'chirish
DELETE FROM buyurtmalar
WHERE status = 'cancelled' 
  AND ordered_at < NOW() - INTERVAL '30 days';
```

---

## 🧹 TRUNCATE - JADVALNI TOZALASH

### 📌 TRUNCATE nima?

**TRUNCATE** — jadvaldagi barcha ma'lumotlarni juda tez o'chirib tashlaydi.

**DELETE va TRUNCATE farqi:**

| Xususiyat | DELETE | TRUNCATE |
|-----------|--------|----------|
| **Tezlik** | Sekinroq (har bir qatorni tekshiradi) | Juda tez (to'g'ridan-to'g'ri tozalaydi) |
| **WHERE** | Ishlatish mumkin | Ishlatib bo'lmaydi |
| **Rollback** | Mumkin | Ba'zi holatlarda qiyin |
| **Triggerlar** | Ishga tushadi | Ishga tushmaydi |

```sql
-- Jadvalni to'liq tozalash
TRUNCATE TABLE loglar;
```

---

## 🔙 RETURNING - NATIJANI QAYTARISH

### 📌 RETURNING nima?

O'zgarish amalga oshgandan so'ng, qaysi qatorlar o'zgarganini darhol ko'rish uchun ishlatiladi.

```sql
-- Narxni yangilab, yangi narxni natija sifatida olish
UPDATE mahsulotlar
SET narx = narx + 50000
WHERE kategoriya = 'Kitob'
RETURNING id, nom, narx;
```

**Natija:**
```
 id |       nom       |  narx
----+-----------------+--------
 7  | PostgreSQL Book | 300000
 8  | Python Book     | 350000
```

---

## 🛡️ TRANZAKSIYALAR (BEGIN, COMMIT, ROLLBACK)

### 📌 Tranzaksiya nima?

**Tranzaksiya** — bir nechta SQL buyruqlarini bitta "paket"ga birlashtirish. Ya'ni, yoki barcha buyruqlar muvaffaqiyatli bajariladi, yoki birontasi xato bo'lsa, hech biri bajarilmaydi (hammasi bekor qilinadi).

**Mashhur misol: Bank o'tkazmasi**
1. Ali hisobidan 100 ming so'm ayirish.
2. Vali hisobiga 100 ming so'm qo'shish.
*(Agar 2-qadamda chiroq o'chib qolsa, 1-qadam ham bekor bo'lishi shart!)*

### 💻 Kalit so'zlar

- `BEGIN` — Tranzaksiyani boshlash.
- `COMMIT` — Barcha o'zgarishlarni tasdiqlash (saqlash).
- `ROLLBACK` — Barcha o'zgarishlarni bekor qilish (ortga qaytarish).

### 🧪 Misol

```sql
BEGIN;

-- 1. Pulni ayirish
UPDATE hisoblar SET balance = balance - 100000 WHERE user_id = 1;

-- 2. Pulni qo'shish
UPDATE hisoblar SET balance = balance + 100000 WHERE user_id = 2;

-- Agar hammasi joyida bo'lsa:
COMMIT;

-- Agar biror xato bo'lsa (masalan, user 2 topilmasa):
ROLLBACK;
```

---

## 🎓 AMALIY MASHG'ULOT

### 📊 MIJOZLAR VA BALANCE JADVALI

Quyidagi ma'lumotlar bilan ishlang:

```
┌────┬────────────┬─────────────┬──────────────┬──────────┬───────────┐
│ id │ ism        │ shahar      │ balance      │ status   │ type      │
├────┼────────────┼─────────────┼──────────────┼──────────┼───────────┤
│ 1  │ Ali        │ Toshkent    │ 1,500,000    │ Active   │ Premium   │
│ 2  │ Madina     │ Samarqand   │ 450,000      │ Active   │ Standard  │
│ 3  │ Bekzod     │ Buxoro      │ 2,000,000    │ Inactive │ Premium   │
│ 4  │ Dilnoza    │ Toshkent    │ 0            │ Active   │ Standard  │
│ 5  │ Sardor     │ Farg'ona    │ 750,000      │ Active   │ Standard  │
│ 6  │ Zarina     │ Toshkent    │ 3,200,000    │ Active   │ Gold      │
│ 7  │ Abbos      │ Namangan    │ 150,000      │ Inactive │ Basic     │
└────┴────────────┴─────────────┴──────────────┴──────────┴───────────┘
```

---

### ✏️ Topshiriqlar

#### 1️⃣ UPDATE - Oddiy yangilash

- Toshkentlik barcha mijozlarning balansiga 50,000 so'm bonus qo'shing.
- Balansi 0 bo'lgan mijozlarning statusini 'Inactive' ga o'zgartiring.
- 'Samarqand' shahrini 'Samarkand' deb to'g'irlang.

---

#### 2️⃣ UPDATE - Shartli yangilash

- 'Premium' foydalanuvchilarning balansini 10% ga oshiring.
- IDsi 3 bo'lgan foydalanuvchini 'Active' qiling va balansini 0 qiling.
- Balansi 1,000,000 dan yuqori va 'Active' bo'lganlarni 'Gold' turiga o'tkazing.

---

#### 3️⃣ DELETE - O'chirish

- Statusi 'Inactive' bo'lgan va balansi 200,000 dan kam bo'lgan mijozlarni o'chiring.
- 'Basic' turidagi barcha mijozlarni o'chirib tashlang.
- Farg'onalik barcha foydalanuvchilarni bazadan o'chiring.

---

#### 4️⃣ RETURNING operatori

- IDsi 5 bo'lgan mijozning shahrini 'Toshkent' ga o'zgartiring va uning yangi ma'lumotlarini qaytaring.
- Barcha 'Active' mijozlarning balansini 5% ga kamaytiring va faqat ularning ismlari va yangi balansini ko'ring.

---

#### 5️⃣ Tranzaksiyalar (Mantiqiy)

- Tranzaksiya boshlang: 1-mijozdan 200,000 ayiring va 2-mijozga qo'shing. O'zgarishlarni saqlang.
- Tranzaksiya boshlang: Barcha foydalanuvchilarni o'chiring. Keyin fikringizdan qayting va o'zgarishni bekor qiling.

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:

- [x] UPDATE bilan ma'lumotlarni shartli yangilash
- [x] DELETE va TRUNCATE farqi
- [x] Ma'lumotlarni o'chirib yubormaslik uchun ehtiyot choralari
- [x] RETURNING orqali o'zgargan qatorlarni ko'rish
- [x] BEGIN, COMMIT, ROLLBACK bilan xavfsiz ishlash

### 📚 Keyingi darsda:

**06-DARS: Jadvallarni bog'lash (JOINS)**
- INNER JOIN, LEFT JOIN, RIGHT JOIN  
- Bir nechta jadvallar bilan birga ishlash  
- Foreign Key bilan munosabatlar  
- Murakkab so'rovlar yasash

---

## 📖 QO'SHIMCHA RESURSLAR

### 💡 Professional maslahatlar

1. **Doimo tekshiring:** `UPDATE` yoki `DELETE` qilishdan oldin, shartingiz to'g'riligini `SELECT` orqali tekshirib ko'ring.
2. **Tranzaksiyalardan foydalaning:** Katta o'zgarishlar qilayotganda `BEGIN` ishlatish xatolikdan asraydi.
3. **Backup:** Muhim ma'lumotlarni o'chirishdan oldin ularning nusxasini oling.
4. **RETURNING:** Web-backendlarda (Node.js, Python) bu operator juda qo'l keladi, chunki bitta so'rovda ham yangilab, ham yangi ma'lumotni front-endga qaytarish mumkin.
