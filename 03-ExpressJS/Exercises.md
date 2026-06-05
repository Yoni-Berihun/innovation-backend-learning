# 🟢 Module 03 — Express.js: Exercises & Answers

> 💡 **How to use this file:**
> - Run `npm init -y && npm install express` in a new folder before starting
> - Try each exercise yourself first — only open the answer after a genuine attempt
> - Every answer has a deep explanation of the *why*, not just the *what*

---

## 📖 Table of Contents

1. [Getting Started](#1--getting-started)
2. [Routing](#2--routing)
3. [Middleware](#3--middleware)
4. [Request & Response](#4--request--response)
5. [Validation & Error Handling](#5--validation--error-handling)
6. [Project Structure](#6--project-structure)
7. [Final Project — Books REST API](#7--final-project--books-rest-api)
8. [Interview Questions](#8--interview-questions)

---

## 1 — Getting Started

---

### 🟢 Easy

**Exercise 1.1 — Your first Express server**

Create an Express server that:
- Listens on port 3000
- Responds `"Hello, Express!"` to `GET /`
- Responds `"About page"` to `GET /about`
- Logs `"Server running on port 3000"` when started

<details>
<summary>✅ Answer & Explanation</summary>

```js
// app.js
const express = require('express');
const app     = express();

app.get('/', (req, res) => {
  res.send('Hello, Express!');
});

app.get('/about', (req, res) => {
  res.send('About page');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

**Explanation:**

`require('express')` imports the Express module. `express()` creates an application instance. `app.get(path, handler)` registers a route — when a GET request matches that path, the handler function runs.

`res.send()` sends a response. If you pass a string, Express automatically sets `Content-Type: text/html`. If you pass an object or array, it sets `Content-Type: application/json`.

`app.listen()` starts the HTTP server. The callback fires once the server is ready.

</details>

---

**Exercise 1.2 — JSON response**

Create an endpoint `GET /user` that responds with this JSON:
```json
{ "id": 1, "name": "Marta", "role": "student" }
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
app.get('/user', (req, res) => {
  res.json({ id: 1, name: 'Marta', role: 'student' });
});
```

**Explanation:**

`res.json()` automatically:
1. Converts the object to a JSON string
2. Sets `Content-Type: application/json`
3. Sends the response and closes the connection

This is the standard response method for REST APIs. Always use `res.json()` for API endpoints — never `res.send()` with a manually stringified object.

</details>

---

**Exercise 1.3 — Status codes**

Create these endpoints:
- `GET /ok` → status 200, `{ message: "All good" }`
- `GET /created` → status 201, `{ message: "Resource created" }`
- `GET /not-found` → status 404, `{ error: "Not found" }`
- `GET /server-error` → status 500, `{ error: "Internal error" }`

<details>
<summary>✅ Answer & Explanation</summary>

```js
app.get('/ok',           (req, res) => res.status(200).json({ message: 'All good' }));
app.get('/created',      (req, res) => res.status(201).json({ message: 'Resource created' }));
app.get('/not-found',    (req, res) => res.status(404).json({ error: 'Not found' }));
app.get('/server-error', (req, res) => res.status(500).json({ error: 'Internal error' }));
```

**Explanation — HTTP Status Codes:**

| Range | Category | Common Codes |
|---|---|---|
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirect | 301 Permanent, 302 Temporary |
| 4xx | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable |
| 5xx | Server Error | 500 Internal Server Error, 503 Service Unavailable |

`res.status(code)` sets the status code. It returns `res` so you can chain `.json()` directly. Always set appropriate status codes — a 200 response for a failed operation is a common mistake that breaks client-side error handling.

</details>

---

### 🟡 Medium

**Exercise 1.4 — nodemon + package.json scripts**

Set up a project with:
- `npm run dev` starts the server with nodemon
- `npm start` starts with node
- Server reads port from `process.env.PORT` with fallback to 3000

<details>
<summary>✅ Answer & Explanation</summary>

```bash
npm init -y
npm install express
npm install nodemon --save-dev
```

```json
// package.json
{
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js"
  }
}
```

```js
// app.js
require('dotenv').config(); // optional — if using .env file
const express = require('express');
const app     = express();
const port    = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Running!'));

app.listen(port, () => console.log(`Server running on port ${port}`));
```

**Explanation:**

`nodemon` watches your files and automatically restarts the server on changes — saves you from manually stopping and restarting after every edit.

`process.env.PORT` reads the PORT environment variable. This is critical for deployment — platforms like Heroku, Render, and Railway assign a random port and set it via environment variable. If you hardcode `3000`, the app won't start in production.

`|| 3000` is the fallback for local development when `PORT` isn't set.

</details>

---

## 2 — Routing

---

### 🟢 Easy

**Exercise 2.1 — Route parameters**

Create these routes:
- `GET /users/:id` → respond with `{ userId: <id> }`
- `GET /posts/:postId/comments/:commentId` → respond with both IDs

Test: `GET /users/42`, `GET /posts/5/comments/12`

<details>
<summary>✅ Answer & Explanation</summary>

```js
app.get('/users/:id', (req, res) => {
  res.json({ userId: req.params.id });
});

app.get('/posts/:postId/comments/:commentId', (req, res) => {
  const { postId, commentId } = req.params;
  res.json({ postId, commentId });
});
```

Test: `GET /users/42` → `{ "userId": "42" }`
Test: `GET /posts/5/comments/12` → `{ "postId": "5", "commentId": "12" }`

**Explanation:**

Route parameters (`:paramName`) capture dynamic segments of the URL. They're available on `req.params` as strings — always strings, even if the URL contains a number.

```js
req.params.id   // "42" (string, not number!)
Number(req.params.id) // 42 (convert when needed)
parseInt(req.params.id, 10) // safer conversion
```

Always validate that the parameter is what you expect (a valid number, a valid UUID, etc.) before using it in a database query.

</details>

---

**Exercise 2.2 — Query strings**

Create `GET /search` that:
- Reads `q` (search query), `page` (default 1), and `limit` (default 10) from query params
- Responds with `{ query, page, limit, message: "Searching for..." }`

Test: `GET /search?q=javascript&page=2&limit=5`

<details>
<summary>✅ Answer & Explanation</summary>

```js
app.get('/search', (req, res) => {
  const query = req.query.q     || '';
  const page  = parseInt(req.query.page,  10) || 1;
  const limit = parseInt(req.query.limit, 10) || 10;

  res.json({
    query,
    page,
    limit,
    message: `Searching for "${query}" — page ${page}, ${limit} results`
  });
});
```

**Explanation:**

Query strings come after `?` in the URL: `/search?q=javascript&page=2`.

`req.query` is an object with all query parameters as strings. `req.query.q` → `"javascript"`.

We use `parseInt(..., 10) || defaultValue` to:
1. Convert string to integer
2. Fall back to default if missing or invalid (`NaN` is falsy)

Query params are **optional** — never assume they exist. Always provide defaults.

</details>

---

### 🟡 Medium

**Exercise 2.3 — express.Router()**

Create a modular routing setup:
- `routes/products.js` — handles `GET /`, `GET /:id`, `POST /`, `DELETE /:id`
- Mount it in `app.js` at `/api/products`
- Each route returns a simple JSON response (no real DB needed)

<details>
<summary>✅ Answer & Explanation</summary>

```js
// routes/products.js
const express  = require('express');
const router   = express.Router();

const products = [
  { id: 1, name: 'Laptop',  price: 999 },
  { id: 2, name: 'Phone',   price: 499 },
  { id: 3, name: 'Monitor', price: 299 }
];

router.get('/', (req, res) => {
  res.json(products);
});

router.get('/:id', (req, res) => {
  const product = products.find(p => p.id === parseInt(req.params.id));
  if (!product) return res.status(404).json({ error: 'Product not found' });
  res.json(product);
});

router.post('/', (req, res) => {
  const newProduct = { id: products.length + 1, ...req.body };
  products.push(newProduct);
  res.status(201).json(newProduct);
});

router.delete('/:id', (req, res) => {
  const index = products.findIndex(p => p.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ error: 'Product not found' });
  products.splice(index, 1);
  res.status(204).end();
});

module.exports = router;
```

```js
// app.js
const express        = require('express');
const productsRouter = require('./routes/products');

const app = express();
app.use(express.json());
app.use('/api/products', productsRouter);

app.listen(3000, () => console.log('Running on 3000'));
```

**Results in:**
```
GET    /api/products       → list all
GET    /api/products/:id   → get one
POST   /api/products       → create
DELETE /api/products/:id   → delete
```

**Explanation:**

`express.Router()` creates a mini-application that handles routing independently. When mounted with `app.use('/api/products', router)`, all routes defined in that router are prefixed with `/api/products`.

This keeps your codebase organized — each resource (users, products, orders) lives in its own file. As your app grows, you simply add new route files rather than making `app.js` thousands of lines long.

</details>

---

### 🔴 Challenging

**Exercise 2.4 — Route-level middleware**

Create an auth middleware that:
- Checks for `Authorization: Bearer <token>` header
- If token is `"valid-token"` → sets `req.user = { id: 1, name: "Admin" }` and calls `next()`
- If missing or wrong → returns `401 { error: "Unauthorized" }`

Apply it only to `GET /dashboard` and `GET /settings`, not to `GET /public`.

<details>
<summary>✅ Answer & Explanation</summary>

```js
const authMiddleware = (req, res, next) => {
  const authHeader = req.headers.authorization;

  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'Unauthorized — no token provided' });
  }

  const token = authHeader.split(' ')[1];

  if (token !== 'valid-token') {
    return res.status(401).json({ error: 'Unauthorized — invalid token' });
  }

  req.user = { id: 1, name: 'Admin' }; // attach user to request
  next();
};

// Public route — no auth
app.get('/public', (req, res) => {
  res.json({ message: 'Anyone can see this' });
});

// Protected routes — auth required
app.get('/dashboard', authMiddleware, (req, res) => {
  res.json({ message: `Welcome, ${req.user.name}!`, user: req.user });
});

app.get('/settings', authMiddleware, (req, res) => {
  res.json({ message: 'Settings page', user: req.user });
});
```

**Test:**
```bash
# Without token
curl http://localhost:3000/dashboard
# → 401 Unauthorized

# With correct token
curl -H "Authorization: Bearer valid-token" http://localhost:3000/dashboard
# → 200 { message: "Welcome, Admin!" }
```

**Explanation:**

The auth check uses `return res.status(401).json(...)` — the `return` is important. Without it, execution would continue to `next()` even after sending the error response, causing a "headers already sent" error.

`req.user = { id: 1, name: 'Admin' }` attaches data to the request object so downstream handlers can access it. This is the standard pattern for authentication middleware — validate the token, decode the user, attach it to `req`, call `next()`.

</details>

---

## 3 — Middleware

---

### 🟢 Easy

**Exercise 3.1 — Request logger**

Write a middleware that logs every request in this format:
```
[2024-01-15 10:30:00] GET /api/users 200 45ms
```
Log the method, path, status code, and response time.

<details>
<summary>✅ Answer & Explanation</summary>

```js
const requestLogger = (req, res, next) => {
  const start = Date.now();

  // Hook into res.on('finish') to log after the response is sent
  res.on('finish', () => {
    const duration  = Date.now() - start;
    const timestamp = new Date().toISOString().replace('T', ' ').slice(0, 19);
    console.log(`[${timestamp}] ${req.method} ${req.originalUrl} ${res.statusCode} ${duration}ms`);
  });

  next();
};

app.use(requestLogger);
```

**Explanation:**

We hook into `res.on('finish')` because the response hasn't been sent yet when the middleware runs — only when it finishes do we know the status code and actual duration.

`Date.now()` captures the start time in milliseconds. Subtracting it after the response gives the total processing time.

`req.originalUrl` includes the full path with query string. `req.path` only has the path portion.

This is essentially what `morgan` (a popular npm logger) does internally. Understanding how to write it yourself is valuable.

</details>

---

### 🟡 Medium

**Exercise 3.2 — Middleware stack**

Set up this middleware pipeline in the correct order:
1. `morgan('dev')` — request logging
2. `express.json()` — parse JSON bodies
3. `cors()` — enable CORS
4. A custom middleware that adds `X-Response-Time` header
5. Your routes
6. 404 handler
7. Global error handler

```bash
npm install morgan cors
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
const express    = require('express');
const morgan     = require('morgan');
const cors       = require('cors');

const app = express();

// 1. Logger — log every request
app.use(morgan('dev'));

// 2. Body parser — parse JSON bodies
app.use(express.json());

// 3. CORS — allow cross-origin requests
app.use(cors());

// 4. Custom response time header
app.use((req, res, next) => {
  const start = process.hrtime();
  res.on('finish', () => {
    const [s, ns] = process.hrtime(start);
    res.setHeader('X-Response-Time', `${(s * 1000 + ns / 1e6).toFixed(2)}ms`);
  });
  next();
});

// 5. Routes
app.get('/users', (req, res) => res.json([{ id: 1, name: 'Marta' }]));
app.post('/users', (req, res) => res.status(201).json(req.body));

// 6. 404 handler — catches unmatched routes
app.use((req, res, next) => {
  res.status(404).json({ error: `Cannot ${req.method} ${req.originalUrl}` });
});

// 7. Global error handler — MUST have 4 params
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({ error: err.message || 'Internal server error' });
});

app.listen(3000);
```

**Explanation:**

Order is everything in Express middleware:
- Logger first → logs every request including those that fail early
- Body parser before routes → `req.body` needs to be parsed before route handlers read it
- Routes in the middle → process valid requests
- 404 handler after routes → only reached if no route matched
- Error handler last → `(err, req, res, next)` signature tells Express this is an error handler

If you put the 404 handler before your routes, every request returns 404.
If you put the error handler before the 404 handler, thrown errors skip the 404 logic.

</details>

---

## 4 — Request & Response

---

### 🟡 Medium

**Exercise 4.1 — Full CRUD in-memory API**

Build a complete in-memory CRUD API for `students`:
- `GET /students` — list all
- `GET /students/:id` — get one (404 if not found)
- `POST /students` — create (requires `name`, `grade`)
- `PUT /students/:id` — update (404 if not found)
- `DELETE /students/:id` — delete, return 204 (404 if not found)

<details>
<summary>✅ Answer & Explanation</summary>

```js
const express = require('express');
const app     = express();
app.use(express.json());

let students = [
  { id: 1, name: 'Marta',  grade: 'A' },
  { id: 2, name: 'Abel',   grade: 'B' },
  { id: 3, name: 'Hana',   grade: 'A+' }
];
let nextId = 4;

// GET all
app.get('/students', (req, res) => {
  res.json(students);
});

// GET one
app.get('/students/:id', (req, res) => {
  const student = students.find(s => s.id === parseInt(req.params.id));
  if (!student) return res.status(404).json({ error: 'Student not found' });
  res.json(student);
});

// POST create
app.post('/students', (req, res) => {
  const { name, grade } = req.body;
  if (!name || !grade) {
    return res.status(400).json({ error: 'name and grade are required' });
  }
  const student = { id: nextId++, name, grade };
  students.push(student);
  res.status(201).json(student);
});

// PUT update
app.put('/students/:id', (req, res) => {
  const index = students.findIndex(s => s.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ error: 'Student not found' });
  students[index] = { ...students[index], ...req.body, id: students[index].id };
  res.json(students[index]);
});

