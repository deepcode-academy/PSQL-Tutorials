# Basic SQL Commands in PostgreSQL

- Topics:
  - `databases`, `tables`, `rows` va `columns` haqida tushuncha
  - `PostgreSQL`dagi ma'lumot turlari
  - Asosiy `SQL` buyruqlari: `CREATE DATABASE`, `CREATE TABLE`, `DROP`, `INSERT`, `SELECT`
  - Primary keys and constraints

> [!NOTE]
> `Databases`, `tables`, `rows` va `columns` — bu `SQL` va ma'lumotlar bazalari boshqaruvi tizimlarining asosiy tushunchalar hisoblanadi.

1. Database

  - **Database** — bu bir joyda to'plangan va mantiqiy tartibda saqlangan ma'lumotlarning yig'indisi. Ma'lumotlar bazasi yordamida biz katta hajmdagi ma'lumotlarni `boshqarish`, `saqlash`, `yangilash` va ulardan samarali foydalanish imkoniyatiga ega bo'lamiz. 

  - **Masalan:** Kompaniya o'z mijozlari, mahsulotlari, buyurtmalari haqidagi ma'lumotlarni ma'lumotlar bazasida saqlaydi.

2. Table

  - **Table** — ma'lumotlarni `struktura` shaklida saqlash uchun ma'lumotlar bazasidagi asosiy obyektlardan biri. Har bir jadvalda columns(ustunlar) va rows(qatorlar) mavjud bo'lib, ularning yordamida ma'lumotlarni `tartiblangan` shaklda saqlash va ularni `tezkor topish` mumkin. 

  - **Masalan:** Talabalar jadvali quyidagi ma'lumotlarni o'zida saqlashi mumkin: `talabalar ID` raqami, `ism`, `familiya`, `yosh` va `guruh`.

3. Column

   - **Column** — jadvalda saqlanadigan `ma'lumotlarning turini` belgilaydigan element. Har bir ustun jadvaldagi ma'lumotlarga ma'lum bir tavsif beradi va unda ma'lum turdagi ma'lumotlar (`raqamlar`, `matn`, `sana` va h.k.) saqlanadi. 

   - **Masalan:** `ism`, `familiya` va `yosh` — bu talabalar jadvalidagi ustunlardir.



