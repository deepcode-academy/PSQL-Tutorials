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

Bu buyruqlar foydalanuvchilarga qanday amallarni bajarishga ruxsat berishni belgilaydi.

### 1️⃣ GRANT (Ruxsat berish)
```sql
-- Faqat SELECT (o'qish) huquqini berish
GRANT SELECT ON xodimlar TO aziz;

-- Barcha huquqlarni berish (SELECT, INSERT, UPDATE, DELETE)
GRANT ALL PRIVILEGES ON TABLE buyurtmalar TO aziz;

-- Database-ga ulanish huquqini berish
GRANT CONNECT ON DATABASE darslik TO aziz;
```

### 2️⃣ REVOKE (Ruxsatni qaytarib olish)
```sql
-- SELECT huquqini qaytarib olish
REVOKE SELECT ON xodimlar FROM aziz;
```

---

## 🏗️ RBAC - ROLE BASED ACCESS CONTROL

Katta proyektlarda har bir foydalanuvchiga alohida huquq berish xato yo'l. Buning o'rniga "Rollar" yaratiladi va foydalanuvchilar shu ro'llarga a'zo qilinadi.

**Ssenariy:**
1. `readonly` roli - faqat ko'ra oladi.
2. `developer` roli - o'zgartira oladi.

```sql
-- 1. Rollarni yaratamiz
CREATE ROLE readonly;
CREATE ROLE developer;

-- 2. Rollarga huquqlarni beramiz
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO developer;

-- 3. Foydalanuvchilarni ro'llarga biriktiramiz
GRANT readonly TO anvar;
GRANT developer TO bekzod;
```

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

### 📊 TEST SSENARIY: "KOMPANIYA BAZASI"

Tasavvur qiling, sizda katta kompaniya bazasi bor va siz admin ekansiz.

**Foydalanuvchilar ro'yxati:**
```
┌────────────┬──────────────────┬─────────────────────────────────┐
│ Ism        │ Lavozim          │ Vazifasi                        │
├────────────┼──────────────────┼─────────────────────────────────┤
│ Jasur      │ Analitika        │ Faqat ma'lumotlarni ko'rishi shart│
│ Madina     │ Operator         │ Ma'lumot qo'shishi va ko'rishi   │
│ Sardor     │ Lead Dev         │ Bazani o'zgartira olishi kerak  │
│ Botir      │ Junior Dev       │ Faqat bitta jadval bilan ishlaydi│
└────────────┴──────────────────┴─────────────────────────────────┘
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