// DELETE
app.delete('/students/:id', (req, res) => {
  const index = students.findIndex(s => s.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ error: 'Student not found' });
  students.splice(index, 1);
  res.status(204).end();
});

app.listen(3000, () => console.log('Students API on port 3000'));
```

**Test with curl:**
```bash
curl http://localhost:3000/students
curl http://localhost:3000/students/1
curl -X POST http://localhost:3000/students -H "Content-Type: application/json" -d '{"name":"Yonas","grade":"B+"}'
curl -X PUT http://localhost:3000/students/1 -H "Content-Type: application/json" -d '{"grade":"A+"}'
curl -X DELETE http://localhost:3000/students/2
```

**Explanation:**

`204 No Content` is the correct status for successful DELETE — the resource is gone, there's nothing to return. Using `res.status(204).end()` sends the status with no body.

`{ ...students[index], ...req.body, id: students[index].id }` spreads the existing student, overlays the new body, but forces the `id` to stay the same — preventing clients from changing IDs through PUT requests.

Always check if the resource exists before operating on it, and return 404 if it doesn't.

</details>

---

## 5 — Validation & Error Handling

---

### 🟡 Medium

**Exercise 5.1 — Validation with express-validator**

Add validation to `POST /students`:
- `name` — required, min 2 chars, only alphabetical
- `grade` — required, must be one of: `A`, `A+`, `B`, `B+`, `C`, `D`, `F`
- `age` — optional, integer between 15 and 100

Return structured errors if validation fails.

```bash
npm install express-validator
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
const { body, validationResult } = require('express-validator');

