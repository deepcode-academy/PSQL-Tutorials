# 🐘 09-DARS: FOYDALANUVCHILARNI BOSHQARISH VA XAVFSIZLIK

## 📋 MAVZU REJASI

- [PostgreSQLda Role va User tushunchasi](#-postgresqlda-role-va-user-tushunchasi)
- [Role yaratish va boshqarish](#-role-yaratish-va-boshqarish)
- [GRANT va REVOKE (Huquqlar berish va qaytarib olish)](#-grant-va-revoke)
- [RBAC - Role Based Access Control (Rollar orqali boshqarish)](#-rbac---role-based-access-control)
- [Xavfsizlik bo'yicha eng yaxshi amaliyotlar](#-xavfsizlik-boyicha-eng-yaxshi-amaliyotlar)
- [Backup va Restore (Zaxiralash va tiklash)](#-backup-va-restore)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz ma'lumotlar bazasini qanday qilib qal'aga aylantirishni o'rganasiz. Biz kim bazaga kura olishi, kim ma'lumotlarni o'zgartira olishi va kim faqat ko'ra olishini (ReadOnly) belgilashni ko'rib chiqamiz. Ma'lumotlar xavfsizligi har qanday loyihaning eng muhim qismidir.

---

## 📊 TUSHUNTIRISH UCHUN JADVAL

Dars davomidagi misollarni tushunish osonroq bo'lishi uchun, bizda quyidagi **Sotuvlar** jadvali bor deb tasavvur qilamiz. Bu jadvalda kompaniyadagi mahsulot sotuvlari tarixi saqlanadi.

**Sotuvlar (sales):**
```
┌────┬──────────────┬─────────────┬──────────┬────────────┬─────────────┐
│ id │ mahsulot     │ kategoriya  │ narx     │ sana       │ sotuvchi    │
├────┼──────────────┼─────────────┼──────────┼────────────┼─────────────┤
│ 1  │ iPhone 15    │ Electronics │ 1,200    │ 2024-01-10 │ Aziz        │
│ 2  │ MacBook Pro  │ Electronics │ 2,500    │ 2024-01-12 │ Komil       │
│ 3  │ AirPods 3    │ Audio       │ 200      │ 2024-01-15 │ Aziz        │
│ 4  │ Python Book  │ Books       │ 30       │ 2024-01-18 │ Shahlo      │
│ 5  │ SQL Guide    │ Books       │ 45       │ 2024-01-20 │ Aziz        │
│ 6  │ Monitor 27"  │ Electronics │ 400      │ 2024-01-21 │ Shahlo      │
│ 7  │ Keyboard     │ Electronics │ 80       │ 2024-01-22 │ Komil       │
│ 8  │ Java Book    │ Books       │ 40       │ 2024-01-22 │ Shahlo      │
│ 9  │ iPad Air     │ Electronics │ 800      │ 2024-01-23 │ Aziz        │
│ 10 │ Mouse        │ Electronics │ 25       │ 2024-01-24 │ Komil       │
└────┴──────────────┴─────────────┴──────────┴────────────┴─────────────┘
```

---

## 👥 ROLE VA USER TUSHUNCHASI

PostgreSQLda foydalanuvchilar va ularning guruhlari o'rtasida katta farq yo'q. Hammasi **ROLE** deb ataladi.

- **User (Foydalanuvchi):** LOGIN qilish huquqiga ega bo'lgan role.
- **Group (Guruh):** Bir nechta foydalanuvchilarni birlashtiruvchi role (odatda LOGIN huquqisiz).

### 🛠️ Role yaratish

```sql
-- Oddiy foydalanuvchi yaratish (parol bilan)
CREATE ROLE aziz LOGIN PASSWORD 'aziz123';

-- Superuser (hamma narsaga huquqi bor admin) yaratish
CREATE ROLE admin_user SUPERUSER LOGIN PASSWORD 'secret';
```

---

## 🔐 GRANT VA REVOKE

Bu buyruqlar foydalanuvchilarga qanday amallarni bajarishga ruxsat berishni belgilaydi. Yuqoridagi `sotuvlar` jadvali misolida ko'ramiz.

### 📌 Real Misol: Shahlo faqat kitoblar bo'limini ko'ra olsin

**Vaziyat:** Shahlo faqat kitoblarni (Books kategoriyasi) ko'rishi kerak, lekin Electronics qismiga kirmasligi kerak.

```sql
-- 1. Shahlo foydalanuvchisini yaratamiz
CREATE ROLE shahlo LOGIN PASSWORD 'shahlo123';

-- 2. Shahlo o'qiy olsin (SELECT)
GRANT SELECT ON sotuvlar TO shahlo;

-- 3. Endi Shahlo faqat Books qatorlarini ko'rish uchun Row-Level Security ishlatamiz (keyinroq)
-- Yoki: Shahlo faqat o'zining sotuvlarini yangilashi mumkin:
GRANT UPDATE(narx) ON sotuvlar TO shahlo;
```

### 🔒 REVOKE - Huquqni qaytarib olish

**Vaziyat:** Komil avval barcha huquqlarga ega edi, lekin endi u faqat ko'ra olsin.

```sql
-- Komildan DELETE va UPDATE huquqlarini qaytarib olamiz
REVOKE DELETE, UPDATE ON sotuvlar FROM komil;

-- Endi Komil faqat SELECT qila oladi
```

---

## 🏗️ RBAC - ROLE BASED ACCESS CONTROL

Katta proyektlarda har bir foydalanuvchiga alohida huquq berish samarasiz. Buning o'rniga "Rollar" yaratiladi va sotuvchilar shu rollarga biriktiriladi.

### 📌 Real Ssenariy: Do'kondagi rollar tizimi

**Kompaniyamizda 3 xil rol bor:**
1. **sales_rep** (Sotuvchi) - Faqat o'zining sotuvlarini ko'ra oladi va yangi sotuv qo'sha oladi.
2. **analyst** (Analitik) - Barcha sotuvlarni ko'ra oladi, lekin o'zgartira olmaydi.
3. **sales_manager** (Menejer) - Hamma narsani qila oladi.

```sql
-- 1. Rollarni yaratamiz
CREATE ROLE sales_rep;
CREATE ROLE analyst;
CREATE ROLE sales_manager;

-- 2. Rollarga mos huquqlarni beramiz
-- Sotuvchi: faqat ko'rish va qo'shish
GRANT SELECT, INSERT ON sotuvlar TO sales_rep;

-- Analitik: faqat statistika uchun ko'rish
GRANT SELECT ON sotuvlar TO analyst;

-- Menejer: hamma narsa (o'zgartirish, o'chirish ham)
GRANT ALL PRIVILEGES ON sotuvlar TO sales_manager;

-- 3. Haqiqiy xodimlarni rollarga biriktiramiz
CREATE ROLE aziz_user LOGIN PASSWORD 'aziz123';
CREATE ROLE komil_user LOGIN PASSWORD 'komil123';
CREATE ROLE sardor_boss LOGIN PASSWORD 'boss456';

GRANT sales_rep TO aziz_user;      -- Aziz oddiy sotuvchi
GRANT analyst TO komil_user;        -- Komil analitik
GRANT sales_manager TO sardor_boss; -- Sardor boss
```

**Natija:**
- Aziz yangi sotuv qo'sha oladi, boshqalarning sotuvlarini o'zgartira olmaydi.
- Komil hisobotlar tayyorlaydi, lekin ma'lumotlarni o'zgartira olmaydi.
- Sardor hamma narsani boshqaradi.

---

## 🛡️ XAVFSIZLIK BO'YICHA MASLAHATLAR

1. **Least Privilege Principle:** Foydalanuvchiga faqat u ishlashi uchun zarur bo'lgan minimal huquqni bering. Dasturchiga (backendga) `SUPERUSER` huquqini bermang!
2. **Schema-level security:** `public` schemadan huquqlarni olib tashlang (`REVOKE ALL ON SCHEMA public FROM PUBLIC`).
3. **Parol xavfsizligi:** Har doim kuchli parollardan foydalaning va ularni vaqti-vaqti bilan o'zgartiring.
4. **IP Cheklovi:** PostgreSQLga faqat kerakli IP manzillardan ulanishga ruxsat bering (`pg_hba.conf` orqali).

---

## 💾 BACKUP VA RESTORE

Ma'lumotlar yo'qolmasligi uchun doimo nusxa olib turish shart.

### 1️⃣ pg_dump (Nusxa olish)
Terminal (CMD) orqali bajariladi:
```bash
pg_dump -U postgres -d darslik_db > backup.sql
```

### 2️⃣ psql (Nusxadan tiklash)
```bash
psql -U postgres -d yangi_db < backup.sql
```

---

## 🎓 AMALIY MASHG'ULOT

### 📊 TEST JADVALLARI

Ushbu mashg'ulotda quyidagi ikki jadval mavjud deb hisoblang. Rollar va huquqlarni shu jadvallarga nisbatan sozlaysiz.

**1. Xodimlar (employees):**
```
┌────┬───────────┬──────────────┬─────────────┬──────────┐
│ id │ ism       │ lavozim      │ bo'lim      │ maosh    │
├────┼───────────┼──────────────┼─────────────┼──────────┤
│ 1  │ Olimjon   │ Manager      │ Admin       │ 8,000    │
│ 2  │ Nigora    │ Accountant   │ Finance     │ 6,500    │
│ 3  │ Jasur     │ Analyst      │ Analytics   │ 7,000    │
└────┴───────────┴──────────────┴─────────────┴──────────┘
```

**2. Mahsulotlar (products):**
```
┌────┬──────────────┬───────────┬──────────┬──────────┐
│ id │ nomi         │ kategoriya│ narx     │ ombor    │
├────┼──────────────┼───────────┼──────────┼──────────┤
│ 1  │ iPhone 15    │ Telefon   │ 1,200    │ 15       │
│ 2  │ MacBook Pro  │ Laptop    │ 2,500    │ 5        │
│ 3  │ AirPods 3    │ Audio     │ 200      │ 50       │
└────┴──────────────┴───────────┴──────────┴──────────┘
```

---

### 👤 FOYDALANUVCHILAR SSENARIYSI

Siz tizim adminisiz. Quyidagi xodimlar uchun mos rollar va huquqlarni yaratishingiz kerak:

```
┌────────────┬──────────────────┬─────────────────────────────────────┐
│ Foydalanuvchi│ Lavozim        │ Vazifasi (Huquqi)                   │
├────────────┼──────────────────┼─────────────────────────────────────┤
│ Jasur      │ Analitik         │ Barcha jadvallarni faqat KO'RA OLISH │
│ Madina     │ Operator         │ Mahsulotlarni KO'RISH va QO'SHISH   │
│ Sardor     │ Lead Developer   │ Barcha jadvallarda HAMMA AMALLAR     │
│ Botir      │ Junior Dev       │ Faqat 'mahsulotlar'ni boshqarish    │
└────────────┴──────────────────┴─────────────────────────────────────┘
```

---

### ✏️ Topshiriqlar

#### 1️⃣ Rollar strukturasi
- `analitik` nomli role yarating va unga bazadagi barcha jadvallarni faqat o'qish (`SELECT`) huquqini bering.
- `operator` nomli role yarating va unga `SELECT` va `INSERT` huquqlarini bering.

#### 2️⃣ Foydalanuvchi boshqaruvi
- `jasur_analitik` foydalanuvchisini yarating va uni `analitik` roliga a'zo qiling.
- `madina_operator` foydalanuvchisini yarating va uni `operator` roliga biriktiring.

#### 3️⃣ Huquqlarni cheklash
- `botir_dev` foydalanuvchisini yarating.
- Unga faqat `mahsulotlar` jadvali bilan ishlashga (barcha huquqlar) ruxsat bering. Boshqa jadvallarni ko'ra olmasin.

#### 4️⃣ Egallash (Inheritance)
- `Sardor` uchun shunday huquq beringki, u ham `analitik`, ham `operator` huquqlariga ega bo'lsin.

#### 5️⃣ Xavfsizlik testi
- `jasur_analitik` foydalanuvchisi bilan bazaga kirib, biron qatorni o'chirib (`DELETE`) ko'ring. Baza qanday xato berganini yozing.

#### 6️⃣ Parolni o'zgartirish
- `madina_operator` ning paroli eskirgan deb hisoblang va unga yangi xavfsiz parol o'rnating.

#### 7️⃣ Zaxira nusxa
- Butun bazangizni `company_security_backup.sql` nomi bilan nusxasini olish buyrug'ini (pg_dump) yozing.

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:
- [x] Role va User farqi (aslida bitta narsaligi).
- [x] GRANT va REVOKE orqali huquqlarni nozik boshqarish.
- [x] RBAC arxitekturasini qurish (Katta loyihalar uchun).
- [x] Bazadan nusxa olish va xavfsizlik qoidalarini.

### 📚 Keyingi darsda (Yakuniy darslar):
**10-DARS: Murakkab Topiklar va Review**
- Window Functions (Murakkab hisobotlar)
- Common Table Expressions (CTE - WITH clause)
- Full-Text Search (Qidiruv tizimi yasash)
- O'tilganlarni yakunlash va Sertifikatga tayyorgarlik! 🎓

---

## 📖 QO'SHIMCHA MASLAHATLAR

1. **Don't use Postgres User:** Hech qachon production ilovangizni `postgres` (superuser) foydalanuvchisi bilan ulamang!
2. **Audit:** Kim, qachon, qaysi huquqdan foydalanganini doimo kuzatib boring (`log_statement = 'all'`).
3. **Password hashing:** Parollarni saqlashda PostgreSQL `scram-sha-256` algoritmidan foydalanishiga ishonch hosil qiling.