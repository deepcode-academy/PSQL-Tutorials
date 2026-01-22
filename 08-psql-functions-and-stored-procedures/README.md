# 🐘 08-DARS: PSQL FUNKSIYALARI, PROSEDURALAR VA TRIGGERLAR

## 📋 MAVZU REJASI

- [Funksiyalar (Functions) va Proseduralar (Stored Procedures) farqi](#-funksiyalar-va-proseduralar-farqi)
- [PL/pgSQL asoslari (O'zgaruvchilar, Shartlar)](#-plpgsql-asoslari)
- [Custom Funksiya yaratish](#-custom-funksiya-yaratish)
- [Stored Procedures (Proseduralar)](#-stored-procedures-proseduralar)
- [Triggerlar - Avtomatlashtirish sirlari](#-triggerlar---avtomatlashtirish-sirlari)
- [Loop (Sikl)lar bilan ishlash](#-loop-sikllar-bilan-ishlash)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz oddiy SQL so'rovlaridan chiqib, ma'lumotlar bazasi ichida haqiqiy dasturlashni o'rganasiz. Biz bazaning o'zida mantiqiy algoritmlar yozishni, hisob-kitoblarni avtomatlashtirishni va ma'lumotlar o'zgarganda avtomatik ishga tushuvchi "qopqonlar" (triggerlar) qo'yishni ko'rib chiqamiz.

---

## ⚖️ FUNKSIYALAR VA PROSEDURALAR FARQI

PostgreSQLda ikkala obyekt ham kodni qayta ishlatish uchun xizmat qiladi, lekin ularning fundamental farqlari bor:

| Xususiyat | Function (Funksiya) | Procedure (Prosedura) |
|-----------|----------------------|-----------------------|
| **Natija** | Doim qiymat qaytarishi shart (`RETURN`) | Qiymat qaytarmaydi |
| **Tranzaksiya** | `COMMIT`/`ROLLBACK` ishlatib bo'lmaydi | Tranzaksiyalarni boshqara oladi |
| **Chaqirish** | `SELECT function_name()` | `CALL procedure_name()` |
| **Maqsad** | Hisob-kitob qilish, ma'lumot qaytarish | Biznes mantiqni bajarish, batch o'zgarishlar |

---

## 🧠 PL/pgSQL ASOSLARI

**PL/pgSQL** — bu PostgreSQL uchun maxsus prosedural til. U yordamida biz o'zgaruvchilar, `IF/ELSE` shartlari va `LOOP` sikllaridan foydalana olamiz.

### 1️⃣ O'zgaruvchilar (Variables)
Kod ichida ma'lumotlarni saqlash uchun ishlatiladi.

```sql
DO $$ 
DECLARE
    ism TEXT := 'Ali';
    yosh INTEGER := 25;
BEGIN
    RAISE NOTICE 'Salom, %! Siz % yoshdasiz.', ism, yosh;
END $$;
```

### 2️⃣ Shartli operatorlar (IF/ELSE)

```sql
DO $$
DECLARE
    balance NUMERIC := 5000;
BEGIN
    IF balance > 10000 THEN
        RAISE NOTICE 'Siz Premium mijozsiz';
    ELSIF balance > 0 THEN
        RAISE NOTICE 'Siz Oddiy mijozsiz';
    ELSE
        RAISE NOTICE 'Balansni to''ldiring';
    END IF;
END $$;
```

---

## 🛠️ CUSTOM FUNKSIYA YARATISH

### 📌 Misol: Chegirmani hisoblash funksiyasi
Tasavvur qiling, har safar chegirma narxini qo'lda hisoblamaslik uchun funksiya yozamiz.

```sql
CREATE OR REPLACE FUNCTION get_discounted_price(price NUMERIC, discount_pct NUMERIC)
RETURNS NUMERIC AS $$
BEGIN
    RETURN price - (price * discount_pct / 100);
END;
$$ LANGUAGE plpgsql;

-- Ishlatish:
SELECT nom, narx, get_discounted_price(narx, 15) AS yangi_narx 
FROM mahsulotlar;
```

---

## ⚡ STORED PROCEDURES (PROSEDURALAR)

Proseduralar asosan ma'lumotlarni o'zgartirish (UPDATE, DELETE, INSERT) uchun ishlatiladi.

### 📌 Misol: Bank o'tkazmasi prosedurasini yozish
Bu prosedura bir vaqtning o'zida ikkita UPDATE bajaradi va xavfsizlikni ta'minlaydi.

```sql
CREATE OR REPLACE PROCEDURE transfer_money(
    sender_id INT, 
    receiver_id INT, 
    amount NUMERIC
)
AS $$
BEGIN
    -- Yuboruvchidan ayirish
    UPDATE accounts SET balance = balance - amount WHERE id = sender_id;
    
    -- Qabul qiluvchiga qo'shish
    UPDATE accounts SET balance = balance + amount WHERE id = receiver_id;
    
    COMMIT; -- Saqlash
END;
$$ LANGUAGE plpgsql;

-- Ishlatish:
CALL transfer_money(1, 2, 50000);
```

---

## 🪤 TRIGGERLAR - AVTOMATLASHTIRISH SIRLARI

**Trigger** — bu biror voqea (INSERT, UPDATE, DELETE) sodir bo'lganda avtomatik ravishda ishga tushuvchi mexanizm.

### 📌 Misol: O'zgarishlar tarixini (Log) saqlash
Mijozning balansi o'zgarganda avtomatik ravishda `audit_logs` jadvaliga yozib boramiz.

**1. Avval trigger funksiyasini yaratamiz:**
```sql
CREATE OR REPLACE FUNCTION log_balance_changes()
RETURNS TRIGGER AS $$
BEGIN
    IF OLD.balance <> NEW.balance THEN
        INSERT INTO audit_logs(user_id, old_val, new_val, changed_at)
        VALUES (OLD.id, OLD.balance, NEW.balance, NOW());
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

**2. Triggerni jadvalga ulaymiz:**
```sql
CREATE TRIGGER trg_balance_update
AFTER UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION log_balance_changes();
```

---

## 🎓 AMALIY MASHG'ULOT

### 📊 TEST MA'LUMOTLAR: "BANK_ACCOUNTS"

Ushbu amaliyot uchun kattaroq ma'lumotlar bazasi bilan ishlaymiz.

**Accounts (Hisoblar):**
```
┌────┬────────────┬──────────────┬──────────────┬──────────┬─────────────┐
│ id │ ism        │ hisob_turi   │ balans       │ status   │ oxirgi_faol │
├────┼────────────┼──────────────┼──────────────┼──────────┼─────────────┤
│ 1  │ Olimjon    │ Depozit      │ 15,000,000   │ Active   │ 2024-01-20  │
│ 2  │ Nigora     │ Kredit       │ -2,500,000   │ Warning  │ 2024-01-22  │
│ 3  │ Jasur      │ Jamg'arma    │ 500,000      │ Active   │ 2024-01-15  │
│ 4  │ Malika     │ Depozit      │ 0            │ Blocked  │ 2023-12-01  │
│ 5  │ Sardor     │ Jamg'arma    │ 7,800,000    │ Active   │ 2024-01-21  │
│ 6  │ Zarina     │ Biznes       │ 45,000,000   │ Active   │ 2024-01-22  │
│ 7  │ Abbos      │ Kredit       │ -150,000     │ Active   │ 2024-01-10  │
│ 8  │ Dilnoza    │ Depozit      │ 1,200,000    │ Inactive │ 2023-11-15  │
│ 9  │ Bekzod     │ Biznes       │ 120,000,000  │ Active   │ 2024-01-22  │
│ 10 │ Feruza     │ Jamg'arma    │ 300,000      │ Warning  │ 2024-01-05  │
│ 11 │ Umidbek    │ Depozit      │ 2,500,000    │ Active   │ 2024-01-18  │
│ 12 │ Shahlo     │ Jamg'arma    │ 900,000      │ Active   │ 2024-01-20  │
└────┴────────────┴──────────────┴──────────────┴──────────┴─────────────┘
```

---

### ✏️ Topshiriqlar

#### 1️⃣ Funksiyalar (Hisob-kitob)
- Foydalanuvchining balansiga qarab ularning "Soliq" (Tax) miqdorini hisoblovchi funksiya yozing (masalan, 1%).
- Balans manfiy (minus) bo'lsa, soliqni 0 qaytarsin.

#### 2️⃣ Shartli Funksiya (IF/ELSE)
- `check_status(balans)` nomli funksiya yarating.
  - Agar balans > 10 mln bo'lsa -> 'VIP'
  - Agar balans 0 dan 10 mln gacha bo'lsa -> 'Normal'
  - Agar balans < 0 bo'lsa -> 'Debtor' qaytarsin.

#### 3️⃣ Stored Procedure (O'zgartirish)
- `deactivate_old_accounts()` nomli prosedura yarating. U oxirgi marta 6 oydan ko'p vaqt oldin faol bo'lgan barcha hisoblarning statusini 'Inactive' ga o'zgartirsin.

#### 4️⃣ Trigger (Avtomatika)
- Agar kimdir balansini UPDATE qilsa va yangi balans 0 dan kichik bo'lib ketsa, avtomatik ravishda statusni 'Warning' ga o'zgartiruvchi trigger yarating.

#### 5️⃣ Loop (Sikl) - Tasavvur qiling
- Barcha `Active` foydalanuvchilarga yil yakuni uchun 5% bonus qo'shib chiquvchi prosedura yozing (Loop ishlatishingiz mumkin yoki bitta UPDATE buyrug'i bilan).

#### 6️⃣ Murakkab mantiq
- Pul o'tkazish prosedurasini (`transfer_money`) shunday yozingki, u yuboruvchining balansi yetarli ekanini tekshirsin. Agar balans yetarli bo'lmasa, xatolik chiqarsin (`RAISE EXCEPTION`).

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:

- [x] Funksiya (Returns) va Prosedura (Call) farqini
- [x] PL/pgSQL tilida dasturlash asoslarini
- [x] Triggerlar yordamida bazani avtopilotga qo'yishni
- [x] Tranzaksiyalar mantiqiy xavfsizligini (Commit/Rollback)
- [x] Ma'lumotlarni tekshirish va xatoliklarni boshqarishni

### 📚 Keyingi darsda:

**09-DARS: Foydalanuvchilarni Boshqarish va Xavfsizlik (RBAC)**
- User va Role yaratish  
- GRANT va REVOKE (Huquqlar berish)  
- Schema darajasida xavfsizlik  
- Parollarni boshqarish va SSL ulanishlar

---

## 📖 QO'SHIMCHA MASLAHATLAR

1. **Naming:** Funksiyalarni `get_...`, proseduralarni esa `update_...` yoki `process_...` deb nomlash yaxshi odat.
2. **Deterministic:** Agar funksiya bir xil kirish parametriga doim bir xil natija qaytarsa, uni `IMMUTABLE` deb belgilash tezlikni oshiradi.
3. **Security:** `SECURITY DEFINER` va `SECURITY INVOKER` farqini biling (Funksiya kimning huquqi bilan ishlashi).
4. **Triggerlar:** Triggerlarni ko'p ishlatish bazani murakkablashtiradi va debuggingni qiyinlashtiradi. Faqat kerakli joyda ishlating.