const validateStudent = [
  body('name')
    .trim()
    .notEmpty().withMessage('Name is required')
    .isLength({ min: 2 }).withMessage('Name must be at least 2 characters')
    .isAlpha('en-US', { ignore: ' ' }).withMessage('Name must contain only letters'),

  body('grade')
    .notEmpty().withMessage('Grade is required')
    .isIn(['A', 'A+', 'B', 'B+', 'C', 'D', 'F']).withMessage('Grade must be A, A+, B, B+, C, D, or F'),

  body('age')
    .optional()
    .isInt({ min: 15, max: 100 }).withMessage('Age must be between 15 and 100')
    .toInt()
];

app.post('/students', validateStudent, (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(422).json({
      error:   'Validation failed',
      details: errors.array().map(e => ({ field: e.path, message: e.msg }))
    });
  }

  const student = { id: nextId++, ...req.body };
  students.push(student);
  res.status(201).json(student);
});
```

**Failed validation response:**
```json
{
  "error": "Validation failed",
  "details": [
    { "field": "name", "message": "Name must contain only letters" },
    { "field": "grade", "message": "Grade must be A, A+, B, B+, C, D, or F" }
  ]
}
```

**Explanation:**

`validationResult(req)` collects all validation errors. We check `!errors.isEmpty()` and return 422 (Unprocessable Entity) — the right code for "the request is well-formed but the data fails business rules."

`.optional()` — the field doesn't have to be present, but if it is, it must pass the validation.

`.toInt()` is a sanitizer — it converts `"21"` to `21`. Sanitization runs after validation, so you can safely use the converted value in `req.body.age` after validation passes.

</details>

---

### 🔴 Challenging

**Exercise 5.2 — Global error handler**

Create a robust error handling system:
1. A custom `AppError` class with `message` and `statusCode`
2. An `asyncHandler` wrapper to avoid try/catch repetition
3. A global error handler middleware that:
   - Returns structured JSON errors
   - In development: includes the stack trace
   - In production: hides internal error details for 500 errors
4. A 404 middleware for unmatched routes

<details>
<summary>✅ Answer & Explanation</summary>

```js
// utils/AppError.js
class AppError extends Error {
  constructor(message, statusCode = 500) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true; // our own errors vs unexpected bugs
    Error.captureStackTrace(this, this.constructor);
  }
}
module.exports = AppError;

