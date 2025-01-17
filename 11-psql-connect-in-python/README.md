# Connect PostgreSQL to Python

1. PostgreSQL serveriga ulanish

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