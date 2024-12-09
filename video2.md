---
author: Introduktion till SQL
date: ""
paging: "%d / %d"
---

# Introduktion till SQL

Hej och välkommen!

## Innehåll

1. Introduktion till PostgreSQL
2. Tabeller, rader & kolumner
3. CRUD
4. Queries med villkor

---

# Introduktion till PostgreSQL

PostgreSQL är en modern relationsdatabas. Den är väl dokumenterad, open source, snabb och har mycket funktionalitet.

- SQL som språk
- Stödjer endast strukturerad data
- Hanterar komplexa strukturer genom relationer
- Anpassad för effektiv sökning med strukturerad data

<https://www.postgresql.org/>

---

# Interna databaser

PostgreSQL är en databas och körs som program i Docker. Inom PostgreSQL finns det också "databaser". Dessa "interna" databaser är till för att organisera olika projekt. Varje projekt har typiskt sätt en egen intern databas.

Skapa med: `CREATE DATABASE <name>;`, exempelvis:

- `CREATE DATABASE todo;`
- `CREATE DATABASE personal-finance;`

Gå in i databas med: `\c <name>`, exempelvis:

- `\c todo`
- `\c personal-finance`

Lista upp databaser med: `\l`

---

# Tabeller

En tabell är en strukturerad samling med data.

- Struktur måste bestämmas (kolumner, datatyper och constraints)
- Strukturen i sig kallas 'schema'
- Innehåller rader och kolumner
  - Varje rad är som ett objekt
  - Varje kolumn är som ett fält/variabel för ett objekt

Skapa tabeller med: `CREATE TABLE <name> (columns)`, exempelvis:

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# Skapa tabell endast om den inte redan finns

```sql
CREATE TABLE IF NOT EXISTS users (
  name TEXT,
  email VARCHAR(100)
);
```

# Radera tabell och data

```sql
DROP TABLE users;
```

# Se vilka tabeller som finns i databas

`\dt`

---

# Kolumner och datatyper

- boolean
- integer
- decimal
- text
- varchar
- timestamp
- date

Läs mer: <https://www.postgresql.org/docs/current/datatype.html>

---

# CRUD med SQL

CRUD (Create, Read, Update, Delete) har matchande nyckelord i SQL.

- CREATE - INSERT
- READ - SELECT
- UPDATE - UPDATE
- DELETE - DELETE

---

# Lägg in data i tabell (CREATE)

Syntax:

```sql
INSERT INTO table (columns) VALUES (values1), (values2);
```

Exempel:

```sql
INSERT INTO products (id, title, price) VALUES (3, 'Candy', 5.3);

INSERT INTO users (name, email)
VALUES ('Ironman', 'tony@stark.com'),
       ('Superman', 'clark@kent.com'),
       ('Batman', 'bruce@wayne.com');
```

---

# Hämta/sök data från tabell (READ)

Syntax:

```sql
SELECT columns FROM table WHERE condition;
```

Exempel:

```sql
-- Hämta alla rader och kolumner
SELECT * FROM users;

-- Hämta alla rader men endast 'name' kolumnen
SELECT name FROM users;

-- Hämta alla rader som heter 'Ironman', och endast email
SELECT email FROM users WHERE name = 'Ironman';

-- Räkna rader
SELECT COUNT(*) FROM users WHERE created_at > '2023-01-01';
```

---

# Uppdatera/ändra på data i tabell (UPDATE)

Syntax:

```sql
UPDATE table SET column = value WHERE condition;
```

Exempel:

```sql
-- Uppdatera alla rader
UPDATE users SET status = 'active';

-- Uppdatera specifik kolumn för en användare baserat på namn
UPDATE users SET email = 'tony@stark.com' WHERE name = 'Ironman';

-- Uppdatera baserat på datum
UPDATE users 
SET membership_level = 'premium'
WHERE created_at > '2023-01-01';
```

---

# Radera data från tabell (DELETE)

Syntax:

```sql
DELETE FROM table WHERE condition;
```

Exempel:

```sql
-- Ta bort alla rader 
DELETE FROM users;

-- Ta bort specifik användare baserat på namn
DELETE FROM users WHERE name = 'Ironman';

-- Ta bort användare baserat på flera villkor
DELETE FROM users 
WHERE status = 'inactive' 
AND last_login < '2023-01-01';
```

---

# Villkor i queries

```sql
-- Flera villkor
SELECT * FROM products 
WHERE price > 100 
AND stock_quantity <= 50;

-- LIKE och wildcards
SELECT name, email 
FROM users 
WHERE email LIKE '%@gmail.com' 
OR email LIKE '%@hotmail.com';

-- IN för flera möjliga värden
SELECT * FROM orders 
WHERE status IN ('pending', 'processing', 'shipped')
AND total_amount > 1000;

-- BETWEEN för intervaller
SELECT * FROM sales 
WHERE sale_date BETWEEN '2023-01-01' AND '2023-12-31'
AND amount BETWEEN 1000 AND 5000;
```

---

# Fler villkor i queries

```sql
-- NULL-hantering
SELECT * FROM customers 
WHERE phone_number IS NOT NULL 
AND last_order_date IS NULL;

-- Kombinera datum och text
SELECT * FROM employees 
WHERE hire_date < '2023-01-01'
AND (department = 'Sales' OR department = 'Marketing')
AND salary >= 50000;

-- Datum-beräkningar
SELECT * FROM log_entries 
WHERE created_at >= NOW() - INTERVAL '24 HOUR'
AND severity IN ('ERROR', 'CRITICAL')
AND message NOT LIKE '%timeout%';

-- Kombinera flera villkor med parenteser
SELECT * FROM orders 
WHERE (status = 'completed' AND total_amount > 1000)
OR (status = 'pending' AND created_at < NOW() - INTERVAL '7 DAY')
ORDER BY created_at DESC;
```
