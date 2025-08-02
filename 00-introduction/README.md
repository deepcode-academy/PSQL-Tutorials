# 🐘 Ma'lumotlar bazasiga kirish

## Ma'lumotlar bazasi (Database) nima?

**Ma'lumotlar bazasi** — bu ma'lumotlarni tartibli va tizimli tarzda saqlash, ularni tez va oson qidirish, o‘zgartirish hamda boshqarish uchun mo‘ljallangan tizimdir. Zamonaviy dasturlarda ma'lumotlar bazasini boshqarish uchun maxsus dasturlar — Database Management Systems (**DBMS**) ishlatiladi.

---

## Ma'lumotlar bazasi turlari

> **Eslatma:** Ma'lumotlar bazalari tuzilishi va ma'lumotlarni qanday usulda saqlashiga ko‘ra bir necha ko‘rinishda bo‘ladi.

### 1. Relatsion ma'lumotlar bazasi (Relational Database, RDBMS)

- **Tavsifi:** Ma'lumotlar qatʼiy strukturaga ega bo‘lgan jadvallar (rows va columns) ko‘rinishida saqlanadi. Jadvallar o‘zaro aloqador bo‘lishi mumkin (asosiy kalitlar orqali).
- **Misollar:** `PostgreSQL`, `MySQL`, `Oracle`, `SQL Server`
- **Qo‘llanilishi:** Internet saytlar, bank tizimlari, korxona axborot tizimlari.

**Jadval yaratish misoli (SQL):**
```sql
CREATE TABLE users (
  user_id INT PRIMARY KEY,         -- Foydalanuvchi uchun unikal identifikator
  username VARCHAR(50),            -- Foydalanuvchi nomi
  email VARCHAR(100),              -- Email manzili
  created_at TIMESTAMP             -- Ro‘yxatdan o‘tgan sana
);
```

---

### 2. NoSQL ma'lumotlar bazasi

- **Tavsifi:** NoSQL (Not Only SQL) bazalarida ma'lumotlar qatʼiy jadval ko‘rinishida emas, balki moslashuvchan va katta hajmdagi ma'lumotlarni saqlash uchun turli formatlarda saqlanadi.
- **Turlari:** document, key-value, column-family, graph.
- **Misollar:** `MongoDB` (document), `Redis` (key-value), `Cassandra` (column-family), `Neo4j` (graph)
- **Qo‘llanilishi:** Katta hajmdagi ma'lumotlar, tez o‘zgaruvchan ma'lumotlar, real-time ilovalar.

---

### 3. Hujjatli ma'lumotlar bazasi (Document-Based Database)

- **Tavsifi:** Ma'lumotlar JSON yoki BSON kabi hujjatlar ko‘rinishida saqlanadi. Har bir hujjat mustaqil ma'lumot birligi bo‘lib, ichki strukturasi murakkab bo‘lishi mumkin.
- **Misollar:** `MongoDB`, `CouchDB`
- **Qo‘llanilishi:** Foydalanuvchi profillari, kontent boshqarish tizimlari.

**MongoDB hujjat misoli:**
```json
{
  "user_id": 1,                  // Foydalanuvchi identifikatori
  "username": "john_doe",        // Foydalanuvchi nomi
  "email": "john@example.com",   // Email manzili
  "created_at": "2025-06-29T21:00:00Z" // Ro‘yxatdan o‘tgan vaqt
}
```

**MongoDB'ga hujjat qo‘shish:**
```javascript
db.users.insertOne({
  user_id: 1,                        // Foydalanuvchi identifikatori
  username: "john_doe",              // Foydalanuvchi nomi
  email: "john@example.com",         // Email manzili
  created_at: new Date()             // Qo‘shilgan vaqt
});
```

---

### 4. Grafik ma'lumotlar bazasi (Graph Database)

- **Tavsifi:** Grafik bazalarida ma'lumotlar tugunlar (node) va ularning o‘zaro bog‘lanishi (edge) orqali saqlanadi. Murakkab aloqalar va tarmoqlar uchun juda qulay.
- **Misollar:** `Neo4j`, `ArangoDB`
- **Qo‘llanilishi:** Ijtimoiy tarmoqlar, tavsiya tizimlari, firibgarlikni aniqlash.

**Neo4j uchun Cypher so‘rovi:**
```cypher
CREATE (user:User {userId: 1, name: "Alice"})         // Foydalanuvchi tuguni yaratish
CREATE (product:Product {productId: 100, name: "Book"}) // Mahsulot tuguni yaratish
CREATE (user)-[:PURCHASED]->(product)                 // Foydalanuvchi va mahsulot o‘rtasida "sotib olgan" bog‘lanishini yaratish
```

---

## Qisqa taqqoslash jadvali

| Ma'lumotlar bazasi turi | Tuzilishi         | Misollar              | Qo‘llanilishi              |
|-------------------------|-------------------|-----------------------|----------------------------|
| Relatsion               | Jadval (SQL)      | MySQL, PostgreSQL     | Korxona, tranzaksiyalar    |
| Hujjatli                | JSON/BSON         | MongoDB, CouchDB      | Kontent, foydalanuvchilar  |
| Kalit-qiymatli          | Key-Value         | Redis, DynamoDB       | Kesh, sessiyalar           |
| Ustunli                 | Column-family     | Cassandra, HBase      | Big data, tahlil           |
| Grafik                  | Node/Edge         | Neo4j, ArangoDB       | Tarmoqlar, tavsiyalar      |

---

## Asosiy atamalar

- **Jadval (Table):** Relatsion bazalarda ma'lumotlarni saqlash uchun asosiy strukturaviy birlik.
- **Hujjat (Document):** Hujjatli bazalarda mustaqil ma'lumot birligi (ko‘pincha JSON ko‘rinishida).
- **Tugun/Bog‘lanish (Node/Edge):** Grafik bazalarda ob'ekt va ular o‘rtasidagi munosabat.
- **Asosiy kalit (Primary Key):** Har bir yozuv uchun unikal identifikator.
- **Tashqi kalit (Foreign Key):** Jadval o‘rtasidagi bog‘liqlikni ifodalovchi maydon.

---

> **Maslahat:**  
> Dastur yoki loyiha uchun ma'lumotlar bazasi tanlashda, sizda qanday ma'lumotlar va ular ustida qanday amallar bajarilishini inobatga oling.