// utils/asyncHandler.js
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
module.exports = asyncHandler;

// middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  const isDev      = process.env.NODE_ENV === 'development';

  if (isDev) {
    // In development — full details
    return res.status(statusCode).json({
      status:  'error',
      message: err.message,
      stack:   err.stack
    });
  }

  // In production — hide internal error details
  if (err.isOperational) {
    return res.status(statusCode).json({
      status:  statusCode < 500 ? 'fail' : 'error',
      message: err.message
    });
  }

  // Unknown/unexpected errors — don't leak details
  console.error('UNEXPECTED ERROR:', err);
  return res.status(500).json({
    status:  'error',
    message: 'Something went wrong. Please try again later.'
  });
};
module.exports = errorHandler;

// app.js usage
const AppError     = require('./utils/AppError');
const asyncHandler = require('./utils/asyncHandler');
const errorHandler = require('./middleware/errorHandler');

app.get('/students/:id', asyncHandler(async (req, res) => {
  const student = await findStudent(req.params.id);
  if (!student) throw new AppError('Student not found', 404);
  res.json(student);
}));

// 404 for unmatched routes
app.use((req, res, next) => {
  next(new AppError(`Cannot ${req.method} ${req.originalUrl}`, 404));
});

// Global error handler — LAST
app.use(errorHandler);
```

**Explanation:**

`isOperational` distinguishes between errors you threw on purpose (user not found, validation failed) and unexpected bugs (database connection lost, null pointer). In production, you show operational error messages to users but hide unexpected ones.

`Error.captureStackTrace(this, this.constructor)` ensures the stack trace points to where the error was thrown, not to the `AppError` constructor itself — makes debugging cleaner.

`asyncHandler` wraps async functions so rejected promises are automatically passed to `next(err)`. Without it, you'd need `try/catch` in every single async route handler.

This pattern is what professional Node.js apps use. Once set up, every route handler looks clean — just throw a descriptive `AppError` and the middleware handles the rest.

</details>

---

## 6 — Project Structure

---

### 🔴 Challenging

**Exercise 6.1 — Restructure with MVC**

Take the students CRUD API from Exercise 4.1 and restructure it properly:

```
students-api/
├── app.js
├── server.js
├── routes/studentRoutes.js
├── controllers/studentController.js
├── middleware/errorHandler.js
├── utils/AppError.js
└── utils/asyncHandler.js
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// utils/AppError.js
class AppError extends Error {
  constructor(message, statusCode = 500) {
    super(message);
    this.statusCode  = statusCode;
    this.isOperational = true;
  }
}
module.exports = AppError;

