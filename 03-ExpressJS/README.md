# 🟢 Module 03 — Express.js

> 👋 **You've mastered Node.js internals. Now let's build real web servers — fast.**
> Express.js is the most widely used Node.js framework in the world. Netflix, IBM, Uber, and Twitter have all used it in production. By the end of this module, you'll be building clean, structured REST APIs with routing, middleware, validation, and error handling.

---

## 📖 Table of Contents

1. [What is Express.js?](#1--what-is-expressjs)
2. [Why Express?](#2--why-express)
3. [Installation & First Server](#3--installation--first-server)
4. [Request & Response Objects](#4--request--response-objects)
5. [Routing](#5--routing)
6. [Middleware](#6--middleware)
7. [Serving Static Files](#7--serving-static-files)
8. [Cookies](#8--cookies)
9. [HTTP Headers](#9--http-headers)
10. [Redirects](#10--redirects)
11. [Sessions](#11--sessions)
12. [Input Validation & Sanitization](#12--input-validation--sanitization)
13. [Handling Forms & File Uploads](#13--handling-forms--file-uploads)
14. [CORS](#14--cors)
15. [Error Handling](#15--error-handling)
16. [Project Structure (MVC Pattern)](#16--project-structure-mvc-pattern)
17. [Async/Await in Express](#17--asyncawait-in-express)
18. [Security Best Practices](#18--security-best-practices)
19. [Express 5 & Alternatives](#19--express-5--alternatives)
20. [Learning Resources](#20--learning-resources)
21. [Quick Knowledge Check](#21--quick-knowledge-check)

---

## 1 — What is Express.js?

Express is a **minimal and flexible web framework** built on top of Node.js. It provides a thin, organized layer on top of Node's built-in `http` module — abstracting away complexity and giving you a clean system for routing, middleware, and responses.

> **"Express is the most popular HTTP server framework built upon Node.js."**

Think of Node.js as the engine and Express as the chassis — Node provides the async, event-driven power, and Express gives it the structure to build web servers and APIs efficiently.

```
Without Express (raw Node.js http module):
─────────────────────────────────────────
const http = require('http');
const server = http.createServer((req, res) => {
  if (req.url === '/users' && req.method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify(users));
  } else if (req.url === '/users' && req.method === 'POST') {
    // manually collect body chunks...
    // manually parse JSON...
    // manually handle errors...
  }
  // gets messy very fast
});

With Express:
─────────────
app.get('/users', (req, res) => res.json(users));
app.post('/users', (req, res) => res.json(req.body));
```

Express is **unopinionated** — it doesn't force a directory structure or specific patterns. That freedom is its greatest strength and also its greatest responsibility.

---

### A Brief History

- **2010** — Created by TJ Holowaychuk. Quickly became the dominant Node.js framework.
- **2014** — Express 4.x released. Removed bundled middleware, made it more modular.
- **2023/2024** — Express 5.x released with native async/await error handling.
- **Today** — Still the #1 downloaded Node.js framework. Its design influenced Fastify, Koa, NestJS, and Hono.

> 💡 Even if you use other frameworks later, Express is the foundation you must know. Most companies still run it in production.

---

## 2 — Why Express?

| Feature | Benefit |
|---|---|
| **Simplicity** | 4 lines to create a working web server |
| **Unopinionated** | You decide the structure — no rigid conventions |
| **Fast** | Thin layer on Node.js — minimal overhead |
| **Middleware ecosystem** | Thousands of npm packages plug straight in |
| **Routing** | Clean, expressive route definitions |
| **JavaScript everywhere** | Same language as your frontend |
| **Massive community** | 10+ years of answers, tutorials, and packages |

**Use cases:**
- REST APIs (most common)
- Backend for React / Vue / Angular SPAs
- Microservices
- Real-time apps (with Socket.IO)
- GraphQL servers (with Apollo)

---

## 3 — Installation & First Server

### Setup

```bash
mkdir my-express-app
cd my-express-app
npm init -y
npm install express
```

### Hello World

```js
// app.js
const express = require('express');
const app     = express();
const port    = 3000;

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

```bash
node app.js
# Open http://localhost:3000 → "Hello, World!"
```

**What those 4 lines do:**
1. `require('express')` — imports the Express module
2. `express()` — creates your application instance
3. `app.get('/', ...)` — registers a route: "when a GET request hits `/`, run this function"
4. `app.listen(3000, ...)` — starts the server on port 3000

### HTTP Method Handlers

Express has a method for every HTTP verb:

```js
app.get('/path',    (req, res) => { /* handle GET    */ });
app.post('/path',   (req, res) => { /* handle POST   */ });
app.put('/path',    (req, res) => { /* handle PUT    */ });
app.delete('/path', (req, res) => { /* handle DELETE */ });
app.patch('/path',  (req, res) => { /* handle PATCH  */ });
app.all('/path',    (req, res) => { /* handle ALL    */ });
```

### Development Setup with nodemon

Install nodemon so your server restarts automatically on file changes:

```bash
npm install nodemon --save-dev
```

Add to `package.json`:
```json
{
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js"
  }
}
```

```bash
npm run dev   # server restarts on every save
```

---

## 4 — Request & Response Objects

Every route handler receives two objects: `req` (request) and `res` (response).

---

### The Request Object (`req`)

`req` contains everything about the incoming HTTP request:

| Property | Description | Example |
|---|---|---|
| `req.params` | URL route parameters | `req.params.id` → `"123"` |
| `req.query` | Query string parameters | `req.query.name` → `"Marta"` |
| `req.body` | Request body (JSON, form data) | `req.body.email` → `"m@x.com"` |
| `req.headers` | All HTTP headers | `req.headers['content-type']` |
| `req.method` | HTTP method | `"GET"`, `"POST"` |
| `req.url` | Full request URL path | `"/users/123?sort=asc"` |
| `req.path` | URL path only | `"/users/123"` |
| `req.hostname` | Request hostname | `"localhost"` |
| `req.ip` | Client IP address | `"127.0.0.1"` |
| `req.cookies` | Cookies (needs cookie-parser) | `req.cookies.token` |
| `req.secure` | Is it HTTPS? | `true` / `false` |
| `req.xhr` | Was it an XMLHttpRequest? | `true` / `false` |

**Query String Parameters** — `?key=value` in the URL:

```js
// GET /search?name=Marta&age=21
app.get('/search', (req, res) => {
  console.log(req.query);        // { name: 'Marta', age: '21' }
  console.log(req.query.name);   // 'Marta'
  console.log(req.query.age);    // '21' (always a string!)
});
```

**POST Body Parameters** — data sent in the request body:

```js
// Must use middleware to parse the body first
app.use(express.json());          // for JSON bodies
app.use(express.urlencoded({ extended: true })); // for form data

app.post('/users', (req, res) => {
  console.log(req.body.name);   // 'Marta'
  console.log(req.body.email);  // 'marta@example.com'
});
```

> 📝 `express.json()` and `express.urlencoded()` replaced the old `body-parser` package. Since Express 4.16, they are built in.

---

### The Response Object (`res`)

`res` is how you send data back to the client:

```js
// Send a string (Content-Type: text/html)
res.send('Hello World');

// Send JSON (Content-Type: application/json) — most common for APIs
res.json({ message: 'Success', data: users });

// Set status code and send
res.status(201).json({ id: 5, name: 'Marta' });
res.status(404).json({ error: 'User not found' });
res.status(500).json({ error: 'Internal server error' });

// Shorthand for status + send
res.sendStatus(200);  // sends "OK"
res.sendStatus(404);  // sends "Not Found"
res.sendStatus(500);  // sends "Internal Server Error"

// Send empty response (no body)
res.end();
res.status(204).end(); // 204 = No Content

// Send a file for download
res.download('./report.pdf');
res.download('./report.pdf', 'user-report.pdf'); // with custom filename

// Redirect
res.redirect('/new-path');
res.redirect(301, '/permanent-new-path');

// Set a header
res.set('X-Custom-Header', 'value');

// Render a template (if using a template engine)
res.render('index', { title: 'Home', user: req.user });
```

---

## 5 — Routing

Routing is how Express decides which code runs for a given URL and HTTP method.

---

### Basic Routes

```js
app.get('/', (req, res) => res.send('Home page'));
app.get('/about', (req, res) => res.send('About page'));
app.post('/contact', (req, res) => res.send('Form submitted'));
```

---

### Route Parameters

Named segments of the URL, prefixed with `:`:

```js
// GET /users/123
app.get('/users/:id', (req, res) => {
  console.log(req.params.id); // "123"
  res.json({ userId: req.params.id });
});

// Multiple parameters
// GET /posts/42/comments/7
app.get('/posts/:postId/comments/:commentId', (req, res) => {
  const { postId, commentId } = req.params;
  res.json({ postId, commentId });
});
```

---

### Modular Routing with `express.Router()`

For larger apps, split routes into separate files:

```js
// routes/users.js
const express = require('express');
const router  = express.Router();

router.get('/',     (req, res) => res.json(getAllUsers()));
router.get('/:id',  (req, res) => res.json(getUserById(req.params.id)));
router.post('/',    (req, res) => res.json(createUser(req.body)));
router.put('/:id',  (req, res) => res.json(updateUser(req.params.id, req.body)));
router.delete('/:id', (req, res) => res.json(deleteUser(req.params.id)));

module.exports = router;
```

```js
// app.js
const usersRouter = require('./routes/users');
app.use('/api/users', usersRouter);

// Results in:
// GET    /api/users       → list all users
// GET    /api/users/:id   → get one user
// POST   /api/users       → create user
// PUT    /api/users/:id   → update user
// DELETE /api/users/:id   → delete user
```

---

### Route Chaining with `app.route()`

Cleaner when multiple methods share the same path:

```js
app.route('/users/:id')
  .get((req, res)    => res.json(getUser(req.params.id)))
  .put((req, res)    => res.json(updateUser(req.params.id, req.body)))
  .delete((req, res) => res.json(deleteUser(req.params.id)));
```

---

### Regular Expression Routes

Match multiple paths with one route:

```js
app.get(/\/post/, (req, res) => {
  // matches: /post, /post/first, /thepost, /posting/something
  res.send('Matched a post-related route');
});
```

---

## 6 — Middleware

Middleware is the backbone of Express. Understanding it deeply separates beginner Express developers from professional ones.

---

### What is Middleware?

A middleware function has access to `req`, `res`, and `next`. It can:
- Execute any code
- Modify `req` or `res`
- End the request-response cycle
- Call `next()` to pass control to the next middleware

```
Request → Middleware 1 → Middleware 2 → Middleware 3 → Route Handler → Response
            (logger)     (auth check)   (body parse)     (your code)
```

```js
const myMiddleware = (req, res, next) => {
  console.log(`${req.method} ${req.path} - ${new Date().toISOString()}`);
  next(); // MUST call next() or the request hangs forever
};
```

> ⚠️ If you don't call `next()` and don't send a response, the request will hang and the client will wait forever (until timeout).

---

### Types of Middleware

**1 — Application-level: runs for every request**

```js
app.use(myMiddleware); // runs on ALL routes
```

**2 — Route-level: runs for specific routes only**

```js
app.get('/admin', authMiddleware, (req, res) => {
  res.send('Admin panel');
});
```

**3 — Built-in Express middleware**

```js
app.use(express.json());                       // parse JSON bodies
app.use(express.urlencoded({ extended: true })); // parse form data
app.use(express.static('public'));              // serve static files
```

**4 — Third-party middleware (installed via npm)**

```js
const morgan      = require('morgan');
const cors        = require('cors');
const cookieParser = require('cookie-parser');

app.use(morgan('dev'));       // HTTP request logger
app.use(cors());              // enable CORS
app.use(cookieParser());      // parse cookies into req.cookies
```

---

### Middleware Order Matters

Middleware runs in the order it's registered. Place global middleware before routes:

```js
app.use(morgan('dev'));        // ✅ logs every request
app.use(express.json());       // ✅ parses body for all routes
app.use(cors());               // ✅ enables CORS

app.get('/users', handler);    // routes come after

app.use(errorHandler);         // ✅ error middleware LAST
```

---

### Passing Data Between Middleware

Use `req.locals` to attach data for downstream handlers:

```js
const attachUser = async (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (token) {
    req.user = await verifyToken(token); // attach to req
  }
  next();
};

app.use(attachUser);

app.get('/profile', (req, res) => {
  res.json(req.user); // available here
});
```

---

### Custom Logger Middleware (Example)

```js
const logger = (req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {
    const duration = Date.now() - start;
    console.log(`${req.method} ${req.originalUrl} ${res.statusCode} - ${duration}ms`);
  });
  next();
};

app.use(logger);
```

---

## 7 — Serving Static Files

Serve images, CSS, JavaScript, and HTML from a folder:

```js
app.use(express.static('public'));
```

With this, files in the `public/` folder are served at the root URL:
- `public/index.html` → `http://localhost:3000/index.html`
- `public/css/style.css` → `http://localhost:3000/css/style.css`
- `public/images/logo.png` → `http://localhost:3000/images/logo.png`

If you have `public/index.html`, it's served automatically at `/`.

**Serve from a virtual path prefix:**

```js
app.use('/static', express.static('public'));
// public/style.css → http://localhost:3000/static/style.css
```

**Send a single file:**

```js
app.get('/download', (req, res) => {
  res.download('./files/report.pdf');                       // triggers download
  res.download('./files/report.pdf', 'monthly-report.pdf'); // custom name
});
```

---

## 8 — Cookies

### Setting Cookies

```js
res.cookie('username', 'Marta');

// With options
res.cookie('token', 'abc123', {
  domain:   '.myapp.com',
  path:     '/',
  expires:  new Date(Date.now() + 7 * 24 * 60 * 60 * 1000), // 7 days
  maxAge:   7 * 24 * 60 * 60 * 1000,                         // in ms
  httpOnly: true,   // not accessible via JS (XSS protection)
  secure:   true,   // HTTPS only
  sameSite: 'Strict'
});

// Clear a cookie
res.clearCookie('username');
```

### Reading Cookies

Install `cookie-parser`:

```bash
npm install cookie-parser
```

```js
const cookieParser = require('cookie-parser');
app.use(cookieParser());

app.get('/', (req, res) => {
  console.log(req.cookies.username); // 'Marta'
});
```

> 🔒 **Security:** Always set `httpOnly: true` and `secure: true` for authentication cookies. This prevents XSS attacks from stealing tokens.

---

## 9 — HTTP Headers

### Reading Request Headers

```js
app.get('/', (req, res) => {
  console.log(req.headers);                   // all headers
  console.log(req.header('User-Agent'));       // one specific header
  console.log(req.headers['content-type']);   // another way
  console.log(req.headers.authorization);     // Bearer token
});
```

### Setting Response Headers

```js
res.set('Content-Type', 'application/json');
res.set('X-Custom-Header', 'MyValue');

// Shorthand for Content-Type
res.type('json');   // → application/json
res.type('html');   // → text/html
res.type('png');    // → image/png

// Multiple headers at once
res.set({
  'Content-Type': 'application/json',
  'X-Powered-By': 'Express',
  'Cache-Control': 'no-store'
});
```

---

## 10 — Redirects

```js
res.redirect('/new-location');           // 302 (temporary)
res.redirect(301, '/permanent-location'); // 301 (permanent)
res.redirect('https://example.com');     // external URL
res.redirect('../parent-path');          // relative
res.redirect('back');                    // back to Referer header (or '/')
```

---

## 11 — Sessions

Sessions allow you to persist user data across multiple HTTP requests. HTTP is stateless by default — sessions fix that.

```bash
npm install express-session
```

```js
const session = require('express-session');

app.use(session({
  secret:            process.env.SESSION_SECRET, // used to sign the session ID cookie
  resave:            false,   // don't save if session unchanged
  saveUninitialized: false,   // don't create session until data stored
  cookie: {
    httpOnly: true,
    secure:   process.env.NODE_ENV === 'production',
    maxAge:   24 * 60 * 60 * 1000 // 24 hours
  }
}));

// Set session data
app.post('/login', (req, res) => {
  req.session.userId   = user.id;
  req.session.userRole = user.role;
  res.json({ message: 'Logged in' });
});

// Read session data
app.get('/profile', (req, res) => {
  if (!req.session.userId) {
    return res.status(401).json({ error: 'Not authenticated' });
  }
  res.json({ userId: req.session.userId });
});

// Destroy session on logout
app.post('/logout', (req, res) => {
  req.session.destroy(err => {
    if (err) return res.status(500).json({ error: 'Could not logout' });
    res.clearCookie('connect.sid');
    res.json({ message: 'Logged out' });
  });
});
```

**Where session data is stored:**

| Store | Use Case |
|---|---|
| Memory (default) | Development only — resets on restart |
| MySQL / MongoDB | Simple persistence |
| Redis | Production — fast, scalable, survives restarts |

> 💡 For production, use Redis with `connect-redis`. The session ID is stored in a cookie; the data lives on the server.

---

## 12 — Input Validation & Sanitization

**Never trust user input.** Validate before using, sanitize before storing.

```bash
npm install express-validator
```

### Validation

```js
const { body, param, query, validationResult } = require('express-validator');

app.post('/users', [
  body('name')
    .isLength({ min: 3 })
    .withMessage('Name must be at least 3 characters')
    .isAlpha()
    .withMessage('Name must contain only letters'),

  body('email')
    .isEmail()
    .withMessage('Must be a valid email address'),

  body('age')
    .isInt({ min: 0, max: 120 })
    .withMessage('Age must be between 0 and 120'),

  body('password')
    .isLength({ min: 8 })
    .withMessage('Password must be at least 8 characters')

], (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(422).json({ errors: errors.array() });
  }
  // input is valid — proceed
  const { name, email, age } = req.body;
  res.status(201).json({ name, email, age });
});
```

### Sanitization (clean the data)

Chain sanitizers after validators:

```js
body('name').isLength({ min: 3 }).trim().escape(),
body('email').isEmail().normalizeEmail(),
body('age').isInt().toInt(),         // convert string → integer
body('active').isBoolean().toBoolean() // convert string → boolean
```

| Sanitizer | What it does |
|---|---|
| `.trim()` | Removes leading/trailing whitespace |
| `.escape()` | Converts `<`, `>`, `&`, `'`, `"` to HTML entities |
| `.normalizeEmail()` | Lowercases email, removes dots in Gmail |
| `.toInt()` | Converts to integer |
| `.toFloat()` | Converts to float |
| `.toBoolean()` | Converts to boolean |
| `.stripLow()` | Removes invisible ASCII control characters |

### Common Validators

```js
.isLength({ min: 3, max: 50 })
.isEmail()
.isURL()
.isNumeric()
.isInt()
.isFloat()
.isBoolean()
.isAlpha()
.isAlphanumeric()
.isJSON()
.isMobilePhone()
.isPostalCode('any')
.isISO8601()          // date format
.isIn(['admin', 'user', 'moderator']) // whitelist
.matches(/^[a-zA-Z]+$/) // regex
.custom(value => {    // completely custom logic
  if (alreadyExists(value)) throw new Error('Already taken');
})
```

---

## 13 — Handling Forms & File Uploads

### HTML Forms (URL-encoded)

```html
<form method="POST" action="/submit">
  <input type="text" name="username" />
  <input type="submit" />
</form>
```

```js
app.use(express.urlencoded({ extended: true }));

app.post('/submit', (req, res) => {
  const username = req.body.username;
  res.send(`Hello, ${username}`);
});
```

### File Uploads with Multer

```bash
npm install multer
```

```html
<form method="POST" action="/upload" enctype="multipart/form-data">
  <input type="file" name="avatar" />
  <input type="submit" />
</form>
```

```js
const multer  = require('multer');

// Store in memory (for processing)
const upload = multer({ storage: multer.memoryStorage() });

// Store on disk
const storage = multer.diskStorage({
  destination: (req, file, cb) => cb(null, './uploads/'),
  filename:    (req, file, cb) => cb(null, Date.now() + '-' + file.originalname)
});
const diskUpload = multer({
  storage,
  limits:    { fileSize: 5 * 1024 * 1024 }, // 5MB max
  fileFilter: (req, file, cb) => {
    const allowed = ['image/jpeg', 'image/png', 'image/gif'];
    if (allowed.includes(file.mimetype)) cb(null, true);
    else cb(new Error('Only images allowed'));
  }
});

// Single file
app.post('/upload', diskUpload.single('avatar'), (req, res) => {
  console.log(req.file.path);       // file saved path
  console.log(req.file.size);       // bytes
  console.log(req.file.mimetype);   // image/jpeg
  res.json({ filename: req.file.filename });
});

// Multiple files
app.post('/gallery', diskUpload.array('photos', 10), (req, res) => {
  console.log(req.files); // array of file objects
  res.json({ count: req.files.length });
});
```

---

## 14 — CORS

**Cross-Origin Resource Sharing** — browsers block requests from different origins by default. When your React frontend (`:3000`) calls your Express API (`:5000`), CORS must be enabled.

```bash
npm install cors
```

```js
const cors = require('cors');

// Allow all origins (development only!)
app.use(cors());

// Specific origins (production)
app.use(cors({
  origin:      ['https://myapp.com', 'https://www.myapp.com'],
  methods:     ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true // allow cookies to be sent cross-origin
}));

// Per-route CORS
app.get('/public-data', cors(), (req, res) => {
  res.json({ data: 'anyone can access this' });
});
```

> ⚠️ Never use `cors()` with no options in production. Specify exactly which origins are allowed.

---

## 15 — Error Handling

Error handling in Express is done with a special middleware that takes 4 parameters: `(err, req, res, next)`.

---

### Basic Error Handler

```js
// MUST be defined LAST, after all routes
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.statusCode || 500).json({
    status:  'error',
    message: err.message || 'Something went wrong'
  });
});
```

---

### Custom Error Class

```js
// utils/AppError.js
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.status     = statusCode >= 400 && statusCode < 500 ? 'fail' : 'error';
    Error.captureStackTrace(this, this.constructor);
  }
}

module.exports = AppError;
```

```js
const AppError = require('./utils/AppError');

app.get('/users/:id', async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) return next(new AppError('User not found', 404));
    res.json(user);
  } catch (err) {
    next(err); // pass to error middleware
  }
});
```

---

### Async Error Handling

Without wrapping, async errors are swallowed in Express 4:

```js
// ❌ Express 4 — async errors aren't caught automatically
app.get('/users', async (req, res) => {
  const users = await User.find(); // if this throws, server crashes
  res.json(users);
});

// ✅ Wrap in try/catch and call next(err)
app.get('/users', async (req, res, next) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (err) {
    next(err);
  }
});

// ✅ Or use a wrapper utility to avoid repeating try/catch
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

> 💡 **Express 5** (latest) handles async errors automatically — `async` route handlers that throw or reject are passed to the error middleware without needing `try/catch`.

---

### 404 Handler

Catch all unmatched routes:

```js
// Place AFTER all routes, BEFORE the error handler
app.use((req, res, next) => {
  next(new AppError(`Cannot ${req.method} ${req.originalUrl}`, 404));
});

// Then the error handler catches it
app.use((err, req, res, next) => {
  res.status(err.statusCode || 500).json({ message: err.message });
});
```

---

## 16 — Project Structure (MVC Pattern)

For real-world apps, organize your code in layers. The **MVC (Model-View-Controller)** pattern is the most common:

```
my-express-app/
├── app.js              ← main entry — creates Express app, registers middleware
├── server.js           ← starts the server (separates app from server)
├── .env                ← environment variables (never commit this)
├── .env.example        ← template for teammates
├── package.json
├── routes/
│   ├── index.js        ← mounts all routers
│   ├── userRoutes.js
│   └── productRoutes.js
├── controllers/
│   ├── userController.js
│   └── productController.js
├── models/             ← database models (Mongoose schemas, Sequelize models)
│   ├── User.js
│   └── Product.js
├── middleware/
│   ├── auth.js         ← authentication middleware
│   ├── validate.js     ← validation middleware
│   └── errorHandler.js
├── utils/
│   ├── AppError.js
│   └── asyncHandler.js
└── config/
    └── db.js           ← database connection
```

**Separation of concerns:**

```js
// routes/userRoutes.js — only routing
const router = require('express').Router();
const { getUsers, createUser, getUserById } = require('../controllers/userController');
const { protect } = require('../middleware/auth');

router.get('/',    protect, getUsers);
router.post('/',   createUser);
router.get('/:id', getUserById);

module.exports = router;
```

```js
// controllers/userController.js — only business logic
const User         = require('../models/User');
const AppError     = require('../utils/AppError');
const asyncHandler = require('../utils/asyncHandler');

exports.getUsers = asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json({ status: 'success', data: users });
});

exports.getUserById = asyncHandler(async (req, res, next) => {
  const user = await User.findById(req.params.id);
  if (!user) return next(new AppError('User not found', 404));
  res.json({ status: 'success', data: user });
});

exports.createUser = asyncHandler(async (req, res) => {
  const user = await User.create(req.body);
  res.status(201).json({ status: 'success', data: user });
});
```

```js
// app.js — only middleware and route mounting
const express = require('express');
const morgan  = require('morgan');
const cors    = require('cors');

const userRoutes    = require('./routes/userRoutes');
const errorHandler  = require('./middleware/errorHandler');

const app = express();

app.use(morgan('dev'));
app.use(cors());
app.use(express.json());

app.use('/api/users', userRoutes);

app.use((req, res, next) => next(new AppError(`Cannot ${req.method} ${req.url}`, 404)));
app.use(errorHandler);

module.exports = app;
```

```js
// server.js — only starts the server
require('dotenv').config();
const app = require('./app');

const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Server running on port ${port}`));
```

---

## 17 — Async/Await in Express

Node.js is asynchronous. Most operations in a real Express app (database queries, file reads, external API calls) are async. Here's the right way to handle them:

```js
// ✅ The correct pattern for every async route handler
app.get('/users/:id', async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    res.json(user);
  } catch (error) {
    next(error); // always pass errors to next()
  }
});
```

**Running async tasks in parallel:**

```js
app.get('/dashboard', async (req, res, next) => {
  try {
    // ❌ Sequential — unnecessarily slow
    const user  = await getUser(req.user.id);
    const posts = await getPosts(req.user.id);
    const stats = await getStats(req.user.id);

    // ✅ Parallel — 3x faster when tasks are independent
    const [user, posts, stats] = await Promise.all([
      getUser(req.user.id),
      getPosts(req.user.id),
      getStats(req.user.id)
    ]);

    res.json({ user, posts, stats });
  } catch (error) {
    next(error);
  }
});
```

---

## 18 — Security Best Practices

### Helmet — Set Security Headers

```bash
npm install helmet
```

```js
const helmet = require('helmet');
app.use(helmet()); // sets 15+ security headers automatically
```

### Rate Limiting — Prevent Brute Force

```bash
npm install express-rate-limit
```

```js
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max:      100,             // max 100 requests per windowMs per IP
  message:  'Too many requests, please try again later.'
});

app.use('/api/', limiter); // apply to all API routes
```

### Environment Variables — Never Hardcode Secrets

```js
// ❌ Never do this
const secret = 'my-super-secret-jwt-key-123';
const dbUrl  = 'mongodb+srv://admin:password@cluster.mongodb.net/mydb';

// ✅ Always use environment variables
const secret = process.env.JWT_SECRET;
const dbUrl  = process.env.DATABASE_URL;
```

### Additional Security Rules

```js
// Disable X-Powered-By header (don't advertise Express)
app.disable('x-powered-by'); // helmet does this too

// Prevent parameter pollution
npm install hpp
app.use(require('hpp')());

// Sanitize MongoDB queries (prevent NoSQL injection)
npm install express-mongo-sanitize
app.use(require('express-mongo-sanitize')());
```

### Security Checklist

- ✅ Use `helmet()` for security headers
- ✅ Rate limit all endpoints
- ✅ Validate and sanitize ALL input
- ✅ Use `httpOnly` and `secure` cookies
- ✅ Store secrets in `.env`, never in code
- ✅ Use HTTPS in production
- ✅ Implement proper error handling (don't leak stack traces)
- ✅ Use parameterized queries / ORM (prevent SQL injection)
- ✅ Keep dependencies updated (`npm audit`)

---

## 19 — Express 5 & Alternatives

### Express 5

Released in 2024 with key improvements:
- **Native async/await error handling** — async route handlers that throw are automatically passed to error middleware. No more `try/catch` everywhere.
- Cleaner API with fewer edge cases
- Largely backward compatible with Express 4

```js
// Express 5 — no try/catch needed, errors bubble up automatically
app.get('/users', async (req, res) => {
  const users = await User.find(); // throws? → goes to error middleware
  res.json(users);
});
```

### Alternatives Worth Knowing

| Framework | Key Feature | Best For |
|---|---|---|
| **Fastify** | 2x faster than Express, schema-based validation | Performance-critical APIs |
| **Koa** | By Express creators, lighter, uses `async/await` natively | Clean async code |
| **NestJS** | TypeScript-first, Angular-like structure, built on Express | Large enterprise apps |
| **Hono** | Ultra-fast, runs on Edge/Bun/Deno/Node | Modern edge deployments |
| **Elysia** | Bun-native, extreme performance | Bun runtime |

> 💡 Learn Express first. NestJS, Fastify, and the others are much easier once you understand what Express does under the hood.

---

## 20 — Learning Resources

### 📺 YouTube

| Resource | Best For |
|---|---|
| [Express.js Crash Course — Traversy Media](https://www.youtube.com/watch?v=L72fhGm1tfE) | Best beginner intro (1.5 hrs) |
| [REST API with Express — Traversy Media](https://www.youtube.com/watch?v=l8WPWK9mS5M) | Build a complete REST API |
| [Express.js Full Course — freeCodeCamp](https://www.youtube.com/watch?v=Oe421EPjeBE) | Comprehensive deep dive |
| [Node.js & Express — The Net Ninja](https://www.youtube.com/playlist?list=PL4cUxeGkcC9jszmQoUkCm7Kgb4T_Lhc0E) | Step-by-step series |
| [Express Middleware Explained — Codevolution](https://www.youtube.com/watch?v=lY6icfhap2o) | Middleware deep dive |
| [MVC with Express — Web Dev Simplified](https://www.youtube.com/watch?v=ntXJPivBFio) | Project structure & MVC |

### 📚 Documentation

| Resource | Link |
|---|---|
| Express Official Docs | [expressjs.com](https://expressjs.com) |
| Express API Reference | [expressjs.com/en/4x/api.html](https://expressjs.com/en/4x/api.html) |
| express-validator | [express-validator.github.io](https://express-validator.github.io/docs/) |
| Multer | [github.com/expressjs/multer](https://github.com/expressjs/multer) |

---

## 21 — Quick Knowledge Check

Try to answer these without looking back:

**Basics**
1. What is Express.js and how does it relate to Node.js?
2. What do `req` and `res` represent in a route handler?
3. What is the difference between `res.send()` and `res.json()`?
4. What does `app.use()` do vs `app.get()`?

**Routing & Middleware**
5. How do you access URL parameters like `/users/:id`?
6. What is the difference between `req.params`, `req.query`, and `req.body`?
7. What is middleware? What happens if you forget to call `next()`?
8. Why does middleware order matter in Express?
9. What is `express.Router()` and why use it?

**Error Handling & Security**
10. What is the signature of an Express error handling middleware?
11. Why must the error handler be registered last?
12. What does `helmet()` do?
13. What is rate limiting and why is it important?
14. Why should you never hardcode secrets in your Express code?

**Advanced**
15. What is the difference between sessions and cookies?
16. What is CORS and when do you need it?
17. What is the difference between validation and sanitization?
18. How does async error handling differ between Express 4 and Express 5?

---

> 🎯 **You've completed Module 03!**
>
> Move to **[Module 04 — REST APIs →](../04-REST-APIs/README.md)**
>
> You now know Express inside out. Time to design and build professional REST APIs. 🚀
