# 🟨 Module 01 — JavaScript Fundamentals

> 👋 Hey there! Welcome to your first real step into backend development.
> Before we touch servers, APIs, or databases — we need to speak the language. That language is **JavaScript**.
> Don't rush. Read carefully, try every example, and have fun with it!

---

## 📖 Table of Contents

- [What is JavaScript?](#what-is-javascript)
- [How JavaScript Works in HTML](#how-javascript-works-in-html)
- [Variables](#variables)
- [Data Types](#data-types)
- [Operators](#operators)
- [Control Flow](#control-flow)
- [Quick Knowledge Check](#quick-knowledge-check)

---

## What is JavaScript?

Think of building a house 🏠:
- **HTML** is the structure — walls, floors, rooms.
- **CSS** is the interior design — colors, fonts, layout.
- **JavaScript** is the electricity — it makes things *work*. Lights turn on, doors open, the doorbell rings.

JavaScript is a **dynamic programming language** that brings web pages to life. It lets you:

- React to user actions (clicks, typing, scrolling)
- Change what's displayed on the page without reloading
- Do math and logic
- Talk to servers and fetch data
- Build entire backend applications (that's why we're here! 🚀)

### What Can JavaScript Actually Do?

| Capability | Example |
|---|---|
| 🖊️ Write dynamic content | Add text to a page based on user input |
| 🎯 React to events | Run code when a button is clicked |
| 📖 Read & modify HTML | Change a heading's text dynamically |
| ✅ Validate data | Check a form before it's submitted to a server |
| 🍪 Store information | Save user preferences with cookies |
| 🌐 Communicate with servers | Fetch data from an API |

> 💡 **Fun Fact:** JavaScript was created in just 10 days in 1995 by Brendan Eich. Today it's the most used programming language in the world!

---

## How JavaScript Works in HTML

There are three ways to add JavaScript to a webpage. Think of them like three ways to add music to a room:

### 1. 🎵 Inline — Speaker right in your ear

```html
<button onclick="alert('You just clicked a button')">Click me!</button>
```

The JS is written directly inside the HTML tag. Quick and easy, but messy for large code.

---

### 2. 🔊 Internal — Speaker in the same room (using `<script>` tag)

```html
<script>
  function greet() {
    alert("I am inside a script tag");
  }
</script>
```

JavaScript lives inside the HTML file but in its own `<script>` block. Better for small projects.

---

### 3. 🎙️ External — Speaker in another room (separate `.js` file)

```html
<!-- index.html -->
<script src="./script.js"></script>
```

```js
// script.js
alert("I am inside an external file");
```

The JavaScript lives in its own file. This is the **professional standard** — keeps your code organized and reusable.

> ✅ **Best Practice:** Always use external JavaScript for real projects. It keeps your HTML clean and your JS reusable.

---

## Variables

### 🧠 What is a Variable?

Imagine you're baking a cake and you write a recipe. Instead of writing "250 grams of flour" everywhere, you label a bag **"flour"**. Now whenever you say "flour", everyone knows exactly what you mean.

That's a variable — **a named container that holds a value**.

```js
let flour = 250; // "flour" now holds the value 250
```

Anywhere you use `flour` in your code, JavaScript replaces it with `250`.

---

### Declaring, Assigning & Initializing

```js
let score;          // Declaration — creates the container (empty for now)
score = 10;         // Assignment — puts a value in the container
let age = 20;       // Initialization — declaration + assignment at once
```

Think of it like this:
- **Declaring** = buying a box 📦
- **Assigning** = putting something in the box
- **Initializing** = buying a box that already has something in it

---

### The Three Keywords: `let`, `const`, `var`

| Keyword | Can be changed? | Scope | When to use |
|---|---|---|---|
| `let` | ✅ Yes | Block | Most variables |
| `const` | ❌ No | Block | Values that never change |
| `var` | ✅ Yes | Function | Older code — avoid in modern JS |

```js
let username = "Alex";       // Can be reassigned later
const PI = 3.14159;          // Can NEVER be changed
username = "Jordan";         // ✅ Works fine
PI = 3;                      // ❌ Error! You can't reassign a const
```

> 💡 **Rule of Thumb:** Use `const` by default. Switch to `let` only when you know the value will change. Avoid `var`.

---

### Calling a Variable

To use a variable, just mention its name:

```js
let score = 10;
console.log(score + 5); // Output: 15
```

During execution, `score` is replaced by `10`, so it becomes `10 + 5 = 15`.

---

### Naming Variables — The Rules & Conventions

**Rules (must follow):**
- Cannot start with a number: ❌ `1score`, ✅ `score1`
- No spaces: ❌ `my score`, ✅ `myScore`
- Cannot use reserved words: ❌ `let`, `if`, `return`
- Case-sensitive: `score` and `Score` are different variables

**Conventions (should follow):**

```js
let firstName = "Ada";            // camelCase for regular variables
const MAX_RETRIES = 3;            // UPPER_SNAKE_CASE for constants
let _privateValue = 42;           // underscore prefix for private vars
let isLoggedIn = true;            // is/has prefix for Booleans
```

> 🧠 **Tip:** Good variable names tell a story. `x` means nothing. `userAge` tells you exactly what it holds.

---

## Data Types

JavaScript values come in different *types*. The type determines what you can do with the value — just like you can drink water but not eat it, and you can eat bread but not pour it.

Data types split into two categories:

```
📦 Primitive Types         📦 Reference Types
─────────────────          ─────────────────
Number                     Object
String                     Array
Boolean                    Function
Undefined
Null
```

---

### 🔢 Number

All numbers in JavaScript — whole or decimal, positive or negative — are of type `number`.

```js
let price = 99.99;
let quantity = 3;
let total = price * quantity;

console.log(total);        // 299.97
console.log(typeof price); // "number"
```

**Special number values to know:**

```js
console.log(10 / 0);             // Infinity
console.log(-10 / 0);            // -Infinity
console.log("hello" / 2);        // NaN (Not a Number)
```

> ⚠️ If you ever see `NaN` in your output, it means you tried to do a math operation on something that isn't a number.

---

### 🔤 String

A string is text — any sequence of characters wrapped in quotes.

```js
let name = "Selam";
let greeting = 'Hello, ' + name + '!';  // String concatenation
console.log(greeting);  // Hello, Selam!
```

**Template Literals** — the modern, cleaner way:

```js
let city = "Addis Ababa";
let message = `Welcome to ${city}!`; // Use backticks + ${}
console.log(message); // Welcome to Addis Ababa!
```

> ✅ Use template literals (backticks) instead of `+` for combining strings. It's cleaner and less error-prone.

---

### ✅ Boolean

Booleans have only two possible values: `true` or `false`. They're the yes/no of programming.

```js
let isLoggedIn = true;
let hasPermission = false;

if (isLoggedIn) {
  console.log("Welcome back!"); // This runs
}
```

> 💡 Booleans power every decision in your code. Every `if` statement, every condition — it all comes down to `true` or `false`.

---

### ❓ Undefined vs Null

These two confuse almost every beginner. Here's the clear distinction:

```js
let age;           // Declared but no value given
console.log(age);  // undefined — JavaScript assigned this automatically

let score = null;  // You explicitly said "this has no value yet"
console.log(score); // null
```

| | `undefined` | `null` |
|---|---|---|
| Set by | JavaScript automatically | You, the developer |
| Means | "I forgot to give this a value" | "I intentionally left this empty" |
| Rule | Don't assign it yourself | Use this when a value is intentionally absent |

---

### 📦 Reference Types

Unlike primitives (which store the actual value), reference types store a **reference** — like a GPS coordinate pointing to the real data.

```
Primitive:   variable → value
Reference:   variable → reference → actual value
```

This matters a lot. Watch what happens:

```js
let studentInfo = { name: "John", age: 20 };
let staffInfo = studentInfo; // Both now point to the SAME object

staffInfo.name = "Lorry";
console.log(studentInfo.name); // "Lorry" — it changed too!
```

Both variables are pointing to the same object in memory. Changing one changes the other. This is called **passing by reference**.

---

### 🗂️ Object

An object stores related data as **key-value pairs** — perfect for representing real-world things.

```js
let student = {
  name: "Marta",
  age: 21,
  isEnrolled: true,
  address: {
    city: "Addis Ababa",
    country: "Ethiopia"
  }
};

console.log(student.name);           // "Marta"
console.log(student.address.city);   // "Addis Ababa"
```

Think of an object like a form — each field (key) has a value.

---

### 📋 Array

An array stores a **list** of values, accessed by their position (index), starting at `0`.

```js
let scores = [95, 80, 72, 88, 91];

console.log(scores[0]); // 95 — first item
console.log(scores[3]); // 88 — fourth item
```

```
Index:   0    1    2    3    4
Value:  [95,  80,  72,  88,  91]
```

> 💡 Arrays are zero-indexed. The first item is always at index `0`, not `1`. This trips up almost every beginner at least once!

---

### ⚙️ Function

A function is a reusable block of code that does a specific job. Instead of writing the same logic over and over, you write it once and call it whenever you need it.

**Without a function (repetitive 😓):**

```js
let result1 = 2 + 3;
let result2 = 7 + 8;
let result3 = 4 + 9;
```

**With a function (clean ✅):**

```js
function add(num1, num2) {
  return num1 + num2;
}

console.log(add(2, 3));  // 5
console.log(add(7, 8));  // 15
console.log(add(4, 9));  // 13
```

**Function anatomy:**

```
function   addNumbers  ( num1,  num2 ) {  return num1 + num2;  }
  ↑             ↑          ↑               ↑
keyword       name      parameters       return value
```

> - **Parameters** are placeholders in the function definition: `num1`, `num2`
> - **Arguments** are the actual values you pass when calling: `add(2, 3)` → `2` and `3` are arguments
> - **`return`** sends a value back to whoever called the function. Without it, the function returns `undefined`.

---

## Operators

### ➕ Arithmetic Operators

| Operator | Name | Example | Result |
|---|---|---|---|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `10 - 4` | `6` |
| `*` | Multiplication | `3 * 7` | `21` |
| `/` | Division | `15 / 3` | `5` |
| `%` | Remainder (Modulo) | `10 % 3` | `1` |
| `**` | Exponent | `2 ** 4` | `16` |

**Operator Precedence** — JavaScript follows math rules: multiplication and division before addition and subtraction.

```js
let result = 10 + 5 * 2;   // 20, NOT 30 (multiplication first)
let fixed  = (10 + 5) * 2; // 30 — parentheses override the order
```

---

### 🔁 Increment & Decrement

```js
let count = 5;
count++;  // count is now 6 (post-increment: returns value THEN increments)
count--;  // count is now 5 (post-decrement)
++count;  // count is now 6 (pre-increment: increments THEN returns value)
```

---

### 📝 Assignment Operators

| Operator | Example | Equivalent to |
|---|---|---|
| `=` | `x = 5` | `x = 5` |
| `+=` | `x += 3` | `x = x + 3` |
| `-=` | `x -= 2` | `x = x - 2` |
| `*=` | `x *= 4` | `x = x * 4` |
| `/=` | `x /= 2` | `x = x / 2` |

---

### ⚖️ Comparison Operators

Comparison operators always return `true` or `false`.

| Operator | Name | Example | Result |
|---|---|---|---|
| `===` | Strict equality | `5 === 5` | `true` |
| `!==` | Strict inequality | `5 !== 3` | `true` |
| `>` | Greater than | `10 > 5` | `true` |
| `<` | Less than | `3 < 8` | `true` |
| `>=` | Greater or equal | `5 >= 5` | `true` |
| `<=` | Less or equal | `4 <= 3` | `false` |

> ⚠️ **Always use `===` instead of `==`.** The strict version checks both the value AND the type, which prevents sneaky bugs.
>
> ```js
> 5 == "5"   // true  ← dangerous! Different types but JS coerces them
> 5 === "5"  // false ← correct! They are different types
> ```

---

## Control Flow

Control flow is how you tell your program to **make decisions** and **repeat actions**. Without it, your code just runs top to bottom like a one-way street.

---

### 🚦 if / else if / else

```js
let score = 75;

if (score >= 90) {
  console.log("Grade: A");
} else if (score >= 75) {
  console.log("Grade: B");  // This runs
} else if (score >= 60) {
  console.log("Grade: C");
} else {
  console.log("Grade: F");
}
```

Think of it like a bouncer at a club — each condition is a rule, and the first rule that matches is the one that runs.

---

### 🔀 switch Statement

When you have many exact-value checks against one variable, `switch` is cleaner than a chain of `if/else`.

```js
let day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of the week 💪");
    break;
  case "Friday":
    console.log("Almost the weekend! 🎉");
    break;
  case "Saturday":
  case "Sunday":
    console.log("Weekend mode 😎");
    break;
  default:
    console.log("Just another weekday.");
}
```

> ⚠️ Don't forget `break`! Without it, execution "falls through" to the next case even if it doesn't match.

---

### ❓ Ternary Operator

A compact one-liner for simple if/else decisions:

```js
// Syntax: condition ? valueIfTrue : valueIfFalse
let age = 20;
let status = age >= 18 ? "adult" : "minor";
console.log(status); // "adult"
```

Great for short conditions, but don't nest them — it gets hard to read fast.

---

### 🔄 Loops

Loops let you repeat code without writing it over and over.

#### `for` loop — when you know how many times to repeat

```js
for (let i = 1; i <= 5; i++) {
  console.log(`Round ${i}`);
}
// Round 1, Round 2, Round 3, Round 4, Round 5
```

The three parts: `let i = 1` (start), `i <= 5` (condition), `i++` (step).

---

#### `while` loop — repeat as long as a condition is true

```js
let attempts = 0;

while (attempts < 3) {
  console.log(`Attempt ${attempts + 1}`);
  attempts++;
}
```

Use `while` when you don't know upfront how many times you need to loop.

---

#### `do...while` loop — runs at least once, then checks

```js
let input;

do {
  input = getUserInput(); // Imagine this gets input from the user
} while (input !== "quit");
```

The block runs first, then the condition is checked. Perfect when you always need at least one execution.

---

### Loop Control: `break` and `continue`

```js
// break — exit the loop entirely
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0, 1, 2, 3, 4
}

// continue — skip this iteration, go to the next
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  console.log(i); // 0, 1, 3, 4
}
```

---

## Quick Knowledge Check

Test yourself! Try to answer these without looking back:

1. What's the difference between `let`, `const`, and `var`?
2. What does "passed by reference" mean? Give an example.
3. What does `===` check that `==` does not?
4. What is `NaN` and when does it appear?
5. What happens if you forget `break` in a `switch` statement?
6. What's the difference between a **parameter** and an **argument**?
7. When would you use a `do...while` loop instead of a `while` loop?
8. What will `typeof null` return? (This one is a famous JavaScript quirk 🙂)

---

> 🎯 **Next Up:** [Module 02 — Node.js Fundamentals →](../02-NodeJS-Fundamentals/README.md)
>
> You now have the JavaScript foundation to understand Node.js. Let's take it to the server side!
