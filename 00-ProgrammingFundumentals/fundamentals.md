# 🖥️ Module 00 — Programming Fundamentals

> 👋 **Before you write a single line of code, you need to understand the world you're entering.**
> This module gives you the mental model every developer carries — what programming is, how the web works, what the different roles mean, and how to set up your tools properly.

---

## 📖 Table of Contents

1. [Why Programming Matters](#1--why-programming-matters)
2. [What is a Computer, Really?](#2--what-is-a-computer-really)
3. [What is Programming?](#3--what-is-programming)
4. [What is a Program?](#4--what-is-a-program)
5. [What is a Programming Language?](#5--what-is-a-programming-language)
6. [Common Programming Languages](#6--common-programming-languages)
7. [A Quick History of Programming](#7--a-quick-history-of-programming)
8. [What Does a Programmer Actually Do?](#8--what-does-a-programmer-actually-do)
9. [Frontend, Backend & Fullstack — Explained](#9--frontend-backend--fullstack--explained)
10. [The Web Development Ecosystem](#10--the-web-development-ecosystem)
11. [Setting Up Your Development Environment](#11--setting-up-your-development-environment)
12. [Visual Studio Code — Your Main Tool](#12--visual-studio-code--your-main-tool)
13. [Essential VS Code Extensions](#13--essential-vs-code-extensions)
14. [The Programming Mindset](#14--the-programming-mindset)
15. [Quick Exercises](#15--quick-exercises)

---

## 1 — Why Programming Matters

Every time your phone alarm rings, a message arrives, or a payment goes through — a programmer turned a real-world problem into a precise set of instructions for a machine.

Programming is the skill of **translating a problem into steps a computer can follow**.

- Computers are incredibly fast at repeating tasks
- Computers are completely literal — they do exactly what you tell them
- The quality of a program depends entirely on how clearly you describe the steps

> 🧠 Programmers don't make machines smart. Programmers make machines **useful**.

---

## 2 — What is a Computer, Really?

A computer is a machine that can:

- **Store** information
- **Retrieve** information
- **Perform calculations**
- **Make simple decisions**

It can do all of that **billions of times per second**.

But a computer is not intelligent. It cannot decide what to do on its own. It only follows the instructions it receives. Those instructions are called a **program**. The person who writes them is a **programmer**.

---

## 3 — What is Programming?

> **Programming is the act of giving a computer a precise set of instructions to solve a problem.**

The key word is **precise**.

A human can interpret vague instructions and fill in gaps automatically. A computer cannot.

### 🍵 The Tea Analogy

You could tell a friend "make me tea" and they'd figure it out. But a computer would need every single step spelled out:

1. Walk to the kitchen
2. Locate the kettle
3. Fill the kettle with water
4. Switch the kettle on
5. Wait for the water to boil
6. Place a teabag in a mug
7. Pour the boiling water into the mug
8. Wait 3 minutes
9. Remove the teabag
10. Serve the tea

That illustrates why programming is about **clarity and structure** above all else.

---

## 4 — What is a Program?

A program is a **complete set of instructions** that tells a computer how to carry out a task.

Programs can be:
- **Tiny** — convert Celsius to Fahrenheit
- **Medium** — calculate the fastest route on a map
- **Huge** — run a bank or a social network

What they all share: clear instructions that produce a result.

### Everyday Programs You Already Use

| Program | What It Does |
|---|---|
| Phone operating system | Manages hardware and runs apps |
| Web browser | Loads, renders, and runs websites |
| Messaging app | Sends and receives messages in real time |
| Navigation app | Calculates routes and tracks location |
| Streaming service | Serves video on demand to millions |

---

## 5 — What is a Programming Language?

Computers only understand **machine code** — zeros and ones.

A programming language is a **human-friendly way to write instructions**. A tool then converts those instructions into machine code. This process is called **compilation** or **interpretation**.

### Why Languages Exist

Writing directly in binary would be:
- Unreadably slow to write
- Almost impossible to debug
- Completely unmaintainable

Languages let humans write code that is readable, writeable, and improvable.

### Binary vs Human-Friendly Code

Binary for the word "Hello":
```
01001000 01100101 01101100 01101100 01101111
```

JavaScript for the same result:
```js
console.log('Hello');
```

---

## 6 — Common Programming Languages

There are hundreds of languages. The fundamentals are shared across all of them.

| Language | Best Known For |
|---|---|
| **JavaScript** | The language of the web — browser interactivity AND server-side with Node.js |
| **Python** | Beginner-friendly, automation, data science, AI/ML, backends |
| **Java** | Enterprise systems, Android apps, large-scale backends |
| **C / C++** | Systems programming, games, performance-critical software |
| **Swift** | Apple iOS and macOS apps |
| **Kotlin** | Android apps (modern replacement for Java on Android) |
| **Go** | Backend services and cloud infrastructure |
| **Rust** | Safety-focused systems programming |
| **PHP** | Web backends (still powers ~77% of websites) |
| **SQL** | Querying and managing databases |
| **TypeScript** | Typed superset of JavaScript — used in large-scale JS projects |

> 💡 In this course, you are learning **JavaScript** — the only language that works both in the browser (frontend) and on the server (backend with Node.js). One language, everywhere.

### Do Language Differences Matter for Beginners?

Not at first. All languages share the same core ideas:
- Variables and data types
- Conditions and control flow
- Loops
- Functions
- Data structures
- Error handling

Learning the fundamentals in JavaScript means you can pick up Python, Go, or any other language much faster later.

---

## 7 — A Quick History of Programming

Understanding the history helps you appreciate the tools you use today.

### 1800s — The First Ideas

**Charles Babbage** designed the *Analytical Engine* — a mechanical machine that could follow instructions.

**Ada Lovelace** wrote notes on how it could solve problems — she is widely considered the world's first programmer.

### 1940s — First Electronic Computers

Early computers filled entire rooms and were programmed by physically wiring circuits or typing binary.

### 1950s — Assembly and High-Level Languages

Assembly made programs slightly more readable. Then came **FORTRAN** and **COBOL** — the first high-level languages that let humans write something resembling readable code.

### 1970s–1990s — Modern Foundations

- **C** became the standard for systems programming
- **C++** added object-oriented design
- **Java** introduced write-once-run-anywhere portability
- **JavaScript** was born in 1995 — added interactivity to the web, later became server-side with Node.js

### Today

Modern programming is more powerful, more collaborative, and more accessible than ever. Tools help you write safer code, share it with teams, and fix errors quickly. Open source communities mean millions of people contribute to tools you use daily.

---

## 8 — What Does a Programmer Actually Do?

Programming is much more than typing code. Here's what a typical day looks like:

### Step 1 — Understand the Problem
What are we building? Who will use it? What are the edge cases? What can go wrong?

### Step 2 — Design the Solution
How should data flow through the system? What are the components? How do they connect?

### Step 3 — Write the Code
Translate the design into precise instructions in a programming language.

### Step 4 — Test It
Does it work with expected inputs? Does it handle unexpected inputs gracefully?

### Step 5 — Debug It
Find and fix the mistakes. Debugging is a skill — it gets faster with experience.

### Step 6 — Review and Maintain
Collaborate with teammates. Improve the code over time as requirements change.

> 🧠 A programmer spends more time **reading and debugging code** than writing new code. Writing clearly from the start saves everyone time.

---

## 9 — Frontend, Backend & Fullstack — Explained

This is one of the most important sections in this module. These words get thrown around constantly in the industry — you need to understand them clearly.

---

### 🖥️ What is the Frontend?

The **frontend** is everything the user **sees and interacts with** in their browser or app.

When you open a website:
- The layout, colors, fonts — **CSS**
- The buttons, forms, text — **HTML**
- The click effects, animations, real-time updates — **JavaScript**

All of that is frontend. It runs **inside the user's browser**.

```
User's Browser
┌────────────────────────────────────┐
│  HTML  →  Structure                │
│  CSS   →  Style                    │
│  JS    →  Behavior & Interactivity │
└────────────────────────────────────┘
```

**Frontend Technologies:**

| Technology | Role |
|---|---|
| HTML | Defines page structure and content |
| CSS | Styles and layout |
| JavaScript | Interactivity and dynamic behavior |
| React / Vue / Angular | JavaScript frameworks for building complex UIs |
| TypeScript | Typed JavaScript for large codebases |
| Tailwind CSS / Bootstrap | CSS frameworks for faster styling |

**Frontend developers** build what users see. Their main concerns are: user experience (UX), accessibility, performance, and browser compatibility.

> 💼 **Industry term:** Frontend is also called *client-side* — because the code runs on the client (user's device).

---

### ⚙️ What is the Backend?

The **backend** is everything that happens **behind the scenes** — on a server, not in the user's browser.

When you click "Login" on a website:
1. Your browser sends your email and password to a **server**
2. The server checks them against a **database**
3. If correct, it sends back a **token** or session
4. Your browser stores that and lets you in

All of steps 1–3 happen on the backend. The user never sees this code.

```
Server (Backend)
┌────────────────────────────────────────┐
│  Receives HTTP requests from browsers  │
│  Processes business logic              │
│  Reads/writes to databases             │
│  Returns data (usually JSON)           │
│  Handles authentication & security     │
└────────────────────────────────────────┘
```

**Backend Technologies:**

| Technology | Role |
|---|---|
| Node.js | JavaScript runtime — runs JS on the server |
| Express.js | Web framework for Node.js |
| Python (Django/Flask) | Backend frameworks in Python |
| Java (Spring Boot) | Enterprise backend framework |
| Go | High-performance backend services |
| PostgreSQL / MySQL | Relational databases |
| MongoDB | NoSQL document database |
| Redis | In-memory caching & session storage |

**Backend developers** build APIs, handle business logic, manage data, and keep the system secure. Their main concerns are: performance, security, data integrity, and scalability.

> 💼 **Industry term:** Backend is also called *server-side* — because the code runs on a server.

---

### 🌐 How Frontend and Backend Communicate

They talk through **HTTP requests**. The frontend sends a request, the backend processes it and sends a response — almost always in **JSON** format.

```
Browser (Frontend)          Server (Backend)
      │                           │
      │  GET /api/users           │
      │ ─────────────────────────▶│
      │                           │  reads database
      │  200 OK + JSON data       │
      │ ◀─────────────────────────│
      │                           │
      │  POST /api/login          │
      │ ─────────────────────────▶│
      │                           │  checks password
      │  200 OK + JWT token       │
      │ ◀─────────────────────────│
```

This separation is called a **REST API** — which you will build in Module 04.

---

### 🗄️ What is a Database?

A **database** is where your application stores data permanently. Without it, everything resets when the server restarts.

| Type | Examples | Best For |
|---|---|---|
| **Relational (SQL)** | PostgreSQL, MySQL, SQLite | Structured data with relationships |
| **NoSQL (Document)** | MongoDB | Flexible, unstructured data |
| **Key-Value** | Redis | Caching, sessions, real-time |
| **Graph** | Neo4j | Highly connected data (social networks) |

---

### 🔗 What is Fullstack?

A **fullstack developer** works on both the frontend and the backend.

```
┌─────────────────────────────────────────────────┐
│                  FULLSTACK DEVELOPER             │
│                                                  │
│  Frontend          +         Backend             │
│  (HTML/CSS/JS/     +         (Node.js/Express/   │
│   React/Vue)       +          DB/APIs)           │
└─────────────────────────────────────────────────┘
```

Fullstack doesn't mean you're equally deep in both — it means you can build a complete feature from the UI to the database by yourself.

> 💡 **This course makes you fullstack-ready.** You'll learn JavaScript (frontend) + Node.js + Express + Database integration (backend).

---

### 📱 What About Mobile?

Mobile apps have their own stack:

| Platform | Technologies |
|---|---|
| **iOS (Apple)** | Swift, SwiftUI, Objective-C |
| **Android** | Kotlin, Java |
| **Cross-platform** | React Native (JavaScript), Flutter (Dart) |

With React Native, JavaScript developers can build mobile apps too — another reason JavaScript is so powerful to learn.

---

### ☁️ What is DevOps / Cloud?

**DevOps** is the practice of deploying, monitoring, and scaling applications. It bridges development and operations.

| Tool / Service | Role |
|---|---|
| **Docker** | Package apps in containers that run anywhere |
| **AWS / Azure / GCP** | Cloud providers to host your app |
| **GitHub Actions / CI/CD** | Automate testing and deployment |
| **Nginx** | Web server and reverse proxy |
| **PM2** | Process manager for Node.js in production |

> 💡 You'll get an introduction to deployment in Module 08. For now, just know that DevOps is how your app goes from your laptop to the internet.

---

### 🗺️ The Full Picture

```
┌──────────────────────────────────────────────────────────────────┐
│                        WEB APPLICATION                            │
│                                                                    │
│  USER                                                              │
│   │                                                                │
│   │  uses                                                          │
│   ▼                                                                │
│  FRONTEND (Browser)                                                │
│  HTML + CSS + JavaScript/React                                     │
│   │                                                                │
│   │  HTTP requests (REST API)                                      │
│   ▼                                                                │
│  BACKEND (Server)                                                  │
│  Node.js + Express.js                                              │
│   │                                                                │
│   │  queries                                                       │
│   ▼                                                                │
│  DATABASE                                                          │
│  PostgreSQL / MongoDB                                              │
│                                                                    │
│  (All of this runs on cloud servers — AWS, Render, Railway, etc.) │
└──────────────────────────────────────────────────────────────────┘
```

---

## 10 — The Web Development Ecosystem

Before setting up tools, you should understand the landscape of terms you'll encounter:

| Term | What It Means |
|---|---|
| **IDE** | Integrated Development Environment — where you write code (VS Code) |
| **Terminal / CLI** | Text interface to run commands on your computer |
| **Git** | Version control — tracks every change you make to code |
| **GitHub** | Website that hosts Git repositories online |
| **npm** | Node Package Manager — installs JavaScript libraries |
| **API** | Application Programming Interface — a way for programs to talk to each other |
| **JSON** | JavaScript Object Notation — the data format most APIs use |
| **HTTP** | HyperText Transfer Protocol — how browsers and servers communicate |
| **localhost** | Your own computer acting as a server during development |
| **Port** | A number that identifies a specific service (e.g. port 3000 for your Node app) |
| **Environment Variable** | A setting stored outside your code (like passwords, API keys) |
| **Dependency** | A library your project relies on (installed via npm) |
| **Framework** | A structured toolkit for building apps (Express, React) |
| **Library** | A collection of utilities you can use in your code (lodash, axios) |
| **Linter** | A tool that checks your code for errors and style issues (ESLint) |
| **Formatter** | A tool that auto-formats your code (Prettier) |

---

## 11 — Setting Up Your Development Environment

You need three things installed before you can start coding:

---

### 📦 Step 1 — Install Node.js

Node.js lets you run JavaScript outside the browser. It also comes with **npm** (Node Package Manager).

**Download:**
👉 [nodejs.org](https://nodejs.org) — download the **LTS** (Long Term Support) version

**Install:**
- Windows: run the `.msi` installer, click through the prompts
- macOS: run the `.pkg` installer, or use `brew install node`
- Linux (Ubuntu/Debian):
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Verify installation:**
```bash
node --version   # should show something like v20.11.0
npm --version    # should show something like 10.2.4
```

> ✅ If you see version numbers, Node.js is installed correctly.

---

### 🐙 Step 2 — Install Git

Git tracks every change you make to your code. It's how developers collaborate and how you submit your work.

**Download:**
👉 [git-scm.com](https://git-scm.com/downloads)

**Install:**
- Windows: run the installer, keep all defaults
- macOS: run `git --version` in Terminal — it will prompt you to install if missing
- Linux: `sudo apt-get install git`

**Verify:**
```bash
git --version   # should show something like git version 2.43.0
```

**First-time setup** (do this once after installing):
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

### 💻 Step 3 — Install Visual Studio Code

VS Code is the most popular code editor in the world for web and backend development.

**Download:**
👉 [code.visualstudio.com](https://code.visualstudio.com)

**Install:**
- Windows / macOS: run the installer
- Linux (Ubuntu): `sudo snap install --classic code`

**Open a project folder:**
```bash
# From terminal
code .          # opens VS Code in the current folder
code myproject  # opens VS Code in a specific folder
```

Or: **File → Open Folder** from inside VS Code.

---

### ✅ Verify Everything Works Together

```bash
node --version     # v20.x.x
npm --version      # 10.x.x
git --version      # 2.x.x
code --version     # 1.x.x
```

---

## 12 — Visual Studio Code — Your Main Tool

VS Code is not just a text editor — it's a full development environment. Here's how to use it effectively.

---

### The VS Code Interface

```
┌─────────────────────────────────────────────────────┐
│  Activity Bar  │  Explorer / Sidebar                 │
│  (left icons)  │                                     │
│                │  EDITOR (center — where you code)   │
│  📁 Explorer   │                                     │
│  🔍 Search     │                                     │
│  🔀 Git        ├─────────────────────────────────────┤
│  🐛 Debug      │  TERMINAL (bottom)                  │
│  🧩 Extensions │  Run commands here                  │
└─────────────────────────────────────────────────────┘
```

---

### Essential Keyboard Shortcuts

| Action | Windows/Linux | macOS |
|---|---|---|
| Open terminal | `` Ctrl + ` `` | `` Cmd + ` `` |
| Open command palette | `Ctrl + Shift + P` | `Cmd + Shift + P` |
| Quick file open | `Ctrl + P` | `Cmd + P` |
| Save file | `Ctrl + S` | `Cmd + S` |
| Format document | `Shift + Alt + F` | `Shift + Option + F` |
| Comment/uncomment | `Ctrl + /` | `Cmd + /` |
| Duplicate line | `Shift + Alt + ↓` | `Shift + Option + ↓` |
| Move line up/down | `Alt + ↑/↓` | `Option + ↑/↓` |
| Multi-cursor | `Alt + Click` | `Option + Click` |
| Find in file | `Ctrl + F` | `Cmd + F` |
| Find in all files | `Ctrl + Shift + F` | `Cmd + Shift + F` |
| Go to definition | `F12` | `F12` |
| Rename symbol | `F2` | `F2` |
| Split editor | `Ctrl + \` | `Cmd + \` |
| Zen mode (distraction-free) | `Ctrl + K Z` | `Cmd + K Z` |

---

### The Integrated Terminal

VS Code has a built-in terminal — use it instead of switching windows:

- Open: `` Ctrl + ` `` (backtick)
- New terminal: `Ctrl + Shift + ` `` 
- Split terminal: click the split icon in the terminal panel
- Switch between terminals: dropdown in the terminal panel

```bash
# Everything you need to run from the terminal:
node app.js          # run a Node.js file
npm init -y          # create package.json
npm install express  # install a package
git status           # check git status
git add .            # stage all changes
git commit -m "msg"  # commit changes
```

---

### Useful VS Code Settings

Open settings: `Ctrl + ,` (comma) or `Cmd + ,`

Recommended settings to add (`settings.json`):

```json
{
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.wordWrap": "on",
  "editor.minimap.enabled": false,
  "terminal.integrated.fontSize": 13,
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": true
}
```

To edit settings.json directly:
1. Open Command Palette (`Ctrl + Shift + P`)
2. Type: `Open User Settings JSON`
3. Paste the settings above

---

## 13 — Essential VS Code Extensions

Extensions are plugins that add features to VS Code. Here's every extension you need for this course — and how to install each one.

---

### How to Install Extensions

**Method 1 — From VS Code UI:**
1. Click the Extensions icon in the Activity Bar (left sidebar) — or press `Ctrl + Shift + X`
2. Type the extension name in the search box
3. Click **Install**

**Method 2 — From terminal:**
```bash
code --install-extension <extension-id>
```

**Method 3 — From VS Code Marketplace:**
👉 [marketplace.visualstudio.com](https://marketplace.visualstudio.com/vscode)
Search → Click **Install** → it opens in VS Code automatically

---

### 🔵 Must-Have — Every Developer Needs These

---

**Prettier — Code Formatter**
- Extension ID: `esbenp.prettier-vscode`
- What it does: Auto-formats your code every time you save. Consistent indentation, quotes, spacing — no more arguing about style.
- Install: `code --install-extension esbenp.prettier-vscode`

```bash
# After installing, add to settings.json:
"editor.formatOnSave": true,
"editor.defaultFormatter": "esbenp.prettier-vscode"
```

---

**ESLint**
- Extension ID: `dbaeumer.vscode-eslint`
- What it does: Highlights errors and bad practices in your JavaScript as you type — before you even run the code. Like spell-check but for code logic.
- Install: `code --install-extension dbaeumer.vscode-eslint`

> 💡 ESLint needs a config file in your project. For a basic setup: `npm init @eslint/config` in your terminal.

---

**GitLens — Git Supercharged**
- Extension ID: `eamodio.gitlens`
- What it does: Shows who wrote each line of code, when, and in which commit. Inline blame, history, and diff views directly in the editor.
- Install: `code --install-extension eamodio.gitlens`

---

**GitHub Copilot** *(free for students)*
- Extension ID: `GitHub.copilot`
- What it does: AI that suggests code completions as you type. Like autocomplete but it predicts entire functions.
- Install: `code --install-extension GitHub.copilot`
- Get free access: [education.github.com/pack](https://education.github.com/pack) — free with a student email

---

**Path Intellisense**
- Extension ID: `christian-kohler.path-intellisense`
- What it does: Autocompletes file paths when you type `require('./...')` or `import`. No more typos in file names.
- Install: `code --install-extension christian-kohler.path-intellisense`

---

**Auto Rename Tag**
- Extension ID: `formulahendry.auto-rename-tag`
- What it does: When you rename an HTML opening tag, the closing tag renames automatically.
- Install: `code --install-extension formulahendry.auto-rename-tag`

---

**Bracket Pair Colorizer** *(now built into VS Code)*
- VS Code 1.67+ has this built in. Enable in settings:
```json
"editor.bracketPairColorization.enabled": true,
"editor.guides.bracketPairs": true
```
Colors matching `{}`, `[]`, `()` pairs so you never lose track of nesting.

---

### 🟢 JavaScript & Node.js Specific

---

**JavaScript (ES6) Code Snippets**
- Extension ID: `xabikos.JavaScriptSnippets`
- What it does: Shorthand snippets for common JavaScript patterns. Type `clg` + Tab → `console.log()`. Type `imp` → `import ... from ...`.
- Install: `code --install-extension xabikos.JavaScriptSnippets`

Common snippets:

| Shortcut | Expands to |
|---|---|
| `clg` | `console.log()` |
| `imp` | `import x from 'x'` |
| `req` | `const x = require('x')` |
| `fn` | `function name() {}` |
| `afn` | `async function name() {}` |
| `nfn` | `const name = () => {}` |
| `dob` | `const { } = object` |

---

**Node.js Extension Pack**
- Extension ID: `waderyan.nodejs-extension-pack`
- What it does: Bundles several Node.js helpful extensions in one install — npm intellisense, module lens, and more.
- Install: `code --install-extension waderyan.nodejs-extension-pack`

---

**npm Intellisense**
- Extension ID: `christian-kohler.npm-intellisense`
- What it does: Autocompletes npm package names in `require()` and `import` statements.
- Install: `code --install-extension christian-kohler.npm-intellisense`

---

**REST Client**
- Extension ID: `humao.rest-client`
- What it does: Send HTTP requests directly from VS Code. Create a `.http` file, write your request, click "Send Request". No need to open Postman.
- Install: `code --install-extension humao.rest-client`

```http
### Get all users
GET http://localhost:3000/api/users

### Create a user
POST http://localhost:3000/api/users
Content-Type: application/json

{
  "name": "Marta",
  "email": "marta@example.com"
}
```

---

**Thunder Client** *(alternative to Postman, inside VS Code)*
- Extension ID: `rangav.vscode-thunder-client`
- What it does: Full API testing client built into VS Code. GUI-based, like a lightweight Postman.
- Install: `code --install-extension rangav.vscode-thunder-client`

---

### 🟡 Productivity & Readability

---

**Better Comments**
- Extension ID: `aaron-bond.better-comments`
- What it does: Color-codes your comments by type — `!` for errors, `?` for questions, `TODO` for tasks, `*` for highlights.
- Install: `code --install-extension aaron-bond.better-comments`

```js
// ! This will throw an error
// ? Is this the right approach?
// TODO: refactor this function
// * This is an important note
// normal comment
```

---

**Todo Tree**
- Extension ID: `Gruntfucius.todo-tree`
- What it does: Scans all files for `TODO`, `FIXME`, `HACK` comments and shows them in a tree view. Never forget a task again.
- Install: `code --install-extension Gruntfucius.todo-tree`

---

**Error Lens**
- Extension ID: `usernamehw.errorlens`
- What it does: Shows error and warning messages inline next to the problematic code — no need to hover. Errors are impossible to miss.
- Install: `code --install-extension usernamehw.errorlens`

---

**Indent Rainbow**
- Extension ID: `oderwat.indent-rainbow`
- What it does: Colors each level of indentation a different color. Makes deeply nested code much easier to read.
- Install: `code --install-extension oderwat.indent-rainbow`

---

**Code Spell Checker**
- Extension ID: `streetsidesoftware.code-spell-checker`
- What it does: Checks spelling in comments, strings, and variable names. Catches `recieve` instead of `receive`.
- Install: `code --install-extension streetsidesoftware.code-spell-checker`

---

### 🔴 Database & Environment

---

**DotENV**
- Extension ID: `mikestead.dotenv`
- What it does: Syntax highlighting for `.env` files. Makes it easy to read environment variables with proper coloring.
- Install: `code --install-extension mikestead.dotenv`

---

**MongoDB for VS Code**
- Extension ID: `mongodb.mongodb-vscode`
- What it does: Connect to MongoDB databases directly inside VS Code. Browse collections, run queries, view documents.
- Install: `code --install-extension mongodb.mongodb-vscode`

---

**SQLTools**
- Extension ID: `mtxr.sqltools`
- What it does: SQL client inside VS Code. Connect to PostgreSQL, MySQL, SQLite — run queries and view tables.
- Install: `code --install-extension mtxr.sqltools`

---

### 🎨 Themes (Make Your Editor Look Great)

A good theme reduces eye strain during long coding sessions. Try these:

| Theme | Extension ID | Style |
|---|---|---|
| **One Dark Pro** | `zhuangtongfa.material-theme` | Dark, popular |
| **Dracula Official** | `dracula-theme.theme-dracula` | Dark purple tones |
| **GitHub Theme** | `GitHub.github-vscode-theme` | Light and dark GitHub-style |
| **Tokyo Night** | `enkia.tokyo-night` | Beautiful dark blue |
| **Catppuccin** | `Catppuccin.catppuccin-vsc` | Pastel, easy on the eyes |
| **Nord** | `arcticicestudio.nord-visual-studio-code` | Cool blue-gray |

**Install a theme:**
1. `Ctrl + Shift + X` → search theme name → Install
2. `Ctrl + K Ctrl + T` → select your theme from the list

---

### 🔡 Fonts (Highly Recommended)

Fonts with **ligatures** merge symbols like `=>`, `===`, `!==` into single characters — much easier to read:

| Font | Where to Get |
|---|---|
| **Fira Code** | [github.com/tonsky/FiraCode](https://github.com/tonsky/FiraCode) |
| **JetBrains Mono** | [jetbrains.com/lp/mono](https://www.jetbrains.com/lp/mono/) |
| **Cascadia Code** | [github.com/microsoft/cascadia-code](https://github.com/microsoft/cascadia-code) |

**Enable ligatures in VS Code settings:**
```json
"editor.fontFamily": "Fira Code",
"editor.fontLigatures": true,
"editor.fontSize": 14
```

---

### Install Everything at Once

Copy and run this in your terminal to install all essential extensions:

```bash
code --install-extension esbenp.prettier-vscode
code --install-extension dbaeumer.vscode-eslint
code --install-extension eamodio.gitlens
code --install-extension christian-kohler.path-intellisense
code --install-extension formulahendry.auto-rename-tag
code --install-extension xabikos.JavaScriptSnippets
code --install-extension christian-kohler.npm-intellisense
code --install-extension humao.rest-client
code --install-extension rangav.vscode-thunder-client
code --install-extension aaron-bond.better-comments
code --install-extension usernamehw.errorlens
code --install-extension oderwat.indent-rainbow
code --install-extension mikestead.dotenv
code --install-extension zhuangtongfa.material-theme
```

---

## 14 — The Programming Mindset

Good programmers think differently from most people. These mental habits matter more than memorizing syntax.

---

### 🧩 Break Big Problems into Small Pieces

Large problems are overwhelming. Every large problem is just a collection of small problems.

When asked to "build a login system", a programmer breaks it down:
1. Create a form (HTML)
2. Validate the input (JS)
3. Send it to the server (HTTP POST)
4. Check against the database (SQL query)
5. Generate a token (crypto)
6. Send the token back (HTTP response)
7. Store it in the browser (localStorage / cookie)

Each step is manageable. The whole list isn't scary — it's just a checklist.

---

### 🔢 Think Logically, Step by Step

Every instruction has to make sense, one step at a time. Computers don't assume context — you have to be explicit.

---

### 😌 Stay Comfortable with Ambiguity

Real projects change. Requirements are often unclear at the start. Good programmers adapt instead of getting stuck waiting for perfect specifications.

---

### 💪 Persistence — Failure Is the Process

You will get errors. Lots of them. Every developer does. The difference between beginners and experienced developers isn't that experienced developers get fewer errors — it's that they've learned to debug them faster.

> 🧠 When your code breaks, that's not failure. That's the process. Every error message is a clue.

---

### 📚 Continuous Learning

Technology changes quickly. No one knows everything. But the fundamentals — variables, logic, functions, data structures — these last forever across every language and every framework.

---

### Why Programming is Valuable

Software is reshaping every industry:
- **Healthcare** — patient management, diagnostics, telemedicine
- **Finance** — banking apps, payment processing, fraud detection
- **Education** — e-learning, student management systems
- **Transportation** — ride-sharing, navigation, logistics
- **Entertainment** — streaming, gaming, social media

Learning programming gives you:
- Creative control over what you build
- The ability to automate work others do manually
- The chance to build tools that millions of people rely on
- Career flexibility across every industry

---

## 15 — Quick Exercises

Before moving to JavaScript, do these:

1. **Notice** one app you use daily. Write down the problem it solves.

2. **Break it down** — list 5 things that must happen behind the scenes when you log in to that app. Which are frontend? Which are backend?

3. **Install** Node.js, Git, and VS Code. Run `node --version`, `git --version`, `code --version` to verify.

4. **Install** at least 5 extensions from Section 13. Open VS Code, explore the interface.

5. **Create** your first project folder:
```bash
mkdir my-first-project
cd my-first-project
code .
```
Create a file called `hello.js` and add:
```js
console.log("Hello, World! I am a backend developer in training.");
```
Run it:
```bash
node hello.js
```

6. **Reflect** — in one sentence each, define: frontend, backend, fullstack. Don't look back. Use your own words.

---

> 🎯 **You've completed Module 00!**
>
> Move to **[Module 01 — JavaScript Fundamentals →](../01-JavaScript-Fundamentals/README.md)**
>
> You now have the mental model and the tools. Time to write real code. 🚀
