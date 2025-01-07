# Indexes and Performance Optimization

- Topics:
  - Purpose of indexes and their impact on query performance
  - Creating and managing indexes
  - Types of indexes: B-tree, hash
  - Understanding query plans with `EXPLAIN`

# Purpose of indexes and their impact on query performance

### What is an Index and Its Purpose?

Index bu jadval ustunidagi ma'lumotlarni tezroq qidirish va ularga murojaat qilish uchun ma'lumotlar bazasida yaratiladigan ma'lumotlar tuzilmasidir. Index kitobning oxiridagi alfavitli ko'rsatkichga o'xshaydi — ma'lumotlarni topishni osonlashtiradi.

Main purposes:
1. **Speed up queries**: Indexlar ma'lumotlarni qidirishda jadvalni to'liq o'qib chiqishga ehtiyojni yo'q qiladi.
2. **Optimize resources**: So'rov vaqtida protsessor va xotira resurslaridan samarali foydalanishni ta'minlaydi.
3. **Support sorted searches**: `ORDER BY`, `WHERE`, va `JOIN` shartlarini bajarishda ishlaydi.

