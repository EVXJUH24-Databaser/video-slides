---
author: Introduktion till databaser
date: ""
paging: "%d / %d"
---

# Introduktion till databaser

## Innehåll

1. Installation genom Docker
2. Vad är en databas?
3. Varför inte listor eller filer?
4. Exempel på databaser
5. Begrepp och förkortningar

---

# Docker

Docker är ett program som hanterar andra program. Vi använder det för att installera och hantera databaser.

**Container**: program som körs i Docker

**Image**: mall för containers

Installera WSL (för Windows): <https://learn.microsoft.com/en-us/windows/wsl/install>

Installera Docker Desktop: <https://docs.docker.com/desktop/>

---

## Installera PostgreSQL

1. `docker pull postgres`
2. `docker run -p 5432:5432 -e POSTGRES_PASSWORD=password --name my-postgres -d postgres`
3. `docker start my-postgres` - om container är stoppad

## Gå in i PostgreSQL

1. `docker exec -it my-postgres bash` - gå in i container
2. `psql -U postgres` - starta klient och ange lösenord
3. `\l` - lista upp databaser
4. `CREATE DATABASE mydb;` - skapa egen databas
5. `\c mydb` - anslut till egen databas

---

# Introduktion till databaser

## Vad är en databas?

- Program som sparar och hanterar data
  - Exempel: uppgifter, transaktioner, blogginlägg, produkter, kommentarer
- Sparar data "persistently": den finns kvar även efter omstart
- Innehåller funktionalitet för avancerad sökning
  - Villkor
  - Funktioner

---

# Varför och varför inte listor?

Bra:

- Sparar data effektivt
- Snabb åtkomst

Dåligt:

- Sparar data temporärt: den finns inte kvar efter omstart
- Låg lagringskapacitet: datorn har lite minne

---

# Varför och varför inte filer?

Bra:

- Sparar data persistently
- Hög lagringskapacitet: datorn har stort lager

Dåligt:

- Sparar data oeffektivt
- Kan inte hantera sökning
  - Detta implementeras manuellt
- Sparar lokalt: andra datorer kan inte se dem

---

# Exempel på databaser

