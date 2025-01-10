# Indexes and Performance Optimization

- Topics:
  - Purpose of indexes and their impact on query performance
  - Creating and managing indexes
  - Types of indexes: B-tree, hash
  - Understanding query plans with `EXPLAIN`

# Purpose of indexes and their impact on query performance

## What is an Index and Its Purpose?

Index bu jadval ustunidagi ma'lumotlarni tezroq qidirish va ularga murojaat qilish uchun ma'lumotlar bazasida yaratiladigan ma'lumotlar tuzilmasidir. Index kitobning oxiridagi alfavitli ko'rsatkichga o'xshaydi — ma'lumotlarni topishni osonlashtiradi.

Main purposes:
1. **Speed up queries**: Indexlar ma'lumotlarni qidirishda jadvalni to'liq o'qib chiqishga ehtiyojni yo'q qiladi.
2. **Optimize resources**: So'rov vaqtida protsessor va xotira resurslaridan samarali foydalanishni ta'minlaydi.
3. **Support sorted searches**: `ORDER BY`, `WHERE`, va `JOIN` shartlarini bajarishda ishlaydi.

# Creating and managing indexes

Indekslar ma'lumotlarni tezkor qidirish va olish uchun ishlatiladi. PostgreSQL indekslarni avtomatik tarzda `PRIMARY KEY` va `UNIQUE` ustunlar uchun yaratadi, ammo qo'shimcha indekslar kerak bo'lganda qo'lda yaratishimiz mumkin.

## Creating an Index

Indeks yaratish uchun `CREATE INDEX` buyruğidan foydalaniladi.

**Syntax:**

```sql
CREATE INDEX index_name
ON table_name (columns);
```

**Example:**

**students** table

| id | name      | age | faculty     | email             |
|----|-----------|-----|-------------|-------------------|
| 1  | Javohir   | 21  | Informatika | javohir@gmail.com |
| 2  | Shahlo    | 20  | Tibbiyot    | shahlo@gmail.com  |
| 3  | Dilshod   | 22  | Fizika      | dilshod@gmail.com |

`name` ustuni bo'yicha index yaratamiz.

```sql
CREATE INDEX my_index
ON students (name);
```

## Viewing Indexes

PostgreSQL indekslarni ko'rish uchun `\d` yoki `pg_indexes` jadvalidan foydalanish mumkin.

```sql
SELECT * FROM pg_indexes WHERE tablename = 'students';
```

## Creating a Unique Index

Unikal indeks bir xil qiymatga ega bo'lgan ma'lumotlar kirishini oldini oladi.

**Syntax:**

```sql
CREATE UNIQUE INDEX index_name
ON table_name (columns);
```

`email` ustuni uchun unikal indeks yaratish:

**Example:**

```sql
CREATE UNIQUE INDEX my_email
ON students (email);
```

## Creating Multi-Column Indexes

Bir nechta ustunlarni o'z ichiga olgan indekslarni yaratish mumkin.

**Syntax:**

```sql
CREATE INDEX index_name
ON table_name (column1, column2);
```

**Example:**

`name` va `age` ustunlari bo'yicha indeks yaratish:

```sql
CREATE INDEX name_age_index
ON students (name, age);
```

## Dropping an Index

Indekslarni o'chirish uchun `DROP INDEX` buyrug'i ishlatiladi.

**Syntax:**

```sql
DROP INDEX index_name;
```

**Example:**

```sql
DROP INDEX name_index;
```

# Types of indexes: `B-tree`, `hash`

## B-tree (Balanced Tree) Index

**B-tree** indeksi, asosan, ma'lumotlar bazalarida keng qo'llaniladi. **B-tree** indeksida ma'lumotlar tasodifiy tarzda joylashtirilmaydi, balki ular alohida tartibda saqlanadi, bu esa qidiruv, qo'shish va o'chirish amallarini bajarishni osonlashtiradi.

