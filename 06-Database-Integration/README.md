Here is the complete, merged, and fully expanded **Database Integration** README — nothing removed, all gaps filled:

---

# 🗄️ Database Integration — Complete Beginner's Course

> A full learning guide covering PostgreSQL, MongoDB, ORM/ODM, Querying, Relationships, and Database Design from scratch.

---

## 📚 Table of Contents

1. [What is a Database?](#1-what-is-a-database)
2. [PostgreSQL — Relational Database](#2-postgresql--relational-database)
3. [MongoDB — NoSQL Database](#3-mongodb--nosql-database)
4. [PostgreSQL vs MongoDB — Which One?](#4-postgresql-vs-mongodb--which-one)
5. [Database Connections](#5-database-connections)
6. [ORM & ODM Concepts](#6-orm--odm-concepts)
7. [Querying Data](#7-querying-data)
8. [Relationships](#8-relationships)
9. [Database Design Basics](#9-database-design-basics)
10. [Migrations & Seeding](#10-migrations--seeding)
11. [Transactions](#11-transactions)
12. [Practice Projects](#12-practice-projects)

---

## 1. What is a Database?

A **database** is an organized place where your application stores and retrieves data — think of it like a super-powered Excel spreadsheet that your code can talk to.

### Why do we need one?

Without a database, data disappears when your app restarts. Databases make data **persistent** (it stays there forever until you delete it).

### Two main types

| Type | Description | Examples |
|------|-------------|---------|
| **Relational (SQL)** | Data stored in tables with rows and columns, like a spreadsheet | PostgreSQL, MySQL, SQLite |
| **NoSQL** | Data stored in flexible formats like JSON documents | MongoDB, Redis, Firebase |

### How an app talks to a database

```
User's Browser
      ↓
  Your App (Node.js / Express)
      ↓
  Database Driver / ORM
      ↓
  Database (PostgreSQL or MongoDB)
```

Every time a user signs up, makes a purchase, or sends a message — your app writes to the database. Every time they load a page — your app reads from it.

---

## 2. PostgreSQL — Relational Database

### What is PostgreSQL?

PostgreSQL (often called **Postgres**) is a powerful, open-source relational database. Data lives in **tables**, and tables relate to each other.

> Founded in 1986, PostgreSQL is renowned for its robustness, ACID compliance, and support for complex queries. It excels in scenarios where data integrity and structured relationships are key — making it ideal for finance, enterprise content management, and any environment that demands rigorous transactional support.

Think of it like this:

```
users table          orders table
-----------          ------------
id | name | email    id | user_id | product
1  | Abi  | a@x.com  1  | 1       | Laptop
2  | Ben  | b@x.com  2  | 1       | Mouse
```

### Installing PostgreSQL

```bash
# Ubuntu/Linux
sudo apt update
sudo apt install postgresql postgresql-contrib

# macOS (with Homebrew)
brew install postgresql

# Start the service
sudo service postgresql start
```

### Connecting via terminal

```bash
# Connect as the default postgres user
sudo -u postgres psql

# Inside psql, create a database
CREATE DATABASE myapp;

# Connect to it
\c myapp

# See all tables
\dt

# See the structure of a specific table
\d users

# Exit
\q
```

### Basic SQL Commands

```sql
-- Create a table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) UNIQUE NOT NULL,
  age INT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Insert data
INSERT INTO users (name, email, age)
VALUES ('Alice', 'alice@email.com', 25);

-- Read data
SELECT * FROM users;
SELECT name, email FROM users WHERE age > 20;

-- Update data
UPDATE users SET age = 26 WHERE name = 'Alice';

-- Delete data
DELETE FROM users WHERE name = 'Alice';
```

> 💡 **SERIAL PRIMARY KEY** means the `id` column auto-increments — PostgreSQL assigns 1, 2, 3... automatically.

### PostgreSQL Data Types Reference

| Type | Use for | Example |
|------|---------|---------|
| `SERIAL` | Auto-incrementing integer IDs | `id SERIAL PRIMARY KEY` |
| `VARCHAR(n)` | Short text with max length | `name VARCHAR(100)` |
| `TEXT` | Long text, no length limit | `bio TEXT` |
| `INT` / `BIGINT` | Whole numbers | `age INT` |
| `DECIMAL(p,s)` | Precise decimals (money) | `price DECIMAL(10,2)` |
| `BOOLEAN` | True/false | `is_active BOOLEAN` |
| `TIMESTAMP` | Date and time | `created_at TIMESTAMP` |
| `DATE` | Date only | `birth_date DATE` |
| `JSONB` | JSON data (searchable) | `metadata JSONB` |

---

## 3. MongoDB — NoSQL Database

### What is MongoDB?

MongoDB stores data as **documents** (basically JSON objects) inside **collections** (like folders). There are no fixed columns — each document can have different fields.

> Developed in 2009, MongoDB is a NoSQL document-oriented database designed to accommodate large volumes of unstructured data. Its primary use cases include applications that require high scalability and flexibility — such as content management systems, mobile applications, and real-time analytics where data structures might change frequently.

```json
// A MongoDB "document" (like a row in SQL)
{
  "_id": "64abc123",
  "name": "Alice",
  "email": "alice@email.com",
  "hobbies": ["coding", "reading"],
  "address": {
    "city": "Addis Ababa",
    "country": "Ethiopia"
  }
}
```

Notice you can store **arrays** and **nested objects** directly — this is very different from SQL.

### SQL Terminology vs MongoDB Terminology

| SQL | MongoDB | Description |
|-----|---------|-------------|
| Database | Database | The top-level container |
| Table | Collection | Group of related records |
| Row | Document | A single record |
| Column | Field | A single piece of data |
| Primary Key | `_id` | Unique identifier |
| JOIN | `$lookup` / populate | Combining related data |

### Installing MongoDB

```bash
# Ubuntu
sudo apt-get install -y mongodb

# macOS
brew tap mongodb/brew
brew install mongodb-community

# Start MongoDB
sudo systemctl start mongod
```

### Basic MongoDB Commands

```javascript
// Open the mongo shell
mongosh

// Create/switch to a database
use myapp

// Insert a document
db.users.insertOne({
  name: "Alice",
  email: "alice@email.com",
  age: 25
})

// Insert many at once
db.users.insertMany([
  { name: "Bob", age: 30 },
  { name: "Carol", age: 22 }
])

// Find all documents
db.users.find()

// Find with a filter
db.users.find({ age: { $gt: 20 } })

// Update
db.users.updateOne(
  { name: "Alice" },
  { $set: { age: 26 } }
)

// Delete
db.users.deleteOne({ name: "Alice" })

// Count documents
db.users.countDocuments()

// Drop (delete) an entire collection
db.users.drop()
```

---

## 4. PostgreSQL vs MongoDB — Which One?

### Feature Comparison

| Feature | PostgreSQL | MongoDB |
|--------|-----------|---------|
| **Founded** | 1986 | 2009 |
| **Type** | Relational (SQL) | NoSQL (Document-based) |
| **Data format** | Tables & rows | JSON-like documents |
| **Schema** | Fixed (defined upfront) | Flexible (varies per document) |
| **Relationships** | Very strong (JOIN support) | Possible but less natural |
| **ACID compliance** | ✅ Full | ✅ Since v4.0 (multi-doc) |
| **Horizontal scaling** | Replication | Sharding (built-in) |
| **Query language** | SQL | MongoDB Query Language (MQL) |
| **Best for** | Finance, enterprise, transactional | Real-time apps, big data, IoT |
| **Open source** | ✅ Fully | Partially (Server Side Public License) |
| **Notable users** | Apple, Skype, Instagram | eBay, Uber, Lyft |

### Data Model Difference — Visual

**PostgreSQL (relational):**
```
users table                posts table
┌────┬───────┬──────────┐  ┌────┬─────────┬────────────┐
│ id │ name  │ email    │  │ id │ user_id │ title      │
├────┼───────┼──────────┤  ├────┼─────────┼────────────┤
│ 1  │ Alice │ a@x.com  │  │ 1  │ 1       │ Hello      │
│ 2  │ Bob   │ b@x.com  │  │ 2  │ 1       │ World      │
└────┴───────┴──────────┘  └────┴─────────┴────────────┘
        linked via user_id ──────────────┘
```

**MongoDB (document-based):**
```json
{
  "_id": "abc123",
  "name": "Alice",
  "email": "a@x.com",
  "posts": [
    { "title": "Hello", "body": "..." },
    { "title": "World", "body": "..." }
  ]
}
```

### When to choose which

> 🧠 **Rule of thumb:**
> - Data is **structured and highly relational** (users, orders, invoices) → **PostgreSQL**
> - Data is **flexible or document-like** (user profiles, product catalogs, logs) → **MongoDB**
> - You need **strict data integrity and transactions** → **PostgreSQL**
> - You need **fast prototyping and flexible schema** → **MongoDB**

---

## 5. Database Connections

Your Node.js app needs to **connect** to the database before it can read or write data.

### Connecting to PostgreSQL with `pg`

```bash
npm install pg
```

```javascript
// db.js
const { Pool } = require('pg');

const pool = new Pool({
  host: process.env.DB_HOST || 'localhost',
  port: process.env.DB_PORT || 5432,
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  max: 10,                // max number of connections in the pool
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

// Test the connection
pool.connect((err, client, release) => {
  if (err) {
    console.error('Connection error:', err.message);
  } else {
    console.log('✅ Connected to PostgreSQL!');
    release(); // always release the client back to the pool
  }
});

module.exports = pool;
```

```javascript
// Using the connection in your app
const pool = require('./db');

async function getUsers() {
  const result = await pool.query('SELECT * FROM users');
  return result.rows; // array of user objects
}

// Parameterized queries — ALWAYS use these to prevent SQL injection
async function getUserByEmail(email) {
  const result = await pool.query(
    'SELECT * FROM users WHERE email = $1',  // $1 is a placeholder
    [email]                                  // value is passed separately
  );
  return result.rows[0];
}
```

> ⚠️ **Never do this** — it opens you to SQL Injection attacks:
> ```javascript
> // ❌ DANGEROUS
> pool.query(`SELECT * FROM users WHERE email = '${email}'`);
>
> // ✅ SAFE — use parameterized queries
> pool.query('SELECT * FROM users WHERE email = $1', [email]);
> ```

### Connecting to MongoDB with `mongoose`

```bash
npm install mongoose
```

```javascript
// db.js
const mongoose = require('mongoose');

async function connectDB() {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log('✅ Connected to MongoDB!');
  } catch (err) {
    console.error('❌ Connection error:', err.message);
    process.exit(1); // stop the app if DB fails
  }
}

// Listen for connection events
mongoose.connection.on('disconnected', () => {
  console.warn('⚠️ MongoDB disconnected');
});

module.exports = connectDB;
```

```javascript
// server.js — connect before starting the server
const connectDB = require('./db');

async function start() {
  await connectDB();            // connect to DB first
  app.listen(3000, () => {
    console.log('Server running on port 3000');
  });
}

start();
```

### Using Environment Variables (Best Practice)

Never hardcode your database credentials in code. Use a `.env` file:

```bash
npm install dotenv
```

```env
# .env  — NEVER commit this file to GitHub
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
DB_USER=postgres
DB_PASS=mysecretpassword
MONGO_URI=mongodb://localhost:27017/myapp
```

```javascript
// server.js — load env vars first
require('dotenv').config();

const pool = new Pool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
});
```

```bash
# .gitignore — add this so .env never gets pushed
.env
node_modules/
```

> 💡 **Connection Pools:** The `Pool` in `pg` keeps several connections open and reuses them — much faster than opening a new connection for every query. Think of it like having 10 phone lines open instead of dialling a new number every time.

---

## 6. ORM & ODM Concepts

### The Problem ORM/ODM Solves

Without ORM/ODM, every time you want to talk to your database you have to:

1. Write raw SQL or MongoDB query strings manually
2. Get back plain data and manually convert it to JavaScript objects
3. Manually validate every field before saving
4. Repeat this for every single operation in your app

```javascript
// Without ORM — raw SQL, manual everything
const result = await pool.query(
  'SELECT id, name, email FROM users WHERE age > $1 AND active = $2',
  [18, true]
);
// result.rows is just a plain array — no methods, no validation
const users = result.rows;
```

**ORM/ODM fixes this** by letting you work with database records as proper JavaScript objects with built-in methods, validation, and relationships:

```javascript
// With ORM — clean, readable, safe
const users = await User.findAll({
  where: { age: { [Op.gt]: 18 }, active: true }
});
// users is an array of User instances with methods like user.save(), user.destroy()
```

### What Exactly is an ORM?

**ORM = Object-Relational Mapper**

A layer that sits between your JavaScript code and your SQL database. It **maps** database tables to JavaScript classes, and rows to JavaScript objects.

```
Your JavaScript Code
        ↕
      ORM (Sequelize)         ← translates JS ↔ SQL
        ↕
   PostgreSQL Database
```

Think of ORM like a **translator**. You speak JavaScript, the database speaks SQL. The ORM translates between the two so you never have to write SQL yourself (unless you want to).

### What Exactly is an ODM?

**ODM = Object-Document Mapper** — same concept as ORM but for document databases like MongoDB.

```
Your JavaScript Code
        ↕
     ODM (Mongoose)           ← translates JS ↔ MongoDB queries
        ↕
    MongoDB Database
```

### ORM vs ODM Summary

| | ORM | ODM |
|--|-----|-----|
| Full name | Object-Relational Mapper | Object-Document Mapper |
| Used with | SQL databases (PostgreSQL, MySQL) | NoSQL databases (MongoDB) |
| Popular library | Sequelize, Prisma, TypeORM | Mongoose |
| Maps | Tables → Classes, Rows → Objects | Collections → Models, Documents → Objects |

---

### Deep Dive: Sequelize (ORM for PostgreSQL)

#### Step 1 — Install

```bash
npm install sequelize pg pg-hstore
```

- `sequelize` — the ORM library itself
- `pg` — the PostgreSQL driver Sequelize uses under the hood
- `pg-hstore` — handles a special PostgreSQL data type

#### Step 2 — Set up the connection

```javascript
// database.js
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize(
  process.env.DB_NAME,
  process.env.DB_USER,
  process.env.DB_PASS,
  {
    host: process.env.DB_HOST || 'localhost',
    dialect: 'postgres',   // tells Sequelize we are using PostgreSQL
    logging: false,        // set to console.log to see raw SQL in terminal
  }
);

async function testConnection() {
  try {
    await sequelize.authenticate();
    console.log('✅ Database connected successfully!');
  } catch (error) {
    console.error('❌ Unable to connect:', error.message);
  }
}

testConnection();
module.exports = sequelize;
```

#### Step 3 — Define a Model

A **Model** is a JavaScript class that represents a table. You define it once and Sequelize handles creating and managing the table.

```javascript
// models/User.js
const { DataTypes } = require('sequelize');
const sequelize = require('../database');

const User = sequelize.define(
  'User',
  {
    id: {
      type: DataTypes.INTEGER,
      primaryKey: true,
      autoIncrement: true,
    },
    name: {
      type: DataTypes.STRING(100),
      allowNull: false,
    },
    email: {
      type: DataTypes.STRING,
      allowNull: false,
      unique: true,
      validate: {
        isEmail: true,      // Sequelize validates format before saving
      },
    },
    age: {
      type: DataTypes.INTEGER,
      validate: { min: 0, max: 120 },
    },
    isActive: {
      type: DataTypes.BOOLEAN,
      defaultValue: true,
    },
  },
  {
    tableName: 'users',
    timestamps: true,       // auto-adds createdAt and updatedAt columns
  }
);

module.exports = User;
```

#### Step 4 — Sync the Model (create the table)

```javascript
// app.js
const sequelize = require('./database');
const User = require('./models/User');

async function main() {
  // sync({ force: false }) — creates table only if it doesn't exist
  // sync({ force: true })  — drops and recreates every time ⚠️ (deletes all data)
  // sync({ alter: true })  — updates the table to match your model (safe for dev)
  await sequelize.sync({ alter: true });
  console.log('✅ Tables synced!');
}

main();
```

#### Step 5 — CRUD Operations with Sequelize

```javascript
const { Op } = require('sequelize');
const User = require('./models/User');

// ── CREATE ──────────────────────────────────────────

// create() — builds AND saves in one step
const alice = await User.create({
  name: 'Alice',
  email: 'alice@email.com',
  age: 25,
});
console.log(alice.id);    // 1 (auto-assigned)
console.log(alice.name);  // 'Alice'

// build() + save() — two steps, modify before saving
const bob = User.build({ name: 'Bob', email: 'bob@email.com', age: 30 });
bob.age = 31;
await bob.save();

// ── READ ────────────────────────────────────────────

const allUsers = await User.findAll();

const adults = await User.findAll({
  where: { age: { [Op.gte]: 18 } },
  order: [['name', 'ASC']],
  limit: 10,
  offset: 0,     // pagination: offset = (page - 1) * limit
});

const user = await User.findOne({ where: { email: 'alice@email.com' } });
const userById = await User.findByPk(1);

// findOrCreate — gets or creates if not found
const [user, wasCreated] = await User.findOrCreate({
  where: { email: 'carol@email.com' },
  defaults: { name: 'Carol', age: 28 },
});
console.log(wasCreated); // true if a new record was created

// ── UPDATE ──────────────────────────────────────────

// Option 1: update an instance
const alice = await User.findByPk(1);
alice.age = 26;
await alice.save();   // only saves changed fields

// Option 2: bulk update
await User.update(
  { isActive: false },
  { where: { age: { [Op.lt]: 18 } } }
);

// ── DELETE ──────────────────────────────────────────

const alice = await User.findByPk(1);
await alice.destroy();

// Bulk delete
await User.destroy({ where: { isActive: false } });
```

#### Sequelize Operators

```javascript
const { Op } = require('sequelize');

// Comparison
{ age: { [Op.gt]: 18 } }              // age > 18
{ age: { [Op.gte]: 18 } }             // age >= 18
{ age: { [Op.lt]: 65 } }              // age < 65
{ age: { [Op.lte]: 65 } }             // age <= 65
{ age: { [Op.ne]: 25 } }              // age != 25
{ age: { [Op.between]: [18, 65] } }   // BETWEEN 18 AND 65

// String matching
{ name: { [Op.like]: 'A%' } }         // LIKE 'A%' (starts with A)
{ name: { [Op.iLike]: 'a%' } }        // case-insensitive LIKE

// Array membership
{ name: { [Op.in]: ['Alice', 'Bob'] } }
{ name: { [Op.notIn]: ['Charlie'] } }

// Logical
{ [Op.and]: [{ age: { [Op.gt]: 18 } }, { isActive: true }] }
{ [Op.or]:  [{ name: 'Alice' }, { name: 'Bob' }] }
```

---

### Deep Dive: Mongoose (ODM for MongoDB)

#### Step 1 — Install

```bash
npm install mongoose
```

#### Step 2 — Connect

```javascript
// database.js
const mongoose = require('mongoose');

async function connectDB() {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log('✅ MongoDB connected!');
  } catch (error) {
    console.error('❌ Connection failed:', error.message);
    process.exit(1);
  }
}

module.exports = connectDB;
```

#### Step 3 — Define a Schema

A **Schema** enforces a consistent structure at the application level — Mongoose checks it before saving anything to MongoDB.

```javascript
// models/User.js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: [true, 'Name is required'],
      trim: true,
      minlength: 2,
      maxlength: 100,
    },
    email: {
      type: String,
      required: [true, 'Email is required'],
      unique: true,
      lowercase: true,
      match: [/^\S+@\S+\.\S+$/, 'Invalid email format'],
    },
    age: {
      type: Number,
      min: [0, 'Age cannot be negative'],
      max: [120, 'Age seems too high'],
    },
    role: {
      type: String,
      enum: ['user', 'admin', 'moderator'],
      default: 'user',
    },
    isActive: {
      type: Boolean,
      default: true,
    },
    tags: [String],           // array of strings
    address: {                // nested sub-document
      street: String,
      city: String,
      country: String,
    },
  },
  {
    timestamps: true,         // auto-adds createdAt and updatedAt
  }
);

const User = mongoose.model('User', userSchema);
// Mongoose creates a collection called 'users' (auto-pluralized, lowercased)

module.exports = User;
```

#### Step 4 — Schema Methods & Virtuals

```javascript
// Instance method — available on a single document
userSchema.methods.getDisplayName = function () {
  return `${this.name} (${this.email})`;
};

// Static method — available on the Model class itself
userSchema.statics.findActiveUsers = function () {
  return this.find({ isActive: true });
};

// Virtual — computed field NOT stored in the DB
userSchema.virtual('isAdult').get(function () {
  return this.age >= 18;
});

// Usage
const user = await User.findOne({ name: 'Alice' });
console.log(user.getDisplayName());  // 'Alice (alice@email.com)'
console.log(user.isAdult);           // true

const activeUsers = await User.findActiveUsers();
```

#### Step 5 — CRUD Operations with Mongoose

```javascript
const User = require('./models/User');

// ── CREATE ──────────────────────────────────────────

const alice = await User.create({
  name: 'Alice',
  email: 'alice@email.com',
  age: 25,
});

const bob = new User({ name: 'Bob', email: 'bob@email.com', age: 30 });
await bob.save();

// ── READ ────────────────────────────────────────────

const allUsers = await User.find();
const adults   = await User.find({ age: { $gte: 18 } });
const alice    = await User.findOne({ email: 'alice@email.com' });
const userById = await User.findById('64abc123def456');

// Chaining
const users = await User
  .find({ isActive: true })
  .sort({ name: 1 })         // 1 = ascending, -1 = descending
  .limit(10)
  .skip(20)
  .select('name email age'); // only return these fields

// ── UPDATE ──────────────────────────────────────────

// Option 1: find → modify → save (triggers validators + middleware)
const alice = await User.findOne({ name: 'Alice' });
alice.age = 26;
await alice.save();

// Option 2: findByIdAndUpdate
const updated = await User.findByIdAndUpdate(
  '64abc123',
  { $set: { age: 26 } },
  { new: true, runValidators: true }
  // new: true → return updated doc, not the old one
);

// Option 3: bulk update
await User.updateMany({ isActive: false }, { $set: { role: 'inactive' } });

// ── DELETE ──────────────────────────────────────────

await User.findByIdAndDelete('64abc123');
await User.deleteOne({ name: 'Alice' });
await User.deleteMany({ isActive: false });
```

#### Mongoose Middleware (Hooks)

```javascript
const bcrypt = require('bcrypt');

// pre('save') — runs BEFORE a document is saved
userSchema.pre('save', async function (next) {
  if (!this.isModified('password')) return next();
  this.password = await bcrypt.hash(this.password, 10);
  next();
});

// post('save') — runs AFTER a document is saved
userSchema.post('save', function (doc) {
  console.log(`New user saved: ${doc.email}`);
});

// pre('find') — auto-filter inactive users on every query
userSchema.pre(/^find/, function (next) {
  this.where({ isActive: true });
  next();
});
```

---

### Prisma — Modern ORM (Bonus)

Prisma is a newer, increasingly popular ORM for PostgreSQL, MySQL, and MongoDB. It uses a **schema file** instead of defining models in JavaScript.

```bash
npm install prisma @prisma/client
npx prisma init
```

```prisma
// prisma/schema.prisma
model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  age       Int?
  posts     Post[]
  createdAt DateTime @default(now())
}

model Post {
  id       Int    @id @default(autoincrement())
  title    String
  author   User   @relation(fields: [authorId], references: [id])
  authorId Int
}
```

```javascript
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

const user = await prisma.user.create({
  data: { name: 'Alice', email: 'alice@email.com', age: 25 },
});

const users = await prisma.user.findMany({
  where: { age: { gt: 18 } },
  orderBy: { name: 'asc' },
});

await prisma.user.update({ where: { id: 1 }, data: { age: 26 } });
await prisma.user.delete({ where: { id: 1 } });
```

> 💡 Prisma gives you **full TypeScript autocomplete** — your editor knows the exact shape of every model.

### Which ORM/ODM Should You Use?

| Situation | Recommendation |
|-----------|---------------|
| Learning PostgreSQL with Node.js | **Sequelize** — widely taught, lots of resources |
| Learning MongoDB with Node.js | **Mongoose** — the industry standard ODM |
| Production PostgreSQL app (especially TypeScript) | **Prisma** — best developer experience |
| Need raw performance and full control | Write raw SQL/queries directly |

---

## 7. Querying Data

### SQL Queries (PostgreSQL)

```sql
-- Select specific columns
SELECT name, email FROM users;

-- Filter with WHERE
SELECT * FROM users WHERE age > 18;
SELECT * FROM users WHERE name = 'Alice' AND age < 30;
SELECT * FROM users WHERE name = 'Alice' OR name = 'Bob';

-- Sort results
SELECT * FROM users ORDER BY age ASC;    -- ascending
SELECT * FROM users ORDER BY age DESC;   -- descending

-- Limit & Offset (pagination)
SELECT * FROM users LIMIT 10 OFFSET 20; -- page 3 of 10 per page

-- Count & Aggregate functions
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users;
SELECT MAX(age), MIN(age) FROM users;
SELECT SUM(price) FROM orders;

-- Pattern matching with LIKE
SELECT * FROM users WHERE name LIKE 'A%';           -- starts with A
SELECT * FROM users WHERE email LIKE '%@gmail.com'; -- gmail users
SELECT * FROM users WHERE name LIKE '%ali%';        -- contains 'ali'

-- NULL checks
SELECT * FROM users WHERE age IS NULL;
SELECT * FROM users WHERE age IS NOT NULL;

-- GROUP BY — group rows and aggregate
SELECT role, COUNT(*) as total
FROM users
GROUP BY role;
-- Returns: admin→5, user→120, moderator→8

-- HAVING — filter groups (like WHERE but for GROUP BY)
SELECT role, COUNT(*) as total
FROM users
GROUP BY role
HAVING COUNT(*) > 10;

-- JOIN (covered in depth in Relationships section)
SELECT users.name, orders.product
FROM users
JOIN orders ON users.id = orders.user_id;
```

### MongoDB Queries (MQL)

```javascript
// Basic find
db.users.find({})                              // all documents
db.users.find({ age: 25 })                     // exact match
db.users.find({ age: { $gt: 18 } })            // greater than
db.users.find({ age: { $gte: 18, $lte: 30 } }) // range

// Comparison operators
// $gt = greater than        $lt = less than
// $gte = greater or equal   $lte = less or equal
// $ne = not equal           $in = in array

db.users.find({ name: { $in: ['Alice', 'Bob'] } })
db.users.find({ name: { $nin: ['Charlie'] } })

// Logical operators
db.users.find({ $and: [{ age: { $gt: 18 } }, { name: 'Alice' }] })
db.users.find({ $or: [{ name: 'Alice' }, { name: 'Bob' }] })
db.users.find({ age: { $not: { $lt: 18 } } })

// Sorting
db.users.find().sort({ age: 1 })   // ascending
db.users.find().sort({ age: -1 })  // descending

// Limit & skip (pagination)
db.users.find().limit(10).skip(20)

// Count
db.users.countDocuments({ age: { $gt: 18 } })

// Projection — select specific fields
db.users.find({}, { name: 1, email: 1, _id: 0 })
// 1 = include, 0 = exclude

// Regex search
db.users.find({ name: { $regex: /^ali/i } })  // starts with 'ali' (case-insensitive)

// Check if field exists
db.users.find({ phone: { $exists: true } })
```

### Querying with Mongoose in Node.js

```javascript
// Find all
const users = await User.find();

// Find with filter
const adults = await User.find({ age: { $gt: 18 } });

// Find one by field
const alice = await User.findOne({ name: 'Alice' });

// Find by MongoDB ID
const user = await User.findById('64abc123');

// Chaining multiple options
const users = await User
  .find({ age: { $gt: 18 } })
  .sort({ name: 1 })
  .limit(10)
  .skip(0)
  .select('name email');

// Count matching documents
const count = await User.countDocuments({ isActive: true });

// Check if any document matches
const exists = await User.exists({ email: 'alice@email.com' });
// returns the _id if found, null if not
```

### Querying with Sequelize in Node.js

```javascript
const { Op } = require('sequelize');

// Find all with filters, sorting, pagination
const users = await User.findAll({
  where: { age: { [Op.gt]: 18 }, isActive: true },
  order: [['name', 'ASC']],
  limit: 10,
  offset: 0,
  attributes: ['id', 'name', 'email'],  // select specific columns
});

// Count
const total = await User.count({ where: { isActive: true } });

// Find and count together (useful for pagination)
const { count, rows } = await User.findAndCountAll({
  where: { isActive: true },
  limit: 10,
  offset: 0,
});
// count = total matching records, rows = current page records

// Raw SQL query when you need full control
const [results] = await sequelize.query(
  'SELECT * FROM users WHERE age > :age',
  { replacements: { age: 18 } }
);
```

### Pagination Pattern (used in real apps)

```javascript
// GET /api/users?page=2&limit=10
app.get('/api/users', async (req, res) => {
  const page  = parseInt(req.query.page)  || 1;
  const limit = parseInt(req.query.limit) || 10;
  const skip  = (page - 1) * limit;

  // MongoDB / Mongoose
  const total = await User.countDocuments();
  const users = await User.find().skip(skip).limit(limit);

  res.json({
    data: users,
    pagination: {
      total,
      page,
      limit,
      totalPages: Math.ceil(total / limit),
    },
  });
});
```

---

## 8. Relationships

Relationships define how different pieces of data are connected to each other.

### Types of Relationships

| Type | Real-world example | Database example |
|------|--------------------|-----------------|
| **One-to-One** | One user has one passport | `users` → `profiles` |
| **One-to-Many** | One user writes many posts | `users` → `posts` |
| **Many-to-Many** | Many students take many courses | `students` ↔ `courses` via `enrollments` |

---

### Relationships in PostgreSQL (SQL)

Use **Foreign Keys** to link tables.

```sql
-- Users table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);

-- One-to-One: each user has one profile
CREATE TABLE profiles (
  id SERIAL PRIMARY KEY,
  bio TEXT,
  avatar_url VARCHAR(255),
  user_id INT UNIQUE REFERENCES users(id) ON DELETE CASCADE
  -- UNIQUE enforces one-to-one (no two profiles for same user)
  -- ON DELETE CASCADE: if user is deleted, profile is deleted too
);

-- One-to-Many: each post belongs to one user
CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  title VARCHAR(200) NOT NULL,
  body TEXT,
  user_id INT REFERENCES users(id) ON DELETE CASCADE
);
```

#### ON DELETE options

```sql
ON DELETE CASCADE     -- delete child records when parent is deleted
ON DELETE SET NULL    -- set foreign key to NULL when parent is deleted
ON DELETE RESTRICT    -- prevent deleting parent if children exist (default)
```

#### JOIN — combining tables in queries

```sql
-- INNER JOIN — only rows that match in BOTH tables
SELECT posts.title, users.name AS author
FROM posts
INNER JOIN users ON posts.user_id = users.id;

-- LEFT JOIN — all posts, even if the user was deleted
SELECT posts.title, users.name AS author
FROM posts
LEFT JOIN users ON posts.user_id = users.id;

-- Multiple JOINs
SELECT
  posts.title,
  users.name AS author,
  COUNT(comments.id) AS comment_count
FROM posts
JOIN users ON posts.user_id = users.id
LEFT JOIN comments ON comments.post_id = posts.id
GROUP BY posts.id, users.name;
```

#### Many-to-Many — requires a junction table

```sql
CREATE TABLE students (id SERIAL PRIMARY KEY, name VARCHAR(100));
CREATE TABLE courses  (id SERIAL PRIMARY KEY, title VARCHAR(100));

-- Junction table: links students and courses
CREATE TABLE enrollments (
  student_id INT REFERENCES students(id) ON DELETE CASCADE,
  course_id  INT REFERENCES courses(id)  ON DELETE CASCADE,
  enrolled_at TIMESTAMP DEFAULT NOW(),
  PRIMARY KEY (student_id, course_id)  -- composite PK prevents duplicates
);

-- Find all courses for a specific student
SELECT courses.title, enrollments.enrolled_at
FROM enrollments
JOIN courses ON enrollments.course_id = courses.id
WHERE enrollments.student_id = 1;

-- Find all students in a specific course
SELECT students.name
FROM enrollments
JOIN students ON enrollments.student_id = students.id
WHERE enrollments.course_id = 3;
```

#### Relationships with Sequelize

```javascript
const User = require('./models/User');
const Post = require('./models/Post');

// Define the association
User.hasMany(Post, { foreignKey: 'user_id', onDelete: 'CASCADE' });
Post.belongsTo(User, { foreignKey: 'user_id' });

// Many-to-Many
Student.belongsToMany(Course, { through: 'enrollments' });
Course.belongsToMany(Student, { through: 'enrollments' });

// Query with associated data (like JOIN)
const users = await User.findAll({
  include: [{ model: Post }],   // fetch users WITH their posts
});

// users[0].Posts is an array of that user's posts

// Nested includes
const users = await User.findAll({
  include: [{
    model: Post,
    include: [{ model: Comment }]
  }]
});
```

---

### Relationships in MongoDB (with Mongoose)

Two approaches: **Referencing** (like foreign keys) or **Embedding** (nesting documents).

#### Referencing — like SQL foreign keys

```javascript
const userSchema = new mongoose.Schema({ name: String, email: String });

const postSchema = new mongoose.Schema({
  title: String,
  body: String,
  author: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',   // tells Mongoose where to look when populating
    required: true,
  },
  comments: [{
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Comment',
  }],
});

const User = mongoose.model('User', userSchema);
const Post = mongoose.model('Post', postSchema);

// Create a post linked to a user
const user = await User.findOne({ name: 'Alice' });
await Post.create({ title: 'My Post', body: '...', author: user._id });

// Populate — replaces the ObjectId with the actual document
const posts = await Post.find().populate('author', 'name email');
// posts[0].author is now { name: 'Alice', email: '...' } — not just an ID

// Nested populate
const posts = await Post.find()
  .populate('author', 'name')
  .populate({ path: 'comments', populate: { path: 'author', select: 'name' } });
```

#### Embedding — nest documents inside documents

```javascript
// Good when data is small and always loaded with the parent
const userSchema = new mongoose.Schema({
  name: String,
  address: {              // embedded sub-document
    street: String,
    city: String,
    country: String,
  },
  socialLinks: {
    twitter: String,
    github: String,
  },
  hobbies: [String],      // array of strings
  recentSearches: [{      // array of sub-documents
    query: String,
    searchedAt: { type: Date, default: Date.now },
  }],
});
```

#### Embedding vs Referencing — Decision Guide

| Situation | Use |
|-----------|-----|
| Data is small and always needed together | **Embed** |
| Data changes independently | **Reference** |
| Data is shared between multiple documents | **Reference** |
| You rarely need the related data | **Reference** |
| Data is large (could exceed 16MB doc limit) | **Reference** |
| You need to query the nested data directly | **Reference** |

> 🧠 **Rule of thumb:** A user's `address` → embed it. A user's `posts` (could be thousands) → reference them.

---

## 9. Database Design Basics

Good database design saves you from enormous headaches later. Here are the core principles.

### Normalization — Removing Duplication

**Bad design (denormalized):**

```
orders table
| order_id | customer_name | customer_email | product |
|----------|--------------|----------------|---------|
| 1        | Alice         | a@x.com        | Laptop  |
| 2        | Alice         | a@x.com        | Mouse   |
```

Alice's email is repeated. If she changes her email, you must update every row — miss one and your data is inconsistent.

**Good design (normalized):**

```
users                          orders
id | name  | email             id | user_id | product
1  | Alice | a@x.com           1  | 1       | Laptop
                               2  | 1       | Mouse
```

Now Alice's email is stored once. Update it once, it's updated everywhere.

### The Three Normal Forms (Simplified)

| Form | Rule | What it prevents |
|------|------|-----------------|
| **1NF** | Each column has one value (no lists in a column) | `hobbies = "coding, reading"` |
| **2NF** | Every non-key column depends on the full primary key | Partial dependencies |
| **3NF** | No column depends on another non-key column | Transitive dependencies |

> 💡 For most beginner projects, just aim for 1NF and 2NF. Full 3NF is a bonus.

### Primary Keys & Foreign Keys

```sql
-- Primary Key: unique identifier for each row
CREATE TABLE users (
  id SERIAL PRIMARY KEY,   -- auto-assigned, unique, never null
  email VARCHAR(150) UNIQUE NOT NULL
);

-- Foreign Key: points to another table's primary key
CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  user_id INT NOT NULL REFERENCES users(id)   -- foreign key
);
```

- **Primary Key** — Never null, never duplicated. Every table should have one.
- **Foreign Key** — Creates the relationship between tables. Can be null (optional relationship).

### Indexes — Making Queries Fast

Without an index, a query scans **every single row** (called a full table scan). On a table with 1 million users, that is slow.

```sql
-- Slow without index (scans all rows)
SELECT * FROM users WHERE email = 'alice@email.com';

-- Add an index on the email column
CREATE INDEX idx_users_email ON users(email);

-- Now the query jumps straight to Alice's row

-- Index on foreign key (very important for JOIN performance)
CREATE INDEX idx_posts_user_id ON posts(user_id);

-- Composite index (for queries that filter by two columns together)
CREATE INDEX idx_orders_user_status ON orders(user_id, status);
-- Fast for: WHERE user_id = 1 AND status = 'pending'
```

> 💡 **Always index:** foreign key columns, columns in WHERE clauses, columns in ORDER BY.
> ⚠️ **Don't over-index:** every index slows down INSERT/UPDATE/DELETE slightly — the database must update the index too.

### Naming Conventions

```sql
-- Tables: lowercase, plural, snake_case
users, blog_posts, order_items, password_reset_tokens

-- Columns: lowercase, snake_case
user_id, created_at, first_name, is_active

-- Primary key: always "id"
-- Foreign key: always "<singular_table_name>_id"
user_id, product_id, category_id

-- Indexes: idx_<table>_<column>
idx_users_email, idx_posts_user_id
```

### Data Types — Choosing the Right Type

| Data | PostgreSQL | MongoDB (Mongoose) |
|------|------------|--------------------|
| Short text | `VARCHAR(255)` | `String` |
| Long text | `TEXT` | `String` |
| Whole number | `INT` or `BIGINT` | `Number` |
| Precise decimal (money) | `DECIMAL(10,2)` | `Number` |
| True/False | `BOOLEAN` | `Boolean` |
| Date & time | `TIMESTAMP` | `Date` |
| Date only | `DATE` | `Date` |
| Auto-increment ID | `SERIAL` | `ObjectId` (auto) |
| JSON data | `JSONB` | Native (any shape) |
| Array | `INT[]` or junction table | `[String]` or `[{ ... }]` |

### Entity Relationship Diagram (ERD)

Before writing code, sketch your tables and their relationships. This is called an **ERD**.

```
[users]                [posts]                [comments]
┌──────────────┐      ┌──────────────┐       ┌──────────────┐
│ id (PK)      │──┐   │ id (PK)      │──┐    │ id (PK)      │
│ name         │  └──▶│ user_id (FK) │  └───▶│ post_id (FK) │
│ email        │      │ title        │        │ body         │
│ created_at   │      │ body         │        │ user_id (FK) │──▶[users]
└──────────────┘      └──────────────┘        └──────────────┘
```

```
[students]           [enrollments]          [courses]
┌──────────┐        ┌──────────────┐       ┌──────────┐
│ id (PK)  │──────▶ │ student_id   │       │ id (PK)  │
│ name     │        │ course_id    │◀───── │ title    │
└──────────┘        │ enrolled_at  │       └──────────┘
                    └──────────────┘
```

### Database Design Process

```
1. Identify your entities
   (What "things" does your app have? Users, Products, Orders...)

2. List the attributes of each entity
   (What data does each thing have? User: name, email, age...)

3. Identify relationships
   (How do they connect? User has many Orders...)

4. Choose primary and foreign keys
5. Draw the ERD
6. Normalize — remove duplication
7. THEN write your code
```

---

## 10. Migrations & Seeding

### What is a Migration?

A migration is a versioned file that describes a change to your database schema. Instead of manually running SQL commands on each server, you write migration files that can be run (and undone) automatically.

```
Without migrations:                  With migrations:
"Did you add the new               migrations/
  phone column to staging?"           001_create_users.js
"I forgot..."                         002_add_phone_to_users.js
"It's broken in production!"          003_create_posts.js
                                    → everyone runs: npm run migrate
```

### Migrations with Sequelize

```bash
# Install the CLI
npm install --save-dev sequelize-cli

# Initialize (creates folders)
npx sequelize-cli init
```

```javascript
// migrations/20240101_create_users.js
module.exports = {
  up: async (queryInterface, Sequelize) => {
    // 'up' = apply the migration
    await queryInterface.createTable('users', {
      id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true,
      },
      name:  { type: Sequelize.STRING,  allowNull: false },
      email: { type: Sequelize.STRING,  allowNull: false, unique: true },
      created_at: { type: Sequelize.DATE, defaultValue: Sequelize.NOW },
    });
  },

  down: async (queryInterface) => {
    // 'down' = undo the migration
    await queryInterface.dropTable('users');
  },
};
```

```bash
# Run all pending migrations
npx sequelize-cli db:migrate

# Undo the last migration
npx sequelize-cli db:migrate:undo

# Undo all migrations
npx sequelize-cli db:migrate:undo:all
```

### Seeding — Filling the Database with Test Data

```javascript
// seeders/20240101_seed_users.js
module.exports = {
  up: async (queryInterface) => {
    await queryInterface.bulkInsert('users', [
      { name: 'Alice', email: 'alice@email.com', created_at: new Date() },
      { name: 'Bob',   email: 'bob@email.com',   created_at: new Date() },
    ]);
  },

  down: async (queryInterface) => {
    await queryInterface.bulkDelete('users', null, {});
  },
};
```

```bash
# Run all seeders
npx sequelize-cli db:seed:all

# Undo all seeders
npx sequelize-cli db:seed:undo:all
```

### Seeding with Mongoose

```javascript
// seeds/users.js
const mongoose = require('mongoose');
const User = require('../models/User');
require('dotenv').config();

const seedUsers = [
  { name: 'Alice', email: 'alice@email.com', age: 25 },
  { name: 'Bob',   email: 'bob@email.com',   age: 30 },
];

async function seed() {
  await mongoose.connect(process.env.MONGO_URI);
  await User.deleteMany({});          // clear existing data
  await User.insertMany(seedUsers);   // insert seed data
  console.log('✅ Database seeded!');
  await mongoose.disconnect();
}

seed();
```

```bash
node seeds/users.js
```

---

## 11. Transactions

### What is a Transaction?

A transaction is a group of database operations that must **all succeed or all fail together**. If one step fails, everything is rolled back as if nothing happened.

The classic example — transferring money:

```
Transfer $100 from Alice to Bob:
  Step 1: Deduct $100 from Alice
  Step 2: Add $100 to Bob

What if the server crashes after Step 1 but before Step 2?
→ Alice lost $100, Bob got nothing ❌

With a transaction:
→ Both steps succeed ✅ OR both are undone ✅ — never a partial result
```

### Transactions in PostgreSQL

```javascript
const pool = require('./db');

async function transferMoney(fromUserId, toUserId, amount) {
  const client = await pool.connect();

  try {
    await client.query('BEGIN');   // start the transaction

    await client.query(
      'UPDATE accounts SET balance = balance - $1 WHERE user_id = $2',
      [amount, fromUserId]
    );

    await client.query(
      'UPDATE accounts SET balance = balance + $1 WHERE user_id = $2',
      [amount, toUserId]
    );

    await client.query('COMMIT');  // save all changes
    console.log('✅ Transfer complete');

  } catch (error) {
    await client.query('ROLLBACK'); // undo all changes
    console.error('❌ Transfer failed, rolled back:', error.message);
    throw error;

  } finally {
    client.release(); // always return the client to the pool
  }
}
```

### Transactions with Sequelize

```javascript
const sequelize = require('./database');

async function createOrderWithItems(userId, items) {
  const t = await sequelize.transaction();

  try {
    const order = await Order.create({ userId, status: 'pending' }, { transaction: t });

    for (const item of items) {
      await OrderItem.create({
        orderId: order.id,
        productId: item.productId,
        quantity: item.quantity,
      }, { transaction: t });

      // Deduct stock
      await Product.decrement('stock', {
        by: item.quantity,
        where: { id: item.productId },
        transaction: t,
      });
    }

    await t.commit();
    return order;

  } catch (error) {
    await t.rollback();
    throw error;
  }
}
```

### Transactions with Mongoose

```javascript
const mongoose = require('mongoose');

async function transferPoints(fromUserId, toUserId, points) {
  const session = await mongoose.startSession();
  session.startTransaction();

  try {
    await User.updateOne(
      { _id: fromUserId },
      { $inc: { points: -points } },
      { session }
    );

    await User.updateOne(
      { _id: toUserId },
      { $inc: { points: +points } },
      { session }
    );

    await session.commitTransaction();
    console.log('✅ Points transferred');

  } catch (error) {
    await session.abortTransaction();
    throw error;

  } finally {
    session.endSession();
  }
}
```

> ⚠️ MongoDB transactions require a **Replica Set** or Atlas cluster. They don't work on a standalone local MongoDB.

---

## 12. Practice Projects

### 🟢 Beginner
- **User Directory** — CRUD app with a `users` table/collection
- **Todo List API** — Express.js + MongoDB with Mongoose; todos belong to users
- **Notes App** — PostgreSQL + Sequelize; users create and manage notes

### 🟡 Intermediate
- **Blog API** — PostgreSQL + Sequelize; users, posts, and comments with full relationships and JOINs
- **Product Catalog** — MongoDB + Mongoose; products with categories (embedded) and reviews (referenced)
- **URL Shortener** — PostgreSQL; links table with click tracking and indexes

### 🔴 Advanced
- **E-Commerce Backend** — PostgreSQL; users, products, orders, order_items with full normalization, transactions, and migrations
- **Social Feed** — MongoDB; users follow others (many-to-many), posts, likes, comments with pagination
- **Multi-tenant SaaS** — PostgreSQL; organizations, members, roles, permissions with row-level security

---

## 📦 Useful Packages Summary

| Purpose | Package | Install |
|---------|---------|---------|
| PostgreSQL driver | `pg` | `npm install pg` |
| PostgreSQL ORM | `sequelize` | `npm install sequelize pg pg-hstore` |
| Sequelize CLI (migrations) | `sequelize-cli` | `npm install --save-dev sequelize-cli` |
| Modern ORM (TypeScript-friendly) | `prisma` | `npm install prisma @prisma/client` |
| MongoDB ODM | `mongoose` | `npm install mongoose` |
| Environment variables | `dotenv` | `npm install dotenv` |
| Input validation | `express-validator` | `npm install express-validator` |
| PostgreSQL GUI | pgAdmin | [pgadmin.org](https://www.pgadmin.org) |
| MongoDB GUI | MongoDB Compass | [mongodb.com/compass](https://www.mongodb.com/products/compass) |
| DB diagram tool | dbdiagram.io | [dbdiagram.io](https://dbdiagram.io) |

---

## 🧠 Key Takeaways

- Use **PostgreSQL** for structured, relational, transactional data. Use **MongoDB** for flexible, document-based, or rapidly changing data.
- Always use **environment variables** for credentials — never hardcode passwords in your code.
- **ORM/ODM** (Sequelize / Mongoose / Prisma) lets you work with DB records as JavaScript objects — cleaner, safer, and faster to write than raw queries.
- Always use **parameterized queries** with raw SQL to prevent SQL Injection attacks.
- Design your database **before** writing code — sketch the ERD first.
- **Normalize** SQL data to remove duplication. In MongoDB, decide between **embedding** vs **referencing** based on how you access the data.
- Add **indexes** on columns you search or filter by frequently — they make queries dramatically faster.
- Use **migrations** to version-control your database schema changes.
- Use **transactions** when multiple database operations must all succeed or all fail together.

---

*Happy coding! 🚀 If you have questions on any section, open an issue or discussion.*