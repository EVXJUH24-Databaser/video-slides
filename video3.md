---
author: Constraints
date: ""
paging: "%d / %d"
---

# Constraints

Extra krav, begränsningar och funktionalitet som kan appliceras på kolumner.

- `NOT NULL`
  - Värde kan inte vara null och måste få ett värde vid insert
- `UNIQUE`
  - Värde måste vara unikt och får inte finnas (för kolumnen) i tabell redan
- `CHECK`
  - Ser till att värde stämmer med vissa villkor
- `DEFAULT`
  - Kolumn får "default" värde om inget specificeras
- `PRIMARY KEY`
  - Identifierande kolumn, ofta `int` eller `uuid`
- `REFERENCES` (FOREIGN KEY)
  - Refererande kolumn (till en primary key)
- `SERIAL`
  - Tekniskt sätt inte en constraint, men har extra funktionalitet
  - `BIGINT` datatyp i bakgrunden
  - Ökar automatiskt (increment)

---

# Constraints exempel

```sql
CREATE TABLE user_accounts (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(100) NOT NULL,
  age INT CHECK (age >= 18 AND age <= 120),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  manager_id INT REFERENCES user_accounts(id) DEFAULT NULL,
);
```
