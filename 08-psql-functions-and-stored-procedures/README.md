# PostgreSQL Functions and Stored Procedures

- Topics:
  - Writing functions and stored procedures in PostgreSQL
  - `PL/pgSQL` basics (PostgreSQL procedural language)
  - Working with `parameters`, `loops`, and `conditional statements`

# Writing functions and stored procedures in PostgreSQL

PostgreSQL’da functions va stored procedures ma'lumotlarni qayta ishlash va qayta-qayta foydalanish mumkin bo'lgan kodlarni yozish uchun ishlatiladi.

## Writing Functions

PostgreSQL funksiyalari odatda `PL/pgSQL` tilida yoziladi. Ular aniq vazifalarni bajaradi va qiymat qaytaradi. 

## Syntax for Creating a Function

```sql
CREATE OR REPLACE FUNCTION function_name(parameter_name data_type)
RETURNS return_type AS $$
BEGIN
    -- Function logic
    RETURN return_value;
END;
$$ LANGUAGE plpgsql;
```

1. `CREATE OR REPLACE FUNCTION function_name(parameter_name data_type)`
   - `CREATE OR REPLACE FUNCTION` - Funksiya yaratadi yoki mavjud bo'lsa yangilaydi.
   - `function_name` - Funksiyaning nomi.
   - `(parameter_name data_type)` - Funksiyaga kiritiladigan parametr va ularning malumot turi.
2. `RETURNS return_type` - Funksiya natija sifatida malumot turini qaytarishini bildiradi.
3. `AS $$ ... $$` - Funksiyaning asosiy kodini o'z ichiga oladi.
4. `BEGIN ... END;` - Funksiya ichida bajariladigan kodlar shu yerda yoziladi.
5. `RETURN return_value;` - Parametrlardan foydalangan holda natijani qaytaradi.
6. `LANGUAGE plpgsql;` - Ushbu funksiya `PL/pgSQL` tilida yozilganligini bildiradi.

## Simple Example

```sql

```