- Features:
  - Ma'lumotlar tartiblangan holda saqlanadi.
  - O(log n) murakkablikka ega (qidiruv, qo'shish va o'chirish).
  - Ma'lumotlar bazasida dinamik (ya'ni, yangi ma'lumot qo'shish yoki o'chirishda struktura o'zgaradi, lekin bu jarayon tez).
  - Ma'lumotlar qidiruvining samaradorligini ta'minlash uchun ko'plab darajalar mavjud.

**Example:**

Agar sizda quyidagi ma'lumotlar bo'lsa:

| ID | Name  |
|----|-------|
| 1  | Ali   |
| 2  | Anvar |
| 3  | Oydin |
| 4  | Aziz  |

B-tree indeksi yordamida, ID bo'yicha qidiruv tezligini oshirish uchun quyidagi tarzda tartiblanadi:

```markdown
      3
    /   \
   2     4
  /
 1
```

B-tree indeksi ID bo'yicha tezkor qidiruvni amalga oshirish imkonini beradi, chunki ma'lumotlarni tartiblangan holda saqlaydi.

## Hash Index

Hash indeksi ma'lumotlarni biror-bir "hash" funksiyasi orqali tezkor qidirish uchun ishlatiladi. Har bir indekslanadigan qiymat uchun hash qiymati hisoblanadi va ma'lumotlar shu qiymatga mos joylarda saqlanadi. Hash indeksi ma'lumotlarga kirish vaqtini minimal darajaga tushiradi.