- [PostgreSQL](https://www.postgresql.org/)
  - Bra på komplexa strukturer och relationer
- [MongoDB](https://www.mongodb.com/)
  - Bra på simpla strukturer och få relationer
- [MySQL](https://www.mysql.com/)
  - Bra på komplexa strukturer och relationer
- [Redis](https://redis.io/)
  - Bra på simpla strukturer och få relationer
  - Extremt snabb
  - Låg lagringskapacitet

---

# Vilka databaser används i vilka situationer?

## SQL

- E-handel
- Sociala medier
- Banker

## NoSQL / Document

- Profil data
- Nyhetssida
- IoT (e.g. sensorer)

## In-memory

- Leaderboards & scoreboards
- Trading app

---

# Hur MongoDB fungerar

- Collections
- Documents
- Filters
- BSON (JSON-variant)

---

# MongoDB exempel

```java
MongoClient client = MongoClients.create("mongodb://localhost:27017");
MongoDatabase database = client.getDatabase("some-db");
    
MongoCollection<Document> users = database.getCollection("users");
    
Document user = new Document()
  .append("name", "Ironman")
  .append("email", "tony@stark.com")
  .append("age", 30)
  .append("skills", Arrays.asList("Java", "MongoDB", "Web Development"));
    
users.insertOne(user);
    
Document filter = new Document("email", "clark@kent.com");
Document foundUser = users.find(filter).first();
    
System.out.println("Found user: " + foundUser);
```

---

# Hur Redis fungerar

- Keys
- Values
- JSON stöd
- Caching

---

# Redis exempel

```java
Jedis jedis = new Jedis("localhost", 6379);

jedis.set("user:1:name", "Ironman");
String name = jedis.get("user:1:name");
System.out.println("Retrieved name: " + name);

jedis.sadd("user:1:skills", "Java", "SQL", "Redis");
boolean hasRedis = jedis.sismember("user:1:skills", "Redis");
System.out.println("Has Redis skill: " + hasRedis);
```

---

# Hur PostgreSQL fungerar

- Tabeller
- Rader
- Kolumner
- Constraints
- Relationer
- Joins
- Transactions
- SQL / queries

---

# PostgreSQL SQL exempel

```sql
-- Create a table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert data into a table
INSERT INTO users (name, email)
VALUES ('Ironman', 'tony@stark.com'),
       ('Superman', 'clark@kent.com'),
       ('Batman', 'bruce@wayne.com');

-- Query data from a table
SELECT * FROM users;
SELECT name, email FROM users WHERE id = 2;
SELECT COUNT(*) FROM users WHERE created_at > '2023-01-01';
```

---

# PostgreSQL kod exempel

```java
Connection conn = DriverManager.getConnection(connectionString);
Statement st = conn.createStatement();
ResultSet rs = st.executeQuery("SELECT * FROM pets");

while (rs.next()) {
    System.out.println("Pet - " + rs.getString("name"));
    System.out.println(" id = " + rs.getInt("id"));
    System.out.println(" type = " + rs.getString("type"));
    System.out.println(" favoriteFood = " + rs.getString("favoriteFood"));
}

rs.close();
st.close();
conn.close();
```

---

# Tabeller

En slags lista som håller information. Består av rader och kolumner.

# Rader

En samling med information likt ett Java objekt

# Kolumner

Ett fält som finns i en rad. Likt fält (class fields) i Java klasser.

---

# Exempel tabeller

| Id | Name            | Email               | Created At          |
| -- | --------------- | ------------------- | ------------------- |
| 1  | Captain America | steve @avengers.com | 2015-05-01 12:00:00 |
| 2  | Ironman         | tony @avengers.com  | 2008-04-30 09:18:00 |
| 3  | Thor            | thor @asgard.com    | 2011-05-06 15:30:00 |
| 4  | Black Widow     | natasha @shield.gov | 2010-06-07 08:00:00 |
| 5  | Hulk            | bruce @gamma.lab    | 2012-03-15 11:45:00 |

| Id | Wizard Name        | Spell                   | Wand Type                 | Enrollment Date |
| -- | ------------------ | ----------------------- | ------------------------- | --------------- |
| 1  | Harry Potter       | Expecto Patronum        | Holly, Phoenix Feather    | 1991-09-01      |
| 2  | Hermione Granger   | Wingardium Leviosa      | Vine, Dragon Heartstring  | 1991-09-01      |
| 3  | Ron Weasley        | Arachnophobia           | Willow, Unicorn Hair      | 1991-09-01      |
| 4  | Albus Dumbledore   | Transfiguration         | Elder, Thestral Tail Hair | 1892-09-01      |
| 5  | Minerva McGonagall | Animagus Transformation | Fir, Dragon Heartstring   | 1947-09-01      |

---

# Begrepp och förkortningar

## Databas

Ett program som är till för att hantera stora mängder med data.

---

# Begrepp och förkortningar

## Typer av databaser

- **SQL**:
  - Anpassad efter relationer och effektiv sökning
  - Strukturerad
  - Stöd för avancerade strukturer och relationer

- **NoSQL**:
  - Anpassad efter enkla och dynamiska sökningar
  - Stöd för simpla strukturer men inga relationer

- **Document**:
  - Variant av **NoSQL**
  - Stöd för dynamisk struktur

- **In-memory**:
  - Anpassad efter extremt snabb sökning
  - Inga stöd för strukturer och relationer
  - Sparar data temporärt

---

# Begrepp och förkortningar

## SQL (Structured Query Language)

Ett språk som vissa databaser använder för kommunikation. Det kan exempelvis användas för att skapa en "sökning".

---

# Begrepp och förkortningar

## Tabell

En strukturerad samling med rader i en SQL databas. De kan jämföras med listor (med objekt) i Java.

---

# Begrepp och förkortningar

## Model och Entity

En mall på en strukturerad rad i en tabell. De kan jämföras med klasser i Java.

Man kallar även ibland själva raderna för models.

---

# Begrepp och förkortningar

## Schema

Strukturen på en tabell. De definierar vilka kolumner, datatyper och constraints som skall finnas.

---

# Begrepp och förkortningar

## Rad / row

En samling med strukturerad data i en tabell. De kan jämföras med objekt i Java.

## Kolumn / column

En egenskap/fält/variabel för en rad i en tabell. De kan jämföras med fält i Java klasser. Varje kolumn har en datatyp.

---

# Begrepp och förkortningar

## ACID

1. **A**tomicity
2. **C**onsistency
3. **I**solation
4. **D**urability

Principer som man följer i databas design.

---

# Begrepp och förkortningar

## CRUD

1. **C**reate
2. **R**ead
3. **U**pdate
4. **D**elete

Vanliga handlingar som utförs inom databas sammanhang.

---

# Begrepp och förkortningar

## INSERT, SELECT, UPDATE, DELETE

CRUD nyckelord för SQL.

- **INSERT** (create): ladda upp data
- **SELECT** (read): hämta/sök data
- **UPDATE** (update): ändra befintlig data
- **DELETE** (delete): radera data

---

# Begrepp och förkortningar

## Query

Ett meddelande som skickas till en databas för att söka information. En "sökning".

---

# Begrepp och förkortningar

## Primary key och foreign key

Två speciella typer av kolumner (fält). Primary key är en identifierande kolumn. Foreign key är en refererande kolumn.

---

# Begrepp och förkortningar

## Relation

En koppling som finns mellan två tabeller, eller mer specifikt, två rader i två tabeller.

Exempel:

- Användare > Email
- Ordrar > Produkter
- Studerande > Kurser

---

# Begrepp och förkortningar

## Docker

Ett program som används för att hantera andra program. Det kan jämföras med Steam: Steam är ett spelbibliotek bland annat, och om man säger att ett spel är ett program så är Steam ett program av program. På samma sätt är Docker ett program av (andra) program.

Docker används för att installera (och avinstallera) program, köra dem, konfigurera dem och mer.

Vi använder Docker för att installera och hantera databaser.

---

# Begrepp och förkortningar

Språk för att kommunicera inom olika områden.

## DDL (Data Definition Language)

- Skapa schemas och tabeller
- Ta bort tabeller
- Modifiera tabeller

## DQL (Data Query Language)

- Hämta data (SELECT)

## DML (Data Manipulation Language)

- Ladda upp data (INSERT)
- Uppdatera data (UPDATE)

## DCL (Data Control Language)

- Hantera tillåtelser och användare

---

# Begrepp och förkortningar

## Constraint

En begränsning som kan sättas på kolumner.

Exempel:

- NOT NULL
- PRIMARY KEY

---

# Begrepp och förkortningar

## Transaction

En samling med SQL kommandon vilka hanteras som en enhet. Kommer med felhantering och restoration.

---

# Begrepp och förkortningar

## Join

Ett speciellt query-nyckelord som får två tabeller att blandas ihop så att sökningen kan ske i båda samtidigt.

---

# Begrepp och förkortningar

## Normalisering

En samling med sätt att designa databaser och tabeller på ett effektivt och säkert sätt.

---

# Begrepp och förkortningar

## ORM (Object-relational mapping)

En process som omvandlar objekt (med exempelvis Java kod) till tabeller, rader och kolumner i en databas.

Exempel ramverk:

- Entity Framework (EF, C#)
- Hibernate (Java)

---

# Begrepp och förkortningar

## Aggregate function

Sökningar (queries) kan innehålla funktioner som exempelvis:

- SUM
- COUNT
- MAX