// utils/asyncHandler.js
module.exports = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
```

```js
// controllers/studentController.js
const AppError     = require('../utils/AppError');
const asyncHandler = require('../utils/asyncHandler');

let students = [
  { id: 1, name: 'Marta', grade: 'A' },
  { id: 2, name: 'Abel',  grade: 'B' }
];
let nextId = 3;

exports.getAll = asyncHandler(async (req, res) => {
  res.json({ status: 'success', data: students });
});

exports.getOne = asyncHandler(async (req, res, next) => {
  const student = students.find(s => s.id === parseInt(req.params.id));
  if (!student) return next(new AppError('Student not found', 404));
  res.json({ status: 'success', data: student });
});

exports.create = asyncHandler(async (req, res) => {
  const { name, grade } = req.body;
  if (!name || !grade) throw new AppError('name and grade are required', 400);
  const student = { id: nextId++, name, grade };
  students.push(student);
  res.status(201).json({ status: 'success', data: student });
});

exports.update = asyncHandler(async (req, res, next) => {
  const index = students.findIndex(s => s.id === parseInt(req.params.id));
  if (index === -1) return next(new AppError('Student not found', 404));
  students[index] = { ...students[index], ...req.body, id: students[index].id };
  res.json({ status: 'success', data: students[index] });
});

