# 🐘 06-DARS: JADVALLARNI BOG'LASH (JOINS) VA MURAKKAB SO'ROVLAR

## 📋 MAVZU REJASI

- [JOIN tushunchasi va turlari](#-join-tushunchasi-va-turlari)
- [INNER JOIN - Ichki bog'lanish](#-inner-join---ichki-boglanish)
- [LEFT JOIN - Chap tomonlama bog'lanish](#-left-join---chap-tomonlama-boglanish)
- [RIGHT JOIN - O'ng tomonlama bog'lanish](#-right-join---ong-tomonlama-boglanish)
- [FULL JOIN - To'liq bog'lanish](#-full-join---toliq-boglanish)
- [CROSS JOIN va SELF JOIN](#-cross-join-va-self-join)
- [UNION, INTERSECT, EXCEPT - To'plamlar bilan ishlash](#-union-intersect-except---toplamlar-bilan-ishlash)
- [Subqueries - Ichki so'rovlar](#-subqueries---ichki-sorovlar)
- [Amaliy mashg'ulot](#-amaliy-mashgulot)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz quyidagilarni o'rganasiz:

✅ Jadvallarni Foreign Key orqali bog'lash  
✅ Turli xil JOIN turlarini qachon va qanday ishlatish  
✅ Bir nechta jadvallardan bir vaqtda ma'lumot olish  
✅ Ichma-ich so'rovlar (Subqueries) yozish  
✅ Ma'lumotlar to'plamlarini birlashtirish (`UNION`)  
✅ Real loyihalarda murakkab hisobotlar tayyorlash

---

## 🤝 JOIN TUSHUNCHASI VA TURLARI

### 📌 JOIN nima?

**JOIN** — SQLda ikki yoki undan ortiq jadvallarni ular orasidagi mantiqiy bog'lanish (odatda Foreign Key) asosida birlashtirish uchun ishlatiladi.

Relatsion ma'lumotlar bazasida ma'lumotlar turli jadvallarga bo'lingan bo'ladi (Normalizatsiya). JOIN bizga bu bo'lingan ma'lumotlarni yagona natija ko'rinishida yig'ib beradi.

### 💻 Test Ma'lumotlar

Keling, talabalar va guruhlar misolida ko'ramiz:

```sql
-- Jadvallar
CREATE TABLE guruhlar (
    id SERIAL PRIMARY KEY,
    nomi VARCHAR(50)
);

CREATE TABLE talabalar (
    id SERIAL PRIMARY KEY,
    ism VARCHAR(50),
    guruh_id INTEGER REFERENCES guruhlar(id)
);

-- Ma'lumotlar
INSERT INTO guruhlar (nomi) VALUES ('Matematika'), ('Fizika'), ('Tarix');
INSERT INTO talabalar (ism, guruh_id) VALUES 
    ('Ali', 1),    -- Matematika
    ('Vali', 1),   -- Matematika
    ('Guli', 2),   -- Fizika
    ('Olim', NULL); -- Guruhsiz
```

**Vizual ko'rinish:**

**Guruhlar (groups):**
```
┌────┬────────────┐
│ id │ nomi       │
├────┼────────────┤
│ 1  │ Matematika │
│ 2  │ Fizika     │
│ 3  │ Tarix      │
└────┴────────────┘
```

**Talabalar (students):**
```
┌────┬──────┬──────────┐
│ id │ ism  │ guruh_id │
├────┼──────┼──────────┤
│ 1  │ Ali  │ 1        │
│ 2  │ Vali │ 1        │
│ 3  │ Guli │ 2        │
│ 4  │ Olim │ NULL     │
└────┴──────┴──────────┘
```

---

## 🔗 INNER JOIN - ICHKI BOG'LANISH

### 📌 INNER JOIN nima?

**INNER JOIN** — faqat ikkala jadvalda ham mos keladigan (bog'langan) qatorlarni qaytaradi. Agar talabaning guruhi bo'lmasa yoki guruhda talaba bo'lmasa, u natijada chiqmaydi.

```sql
SELECT talabalar.ism, guruhlar.nomi
FROM talabalar
INNER JOIN guruhlar ON talabalar.guruh_id = guruhlar.id;
```

**Natija:**
```
 ism  |    nomi    
------+------------
 Ali  | Matematika
 Vali | Matematika
 Guli | Fizika
```
*(Olim chiqmaydi, chunki uning guruh_id = NULL. Tarix chiqmaydi, chunki unda talaba yo'q)*

---

## 👈 LEFT JOIN - CHAP TOMONLAMA BOG'LANISH

### 📌 LEFT JOIN nima?

**LEFT JOIN** — chap jadvaldagi (birinchi yozilgan) **hamma** qatorlarni qaytaradi. O'ng jadvaldan esa faqat mos keladiganlarini. Agar mos kelmasa, o'ng jadval ustunlari `NULL` bilan to'ladi.

```sql
SELECT talabalar.ism, guruhlar.nomi
FROM talabalar
LEFT JOIN guruhlar ON talabalar.guruh_id = guruhlar.id;
```

**Natija:**
```
 ism  |    nomi    
------+------------
 Ali  | Matematika
 Vali | Matematika
 Guli | Fizika
 Olim | NULL        <-- Mana farqi!
```

---

## 👉 RIGHT JOIN - O'NG TOMONLAMA BOG'LANISH

### 📌 RIGHT JOIN nima?

**RIGHT JOIN** — o'ng jadvaldagi (ikkinchi yozilgan) **hamma** qatorlarni qaytaradi. Chap jadvaldan esa faqat mos keladiganlarini.

```sql
SELECT talabalar.ism, guruhlar.nomi
FROM talabalar
RIGHT JOIN guruhlar ON talabalar.guruh_id = guruhlar.id;
```

**Natija:**
```
 ism  |    nomi    
------+------------
 Ali  | Matematika
 Vali | Matematika
 Guli | Fizika
 NULL | Tarix       <-- Talabasi yo'q guruh ham chiqdi
```

---

## 🔄 FULL JOIN - TO'LIQ BOG'LANISH

### 📌 FULL JOIN nima?

**FULL JOIN** — ikkala jadvaldagi barcha qatorlarni qaytaradi. Mos kelmagan joylarga `NULL` qo'yiladi.

```sql
SELECT talabalar.ism, guruhlar.nomi
FROM talabalar
FULL JOIN guruhlar ON talabalar.guruh_id = guruhlar.id;
```

**Natija:**
```
 ism  |    nomi    
------+------------
 Ali  | Matematika
 Vali | Matematika
 Guli | Fizika
 Olim | NULL
 NULL | Tarix
```

---

## ✖️ CROSS JOIN VA SELF JOIN

### 📌 CROSS JOIN (Dekart ko'paytmasi)
Birinchi jadvaldagi har bir qatorni ikkinchi jadvaldagi barcha qatorlar bilan bog'laydi.

```sql
SELECT talabalar.ism, guruhlar.nomi
FROM talabalar
CROSS JOIN guruhlar;
-- Natija: 4 ta talaba * 3 ta guruh = 12 ta qator
```

### 📌 SELF JOIN
Jadvalni o'zini-o'ziga bog'lash. Odatda iyerarxiya (boshliq va xodim) uchun ishlatiladi.

```sql
SELECT e1.name AS xodim, e2.name AS boshliq
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.id;
```

---

## ➕ UNION, INTERSECT, EXCEPT

### 📌 UNION - Birlashtirish
Ikki yoki undan ortiq SELECT natijalarini bitta to'plamga birlashtiradi (dublikatlarni olib tashlaydi).

```sql
SELECT shahar FROM mijozlar
UNION
SELECT shahar FROM xodimlar;
```

### 📌 INTERSECT - Kesishish
Ikkala so'rovda ham mavjud bo'lgan qatorlarni qaytaradi.

```sql
SELECT ism FROM xodimlar
INTERSECT
SELECT ism FROM mijozlar;
```

### 📌 EXCEPT - Ayirma
Birinchi so'rovda bor, lekin ikkinchisida yo'q qatorlarni qaytaradi.

```sql
SELECT ism FROM xodimlar
EXCEPT
SELECT ism FROM mijozlar;
```

---

## 🕵️ SUBQUERIES - ICHKI SO'ROVLAR

### 📌 Subquery nima?

**Subquery** — boshqa bir SQL so'rovining ichida kelgan so'rov.

#### 1️⃣ WHERE ichida Subquery

```sql
-- O'rtacha maoshdan ko'p oladigan xodimlar
SELECT ism, maosh
FROM xodimlar
WHERE maosh > (SELECT AVG(maosh) FROM xodimlar);
```

#### 2️⃣ SELECT ichida Subquery

```sql
-- Har bir xodim ismi va uning bo'limidagi xodimlar soni
SELECT 
    ism, 
    (SELECT COUNT(*) FROM xodimlar x2 WHERE x2.bo'lim = x1.bo'lim) AS bo'lim_xodimlari
FROM xodimlar x1;
```

---

## 🎓 AMALIY MASHG'ULOT

### 📊 TEST MA'LUMOTLAR

Ushbu amaliyot uchun **Kutubxona** tizimi ma'lumotlaridan foydalanamiz.

#### 📚 Kitoblar (books)
```
┌────┬──────────────────────┬─────────────┬──────────┐
│ id │ nom                  │ muallif_id  │ narx     │
├────┼──────────────────────┼─────────────┼──────────┤
│ 1  │ O'tkan Kunlar        │ 1           │ 45,000   │
│ 2  │ Mehrobdan Chayon     │ 1           │ 40,000   │
│ 3  │ Shaxmat sirlari      │ 2           │ 35,000   │
│ 4  │ Fizika asoslari      │ NULL        │ 50,000   │
└────┴──────────────────────┴─────────────┴──────────┘
```

#### ✍️ Mualliflar (authors)
```
┌────┬─────────────────────┬──────────────┐
│ id │ ism                 │ davlat       │
├────┼─────────────────────┼──────────────┤
│ 1  │ Abdulla Qodiriy     │ O'zbekiston  │
│ 2  │ Garry Kasparov      │ Rossiya      │
│ 3  │ Stephen King        │ AQSH         │
└────┴─────────────────────┴──────────────┘
```

#### 🛒 Sotuvlar (sales)
```
┌────┬──────────┬────────┬────────────┐
│ id │ kitob_id │ miqdor │ sana       │
├────┼──────────┼────────┼────────────┤
│ 1  │ 1        │ 5      │ 2024-01-10 │
│ 2  │ 1        │ 2      │ 2024-01-12 │
│ 3  │ 3        │ 10     │ 2024-01-15 │
└────┴──────────┴────────┼────────────┘
```

---

### ✏️ Topshiriqlar

#### 1️⃣ INNER JOIN - Asosiy
- Barcha kitoblarning nomi va ularning mualliflari ismini chiqaring. (Faqat muallifi bor kitoblar)
- Sotilgan barcha kitoblarning nomi va sotilish miqdorini ko'rsating.

#### 2️⃣ LEFT JOIN va NULL bilan ishlash
- Barcha kitoblar ro'yxatini va ularning mualliflarini chiqaring. (Muallifi bo'lmagan kitoblar ham chiqsin - `NULL`)
- Hali birorta ham kitobi sotilmagan mualliflar ismini toping.

#### 3️⃣ RIGHT va FULL JOIN
- Barcha mualliflar va ularning kitoblarini chiqaring (Hali kitobi yo'q mualliflar ham chiqsin).
- Barcha kitoblar va barcha mualliflarning to'liq ro'yxatini chiqaring (FULL JOIN).

#### 4️⃣ Ko'p jadvalli JOIN (3 ta jadval)
- Sotilgan har bir kitobning nomi, muallifi va sotilgan miqdorini yagona jadvalda ko'rsating.

#### 5️⃣ Agregat funksiyalar bilan JOIN
- Har bir muallifning jami nechta kitobi sotilganini hisoblang.
- Har bir davlat bo'yicha kitoblar sonini aniqlang.

#### 6️⃣ Subqueries - Ichki so'rovlar
- Eng qimmat kitobning muallifi kimligini aniqlang (Subquery bilan).
- O'rtacha sotilish miqdoridan ko'p sotilgan kitoblarni toping.

#### 7️⃣ To'plamlar (UNION, EXCEPT)
- Kitoblar bazasida muallif sifatida ham, xaridor sifatida ham mavjud bo'lgan ismlarni toping (Agar xaridorlar jadvali bo'lsa).
- Birorta ham sotuvda bo'lmagan kitoblarni `EXCEPT` yordamida aniqlang.

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:

- [x] JOIN tushunchasi va turlarini (INNER, LEFT, RIGHT, FULL)
- [x] Jadvallarni Foreign Key orqali bog'lashni
- [x] CROSS va SELF JOIN farqlarini
- [x] To'plamlar bilan ishlashni (UNION, INTERSECT, EXCEPT)
- [x] Ichma-ich so'rovlar (Subqueries) yozishni

### 📚 Keyingi darsda:

**07-DARS: Indekslar va Performance Optimizatsiya**
- Nega so'rovlar sekin ishlaydi?
- B-Tree indekslar qanday ishlaydi?
- Ma'lumot qidirishni 100 barobar tezlashtirish
- `EXPLAIN ANALYZE` bilan so'rovni tahlil qilish

---

## 📖 QO'SHIMCHA MASLAHATLAR

1. **Alias ishlatish:** Jadvallarga qisqa nom berish (masalan, `talabalar t`) kodni o'qishni osonlashtiradi.
2. **JOIN tartibi:** Doimo kichikroq jadvalni birinchi yozishga harakat qiling (bu ba'zi DB'larda tezlikka ta'sir qiladi).
3. **NULL tekshirish:** `LEFT JOIN` ishlatganda o'ng tarafdan kelayotgan `NULL` qiymatlarga tayyor turing.
4. **Subquery vs JOIN:** Agar so'rovni ham JOIN, ham Subquery bilan yozish mumkin bo'lsa, odatda JOIN tezroq ishlaydi.