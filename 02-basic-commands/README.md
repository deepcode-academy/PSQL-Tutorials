# Basic SQL Commands in PostgreSQL

- Topics:
  - `databases`, `tables`, `rows` va `columns` haqida tushuncha
  - `PostgreSQL`dagi ma'lumot turlari
  - Asosiy `SQL` buyruqlari: `CREATE DATABASE`, `CREATE TABLE`, `DROP`, `INSERT`, `SELECT`
  - Primary keys and constraints

> [!NOTE]
> `Databases`, `tables`, `rows` va `columns` — bu `SQL` va ma'lumotlar bazalari boshqaruvi tizimlarining asosiy tushunchalar hisoblanadi.

# `databases`, `tables`, `rows` VA `columns` HAQIDA TUSHUNCHA

# TYPES OF DATABASES
> [!NOTE]
> `Databases` turli shaklda bo'lishi mumkin va ular qanday turdagi ma'lumotlar bilan ishlashiga qarab tasniflanadi


1. **Relational Database (RDBMS):** Bu eng ko'p ishlatiladigan ma'lumotlar bazasi turi bo'lib, ma'lumotlar jadvallar shaklida saqlanadi. Har bir jadval o'zaro bog'liq bo'lgan ustunlar va qatorlardan tashkil topgan. 
   - **Example:** `PostgreSQL`, `MySQL`, `Oracle`.
2. **NoSQL Database:** NoSQL(Not Only SQL) ma'lumotlar bazalari katta hajmdagi va tezkor o'zgaruvchan ma'lumotlarni saqlash uchun ishlatiladi. NoSQL bazalarida ma'lumotlar jadval shaklida emas, balki boshqa formatlarda saqlanishi mumkin (example, `such as documents`, `key-value pairs`). 
   - **Example:** `MongoDB`, `Cassandra`, `Redis`.
3. **Document-Based Database:** Bunda ma'lumotlar `JSON`, `XML` kabi hujjat formatida saqlanadi, har bir hujjat ma'lumotni o'zida saqlaydi va murakkab tuzilmaga ega bo'lishi mumkin. 
   - **Example:** `MongoDB`.

4. **Graph Database:**
   - Bu ma'lumotlar bazasi tarmoq aloqalari va murakkab bog'lanishlarni ifodalashda qo'llaniladi. Grafik bazalarida ma'lumot tugunlar va bog'lanishlar shaklida saqlanadi. 
   - **Example:** `Neo4j`.

1. Database
   - **Database** — bu bir joyda to'plangan va mantiqiy tartibda saqlangan ma'lumotlarning yig'indisi. Ma'lumotlar bazasi yordamida biz katta hajmdagi ma'lumotlarni `boshqarish`, `saqlash`, `yangilash` va ulardan samarali foydalanish imkoniyatiga ega bo'lamiz. 

   - **Masalan:** Kompaniya o'z mijozlari, mahsulotlari, buyurtmalari haqidagi ma'lumotlarni ma'lumotlar bazasida saqlaydi.

2. Table
   - **Table** — ma'lumotlarni `struktura` shaklida saqlash uchun ma'lumotlar bazasidagi asosiy obyektlardan biri. Har bir jadvalda columns(ustunlar) va rows(qatorlar) mavjud bo'lib, ularning yordamida ma'lumotlarni `tartiblangan` shaklda saqlash va ularni `tezkor topish` mumkin. 

     - **Masalan:** Talabalar jadvali quyidagi ma'lumotlarni o'zida saqlashi mumkin: `talabalar ID` raqami, `ism`, `familiya`, `yosh` va `guruh`.

3. Column
   - **Column** — jadvalda saqlanadigan `ma'lumotlarning turini` belgilaydigan element. Har bir ustun jadvaldagi ma'lumotlarga ma'lum bir tavsif beradi va unda ma'lum turdagi ma'lumotlar (`raqamlar`, `matn`, `sana` va h.k.) saqlanadi. 

   - **Masalan:** `ism`, `familiya` va `yosh` — bu talabalar jadvalidagi ustunlardir.

4. Row
    - **Row** — jadvaldagi ma'lumotlarning bir birlikdagi ko'rinishi bo'lib, har bir qator alohida bir yozuvni ifodalaydi. 
    - **Masalan:** talabalar jadvalida har bir qator bitta talaba haqida ma'lumot saqlaydi: talaba `ID` raqami, `ismi`, `yoshi` va `guruh`.

**Example:**

Agar bizda `students` nomli jadval bo'lsa, uning tarkibi quyidagicha bo'lishi mumkin:

|ID | Name        | Surname       | Age    | Class |
|---|-------------|---------------|--------|-------|
|1  | Ali         | Valiyev       | 20     | A1    |
|2  | Madina      | Ismailova     | 21     | A2    |
|3  | Bekzod      | Karimov       | 22     | A3    |

- **Database** - bunda bizning `"University"` nomli ma'lumotlar bazamiz bor deb tasavvur qilaylik.
- **Table** - `students` jadvali.
- **Columns** - `ID`, `Name`, `Surname`, `Age`, `Class`.
- **Rows** - har bir talaba haqida ma'lumotni ifodalovchi yozuvlar.