exports.remove = asyncHandler(async (req, res, next) => {
  const index = students.findIndex(s => s.id === parseInt(req.params.id));
  if (index === -1) return next(new AppError('Student not found', 404));
  students.splice(index, 1);
  res.status(204).end();
});
```

```js
// routes/studentRoutes.js
const express    = require('express');
const router     = express.Router();
const controller = require('../controllers/studentController');

router.get('/',     controller.getAll);
router.get('/:id',  controller.getOne);
router.post('/',    controller.create);
router.put('/:id',  controller.update);
router.delete('/:id', controller.remove);

module.exports = router;
```

```js
// middleware/errorHandler.js
module.exports = (err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  res.status(statusCode).json({
    status:  'error',
    message: err.message || 'Internal server error'
  });
};
```

```js
// app.js
const express        = require('express');
const morgan         = require('morgan');
const studentRoutes  = require('./routes/studentRoutes');
const AppError       = require('./utils/AppError');
const errorHandler   = require('./middleware/errorHandler');

const app = express();
app.use(morgan('dev'));
app.use(express.json());

app.use('/api/students', studentRoutes);

app.use((req, res, next) => next(new AppError(`Cannot ${req.method} ${req.url}`, 404)));
app.use(errorHandler);

module.exports = app;
```

```js
// server.js
const app  = require('./app');
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Running on port ${port}`));
```

**Explanation:**

The separation of `app.js` and `server.js` is crucial for testing — you can import `app` in your test files without actually starting a server on a port. This is the standard pattern for testable Express applications.

Each layer has one job:
- Routes → declare endpoints and connect to controller functions
- Controllers → read from `req`, call business logic, write to `res`
- Utils → reusable helpers used across the app
- Middleware → cross-cutting concerns (logging, auth, error handling)

</details>

---

## 7 — Final Project — Books REST API

Build a complete REST API for managing books with:
- Full CRUD operations
- Input validation
- Proper error handling
- MVC structure
- Search by query string
- Pagination

**Endpoints:**
```
GET    /api/books              list all (with ?search=&page=&limit=)
GET    /api/books/:id          get one
POST   /api/books              create
PUT    /api/books/:id          update
DELETE /api/books/:id          delete
GET    /api/books/stats        count by genre
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// controllers/bookController.js
const AppError     = require('../utils/AppError');
const asyncHandler = require('../utils/asyncHandler');

let books = [
  { id: 1, title: 'Clean Code',       author: 'Robert Martin',  genre: 'Programming', year: 2008 },
  { id: 2, title: 'The Pragmatic Programmer', author: 'Hunt & Thomas', genre: 'Programming', year: 1999 },
  { id: 3, title: 'Sapiens',          author: 'Yuval Noah Harari', genre: 'History', year: 2011 },
  { id: 4, title: 'Atomic Habits',    author: 'James Clear',    genre: 'Self-Help', year: 2018 },
  { id: 5, title: 'Deep Work',        author: 'Cal Newport',    genre: 'Self-Help', year: 2016 }
];
let nextId = 6;

exports.getAll = asyncHandler(async (req, res) => {
  const { search, page = 1, limit = 10 } = req.query;

  let result = [...books];

  // Search filter
  if (search) {
    const q = search.toLowerCase();
    result = result.filter(b =>
      b.title.toLowerCase().includes(q) ||
      b.author.toLowerCase().includes(q)
    );
  }

  // Pagination
  const total      = result.length;
  const pageNum    = parseInt(page, 10);
  const limitNum   = parseInt(limit, 10);
  const startIndex = (pageNum - 1) * limitNum;
  const paginated  = result.slice(startIndex, startIndex + limitNum);

  res.json({
    status: 'success',
    total,
    page: pageNum,
    pages: Math.ceil(total / limitNum),
    data: paginated
  });
});

exports.getOne = asyncHandler(async (req, res, next) => {
  const book = books.find(b => b.id === parseInt(req.params.id));
  if (!book) return next(new AppError('Book not found', 404));
  res.json({ status: 'success', data: book });
});

exports.getStats = asyncHandler(async (req, res) => {
  const stats = books.reduce((acc, book) => {
    acc[book.genre] = (acc[book.genre] || 0) + 1;
    return acc;
  }, {});
  res.json({ status: 'success', data: stats });
});

exports.create = asyncHandler(async (req, res) => {
  const { title, author, genre, year } = req.body;
  const book = { id: nextId++, title, author, genre, year: parseInt(year) };
  books.push(book);
  res.status(201).json({ status: 'success', data: book });
});

exports.update = asyncHandler(async (req, res, next) => {
  const index = books.findIndex(b => b.id === parseInt(req.params.id));
  if (index === -1) return next(new AppError('Book not found', 404));
  books[index] = { ...books[index], ...req.body, id: books[index].id };
  res.json({ status: 'success', data: books[index] });
});

exports.remove = asyncHandler(async (req, res, next) => {
  const index = books.findIndex(b => b.id === parseInt(req.params.id));
  if (index === -1) return next(new AppError('Book not found', 404));
  books.splice(index, 1);
  res.status(204).end();
});
```

