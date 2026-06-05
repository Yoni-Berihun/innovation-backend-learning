# 🟨 JavaScript Fundamentals — Exercises & Answers

> 💡 **How to use this file:**
> - Try each exercise on your own first — open VS Code, create a `.js` file, and run it with `node filename.js`
> - Only check the answer after you've given it a real attempt
> - Read the explanation carefully even if you got it right — the *why* matters more than the *what*
> - Prefer the interactive version at `exercises-toggle.html` to show/hide answers with a button

---

## 📖 Table of Contents

1. [Variables & Data Types](#1--variables--data-types)
2. [Operators](#2--operators)
3. [Control Flow](#3--control-flow)
4. [Functions](#4--functions)
5. [Arrays & Objects](#5--arrays--objects)
6. [ES6+ Features](#6--es6-features)
7. [Asynchronous JavaScript, Promises & Async/Await](#7--asynchronous-javascript-promises--asyncawait)
8. [Interview Questions](#8--interview-questions)
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

## 8 — Practice Problems

> 🎯 These are real coding problems. Try to solve them yourself first — the answer is hidden. Tap **"Show Answer"** only after a genuine attempt.

---

### 🟢 String Problems

---

**Problem 1 — Reverse a String**

Write a function that takes a string and returns it reversed.
Concept: Strings, `split`, `reverse`, `join`

<details>
<summary>Show Answer</summary>

```js
function reverseString(str) {
  return str.split('').reverse().join('');
}
console.log(reverseString("hello")); // "olleh"
```

**Explanation:** `split('')` breaks the string into an array of individual characters. `reverse()` reverses the array in place. `join('')` stitches them back into a string. This is the classic one-liner — memorize it.

</details>

---

**Problem 2 — Convert Celsius to Fahrenheit**

Write a function that converts Celsius to Fahrenheit.
Formula: `F = (C × 9/5) + 32`

<details>
<summary>Show Answer</summary>

```js
function celsiusToFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
console.log(celsiusToFahrenheit(25)); // 77
console.log(celsiusToFahrenheit(0));  // 32
console.log(celsiusToFahrenheit(100));// 212
```

**Explanation:** Direct application of the formula. Note operator precedence — multiplication runs before addition, so no parentheses needed around `celsius * 9/5`.

</details>

---

**Problem 3 — Check for a Palindrome**

Write a function that checks if a string is a palindrome (reads same forwards and backwards). Ignore case and non-alphanumeric characters.

<details>
<summary>Show Answer</summary>

```js
function isPalindrome(str) {
  const clean    = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  const reversed = clean.split('').reverse().join('');
  return clean === reversed;
}
console.log(isPalindrome("racecar")); // true
console.log(isPalindrome("hello"));   // false
console.log(isPalindrome("A man a plan a canal Panama")); // true
```

**Explanation:** `/[^a-z0-9]/g` is a regex that matches anything that is NOT a letter or digit — the `^` inside `[]` means "not". `replace` removes all those characters. Then we compare the cleaned string to its reverse.

</details>

---

**Problem 4 — Count Vowels in a String**

Write a function that returns the number of vowels (a, e, i, o, u) in a string.

<details>
<summary>Show Answer</summary>

```js
function countVowels(str) {
  const vowels = "aeiou";
  let count = 0;
  for (let char of str.toLowerCase()) {
    if (vowels.includes(char)) count++;
  }
  return count;
}
console.log(countVowels("javascript")); // 3
console.log(countVowels("hello"));      // 2

// Alternative — one-liner with regex
function countVowels2(str) {
  return (str.match(/[aeiou]/gi) || []).length;
}
```

**Explanation:** `for...of` iterates over each character. `vowels.includes(char)` checks membership. The regex alternative: `/[aeiou]/gi` matches all vowels (case-insensitive), `match` returns an array of matches or `null`, so `|| []` handles the null case.

</details>

---

**Problem 5 — Find Maximum Number in Array**

Write a function that returns the highest value in an array of numbers.

<details>
<summary>Show Answer</summary>

```js
function findMax(arr) {
  return Math.max(...arr);
}
console.log(findMax([1, 5, 3, 9, 2])); // 9

// Without spread — for very large arrays
function findMaxLoop(arr) {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) max = arr[i];
  }
  return max;
}
```

**Explanation:** `Math.max(...arr)` uses the spread operator to pass all array elements as individual arguments. For very large arrays (100,000+ elements), use the loop version — spread can cause a stack overflow on huge arrays.

</details>

---

**Problem 6 — Display Current Day and Time**

Write a program that displays the current day and time in this format:
```
Today is : Tuesday.
Current time is : 10 PM : 30 : 38
```

<details>
<summary>Show Answer</summary>

```js
const days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
const now  = new Date();

const day     = days[now.getDay()];
let   hours   = now.getHours();
const minutes = String(now.getMinutes()).padStart(2, '0');
const seconds = String(now.getSeconds()).padStart(2, '0');
const ampm    = hours >= 12 ? 'PM' : 'AM';
hours = hours % 12 || 12; // convert 0 to 12 for midnight

console.log(`Today is : ${day}.`);
console.log(`Current time is : ${hours} ${ampm} : ${minutes} : ${seconds}`);
```

**Explanation:** `new Date()` creates a Date object for the current moment. `getDay()` returns 0–6 (Sunday–Saturday). `getHours()` returns 0–23, so we convert to 12-hour format with `hours % 12 || 12` (`|| 12` handles midnight which would be 0). `padStart(2,'0')` ensures two digits e.g. `05` not `5`.

</details>

---

**Problem 7 — Get Current Date in Various Formats**

Write a program to display the current date in `mm-dd-yyyy`, `mm/dd/yyyy`, `dd-mm-yyyy`, and `dd/mm/yyyy` formats.

<details>
<summary>Show Answer</summary>

```js
const now = new Date();
const mm  = String(now.getMonth() + 1).padStart(2, '0'); // months are 0-indexed!
const dd  = String(now.getDate()).padStart(2, '0');
const yyyy = now.getFullYear();

console.log(`${mm}-${dd}-${yyyy}`); // mm-dd-yyyy
console.log(`${mm}/${dd}/${yyyy}`); // mm/dd/yyyy
console.log(`${dd}-${mm}-${yyyy}`); // dd-mm-yyyy
console.log(`${dd}/${mm}/${yyyy}`); // dd/mm/yyyy
```

**Explanation:** `getMonth()` returns 0–11, so we add 1 to get 1–12. `getDate()` returns the day of the month (1–31). `getFullYear()` returns the 4-digit year. `padStart(2,'0')` pads single digits with a leading zero.

</details>

---

**Problem 8 — Calculate Area of Triangle (Sides: 5, 6, 7)**

Write a program to find the area of a triangle where the three sides are 5, 6, 7. Use Heron's formula.

<details>
<summary>Show Answer</summary>

```js
function triangleArea(a, b, c) {
  const s    = (a + b + c) / 2; // semi-perimeter
  const area = Math.sqrt(s * (s - a) * (s - b) * (s - c));
  return area.toFixed(2);
}

console.log(triangleArea(5, 6, 7)); // "14.70"
```

**Explanation:** Heron's formula: first compute the semi-perimeter `s = (a+b+c)/2`, then `area = √(s(s−a)(s−b)(s−c))`. `Math.sqrt` computes the square root. `toFixed(2)` rounds to 2 decimal places.

</details>

---

**Problem 9 — Check Leap Year**

Write a function that determines whether a given year is a leap year in the Gregorian calendar.

<details>
<summary>Show Answer</summary>

```js
function isLeapYear(year) {
  return (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);
}

console.log(isLeapYear(2000)); // true  (divisible by 400)
console.log(isLeapYear(1900)); // false (divisible by 100 but not 400)
console.log(isLeapYear(2024)); // true  (divisible by 4, not by 100)
console.log(isLeapYear(2023)); // false
```

**Explanation:** A leap year is divisible by 4 — BUT century years (divisible by 100) are NOT leap years UNLESS they're also divisible by 400. So 2000 is a leap year, 1900 is not.

</details>

---

**Problem 10 — Days Left Before Christmas**

Write a program to calculate the number of days left before Christmas (Dec 25).

<details>
<summary>Show Answer</summary>

```js
function daysUntilChristmas() {
  const now       = new Date();
  let christmas   = new Date(now.getFullYear(), 11, 25); // month 11 = December

  if (now > christmas) {
    // Christmas has passed this year — calculate for next year
    christmas = new Date(now.getFullYear() + 1, 11, 25);
  }

  const msPerDay = 1000 * 60 * 60 * 24;
  const diff     = Math.ceil((christmas - now) / msPerDay);
  return diff;
}

console.log(`${daysUntilChristmas()} days until Christmas!`);
```

**Explanation:** `new Date(year, month, day)` — months are 0-indexed so December is 11. Subtracting two Date objects gives milliseconds difference. Dividing by `msPerDay` converts to days. `Math.ceil` rounds up so today still counts as 1 day.

</details>

---

**Problem 11 — Rotate String**

Write a program to rotate the string `'w3resource'` to the right — periodically remove one letter from the end and attach it to the front.

<details>
<summary>Show Answer</summary>

```js
function rotateString(str, steps) {
  const rotations = [];
  for (let i = 0; i < steps; i++) {
    str = str[str.length - 1] + str.slice(0, str.length - 1);
    rotations.push(str);
  }
  return rotations;
}

console.log(rotateString('w3resource', 5));
// ['ew3resourc', 'cew3resour', 'rcew3resou', 'urcew3reso', 'ourcew3res']
```

**Explanation:** Each rotation takes the last character (`str[str.length - 1]`) and prepends it to the rest (`str.slice(0, str.length - 1)`). We repeat this `steps` times, pushing each result to track the progression.

</details>

---

**Problem 12 — Difference Between Number and 13**

Write a program to get the difference between a number and 13. If the number is greater than 13, return double the absolute difference.

<details>
<summary>Show Answer</summary>

```js
function diff13(n) {
  return n > 13 ? (n - 13) * 2 : 13 - n;
}

console.log(diff13(13)); // 0
console.log(diff13(10)); // 3
console.log(diff13(20)); // 14  (20-13=7, doubled = 14)
```

**Explanation:** Ternary operator handles both cases cleanly. When `n > 13`, the difference is `n - 13`, doubled. Otherwise it's `13 - n`.

</details>

---

**Problem 13 — Sum Two Integers (Triple if Equal)**

Write a program to compute the sum of two integers. If they are equal, return triple their sum.

<details>
<summary>Show Answer</summary>

```js
function sumOrTriple(a, b) {
  const sum = a + b;
  return a === b ? sum * 3 : sum;
}

console.log(sumOrTriple(1, 2)); // 3
console.log(sumOrTriple(3, 3)); // 18  (3+3=6, tripled = 18)
```

</details>

---

**Problem 14 — Check if Number is Multiple of 3 or 7**

Write a program to check if a positive number is a multiple of 3 or 7.

<details>
<summary>Show Answer</summary>

```js
function isMultiple3or7(n) {
  return n % 3 === 0 || n % 7 === 0;
}

console.log(isMultiple3or7(3));  // true
console.log(isMultiple3or7(7));  // true
console.log(isMultiple3or7(21)); // true  (multiple of both)
console.log(isMultiple3or7(10)); // false
```

</details>

---

**Problem 15 — Check if String Starts with 'Java'**

Write a program to check whether a string starts with 'Java'. If it does, return the string, otherwise return `'Not!'`.

<details>
<summary>Show Answer</summary>

```js
function startsWithJava(str) {
  return str.startsWith('Java') ? str : 'Not!';
}

console.log(startsWithJava('JavaScript')); // "JavaScript"
console.log(startsWithJava('Python'));     // "Not!"
console.log(startsWithJava('Java'));       // "Java"
```

**Explanation:** `String.prototype.startsWith(prefix)` returns `true` if the string begins with the given prefix. Clean, readable, and no regex needed.

</details>

---

**Problem 16 — Find Largest of Three Integers**

Write a function to find the largest of three integers.

<details>
<summary>Show Answer</summary>

```js
function largestOfThree(a, b, c) {
  return Math.max(a, b, c);
}

// Without Math.max:
function largestOfThreeManual(a, b, c) {
  if (a >= b && a >= c) return a;
  if (b >= a && b >= c) return b;
  return c;
}

console.log(largestOfThree(5, 10, 3));  // 10
console.log(largestOfThree(-1, -5, 0)); // 0
```

</details>

---

**Problem 17 — Find Closest Value to 100**

Write a program to find the closest value to 100 from two numbers. If both are equidistant, return the larger value.

<details>
<summary>Show Answer</summary>

```js
function closestTo100(a, b) {
  const diffA = Math.abs(100 - a);
  const diffB = Math.abs(100 - b);
  if (diffA === diffB) return Math.max(a, b);
  return diffA < diffB ? a : b;
}

console.log(closestTo100(90, 105)); // 105  (5 away vs 10 away)
console.log(closestTo100(80, 120)); // 120  (both 20 away — return larger)
```

**Explanation:** `Math.abs` gives the absolute distance from 100. When distances are equal, we return the larger number with `Math.max`.

</details>

---

**Problem 18 — Swap First and Last Characters in String**

Write a program to swap the first and last characters of a string. String length must be ≥ 1.

<details>
<summary>Show Answer</summary>

```js
function swapFirstLast(str) {
  if (str.length <= 1) return str;
  return str[str.length - 1] + str.slice(1, -1) + str[0];
}

console.log(swapFirstLast("hello")); // "oellh"
console.log(swapFirstLast("a"));     // "a"
console.log(swapFirstLast("ab"));    // "ba"
```

**Explanation:** Build a new string: last char + everything from index 1 to second-to-last (`slice(1, -1)`) + first char. `slice(1, -1)` on a 2-char string returns an empty string, which is correct.

</details>

---

**Problem 19 — Capitalize First Letter of Each Word**

Write a program to capitalize the first letter of each word in a string.

<details>
<summary>Show Answer</summary>

```js
function titleCase(str) {
  return str
    .split(' ')
    .map(word => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
    .join(' ');
}

console.log(titleCase("hello world")); // "Hello World"
console.log(titleCase("the quick brown fox")); // "The Quick Brown Fox"
```

**Explanation:** Split by space, capitalize the first letter of each word, lowercase the rest, then rejoin. `.charAt(0).toUpperCase()` gets the first character uppercased. `.slice(1).toLowerCase()` gets the remaining characters lowercased.

</details>

---

**Problem 20 — Reverse Alphabetical Order (a→z, z→a)**

Write a program where `a` turns into `z`, `b` into `y`, ..., `z` into `a`. (Mirror the alphabet.)

<details>
<summary>Show Answer</summary>

```js
function mirrorAlphabet(str) {
  return str.split('').map(char => {
    if (char >= 'a' && char <= 'z') {
      return String.fromCharCode(219 - char.charCodeAt(0));
      // 'a'=97, 'z'=122. 97+122=219. So 219-97=122='z', 219-122=97='a'
    }
    return char;
  }).join('');
}

console.log(mirrorAlphabet("hello")); // "svool"
console.log(mirrorAlphabet("abc"));   // "zyx"
```

**Explanation:** `charCodeAt(0)` gets the ASCII code of a character. `String.fromCharCode` converts back. The magic number 219 = 97 ('a') + 122 ('z'). Subtracting any letter's code from 219 gives its mirror.

</details>

---

**Problem 21 — Sort Letters Alphabetically in String**

Write a program to sort the letters of a string alphabetically.

<details>
<summary>Show Answer</summary>

```js
function sortLetters(str) {
  return str.split('').sort().join('');
}

console.log(sortLetters("javascript")); // "aacijprstv"
console.log(sortLetters("hello"));      // "ehllo"
```

</details>

---

**Problem 22 — Count Equal Number of 'p's and 't's**

Write a program to check if a string contains an equal number of `p`s and `t`s.

<details>
<summary>Show Answer</summary>

```js
function equalPT(str) {
  const countP = (str.match(/p/g) || []).length;
  const countT = (str.match(/t/g) || []).length;
  return countP === countT;
}

console.log(equalPT("ptpt"));   // true  (2p, 2t)
console.log(equalPT("ppt"));    // false (2p, 1t)
console.log(equalPT("apple"));  // false (1p, 0t)
```

</details>

---

**Problem 23 — Remove Characters Appearing More Than Once**

Write a program to remove all characters from a string that appear more than once.

<details>
<summary>Show Answer</summary>

```js
function removeRepeated(str) {
  return str.split('').filter((char, index, arr) =>
    arr.indexOf(char) === arr.lastIndexOf(char)
  ).join('');
}

console.log(removeRepeated("hello"));      // "he" — l and o appear once... wait
// h=1, e=1, l=2, o=1 → remove l → "heo"
console.log(removeRepeated("programming")); // chars that appear exactly once
```

**Explanation:** `indexOf(char) === lastIndexOf(char)` is true only when a character appears exactly once — its first occurrence is also its last.

</details>

---

**Problem 24 — Replace First Digit in String with $**

Write a program to replace the first digit in a string with the `$` character. The string must have at least one digit.

<details>
<summary>Show Answer</summary>

```js
function replaceFirstDigit(str) {
  return str.replace(/\d/, '$');
}

console.log(replaceFirstDigit("ab12cd"));  // "ab$2cd"
console.log(replaceFirstDigit("hello5"));  // "hello$"
```

**Explanation:** `/\d/` matches the first digit (no `g` flag = first match only). `replace` substitutes it with `$`. Note: `$` is a special character in `replace` patterns, but here it works because `$` alone isn't a special replacement pattern.

</details>

---

**Problem 25 — Check if Last Digit of Three Numbers is Same**

Write a program to check whether the last digit of three positive integers is the same.

<details>
<summary>Show Answer</summary>

```js
function sameLastDigit(a, b, c) {
  return (a % 10 === b % 10) && (b % 10 === c % 10);
}

console.log(sameLastDigit(23, 13, 3));  // true  (all end in 3)
console.log(sameLastDigit(21, 22, 23)); // false
```

</details>

---

### 🟡 Array Problems

---

**Problem 26 — Find the Longest String in an Array**

Write a function to find the longest string in an array.

<details>
<summary>Show Answer</summary>

```js
function longestString(arr) {
  return arr.reduce((longest, str) =>
    str.length > longest.length ? str : longest
  , '');
}

console.log(longestString(["cat", "elephant", "dog", "hippopotamus"])); // "hippopotamus"
```

**Explanation:** `reduce` tracks the longest seen so far. For each string, if it's longer than the current champion, it becomes the new champion. Starting with `''` handles empty arrays gracefully.

</details>

---

**Problem 27 — Find Largest Even Number in Array**

Write a program to get the largest even number from an array of integers.

<details>
<summary>Show Answer</summary>

```js
function largestEven(arr) {
  const evens = arr.filter(n => n % 2 === 0);
  return evens.length > 0 ? Math.max(...evens) : null;
}

console.log(largestEven([1, 3, 5, 8, 6, 2])); // 8
console.log(largestEven([1, 3, 5]));           // null (no evens)
```

</details>

---

**Problem 28 — Swap First and Last Elements in Array**

Write a program to swap the first and last elements of an array. Length must be ≥ 1.

<details>
<summary>Show Answer</summary>

```js
function swapEnds(arr) {
  if (arr.length <= 1) return arr;
  const result = [...arr]; // don't mutate the original
  [result[0], result[result.length - 1]] = [result[result.length - 1], result[0]];
  return result;
}

console.log(swapEnds([1, 2, 3, 4, 5])); // [5, 2, 3, 4, 1]
console.log(swapEnds([10, 20]));         // [20, 10]
```

**Explanation:** Destructuring assignment `[a, b] = [b, a]` swaps two variables without a temp variable. We spread into `result` first to avoid mutating the original array.

</details>

---

**Problem 29 — Rotate Elements Left in Array (Length 3)**

Write a program to rotate elements left in an array of length 3.

<details>
<summary>Show Answer</summary>

```js
function rotateLeft(arr) {
  return [...arr.slice(1), arr[0]];
}

console.log(rotateLeft([10, 20, 30])); // [20, 30, 10]

// General version — rotate any array left by n steps
function rotateLeftN(arr, n = 1) {
  const steps = n % arr.length;
  return [...arr.slice(steps), ...arr.slice(0, steps)];
}
console.log(rotateLeftN([1, 2, 3, 4, 5], 2)); // [3, 4, 5, 1, 2]
```

</details>

---

**Problem 30 — Create Array with Middle Elements from Two Arrays**

Write a program to create an array taking the middle element from two arrays of length 3.

<details>
<summary>Show Answer</summary>

```js
function middleElements(arr1, arr2) {
  return [arr1[1], arr2[1]];
}

console.log(middleElements([1, 2, 3], [4, 5, 6])); // [2, 5]
```

</details>

---

**Problem 31 — Find Most Frequent Number in Array**

Write a program to find the number appearing most frequently in an array.

<details>
<summary>Show Answer</summary>

```js
function mostFrequent(arr) {
  const freq = {};
  arr.forEach(n => freq[n] = (freq[n] || 0) + 1);

  return Object.entries(freq).reduce((best, [num, count]) =>
    count > best.count ? { num: Number(num), count } : best
  , { num: null, count: 0 }).num;
}

console.log(mostFrequent([1, 3, 2, 3, 5, 3, 2])); // 3
console.log(mostFrequent([1, 1, 2, 2, 3]));         // 1 (first with max freq)
```

**Explanation:** Build a frequency map using an object. Then find the key with the highest count using `reduce`. `Object.entries` gives `[key, value]` pairs. Object keys are always strings, so we convert back with `Number(num)`.

</details>

---

**Problem 32 — Find Maximum Difference Between Adjacent Elements**

Write a program to find the maximum difference between any two adjacent elements of an array.

<details>
<summary>Show Answer</summary>

```js
function maxAdjacentDiff(arr) {
  let max = 0;
  for (let i = 0; i < arr.length - 1; i++) {
    const diff = Math.abs(arr[i + 1] - arr[i]);
    if (diff > max) max = diff;
  }
  return max;
}

console.log(maxAdjacentDiff([1, 2, 10, 3])); // 8  (|10-2|=8)
console.log(maxAdjacentDiff([5, 1, 4, 2]));  // 4  (|1-5|=4)
```

</details>

---

**Problem 33 — Sum of Absolute Differences of Consecutive Numbers**

Write a program to compute the sum of absolute differences of consecutive numbers in an array.

<details>
<summary>Show Answer</summary>

```js
function sumAbsoluteDiffs(arr) {
  let sum = 0;
  for (let i = 0; i < arr.length - 1; i++) {
    sum += Math.abs(arr[i + 1] - arr[i]);
  }
  return sum;
}

console.log(sumAbsoluteDiffs([1, 3, 7, 2])); // |3-1| + |7-3| + |2-7| = 2+4+5 = 11
```

</details>

---

**Problem 34 — Count Inversions in Array**

Write a program to count inversions in an array. Two elements form an inversion if `arr[i] > arr[j]` and `i < j`.

<details>
<summary>Show Answer</summary>

```js
function countInversions(arr) {
  let count = 0;
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] > arr[j]) count++;
    }
  }
  return count;
}

console.log(countInversions([3, 1, 2])); // 2  → (3,1) and (3,2)
console.log(countInversions([1, 2, 3])); // 0  → already sorted
console.log(countInversions([3, 2, 1])); // 3  → (3,2),(3,1),(2,1)
```

**Explanation:** An inversion is a pair that is out of order. The brute-force O(n²) approach checks every pair. For large arrays a merge-sort approach runs O(n log n), but for learning purposes the nested loop is clearest.

</details>

---

**Problem 35 — Find kth Greatest Element**

Write a program to find the kth greatest element in an array of integers.

<details>
<summary>Show Answer</summary>

```js
function kthGreatest(arr, k) {
  const sorted = [...arr].sort((a, b) => b - a); // descending
  return sorted[k - 1];
}

console.log(kthGreatest([3, 1, 4, 1, 5, 9, 2], 1)); // 9  (1st greatest)
console.log(kthGreatest([3, 1, 4, 1, 5, 9, 2], 3)); // 4  (3rd greatest)
```

**Explanation:** Sort descending (largest first), then return the element at index `k-1`. We use `[...arr]` to avoid mutating the original. The comparator `(a, b) => b - a` sorts numerically in descending order.

</details>

---

**Problem 36 — Create Prefix Sum Array**

Write a program to create an array of prefix sums. `y[i] = x[0] + x[1] + ... + x[i]`

<details>
<summary>Show Answer</summary>

```js
function prefixSum(arr) {
  const result = [];
  let sum = 0;
  for (const n of arr) {
    sum += n;
    result.push(sum);
  }
  return result;
}

console.log(prefixSum([1, 2, 3, 4])); // [1, 3, 6, 10]

// One-liner with reduce
const prefixSum2 = arr => arr.reduce((acc, n) => {
  acc.push((acc[acc.length - 1] || 0) + n);
  return acc;
}, []);
```

**Explanation:** Each element in the result is the running total up to that index. Prefix sums are widely used in competitive programming for range sum queries — once built, any subarray sum `arr[i..j]` can be computed in O(1) as `prefix[j] - prefix[i-1]`.

</details>

---

**Problem 37 — Find Max Sum of k Consecutive Numbers**

Write a program to find the maximum possible sum of k consecutive numbers in an array.

<details>
<summary>Show Answer</summary>

```js
function maxKConsecutiveSum(arr, k) {
  // Sliding window — O(n)
  let windowSum = arr.slice(0, k).reduce((a, b) => a + b, 0);
  let maxSum    = windowSum;

  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k]; // add new element, remove oldest
    if (windowSum > maxSum) maxSum = windowSum;
  }
  return maxSum;
}

console.log(maxKConsecutiveSum([1, 4, 2, 10, 2, 3, 1, 0, 20], 4)); // 24 (2+10+2+3? No: 10+2+3+1? 
// let's trace: window sums: [1+4+2+10]=17, [4+2+10+2]=18, [2+10+2+3]=17, [10+2+3+1]=16, [2+3+1+0]=6, [3+1+0+20]=24 → 24
```

**Explanation:** The **sliding window** technique avoids recomputing the sum from scratch for every window. Instead, each step adds the new element entering the window and subtracts the element leaving it. This gives O(n) time instead of O(n×k).

</details>

---

**Problem 38 — Check if Array is Permutation of 1 to n**

Write a program to check if an array of integers is a permutation of numbers from 1 to n.

<details>
<summary>Show Answer</summary>

```js
function isPermutation1toN(arr) {
  const n = arr.length;
  const sorted = [...arr].sort((a, b) => a - b);
  return sorted.every((val, idx) => val === idx + 1);
}

console.log(isPermutation1toN([3, 1, 2]));    // true
console.log(isPermutation1toN([1, 2, 4]));    // false (missing 3, has 4)
console.log(isPermutation1toN([1, 1, 2]));    // false (duplicate)
```

</details>

---

### 🔴 Advanced / Algorithmic Problems

---

**Problem 39 — Number of Trailing Zeros in Factorial**

Write a program to find the number of trailing zeros in `n!`.

<details>
<summary>Show Answer</summary>

```js
function trailingZerosFactorial(n) {
  let count = 0;
  while (n >= 5) {
    n = Math.floor(n / 5);
    count += n;
  }
  return count;
}

console.log(trailingZerosFactorial(5));  // 1  (5! = 120)
console.log(trailingZerosFactorial(10)); // 2  (10! = 3628800)
console.log(trailingZerosFactorial(25)); // 6
```

**Explanation:** Trailing zeros come from factors of 10 = 2 × 5. Since there are always more 2s than 5s in a factorial, we just count factors of 5. Every multiple of 5 contributes one factor, every multiple of 25 contributes an extra, etc. The while loop counts all of them efficiently without computing the actual factorial.

</details>

---

**Problem 40 — Find Distinct Prime Factors**

Write a program to find all distinct prime factors of a given integer.

<details>
<summary>Show Answer</summary>

```js
function primeFactors(n) {
  const factors = [];
  for (let d = 2; d * d <= n; d++) {
    if (n % d === 0) {
      factors.push(d);
      while (n % d === 0) n = Math.floor(n / d); // remove all copies of d
    }
  }
  if (n > 1) factors.push(n); // remaining n is a prime factor
  return factors;
}

console.log(primeFactors(12));  // [2, 3]
console.log(primeFactors(100)); // [2, 5]
console.log(primeFactors(13));  // [13]  (prime)
```

**Explanation:** We test divisibility for each `d` from 2 upward. If `n` is divisible by `d`, we add it to factors and divide out all copies of `d`. We only need to go up to `√n` because if `n` has a factor larger than `√n`, its co-factor must be smaller. Any `n > 1` remaining after the loop is itself a prime factor.

</details>

---

**Problem 41 — Replace $ in Expression to Make True**

Write a program to check whether it is possible to replace `$` in expression `x $ y = z` with `+`, `-`, `*`, or `/` to obtain a correct expression.

<details>
<summary>Show Answer</summary>

```js
function findOperator(x, y, z) {
  if (x + y === z) return '+';
  if (x - y === z) return '-';
  if (x * y === z) return '*';
  if (y !== 0 && x / y === z) return '/';
  return 'No valid operator found';
}

console.log(findOperator(10, 30, 300)); // "*"  (10 * 30 = 300)
console.log(findOperator(10, 5, 2));    // "/"  (10 / 5 = 2)
console.log(findOperator(5, 3, 8));     // "+"  (5 + 3 = 8)
```

</details>

---

**Problem 42 — Dot Product of Two 3D Vectors**

Write a program to create the dot product of two 3D vectors.

<details>
<summary>Show Answer</summary>

```js
function dotProduct(v1, v2) {
  return v1[0] * v2[0] + v1[1] * v2[1] + v1[2] * v2[2];
}

// Generic version for any length
function dotProductN(v1, v2) {
  return v1.reduce((sum, val, i) => sum + val * v2[i], 0);
}

console.log(dotProduct([1, 2, 3], [4, 5, 6])); // 1*4 + 2*5 + 3*6 = 4+10+18 = 32
```

**Explanation:** The dot product multiplies corresponding elements and sums them. It's used everywhere in graphics, machine learning (neural network weights), and physics. The generic `reduce` version works for any vector length.

</details>

---

**Problem 43 — Validate with Luhn Algorithm**

Write a program to validate a credit card number using the Luhn Algorithm.

<details>
<summary>Show Answer</summary>

```js
function luhnCheck(num) {
  const digits = String(num).split('').reverse().map(Number);
  const sum = digits.reduce((total, digit, idx) => {
    if (idx % 2 === 1) { // every second digit from the right
      digit *= 2;
      if (digit > 9) digit -= 9;
    }
    return total + digit;
  }, 0);
  return sum % 10 === 0;
}

console.log(luhnCheck(4532015112830366)); // true  (valid Visa test number)
console.log(luhnCheck(1234567890123456)); // false
```

**Explanation:** The Luhn algorithm: reverse the digits, double every second digit from the right, subtract 9 if the doubled value exceeds 9, then sum all digits. If the total is divisible by 10, it's valid. Used to validate credit card numbers, IMEI numbers, and more.

</details>

---

**Problem 44 — Generate Random Hexadecimal Color Code**

Write a program to generate a random hexadecimal color code.

<details>
<summary>Show Answer</summary>

```js
function randomHexColor() {
  return '#' + Math.floor(Math.random() * 0xFFFFFF)
    .toString(16)
    .padStart(6, '0');
}

console.log(randomHexColor()); // e.g. "#a3f2c1"
console.log(randomHexColor()); // e.g. "#0b7f3d"
```

**Explanation:** `0xFFFFFF` is 16777215 in decimal (max value for a 6-digit hex color). `Math.random() * 0xFFFFFF` gives a random decimal, `Math.floor` makes it an integer, `.toString(16)` converts to hex, and `padStart(6, '0')` ensures exactly 6 characters (e.g. `f3` becomes `0000f3`).

</details>

---

**Problem 45 — Deep Clone an Object**

Write a program to create a deep clone of an object (nested objects should also be cloned, not referenced).

<details>
<summary>Show Answer</summary>

```js
// Method 1 — JSON trick (works for plain data, no functions/Dates/undefined)
function deepCloneJSON(obj) {
  return JSON.parse(JSON.stringify(obj));
}

// Method 2 — structuredClone (Node.js 17+, handles Dates, Maps, Sets)
function deepClone(obj) {
  return structuredClone(obj);
}

// Method 3 — recursive manual clone
function deepCloneManual(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (Array.isArray(obj)) return obj.map(deepCloneManual);
  return Object.fromEntries(
    Object.entries(obj).map(([k, v]) => [k, deepCloneManual(v)])
  );
}

const original = { a: 1, nested: { b: 2, arr: [1, 2, 3] } };
const clone    = deepCloneManual(original);
clone.nested.b = 99;
console.log(original.nested.b); // 2 — original unchanged
```

**Explanation:** The JSON method is the simplest but fails with `Date`, `undefined`, `function`, `Map`, `Set`, and circular references. `structuredClone` handles most cases and is the modern standard. The recursive manual version is educational — it handles arrays and objects but not special types.

</details>

---

**Problem 46 — Find Median of Array**

Write a program to find the median of an array of numbers.

<details>
<summary>Show Answer</summary>

```js
function median(arr) {
  const sorted = [...arr].sort((a, b) => a - b);
  const mid    = Math.floor(sorted.length / 2);
  return sorted.length % 2 !== 0
    ? sorted[mid]
    : (sorted[mid - 1] + sorted[mid]) / 2;
}

console.log(median([1, 3, 2]));        // 2
console.log(median([1, 2, 3, 4]));     // 2.5  ((2+3)/2)
console.log(median([7, 1, 5, 3, 9])); // 5
```

**Explanation:** Sort the array. If odd length, the median is the middle element. If even length, it's the average of the two middle elements. We spread into a new array before sorting to avoid mutating the original.

</details>

---

**Problem 47 — Hash String to Number**

Write a program to hash a string into a whole number.

<details>
<summary>Show Answer</summary>

```js
function hashString(str) {
  let hash = 0;
  for (let i = 0; i < str.length; i++) {
    hash = (hash << 5) - hash + str.charCodeAt(i);
    hash |= 0; // convert to 32-bit integer
  }
  return Math.abs(hash);
}

console.log(hashString("hello"));      // consistent number
console.log(hashString("hello"));      // same number — deterministic
console.log(hashString("Hello"));      // different — case sensitive
```

**Explanation:** This is the djb2 hash function. `hash << 5` shifts bits left by 5 (multiplies by 32), `- hash` makes it `hash * 31`. Adding the char code mixes in the character. `|= 0` keeps it as a 32-bit integer to prevent floating point. Hashing is used for hash maps, checksums, cache keys, and data distribution.

</details>

---

**Problem 48 — Group Array Elements by Function**

Write a program to group elements of an array based on a given function.

<details>
<summary>Show Answer</summary>

```js
function groupBy(arr, fn) {
  return arr.reduce((groups, item) => {
    const key = fn(item);
    if (!groups[key]) groups[key] = [];
    groups[key].push(item);
    return groups;
  }, {});
}

console.log(groupBy([1, 2, 3, 4, 5, 6], n => n % 2 === 0 ? 'even' : 'odd'));
// { odd: [1,3,5], even: [2,4,6] }

console.log(groupBy(['one','two','three','four','five'], w => w.length));
// { 3: ['one','two'], 4: ['four','five'], 5: ['three'] }
```

**Explanation:** `reduce` builds an object where each key is a group name returned by `fn`. This is the same as Lodash's `_.groupBy` — a utility you'll implement constantly when transforming API data.

</details>

---

**Problem 49 — Curry a Function**

Write a program to curry a function — convert a function that takes multiple arguments into a sequence of functions each taking a single argument.

<details>
<summary>Show Answer</summary>

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    }
    return function(...moreArgs) {
      return curried(...args, ...moreArgs);
    };
  };
}

function add(a, b, c) { return a + b + c; }

const curriedAdd = curry(add);

console.log(curriedAdd(1)(2)(3));    // 6  — one arg at a time
console.log(curriedAdd(1, 2)(3));    // 6  — two at once, then one
console.log(curriedAdd(1)(2, 3));    // 6  — one, then two
console.log(curriedAdd(1, 2, 3));    // 6  — all at once
```

**Explanation:** Currying is a fundamental functional programming concept. `fn.length` gives the number of expected arguments. If we have enough, call `fn` directly. Otherwise, return a new function that collects more arguments. This enables **partial application** — creating specialized functions from general ones: `const add5 = curriedAdd(5)`, then `add5(3)` = 8.

</details>

---

**Problem 50 — Perform Deep Comparison Between Two Values**

Write a program to deeply compare two values to check if they are equivalent (all nested properties equal).

<details>
<summary>Show Answer</summary>

```js
function deepEqual(a, b) {
  if (a === b) return true;
  if (a === null || b === null) return false;
  if (typeof a !== typeof b) return false;
  if (typeof a !== 'object') return a === b;

  const keysA = Object.keys(a);
  const keysB = Object.keys(b);

  if (keysA.length !== keysB.length) return false;

  return keysA.every(key => deepEqual(a[key], b[key]));
}

console.log(deepEqual({ a: 1, b: { c: 2 } }, { a: 1, b: { c: 2 } })); // true
console.log(deepEqual({ a: 1, b: { c: 2 } }, { a: 1, b: { c: 3 } })); // false
console.log(deepEqual([1, [2, 3]], [1, [2, 3]]));                       // true
```

**Explanation:** `===` handles primitives and same-reference objects. For different objects, we compare keys count first (fast exit), then recursively compare each key's value. Arrays are objects in JS, so `Object.keys([1,2])` returns `['0','1']` and this works for arrays too.

</details>

---

> 🎯 **All exercises complete!**
>
> Move to **[Module 02 — Node.js Fundamentals →](../02-NodeJS-Fundamentals/README.md)**
>
> You've built a solid JavaScript foundation. Time to run it on the server. 🚀