- Features:
  - Tezkor qidiruv imkonini beradi (yangi ma'lumotlarni tezda topish).
  - Hash indeksi faqat oqish (read) operatsiyalari uchun mos, chunki bu indeks qidiruvni osonlashtiradi, ammo oraliq va interval qidiruvlarni qo'llab-quvvatlamaydi.
  - O(1) murakkablikka ega, ya'ni ma'lumotlarni topishning vaqtini aniq belgilash mumkin.

| ID | Name  |
|----|-------|
| 1  | Ali   |
| 2  | Anvar |
| 3  | Oydin |
| 4  | Aziz  |

Hash funksiyasi yordamida `ID` bo'yicha qiymatlar quyidagicha tarqatiladi:

```markdown
Hash(1) = 5
Hash(2) = 3
Hash(3) = 2
Hash(4) = 8
```

Har bir `ID` qiymatiga mos ravishda hash qiymati hisoblanadi va ma'lumotlar shu qiymat bo'yicha saqlanadi. Masalan, `ID=3` bo'yicha qidiruv, hash funksiyasi orqali to'g'ridan-to'g'ri 2 manzilini ko'rsatadi va shu joydan ma'lumot olinadi.

# Understanding query plans with `EXPLAIN`

SQL'da `EXPLAIN` buyrug'i so'rovning bajarilish rejasini ko'rsatadi. Bu, so'rovni optimallashtirish uchun juda muhim, chunki `EXPLAIN` yordamida so'rovning qanday bajarilishini va ma'lumotlar bazasi qanday metodlarni tanlayotganini tushunish mumkin. Bu buyruq bajariladigan so'rovning rejasini chiqaradi, masalan, qanday indekslar ishlatilayotgani, jadvalga qaysi usul bilan murojaat qilinayotgani va boshqalar.

## Using `EXPLAIN`

`EXPLAIN` buyrug'i so'rovning ma'lumotlar bazasida qanday bajarilishini ko'rsatadi.

```sql
EXPLAIN SELECT * FROM students WHERE age > 18;
```

Yuqoridagi buyruq so'rovni bajarishdan oldin ma'lumotlar bazasiga qanday strategiya ishlatish kerakligini tushunishga yordam beradi.

## JOIN with another table

```sql
EXPLAIN SELECT employees.name, departments.name
FROM employees
JOIN departments ON employees.department_id = departments.id
WHERE employees.age > 30;
```

Bu so'rovda ikkita jadval: `employees` va `departments` qo'shilmoqda. `EXPLAIN` yordamida uning bajarilish rejasini ko'rib chiqsak:

- Seq Scan: Bu, ma'lumotlar bazasi jadvalni to'liq tekshiradi. Agar indeks mavjud bo'lmasa, jadvalning har bir qatori tekshiriladi.
- Index Scan: Agar indeks mavjud bo'lsa, `EXPLAIN` indeksni ishlatish haqida ma'lumot beradi.
- Nested Loop: Bu usul birinchi jadvalni tanlab, keyin ikkinchi jadvalni har bir qator uchun tekshiradi. Bu usul kichik jadvalga yaxshi ishlaydi.

## Example of EXPLAIN Output

```shell
Seq Scan on employees  (cost=0.00..1000.00 rows=1000 width=32)
  Filter: (age > 30)
  ->  Index Scan using departments_pkey on departments  (cost=0.00..50.00 rows=10 width=8)
```

- Other Parameters:
  - Cost: Bajarish uchun taxminiy xarajatlar. Bu, ma'lumotlar bazasining so'rovni bajarish uchun qancha resurs sarflayotganini ko'rsatadi.
  - Rows: Qancha qator qaytarilishi taxmin qilinayotganini ko'rsatadi.
  - Width: Qatorning o'rtacha o'lchami (baytlar bilan).

- Optimizing Execution
  - `EXPLAIN` orqali so'rovning qanday ishlashini ko'rib chiqqandan so'ng, kerakli optimallashtirishni amalga oshirish mumkin.
    - Indekslar qo'shish
    - Jadvalni to'liq tekshirish o'rniga indekslarni ishlatishni tanlash
    - `JOIN` usullarini optimallashtirish

# PRACTICS

users table

|id|first_name|last_name|birth_date|gender|phone|address|faculty|year|average_grade|created_at|
|--|----------|---------|----------|------|-----|-------|-------|----|-------------|----------|
|1|John|Doe|2001-07-15

| id | first_name | last_name | Birth Date | Gender | Phone        | Address            | Faculty           | 	Year | Grade | Created At         |
|----|------------|-----------|------------|--------|--------------|--------------------|-------------------|-------|-------|--------------------|
| 1  | John       | Doe       | 2001-07-15 | M      | +1234567890  | New York, USA      | Computer Science  | 3     | 89.50 | (auto-generated)   |
| 2  | Jane       | Smith     | 2002-02-10 | F      | +9876543210  | Los Angeles, USA   | Mathematics       | 2     | 92.10 | (auto-generated)   |
| 3  | Alice      | Johnson   | 2000-11-20 | F      | +1122334455  | Chicago, USA       | Physics           | 4     | 88.90 | (auto-generated)   |
| 4  | Bob        | Williams  | 2003-04-05 | M      | +9988776655  | San Francisco, USA | Engineering       | 1     | 76.40 | (auto-generated)   |
| 5  | Emily      | Brown     | 2001-09-25 | F      | +6677889900  | Seattle, USA       | Biology           | 3     | 91.30 | (auto-generated)   |
| 6  | Michael    | Davis     | 2002-12-18 | M      | +5544332211  | Boston, USA        | History           | 2     | 84.70 | (auto-generated)   |
| 7  | Sophia     | Garcia    | 2001-03-30 | F      | +4433221100  | Miami, USA         | Chemistry         | 4     | 87.20 | (auto-generated)   |
| 8  | Chris      | Martinez  | 2003-06-12 | M      | +3322110044  | Houston, USA       | Economics         | 1     | 79.50 | (auto-generated)   |
| 9  | Olivia     | Rodriguez | 2002-01-22 | F      | +9988002211  | Austin, USA        | Art               | 3     | 93.80 | (auto-generated)   |
| 10 | Ethan      | Wilson    | 2000-08-08 | M      | +1122330055  | Denver, USA        | Law               | 4     | 85.10 | (auto-generated)   |