```js
// routes/bookRoutes.js
const express    = require('express');
const router     = express.Router();
const controller = require('../controllers/bookController');
const { body, validationResult } = require('express-validator');

const validate = [
  body('title').trim().notEmpty().withMessage('Title is required'),
  body('author').trim().notEmpty().withMessage('Author is required'),
  body('genre').trim().notEmpty().withMessage('Genre is required'),
  body('year').isInt({ min: 1000, max: new Date().getFullYear() }).withMessage('Invalid year'),
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(422).json({ errors: errors.array() });
    }
    next();
  }
];

router.get('/stats', controller.getStats);  // ← must be before /:id
router.get('/',      controller.getAll);
router.get('/:id',   controller.getOne);
router.post('/',     validate, controller.create);
router.put('/:id',   controller.update);
router.delete('/:id',controller.remove);

module.exports = router;
```

**Test commands:**
```bash
# List all
curl http://localhost:3000/api/books

# Search
curl "http://localhost:3000/api/books?search=code&page=1&limit=3"

# Get one
curl http://localhost:3000/api/books/1

# Create
curl -X POST http://localhost:3000/api/books \
  -H "Content-Type: application/json" \
  -d '{"title":"JavaScript","author":"Flanagan","genre":"Programming","year":2020}'

# Update
curl -X PUT http://localhost:3000/api/books/1 \
  -H "Content-Type: application/json" \
  -d '{"year":2009}'

# Delete
curl -X DELETE http://localhost:3000/api/books/2

# Stats
curl http://localhost:3000/api/books/stats
```

**Key note:** `/stats` must be defined **before** `/:id` in the router. Otherwise Express would try to match `"stats"` as an `:id` parameter and never reach the stats route.

</details>

---

## 8 — Interview Questions

> 🎯 Real questions asked in Node.js/Express interviews. Tap **"Show Answer"** after forming your own response.

---

**Q1. What is Express.js and why is it used?**

<details>
<summary>Show Answer</summary>

Express.js is a minimal, unopinionated web framework for Node.js. It sits on top of Node's built-in `http` module and provides:
- A clean routing system (`app.get`, `app.post`, etc.)
- Middleware pipeline for request/response processing
- Helpers for sending responses (`res.json`, `res.status`, `res.send`)
- `express.Router()` for modular route organization

It's used because writing raw Node.js HTTP servers is verbose and repetitive. Express provides just enough structure without being opinionated about architecture, making it suitable for everything from simple APIs to complex microservices.

</details>

---

**Q2. What is middleware in Express? How does it work?**

<details>
<summary>Show Answer</summary>

Middleware is a function with the signature `(req, res, next)` that intercepts the request-response cycle. It can read and modify `req` and `res`, end the cycle, or call `next()` to pass control to the next middleware.

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.path}`);
  next(); // pass to next handler
});
```

Middleware can be:
- **Application-level** — `app.use()` — runs for all requests
- **Route-level** — passed as second argument to a route
- **Error-handling** — `(err, req, res, next)` — 4 parameters
- **Built-in** — `express.json()`, `express.static()`
- **Third-party** — `morgan`, `cors`, `helmet`

The key rule: **always call `next()` or send a response**. If you do neither, the request hangs.

</details>

---

**Q3. What is the difference between `req.params`, `req.query`, and `req.body`?**

<details>
<summary>Show Answer</summary>

| | `req.params` | `req.query` | `req.body` |
|---|---|---|---|
| Source | URL path segments | URL query string | HTTP request body |
| Example URL | `/users/123` | `/search?q=node` | POST with JSON |
| Access | `req.params.id` → `"123"` | `req.query.q` → `"node"` | `req.body.name` → `"Marta"` |
| Type | Always string | Always string | Depends on middleware |
| Required setup | `:param` in route | None | `express.json()` middleware |

