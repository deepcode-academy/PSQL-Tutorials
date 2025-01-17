# Connect PostgreSQL to Python

## Connecting to the PostgreSQL Server

```python
import psycopg2

try:
    # PostgreSQL serveriga ulanish
    connection = psycopg2.connect(
        database="testdb",        # Yaratilgan ma'lumotlar bazasi nomi
        user="postgres",          # PostgreSQL foydalanuvchi nomi
        password="12345",         # PostgreSQL foydalanuvchi paroli
        host="localhost",         # Server manzili (localhost bu kompyuteringiz)
        port="5432"               # PostgreSQL port raqami (standart: 5432)
    )
    print("PostgreSQL ga muvaffaqiyatli ulandik!")

except Exception as error:
    # Agar ulanishda xatolik yuz bersa, xabar chiqariladi
    print("Ulanishda xato yuz berdi:", error)
finally:
    if connection:
        # Har qanday holatda ulanish yopiladi
        connection.close()
        print("Ulanish yopildi.")
```

- `psycopg2.connect`: Ushbu funksiya PostgreSQL serveriga ulanishni amalga oshiradi. U `database`, `user`, `password`, `host`, va `port` ma'lumotlarini qabul qiladi.
- `try-except`: Agar ulanishda xatolik yuz bersa, foydalanuvchi xato haqida xabar oladi.
- `finally`: Har doim bajariladi. Bu yerda ulanish (connection) yopiladi.

## Creating a PostgreSQL Table

```python
import psycopg2

try:
    connection = psycopg2.connect(
        database="testdb",
        user="postgres",
        password="12345",
        host="localhost",
        port="5432"
    )
    cursor = connection.cursor()  # SQL buyruqlarini bajarish uchun kursor yaratish

    # Jadval yaratish SQL so'rovi
    create_table_query = """
    CREATE TABLE users (
        id SERIAL PRIMARY KEY,   # Avtomatik o'suvchi `id` ustuni (asosiy kalit)
        name VARCHAR(100),       # 100 belgigacha bo'lgan matnli `name` ustuni
        age INT                  # Butun sonlarni qabul qiluvchi `age` ustuni
    );
    """
    cursor.execute(create_table_query)  # SQL so'rovini bajarish
    connection.commit()  # Jadvalni yaratishni tasdiqlash
    print("Jadval muvaffaqiyatli yaratildi.")

except Exception as error:
    print("Xato yuz berdi:", error)

finally:
    if connection:
        cursor.close()  # Kursorni yopish
        connection.close()  # Ulanishni yopish
        print("Ulanish yopildi.")
```
- `cursor = connection.cursor()`: cursor obyekti orqali SQL buyruqlari bajariladi.
- `CREATE TABLE`: SQL buyrug'i orqali `users` jadvali yaratiladi.
  - `id SERIAL PRIMARY KEY`: `id` ustuni asosiy kalit bo'lib, avtomatik raqamlanadi.
  - `name VARCHAR(100)`: `name` ustuni matn saqlaydi (100 ta belgigacha).
  - `age INT`: `age` ustuni butun son saqlaydi.
- `cursor.execute`: SQL buyrug'ini bajaradi.
- `connection.commit()`: O'zgarishlarni tasdiqlaydi (ma'lumotlar bazasiga saqlaydi).

## Inserting Data

```python
import psycopg2

try:
    connection = psycopg2.connect(
        database="testdb",
        user="postgres",
        password="12345",
        host="localhost",
        port="5432"
    )
    cursor = connection.cursor()

    # Ma'lumot qo'shish
    insert_query = """
    INSERT INTO users (name, age) VALUES ('Ali', 25), ('Vali', 30);
    """
    cursor.execute(insert_query)  # SQL so'rovini bajarish
    connection.commit()  # Kiritilgan ma'lumotlarni saqlash
    print("Ma'lumotlar muvaffaqiyatli qo'shildi.")

except Exception as error:
    print("Xato yuz berdi:", error)

finally:
    if connection:
        cursor.close()
        connection.close()
        print("Ulanish yopildi.")
```

