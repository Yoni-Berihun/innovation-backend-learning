# 🧪 Testing in MERN Stack — Complete Beginner's Guide

> A step-by-step module for absolute beginners on how to write **Unit**, **API**, **Integration**, and **Error** tests in a MERN (MongoDB, Express, React, Node.js) application.

---

## 📚 Table of Contents

1. [What is Testing and Why Does it Matter?](#-what-is-testing-and-why-does-it-matter)
2. [Tools You Will Use](#-tools-you-will-use)
3. [Project Setup](#-project-setup)
4. [Unit Testing](#1--unit-testing)
5. [API Testing](#2--api-testing)
6. [Integration Testing](#3--integration-testing)
7. [Error Testing](#4--error-testing)
8. [Recommended Folder Structure](#5--recommended-folder-structure)
9. [Running Your Tests](#6--running-your-tests)
10. [Mental Model — What to Test and When](#7--mental-model--what-to-test-and-when)

---

## 🧠 What is Testing and Why Does it Matter?

Imagine you build a login feature. You manually click the login button, type a username and password, and it works. 

Two weeks later, a teammate changes something in the database code — and now login silently breaks. Nobody notices until a real user complains.

**Testing is writing code that automatically checks your other code.**

Every time something changes in your project, you run your tests and they instantly tell you whether something broke — without you needing to manually click through every feature.

### In a MERN app, you have four layers that all need testing:

| Layer | What it does |
|---|---|
| **React Frontend** | What users see and interact with |
| **Node + Express Backend** | Handles business logic and routes |
| **MongoDB Database** | Stores your application data |
| **REST API** | Connects the frontend to the backend |

---

## 🛠 Tools You Will Use

| Tool | Purpose |
|---|---|
| [Jest](https://jestjs.io/) | The main test runner — works for both frontend and backend |
| [Supertest](https://github.com/visionmedia/supertest) | Sends HTTP requests to your Express app during tests |
| [React Testing Library](https://testing-library.com/react) | Tests React components in a realistic way |
| [mongodb-memory-server](https://github.com/nodkz/mongodb-memory-server) | Runs a real MongoDB instance in memory — no real DB needed during tests |

---

## ⚙️ Project Setup

### Install testing dependencies (Backend)

```bash
npm install --save-dev jest supertest mongodb-memory-server
```

### Install testing dependencies (Frontend — if using Create React App, Jest is already included)

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

### Add test script to `package.json`

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

### Separate your app from your server startup

> ⚠️ This is the most important setup step. Supertest needs to import your Express app **without** it starting a real server on a port.

```js
// app.js — just the Express app, no listen()
const express = require('express');
const app = express();
app.use(express.json());

const userRoutes = require('./routes/userRoutes');
const authRoutes = require('./routes/authRoutes');

app.use('/api/users', userRoutes);
app.use('/api/auth', authRoutes);

module.exports = app; // ← export the app
```

```js
// server.js — this is where the server actually starts
const app = require('./app');
app.listen(5000, () => console.log('Server running on port 5000'));
```

---

## 1. 🔬 Unit Testing

### What is it?

A **unit test** tests the **smallest possible piece of your code in isolation** — usually a single function.

It does NOT:
- Connect to a database
- Make HTTP requests
- Render any UI

It only checks: *"Given input X, does my function return output Y?"*

### Why isolation matters

If your function depends on a database and the test fails, you won't know if the **function logic** is broken or if the **database** is down. Isolation removes that guesswork.

---

### Backend Unit Test Example

Let's say you have a utility function that calculates a user's age:

```js
// utils/calculateAge.js
function calculateAge(birthYear) {
  const currentYear = new Date().getFullYear();
  return currentYear - birthYear;
}

module.exports = calculateAge;
```

Now write a test file right next to it:

```js
// utils/calculateAge.test.js
const calculateAge = require('./calculateAge');

describe('calculateAge', () => {

  test('returns correct age for a given birth year', () => {
    const age = calculateAge(2000);
    expect(age).toBe(new Date().getFullYear() - 2000);
  });

  test('returns 0 if birth year is the current year', () => {
    const currentYear = new Date().getFullYear();
    expect(calculateAge(currentYear)).toBe(0);
  });

});
```

### Breaking down the syntax

| Keyword | What it does |
|---|---|
| `describe('label', fn)` | Groups related tests together under a label |
| `test('description', fn)` | Defines a single test case |
| `expect(value)` | Starts an assertion — "I expect this value..." |
| `.toBe(expected)` | "...to equal this exactly" |
| `.toHaveProperty('key')` | Checks if an object has a certain property |
| `.not.toBe(value)` | Asserts the value is NOT equal to something |

---

### Frontend Unit Test Example

Testing a simple React component:

```jsx
// components/Greeting.jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default Greeting;
```

```jsx
// components/Greeting.test.jsx
import { render, screen } from '@testing-library/react';
import Greeting from './Greeting';

test('renders the correct greeting message', () => {
  render(<Greeting name="Egata" />);

  const heading = screen.getByText('Hello, Egata!');

  expect(heading).toBeInTheDocument();
});
```

**Key concepts:**

- `render(...)` — mounts the component into a virtual DOM (no real browser needed)
- `screen.getByText(...)` — finds an element by its visible text content
- `toBeInTheDocument()` — asserts the element actually exists on the page

---

## 2. 🌐 API Testing

### What is it?

API testing sends **real HTTP requests** to your Express routes and checks that the responses are correct — the right status code, the right response body, and the right behavior.

> For backend developers, this is the most important type of testing to learn first.

---

### Writing API Tests with Supertest

```js
// tests/api/user.test.js
const request = require('supertest');
const app = require('../../app');
const mongoose = require('mongoose');
const { MongoMemoryServer } = require('mongodb-memory-server');

let mongoServer;

// Runs ONCE before all tests in this file — set up the in-memory database
beforeAll(async () => {
  mongoServer = await MongoMemoryServer.create();
  await mongoose.connect(mongoServer.getUri());
});

// Runs ONCE after all tests — disconnect and stop the database
afterAll(async () => {
  await mongoose.disconnect();
  await mongoServer.stop();
});

// Runs after EACH test — clears the database so tests don't affect each other
afterEach(async () => {
  const collections = mongoose.connection.collections;
  for (const key in collections) {
    await collections[key].deleteMany({});
  }
});

// ─────────────────────────────────────────────
// TEST CASES
// ─────────────────────────────────────────────

describe('POST /api/users/register', () => {

  test('should register a new user and return 201', async () => {
    const response = await request(app)
      .post('/api/users/register')
      .send({
        name: 'Egata',
        email: 'egata@example.com',
        password: 'secret123'
      });

    expect(response.statusCode).toBe(201);
    expect(response.body).toHaveProperty('_id');
    expect(response.body.email).toBe('egata@example.com');
    expect(response.body).not.toHaveProperty('password'); // password should never be returned
  });

  test('should return 400 if email is missing', async () => {
    const response = await request(app)
      .post('/api/users/register')
      .send({
        name: 'Egata',
        password: 'secret123'
        // no email field!
      });

    expect(response.statusCode).toBe(400);
    expect(response.body).toHaveProperty('error');
  });

});

describe('GET /api/users/:id', () => {

  test('should return a user by their ID', async () => {
    // Step 1: Create the user first
    const createRes = await request(app)
      .post('/api/users/register')
      .send({ name: 'Egata', email: 'egata@example.com', password: 'secret123' });

    const userId = createRes.body._id;

    // Step 2: Fetch the user by ID
    const getRes = await request(app).get(`/api/users/${userId}`);

    expect(getRes.statusCode).toBe(200);
    expect(getRes.body.name).toBe('Egata');
  });

  test('should return 404 for a non-existent user ID', async () => {
    const fakeId = new mongoose.Types.ObjectId();
    const response = await request(app).get(`/api/users/${fakeId}`);
    expect(response.statusCode).toBe(404);
  });

});
```

### What the lifecycle hooks do

```
beforeAll  →  Runs once before any test in this file starts
afterAll   →  Runs once after all tests in this file finish
beforeEach →  Runs before every single test
afterEach  →  Runs after every single test (great for cleanup)
```

---

## 3. 🔗 Integration Testing

### What is it?

Integration testing checks that **multiple parts of your system work correctly together**.

Where unit tests isolate a single function and API tests check a single endpoint, integration tests verify the full chain:

```
HTTP Request → Express Route → Controller → Service → MongoDB → Response
```

The goal is to catch bugs that only appear when pieces **connect**. For example: your user creation function works fine alone, but when it connects to the Mongoose model, the password hashing middleware silently fails.

---

### Integration Test Example — Full Authentication Flow

```js
// tests/integration/auth.integration.test.js
const request = require('supertest');
const app = require('../../app');
const mongoose = require('mongoose');
const { MongoMemoryServer } = require('mongodb-memory-server');
const User = require('../../models/User');

let mongoServer;

beforeAll(async () => {
  mongoServer = await MongoMemoryServer.create();
  await mongoose.connect(mongoServer.getUri());
});

afterAll(async () => {
  await mongoose.disconnect();
  await mongoServer.stop();
});

afterEach(async () => {
  await User.deleteMany({});
});

describe('Full Auth Flow Integration', () => {

  test('user can register, then login with the same credentials', async () => {
    // STEP 1: Register a new user
    const registerRes = await request(app)
      .post('/api/auth/register')
      .send({ name: 'Egata', email: 'egata@test.com', password: 'pass1234' });

    expect(registerRes.statusCode).toBe(201);

    // STEP 2: Login with those same credentials
    const loginRes = await request(app)
      .post('/api/auth/login')
      .send({ email: 'egata@test.com', password: 'pass1234' });

    expect(loginRes.statusCode).toBe(200);
    expect(loginRes.body).toHaveProperty('token'); // JWT should be returned

    // STEP 3: Verify the user actually exists in the database
    const userInDb = await User.findOne({ email: 'egata@test.com' });
    expect(userInDb).not.toBeNull();
    expect(userInDb.name).toBe('Egata');

    // STEP 4: Confirm the password was hashed, not stored as plain text
    expect(userInDb.password).not.toBe('pass1234');
  });

  test('login fails after account is deleted from database', async () => {
    // Register a user
    await request(app)
      .post('/api/auth/register')
      .send({ name: 'Egata', email: 'egata@test.com', password: 'pass1234' });

    // Delete the user directly from the database (simulates admin deletion)
    await User.deleteOne({ email: 'egata@test.com' });

    // Attempt to login — should now fail
    const loginRes = await request(app)
      .post('/api/auth/login')
      .send({ email: 'egata@test.com', password: 'pass1234' });

    expect(loginRes.statusCode).toBe(401);
  });

  test('accessing a protected route with a valid token succeeds', async () => {
    // Register and login to get a token
    await request(app)
      .post('/api/auth/register')
      .send({ name: 'Egata', email: 'egata@test.com', password: 'pass1234' });

    const loginRes = await request(app)
      .post('/api/auth/login')
      .send({ email: 'egata@test.com', password: 'pass1234' });

    const token = loginRes.body.token;

    // Use the token to access a protected route
    const profileRes = await request(app)
      .get('/api/users/profile')
      .set('Authorization', `Bearer ${token}`);

    expect(profileRes.statusCode).toBe(200);
    expect(profileRes.body.email).toBe('egata@test.com');
  });

});
```

**The key difference from API testing:**
You are checking the **database state directly** alongside the HTTP response. You confirm that the full pipeline produced the correct side effects — not just that the API returned the right JSON.

---

## 4. ❌ Error Testing

### What is it?

Error testing specifically verifies that your app **handles bad situations gracefully**.

A well-tested app doesn't just work when everything goes right. It responds predictably when things go wrong — with the correct error message and the correct HTTP status code.

---

### HTTP Status Codes You Should Know

| Status Code | Meaning | When to use |
|---|---|---|
| `200` | OK | Request succeeded |
| `201` | Created | Resource was created successfully |
| `400` | Bad Request | The client sent invalid data |
| `401` | Unauthorized | Not logged in / no token |
| `403` | Forbidden | Logged in but not allowed |
| `404` | Not Found | Resource doesn't exist |
| `409` | Conflict | Duplicate data (e.g., email already taken) |
| `500` | Internal Server Error | Something crashed on the server |

---

### Error Test Examples

```js
// tests/error/errors.test.js
const request = require('supertest');
const app = require('../../app');
const mongoose = require('mongoose');
const { MongoMemoryServer } = require('mongodb-memory-server');
const User = require('../../models/User');

let mongoServer;

beforeAll(async () => {
  mongoServer = await MongoMemoryServer.create();
  await mongoose.connect(mongoServer.getUri());
});

afterAll(async () => {
  await mongoose.disconnect();
  await mongoServer.stop();
});

afterEach(async () => {
  await User.deleteMany({});
});

// ─────────────────────────────────────────────
// VALIDATION ERRORS (400)
// ─────────────────────────────────────────────

describe('Validation Errors — 400 Bad Request', () => {

  test('returns 400 when name field is missing', async () => {
    const res = await request(app)
      .post('/api/users/register')
      .send({ email: 'test@test.com', password: '123456' }); // no name!

    expect(res.statusCode).toBe(400);
    expect(res.body.error).toMatch(/name/i); // error message should mention "name"
  });

  test('returns 400 when password is too short', async () => {
    const res = await request(app)
      .post('/api/users/register')
      .send({ name: 'Egata', email: 'test@test.com', password: '123' }); // too short

    expect(res.statusCode).toBe(400);
  });

  test('returns 400 when email format is invalid', async () => {
    const res = await request(app)
      .post('/api/users/register')
      .send({ name: 'Egata', email: 'not-an-email', password: 'pass1234' });

    expect(res.statusCode).toBe(400);
    expect(res.body.error).toMatch(/email/i);
  });

});

// ─────────────────────────────────────────────
// CONFLICT ERRORS (409)
// ─────────────────────────────────────────────

describe('Conflict Errors — 409 Conflict', () => {

  test('returns 409 when registering with a duplicate email', async () => {
    const userData = { name: 'Egata', email: 'dupe@test.com', password: 'pass1234' };

    // First registration — should succeed
    await request(app).post('/api/users/register').send(userData);

    // Second registration with same email — should fail
    const res = await request(app).post('/api/users/register').send(userData);

    expect(res.statusCode).toBe(409);
    expect(res.body.error).toMatch(/already exists/i);
  });

});

// ─────────────────────────────────────────────
// AUTHENTICATION ERRORS (401)
// ─────────────────────────────────────────────

describe('Authentication Errors — 401 Unauthorized', () => {

  test('returns 401 when accessing a protected route with no token', async () => {
    const res = await request(app)
      .get('/api/users/profile');
      // No Authorization header!

    expect(res.statusCode).toBe(401);
  });

  test('returns 401 when token is fake or expired', async () => {
    const res = await request(app)
      .get('/api/users/profile')
      .set('Authorization', 'Bearer this.is.not.a.real.token');

    expect(res.statusCode).toBe(401);
  });

  test('returns 401 when login credentials are wrong', async () => {
    // Register a user
    await request(app)
      .post('/api/auth/register')
      .send({ name: 'Egata', email: 'egata@test.com', password: 'correctpassword' });

    // Try to login with wrong password
    const res = await request(app)
      .post('/api/auth/login')
      .send({ email: 'egata@test.com', password: 'wrongpassword' });

    expect(res.statusCode).toBe(401);
  });

});

// ─────────────────────────────────────────────
// NOT FOUND ERRORS (404)
// ─────────────────────────────────────────────

describe('Not Found Errors — 404 Not Found', () => {

  test('returns 404 for a non-existent user ID', async () => {
    const fakeId = new mongoose.Types.ObjectId();
    const res = await request(app).get(`/api/users/${fakeId}`);
    expect(res.statusCode).toBe(404);
  });

  test('returns 404 for completely unknown routes', async () => {
    const res = await request(app).get('/api/this-route-does-not-exist');
    expect(res.statusCode).toBe(404);
  });

});
```

---

## 5. 📁 Recommended Folder Structure

```
your-mern-project/
├── src/
│   ├── controllers/        ← Business logic
│   ├── models/             ← Mongoose schemas
│   ├── routes/             ← Express route definitions
│   ├── middleware/         ← Auth, error handlers, etc.
│   └── utils/              ← Pure helper functions
├── tests/
│   ├── unit/
│   │   ├── calculateAge.test.js
│   │   └── formatEmail.test.js
│   ├── api/
│   │   ├── user.test.js
│   │   └── auth.test.js
│   ├── integration/
│   │   └── auth.integration.test.js
│   └── error/
│       └── errors.test.js
├── app.js                  ← Express app (no listen)
├── server.js               ← Server startup
└── package.json
```

> 💡 **Tip:** Keep test files close to the code they test. For unit tests, placing `calculateAge.test.js` right next to `calculateAge.js` is a very common and clean convention.

---

## 6. 🏃 Running Your Tests

```bash
# Run all tests once
npm test

# Run tests and automatically re-run when files change (great during development)
npx jest --watch

# Run tests and generate a coverage report
# (Shows which lines of your code are NOT covered by tests)
npx jest --coverage

# Run a specific test file
npx jest tests/api/user.test.js

# Run tests matching a specific name
npx jest --testNamePattern="register"
```

### Understanding Coverage Reports

When you run `npx jest --coverage`, you'll see a table like this:

```
--------------------|---------|----------|---------|---------|
File                | % Stmts | % Branch | % Funcs | % Lines |
--------------------|---------|----------|---------|---------|
controllers/        |         |          |         |         |
  userController.js |   85.71 |    75.00 |   100   |   85.71 |
utils/              |         |          |         |         |
  calculateAge.js   |   100   |   100    |   100   |   100   |
--------------------|---------|----------|---------|---------|
```

- **Stmts** — What % of code statements were executed
- **Branch** — What % of if/else branches were tested
- **Funcs** — What % of functions were called
- **Lines** — What % of lines were covered

Aim for **80%+ coverage** as a beginner goal.

---

## 7. 🧭 Mental Model — What to Test and When

```
┌─────────────────┬──────────────────────────────────────────────────┐
│ Test Type       │ Ask yourself...                                  │
├─────────────────┼──────────────────────────────────────────────────┤
│ Unit Test       │ Is this a pure function I can test in isolation? │
│ API Test        │ Does this endpoint return the right response?    │
│ Integration     │ Does the full flow from request to DB work?      │
│ Error Test      │ What happens when the user sends bad data?       │
└─────────────────┴──────────────────────────────────────────────────┘
```

### A Good Rule of Thumb for Beginners

Start with **API tests** first. They give you the most coverage for the least setup, and they directly test the contract your frontend depends on. Once those are solid, add unit tests for complex utility functions and integration tests for your most critical user flows.

---

## 🎯 Quick Reference Cheat Sheet

```js
// Lifecycle hooks
beforeAll(async () => { /* runs once before all tests */ });
afterAll(async () => { /* runs once after all tests */ });
beforeEach(async () => { /* runs before each test */ });
afterEach(async () => { /* runs after each test */ });

// Common matchers
expect(value).toBe(expected)               // strict equality
expect(value).toEqual(expected)            // deep equality (objects/arrays)
expect(value).toHaveProperty('key')        // object has a property
expect(value).toMatch(/regex/)             // string matches pattern
expect(value).toBeNull()                   // value is null
expect(value).not.toBeNull()               // value is NOT null
expect(value).toBeGreaterThan(n)           // number comparison
expect(fn).toThrow()                       // function throws an error

// Supertest request methods
request(app).get('/route')
request(app).post('/route').send({ data })
request(app).put('/route').send({ data })
request(app).delete('/route')
request(app).get('/route').set('Authorization', `Bearer ${token}`)
```

---

## 📖 Further Reading

- [Jest Official Docs](https://jestjs.io/docs/getting-started)
- [Supertest on GitHub](https://github.com/visionmedia/supertest)
- [React Testing Library Docs](https://testing-library.com/docs/react-testing-library/intro/)
- [mongodb-memory-server Docs](https://github.com/nodkz/mongodb-memory-server)

---

> Made with ❤️ for MERN Stack learners. Happy testing!