```js
// Route: /posts/:id/comments?page=2
// Body: { "text": "Great post" }
app.post('/posts/:id/comments', (req, res) => {
  req.params.id;  // "5"  — from URL path
  req.query.page; // "2"  — from query string
  req.body.text;  // "Great post" — from request body
});
```

</details>

---

**Q4. How does error handling work in Express?**

<details>
<summary>Show Answer</summary>

Express has a special 4-parameter middleware signature for error handling: `(err, req, res, next)`.

You trigger it by calling `next(error)` from any middleware or route handler:

```js
app.get('/users/:id', async (req, res, next) => {
  try {
    const user = await findUser(req.params.id);
    if (!user) return next(new Error('User not found'));
    res.json(user);
  } catch (err) {
    next(err);
  }
});

// Error handler — must be LAST middleware
app.use((err, req, res, next) => {
  res.status(err.statusCode || 500).json({ error: err.message });
});
```

Key rules:
- Error handler must have exactly 4 parameters
- Must be registered after all routes
- In async handlers, always wrap in `try/catch` and call `next(err)` (Express 4)
- Express 5 handles async errors automatically

</details>

---

**Q5. What is `express.Router()` and when would you use it?**

<details>
<summary>Show Answer</summary>

`express.Router()` creates a mini-Express application that handles routing independently. It lets you group related routes in separate files and mount them on the main app.

```js
// routes/users.js
const router = express.Router();
router.get('/', listUsers);
router.post('/', createUser);
module.exports = router;

// app.js
app.use('/api/users', require('./routes/users'));
```

Use it when:
- Your app has multiple resources (users, products, orders)
- Route files are getting too long
- You want to apply middleware to a group of routes
- You're building a modular, maintainable codebase

Without Router, everything would be in one file — unreadable for anything beyond trivial apps.

</details>

---

**Q6. What is CORS and how do you handle it in Express?**

<details>
<summary>Show Answer</summary>

CORS (Cross-Origin Resource Sharing) is a browser security policy that blocks HTTP requests to a different origin (domain, port, or protocol) than the one that served the page.

When your React app at `localhost:3000` calls your Express API at `localhost:5000`, the browser blocks it because the ports differ — even though both are localhost.

Fix it with the `cors` npm package:

```js
const cors = require('cors');

// Development — allow all
app.use(cors());

// Production — specific origins only
app.use(cors({
  origin: ['https://myapp.com'],
  credentials: true
}));
```

Without CORS configuration, browser-based clients get `Access-Control-Allow-Origin` errors. Non-browser clients (like curl, Postman, server-to-server) are not affected by CORS.

</details>

---

**Q7. What is the difference between `res.send()`, `res.json()`, and `res.end()`?**

<details>
<summary>Show Answer</summary>

| Method | Content-Type | Body | Use When |
|---|---|---|---|
| `res.send(data)` | Auto-detected | String → HTML, Object → JSON | General purpose |
| `res.json(data)` | `application/json` | Always JSON | API responses |
| `res.end()` | Not set | Empty | Empty response, 204 No Content |

```js
res.send('Hello');           // text/html
res.send({ name: 'Marta' }); // application/json (auto-detected)
res.json({ name: 'Marta' }); // application/json (explicit — preferred for APIs)
res.status(204).end();       // no body, used for DELETE success
```

For REST APIs, always use `res.json()` — it's explicit and always sets the correct Content-Type header regardless of what you pass.

</details>

---

**Q8. How would you protect a route in Express?**

<details>
<summary>Show Answer</summary>

Create an authentication middleware and apply it to protected routes:

```js
const jwt = require('jsonwebtoken');

const protect = async (req, res, next) => {
  try {
    const token = req.headers.authorization?.split(' ')[1];
    if (!token) return res.status(401).json({ error: 'No token provided' });

    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ error: 'Invalid or expired token' });
  }
};

// Apply to specific routes
app.get('/profile', protect, (req, res) => {
  res.json({ user: req.user });
});

// Apply to all routes in a router
router.use(protect);
router.get('/', listAll);
```

The middleware extracts the token from the `Authorization` header, verifies it, and attaches the decoded payload (`req.user`) for downstream handlers. This is covered fully in Module 05 (Authentication).

</details>

---

> 🎯 **Exercises complete!**
>
> Move to **[Module 04 — REST APIs →](../04-REST-APIs/README.md)**
>
> You now know Express inside out. Time to design professional REST APIs. 🚀
