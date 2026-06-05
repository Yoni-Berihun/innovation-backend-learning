# 🟩 Module 02 — Node.js Fundamentals: Exercises & Answers

> 💡 **How to use this file:**
> - Create a project folder: `mkdir node-exercises && cd node-exercises && npm init -y`
> - Each exercise is a standalone `.js` file — create it, run it with `node filename.js`
> - Try it yourself first — only open the answer after a genuine attempt
> - The explanations are where the real learning happens

---

## 📖 Table of Contents

1. [Running Node.js & REPL](#1--running-nodejs--repl)
2. [The Module System](#2--the-module-system)
3. [Built-in Modules](#3--built-in-modules)
4. [NPM & package.json](#4--npm--packagejson)
5. [File System (fs)](#5--file-system-fs)
6. [The Event Loop & Async](#6--the-event-loop--async)
7. [Environment Variables](#7--environment-variables)
8. [HTTP Server](#8--http-server)
9. [Streams & Buffers](#9--streams--buffers)
10. [Final Project — Mini File-Based API](#10--final-project--mini-file-based-api)

---

## 1 — Running Node.js & REPL

---

### 🟢 Easy

**Exercise 1.1 — Your first Node script**

Create a file `hello.js`. Log:
- `"Hello, Node.js!"`
- The current Node.js version (hint: `process.version`)
- The current platform (hint: `process.platform`)
- The current working directory (hint: `process.cwd()`)

Run it with `node hello.js`

<details>
<summary>✅ Answer & Explanation</summary>

```js
// hello.js
console.log("Hello, Node.js!");
console.log("Node version:", process.version);
console.log("Platform:", process.platform);
console.log("Working directory:", process.cwd());
```

**Expected output (will vary by machine):**
```
Hello, Node.js!
Node version: v20.11.0
Platform: linux
Working directory: /home/user/node-exercises
```

**Explanation:**

`process` is a **global object** in Node.js — you never need to `require` it. It provides information about and control over the current Node.js process:

- `process.version` — the version of Node.js running
- `process.platform` — `"linux"`, `"darwin"` (macOS), or `"win32"`
- `process.cwd()` — current working directory (where you ran `node`)
- `process.env` — environment variables
- `process.exit(0)` — exit with success code
- `process.argv` — command line arguments

`process` is the equivalent of `window` in Node.js — it's always there, globally available.

</details>

---

**Exercise 1.2 — Command line arguments**

Create `args.js`. Make it accept two numbers as command-line arguments and log their sum.

Run as: `node args.js 15 27` → should log `42`

<details>
<summary>✅ Answer & Explanation</summary>

```js
// args.js
const args = process.argv;

// args[0] = 'node' (the executable)
// args[1] = '/path/to/args.js' (the script file)
// args[2] = first user argument
// args[3] = second user argument

const a = Number(args[2]);
const b = Number(args[3]);

if (isNaN(a) || isNaN(b)) {
  console.log("Usage: node args.js <number1> <number2>");
  process.exit(1);
}

console.log(`${a} + ${b} = ${a + b}`);
```

**Run:**
```bash
node args.js 15 27   # → 15 + 27 = 42
node args.js 100 50  # → 100 + 50 = 150
node args.js         # → Usage: node args.js <number1> <number2>
```

**Explanation:**

`process.argv` is an array containing:
- Index 0: The `node` binary path
- Index 1: The script file path
- Index 2+: Your actual arguments

That's why user arguments start at index 2, not 0. This catches everyone out at least once.

`Number(args[2])` converts the string `"15"` to the number `15` — command-line arguments are always strings. `isNaN()` checks if the conversion failed (e.g. user passed `"hello"` instead of a number).

`process.exit(1)` exits with a non-zero code — by convention, `0` means success, `1` means an error occurred. CI/CD systems and shell scripts use this exit code.

</details>

---

### 🟡 Medium

**Exercise 1.3 — Process info dashboard**

Create `sysinfo.js` that logs a formatted "system report":

```
=== Node.js System Report ===
Node Version  : v20.11.0
Platform      : linux
Architecture  : x64
CPU Cores     : 8
Total Memory  : 16.00 GB
Free Memory   : 4.23 GB
Uptime        : 3.45 hours
PID           : 12345
```

Hint: Use `os` module for CPU, memory, and uptime info.

<details>
<summary>✅ Answer & Explanation</summary>

```js
// sysinfo.js
const os = require('os');

const totalMem = (os.totalmem() / 1024 ** 3).toFixed(2);
const freeMem  = (os.freemem()  / 1024 ** 3).toFixed(2);
const uptime   = (os.uptime()   / 3600).toFixed(2);

console.log("=== Node.js System Report ===");
console.log(`Node Version  : ${process.version}`);
console.log(`Platform      : ${process.platform}`);
console.log(`Architecture  : ${os.arch()}`);
console.log(`CPU Cores     : ${os.cpus().length}`);
console.log(`Total Memory  : ${totalMem} GB`);
console.log(`Free Memory   : ${freeMem} GB`);
console.log(`Uptime        : ${uptime} hours`);
console.log(`PID           : ${process.pid}`);
```

**Explanation:**

The `os` module (built-in, no install) exposes operating system info:
- `os.totalmem()` / `os.freemem()` — memory in **bytes**. Dividing by `1024 ** 3` (1 GB in bytes) converts to gigabytes.
- `os.cpus()` — array of CPU core objects. `.length` gives the core count.
- `os.uptime()` — system uptime in **seconds**. Dividing by 3600 gives hours.
- `os.arch()` — CPU architecture: `"x64"`, `"arm64"`, etc.
- `process.pid` — the process ID assigned by the OS

This is the kind of code you'd write for health check endpoints in production APIs — logging server resources when a request comes in to help diagnose performance issues.

</details>

---

## 2 — The Module System

---

### 🟢 Easy

**Exercise 2.1 — Create and use a module**

Create two files:

`greetings.js` — export three functions:
- `sayHello(name)` → returns `"Hello, {name}!"`
- `sayGoodbye(name)` → returns `"Goodbye, {name}!"`
- `sayGoodMorning(name)` → returns `"Good morning, {name}!"`

`app.js` — import and use all three.

<details>
<summary>✅ Answer & Explanation</summary>

```js
// greetings.js
function sayHello(name)      { return `Hello, ${name}!`; }
function sayGoodbye(name)    { return `Goodbye, ${name}!`; }
function sayGoodMorning(name){ return `Good morning, ${name}!`; }

module.exports = { sayHello, sayGoodbye, sayGoodMorning };
```

```js
// app.js
const { sayHello, sayGoodbye, sayGoodMorning } = require('./greetings');

console.log(sayHello("Marta"));       // Hello, Marta!
console.log(sayGoodbye("Abel"));      // Goodbye, Abel!
console.log(sayGoodMorning("Hana")); // Good morning, Hana!
```

**Explanation:**

`module.exports` is an object. When you `require('./greetings')`, you receive whatever is assigned to `module.exports`. You can export:

```js
// Export multiple items (as an object)
module.exports = { sayHello, sayGoodbye };

// Export a single item (the module IS the function)
module.exports = sayHello;

// Add to exports one at a time
module.exports.sayHello = sayHello;
module.exports.PI = 3.14;
```

The `./` prefix is critical — it tells Node to look for the file relative to the current directory. Without it, Node looks inside `node_modules`.

</details>

---

### 🟡 Medium

**Exercise 2.2 — Module with state (counter)**

Create `counter.js` that exports a counter module with:
- `increment()` — increases count by 1
- `decrement()` — decreases count by 1
- `reset()` — sets count back to 0
- `getCount()` — returns current count

Demonstrate that requiring the same module from two different files gives the **same instance** (Node caches modules).

<details>
<summary>✅ Answer & Explanation</summary>

```js
// counter.js
let count = 0;

function increment() { count++; }
function decrement() { count--; }
function reset()     { count = 0; }
function getCount()  { return count; }

module.exports = { increment, decrement, reset, getCount };
```

```js
// app.js
const counter = require('./counter');

counter.increment();
counter.increment();
counter.increment();
console.log(counter.getCount()); // 3

counter.decrement();
console.log(counter.getCount()); // 2

// Require it again — same instance!
const counter2 = require('./counter');
console.log(counter2.getCount()); // 2 — NOT 0!

counter2.increment();
console.log(counter.getCount());  // 3 — counter sees counter2's change!
```

**Explanation — Module Caching:**

Node.js **caches modules** after the first `require`. Every subsequent `require('./counter')` returns the **exact same object** from cache — not a fresh copy.

This means:
- `counter` and `counter2` are the same object
- Changes through one variable are visible through the other
- `count` in `counter.js` persists between requires — it's not reset

**Why is this useful?** It enables the **singleton pattern** — one shared instance of a database connection, configuration object, or logger across your entire app.

```js
// db.js — only ONE connection is ever created, no matter how many files require this
const mongoose = require('mongoose');
mongoose.connect(process.env.DB_URL);
module.exports = mongoose; // Same connection everywhere
```

</details>

---

### 🔴 Challenging

**Exercise 2.3 — Module dependency chain**

Create three files that chain imports:

```
utils.js  →  validator.js  →  app.js
```

- `utils.js` — exports `capitalize(str)` and `truncate(str, maxLen)`
- `validator.js` — imports utils and exports `validateUser(user)` which checks `name` (required, min 2 chars) and `email` (required, must contain @). Returns `{ valid: boolean, errors: string[] }`
- `app.js` — imports validator and tests with valid and invalid users

<details>
<summary>✅ Answer & Explanation</summary>

```js
// utils.js
function capitalize(str) {
  if (!str) return '';
  return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
}

function truncate(str, maxLen) {
  if (str.length <= maxLen) return str;
  return str.slice(0, maxLen) + '...';
}

module.exports = { capitalize, truncate };
```

```js
// validator.js
const { capitalize } = require('./utils');

function validateUser(user) {
  const errors = [];

  if (!user.name || user.name.trim().length < 2) {
    errors.push('Name must be at least 2 characters');
  }

  if (!user.email || !user.email.includes('@')) {
    errors.push('Email must be a valid address');
  }

  return {
    valid: errors.length === 0,
    errors,
    // Normalize the name if valid
    normalized: errors.length === 0 ? { ...user, name: capitalize(user.name) } : null
  };
}

module.exports = { validateUser };
```

```js
// app.js
const { validateUser } = require('./validator');

const users = [
  { name: "marta",  email: "marta@example.com" },  // valid
  { name: "A",      email: "marta@example.com" },   // name too short
  { name: "Abel",   email: "not-an-email" },         // bad email
  { name: "",       email: "" },                     // both invalid
];

users.forEach(user => {
  const result = validateUser(user);
  if (result.valid) {
    console.log(`✅ Valid:`, result.normalized);
  } else {
    console.log(`❌ Invalid:`, result.errors);
  }
});
```

**Explanation:**

This models a real Node.js project structure:
- `utils.js` — pure helper functions, no dependencies
- `validator.js` — business logic, depends on utils
- `app.js` — entry point, depends on validator

This layered dependency approach prevents circular dependencies and makes each file easy to test in isolation. When `app.js` requires `validator.js`, Node first checks its cache — if `utils.js` was already loaded, it uses the cached version.

</details>

---

## 3 — Built-in Modules

---

### 🟢 Easy

**Exercise 3.1 — Path module**

Create `paths.js`. Given the path `/home/user/projects/my-app/src/index.js`, use the `path` module to extract:
- The directory name
- The file name (with extension)
- The file name (without extension)
- The file extension
- Join `__dirname`, `'data'`, and `'users.json'` into a full path

<details>
<summary>✅ Answer & Explanation</summary>

```js
// paths.js
const path = require('path');

const filePath = '/home/user/projects/my-app/src/index.js';

console.log(path.dirname(filePath));         // /home/user/projects/my-app/src
console.log(path.basename(filePath));        // index.js
console.log(path.basename(filePath, '.js')); // index
console.log(path.extname(filePath));         // .js

// Join paths safely — handles slashes for you
const dataPath = path.join(__dirname, 'data', 'users.json');
console.log(dataPath); // /absolute/path/to/current/dir/data/users.json

// path.resolve — creates an absolute path
const absPath = path.resolve('data', 'users.json');
console.log(absPath); // starts from cwd
```

**Explanation:**

The `path` module is **critical for file operations** in Node.js. Never manually concatenate paths with string `+` — it breaks on Windows (which uses `\` instead of `/`). Use `path.join()` which handles separators automatically.

`__dirname` is always the **absolute directory of the current file** — not where you ran `node` from. This makes your paths reliable regardless of where the script is executed from.

```js
// ❌ Fragile — breaks on Windows
const filePath = './data' + '/' + 'users.json';

// ✅ Robust — works everywhere
const filePath = path.join(__dirname, 'data', 'users.json');
```

</details>

---

### 🟡 Medium

**Exercise 3.2 — EventEmitter**

Create a custom event system for a simple order tracking app using Node's `EventEmitter`:

- Create an `OrderTracker` that extends `EventEmitter`
- Emit events: `'order:placed'`, `'order:processing'`, `'order:shipped'`, `'order:delivered'`
- Each event passes an order object `{ id, item, customer }`
- Listen to all events and log appropriate messages
- Test by simulating an order going through all stages

<details>
<summary>✅ Answer & Explanation</summary>

```js
// orderTracker.js
const EventEmitter = require('events');

class OrderTracker extends EventEmitter {
  placeOrder(order) {
    console.log(`📦 Order placed: ${order.item} for ${order.customer}`);
    this.emit('order:placed', order);
  }

  processOrder(order) {
    this.emit('order:processing', order);
  }

  shipOrder(order) {
    this.emit('order:shipped', order);
  }

  deliverOrder(order) {
    this.emit('order:delivered', order);
  }
}

const tracker = new OrderTracker();

// Register listeners
tracker.on('order:placed', (order) => {
  console.log(`✅ [${order.id}] Order confirmed. Preparing...`);
});

tracker.on('order:processing', (order) => {
  console.log(`🔄 [${order.id}] ${order.item} is being packed`);
});

tracker.on('order:shipped', (order) => {
  console.log(`🚚 [${order.id}] Out for delivery to ${order.customer}`);
});

tracker.on('order:delivered', (order) => {
  console.log(`🎉 [${order.id}] Delivered! Enjoy your ${order.item}!`);
});

// Simulate an order flow
const order = { id: "ORD-001", item: "Laptop", customer: "Marta" };

tracker.placeOrder(order);
setTimeout(() => tracker.processOrder(order), 1000);
setTimeout(() => tracker.shipOrder(order), 2000);
setTimeout(() => tracker.deliverOrder(order), 3000);
```

**Explanation:**

`EventEmitter` is the backbone of Node.js. The `http` module, `fs` streams, `net` sockets — they all extend EventEmitter.

Key methods:
- `.on(event, listener)` — register a persistent listener
- `.once(event, listener)` — listen only to the first occurrence
- `.emit(event, ...args)` — fire the event, passing data to all listeners
- `.off(event, listener)` — remove a specific listener
- `.removeAllListeners(event)` — remove all listeners for an event

**Observer pattern in action:** The `OrderTracker` (emitter) doesn't know or care about its listeners. Listeners don't know about each other. They're all decoupled — this is why it's so powerful for building scalable systems.

</details>

---

## 4 — NPM & package.json

---

### 🟢 Easy

**Exercise 4.1 — Set up a project from scratch**

1. Create a new folder `npm-practice`
2. Initialize with `npm init` (answer the prompts) and then with `npm init -y` (see the difference)
3. Install `lodash` as a dependency and `nodemon` as a devDependency
4. Add a `"start"` script that runs `node index.js` and a `"dev"` script that runs `nodemon index.js`
5. Create `index.js` that uses lodash's `_.chunk([1,2,3,4,5,6], 2)` and log the result

<details>
<summary>✅ Answer & Explanation</summary>

```bash
mkdir npm-practice && cd npm-practice
npm init -y
npm install lodash
npm install nodemon --save-dev
```

```json
// package.json (after edits)
{
  "name": "npm-practice",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

```js
// index.js
const _ = require('lodash');

const chunks = _.chunk([1, 2, 3, 4, 5, 6], 2);
console.log(chunks); // [[1, 2], [3, 4], [5, 6]]

const unique = _.uniq([1, 2, 2, 3, 3, 3, 4]);
console.log(unique); // [1, 2, 3, 4]
```

```bash
npm start    # runs: node index.js
npm run dev  # runs: nodemon index.js (restarts on file changes)
```

**Explanation:**

`npm init -y` skips all prompts and creates `package.json` with sensible defaults. Use it when prototyping.

`dependencies` vs `devDependencies`:
- `dependencies` — needed to **run** the app (lodash, express, mongoose)
- `devDependencies` — needed only during **development** (nodemon, jest, eslint)

When deploying to production, you can skip devDependencies with `npm install --production`. This keeps your production builds lean and reduces attack surface.

`nodemon` watches your files and automatically restarts Node when you save — a massive productivity boost during development. Without it, you'd need to manually stop and restart after every change.

</details>

---

### 🟡 Medium

**Exercise 4.2 — Semantic Versioning**

Given these version ranges in `package.json`, identify what versions would be installed:

```json
{
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "~7.5.0",
    "dotenv": "16.3.1",
    "lodash": ">=4.0.0 <5.0.0"
  }
}
```

1. What is the newest version of `express` that would be installed?
2. What is the newest version of `mongoose` that would be installed?
3. Will `dotenv` 16.3.2 be installed? Why?
4. Will `lodash` 5.0.0 be installed? Why?

<details>
<summary>✅ Answer & Explanation</summary>

```
1. express "^4.18.2"  → any 4.x.x ≥ 4.18.2 (e.g. 4.19.0, 4.20.1 — but NOT 5.0.0)
2. mongoose "~7.5.0"  → any 7.5.x ≥ 7.5.0  (e.g. 7.5.1, 7.5.9  — but NOT 7.6.0)
3. dotenv "16.3.1"    → ONLY 16.3.1 exactly. 16.3.2 would NOT be installed.
4. lodash ">=4.0.0 <5.0.0" → lodash 5.0.0 would NOT be installed (strict less than 5)
```

**Explanation — SemVer rules:**

`MAJOR.MINOR.PATCH` — `4.18.2`

- `^` (caret) — **compatible changes**: allows MINOR and PATCH updates, same MAJOR. Safe for most upgrades.
- `~` (tilde) — **patch-level changes only**: allows PATCH updates only, same MAJOR.MINOR. Most conservative.
- exact `"16.3.1"` — **pin to exact version**. Nothing else accepted.
- `>=` `<` — **range**: explicit mathematical comparison.

**Why does this matter?** In production, you want reproducible builds — the same code always installs the same packages. This is what `package-lock.json` is for: it locks the exact resolved version of every package, even transitive dependencies. With `package-lock.json` committed, `npm install` always installs the exact same tree.

</details>

---

## 5 — File System (fs)

---

### 🟢 Easy

**Exercise 5.1 — Read a JSON config file**

Create `config.json`:
```json
{
  "appName": "My Node App",
  "version": "1.0.0",
  "port": 3000,
  "database": {
    "host": "localhost",
    "name": "myapp_db"
  }
}
```

Create `readConfig.js` that reads and parses this file, then logs:
- The app name
- The port
- The database host

Handle the case where the file doesn't exist.

<details>
<summary>✅ Answer & Explanation</summary>

```js
// readConfig.js
const { readFile } = require('fs/promises');
const path = require('path');

async function loadConfig() {
  try {
    const filePath = path.join(__dirname, 'config.json');
    const raw = await readFile(filePath, 'utf8');
    const config = JSON.parse(raw);

    console.log(`App Name : ${config.appName}`);
    console.log(`Port     : ${config.port}`);
    console.log(`DB Host  : ${config.database.host}`);

    return config;
  } catch (error) {
    if (error.code === 'ENOENT') {
      console.error('Config file not found. Creating default...');
      // Could create a default config here
    } else if (error instanceof SyntaxError) {
      console.error('Config file contains invalid JSON:', error.message);
    } else {
      console.error('Unexpected error:', error.message);
    }
    return null;
  }
}

loadConfig();
```

**Explanation:**

`error.code === 'ENOENT'` — Node's file system errors have a `code` property. `ENOENT` means "Error NO ENTry" — the file or directory doesn't exist. Other common codes:
- `EACCES` — permission denied
- `EISDIR` — tried to read a directory as a file
- `EEXIST` — file already exists

Always use `path.join(__dirname, 'config.json')` instead of just `'./config.json'`. The former is an absolute path relative to the file, the latter is relative to wherever you run `node` from — which can differ.

Checking `error instanceof SyntaxError` handles malformed JSON files separately from missing files — two different problems that warrant different messages.

</details>

---

### 🟡 Medium

**Exercise 5.2 — Simple file-based logger**

Build a `logger.js` module that:
- Exports `log(level, message)` where `level` is `'INFO'`, `'WARN'`, or `'ERROR'`
- Writes to `app.log` in format: `[2024-01-15T10:30:00.000Z] [INFO] Server started`
- Also logs to `console` (in color if possible using ANSI codes)
- Exports `clearLog()` that empties the log file

<details>
<summary>✅ Answer & Explanation</summary>

```js
// logger.js
const { appendFile, writeFile } = require('fs/promises');
const path = require('path');

const LOG_FILE = path.join(__dirname, 'app.log');

// ANSI color codes
const colors = {
  INFO:  '\x1b[36m',  // Cyan
  WARN:  '\x1b[33m',  // Yellow
  ERROR: '\x1b[31m',  // Red
  RESET: '\x1b[0m'
};

async function log(level, message) {
  const timestamp = new Date().toISOString();
  const line = `[${timestamp}] [${level}] ${message}`;

  // Console with color
  console.log(`${colors[level] || ''}${line}${colors.RESET}`);

  // File without color codes (ANSI codes look ugly in log files)
  try {
    await appendFile(LOG_FILE, line + '\n');
  } catch (error) {
    console.error('Failed to write to log file:', error.message);
  }
}

async function clearLog() {
  try {
    await writeFile(LOG_FILE, '');
    console.log('Log file cleared.');
  } catch (error) {
    console.error('Failed to clear log:', error.message);
  }
}

module.exports = { log, clearLog };
```

```js
// app.js — test the logger
const logger = require('./logger');

async function main() {
  await logger.log('INFO', 'Server starting...');
  await logger.log('INFO', 'Connected to database');
  await logger.log('WARN', 'Memory usage above 80%');
  await logger.log('ERROR', 'Failed to process request: timeout');
  await logger.log('INFO', 'Server running on port 3000');
}

main();
```

**Explanation:**

This is a simplified version of what real logging libraries like `winston` and `pino` do internally.

`appendFile` adds content to the end of a file, creating it if it doesn't exist — perfect for logs. If you used `writeFile`, it would overwrite the entire log every time.

ANSI escape codes (`\x1b[31m`) add color in terminals. We use them in console output but strip them from the file — log files are often parsed by scripts or tools that would be confused by escape codes.

Real production loggers also:
- Rotate log files (new file each day or when size exceeds limit)
- Support multiple destinations (file, database, external service)
- Include structured JSON logging for easy parsing

</details>

---

### 🔴 Challenging

**Exercise 5.3 — Directory scanner**

Build `scanner.js` that:
1. Accepts a directory path as a command-line argument
2. Recursively lists all files (not directories)
3. Shows each file's name, size in KB, and last modified date
4. Shows a summary: total files, total size
5. Groups files by extension

```
📁 Scanning: /home/user/my-project
─────────────────────────────────────────
index.js          │  2.45 KB  │  2024-01-15
README.md         │  8.12 KB  │  2024-01-14
package.json      │  0.89 KB  │  2024-01-10
src/server.js     │  5.23 KB  │  2024-01-15

Summary: 4 files, 16.69 KB total

By extension:
  .js  : 2 files
  .md  : 1 file
  .json: 1 file
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// scanner.js
const fs = require('fs');
const path = require('path');

function scanDirectory(dirPath, baseDir = dirPath) {
  const results = [];

  try {
    const entries = fs.readdirSync(dirPath, { withFileTypes: true });

    for (const entry of entries) {
      const fullPath = path.join(dirPath, entry.name);

      if (entry.isDirectory()) {
        // Skip node_modules and hidden dirs
        if (entry.name === 'node_modules' || entry.name.startsWith('.')) continue;
        results.push(...scanDirectory(fullPath, baseDir));
      } else {
        const stats = fs.statSync(fullPath);
        results.push({
          path: path.relative(baseDir, fullPath),
          size: stats.size,
          modified: stats.mtime,
          ext: path.extname(entry.name) || '(no ext)'
        });
      }
    }
  } catch (error) {
    console.error(`Cannot read ${dirPath}: ${error.message}`);
  }

  return results;
}

function formatSize(bytes) {
  return (bytes / 1024).toFixed(2) + ' KB';
}

function formatDate(date) {
  return date.toISOString().split('T')[0];
}

// Main
const targetDir = process.argv[2] || '.';
const absDir = path.resolve(targetDir);

console.log(`\n📁 Scanning: ${absDir}`);
console.log('─'.repeat(60));

const files = scanDirectory(absDir);

// Display files
files.forEach(f => {
  const name   = f.path.padEnd(30);
  const size   = formatSize(f.size).padStart(10);
  const date   = formatDate(f.modified);
  console.log(`${name} │ ${size} │ ${date}`);
});

// Summary
const totalSize = files.reduce((sum, f) => sum + f.size, 0);
console.log(`\nSummary: ${files.length} files, ${formatSize(totalSize)} total`);

// Group by extension
const byExt = files.reduce((acc, f) => {
  acc[f.ext] = (acc[f.ext] || 0) + 1;
  return acc;
}, {});

console.log('\nBy extension:');
Object.entries(byExt)
  .sort((a, b) => b[1] - a[1])
  .forEach(([ext, count]) => console.log(`  ${ext.padEnd(8)}: ${count} file${count > 1 ? 's' : ''}`));
```

**Run:** `node scanner.js /path/to/project`

**Explanation:**

This uses **recursive directory traversal** — a common pattern in file system tools, build scripts, and deployment utilities.

`fs.readdirSync(path, { withFileTypes: true })` returns `Dirent` objects that have `.isDirectory()` and `.isFile()` methods — more efficient than calling `fs.statSync` on every entry just to check the type.

`fs.statSync(path)` returns file metadata: `size` (bytes), `mtime` (modified time), `ctime` (created time), etc.

`path.relative(baseDir, fullPath)` converts an absolute path to one relative to the base directory — so you see `src/server.js` instead of `/home/user/project/src/server.js`.

We skip `node_modules` because it can contain hundreds of thousands of files and would make the output useless.

</details>

---

## 6 — The Event Loop & Async

---

### 🟢 Easy

**Exercise 6.1 — Execution order prediction**

Without running the code, write down the expected output order. Then run it to verify.

```js
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

process.nextTick(() => console.log('4'));

setTimeout(() => console.log('5'), 100);

Promise.resolve().then(() => {
  console.log('6');
  process.nextTick(() => console.log('7'));
});

console.log('8');
```

<details>
<summary>✅ Answer & Explanation</summary>

```
1
8
4
3
6
7
2
5
```

**Explanation — execution order step by step:**

1. `console.log('1')` — synchronous, runs immediately
2. `setTimeout(cb, 0)` — macrotask, goes to timer queue
3. `Promise.resolve().then(→'3')` — microtask, queued
4. `process.nextTick(→'4')` — nextTick queue (highest priority microtask)
5. `setTimeout(cb, 100)` — macrotask, goes to timer queue (100ms)
6. `Promise.resolve().then(→'6')` — microtask queued
7. `console.log('8')` — synchronous, runs immediately

**After all synchronous code finishes:**
- nextTick queue runs first: `'4'`
- Promise microtasks run: `'3'`, then `'6'`
- After `'6'` runs, it queues another nextTick `'7'`
- That nextTick runs before any macrotasks: `'7'`
- Then macrotasks: `'2'` (0ms timer), then `'5'` (100ms timer)

**Key rules:**
1. All synchronous code runs first
2. `process.nextTick` runs before Promises
3. All microtasks (nextTick + Promises) drain before any macrotask runs
4. macrotasks run in order (timers by delay, then I/O)

</details>

---

### 🟡 Medium

**Exercise 6.2 — Async file operations chain**

Create `pipeline.js` that:
1. Reads `input.txt` (create it with some text)
2. Converts the content to uppercase
3. Counts the words
4. Writes a report to `output.txt` containing the uppercase text AND the word count
5. Logs "Pipeline complete" when done
6. All steps must be async using `fs/promises`

<details>
<summary>✅ Answer & Explanation</summary>

```js
// First create input.txt
// echo "hello world this is a test file with some words" > input.txt

// pipeline.js
const { readFile, writeFile } = require('fs/promises');
const path = require('path');

async function runPipeline() {
  try {
    console.log('Step 1: Reading input.txt...');
    const content = await readFile(path.join(__dirname, 'input.txt'), 'utf8');

    console.log('Step 2: Processing content...');
    const uppercase = content.toUpperCase();
    const wordCount = content.trim().split(/\s+/).length;

    console.log('Step 3: Writing output.txt...');
    const report = [
      `Word Count: ${wordCount}`,
      `─`.repeat(40),
      uppercase
    ].join('\n');

    await writeFile(path.join(__dirname, 'output.txt'), report, 'utf8');

    console.log('Pipeline complete!');
    console.log(`Processed ${wordCount} words.`);

  } catch (error) {
    if (error.code === 'ENOENT') {
      console.error('input.txt not found. Please create it first.');
    } else {
      console.error('Pipeline failed:', error.message);
    }
    process.exit(1);
  }
}

runPipeline();
```

**Explanation:**

Each `await` pauses the function until the Promise resolves — but only **this function** pauses. The event loop is free to handle other work while the file operation runs in libuv's thread pool.

`content.trim().split(/\s+/)` splits on any whitespace (spaces, tabs, newlines) and `.length` gives the word count. The regex `/\s+/` matches one or more whitespace characters — more robust than `.split(' ')` which only handles single spaces.

`process.exit(1)` in the catch block is important — without it, the process exits with code 0 (success) even on failure. Build tools and CI/CD pipelines check exit codes to know if a step succeeded.

</details>

---

### 🔴 Challenging

**Exercise 6.3 — Parallel vs Sequential performance test**

Create `performance.js` that simulates slow database queries and compares sequential vs parallel execution:

- Create a function `simulateDbQuery(queryName, delayMs)` that returns a Promise resolving after `delayMs`
- Run 5 queries sequentially and measure total time
- Run the same 5 queries in parallel with `Promise.all` and measure total time
- Log both results and the speedup factor

<details>
<summary>✅ Answer & Explanation</summary>

```js
// performance.js
function simulateDbQuery(queryName, delayMs) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve({ query: queryName, data: `Result of ${queryName}` });
    }, delayMs);
  });
}

const queries = [
  { name: 'getUser',         delay: 200 },
  { name: 'getUserPosts',    delay: 350 },
  { name: 'getNotifications',delay: 150 },
  { name: 'getUserSettings', delay: 100 },
  { name: 'getRecentActivity',delay: 400 }
];

async function runSequential() {
  console.log('\n⏳ Running queries sequentially...');
  const start = Date.now();

  const results = [];
  for (const q of queries) {
    const result = await simulateDbQuery(q.name, q.delay);
    results.push(result);
    console.log(`  ✓ ${q.name} completed (${q.delay}ms)`);
  }

  const duration = Date.now() - start;
  console.log(`Sequential total: ${duration}ms`);
  return duration;
}

async function runParallel() {
  console.log('\n⚡ Running queries in parallel...');
  const start = Date.now();

  const promises = queries.map(q => simulateDbQuery(q.name, q.delay));
  const results = await Promise.all(promises);

  const duration = Date.now() - start;
  results.forEach(r => console.log(`  ✓ ${r.query} completed`));
  console.log(`Parallel total: ${duration}ms`);
  return duration;
}

async function main() {
  const seqTime  = await runSequential();
  const parTime  = await runParallel();
  const speedup  = (seqTime / parTime).toFixed(2);
  const timeSaved = seqTime - parTime;

  console.log(`\n📊 Results:`);
  console.log(`  Sequential : ${seqTime}ms`);
  console.log(`  Parallel   : ${parTime}ms`);
  console.log(`  Speedup    : ${speedup}x faster`);
  console.log(`  Time saved : ${timeSaved}ms`);
}

main();
```

**Expected output:**
```
Sequential total: ~1200ms  (200+350+150+100+400)
Parallel total  : ~400ms   (longest single query)
Speedup         : ~3x faster
```

**Explanation:**

Sequential execution: each query waits for the previous one. Total = sum of all delays.

Parallel execution: all queries start simultaneously. Total ≈ the longest single query.

**The rule:** Use `Promise.all` when queries are **independent** (don't need each other's results). Use sequential `await` only when each step depends on the previous result.

```js
// ❌ Sequential — but these queries ARE independent — unnecessary slowdown
const user    = await getUser(id);
const posts   = await getPosts(userId);    // doesn't need user
const friends = await getFriends(userId);  // doesn't need user or posts

// ✅ Parallel — all start at the same time
const [user, posts, friends] = await Promise.all([
  getUser(id),
  getPosts(userId),
  getFriends(userId)
]);
```

In a real backend API handling 100+ requests per second, this difference compounds significantly. Saving 800ms per request across 100 concurrent requests = 80 seconds of saved computation per second.

</details>

---

## 7 — Environment Variables

---

### 🟢 Easy

**Exercise 7.1 — Config module**

1. Create `.env` with: `PORT=3000`, `NODE_ENV=development`, `APP_NAME=MyApp`, `MAX_CONNECTIONS=10`
2. Install `dotenv`
3. Create `config.js` that loads and exports all variables with sensible defaults
4. Create `app.js` that imports and uses the config

<details>
<summary>✅ Answer & Explanation</summary>

```bash
# .env
PORT=3000
NODE_ENV=development
APP_NAME=MyApp
MAX_CONNECTIONS=10
```

```js
// config.js
require('dotenv').config();

const config = {
  port:           parseInt(process.env.PORT, 10) || 3000,
  nodeEnv:        process.env.NODE_ENV || 'development',
  appName:        process.env.APP_NAME || 'Node App',
  maxConnections: parseInt(process.env.MAX_CONNECTIONS, 10) || 5,
  isDevelopment:  process.env.NODE_ENV === 'development',
  isProduction:   process.env.NODE_ENV === 'production'
};

// Validate required variables
if (!config.appName) {
  throw new Error('APP_NAME environment variable is required');
}

module.exports = config;
```

```js
// app.js
const config = require('./config');

console.log(`${config.appName} starting...`);
console.log(`Environment : ${config.nodeEnv}`);
console.log(`Port        : ${config.port}`);
console.log(`Max conn.   : ${config.maxConnections}`);

if (config.isDevelopment) {
  console.log('Running in development mode — verbose logging enabled');
}
```

**Explanation:**

`parseInt(process.env.PORT, 10)` — `process.env` values are always **strings**. `parseInt` with radix `10` converts `"3000"` to the number `3000`. Without this, comparisons like `config.port === 3000` would fail because `"3000" !== 3000`.

Always provide **defaults** using `|| defaultValue`. This ensures your app works even without a `.env` file (e.g. in production where env vars are set by the cloud provider, not a file).

The `isDevelopment` / `isProduction` helpers clean up conditional logic throughout your codebase:
```js
// Without helper:
if (process.env.NODE_ENV === 'development') { ... }
// With helper:
if (config.isDevelopment) { ... }
```

</details>

---

## 8 — HTTP Server

---

### 🟡 Medium

**Exercise 8.1 — REST-style HTTP server (no Express)**

Build an HTTP server that handles:
- `GET /` → `{ message: "Welcome to my API" }`
- `GET /users` → returns an array of user objects
- `GET /users/:id` → returns a single user or 404
- `POST /users` → reads JSON body, adds user, returns created user
- Any other route → 404

Test with `curl` or a REST client like Thunder Client / Postman.

<details>
<summary>✅ Answer & Explanation</summary>

```js
// server.js
const http = require('http');

// In-memory "database"
let users = [
  { id: 1, name: "Marta",  email: "marta@example.com" },
  { id: 2, name: "Abel",   email: "abel@example.com" },
  { id: 3, name: "Hana",   email: "hana@example.com" }
];
let nextId = 4;

function sendJSON(res, statusCode, data) {
  res.writeHead(statusCode, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify(data, null, 2));
}

function readBody(req) {
  return new Promise((resolve, reject) => {
    let body = '';
    req.on('data', chunk => body += chunk.toString());
    req.on('end',  () => {
      try { resolve(JSON.parse(body || '{}')); }
      catch (e) { reject(new Error('Invalid JSON')); }
    });
    req.on('error', reject);
  });
}

const server = http.createServer(async (req, res) => {
  const { method, url } = req;

  // GET /
  if (method === 'GET' && url === '/') {
    return sendJSON(res, 200, { message: 'Welcome to my API', version: '1.0.0' });
  }

  // GET /users
  if (method === 'GET' && url === '/users') {
    return sendJSON(res, 200, users);
  }

  // GET /users/:id
  const userMatch = url.match(/^\/users\/(\d+)$/);
  if (method === 'GET' && userMatch) {
    const user = users.find(u => u.id === parseInt(userMatch[1]));
    if (!user) return sendJSON(res, 404, { error: 'User not found' });
    return sendJSON(res, 200, user);
  }

  // POST /users
  if (method === 'POST' && url === '/users') {
    try {
      const body = await readBody(req);
      if (!body.name || !body.email) {
        return sendJSON(res, 400, { error: 'name and email are required' });
      }
      const newUser = { id: nextId++, name: body.name, email: body.email };
      users.push(newUser);
      return sendJSON(res, 201, newUser);
    } catch (error) {
      return sendJSON(res, 400, { error: error.message });
    }
  }

  // 404 fallback
  sendJSON(res, 404, { error: `Cannot ${method} ${url}` });
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
  console.log('Try: curl http://localhost:3000/users');
});
```

**Test commands:**
```bash
curl http://localhost:3000/
curl http://localhost:3000/users
curl http://localhost:3000/users/1
curl http://localhost:3000/users/99
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Yonas","email":"yonas@example.com"}'
```

**Explanation:**

`readBody` collects the request body — HTTP requests arrive in **chunks** (stream). We listen to `data` events, concatenate chunks, then parse JSON when `end` fires. This is exactly what `express.json()` middleware does internally — it's not magic, just stream processing.

`req.url.match(/^\/users\/(\d+)$/)` extracts the ID from the URL. `(\d+)` is a capture group matching one or more digits. This is basic routing — Express makes this much cleaner with `req.params.id`.

`sendJSON` is a helper to avoid repeating `writeHead` + `end` + `JSON.stringify` on every route. DRY (Don't Repeat Yourself) principle in action.

This exercise shows you what Express is doing for you. After this, you'll appreciate Express much more.

</details>

---

## 9 — Streams & Buffers

---

### 🟡 Medium

**Exercise 9.1 — File copy with streams**

Build `copyFile.js` that copies a file using streams (not `fs.copyFile` — do it manually with `createReadStream` and `createWriteStream`). Show progress as percentage.

```bash
node copyFile.js source.txt destination.txt
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// copyFile.js
const fs = require('fs');
const path = require('path');

const [,, src, dest] = process.argv;

if (!src || !dest) {
  console.error('Usage: node copyFile.js <source> <destination>');
  process.exit(1);
}

const srcPath  = path.resolve(src);
const destPath = path.resolve(dest);

// Get file size for progress calculation
const { size: totalSize } = fs.statSync(srcPath);
let bytesCopied = 0;

const readStream  = fs.createReadStream(srcPath);
const writeStream = fs.createWriteStream(destPath);

readStream.on('data', chunk => {
  bytesCopied += chunk.length;
  const pct = ((bytesCopied / totalSize) * 100).toFixed(1);
  process.stdout.write(`\rCopying... ${pct}%`); // \r overwrites the same line
});

readStream.on('error', err => {
  console.error('\nRead error:', err.message);
  process.exit(1);
});

writeStream.on('error', err => {
  console.error('\nWrite error:', err.message);
  process.exit(1);
});

writeStream.on('finish', () => {
  const kb = (totalSize / 1024).toFixed(2);
  console.log(`\n✅ Copied ${kb} KB: ${src} → ${dest}`);
});

// Pipe: connect read stream to write stream
readStream.pipe(writeStream);
```

**Explanation:**

`createReadStream` reads the file in chunks (default 64KB each). `createWriteStream` writes those chunks one by one. `pipe` connects them — data flows automatically, with **backpressure** handled internally (if the write stream is slow, the read stream pauses automatically).

`process.stdout.write('\r...')` overwrites the current terminal line (carriage return without newline) to show live progress without flooding the terminal with lines.

**Why streams?** Try copying a 4GB file. With `fs.readFile` + `fs.writeFile`, Node would load all 4GB into RAM before writing. With streams, memory usage stays at ~64KB regardless of file size.

</details>

---

## 10 — Final Project — Mini File-Based API

Build a complete, file-based REST API for managing a student list — no database, no Express. Data persists in `students.json`.

### Requirements:

```
GET  /students         → list all students
GET  /students/:id     → get one student
POST /students         → add a student { name, age, grade }
PUT  /students/:id     → update a student
DELETE /students/:id   → delete a student
```

Data persists across restarts (stored in `students.json`). Include input validation. Structure your code across multiple files: `server.js`, `studentController.js`, `fileStore.js`.

<details>
<summary>✅ Answer & Explanation</summary>

```js
// fileStore.js — handles all file I/O
const { readFile, writeFile } = require('fs/promises');
const path = require('path');

const DB_PATH = path.join(__dirname, 'students.json');

async function readAll() {
  try {
    const raw = await readFile(DB_PATH, 'utf8');
    return JSON.parse(raw);
  } catch (error) {
    if (error.code === 'ENOENT') return []; // File doesn't exist yet
    throw error;
  }
}

async function writeAll(data) {
  await writeFile(DB_PATH, JSON.stringify(data, null, 2));
}

module.exports = { readAll, writeAll };
```

```js
// studentController.js — business logic
const { readAll, writeAll } = require('./fileStore');

let nextId = null;

async function getNextId() {
  if (nextId === null) {
    const students = await readAll();
    nextId = students.length > 0 ? Math.max(...students.map(s => s.id)) + 1 : 1;
  }
  return nextId++;
}

function validate(body) {
  const errors = [];
  if (!body.name || body.name.trim().length < 2) errors.push('name must be at least 2 chars');
  if (!body.age  || isNaN(body.age) || body.age < 1 || body.age > 120) errors.push('age must be 1-120');
  if (!body.grade || !['A','B','C','D','F'].includes(body.grade.toUpperCase())) errors.push('grade must be A-F');
  return errors;
}

async function getAll()      { return readAll(); }

async function getById(id) {
  const students = await readAll();
  return students.find(s => s.id === id) || null;
}

async function create(body) {
  const errors = validate(body);
  if (errors.length) return { error: errors };

  const students = await readAll();
  const student = {
    id: await getNextId(),
    name: body.name.trim(),
    age: parseInt(body.age),
    grade: body.grade.toUpperCase(),
    createdAt: new Date().toISOString()
  };
  students.push(student);
  await writeAll(students);
  return student;
}

async function update(id, body) {
  const students = await readAll();
  const index = students.findIndex(s => s.id === id);
  if (index === -1) return null;

  students[index] = { ...students[index], ...body, id, updatedAt: new Date().toISOString() };
  await writeAll(students);
  return students[index];
}

async function remove(id) {
  const students = await readAll();
  const index = students.findIndex(s => s.id === id);
  if (index === -1) return false;

  students.splice(index, 1);
  await writeAll(students);
  return true;
}

module.exports = { getAll, getById, create, update, remove };
```

```js
// server.js — HTTP layer
const http = require('http');
const controller = require('./studentController');

function sendJSON(res, status, data) {
  res.writeHead(status, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify(data, null, 2));
}

function readBody(req) {
  return new Promise((resolve, reject) => {
    let body = '';
    req.on('data', chunk => body += chunk);
    req.on('end', () => {
      try { resolve(JSON.parse(body || '{}')); }
      catch { reject(new Error('Invalid JSON body')); }
    });
    req.on('error', reject);
  });
}

const server = http.createServer(async (req, res) => {
  const { method, url } = req;
  const idMatch = url.match(/^\/students\/(\d+)$/);
  const id = idMatch ? parseInt(idMatch[1]) : null;

  try {
    if (method === 'GET' && url === '/students') {
      return sendJSON(res, 200, await controller.getAll());
    }

    if (method === 'GET' && id) {
      const student = await controller.getById(id);
      return student
        ? sendJSON(res, 200, student)
        : sendJSON(res, 404, { error: 'Student not found' });
    }

    if (method === 'POST' && url === '/students') {
      const body   = await readBody(req);
      const result = await controller.create(body);
      return result.error
        ? sendJSON(res, 400, { errors: result.error })
        : sendJSON(res, 201, result);
    }

    if (method === 'PUT' && id) {
      const body   = await readBody(req);
      const result = await controller.update(id, body);
      return result
        ? sendJSON(res, 200, result)
        : sendJSON(res, 404, { error: 'Student not found' });
    }

    if (method === 'DELETE' && id) {
      const deleted = await controller.remove(id);
      return deleted
        ? sendJSON(res, 200, { message: 'Student deleted' })
        : sendJSON(res, 404, { error: 'Student not found' });
    }

    sendJSON(res, 404, { error: `Cannot ${method} ${url}` });

  } catch (error) {
    console.error(error);
    sendJSON(res, 500, { error: 'Internal server error' });
  }
});

server.listen(3000, () => {
  console.log('Student API running at http://localhost:3000');
  console.log('Endpoints:');
  console.log('  GET    /students');
  console.log('  GET    /students/:id');
  console.log('  POST   /students');
  console.log('  PUT    /students/:id');
  console.log('  DELETE /students/:id');
});
```

**Test it:**
```bash
# Create students
curl -X POST http://localhost:3000/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Marta","age":21,"grade":"A"}'

curl -X POST http://localhost:3000/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Abel","age":22,"grade":"B"}'

# List all
curl http://localhost:3000/students

# Get one
curl http://localhost:3000/students/1

# Update
curl -X PUT http://localhost:3000/students/1 \
  -H "Content-Type: application/json" \
  -d '{"grade":"A+"}'

# Delete
curl -X DELETE http://localhost:3000/students/2
```

**Explanation — what this covers:**

- **Three-layer architecture**: `server.js` (HTTP), `studentController.js` (business logic), `fileStore.js` (data). This mirrors how real Express apps are structured: routes → controllers → models.
- **Async file I/O**: Every operation reads from / writes to disk without blocking the event loop
- **Error handling**: ENOENT handled gracefully, 400/404/500 responses with meaningful messages
- **Input validation**: Centralized in `validate()`, reused across create and update
- **Module system**: Three files communicating through clean exports — no global state leakage
- **Persistence**: Data survives server restarts — restart the server and your students are still there

This project is essentially what you'll build in Module 04 — but with Express handling the HTTP boilerplate for you. After this, you'll appreciate exactly what Express eliminates.

</details>

---

> 🎯 **Exercises complete!**
>
> Move to **[Module 03 — Express.js →](../03-ExpressJS/README.md)**
>
> You've just built a REST API with zero dependencies. Now let's use Express to make this 10x faster to write! 🚀

---

## 11 — Interview Questions

> 🎯 These are real interview questions asked at companies using Node.js. Each answer is hidden — tap **"Show Answer"** only after you've thought through your own response.
> Go through these after completing the exercises above. If a concept feels unclear, go back to the README section for that topic.

---

### 🟢 Fresher Level

---

**Q1. How does Node.js work?**

<details>
<summary>Show Answer</summary>

Node.js works on a **single-threaded, event-driven architecture** using three main components:

- **V8 Engine** — compiles JavaScript into fast machine code
- **Event Loop** — handles asynchronous tasks (I/O, timers, requests) without blocking the main thread
- **libuv** — provides a background thread pool for file system, DNS, crypto, and compression operations

When a slow operation is triggered (like reading a file), Node delegates it to libuv's thread pool. The main thread stays free. When the operation completes, its callback is placed in the callback queue. The event loop picks it up when the call stack is empty and executes it.

This design allows Node.js to process thousands of concurrent connections efficiently without creating a new thread for each one.

</details>

---

**Q2. What is NPM?**

<details>
<summary>Show Answer</summary>

NPM stands for **Node Package Manager**. It is the default package manager bundled with Node.js and serves two purposes:

1. **Package registry** — a public repository of over 2 million reusable JavaScript packages that anyone can publish to and install from
2. **CLI tool** — command-line commands to install, update, remove, and manage packages in your project

Key concepts:
- `package.json` — metadata file that lists your project's dependencies, scripts, and configuration
- `node_modules/` — folder where installed packages live
- `package-lock.json` — exact snapshot of every installed package version for reproducible installs

Common commands: `npm install`, `npm run`, `npm init`, `npm update`, `npm uninstall`

</details>

---

**Q3. Why is Node.js single-threaded?**

<details>
<summary>Show Answer</summary>

Node.js is single-threaded by design because it's built on JavaScript, which itself is single-threaded. This design choice was intentional for several reasons:

- **Simplicity** — no race conditions, deadlocks, or complex thread synchronization to manage
- **Memory efficiency** — traditional multi-threaded servers create a new thread per connection; each thread consumes memory (typically ~1MB stack per thread). Node handles thousands of connections on one thread
- **Non-blocking I/O** — instead of using threads to wait for I/O, Node uses the event loop + callbacks/promises. I/O operations are delegated to libuv's thread pool in the background

The tradeoff: CPU-heavy tasks block the single thread. That's why CPU-intensive work should be offloaded to Worker Threads.

</details>

---

**Q4. If Node.js is single-threaded, how does it handle concurrency?**

<details>
<summary>Show Answer</summary>

Node.js handles concurrency through its **event-driven, non-blocking I/O model**:

1. A request comes in — Node starts processing it
2. If it hits a slow operation (database query, file read, API call), it hands it off to libuv's thread pool and moves on immediately
3. The main JavaScript thread is now free to handle the next request
4. When the slow operation completes, its callback is queued in the callback queue
5. The event loop picks it up when the call stack is empty and runs the callback

This is **concurrency without parallelism** — tasks don't run at the exact same time, but Node never sits idle waiting. It always moves to the next available task.

Example: 1000 users request a database query simultaneously. Node fires off all 1000 queries and handles the responses as they come back — never blocking on any single one.

</details>

---

**Q5. What is the difference between synchronous and asynchronous functions?**

<details>
<summary>Show Answer</summary>

| | Synchronous | Asynchronous |
|---|---|---|
| Execution | Blocks until complete | Continues immediately, completes later |
| Effect | Halts all other code | Other code runs while it completes |
| Error handling | `try/catch` | Callbacks, `.catch()`, or `try/catch` in `async` functions |
| Use case | Simple sequential logic | I/O operations, network requests, timers |
| Example | `fs.readFileSync()` | `fs.readFile()`, `fetch()`, `setTimeout()` |

```js
// Synchronous — blocks
const data = fs.readFileSync('./file.txt', 'utf8');
console.log(data); // waits for file read before continuing

// Asynchronous — non-blocking
fs.readFile('./file.txt', 'utf8', (err, data) => {
  console.log(data); // runs when ready
});
console.log('this runs BEFORE the file is read');
```

In a web server, using synchronous I/O inside a request handler blocks ALL incoming requests until it finishes. Always use async in request handlers.

</details>

---

**Q6. What are modules in Node.js?**

<details>
<summary>Show Answer</summary>

A module in Node.js is a **reusable block of code** encapsulated in its own file. Every file in Node.js is automatically a module — its variables and functions are private by default and must be explicitly exported to be used by other files.

There are three types:

1. **Built-in modules** — ship with Node.js, no install needed (`fs`, `http`, `path`, `os`, `events`, `crypto`)
2. **Local modules** — files you create yourself, imported with `require('./myFile')`
3. **Third-party modules** — installed via npm (`express`, `mongoose`, `dotenv`)

```js
// Exporting (CommonJS)
module.exports = { add, subtract };

// Importing
const { add } = require('./math');
```

Node.js caches modules after the first `require` — subsequent requires return the same cached instance.

</details>

---

**Q7. What is the purpose of the `require` keyword?**

<details>
<summary>Show Answer</summary>

`require()` is Node.js's built-in function for **loading modules**. It does three things:

1. **Resolves** the module path (file path or package name)
2. **Loads and executes** the module file
3. **Returns** whatever is assigned to `module.exports` in that file

```js
const fs      = require('fs');         // built-in module
const express = require('express');    // npm package (from node_modules)
const utils   = require('./utils');    // local file (relative path)
```

Resolution order when you call `require('something')`:
1. Check if it's a built-in module (`fs`, `http`, etc.)
2. Look for `./node_modules/something` in the current directory
3. Walk up the directory tree looking for `node_modules/` at each level
4. Throw `MODULE_NOT_FOUND` error if not found anywhere

Note: `require()` is **synchronous** — the file is loaded and executed before continuing. This is fine at startup but should never be called inside a request handler or hot code path.

</details>

---

**Q8. What is the V8 engine?**

<details>
<summary>Show Answer</summary>

V8 is Google's **open-source JavaScript engine**, written in C++. It's the same engine that powers Google Chrome and is what Node.js uses to execute JavaScript on the server.

What V8 does:
- **Compiles** JavaScript source code directly to machine code (not interpreted line by line)
- **Manages memory** via a heap allocator
- **Performs garbage collection** — automatically frees memory no longer referenced
- **Optimizes** frequently-run code paths (JIT compilation — Just In Time)

Two key components of V8:
- **Memory Heap** — where objects and variables are stored. `let x = 123` allocates space here
- **Call Stack** — tracks execution flow (LIFO). Each function call pushes a frame; each return pops it

Google invests heavily in V8 performance because it powers Gmail, Google Docs, and other Google apps. Node.js benefits from all of that investment.

</details>

---

**Q9. How to handle environment variables in Node.js?**

<details>
<summary>Show Answer</summary>

Environment variables are accessed via the global `process.env` object:

```js
const port = process.env.PORT || 3000;
const dbUrl = process.env.DATABASE_URL;
```

For local development, use the `dotenv` package to load variables from a `.env` file:

```bash
npm install dotenv
```

```js
// At the top of your entry file
require('dotenv').config();

const port = process.env.PORT; // now loaded from .env
```

`.env` file:
```
PORT=3000
DATABASE_URL=mongodb://localhost:27017/myapp
JWT_SECRET=your-secret-key
```

Best practices:
- **Never commit `.env`** to Git — add it to `.gitignore`
- **Always provide `.env.example`** with variable names but no real values
- **Centralize** all env var access in a single `config.js` module
- **Validate** required variables at startup so you fail fast if something is missing

In production (cloud), env vars are set through the hosting platform's dashboard or CLI — no `.env` file needed.

</details>

---

**Q10. What is REPL in Node.js?**

<details>
<summary>Show Answer</summary>

REPL stands for **Read-Evaluate-Print-Loop**. It's an interactive JavaScript shell that comes with Node.js.

Each letter describes a step:
- **Read** — reads the input you type
- **Evaluate** — executes the JavaScript
- **Print** — outputs the result
- **Loop** — repeats, waiting for the next input

Start it by typing `node` in your terminal with no file argument:

```bash
$ node
> 2 + 2
4
> 'hello'.toUpperCase()
'HELLO'
> const arr = [1,2,3]
undefined
> arr.map(x => x * 2)
[ 2, 4, 6 ]
> .exit
```

REPL is useful for:
- Quickly testing JavaScript expressions
- Debugging a specific piece of logic
- Experimenting with Node.js APIs without creating a file
- Checking what a function returns before using it in your code

Special REPL commands: `.exit`, `.help`, `.load <file>`, `.save <file>`

</details>

---

**Q11. What is `package.json` in Node.js?**

<details>
<summary>Show Answer</summary>

`package.json` is the **manifest file** for a Node.js project. It lives in the project root and contains:

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "A Node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.5.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest": "^29.0.0"
  }
}
```

Key fields:
- `name` / `version` — project identity
- `main` — entry point file
- `scripts` — custom commands runnable with `npm run <name>`
- `dependencies` — packages needed to run the app in production
- `devDependencies` — packages only needed during development (testing, linting, hot-reload)
- `engines` — specify required Node.js version

When you share your project, others run `npm install` to install all listed dependencies. This is why `node_modules/` doesn't need to be committed to Git.

</details>

---

**Q12. How to create a simple HTTP server in Node.js?**

<details>
<summary>Show Answer</summary>

Use the built-in `http` module — no external packages needed:

```js
const http = require('http');

const server = http.createServer((req, res) => {
  // req = IncomingMessage (request data)
  // res = ServerResponse (send response)

  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({ message: 'Hello, World!' }));
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

The callback passed to `createServer` runs for every incoming HTTP request. `req.url` and `req.method` let you handle different routes.

In production, you'd use **Express.js** on top of the `http` module — it provides routing, middleware, error handling, and many other conveniences that would otherwise require significant boilerplate.

</details>

---

**Q13. What are Promises in Node.js?**

<details>
<summary>Show Answer</summary>

A Promise is an **object representing the eventual completion or failure** of an asynchronous operation. It provides a cleaner alternative to nested callbacks.

A Promise has three states:
- **Pending** — operation in progress
- **Fulfilled** — operation succeeded (`.then()` runs)
- **Rejected** — operation failed (`.catch()` runs)

```js
// Creating a Promise
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) resolve({ data: 'result' });
    else reject(new Error('Something went wrong'));
  }, 1000);
});

// Consuming
fetchData
  .then(result => console.log(result))
  .catch(error => console.error(error))
  .finally(() => console.log('Always runs'));
```

**Promise chaining** solves callback hell:
```js
getUser(id)
  .then(user => getPosts(user.id))
  .then(posts => renderPage(posts))
  .catch(err => handleError(err));
```

**`async/await`** is syntactic sugar on top of Promises:
```js
async function loadPage() {
  try {
    const user  = await getUser(id);
    const posts = await getPosts(user.id);
    renderPage(posts);
  } catch (err) {
    handleError(err);
  }
}
```

</details>

---

### 🟡 Intermediate Level

---

**Q14. What is event-driven programming in Node.js?**

<details>
<summary>Show Answer</summary>

Event-driven programming is a paradigm where the flow of the program is determined by **events** — things that happen (user actions, I/O completions, timer expirations, messages).

In Node.js, event-driven programming is implemented through the `EventEmitter` class from the `events` module:

```js
const EventEmitter = require('events');
const emitter = new EventEmitter();

// Register a listener
emitter.on('data-received', (data) => {
  console.log('Got data:', data);
});

// Emit an event
emitter.emit('data-received', { user: 'Marta', score: 95 });
```

Node's core modules (http, fs, streams) all use EventEmitter internally. When you write:
```js
server.on('request', handler);
readStream.on('data', chunk => { ... });
```
...you're using the event-driven pattern.

Two key components:
1. **Event emitter** — fires events when something happens
2. **Event listener/handler** — function registered to respond to that event

This pattern decouples the producer of events from the consumers — neither needs to know about the other.

</details>

---

**Q15. What is a Buffer in Node.js?**

<details>
<summary>Show Answer</summary>

A `Buffer` is a **fixed-size chunk of raw binary memory** allocated outside of V8's JavaScript heap. It's used to handle binary data directly — useful when working with file I/O, network streams, cryptography, or image processing.

```js
// Create a buffer from a string
const buf = Buffer.from('Hello, Node!', 'utf8');
console.log(buf);           // <Buffer 48 65 6c 6c 6f 2c ...>
console.log(buf.toString()); // 'Hello, Node!'
console.log(buf.length);     // 12 bytes

// Allocate empty buffer (filled with zeros)
const empty = Buffer.alloc(16);

// Create from raw bytes
const raw = Buffer.from([0x48, 0x65, 0x6c, 0x6c, 0x6f]);
console.log(raw.toString()); // 'Hello'
```

When you `readFile` without specifying encoding, Node returns a Buffer:
```js
fs.readFile('./image.png', (err, data) => {
  // data is a Buffer — raw bytes of the image
});

fs.readFile('./text.txt', 'utf8', (err, data) => {
  // data is a string — decoded from buffer using utf8
});
```

Buffers are immutable in size once created. For resizable binary handling, use streams.

</details>

---

**Q16. What are Streams in Node.js?**

<details>
<summary>Show Answer</summary>

Streams are a way to **handle data in chunks** rather than loading it all into memory at once. They're essential for working with large files, network data, or any continuous data source.

Four types:

| Type | Direction | Example |
|---|---|---|
| Readable | Read data | `fs.createReadStream()`, `http.IncomingMessage` |
| Writable | Write data | `fs.createWriteStream()`, `http.ServerResponse` |
| Duplex | Both | TCP sockets |
| Transform | Modify as it passes through | `zlib.createGzip()` |

```js
// Reading a large file in chunks — never loads whole file into RAM
const readable = fs.createReadStream('./largefile.txt', 'utf8');

readable.on('data', chunk => process.stdout.write(chunk));
readable.on('end',  () => console.log('\nDone'));
readable.on('error', err => console.error(err));

// Piping — chain streams together
fs.createReadStream('./input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('./output.gz'));
```

**Why streams?** Copying a 4GB file with `readFileSync` would require 4GB of RAM. With streams, memory usage stays at ~64KB regardless of file size — only one chunk in memory at a time.

Streams also implement **backpressure** automatically — if the writer is slower than the reader, the reader pauses until the writer catches up.

</details>

---

**Q17. What is callback hell and how do you avoid it?**

<details>
<summary>Show Answer</summary>

Callback hell (also called the "pyramid of doom") is what happens when you nest multiple async callbacks inside each other:

```js
// ❌ Callback hell
getUser(id, (err, user) => {
  if (err) handleError(err);
  getPosts(user.id, (err, posts) => {
    if (err) handleError(err);
    getComments(posts[0].id, (err, comments) => {
      if (err) handleError(err);
      getLikes(comments[0].id, (err, likes) => {
        // It keeps going...
      });
    });
  });
});
```

Problems: hard to read, hard to debug, error handling repeated everywhere, impossible to maintain.

**Three solutions:**

1. **Promises with chaining:**
```js
getUser(id)
  .then(user => getPosts(user.id))
  .then(posts => getComments(posts[0].id))
  .then(comments => getLikes(comments[0].id))
  .catch(handleError); // ONE catch for everything
```

2. **async/await (most readable):**
```js
async function loadData(id) {
  try {
    const user     = await getUser(id);
    const posts    = await getPosts(user.id);
    const comments = await getComments(posts[0].id);
    const likes    = await getLikes(comments[0].id);
  } catch (err) {
    handleError(err);
  }
}
```

3. **Modularization** — break each callback into a named function so nesting is flattened.

</details>

---

**Q18. What is the difference between `setImmediate()` and `process.nextTick()`?**

<details>
<summary>Show Answer</summary>

Both schedule a callback to run asynchronously — but at different points in the event loop:

| | `process.nextTick()` | `setImmediate()` |
|---|---|---|
| When it runs | Before the event loop continues — after current operation, before I/O | In the **check phase** of the event loop — after I/O callbacks |
| Priority | Higher | Lower |
| Phases | Not a phase — runs between every phase | Check phase |
| Can block I/O? | Yes, if overused | No |

```js
setImmediate(() => console.log('setImmediate'));
process.nextTick(() => console.log('nextTick'));
console.log('sync');

// Output:
// sync
// nextTick   ← runs before event loop moves on
// setImmediate ← runs in check phase
```

**Practical rule:**
- Use `process.nextTick()` when you want something to run after the current synchronous code but before any I/O — useful for emitting events that listeners might not have registered yet
- Use `setImmediate()` when you want to run after I/O callbacks — safer, less risk of starving the event loop

> ⚠️ Recursively calling `process.nextTick()` in a loop will starve the event loop — I/O will never run.

</details>

---

**Q19. What is the difference between `spawn()` and `fork()` in Node.js?**

<details>
<summary>Show Answer</summary>

Both create child processes — but for different purposes:

| | `spawn()` | `fork()` |
|---|---|---|
| Purpose | Run any system command or external program | Create a new Node.js process running a JS file |
| IPC channel | Not by default | Built-in IPC channel for messaging |
| Communication | Via stdin/stdout streams | Via `process.send()` / `process.on('message')` |
| Use case | Running `ls`, `python script.py`, `ffmpeg` | Running CPU-heavy Node.js tasks in parallel |

```js
const { spawn } = require('child_process');

// spawn — run a system command
const ls = spawn('ls', ['-la', '/home']);
ls.stdout.on('data', data => console.log(data.toString()));

// fork — run a Node.js file with messaging
const { fork } = require('child_process');
const child = fork('./worker.js');

child.send({ task: 'compute', data: [1, 2, 3] });
child.on('message', result => console.log('Result:', result));
```

</details>

---

**Q20. What is CORS in Node.js?**

<details>
<summary>Show Answer</summary>

CORS stands for **Cross-Origin Resource Sharing**. It's a browser security mechanism that blocks web pages from making requests to a different domain than the one that served the page, unless the server explicitly allows it.

Example: Your frontend is at `http://localhost:3000` and your API is at `http://localhost:5000`. The browser will block the request by default — different ports = different origins.

**Why it matters for Node.js backends:** Your API needs to send the right HTTP headers to allow frontend requests from other origins.

Using the `cors` npm package with Express:

```js
const cors = require('cors');
const express = require('express');
const app = express();

// Allow all origins (development only)
app.use(cors());

// Specific origins (production)
app.use(cors({
  origin: ['https://myapp.com', 'https://www.myapp.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  credentials: true
}));
```

Without the `cors` package, you can set headers manually:
```js
res.setHeader('Access-Control-Allow-Origin', 'https://myapp.com');
res.setHeader('Access-Control-Allow-Methods', 'GET,POST,PUT,DELETE');
```

</details>

---

**Q21. How do you manage sessions in Node.js?**

<details>
<summary>Show Answer</summary>

Sessions allow you to persist data across multiple HTTP requests from the same user (HTTP is stateless by default).

Using `express-session`:

```js
const session = require('express-session');

app.use(session({
  secret: process.env.SESSION_SECRET, // signs the session ID cookie
  resave: false,            // don't save if session unchanged
  saveUninitialized: false, // don't create session until data is stored
  cookie: {
    secure: process.env.NODE_ENV === 'production', // HTTPS only in prod
    httpOnly: true,  // prevents client-side JS from reading cookie
    maxAge: 24 * 60 * 60 * 1000 // 24 hours
  }
}));

// Set session data
app.post('/login', (req, res) => {
  req.session.userId = user.id;
  req.session.role = user.role;
  res.json({ message: 'Logged in' });
});

// Read session data
app.get('/profile', (req, res) => {
  if (!req.session.userId) return res.status(401).json({ error: 'Not logged in' });
  res.json({ userId: req.session.userId });
});

// Destroy session on logout
app.post('/logout', (req, res) => {
  req.session.destroy();
  res.json({ message: 'Logged out' });
});
```

Session data is stored on the **server** by default (in memory). For production, use a persistent store like Redis (`connect-redis`) so sessions survive server restarts.

</details>

---

### 🔴 Experienced Level

---

**Q22. What is piping in Node.js?**

<details>
<summary>Show Answer</summary>

Piping is the process of **connecting the output of a readable stream directly to the input of a writable stream**. Data flows automatically between them without buffering the entire content in memory.

```js
const fs = require('fs');

// Without pipe — manual and verbose
const readable = fs.createReadStream('./input.txt');
const writable = fs.createWriteStream('./output.txt');

readable.on('data', chunk => {
  const canContinue = writable.write(chunk);
  if (!canContinue) readable.pause(); // backpressure
});
writable.on('drain', () => readable.resume());
readable.on('end', () => writable.end());

// With pipe — one line, handles everything
fs.createReadStream('./input.txt')
  .pipe(fs.createWriteStream('./output.txt'));
```

**Pipe handles backpressure automatically** — if the writable stream is slower than the readable, pipe pauses the readable until the writable drains.

**Chaining pipes** for multi-step transformations:

```js
const zlib = require('zlib');

// Read → Compress → Write, all streaming
fs.createReadStream('./video.mp4')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('./video.mp4.gz'))
  .on('finish', () => console.log('Compression complete'));
```

Piping is used everywhere: HTTP responses, file processing, database exports, log streaming.

</details>

---

**Q23. What is a cluster in Node.js?**

<details>
<summary>Show Answer</summary>

The `cluster` module allows you to create **multiple child processes** (workers) that all share the same server port. This takes advantage of multi-core machines — by default, Node.js only uses one CPU core.

```js
const cluster = require('cluster');
const http    = require('http');
const os      = require('os');

if (cluster.isMaster) {
  const numCPUs = os.cpus().length;
  console.log(`Master PID ${process.pid} running`);

  // Fork a worker for each CPU core
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code) => {
    console.log(`Worker ${worker.process.pid} died. Restarting...`);
    cluster.fork(); // auto-restart dead workers
  });

} else {
  // Each worker runs this
  http.createServer((req, res) => {
    res.end(`Hello from worker ${process.pid}`);
  }).listen(3000);

  console.log(`Worker PID ${process.pid} started`);
}
```

Key points:
- Master process forks workers; workers handle requests
- OS distributes incoming connections across workers
- Workers share the same port but run in separate processes (separate memory)
- If a worker crashes, the master can restart it

For most production apps, use **PM2** (`pm2 start app.js -i max`) which manages clustering automatically without you writing the cluster code.

</details>

---

**Q24. What's wrong with this code? How do you fix it?**

```js
new Promise((resolve, reject) => {
  throw new Error('error');
}).then(console.log);
```

<details>
<summary>Show Answer</summary>

**The problem:** There is no `.catch()` after `.then()`. When the error is thrown inside the Promise constructor, it becomes a **rejected Promise**. Without a `.catch()`, this rejection is unhandled — it will silently fail in older Node versions and throw an `UnhandledPromiseRejection` warning (or crash) in modern Node.js.

**The fix:**
```js
new Promise((resolve, reject) => {
  throw new Error('error');
})
  .then(console.log)
  .catch(console.error); // ← always handle rejections
```

**Detecting unhandled rejections globally** (a safety net, not a replacement for proper handling):
```js
process.on('unhandledRejection', (err) => {
  console.error('Unhandled rejection:', err);
  process.exit(1); // exit with error code
});
```

**Best practice:** Every Promise chain must have a `.catch()`. With `async/await`, wrap in `try/catch`. Never leave a Promise floating without rejection handling — silent failures are the hardest bugs to debug.

</details>

---

**Q25. What's the output of this code? Explain each step.**

```js
Promise.resolve(1)
  .then((x) => x + 1)
  .then((x) => { throw new Error('My Error') })
  .catch(() => 1)
  .then((x) => x + 1)
  .then((x) => console.log(x))
  .catch(console.error)
```

<details>
<summary>Show Answer</summary>

**Output: `2`**

Step-by-step trace:

```
Promise.resolve(1)          → fulfilled with 1
.then(x => x + 1)           → 1 + 1 = 2, fulfilled with 2
.then(x => { throw ... })   → throws, rejected with Error('My Error')
.catch(() => 1)             → catches the error, returns 1, FULFILLED with 1
.then(x => x + 1)           → 1 + 1 = 2, fulfilled with 2
.then(x => console.log(x))  → logs 2, returns undefined
.catch(console.error)       → no error, never runs
```

**Key insight:** `.catch()` doesn't just handle errors — it **recovers** the Promise chain. After a `.catch()` returns a value, the chain continues as fulfilled. The Promise chain resumes normally after the error is handled.

This is why the last `.catch(console.error)` never runs — the error was already handled in the earlier `.catch(() => 1)`.

This question is commonly asked to test whether candidates understand Promise chain recovery, not just basic error handling.

</details>

---

**Q26. What's wrong with this API key comparison function?**

```js
function checkApiKey(apiKeyFromDb, apiKeyReceived) {
  if (apiKeyFromDb === apiKeyReceived) {
    return true;
  }
  return false;
}
```

<details>
<summary>Show Answer</summary>

**The problem: Timing Attack vulnerability.**

String comparison with `===` is not constant-time — JavaScript (and V8) compares strings **character by character and stops as soon as it finds a mismatch**. This means:

- If the first character is wrong → comparison takes very little time
- If the first 20 characters are correct → comparison takes longer

An attacker can measure response times and **deduce the correct API key character by character**. This is a **timing attack** — a form of side-channel attack.

**The fix — use constant-time comparison:**

```js
const crypto = require('crypto');

function checkApiKey(apiKeyFromDb, apiKeyReceived) {
  // Both must be the same length (pad/hash first if needed)
  const a = Buffer.from(apiKeyFromDb);
  const b = Buffer.from(apiKeyReceived);

  if (a.length !== b.length) return false;

  // timingSafeEqual always compares all bytes — same time regardless of where mismatch is
  return crypto.timingSafeEqual(a, b);
}
```

`crypto.timingSafeEqual()` is designed specifically for this — it always takes the same amount of time regardless of where (or if) the values differ.

**Always use constant-time comparison for:** API keys, tokens, passwords, HMACs, and any secret credential comparison.

</details>

---

**Q27. What is the purpose of `NODE_ENV`?**

<details>
<summary>Show Answer</summary>

`NODE_ENV` is an environment variable that tells your application **which environment it's running in**. It's not built into Node.js — it's a convention the community adopted universally.

Common values: `development`, `production`, `test`, `staging`

```js
const isDev  = process.env.NODE_ENV === 'development';
const isProd = process.env.NODE_ENV === 'production';
```

How it's typically used:

```js
// Express automatically enables performance optimizations when NODE_ENV=production
// - Caches view templates
// - Caches CSS extensions
// - Generates less verbose error messages

// Custom usage in your code
if (process.env.NODE_ENV === 'development') {
  app.use(morgan('dev'));        // verbose request logging
  app.use(errorHandler());      // detailed error stack traces
}

if (process.env.NODE_ENV === 'production') {
  app.use(helmet());            // security headers
  app.use(compression());       // gzip responses
  // Don't show stack traces to end users
}
```

Set it:
```bash
NODE_ENV=production node server.js
# or in .env:
NODE_ENV=development
```

> ⚠️ **Never send detailed error messages (stack traces) to clients in production.** They expose your file structure, library versions, and logic — valuable information for attackers.

</details>

---

**Q28. What is a test pyramid in Node.js?**

<details>
<summary>Show Answer</summary>

The test pyramid describes the **ideal ratio** of different types of tests in your project:

```
         /\
        /  \
       / E2E\      ← Few (slow, expensive, fragile)
      /──────\
     /Integra-\
    / tion Tests\  ← Some (moderate speed)
   /─────────────\
  /   Unit Tests  \ ← Many (fast, cheap, isolated)
 /─────────────────\
```

**Unit Tests (base — most tests here):**
- Test individual functions/modules in isolation
- Dependencies are mocked/stubbed
- Very fast, run in milliseconds
- Example: `add(2, 3)` returns `5`

**Integration Tests (middle):**
- Test how components work together
- May use a real or in-memory database
- Slower than unit tests
- Example: POST `/users` creates a record in the database

**End-to-End Tests (top — fewest):**
- Test the entire system from user perspective
- Hit real endpoints, real database
- Slowest, most brittle
- Example: simulate a full user login flow

**In Node.js with Express, a typical test stack:**
```
Unit Tests      → Jest or Mocha + Chai
Integration     → Supertest (HTTP testing) + Jest
E2E             → Playwright or Cypress
```

The pyramid ratio prevents "ice cream cone" anti-pattern — too many E2E tests, not enough units — which results in a slow, brittle test suite.

</details>

---

**Q29. When are worker processes useful and how do you handle them?**

<details>
<summary>Show Answer</summary>

Worker processes are useful when you need to **perform CPU-intensive or time-consuming tasks without blocking the main event loop**.

Common use cases:
- Sending bulk emails
- Image/video processing
- PDF generation
- Machine learning inference
- Large data aggregation/reporting
- Cryptographic operations at scale

**Option 1 — Worker Threads (same process, separate thread):**
```js
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

if (isMainThread) {
  const worker = new Worker(__filename, {
    workerData: { numbers: [1, 2, 3, 4, 5] }
  });
  worker.on('message', result => console.log('Sum:', result));
} else {
  const sum = workerData.numbers.reduce((a, b) => a + b, 0);
  parentPort.postMessage(sum);
}
```

**Option 2 — Child Process (separate Node.js process):**
```js
const { fork } = require('child_process');
const child = fork('./heavyTask.js');
child.send({ data: largeDataset });
child.on('message', result => console.log(result));
```

**Option 3 — Message Queue (production scale):**

For distributed systems, use a message queue (RabbitMQ, BullMQ with Redis, AWS SQS):
```js
// Publisher — add task to queue
await queue.add('send-email', { to: 'user@email.com', template: 'welcome' });

// Worker — process tasks from queue
queue.process('send-email', async (job) => {
  await sendEmail(job.data);
});
```

Message queues persist tasks across restarts, support retry logic, and can scale workers horizontally.

</details>

---

> 🏁 **Interview prep complete!**
>
> Review any answers that surprised you, then go back to the README to deepen your understanding of those specific topics.
>
> Move to **[Module 03 — Express.js →](../03-ExpressJS/README.md)** 🚀
