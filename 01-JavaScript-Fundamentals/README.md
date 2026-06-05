# 🟨 Module 01 — JavaScript Fundamentals

> 👋 **Welcome, future developer!**
> JavaScript is the language of the web — and soon, the language of your backend too.
> This module is your complete foundation. Read every section, try every example, and never skip an exercise.

---

## 📖 Table of Contents

1. [What is JavaScript?](#1--what-is-javascript)
2. [How to Use JavaScript in HTML](#2--how-to-use-javascript-in-html)
3. [Variables & Data Types](#3--variables--data-types)
4. [Operators](#4--operators)
5. [Control Flow](#5--control-flow)
6. [Functions](#6--functions)
7. [Arrays & Objects](#7--arrays--objects)
8. [ES6+ Features](#8--es6-features)
9. [Asynchronous JavaScript](#9--asynchronous-javascript)
10. [Promises](#10--promises)
11. [Async / Await](#11--async--await)
12. [Learning Resources](#12--learning-resources)
13. [Quick Knowledge Check](#13--quick-knowledge-check)

---

## 1 — What is JavaScript?

JavaScript is a **dynamic programming language** used for web development, web applications, game development, and a whole lot more. It lets you implement features on web pages that cannot be done with HTML and CSS alone.

Any time you see a dropdown menu open on click, content appear without a page refresh, or colors change dynamically — that's JavaScript doing its job.

---

### 🏠 The House Analogy

Think of building a house:
- **HTML** is the structure — walls, floors, rooms
- **CSS** is the interior design — colors, fonts, layout
- **JavaScript** is the electricity ⚡ — it makes everything *work*

HTML and CSS are called **markup languages** rather than programming languages. At their core, they define structure and style but have very little dynamism.

JavaScript, on the other hand:
- Supports **Math calculations**
- Dynamically **adds or removes HTML** from the page
- **Creates and changes styles** on the fly
- **Fetches content** from other servers
- Reacts to **user interaction** in real time
- Powers **entire backend servers** with Node.js

---

### 💼 What Can JavaScript Do?

| Capability | Real Example |
|---|---|
| 🖊️ Give HTML designers a programming tool | Add interactivity without knowing a backend language |
| 📝 Write dynamic text into HTML | `document.write("<h1>" + name + "</h1>")` |
| 🎯 React to events | Run code when a button is clicked or a page loads |
| 📖 Read & write HTML elements | Change a heading's text after login |
| ✅ Validate data | Check form input before sending to a server |
| 🌐 Detect the visitor's browser | Load a browser-specific page |
| 🍪 Create and read cookies | Store user preferences locally |
| 🖥️ Run on the server | Handle HTTP requests with Node.js |

> 💡 **Fun Fact:** JavaScript was created in just **10 days** in 1995 by Brendan Eich at Netscape. Today it's the #1 most used programming language in the world for 11+ years running.

---

## 2 — How to Use JavaScript in HTML

Just like CSS, JavaScript can be added to HTML in three ways:

---

### 🎵 Method 1 — Inline JavaScript

The JS lives directly inside an HTML tag attribute:

```html
<button onclick="alert('You just clicked a button')">Click me!</button>
```

The value of `onclick` can be any valid JavaScript — math, DOM changes, function calls. Quick for tiny things, but messy at scale. **Avoid for real projects.**

---

### 🔊 Method 2 — Internal JavaScript (using `<script>`)

```html
<script>
  function greet() {
    alert("I am inside a script tag");
  }
</script>
```

JavaScript lives inside the HTML file in its own `<script>` block. Better for small projects, but still mixes HTML and JS.

---

### 🎙️ Method 3 — External JavaScript (separate `.js` file)

```html
<!-- index.html -->
<script src="./script.js"></script>
```

```js
// script.js
alert("I am inside an external file");
```

The `src` attribute tells the browser to also load `script.js`. Notice the `.js` extension — that's the JavaScript file extension, just like `.html` for HTML.

`script.js` can be in the same folder as `index.html`, or loaded from another website using a full URL (`https://.../script.js`).

> ✅ **Best Practice:** Always use **external JavaScript** for real projects. Keeps your HTML clean, your JS reusable, and your codebase organized.

---

## 3 — Variables & Data Types

### 🧠 What is a Variable?

> **Analogy #1 — The Name:**
> When a child is born, they're given a name. Throughout their life, calling that name refers to that specific person. Variables work the same way — a name that refers to a value.

> **Analogy #2 — Math:**
> When we say `x = 1`, it means "anywhere you see x, replace it with 1". `x` is the variable, `1` is the value. `x` *points to* `1`.

> **Analogy #3 — A Container:**
> A variable is a labeled box 📦. The **name** is the label, the **value** is what's inside, the **type** is what kind of thing it holds.

```js
/* x is 1
 * Anywhere x appears after this line,
 * the compiler replaces x with 1.
 */
let x = 1;
let y = 1; // y also holds 1 — but it's a DIFFERENT 1 from x's

console.log(x); // 1
console.log(typeof x); // "number"
```

Variables exist to hold values and let us refer to them whenever needed. Anywhere a variable is mentioned, the value of that variable is what's used in the computation.

---

### Declaring, Assigning & Initializing

```js
let score;       // Declaration  — creates the container (empty)
score = 10;      // Assignment   — puts a value in
let age = 20;    // Initialization — declaration + assignment at once
```

- **Declaring** = buying a box 📦
- **Assigning** = putting something in the box
- **Initializing** = buying a box that already has something in it

---

### The Three Keywords: `let`, `const`, `var`

| Keyword | Reassignable? | Scope | Use When |
|---|---|---|---|
| `let` | ✅ Yes | Block `{}` | Value will change |
| `const` | ❌ No | Block `{}` | Value never changes |
| `var` | ✅ Yes | Function | Legacy code only — avoid! |

```js
let username = "Alex";   // can be reassigned
const PI = 3.14159;      // can NEVER be changed
username = "Jordan";     // ✅ fine
PI = 3;                  // ❌ TypeError: Assignment to constant variable
```

> 💡 **Rule of Thumb:** Start with `const`. Switch to `let` only when you know the value needs to change. Never use `var` in modern code.

---

### How to Call a Variable

To use a variable, just mention its name. During execution it's replaced with its value:

```js
let score = 1;
console.log(score + 1); // 2 → score replaced by 1, so: 1 + 1 = 2
```

---

### How to Name Variables

**Rules (required):**
- Cannot start with a number: ❌ `1score` → ✅ `score1`
- No spaces: ❌ `my score` → ✅ `myScore`
- Cannot use reserved words like `let`, `if`, `return`, `catch`, `class`
- Case-sensitive: `score` and `Score` are **different** variables
- Can start with letter, `_`, or `$` — but always start with a letter by convention

**Reserved Words** (you don't need to memorize — just know they exist):
> `arguments` `await` `break` `case` `catch` `class` `const` `continue` `debugger` `default` `delete` `do` `else` `enum` `eval` `export` `extends` `false` `finally` `for` `function` `if` `implements` `import` `in` `Infinity` `instanceof` `interface` `let` `NaN` `new` `null` `package` `private` `protected` `public` `return` `static` `super` `switch` `this` `throw` `true` `try` `typeof` `undefined` `var` `void` `while` `with` `yield`

> 📝 You don't need to memorize these. If you try to use one, you'll get an error — and you'll learn them naturally through experience.

**Naming Conventions (best practices):**

```js
let firstName = "Ada";          // camelCase — for regular variables
const MAX_RETRIES = 3;          // UPPER_SNAKE_CASE — for constants
const PROGRAM_NAME = "Planner"; // UPPER_SNAKE_CASE — multi-word constants
let _privateValue = 42;         // underscore prefix — signals private use
let isLoggedIn = true;          // is/has prefix — for Booleans
let publishedDate = "Aug 2023"; // descriptive, self-explanatory
```

> 🧠 **Tip:** A variable name should tell a story. `x` tells you nothing. `userAge` tells you exactly what it is and why it exists.

---

### Data Types

The **type** of a variable determines what operations you can perform on it. You can add two numbers, but you can't pour water that is stored in a candy box — the type of thing inside determines what you can do with it.

```
┌──────────────────────────────────────────────┐
│             JAVASCRIPT TYPES                  │
├──────────────────────┬───────────────────────┤
│   PRIMITIVE TYPES    │    REFERENCE TYPES    │
│  (stored by VALUE)   │  (stored by REFERENCE)│
├──────────────────────┼───────────────────────┤
│  Number              │  Object               │
│  String              │  Array                │
│  Boolean             │  Function             │
│  Undefined           │                       │
│  Null                │                       │
│  BigInt              │                       │
│  Symbol              │                       │
└──────────────────────┴───────────────────────┘
```

We will focus on the most commonly used types. BigInt and Symbol are advanced and rarely needed as a beginner.

---

### 🔢 Number

In JavaScript, **all numbers** — whole or decimal, positive or negative — are of type `number`. There is no separate `int` or `float`.

```js
let score1 = 2;
let score2 = 5;
let averageScore = (score1 + score2) / 2;

console.log(averageScore);        // 3.5
console.log(typeof score1);       // "number"
```

> ✏️ **Exercise:** Copy this code into your editor and run it. Play with the values. Use `typeof` to inspect variables.

**Special number values to know:**

```js
let result = 12 / 0;
console.log(result);                    // Infinity

console.log(Number.NEGATIVE_INFINITY);  // -Infinity

const invalid = "Ella" / 2;            // dividing string by number
console.log(invalid);                   // NaN — Not a Number
```

> ⚠️ **NaN** means you tried a math operation on something that isn't a number. You won't hit `Infinity` often as a beginner — but `NaN` appears frequently. When you see it, trace back to what value was involved.

---

### 🔤 String

A string is a **collection of characters** enclosed in quotes — single `'`, double `"`, or backtick `` ` ``.

```js
let author = "Sleekcodes";
let publishedDate = "14 August 2023";

console.log("Written by: " + author);          // Written by: Sleekcodes
console.log("Published on: " + publishedDate); // Published on: 14 August 2023
```

The `+` between strings **concatenates** them — joins them together into one string.

During execution, `author` gets replaced with `"Sleekcodes"` and `publishedDate` gets replaced with `"14 August 2023"` wherever they appear.

Strings convey data in text format. A string with zero characters is an **empty string**: `""`.

**Template Literals** (modern, preferred):

```js
let city = "Addis Ababa";
let message = `Welcome to ${city}!`; // backtick + ${}
console.log(message); // Welcome to Addis Ababa!
```

> ✅ Use template literals over `+` concatenation. They're cleaner, support multi-line strings, and allow any expression inside `${}`.

---

### ✅ Boolean

Booleans represent only **two states**: `true` or `false`. On/Off. Yes/No.

```js
let isQualified = true;

if (isQualified) {
  console.log("Tola is qualified"); // runs because isQualified is true
}
```

> ✏️ **Exercise:** Change `isQualified` to `false` and observe what happens.

> 💡 Booleans power every decision in your code. Every `if`, every condition, every comparison — all of it resolves to `true` or `false`.

---

### ❓ Undefined

`undefined` is both a **value and a type**. It means a variable was declared but never assigned a value. JavaScript assigns it automatically.

```js
let age; // declared, no value
console.log(age); // undefined
```

> ✏️ **Exercise:** Use `typeof age` and see what it returns. Then assign `undefined` explicitly and check again.

---

### 🚫 Null

`null` is a value **you assign intentionally** to say "this is empty right now."

```js
let age = null;
console.log(age); // null
```

| | `undefined` | `null` |
|---|---|---|
| Who sets it? | JavaScript automatically | You, the developer |
| Meaning | "I forgot to give this a value" | "I intentionally set this empty" |
| Rule | Don't assign it yourself | Use when absence is intentional |

> 🧠 As a rule: never manually assign `undefined`. Use `null` when you want to say "empty on purpose." Let JavaScript auto-assign `undefined` when needed.

---

### 📦 Primitive vs Reference Types

![Primitive vs Reference in memory](.images/image.png)

*(Part A is your code. Part B is what happens in memory.)*

For **primitive types**, the value is stored directly in the variable. Simple and straightforward.

For **reference types**, something different happens. Instead of the value being directly assigned to the variable, a **reference** (like a GPS coordinate or memory address) is generated for the value — and that reference is what gets assigned to the variable.

```
Primitive:  variable ──────────────→ value
Reference:  variable → reference address → actual value
```

This is called **passing by reference** — and it's one of the most important concepts in JavaScript.

Consider this carefully:

```js
let studentInfo = {
  name: "John Doe",
  age: 205
};

let staffInfo = studentInfo; // staffInfo stores the SAME reference

staffInfo.name = "Lorry Sante"; // changes the actual object

console.log(studentInfo.name); // "Lorry Sante" — it changed too!
```

> 💡 **Try this:** Log both `studentInfo.name` and `staffInfo.name` before and after the change to see the effect.

![Both variables pointing to same object](.images/image-1.png)

Both `studentInfo` and `staffInfo` point to the **exact same object** in memory. Changing the name through `staffInfo` affects `studentInfo` too — because they're both GPS coordinates pointing to the same location.

![Memory reference diagram](.images/image-2.png)

If the reference for `studentInfo` is `000xx2`, then:
- `staffInfo = studentInfo` → `staffInfo` now also holds `000xx2`
- `staffInfo.name` → navigates to `000xx2` and reads `.name`
- `studentInfo.name` → also navigates to `000xx2` and reads `.name`

They are like **different roads to the same destination**.

> 🔁 If this is your first time seeing this concept, re-read this section before continuing. Understanding pass-by-reference is one of the most important concepts in your entire JavaScript career.

---

### 🗂️ Object

An object stores **related data as key-value pairs** — perfect for representing real-world things.

```js
let studentInfo = {
  name: "John Doe",
  age: 205
};
```

To access a value, use **dot notation**: `objectName.key`

```js
console.log(studentInfo.name); // "John Doe"
console.log(studentInfo.age);  // 205
```

Objects can **nest** other objects:

```js
let studentInfo = {
  name: "John Doe",
  age: 205,
  beneficiary: {
    name: "Tira Doe",
    age: 200,
    relationship: "Wife"
  }
};

// Access nested values with chained dot notation
console.log(studentInfo.beneficiary.name); // "Tira Doe"
```

> 💡 There's no limit to nesting objects — but keep it reasonable. Deep nesting gets hard to read and maintain.

Objects can hold arrays and functions too. That's what makes them so powerful for modeling real-world data.

---

### 📋 Array

An array stores a **list of values**, accessed by position (**index**), starting at `0`.

```js
let scores = [1, 3, 5, 6, 9, 12];
```

![Array index diagram](.images/image-3.png)

```
Index:   0    1    2    3    4    5
Value:  [1,   3,   5,   6,   9,  12]
```

To access a value: `arrayName[index]`

```js
console.log(scores[0]); // 1 — first item
console.log(scores[3]); // 6 — fourth item
```

To access value `80` at position 3: `scores[3]`

Arrays can technically hold mixed types, but **don't do it** — keep all values the same type:

```js
// ❌ Bad practice
let mixed = [1, "hello", true, { name: "John" }];

// ✅ Good practice
let prices = [10.99, 24.50, 5.00, 99.99];
```

> ⚠️ Arrays are **zero-indexed**. The first item is always at index `0`, not `1`. This catches almost every beginner at least once!

---

### ⚙️ Function (Introduction)

A function is a **reusable block of code** that performs a specific task. Write it once, call it anytime.

**Without a function (repetitive 😓):**

```js
let num1 = 2;
let num2 = 3;
let result = num1 + num2;
console.log(result); // 5

let num3 = 3;
let num4 = 8;
let result2 = num3 + num4;
console.log(result2); // 11
```

**With a function (clean ✅):**

```js
// function declaration
function addNumbers(num1, num2) {
  return num1 + num2;
}

console.log(addNumbers(2, 3)); // 5
console.log(addNumbers(3, 8)); // 11
```

You'll agree that scenario 2 contains less code, looks neater, and feels more natural.

Functions allow you to write helpers that you can call to get a specific job done anytime you want. You tell it how to do the job once, and it delivers every time you call it.

> Functions are covered in full depth in [Section 6 — Functions](#6--functions).

---

### Summary — Variables & Types

- Variables are **pointers** to values. When you mention a variable in code, the name is replaced with the value it holds at execution time. It's like calling someone's name — the name doesn't respond, the person (value) behind the name is what you get.
- Use `const` by default, `let` when the value needs to change, never `var`
- **Primitive types** store values directly. **Reference types** store memory addresses that point to the actual value.
- Don't assign `undefined` yourself — use `null` for intentional emptiness
- Variable names should be descriptive, camelCase, and tell a story

```js
let a = 2;
let b = 3;
console.log(a + b); // 5
```

---