- `INSERT INTO`: Jadvalga yangi ma'lumotlarni qo'shish.
  - `users (name, age)`: Bu yerda `users` jadvaliga `name` va `age` qiymatlari kiritilmoqda.
  - `('Ali', 25), ('Vali', 30)`: Har bir qator uchun nom va yosh kiritilmoqda.
- `cursor.execute`: Kursor SQL so'rovini bajaradi.
- `connection.commit()`: Kiritilgan ma'lumotlarni tasdiqlaydi.

## Reading Data

```python
import psycopg2

try:
    connection = psycopg2.connect(
        database="testdb",
        user="postgres",
        password="12345",
        host="localhost",
        port="5432"
    )
    cursor = connection.cursor()

    # Ma'lumotlarni o'qish
    select_query = "SELECT * FROM users;"
    cursor.execute(select_query)  # SQL so'rovini bajarish
    records = cursor.fetchall()  # Barcha natijalarni olish

    print("Ma'lumotlar:")
    for row in records:  # Har bir qatorni chop etish
        print(f"ID: {row[0]}, Name: {row[1]}, Age: {row[2]}")

except Exception as error:
    print("Xato yuz berdi:", error)

finally:
    if connection:
        cursor.close()
        connection.close()
        print("Ulanish yopildi.")
```

- `SELECT * FROM users;`: `users` jadvalidan barcha ustunlarni o'qish.
- `cursor.fetchall()`: Barcha qatorlarni ro'yxat sifatida qaytaradi.
- `for row in records`: Har bir qatorni row orqali ko'rib chiqadi.
- `row[0], row[1], row[2]`: Jadvalning `id`, `name`, va `age` ustunlari qiymatlarini aks ettiradi.

## Updating Data

```python
import psycopg2

try:
    connection = psycopg2.connect(
        database="testdb",
        user="postgres",
        password="12345",
        host="localhost",
        port="5432"
    )
    cursor = connection.cursor()

    # Ma'lumotni yangilash
    update_query = "UPDATE users SET age = 26 WHERE name = 'Ali';"
    cursor.execute(update_query)  # SQL buyrug'ini bajarish
    connection.commit()  # O'zgarishlarni saqlash
    print("Ma'lumot muvaffaqiyatli yangilandi.")

except Exception as error:
    print("Xato yuz berdi:", error)

finally:
    if connection:
        cursor.close()
        connection.close()
        print("Ulanish yopildi.")
```

- `UPDATE users`: `users` jadvalidagi ma'lumotlarni yangilash.
  - `SET age = 26`: `age` ustunini `26` ga o'zgartirish.
  - `WHERE name = 'Ali'`: Faqat `name` ustunidagi `Ali` bo'lgan qatorni yangilash.
- `connection.commit()`: Yangilanishni tasdiqlash.

## Deleting Data

```python
import psycopg2

try:
    connection = psycopg2.connect(
        database="testdb",
        user="postgres",
        password="12345",
        host="localhost",
        port="5432"
    )
    cursor = connection.cursor()

    # Ma'lumotlarni o'chirish
    delete_query = "DELETE FROM users WHERE name = 'Ali';"
    cursor.execute(delete_query)  # SQL buyrug'ini bajarish
    connection.commit()  # O'zgarishlarni tasdiqlash
    print("Ma'lumot muvaffaqiyatli o'chirildi.")

except Exception as error:
    print("Xato yuz berdi:", error)

finally:
    if connection:
        cursor.close()
        connection.close()
        print("Ulanish yopildi.")
```

- `DELETE FROM users`: users jadvalidan ma'lumotlarni o'chirish.
  - `WHERE name = 'Ali'`: Faqat `name` ustunida `Ali` bo'lgan qatorni o'chiradi.
- `connection.commit()`: O'chirishni tasdiqlaydi.