# 🟨 JavaScript Fundamentals — Exercises & Answers

> 💡 **How to use this file:**
> - Try each exercise on your own first — open VS Code, create a `.js` file, and run it with `node filename.js`
> - Only check the answer after you've given it a real attempt
> - Read the explanation carefully even if you got it right — the *why* matters more than the *what*

---

## 📖 Table of Contents

1. [Variables & Data Types](#1--variables--data-types)
2. [Operators](#2--operators)
3. [Control Flow](#3--control-flow)
4. [Functions](#4--functions)
5. [Arrays & Objects](#5--arrays--objects)
6. [ES6+ Features](#6--es6-features)
7. [Asynchronous JavaScript, Promises & Async/Await](#7--asynchronous-javascript-promises--asyncawait)

---

## 1 — Variables & Data Types

---

### 🟢 Easy

---

**Exercise 1.1 — Declare and log**

Declare three variables:
- `name` holding your name (string)
- `age` holding your age (number)
- `isStudent` holding `true` (boolean)

Log all three to the console.

<details>
<summary>✅ Answer & Explanation</summary>

```js
let name = "Marta";
let age = 21;
let isStudent = true;

console.log(name);      // "Marta"
console.log(age);       // 21
console.log(isStudent); // true
```

**Explanation:**

`let` is used here because these values *could* change (a name can be updated, age changes every year). Each variable holds a different **primitive type**:
- `"Marta"` → `string` (text wrapped in quotes)
- `21` → `number` (all numbers in JS are the same type — no int/float distinction)
- `true` → `boolean` (only two values possible: `true` or `false`)

When you call `console.log(name)`, JavaScript replaces `name` with its value `"Marta"` at execution time. The variable name is just a label pointing to the value.

</details>

---

**Exercise 1.2 — typeof inspector**

What does `typeof` return for each of these? Predict first, then verify in your console:

```js
typeof 42
typeof "hello"
typeof true
typeof undefined
typeof null
typeof {}
typeof []
typeof function(){}
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
typeof 42            // "number"
typeof "hello"       // "string"
typeof true          // "boolean"
typeof undefined     // "undefined"
typeof null          // "object"   ← famous JS bug!
typeof {}            // "object"
typeof []            // "object"   ← arrays are objects in JS
typeof function(){}  // "function"
```

**Explanation:**

`typeof` is an operator (not a function) that returns a string describing the type of a value.

The **biggest surprise** here is `typeof null === "object"`. This is a **well-known bug in JavaScript** that has existed since 1995 and was never fixed because fixing it would break millions of existing websites. In reality, `null` is its own primitive type — it just reports as `"object"` due to how the original JS engine stored type tags in memory.

Another surprise: `typeof []` returns `"object"`, not `"array"`. Arrays in JavaScript are a special kind of object. To properly check if something is an array, use:

```js
Array.isArray([]);   // true
Array.isArray({});   // false
```

</details>

---

**Exercise 1.3 — const vs let**

What happens when you run this code? Why?

```js
const PI = 3.14159;
PI = 3;
console.log(PI);
```

<details>
<summary>✅ Answer & Explanation</summary>

**Output:** `TypeError: Assignment to constant variable.`

The code throws an error on line 2 and never reaches `console.log`.

**Explanation:**

`const` means the **binding** (the connection between the name and value) cannot be changed after it's created. Once you say `const PI = 3.14159`, the name `PI` is permanently locked to that value.

Important distinction — `const` prevents **reassignment**, not **mutation**:

```js
const person = { name: "Abel" };
person.name = "Yonas";    // ✅ This works — we're mutating the object, not reassigning person
person = { name: "Hana" }; // ❌ TypeError — we're trying to reassign the variable itself
```

This trips up many beginners. `const` with objects/arrays doesn't mean the object is frozen — it means the variable can't point to a *different* object.

</details>

---

**Exercise 1.4 — undefined vs null**

What is logged and why?

```js
let score;
let grade = null;

console.log(score);
console.log(grade);
console.log(score === grade);
console.log(score == grade);
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(score);           // undefined
console.log(grade);           // null
console.log(score === grade); // false
console.log(score == grade);  // true  ← interesting!
```

**Explanation:**

`score` was declared with `let` but never assigned a value. JavaScript automatically assigns `undefined` to any declared-but-not-initialized variable.

`grade` was explicitly set to `null` by the programmer — a deliberate signal that "this has no value right now."

**Why does `score == grade` return `true`?**

This is JavaScript's **loose equality** (`==`) at work. JS has a special rule that says `null == undefined` is `true` — they are considered "equal" under loose equality because both represent "emptiness." But with **strict equality** (`===`), they are not equal because they are different types.

This is exactly why we always prefer `===` — it doesn't have these surprising coercion rules.

</details>

---

### 🟡 Medium

---

**Exercise 1.5 — Reference trap**

What is the output? Explain what's happening in memory.

```js
let student = { name: "John", grade: "A" };
let topStudent = student;

topStudent.name = "Marta";
topStudent.grade = "A+";

console.log(student.name);
console.log(student.grade);
console.log(student === topStudent);
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(student.name);        // "Marta"
console.log(student.grade);       // "A+"
console.log(student === topStudent); // true
```

**Explanation:**

This is the classic **pass-by-reference** trap.

When you write `let topStudent = student`, you are **not** copying the object. You are copying the **memory address** (reference) that `student` holds. Both `student` and `topStudent` now point to the **exact same object** in memory.

Visualized:

```
student     ──→  [ memory address: 0x001 ] ──→  { name: "John", grade: "A" }
topStudent  ──→  [ memory address: 0x001 ] ──→  (same object!)
```

When you change `topStudent.name`, you're navigating to address `0x001` and changing the `name` property there. Since `student` also points to `0x001`, it sees the same change.

**How to actually copy an object (avoid this trap):**

```js
// Shallow copy — works for flat objects
let topStudent = { ...student };

// Or
let topStudent = Object.assign({}, student);

// Deep copy — for nested objects
let topStudent = JSON.parse(JSON.stringify(student));

// Modern deep copy (Node.js 17+, browsers)
let topStudent = structuredClone(student);
```

After a proper copy, changing `topStudent` will NOT affect `student`.

</details>

---

**Exercise 1.6 — Data type prediction**

Without running the code, predict the output of each line:

```js
console.log(typeof (1 + 2));
console.log(typeof ("hello" + 1));
console.log(typeof (true + 1));
console.log(typeof (null + 1));
console.log(typeof (undefined + 1));
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(typeof (1 + 2));           // "number"   → 3
console.log(typeof ("hello" + 1));     // "string"   → "hello1"
console.log(typeof (true + 1));        // "number"   → 2
console.log(typeof (null + 1));        // "number"   → 1
console.log(typeof (undefined + 1));   // "number"   → NaN
```

**Explanation — JavaScript Type Coercion:**

JavaScript tries to be "helpful" by automatically converting types when you mix them. This is called **type coercion** and it causes many bugs.

- `"hello" + 1` → `"hello1"`: When `+` sees a string, it converts everything to a string and concatenates. Result is `"string"`.
- `true + 1` → `2`: `true` is coerced to `1` (and `false` to `0`) in numeric contexts. So `1 + 1 = 2`.
- `null + 1` → `1`: `null` is coerced to `0` in numeric contexts. So `0 + 1 = 1`.
- `undefined + 1` → `NaN`: `undefined` cannot be converted to a meaningful number, so the result is `NaN` (Not a Number). But `typeof NaN` is still `"number"` — another JS quirk!

This is why TypeScript (typed JavaScript) was invented — to catch these coercion bugs at compile time.

</details>

---

### 🔴 Challenging

---

**Exercise 1.7 — Deep reference**

What is logged? Explain every line.

```js
let a = [1, 2, 3];
let b = a;
let c = [...a];

b.push(4);
c.push(99);

console.log(a);
console.log(b);
console.log(c);
console.log(a === b);
console.log(a === c);
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(a);       // [1, 2, 3, 4]
console.log(b);       // [1, 2, 3, 4]
console.log(c);       // [1, 2, 3, 99]
console.log(a === b); // true
console.log(a === c); // false
```

**Explanation:**

`let b = a` — `b` is assigned the **same reference** as `a`. They point to the same array in memory. When `b.push(4)` runs, it modifies the array that both `a` and `b` point to.

`let c = [...a]` — the **spread operator** creates a **new array** and copies all values from `a` into it. `c` points to a completely different location in memory. So `c.push(99)` only affects `c`.

`a === b` is `true` because they hold the same memory reference.
`a === c` is `false` because they hold different memory references, even though `c` started with the same values as `a`.

**Key lesson:** `===` on objects/arrays compares **references**, not contents. Two arrays with identical values are NOT `===` unless they are literally the same array.

```js
[1,2,3] === [1,2,3] // false — different objects in memory
```

</details>

---

## 2 — Operators

---

### 🟢 Easy

---

**Exercise 2.1 — Arithmetic**

Calculate and log the results. Predict before running:

```js
console.log(10 + 3);
console.log(10 - 3);
console.log(10 * 3);
console.log(10 / 3);
console.log(10 % 3);
console.log(2 ** 10);
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(10 + 3);   // 13
console.log(10 - 3);   // 7
console.log(10 * 3);   // 30
console.log(10 / 3);   // 3.3333333333333335  ← floating point!
console.log(10 % 3);   // 1
console.log(2 ** 10);  // 1024
```

**Explanation:**

- `10 / 3` → `3.3333...` because JavaScript uses **64-bit floating point** (IEEE 754). There is no integer division by default.
- `10 % 3` → `1` because 3 goes into 10 three times (3×3=9), leaving a remainder of **1**. Modulo is extremely useful for: checking if a number is even/odd (`n % 2 === 0`), cycling through indices in arrays, and clock/time calculations.
- `2 ** 10` → `1024` — This is 2 raised to the power of 10 (2×2×2×2×2×2×2×2×2×2).

</details>

---

**Exercise 2.2 — Operator precedence**

What is the output of each? Don't use a calculator — trace through manually:

```js
console.log(2 + 3 * 4);
console.log((2 + 3) * 4);
console.log(10 - 2 + 3);
console.log(10 - (2 + 3));
console.log(2 ** 3 ** 2);
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(2 + 3 * 4);      // 14  → 3*4=12, then 2+12=14
console.log((2 + 3) * 4);    // 20  → 2+3=5, then 5*4=20
console.log(10 - 2 + 3);     // 11  → left to right: 10-2=8, then 8+3=11
console.log(10 - (2 + 3));   // 5   → 2+3=5, then 10-5=5
console.log(2 ** 3 ** 2);    // 512 → right to left: 3**2=9, then 2**9=512
```

**Explanation:**

JavaScript follows **BODMAS/PEMDAS**:
1. **B**rackets/Parentheses first
2. **O**rders (exponents `**`)
3. **D**ivision and **M**ultiplication (left to right)
4. **A**ddition and **S**ubtraction (left to right)

The tricky one: `2 ** 3 ** 2` — exponentiation is **right-associative**, meaning it evaluates right to left. So it's `2 ** (3 ** 2)` = `2 ** 9` = `512`, NOT `(2 ** 3) ** 2` = `8 ** 2` = `64`.

This is different from most other operators which are left-to-right.

</details>

---

**Exercise 2.3 — Comparison operators**

Predict true or false for each:

```js
console.log(5 === 5);
console.log(5 === "5");
console.log(5 == "5");
console.log(0 == false);
console.log(0 === false);
console.log(null == undefined);
console.log(null === undefined);
console.log(NaN === NaN);
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(5 === 5);           // true
console.log(5 === "5");         // false  — number vs string
console.log(5 == "5");          // true   — "5" is coerced to 5
console.log(0 == false);        // true   — false is coerced to 0
console.log(0 === false);       // false  — number vs boolean
console.log(null == undefined); // true   — special JS rule
console.log(null === undefined);// false  — different types
console.log(NaN === NaN);       // false  — NaN is never equal to anything, including itself!
```

**Explanation:**

This exercise demonstrates exactly why `===` is safer than `==`.

`==` applies **type coercion** before comparing:
- `"5"` is converted to `5` before comparison
- `false` is converted to `0`
- `null` and `undefined` are treated as equal to each other (and nothing else)

`===` never converts — it immediately returns `false` if types differ.

**The most surprising:** `NaN === NaN` is `false`. `NaN` (Not a Number) is the only value in JavaScript that is not equal to itself. To check for NaN, use:

```js
Number.isNaN(NaN);   // true  ← safest method
isNaN("hello");      // true  ← also checks strings (less reliable)
```

</details>

---

### 🟡 Medium

---

**Exercise 2.4 — Increment behavior**

What is the output? Trace through carefully:

```js
let x = 5;
console.log(x++);
console.log(x);
console.log(++x);
console.log(x);
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
let x = 5;
console.log(x++); // 5  ← returns THEN increments
console.log(x);   // 6  ← now x is 6
console.log(++x); // 7  ← increments THEN returns
console.log(x);   // 7  ← still 7
```

**Explanation:**

There are two forms of the increment operator, and they behave differently when used in an expression:

**Post-increment `x++`:** Returns the **current value first**, then increments. Think of it as "use it, then add one."
- `console.log(x++)` → logs `5` (current value), then `x` becomes `6`

**Pre-increment `++x`:** Increments first, then returns the **new value**. Think of it as "add one, then use it."
- `console.log(++x)` → `x` becomes `7`, then logs `7`

In practice, when you just want to increment (not use the return value), both are equivalent:
```js
x++;   // same effect as
++x;   // when result isn't used
```

The difference only matters when the expression's return value is used directly (like inside `console.log` or an assignment).

</details>

---

**Exercise 2.5 — Assignment shorthand**

Rewrite this using assignment operators, then predict the final value of `score`:

```js
let score = 100;
score = score + 20;
score = score - 15;
score = score * 2;
score = score / 3;
score = score % 7;
console.log(score);
```

<details>
<summary>✅ Answer & Explanation</summary>

**Rewritten:**
```js
let score = 100;
score += 20;   // score = 120
score -= 15;   // score = 105
score *= 2;    // score = 210
score /= 3;    // score = 70
score %= 7;    // score = 0
console.log(score); // 0
```

**Trace:**
1. `100 + 20 = 120`
2. `120 - 15 = 105`
3. `105 * 2 = 210`
4. `210 / 3 = 70`
5. `70 % 7 = 0` (70 divides evenly by 7 with no remainder)

**Explanation:**

Assignment operators (`+=`, `-=`, `*=`, `/=`, `%=`) are shorthand — they perform the operation and assign the result back to the variable in one step. They make code cleaner, especially in loops:

```js
// Without shorthand (verbose)
total = total + item.price;

// With shorthand (clean)
total += item.price;
```

</details>

---

### 🔴 Challenging

---

**Exercise 2.6 — Logical operators**

Predict the output. These use `&&` (AND), `||` (OR), and `!` (NOT):

```js
console.log(true && false);
console.log(true || false);
console.log(!true);
console.log(5 > 3 && 10 < 20);
console.log(5 > 3 || 10 > 20);
console.log(0 || "default");
console.log(1 && "yes");
console.log(null ?? "fallback");
console.log(0 ?? "fallback");
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
console.log(true && false);      // false
console.log(true || false);      // true
console.log(!true);              // false
console.log(5 > 3 && 10 < 20);  // true  → true && true = true
console.log(5 > 3 || 10 > 20);  // true  → true || false = true
console.log(0 || "default");     // "default"  ← short-circuit
console.log(1 && "yes");         // "yes"       ← short-circuit
console.log(null ?? "fallback"); // "fallback"
console.log(0 ?? "fallback");    // 0           ← 0 is NOT null/undefined!
```

**Explanation:**

**`&&` (AND):** Returns the first **falsy** value, or the last value if all are truthy.
- `1 && "yes"` → both are truthy, returns the last: `"yes"`

**`||` (OR):** Returns the first **truthy** value, or the last value if all are falsy.
- `0 || "default"` → `0` is falsy, so returns `"default"`

**Short-circuit evaluation:** JS stops evaluating as soon as the result is determined.
- `false && <anything>` → always false, second part never runs
- `true || <anything>` → always true, second part never runs

**`??` (Nullish Coalescing — ES2020):** Returns the right side only if the left is **`null` or `undefined`** specifically. This is different from `||` which triggers on any falsy value (`0`, `""`, `false`).

```js
let count = 0;
let display = count || "No items";   // "No items" — BAD! 0 is valid
let display = count ?? "No items";   // 0          — CORRECT!
```

**Falsy values in JavaScript:** `false`, `0`, `""`, `null`, `undefined`, `NaN`
Everything else is truthy, including `[]`, `{}`, and `"0"`.

</details>

---

## 3 — Control Flow

---

### 🟢 Easy

---

**Exercise 3.1 — Grade classifier**

Write a function `getGrade(score)` that takes a score (0–100) and returns the grade:
- 90–100 → `"A"`, 80–89 → `"B"`, 70–79 → `"C"`, 60–69 → `"D"`, below 60 → `"F"`

Test with: `95`, `83`, `72`, `55`

<details>
<summary>✅ Answer & Explanation</summary>

```js
function getGrade(score) {
  if (score >= 90) return "A";
  else if (score >= 80) return "B";
  else if (score >= 70) return "C";
  else if (score >= 60) return "D";
  else return "F";
}

console.log(getGrade(95)); // "A"
console.log(getGrade(83)); // "B"
console.log(getGrade(72)); // "C"
console.log(getGrade(55)); // "F"
```

**Explanation:**

The key insight here is **ordering**. Because we check `>= 90` first, any score reaching the `>= 80` check is already guaranteed to be less than 90 — it was caught above. This means we never need to write `score >= 80 && score < 90`. The `else if` chain handles the upper boundary automatically.

This is called a **guard clause pattern** — each condition guards a specific range, and the chain flows downward. The moment a condition matches, the function returns immediately and the rest is skipped.

</details>

---

**Exercise 3.2 — Day type with switch**

Write a function `getDayType(day)` that returns `"Weekday"` or `"Weekend"` using a `switch` statement.

Test: `"Monday"`, `"Saturday"`, `"Sunday"`, `"Friday"`

<details>
<summary>✅ Answer & Explanation</summary>

```js
function getDayType(day) {
  switch (day) {
    case "Saturday":
    case "Sunday":
      return "Weekend";
    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
      return "Weekday";
    default:
      return "Invalid day";
  }
}

console.log(getDayType("Monday"));   // "Weekday"
console.log(getDayType("Saturday")); // "Weekend"
console.log(getDayType("Sunday"));   // "Weekend"
console.log(getDayType("Friday"));   // "Weekday"
```

**Explanation:**

This uses **intentional fall-through** — when multiple `case` labels have no `break` or `return` between them, they all share the same code block. `"Saturday"` and `"Sunday"` both fall through to `return "Weekend"`. This is cleaner than repeating the return for every day.

We use `return` instead of `break` here. `return` exits the function entirely. `break` only exits the `switch` and lets execution continue after it.

The `default` case handles unexpected input — always include it.

</details>

---

### 🟡 Medium

---

**Exercise 3.3 — FizzBuzz**

Loop from 1 to 30 and log:
- `"FizzBuzz"` if divisible by both 3 and 5
- `"Fizz"` if divisible by 3 only
- `"Buzz"` if divisible by 5 only
- The number itself otherwise

<details>
<summary>✅ Answer & Explanation</summary>

```js
for (let i = 1; i <= 30; i++) {
  if (i % 3 === 0 && i % 5 === 0) {
    console.log("FizzBuzz");
  } else if (i % 3 === 0) {
    console.log("Fizz");
  } else if (i % 5 === 0) {
    console.log("Buzz");
  } else {
    console.log(i);
  }
}
```

**First 15 outputs:** 1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz...

**Explanation:**

The critical detail is **order of checking**. The `FizzBuzz` case (divisible by both 3 AND 5) must come **first**. If you check `% 3` first, the number 15 would log `"Fizz"` and never reach `"FizzBuzz"`.

`%` is the **modulo operator** — it returns the remainder after division. `15 % 3 === 0` means 15 divides evenly by 3 with no remainder.

This is one of the most famous interview questions in programming. It tests: modulo, conditionals, loops, and logical order of evaluation — all at once.

</details>

---

**Exercise 3.4 — Loop prediction**

What does each loop log? Trace manually before running:

```js
// Loop A
for (let i = 0; i < 5; i += 2) {
  console.log(i);
}

// Loop B
let i = 10;
while (i > 0) {
  console.log(i);
  i -= 3;
}

// Loop C
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) continue;
  if (i > 7) break;
  console.log(i);
}
```

<details>
<summary>✅ Answer & Explanation</summary>

```
Loop A: 0, 2, 4
Loop B: 10, 7, 4, 1
Loop C: 1, 3, 5, 7
```

**Explanation:**

**Loop A** — step is `+= 2` not the default `+1`. Counts 0, 2, 4, then 6 which fails `< 5`.

**Loop B** — trace: 10 → 7 → 4 → 1 → -2 (stops). The last logged value is `1` because after logging `1`, it becomes `-2` which fails `> 0`.

**Loop C** — `continue` skips the rest of the current iteration. `break` exits the loop entirely. Order matters here: the `continue` check runs first, so even numbers are always skipped. At `i=9`: it's odd so not continued, but `9 > 7` is true → `break`. The numbers logged are all odd numbers from 1–7.

</details>

---

### 🔴 Challenging

---

**Exercise 3.5 — Number pattern with nested loops**

Using nested loops, print this pattern:

```
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
for (let row = 1; row <= 5; row++) {
  let line = "";
  for (let col = 1; col <= row; col++) {
    line += col + " ";
  }
  console.log(line.trim());
}
```

**Explanation:**

The **outer loop** controls which row we're on (1–5). The **inner loop** runs from 1 up to the current row number — so on row 3, the inner loop runs 3 times producing "1 2 3".

The inner loop ceiling `col <= row` is what creates the triangle — as `row` grows, the inner loop runs more times.

We build a string `line` inside the inner loop and log once per row, rather than calling `console.log` per number (which would put each number on its own line).

Nested loops are widely used in backend development for: processing 2D data structures, generating reports, working with matrix-shaped database results, and nested API response transformations.

</details>

---

## 4 — Functions

---

### 🟢 Easy

---

**Exercise 4.1 — Build a calculator**

Write four functions: `add`, `subtract`, `multiply`, `divide`. Each takes two numbers and returns the result. Handle division by zero in `divide`.

<details>
<summary>✅ Answer & Explanation</summary>

```js
function add(a, b) { return a + b; }
function subtract(a, b) { return a - b; }
function multiply(a, b) { return a * b; }

function divide(a, b) {
  if (b === 0) return "Error: Cannot divide by zero";
  return a / b;
}

console.log(add(10, 5));      // 15
console.log(subtract(10, 5)); // 5
console.log(multiply(10, 5)); // 50
console.log(divide(10, 5));   // 2
console.log(divide(10, 0));   // "Error: Cannot divide by zero"
```

**Explanation:**

Each function has a **single responsibility** — one job, done well. This is the **Single Responsibility Principle**, a core concept in software engineering.

The `divide` function uses a **guard clause** — checking for the error condition at the top and returning early. Without it, `divide(10, 0)` returns `Infinity` — valid in JS but almost always a bug in real code.

Guard clauses keep the "happy path" (normal execution) clean and at the bottom, with edge cases handled first at the top. This is a pattern you'll use constantly in Node.js route handlers.

</details>

---

**Exercise 4.2 — Default parameters**

Write `greetUser(name, greeting)` where `greeting` defaults to `"Hello"` if not provided.

```js
greetUser("Marta");           // "Hello, Marta!"
greetUser("Abel", "Welcome"); // "Welcome, Abel!"
greetUser();                  // "Hello, stranger!"
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
function greetUser(name = "stranger", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}

console.log(greetUser("Marta"));            // "Hello, Marta!"
console.log(greetUser("Abel", "Welcome"));  // "Welcome, Abel!"
console.log(greetUser());                   // "Hello, stranger!"
```

**Explanation:**

Default parameters (ES6) activate when the argument is `undefined` — either not passed, or explicitly passed as `undefined`.

```js
greetUser(undefined, "Hi"); // "Hi, stranger!" — undefined triggers default
greetUser(null, "Hi");      // "Hi, null!"     — null does NOT trigger default!
```

**Before ES6**, developers used the `||` trick:
```js
name = name || "stranger";
```
This has a flaw: `0`, `false`, and `""` also trigger the fallback, which is usually wrong. ES6 defaults are safer and clearer.

</details>

---

### 🟡 Medium

---

**Exercise 4.3 — Function types & hoisting**

Rewrite this function as a function expression and an arrow function. Then explain the hoisting difference.

```js
function double(n) {
  return n * 2;
}
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// Function Declaration (original)
function double(n) {
  return n * 2;
}

// Function Expression
const double = function(n) {
  return n * 2;
};

// Arrow Function
const double = n => n * 2;

console.log(double(5)); // 10 — all three work identically
```

**Explanation — Hoisting:**

**Function declarations are hoisted** — JavaScript moves the entire function to the top of its scope before execution. You can call it before it appears in the code:

```js
console.log(double(5)); // ✅ 10 — works before the declaration
function double(n) { return n * 2; }
```

**Function expressions and arrow functions are NOT fully hoisted.** The variable is hoisted but `undefined` until the line runs:

```js
console.log(double(5)); // ❌ TypeError: double is not a function
const double = n => n * 2;
```

**Arrow functions** also differ in one critical way: they have no own `this` binding. This makes them wrong for object methods that need `this`, but perfect for callbacks and array methods like `map`, `filter`, `reduce`.

</details>

---

**Exercise 4.4 — Scope challenge**

What is the output? Explain each line:

```js
let message = "global";

function outer() {
  let message = "outer";

  function inner() {
    let message = "inner";
    console.log(message);
  }

  inner();
  console.log(message);
}

outer();
console.log(message);
```

<details>
<summary>✅ Answer & Explanation</summary>

```
"inner"
"outer"
"global"
```

**Explanation — Lexical Scope & Scope Chain:**

JavaScript uses **lexical scope** — a function looks for variables in its own scope first, then walks up to the parent scope, then grandparent, all the way to global. This is called the **scope chain**.

```
global scope  { message: "global" }
  └── outer() { message: "outer"  }
        └── inner() { message: "inner" }
```

When `inner()` runs, it finds its own `message = "inner"` immediately and uses it.
When `outer()` logs after `inner()` returns, it uses its own `message = "outer"`.
Global code uses the global `message = "global"`.

Each function call creates a new **execution context** with its own scope. The inner `message` variables are completely separate — same name, different containers.

**If `inner()` had no local `message`:** It would walk up the scope chain and find `"outer"`.

</details>

---

### 🔴 Challenging

---

**Exercise 4.5 — Closure**

What is the output? Explain what a closure is and why this works:

```js
function makeCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}

const counter1 = makeCounter();
const counter2 = makeCounter();

console.log(counter1()); // ?
console.log(counter1()); // ?
console.log(counter1()); // ?
console.log(counter2()); // ?
console.log(counter2()); // ?
```

<details>
<summary>✅ Answer & Explanation</summary>

```
1
2
3
1
2
```

**Explanation — Closures:**

A **closure** is when a function "remembers" the variables from its parent scope even after the parent function has finished executing.

Step by step:
1. `makeCounter()` runs, creates `count = 0`
2. `makeCounter()` returns the inner function
3. Normally, local variables are garbage collected when a function finishes
4. **But** because the returned inner function still references `count`, JavaScript keeps `count` alive in memory
5. The inner function has "closed over" the `count` variable

Each call to `makeCounter()` creates a **separate** `count`. That's why `counter1` and `counter2` have independent counts — each closed over a different `count` instance.

**Real-world use — private state:**
```js
function createBankAccount(initialBalance) {
  let balance = initialBalance; // private — no direct access from outside

  return {
    deposit(amount)  { balance += amount; },
    withdraw(amount) { balance -= amount; },
    getBalance()     { return balance; }
  };
}

const account = createBankAccount(1000);
account.deposit(500);
console.log(account.getBalance()); // 1500
console.log(account.balance);      // undefined — truly private!
```

Closures are one of the most powerful patterns in JavaScript. You'll see them in Node.js middleware, event handlers, and module patterns constantly.

</details>

---

## 5 — Arrays & Objects

---

### 🟢 Easy

---

**Exercise 5.1 — Array methods**

Given:
```js
let fruits = ["apple", "banana", "mango", "orange", "grape"];
```

1. Add `"pineapple"` to the end
2. Remove the first element and log it
3. Find the index of `"mango"`
4. Check if `"banana"` is in the array
5. Log the total count of fruits

<details>
<summary>✅ Answer & Explanation</summary>

```js
let fruits = ["apple", "banana", "mango", "orange", "grape"];

fruits.push("pineapple");
console.log(fruits); // ["apple", "banana", "mango", "orange", "grape", "pineapple"]

const removed = fruits.shift();
console.log(removed); // "apple"

console.log(fruits.indexOf("mango"));   // 1 (after "apple" removed)
console.log(fruits.includes("banana")); // true
console.log(fruits.length);             // 5
```

**Explanation — Mutating vs Non-mutating methods:**

| Method | Effect | Mutates? | Returns |
|---|---|---|---|
| `push(item)` | Add to end | ✅ Yes | New length |
| `pop()` | Remove from end | ✅ Yes | Removed item |
| `unshift(item)` | Add to start | ✅ Yes | New length |
| `shift()` | Remove from start | ✅ Yes | Removed item |
| `indexOf(item)` | Find position | ❌ No | Index or `-1` |
| `includes(item)` | Check existence | ❌ No | `true`/`false` |
| `.length` | Count | ❌ No | Number |

Knowing which methods mutate the original and which don't is critical — accidental mutation causes hard-to-find bugs, especially when the same array is used in multiple places.

</details>

---

**Exercise 5.2 — Object operations**

Given:
```js
let student = { name: "Hana", age: 20, grades: [85, 90, 78] };
```

1. Log the student's name
2. Add `city: "Dire Dawa"`
3. Update `age` to `21`
4. Delete the `grades` property
5. Log all keys, then all values

<details>
<summary>✅ Answer & Explanation</summary>

```js
let student = { name: "Hana", age: 20, grades: [85, 90, 78] };

console.log(student.name);            // "Hana"

student.city = "Dire Dawa";           // Add new property
student.age = 21;                     // Update existing

delete student.grades;                // Remove property

console.log(Object.keys(student));    // ["name", "age", "city"]
console.log(Object.values(student));  // ["Hana", 21, "Dire Dawa"]
```

**Explanation:**

Objects in JavaScript are **dynamic** — properties can be added, updated, or deleted at any time after creation.

- **Add/Update:** Same syntax `obj.key = value`. If the key exists it overwrites, if not it creates.
- **Delete:** `delete obj.key` fully removes the property. Accessing it afterward returns `undefined`.

`Object.keys()`, `Object.values()`, and `Object.entries()` are essential for iterating objects:

```js
for (const [key, value] of Object.entries(student)) {
  console.log(`${key}: ${value}`);
}
// name: Hana
// age: 21
// city: Dire Dawa
```

</details>

---

### 🟡 Medium

---

**Exercise 5.3 — map, filter, reduce**

Given:
```js
const scores = [45, 82, 67, 91, 55, 78, 88, 34, 95, 61];
```

1. `map` — apply 10% bonus to each score, round to nearest integer
2. `filter` — get only scores above 70
3. `reduce` — calculate the average
4. Chain: apply bonus → filter >= 60 → average of remaining

<details>
<summary>✅ Answer & Explanation</summary>

```js
const scores = [45, 82, 67, 91, 55, 78, 88, 34, 95, 61];

// 1. map
const boosted = scores.map(s => Math.round(s * 1.1));
console.log(boosted); // [50, 90, 74, 100, 61, 86, 97, 37, 105, 67]

// 2. filter
const passing = scores.filter(s => s > 70);
console.log(passing); // [82, 91, 78, 88, 95]

// 3. reduce
const avg = scores.reduce((sum, s) => sum + s, 0) / scores.length;
console.log(avg); // 69.6

// 4. chained
const result = scores
  .map(s => Math.round(s * 1.1))
  .filter(s => s >= 60)
  .reduce((sum, s, _, arr) => sum + s / arr.length, 0);

console.log(result.toFixed(2)); // "79.29"
```

**Explanation:**

These three methods are the backbone of functional data processing:

- **`map(fn)`** — transforms every element, returns a **new array of the same length**. Original untouched.
- **`filter(fn)`** — returns a **new array** with only elements where `fn` returns `true`. Original untouched.
- **`reduce(fn, initial)`** — reduces all elements to a **single value**. The `0` is the initial accumulator value — without it, the first element is used, which breaks for many use cases.

The **chained** version is a data pipeline: each method receives the result of the previous and passes its result to the next. Nothing is mutated at any step. This is **functional programming** style and is extremely common in modern backend code for transforming API responses or database results.

</details>

---

**Exercise 5.4 — Destructuring**

Rewrite this using destructuring:

```js
const user = {
  id: 101,
  firstName: "Dawit",
  lastName: "Bekele",
  address: { city: "Addis Ababa", country: "Ethiopia" },
  scores: [88, 92, 76, 95]
};

const id        = user.id;
const firstName = user.firstName;
const city      = user.address.city;
const topScore  = user.scores[0];
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// Object destructuring
const { id, firstName, lastName } = user;

// Nested destructuring
const { address: { city, country } } = user;

// Array destructuring from object property
const [topScore, secondScore, ...rest] = user.scores;

// All at once
const {
  id,
  firstName,
  address: { city },
  scores: [topScore]
} = user;

console.log(id);        // 101
console.log(firstName); // "Dawit"
console.log(city);      // "Addis Ababa"
console.log(topScore);  // 88
```

**Explanation:**

Destructuring is one of the most-used ES6 features in real projects. It shines in three key places:

**Function parameters:**
```js
function displayUser({ firstName, address: { city } }) {
  console.log(`${firstName} from ${city}`);
}
displayUser(user); // "Dawit from Addis Ababa"
```

**API responses:**
```js
const { data: { users, total }, status } = await response.json();
```

**Module imports:**
```js
const { Router, json } = require('express');
```

</details>

---

### 🔴 Challenging

---

**Exercise 5.5 — Data transformation pipeline**

Given:
```js
const students = [
  { name: "Marta",  scores: [88, 92, 76] },
  { name: "Abel",   scores: [55, 60, 45] },
  { name: "Hana",   scores: [95, 98, 91] },
  { name: "Yonas",  scores: [70, 65, 72] },
  { name: "Tigist", scores: [40, 50, 35] }
];
```

Using one chained expression: add an `average` to each student → filter to only average >= 70 → sort by average descending → return only `name` and `average`.

<details>
<summary>✅ Answer & Explanation</summary>

```js
const result = students
  .map(student => ({
    ...student,
    average: parseFloat(
      (student.scores.reduce((sum, s) => sum + s, 0) / student.scores.length).toFixed(2)
    )
  }))
  .filter(student => student.average >= 70)
  .sort((a, b) => b.average - a.average)
  .map(({ name, average }) => ({ name, average }));

console.log(result);
// [
//   { name: "Hana",  average: 94.67 },
//   { name: "Marta", average: 85.33 },
//   { name: "Yonas", average: 69.0  }  ← 69 is below 70, filtered out
// ]

// Actual result:
// [
//   { name: "Hana",  average: 94.67 },
//   { name: "Marta", average: 85.33 }
// ]
```

**Explanation — step by step:**

**Step 1 — first `map`:** `{ ...student, average: ... }` creates a new object with all original properties **plus** the new `average`. The spread operator copies existing properties without mutating the original. Note the `()` around `{}` — without them, JS treats `{` as a function body, not an object literal.

**Step 2 — `filter`:** Yonas (69) and Tigist (41.67) don't pass the `>= 70` check and are dropped.

**Step 3 — `sort`:** The comparator `(a, b) => b.average - a.average` sorts **descending**. If the result is negative, `a` comes first; positive means `b` comes first. For ascending, flip to `a.average - b.average`.

**Step 4 — second `map`:** Destructures each object to return only `name` and `average`, dropping the `scores` array from the output.

This exact pattern — transform → filter → sort → shape — is what you write every day in Node.js APIs when processing database query results before sending a response.

</details>

---

## 6 — ES6+ Features

---

### 🟢 Easy

---

**Exercise 6.1 — Arrow function conversion**

Convert each to an arrow function:

```js
function square(n) {
  return n * n;
}

function isEven(n) {
  return n % 2 === 0;
}

function greet(name) {
  return "Hello, " + name;
}
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
const square  = n => n * n;
const isEven  = n => n % 2 === 0;
const greet   = name => `Hello, ${name}`;

console.log(square(5));    // 25
console.log(isEven(4));    // true
console.log(greet("Hana")); // "Hello, Hana"
```

**Explanation — Arrow function shorthand rules:**

1. **Single parameter** → parentheses optional: `n => n * n`
2. **Multiple parameters** → parentheses required: `(a, b) => a + b`
3. **No parameters** → empty parens required: `() => "hello"`
4. **Single expression body** → no `{}` or `return` needed (implicit return)
5. **Multi-line body** → must use `{}` and explicit `return`:

```js
const calculate = (a, b) => {
  const sum = a + b;
  return sum * 2;
};
```

The most important behavioral difference: **arrow functions have no own `this`**. They inherit `this` from the surrounding scope. This makes them wrong for object methods but perfect for callbacks.

</details>

---

**Exercise 6.2 — Template literals**

Rewrite these using template literals:

```js
const name = "Selam";
const age = 22;
const city = "Hawassa";

console.log("My name is " + name + " and I am " + age + " years old.");
console.log("I live in " + city + ".");
console.log("In 10 years I will be " + (age + 10) + ".");
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
const name = "Selam";
const age = 22;
const city = "Hawassa";

console.log(`My name is ${name} and I am ${age} years old.`);
console.log(`I live in ${city}.`);
console.log(`In 10 years I will be ${age + 10}.`);
```

**Explanation:**

Template literals use backticks (`` ` ``) and `${}` for interpolation. The expression inside `${}` is **evaluated** — you can put any valid JavaScript expression inside:

```js
`${age + 10}`         // arithmetic
`${isAdult ? "adult" : "minor"}` // ternary
`${user.firstName}`   // object property
`${arr.join(", ")}`   // method call
```

**Multi-line strings** are another huge advantage:
```js
// Old way (ugly)
const html = "<div>\n  <h1>" + name + "</h1>\n</div>";

// Template literal (clean)
const html = `
  <div>
    <h1>${name}</h1>
  </div>
`;
```

You'll use template literals constantly in Node.js for building SQL queries, log messages, HTML responses, and error messages.

</details>

---

### 🟡 Medium

---

**Exercise 6.3 — Spread & Rest**

1. Merge two arrays without `concat`
2. Merge two objects (second object overrides first on conflict)
3. Write a `sum` function that accepts any number of arguments

```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const obj1 = { name: "Abel", role: "student" };
const obj2 = { role: "admin", city: "Jimma" };
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// 1. Merge arrays with spread
const merged = [...arr1, ...arr2];
console.log(merged); // [1, 2, 3, 4, 5, 6]

// 2. Merge objects (obj2 overrides on conflict)
const combined = { ...obj1, ...obj2 };
console.log(combined); // { name: "Abel", role: "admin", city: "Jimma" }
// "role" from obj2 ("admin") overwrites obj1's "role" ("student")

// 3. Rest — collect any number of args into an array
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}
console.log(sum(1, 2, 3));          // 6
console.log(sum(10, 20, 30, 40));   // 100
console.log(sum(5));                // 5
```

**Explanation:**

**Spread `...`** expands an iterable (array/object) into individual elements. Used when **giving** values out.

**Rest `...`** collects multiple values into an array. Used when **receiving** values in.

Same syntax (`...`), opposite purposes. The position tells you which one it is:
- In a **function call** or **array/object literal** → spread (expanding)
- In a **function parameter** or **destructuring** → rest (collecting)

```js
// Spread in function call
console.log(Math.max(...arr1)); // 3 — expands [1,2,3] to Math.max(1,2,3)

// Rest in function parameter
function first(a, b, ...rest) {
  console.log(a, b, rest);
}
first(1, 2, 3, 4, 5); // 1  2  [3, 4, 5]
```

</details>

---

**Exercise 6.4 — Destructuring with defaults and rename**

```js
const config = {
  host: "localhost",
  port: 3000,
  db: "myapp"
};
```

Destructure the object to get:
- `host` as-is
- `port` renamed to `serverPort`
- `db` renamed to `database`
- `timeout` with a default of `5000` (not in the object)

<details>
<summary>✅ Answer & Explanation</summary>

```js
const { host, port: serverPort, db: database, timeout = 5000 } = config;

console.log(host);       // "localhost"
console.log(serverPort); // 3000
console.log(database);   // "myapp"
console.log(timeout);    // 5000 — default, not in original object
```

**Explanation:**

Destructuring syntax for renaming: `{ originalKey: newName }`.
For defaults: `{ key = defaultValue }`.
Combined: `{ originalKey: newName = defaultValue }`.

```js
const { port: serverPort = 8080 } = config; // rename AND default
```

**Why rename?** In real projects you often receive data from external sources (APIs, databases) where key names might conflict with existing variables or don't match your naming conventions.

```js
// API returns "user_name" but we want camelCase
const { user_name: userName, user_id: userId } = apiResponse;
```

This pattern is extremely common in Node.js when destructuring `process.env` for environment variables:
```js
const { PORT = 3000, DB_HOST = "localhost", DB_NAME } = process.env;
```

</details>

---

### 🔴 Challenging

---

**Exercise 6.5 — Modules simulation**

You can't use real `import`/`export` without a bundler in plain Node.js files, so use CommonJS (`module.exports` / `require`). Create two files:

**`mathUtils.js`** — export `add`, `multiply`, and a constant `TAX_RATE = 0.15`
**`app.js`** — import and use all three

<details>
<summary>✅ Answer & Explanation</summary>

```js
// mathUtils.js
function add(a, b) { return a + b; }
function multiply(a, b) { return a * b; }
const TAX_RATE = 0.15;

module.exports = { add, multiply, TAX_RATE };
```

```js
// app.js
const { add, multiply, TAX_RATE } = require('./mathUtils');

const price = 100;
const quantity = 3;
const subtotal = multiply(price, quantity);
const tax = multiply(subtotal, TAX_RATE);
const total = add(subtotal, tax);

console.log(`Subtotal: ${subtotal}`);  // 300
console.log(`Tax: ${tax}`);            // 45
console.log(`Total: ${total}`);        // 345
```

Run: `node app.js`

**Explanation:**

Node.js uses the **CommonJS** module system by default:
- `module.exports = { ... }` — exports values from a file
- `require('./filename')` — imports them (no `.js` extension needed)

**ES Modules** (`import`/`export`) also work in Node.js but require either a `.mjs` extension or `"type": "module"` in `package.json`.

**CommonJS vs ES Modules:**

| Feature | CommonJS | ES Modules |
|---|---|---|
| Syntax | `require` / `module.exports` | `import` / `export` |
| Loading | Synchronous | Asynchronous |
| Default in Node.js | ✅ Yes | Needs config |
| Tree-shakeable | ❌ No | ✅ Yes |
| Used in | Older Node.js code | Modern Node.js / browsers |

In this course you'll use CommonJS first (it's default in Node.js), then ES modules as you progress.

</details>

---

## 7 — Asynchronous JavaScript, Promises & Async/Await

---

### 🟢 Easy

---

**Exercise 7.1 — Event loop prediction**

What is the output order? Predict before running:

```js
console.log("A");

setTimeout(() => console.log("B"), 0);

console.log("C");

setTimeout(() => console.log("D"), 100);

console.log("E");
```

<details>
<summary>✅ Answer & Explanation</summary>

```
A
C
E
B
D
```

**Explanation — The Event Loop:**

JavaScript is **single-threaded** — it executes one thing at a time. But it can handle async tasks through the Event Loop.

Here's what happens:

1. `console.log("A")` — synchronous, runs immediately → **A**
2. `setTimeout(..., 0)` — handed to the browser/Node.js timer API, even with 0ms delay it goes to the callback queue, not the call stack
3. `console.log("C")` — synchronous → **C**
4. `setTimeout(..., 100)` — also handed to the timer API, waits 100ms
5. `console.log("E")` — synchronous → **E**
6. Call stack is now empty. Event Loop checks the callback queue.
7. 0ms timer has expired → runs callback → **B**
8. 100ms timer expires → runs callback → **D**

```
┌──────────────┐    ┌─────────────┐    ┌────────────────┐
│  Call Stack  │    │  Timer API  │    │ Callback Queue │
├──────────────┤    ├─────────────┤    ├────────────────┤
│ log("A")     │    │ 0ms timer   │    │ () => log("B") │
│ log("C")     │    │ 100ms timer │    │ () => log("D") │
│ log("E")     │    └─────────────┘    └────────────────┘
└──────────────┘            ↓ when done, push to queue ↑
         ↑ Event Loop moves from queue to stack when stack is empty
```

This is why even `setTimeout(..., 0)` never runs before synchronous code — it always waits for the call stack to clear.

</details>

---

**Exercise 7.2 — Create a Promise**

Create a Promise called `checkAge` that:
- Receives an `age` parameter
- Resolves with `"Access granted"` if age >= 18
- Rejects with `"Access denied: must be 18+"` otherwise

Consume it with `.then()` and `.catch()`. Test with age `20` and `15`.

<details>
<summary>✅ Answer & Explanation</summary>

```js
function checkAge(age) {
  return new Promise((resolve, reject) => {
    if (age >= 18) {
      resolve("Access granted ✅");
    } else {
      reject("Access denied: must be 18+ ❌");
    }
  });
}

// Test with 20
checkAge(20)
  .then(message => console.log(message))   // "Access granted ✅"
  .catch(error => console.log(error));

// Test with 15
checkAge(15)
  .then(message => console.log(message))
  .catch(error => console.log(error));     // "Access denied: must be 18+ ❌"
```

**Explanation:**

A Promise is constructed with `new Promise((resolve, reject) => { ... })`.

- **`resolve(value)`** — marks the Promise as fulfilled. `.then()` receives the value.
- **`reject(value)`** — marks the Promise as rejected. `.catch()` receives the value.

**Three states of a Promise:**
```
PENDING  → (waiting, async work in progress)
    ↓ resolve()          ↓ reject()
FULFILLED              REJECTED
(.then runs)           (.catch runs)
```

Once a Promise settles (fulfilled or rejected) it **cannot change state**. Calling `resolve` after `reject` (or vice versa) has no effect.

`.finally()` runs after either outcome — useful for cleanup:
```js
checkAge(20)
  .then(msg => console.log(msg))
  .catch(err => console.log(err))
  .finally(() => console.log("Check complete.")); // always runs
```

</details>

---

### 🟡 Medium

---

**Exercise 7.3 — Promise chaining**

Simulate a user login flow using chained Promises:
1. `getUser(id)` — resolves after 500ms with `{ id, name: "Marta" }`
2. `getPermissions(user)` — resolves after 300ms with `["read", "write"]`
3. `logAccess(permissions)` — resolves with `"Access logged"`

Chain all three and log the final result. Add a `.catch()` at the end.

<details>
<summary>✅ Answer & Explanation</summary>

```js
function getUser(id) {
  return new Promise(resolve =>
    setTimeout(() => resolve({ id, name: "Marta" }), 500)
  );
}

function getPermissions(user) {
  return new Promise(resolve =>
    setTimeout(() => resolve({ user, permissions: ["read", "write"] }), 300)
  );
}

function logAccess(data) {
  return new Promise(resolve =>
    setTimeout(() => resolve(`Access logged for ${data.user.name}`), 200)
  );
}

getUser(1)
  .then(user => getPermissions(user))
  .then(data => logAccess(data))
  .then(result => console.log(result))   // "Access logged for Marta"
  .catch(error => console.error("Error:", error));
```

**Explanation:**

Promise chaining works because `.then()` always returns a **new Promise**. If your `.then()` callback returns a Promise, the next `.then()` waits for that Promise to resolve before running.

```
getUser(1) ──resolves──→ .then(user → getPermissions)
                                   ──resolves──→ .then(data → logAccess)
                                                          ──resolves──→ .then(result → log)
```

Total time: ~1000ms (500 + 300 + 200) because steps run **sequentially**.

**One `.catch()` handles all errors in the chain.** If any Promise rejects, execution jumps directly to `.catch()`, skipping all remaining `.then()` calls.

This is a massive improvement over callback hell, where each step needed its own error handler.

</details>

---

**Exercise 7.4 — Promise.all vs sequential**

You need to fetch three independent pieces of data:
- `getUsers()` — takes 300ms
- `getPosts()` — takes 200ms
- `getComments()` — takes 400ms

Write both versions: sequential (one after another) and parallel (all at once). Log how much faster parallel is.

<details>
<summary>✅ Answer & Explanation</summary>

```js
// Simulated async functions
const getUsers    = () => new Promise(r => setTimeout(() => r(["Marta", "Abel"]),  300));
const getPosts    = () => new Promise(r => setTimeout(() => r(["Post 1", "Post 2"]), 200));
const getComments = () => new Promise(r => setTimeout(() => r(["Comment 1"]),       400));

// ❌ Sequential — slow (waits for each to finish before starting next)
async function loadSequential() {
  const start = Date.now();
  const users    = await getUsers();
  const posts    = await getPosts();
  const comments = await getComments();
  console.log(`Sequential: ${Date.now() - start}ms`); // ~900ms
  return { users, posts, comments };
}

// ✅ Parallel — fast (all start at the same time)
async function loadParallel() {
  const start = Date.now();
  const [users, posts, comments] = await Promise.all([
    getUsers(),
    getPosts(),
    getComments()
  ]);
  console.log(`Parallel: ${Date.now() - start}ms`); // ~400ms (longest one)
  return { users, posts, comments };
}

loadSequential();
loadParallel();
```

**Explanation:**

**Sequential `await`:** Each line waits for the previous to finish. Total = 300 + 200 + 400 = **~900ms**.

**`Promise.all` parallel:** All three Promises start simultaneously. Total = the slowest one = **~400ms**. That's **2.25× faster**.

`Promise.all` takes an array of Promises and resolves when **all** of them resolve, with an array of their results (in the same order as input, regardless of which finished first).

If **any** Promise rejects, `Promise.all` immediately rejects. Use `Promise.allSettled` if you want all results even when some fail:

```js
const results = await Promise.allSettled([getUsers(), getPosts(), getComments()]);
results.forEach(r => {
  if (r.status === "fulfilled") console.log(r.value);
  else console.log("Failed:", r.reason);
});
```

**Rule:** Use `Promise.all` when tasks are **independent**. Use sequential `await` only when each task **depends on the previous result**.

</details>

---

### 🔴 Challenging

---

**Exercise 7.5 — Full async flow with error handling**

Write an async function `loadStudentReport(studentId)` that:
1. Fetches student data from `https://jsonplaceholder.typicode.com/users/{id}`
2. Fetches their posts from `https://jsonplaceholder.typicode.com/posts?userId={id}`
3. Returns an object: `{ student: { name, email }, postCount, firstPostTitle }`
4. Handles errors gracefully — if anything fails, log the error and return `null`
5. Test with id `1` and with id `999` (doesn't exist — will return empty array for posts)

<details>
<summary>✅ Answer & Explanation</summary>

```js
async function loadStudentReport(studentId) {
  try {
    // Run both fetches in parallel — they don't depend on each other's result
    const [userResponse, postsResponse] = await Promise.all([
      fetch(`https://jsonplaceholder.typicode.com/users/${studentId}`),
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${studentId}`)
    ]);

    // Check HTTP status — fetch doesn't throw on 404, we check manually
    if (!userResponse.ok) {
      throw new Error(`User not found: HTTP ${userResponse.status}`);
    }

    const user  = await userResponse.json();
    const posts = await postsResponse.json();

    return {
      student: {
        name: user.name,
        email: user.email
      },
      postCount: posts.length,
      firstPostTitle: posts[0]?.title ?? "No posts found"
    };

  } catch (error) {
    console.error(`Failed to load report for student ${studentId}:`, error.message);
    return null;
  }
}

// Test
loadStudentReport(1).then(console.log);
loadStudentReport(999).then(console.log);
```

**Explanation — key concepts used:**

**`Promise.all` for independent requests:** The user data and posts don't depend on each other, so we fetch both at the same time. If we used sequential `await`, we'd wait unnecessarily.

**Manual status check:** `fetch()` only throws on network errors (no internet, DNS failure). A 404 or 500 response is **not** an error to `fetch` — it still resolves. You must check `response.ok` (true for 2xx status codes) and throw manually.

**Two `await` for JSON:** `fetch()` returns a Response object. Calling `.json()` on it returns another Promise — so it also needs `await`. This trips up many beginners.

**Optional chaining `?.`:** `posts[0]?.title` safely accesses `title` even if `posts[0]` is `undefined` (empty array). Without it, you'd get `TypeError: Cannot read property 'title' of undefined`.

**Nullish coalescing `??`:** Returns the right side only if the left is `null` or `undefined` — perfect for providing fallback values.

**`try/catch` in async functions:** This is the equivalent of `.catch()` in Promise chains. Any `await` that rejects will be caught here. Always wrap async operations in `try/catch` in production code.

This pattern — parallel fetch → check status → parse JSON → shape data → handle errors — is the foundation of every Node.js API client you'll ever write.

</details>

---

## 🏁 Final Challenge — Build a Mini Student API Simulator

Put everything together. Build a complete in-memory student management system:

```js
// Requirements:
// 1. Store students in an array of objects: { id, name, scores: [] }
// 2. addStudent(name) — adds a new student, auto-increments id
// 3. addScore(studentId, score) — adds a score to a student
// 4. getAverage(studentId) — returns the student's average score
// 5. getTopStudents(n) — returns top n students by average, sorted descending
// 6. getAllReport() — returns all students with their averages
// 7. Wrap getTopStudents and getAllReport in async functions (simulate real DB delay with setTimeout + Promise)
```

<details>
<summary>✅ Answer & Explanation</summary>

```js
// In-memory "database"
const students = [];
let nextId = 1;

// 1. Add a student
function addStudent(name) {
  const student = { id: nextId++, name, scores: [] };
  students.push(student);
  return student;
}

// 2. Add a score to a student
function addScore(studentId, score) {
  const student = students.find(s => s.id === studentId);
  if (!student) throw new Error(`Student ${studentId} not found`);
  student.scores.push(score);
  return student;
}

// 3. Get average
function getAverage(studentId) {
  const student = students.find(s => s.id === studentId);
  if (!student) throw new Error(`Student ${studentId} not found`);
  if (student.scores.length === 0) return 0;
  return parseFloat(
    (student.scores.reduce((sum, s) => sum + s, 0) / student.scores.length).toFixed(2)
  );
}

// 4. Get top N students (async, simulates DB delay)
async function getTopStudents(n) {
  return new Promise(resolve => {
    setTimeout(() => {
      const ranked = students
        .map(s => ({ ...s, average: getAverage(s.id) }))
        .sort((a, b) => b.average - a.average)
        .slice(0, n)
        .map(({ id, name, average }) => ({ id, name, average }));
      resolve(ranked);
    }, 100);
  });
}

// 5. Get full report (async)
async function getAllReport() {
  return new Promise(resolve => {
    setTimeout(() => {
      const report = students.map(s => ({
        id: s.id,
        name: s.name,
        scores: s.scores,
        average: getAverage(s.id)
      }));
      resolve(report);
    }, 100);
  });
}

// --- Test it ---
addStudent("Marta");
addStudent("Abel");
addStudent("Hana");
addStudent("Yonas");

addScore(1, 88); addScore(1, 92); addScore(1, 76);
addScore(2, 55); addScore(2, 60); addScore(2, 45);
addScore(3, 95); addScore(3, 98); addScore(3, 91);
addScore(4, 70); addScore(4, 65); addScore(4, 72);

async function run() {
  console.log("=== Top 2 Students ===");
  const top = await getTopStudents(2);
  console.log(top);
  // [ { id: 3, name: "Hana", average: 94.67 }, { id: 1, name: "Marta", average: 85.33 } ]

  console.log("\n=== Full Report ===");
  const report = await getAllReport();
  console.log(report);
}

run();
```

**Explanation — what this covers:**

- **Closures:** `nextId` and `students` are closed over by all functions — a real module pattern
- **`Array.find()`:** Locates a student by id, returns `undefined` if not found
- **Guard clauses:** Every function validates input before proceeding
- **`map` + `sort` + `slice`:** The transformation pipeline from Exercise 5.5 used in a real context
- **Promises with `setTimeout`:** Simulates real async database operations
- **`async/await`:** The `run()` function orchestrates everything cleanly
- **Object shaping:** Final `map` returns only the properties the caller needs

This is essentially a simplified version of what your first Node.js REST API will look like — just replace the in-memory array with a real database call.

</details>

---

> 🎯 **Exercises complete!**
>
> Move to **[Module 02 — Node.js Fundamentals →](../02-NodeJS-Fundamentals/README.md)**
>
> You've built a solid JavaScript foundation. Time to run it on the server. 🚀
