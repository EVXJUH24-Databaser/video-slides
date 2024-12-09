---
author: SQL funktioner
date: ""
paging: "%d / %d"
---

# SQL funktioner

Hej och välkommen!

## Innehåll

1. Typer av funktioner
2. Matematiska funktioner
3. Sträng funktioner
4. Datum och tid funktioner
5. Aggregate funktioner
6. Andra funktioner

---

# Typer av funktioner

- Matematiska funktioner
- Sträng funktioner
- Tid och datum funktioner
- Aggregate funktioner
- GROUP BY, ORDER BY, HAVING

---

# Matematiska funktioner

- Enkla (+, -, *, /)
- Bitwise (AND, NOT, XOR, SHIFT)
- Funktioner (abs, exp, floor, log, sqrt)

<https://www.postgresql.org/docs/current/functions-math.html>

---

# Exempel på matematiska funktioner

```sql
CREATE TABLE products (price DECIMAL(10, 2));

SELECT price * 1.08 - 5 FROM products;
SELECT sqrt(price) FROM products;
SELECT abs(-price * 50) FROM products;
```

---

# Sträng funktioner

- Concatenation (||)
- Längd på sträng (char_length)
- Omvandla små eller stora bokstäver (lower & upper)
- Substring
- Trim (ltrim, btrim)

Många av dessa funktioner kan och bör göras med kod istället.

<https://www.postgresql.org/docs/current/functions-string.html>

---

# Exempel på sträng funktioner

```sql
CREATE TABLE employees (
    name VARCHAR(255),
    place VARCHAR(255)
);

SELECT 'Welcome to ' || place || ', ' || name || '.' FROM employees;
SELECT place FROM employees WHERE char_length(name) > 3;
SELECT upper(name) FROM employees;
```

---

# Datum och tid funktioner

- Addition och subtraktion (dagar, intervaller m.m)
- Ålder (age)
- Nuvarande tid (current_time, current_date)
- Truncate (date_trunc)
- Extrahera (extract)

<https://www.postgresql.org/docs/current/functions-datetime.html>

---

# Exempel på datum funktioner

```sql
CREATE TABLE appointments (
    start_date DATE,
    end_date DATE
);

INSERT INTO appointments (start_date, end_date) VALUES (current_date, '2020-05-14');
SELECT * FROM appointments WHERE start_date - 30 > '2024-10-01';
SELECT EXTRACT(DAY FROM start_date) FROM appointments WHERE end_date BETWEEN '2019-12-31' AND '2020-01-31';
```

---

# Aggregate funktioner

- Sum
- Count
- Min & max
- Avg

<https://www.postgresql.org/docs/current/functions-aggregate.html>

---

# Exempel på aggregate funktioner

```sql
CREATE TABLE salaries (
    employee_name TEXT,
    salary DECIMAL(10, 2),
    department VARCHAR(255)
);

SELECT avg(salary) FROM salaries;
SELECT count(*) FROM salaries;
SELECT sum(salary) FROM salaries;
```

---

# Andra funktioner

```sql
-- GROUP BY: Lägg alla med samma kolumner i en grupp
SELECT department, avg(salary) FROM salaries GROUP BY department;

-- ORDER BY: Sortera baserat på kolumn
SELECT employee_name, salary FROM salaries ORDER BY salary DESC;

-- HAVING: Inkludera endast grupper som matchar villkor
SELECT department, avg(salary) FROM salaries GROUP BY department HAVING avg(salary) > 30000;
```

<https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-GROUP>
