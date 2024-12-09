---
author: Relationer och joins
date: ""
paging: "%d / %d"
---

# Relationer och joins

Hej och välkommen!

## Innehåll

1. Introduktion till relationer
2. Typer av relationer
3. Exempel på relationer
4. Introduktion till joins
5. Typer av joins
6. Exempel på joins

---

# Hur sparas listor av saker i Relationsdatabaser?

En kolumn kan endast spara ett värde, inte flera som en array. Istället hanteras listor av saker med "relationer". En relation bildas när en rad i en tabell har en länk till en annan rad, oftast i en annan tabell.

**Dåligt:**

| name    | emails                                |
| ------- | ------------------------------------- |
| Ironman | a @mail.com, b @mail.com, c @mail.com |

**Bra:**

| id | name    |
| -- | ------- |
| 1  | Ironman |

| person_id | email       |
| --------- | ----------- |
| 1         | a @mail.com |
| 1         | b @mail.com |
| 1         | c @mail.com |

---

# Exempel relation: Människor <-> Husdjur

**Persons:**

| name   | age |
| ------ | --- |
| Bob    | 30  |
| Rachel | 27  |

**Pets:**

| name    | owner  |
| ------- | ------ |
| Charlie | Bob    |
| Oskar   | Bob    |
| Nevad   | Rachel |

---

# Primary keys och foreign keys

En relation är en länk mellan två rader. Primary keys och foreign keys används för att skapa länkar.

**Primary key**: en identifierande kolumn. Den är självstående.

**Foreign key**: en refererande kolumn. Refererar till en primary key.

För att referera till en primary key har foreign keys alltid samma värde som en primary key. En primary key kan dock vara helt unik, och utan foreign key.

I exemplet med människor och husdjur är `humans:name` en primary key och `pets:owner` en foreign key.

---

# Typer av relationer

## One-to-One

En rad refererar till exakt en rad. Förekommer sällan.

Exempel: barn -> pappa, nyckel -> lås

## One-to-Many och Many-to-One

En rad refererar till flera (0-n) rader. One-to-Many och Many-to-One är samma sak från olika perspektiv.

Exempel: människor <-> husdjur, användare <-> kommentarer

## Many-to-Many

Flera rader refererar till flera rader. Om One-to-Many går åt båda hållen så är det Many-to-Many. Skapas med "junction" tabeller.

Exempel: användare <-> git repos, spelare <-> spel

---

# Constraints

- `ON DELETE`
  - `RESTRICT` - Förhindra radering när relationer finns
  - `CASCADE` - Radera alla rader med matchande foreign key
  - `SET NULL` - Radera matchande foreign keys (sätt till NULL)
- `ON UPDATE`
  - `RESTRICT` - Förhindra ändring av primary key
  - `CASCADE` - Uppdaterar foreign key om primary key ändras
  - `SET NULL` - Radera matchande foreign keys (sätt till NULL)

---

# Constraints exempel

```sql
CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(id)
        ON DELETE CASCADE
        ON UPDATE SET NULL
);
```

---

# Introduktion till joins

En join är en slags query som kan hämta data från två eller fler tabeller samtidigt. De används i samband med relationer.

Joins kan användas för att hämta data i en tabell om man endast har information om en annan tabell.

Exempel: du har namnet på en person men vill veta vad namnen på husdjuren är.

---

# Typer av joins

- `INNER JOIN` - Hämtar alla rader med matchande värden
- `LEFT JOIN` - Hämtar alla rader från vänster, och de rader från höger som matchar
- `RIGHT JOIN` - Hämtar alla rader från höger, och de rader från vänster som matchar
- `FULL OUTER JOIN` - Hämtar alla rader och matchar de som går
- `CROSS JOIN` - Kombinerar alla rader och kolumner från båda tabeller

---

# Joins exempel

```sql
SELECT 
    employees.name,
    departments.dept_name
FROM employees
INNER JOIN departments ON employees.dept_id = departments.dept_id;

SELECT 
    employees.name,
    departments.dept_name
FROM employees
LEFT JOIN departments ON employees.dept_id = departments.dept_id;

SELECT 
    employees.name,
    departments.dept_name
FROM employees
FULL OUTER JOIN departments ON employees.dept_id = departments.dept_id;
```

---

# Joins exempel (One-to-Many)

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT, password TEXT);
CREATE TABLE comments (id SERIAL PRIMARY KEY, content TEXT, user_id INT REFERENCES users(id));

-- Hämta all kommentarer från ett användarnamn
SELECT content from comments 
INNER JOIN users 
  ON users.id = comments.user_id 
WHERE users.name = 'Ironman';
```

---

# Joins och Many-to-Many

Many-to-Many kräver ibland två joins.

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT);
CREATE TABLE repos (id SERIAL PRIMARY KEY, name TEXT, public BOOLEAN);
CREATE TABLE user_repos(repo_id INT REFERENCES repos(id), user_id INT REFERENCES users(id));

-- Hämta all repos från ett användarnamn
SELECT repos.name FROM repos 
INNER JOIN user_repos 
  ON repos.id = user_repos.repo_id 
INNER JOIN users 
  ON users.id = user_repos.user_id 
WHERE users.name = 'Ironman';
```
