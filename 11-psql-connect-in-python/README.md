# 🐘 11-DARS: PYTHON VA POSTGRESQL INTEGRATSIYASI

## 📋 MAVZU REJASI

- [Python va PostgreSQL - Nega birgalikda?](#-python-va-postgresql---nega-birgalikda)
- [psycopg2 kutubxonasi bilan ishlash](#-psycopg2-kutubxonasi-bilan-ishlash)
- [Ulanish boshqaruvi (Connection Management)](#-ulanish-boshqaruvi)
- [CRUD amallar (Create, Read, Update, Delete)](#-crud-amallar)
- [SQL Injection-dan qanday himoyalanish](#-sql-injection-dan-himoyalanish)
- [ORM vs Raw SQL (Qachon qaysi birini ishlatish)](#-orm-vs-raw-sql)
- [Amaliy loyiha: Oddiy API yaratish](#-amaliy-loyiha)

---

## 🎯 DARS MAQSADI

Ushbu darsda siz PostgreSQL bilimlaringizni Python dasturlash bilan birlashtirasiz. Backend ilovalar yaratish uchun bazaga ulanish, ma'lumotlarni olish va xavfsiz ishlashni o'rganasiz. Bu dars sizni real dunyo web development'ga tayyorlaydi.

---

## 🤝 PYTHON VA POSTGRESQL - NEGA BIRGALIKDA?

**PostgreSQL** — kuchli ma'lumot bazasi.  
**Python** — oddiy va kuchli dasturlash tili.

Ularni birlashtirib, siz:
- 📊 Web API'lar yasashingiz mumkin (FastAPI, Flask, Django).
- 🤖 Ma'lumotlarni tahlil qiluvchi dasturlar yozishingiz mumkin.
- 🏢 Korporativ tizimlar (ERP, CRM) yaratasizингиз.

---

## 📦 PSYCOPG2 KUTUBXONASI BILAN ISHLASH

### O'rnatish

```bash
pip install psycopg2-binary
```

### Birinchi ulanish

```python
import psycopg2

# PostgreSQL bazasiga ulanish
conn = psycopg2.connect(
    database="kompaniya_db",
    user="postgres",
    password="parol123",
    host="localhost",
    port="5432"
)

print("✅ Bazaga muvaffaqiyatli ulandi!")
conn.close()
```

---

## 🔌 ULANISH BOSHQARUVI

### ❌ Noto'g'ri usul (Connection Leak)

```python
conn = psycopg2.connect(...)
cursor = conn.cursor()
cursor.execute("SELECT * FROM users")
# ❌ Agar xato yuz bersa, connection hech qachon yopilmaydi!
```

### ✅ To'g'ri usul (Context Manager)

```python
import psycopg2
from psycopg2 import OperationalError

try:
    with psycopg2.connect(
        database="kompaniya_db",
        user="postgres",
        password="parol123",
        host="localhost",
        port="5432"
    ) as conn:
        with conn.cursor() as cursor:
            cursor.execute("SELECT * FROM xodimlar")
            print(cursor.fetchall())
except OperationalError as e:
    print(f"❌ Ulanish xatosi: {e}")
```

---

## ✏️ CRUD AMALLAR

### 📊 Amaliy jadval

Quyidagi `mahsulotlar` jadvali bilan ishlaymiz:

```
┌────┬─────────────┬───────────┬──────────┐
│ id │ nomi        │ narx      │ soni     │
├────┼─────────────┼───────────┼──────────┤
│ 1  │ Laptop      │ 5,000     │ 10       │
│ 2  │ Mouse       │ 50        │ 200      │
└────┴─────────────┴───────────┴──────────┘
```

### 1️⃣ CREATE - Ma'lumot qo'shish

```python
import psycopg2

conn = psycopg2.connect(database="darslik", user="postgres", password="1234")
cursor = conn.cursor()

# ⚠️ Xavfli (SQL Injection ga o'chiq):
# nomi = input("Mahsulot nomi: ")
# cursor.execute(f"INSERT INTO mahsulotlar (nomi, narx) VALUES ('{nomi}', 100)")

# ✅ Xavfsiz usul (Parameterized Query):
cursor.execute(
    "INSERT INTO mahsulotlar (nomi, narx, soni) VALUES (%s, %s, %s)",
    ("Keyboard", 120, 50)
)
conn.commit()
print("✅ Mahsulot qo'shildi!")

cursor.close()
conn.close()
```

### 2️⃣ READ - Ma'lumotlarni o'qish

```python
cursor.execute("SELECT * FROM mahsulotlar WHERE narx > %s", (100,))
mahsulotlar = cursor.fetchall()

for row in mahsulotlar:
    print(f"ID: {row[0]}, Nomi: {row[1]}, Narx: {row[2]}")
```

### 3️⃣ UPDATE - Yangilash

```python
yangi_narx = 5500
mahsulot_id = 1

cursor.execute(
    "UPDATE mahsulotlar SET narx = %s WHERE id = %s",
    (yangi_narx, mahsulot_id)
)
conn.commit()
print(f"✅ Mahsulot {mahsulot_id} yangilandi!")
```

### 4️⃣ DELETE - O'chirish

```python
cursor.execute("DELETE FROM mahsulotlar WHERE soni = 0")
conn.commit()
print(f"✅ {cursor.rowcount} ta mahsulot o'chirildi")
```

---

## 🛡️ SQL INJECTION-DAN HIMOYALANISH

### ❌ Xavfli kod:

```python
ism = input("Ism kiriting: ")
cursor.execute(f"SELECT * FROM users WHERE ism = '{ism}'")
```

Agar foydalanuvchi `Ali' OR '1'='1` deb kiritsa, barcha foydalanuvchilar ma'lumotlari chiqib ketadi!

### ✅ Xavfsiz yechim:

```python
ism = input("Ism kiriting: ")
cursor.execute("SELECT * FROM users WHERE ism = %s", (ism,))
```

**Qoida:** Hech qachon f-string yoki `+` bilan SQL yozmang. Doim `%s` placeholder ishlan!

---

## 🎓 AMALIY LOYIHA: ODDIY TODO API

### Vazifa: `tasks` jadvali bilan ishlash

```python
import psycopg2

def create_task(title, description):
    """Yangi task yaratish"""
    with psycopg2.connect(database="todo_db", user="postgres", password="1234") as conn:
        with conn.cursor() as cursor:
            cursor.execute(
                "INSERT INTO tasks (title, description, completed) VALUES (%s, %s, false) RETURNING id",
                (title, description)
            )
            task_id = cursor.fetchone()[0]
            conn.commit()
            return task_id

def get_all_tasks():
    """Barcha tasklarni olish"""
    with psycopg2.connect(database="todo_db", user="postgres", password="1234") as conn:
        with conn.cursor() as cursor:
            cursor.execute("SELECT id, title, completed FROM tasks")
            return cursor.fetchall()

def mark_completed(task_id):
    """Taskni bajarilgan deb belgilash"""
    with psycopg2.connect(database="todo_db", user="postgres", password="1234") as conn:
        with conn.cursor() as cursor:
            cursor.execute("UPDATE tasks SET completed = true WHERE id = %s", (task_id,))
            conn.commit()

# Ishlatish:
new_id = create_task("PostgreSQL o'rganish", "Barcha darslarni yakunlash")
print(f"Task yaratildi, ID: {new_id}")

barcha = get_all_tasks()
for task in barcha:
    status = "✅" if task[2] else "⏳"
    print(f"{status} {task[1]}")
```

---

## 📚 ORM VS RAW SQL

| Xususiyat | Raw SQL (psycopg2) | ORM (SQLAlchemy, Django ORM) |
|-----------|---------------------|------------------------------|
| **Tezlik** | Tezroq | Biroz sekinroq |
| **Yozish** | Ko'p kod | Qisqa kod |
| **Xavfsizlik** | Qo'lda boshqarish kerak | Avtomatik himoyalangan |
| **Murakkab so'rovlar** | Oson yoziladi | Ba'zan qiyin |

**Maslahat:** Oddiy CRUD uchun ORM, murakkab analytics uchun Raw SQL ishlatish maqul.

---

## 🎯 DARS YAKUNLARI

### ✅ Siz o'rgandingiz:
- [x] Python orqali PostgreSQL-ga ulanishni
- [x] Context Manager bilan xavfsiz ishlashni
- [x] CRUD amallarini Python'da bajarishni
- [x] SQL Injection xavfidan himoyalanishni
- [x] Real loyihada ishlatish uchun amaliy misollar

### 📚 Keyingi bosqich:
- **FastAPI** yoki **Django** frameworklari bilan ishlashni o'rganing.
- Production uchun **Connection Pooling** (Ulanish havzasi) texnikasini o'rganing.
- **Docker** bilan PostgreSQL va Python ilovangizni konteynerga o'rnating.

---

## 💡 PROFESSIONAL MASLAHATLAR

1. **Environment Variables:** Parol va ulanish ma'lumotlarini kodda yozmang, `.env` faylida saqlang.
2. **Connection Pooling:** Har safar yangi ulanish ochmasdan, ulanishlar havzasidan foydalaning (`psycopg2.pool`).
3. **Logging:** Barcha bazaga murojat va xatoliklarni log'ga yozib boring.
4. **Async:** Katta yuklamalar uchun `asyncpg` kutubxonasini o'rganing (async/await bilan).

### .env fayl misoli:
```
DB_NAME=kompaniya_db
DB_USER=postgres
DB_PASSWORD=secretpass
DB_HOST=localhost
DB_PORT=5432
```

### Python kodda:
```python
import os
from dotenv import load_dotenv

load_dotenv()

conn = psycopg2.connect(
    database=os.getenv("DB_NAME"),
    user=os.getenv("DB_USER"),
    password=os.getenv("DB_PASSWORD"),
    host=os.getenv("DB_HOST"),
    port=os.getenv("DB_PORT")
)
```

**Tabriklaymiz! Siz endi PostgreSQL + Python ustasiz!** 🎊