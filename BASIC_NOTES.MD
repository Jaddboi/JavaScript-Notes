# Table of Contents

- [What Is JavaScript?](#what-is-javascript)
- [Connecting JavaScript to HTML](#connecting-javascript-to-html)
- [Using the Console and Comments](#using-the-console-and-comments-in-javascript)
- [Mastering Variables](#mastering-variables-in-javascript)
- [Working with Numbers](#working-with-numbers-in-javascript)
- [Grasping Booleans](#grasping-booleans-in-javascript)
- [Manipulating Strings](#manipulating-strings-in-javascript)
- [Type Conversion](#type-conversion-in-javascript)
- [Conditional Statements](#conditional-statements-in-javascript)
- [For Loops](#for-loops-in-javascript)
- [While Loops](#while-loops-in-javascript)
- [Logical Operations](#logical-operations-in-javascript)
- [Introduction to Arrays](#introduction-to-arrays-in-javascript)
- [JavaScript Objects](#javascript-objects)
- [Function Basics](#function-basics-in-javascript)
- [Callback Functions](#callback-functions-in-javascript)
- [Variable Scope](#variable-scope-in-javascript)
- [Object Methods](#object-methods-in-javascript)
- [JSON Handling](#json-handling-in-javascript)
- [Dates and Time](#dates-and-time-in-javascript)
- [`setTimeout()` and `setInterval()`](#settimeout-and-setinterval-in-javascript)

# What Is JavaScript?

JavaScript is a programming language used to make websites interactive. It can respond to user actions, change what appears on a page, and work with data.

For example, JavaScript can:

- Show a message when someone clicks a button
- Check whether a form has been filled out
- Update a page without reloading it
- Fetch information from an API

## HTML, CSS, and JavaScript

| Technology | What it does                                |
| ---------- | ------------------------------------------- |
| HTML       | Defines the content and structure of a page |
| CSS        | Controls how the page looks                 |
| JavaScript | Controls how the page behaves               |

For example, HTML creates a button, CSS styles it, and JavaScript decides what happens when it is clicked.

## Where Does JavaScript Run?

JavaScript commonly runs in a **web browser**, where it can interact with a webpage. It can also run outside the browser using **Node.js**, which lets developers build servers and other programs.

## A Simple Example

```js
const name = "Jadd";

console.log(`Hello, ${name}!`);
```

Here, `const` creates a variable, and `console.log()` prints a message to the console.

## Is JavaScript the Same as Java?

No. Despite their similar names, **JavaScript and Java are different programming languages**.

## Key Takeaway

JavaScript adds behavior and interactivity to websites. It can also be used to write programs outside the browser.

# Connecting JavaScript to HTML

To make a webpage interactive, you need to connect your JavaScript code to your HTML page. The most common way is to put the JavaScript in a separate `.js` file and link it with a `<script>` tag.

## Using an External JavaScript File

Suppose your project has these two files:

```text
project/
├── index.html
└── script.js
```

In `index.html`, add a `<script>` tag:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>My Page</title>
    <script src="script.js" defer></script>
  </head>
  <body>
    <h1>Hello!</h1>
    <button id="greet-button">Click me</button>
  </body>
</html>
```

The `src` attribute tells the browser where to find the JavaScript file. The `defer` attribute tells the browser to wait until it has finished reading the HTML before running the script.

In `script.js`, you can interact with the HTML:

```js
const button = document.querySelector("#greet-button");

button.addEventListener("click", function () {
  alert("Hello!");
});
```

When someone clicks the button, an alert appears.

## File Paths Matter

The value of `src` must point to the correct file. If your JavaScript file is inside a `js` folder, use:

```html
<script src="js/script.js" defer></script>
```

If the path is wrong, the browser cannot load your JavaScript.

## Another Option: Put the Script Before `</body>`

You can also place the `<script>` tag near the end of the HTML body:

```html
<body>
  <h1>Hello!</h1>

  <script src="script.js"></script>
</body>
```

Because the browser reads the HTML from top to bottom, it will reach the script after reading the page content.

## Writing JavaScript Directly in HTML

You can write JavaScript between `<script>` tags:

```html
<script>
  console.log("Hello!");
</script>
```

This works for small examples, but a separate `.js` file is usually easier to organize as your code grows.

## How to Check That It Works

Add this to `script.js`:

```js
console.log("JavaScript is connected!");
```

Open the webpage in your browser, then open the browser's developer tools and look at the **Console** tab. If you see the message, your JavaScript file loaded successfully.

## Key Takeaway

Use `<script src="script.js" defer></script>` to connect an external JavaScript file to an HTML page. Make sure the file path is correct.

# Using the Console and Comments in JavaScript

The **console** helps you see what your JavaScript code is doing. **Comments** let you write notes inside your code for yourself and other developers.

## What Is the Console?

The console is part of your browser's developer tools. You can use it to view messages from your JavaScript code and find errors.

To open it, right-click a webpage, select **Inspect**, and open the **Console** tab.

## Printing Values with `console.log()`

Use `console.log()` to print a value to the console:

```js
console.log("Hello, world!");
```

You can also print variables:

```js
const score = 10;

console.log(score);
```

This is useful when you want to check whether your code is producing the value you expect.

You can print more than one value at a time:

```js
const name = "Jadd";
const age = 20;

console.log(name, age);
```

## Other Console Methods

| Method            | Purpose                               |
| ----------------- | ------------------------------------- |
| `console.log()`   | Prints a general message or value     |
| `console.warn()`  | Prints a warning                      |
| `console.error()` | Prints an error message               |
| `console.table()` | Displays arrays or objects as a table |

```js
console.warn("Your password is too short.");
console.error("Something went wrong.");
```

For an array of objects, `console.table()` can make the data easier to read:

```js
const students = [
  { name: "Alex", grade: 85 },
  { name: "Sam", grade: 92 },
];

console.table(students);
```

## What Are Comments?

Comments are notes written inside your code. JavaScript ignores them when it runs.

Comments are useful for explaining **why** code does something or temporarily disabling a line while debugging.

### Single-Line Comments

Start a single-line comment with `//`:

```js
// Calculate the total price after tax.
const total = 20 * 1.13;
```

You can also place one after a statement:

```js
const taxRate = 0.13; // 13% tax
```

### Multi-Line Comments

Use `/*` and `*/` for a comment that spans multiple lines:

```js
/*
  Calculate the final price
  using the item price and tax rate.
*/
const total = 20 * 1.13;
```

## Comments vs. Console Messages

Comments and console messages serve different purposes:

```js
// Explain why this calculation is needed.
const total = price * 1.13;

console.log(total); // Show the calculated value while debugging.
```

- A **comment** is visible to someone reading the code.
- A **console message** appears when the code runs.

## Key Takeaway

Use the console to inspect values and find problems while your code runs. Use comments to explain parts of your code that might be unclear.

# Mastering Variables in JavaScript

A **variable** is a named place to store a value. You can use that name to access the value later in your code.

```js
const username = "Jadd";

console.log(username); // Jadd
```

Here, `username` is the variable's name, and `"Jadd"` is its value.

## Declaring Variables

JavaScript has three keywords for declaring variables: `const`, `let`, and `var`.

| Keyword | Can you assign a new value? | Typical use                                               |
| ------- | --------------------------- | --------------------------------------------------------- |
| `const` | No                          | Use by default when the variable will keep the same value |
| `let`   | Yes                         | Use when the variable needs to change                     |
| `var`   | Yes                         | Older syntax; generally avoid in new code                 |

### Using `const`

Use `const` when you do not plan to assign a new value to the variable:

```js
const firstName = "Jadd";
const birthYear = 2004;
```

Trying to assign a new value causes an error:

```js
const firstName = "Jadd";

firstName = "Alex"; // Error
```

You must give a `const` variable a value when you declare it:

```js
const score; // Error
```

### Using `let`

Use `let` when the value needs to change:

```js
let score = 0;

score = 10;

console.log(score); // 10
```

You can declare a `let` variable without giving it a value immediately:

```js
let result;

result = 42;
```

Until you assign a value, `result` is `undefined`.

### What About `var`?

You may see `var` in older JavaScript code:

```js
var message = "Hello";
```

For new code, prefer `const` and `let`. They have clearer rules about where a variable can be used.

## Declaration, Assignment, and Reassignment

These words describe different actions:

```js
let points = 5; // Declare the variable and assign its first value
points = 10; // Reassign it to a new value
```

You can use the current value when calculating a new one:

```js
let points = 5;

points = points + 1;

console.log(points); // 6
```

The [Working with Numbers](#working-with-numbers-in-javascript) section introduces shorter assignment operators such as `+=` and `-=`.

## Variables Can Hold Different Types of Values

```js
const name = "Jadd"; // String
const age = 20; // Number
const isStudent = true; // Boolean
const courses = ["CSC369", "CSC343"]; // Array
```

JavaScript does not require you to state a variable's type when you declare it.

A variable declared with `let` can even be assigned a value of a different type later:

```js
let answer = 42;
answer = "forty-two";
```

This is allowed, but keeping a variable's purpose consistent usually makes your code easier to understand.

## Naming Variables

Variable names should describe the values they store:

```js
const totalPrice = 25;
const userEmail = "jadd@example.com";
```

Avoid names that do not explain their purpose:

```js
const x = 25;
const data = "jadd@example.com";
```

JavaScript variable names:

- Can contain letters, numbers, `_`, and `$`
- Cannot start with a number
- Cannot contain spaces or hyphens
- Are case-sensitive: `userName` and `username` are different names
- Cannot be reserved words such as `const` or `return`

JavaScript developers commonly use **camelCase** for variable names:

```js
const firstName = "Jadd";
const totalItemCount = 3;
```

## Using `const` with Arrays and Objects

`const` prevents reassignment, but it does not freeze an array or object. You may change the contents while keeping the same array or object:

```js
const colors = ["red"];
const user = { name: "Jadd" };

colors.push("blue"); // Works
user.name = "Alex"; // Works

colors = ["green"]; // Error
user = { name: "Sam" }; // Error
```

## Key Takeaway

Variables give names to values so you can use them throughout your code. Use `const` by default, use `let` when you need to reassign a value, and choose clear names that describe what each variable stores.

# Working with Numbers in JavaScript

JavaScript uses the `number` type for most numeric values, including whole numbers and decimals.

```js
const age = 20;
const price = 19.99;

console.log(typeof age); // "number"
console.log(typeof price); // "number"
```

## Basic Arithmetic

You can perform calculations with arithmetic operators:

| Operator | Meaning        | Example   | Result     |
| -------- | -------------- | --------- | ---------- |
| `+`      | Addition       | `10 + 3`  | `13`       |
| `-`      | Subtraction    | `10 - 3`  | `7`        |
| `*`      | Multiplication | `10 * 3`  | `30`       |
| `/`      | Division       | `10 / 3`  | `3.333...` |
| `%`      | Remainder      | `10 % 3`  | `1`        |
| `**`     | Exponentiation | `10 ** 3` | `1000`     |

```js
const total = 15 + 5;
const half = total / 2;

console.log(half); // 10
```

The remainder operator `%` is useful for checking whether a number is even:

```js
const number = 8;

console.log(number % 2); // 0
```

A whole number is even when its remainder after division by `2` is `0`.

## Order of Operations

JavaScript follows the usual order of operations: exponentiation, multiplication and division, then addition and subtraction.

```js
console.log(2 + 3 * 4); // 14
```

Use parentheses to make the intended order clear:

```js
console.log((2 + 3) * 4); // 20
```

## Updating a Number

You can calculate a new value and assign it to a variable declared with `let`:

```js
let score = 10;

score = score + 5;

console.log(score); // 15
```

Assignment operators provide a shorter way to write this:

```js
let score = 10;

score += 5; // Same as score = score + 5
score -= 2; // Same as score = score - 2
score *= 3; // Same as score = score * 3
score /= 2; // Same as score = score / 2
```

To increase or decrease a number by one, you can use `++` or `--`:

```js
let count = 0;

count++;
console.log(count); // 1

count--;
console.log(count); // 0
```

## Numbers vs. Numeric Strings

A number and a string containing digits are different values. Be careful with `+`: if either value is a string, JavaScript may join them as text instead of adding them.

```js
console.log(5 + "3"); // "53"
```

The [Type Conversion](#type-conversion-in-javascript) section explains how to convert numeric strings with `Number()` and handle invalid conversions.

## Useful `Math` Methods

JavaScript's `Math` object provides tools for common calculations:

```js
console.log(Math.round(4.6)); // 5: nearest whole number
console.log(Math.floor(4.9)); // 4: rounds down
console.log(Math.ceil(4.1)); // 5: rounds up
console.log(Math.abs(-7)); // 7: absolute value
console.log(Math.max(3, 8)); // 8: largest value
console.log(Math.min(3, 8)); // 3: smallest value
```

`Math.random()` produces a number from `0` up to, but not including, `1`:

```js
console.log(Math.random());
```

For a random whole number from `1` through `6`:

```js
const diceRoll = Math.floor(Math.random() * 6) + 1;
```

## Decimal Precision

Some decimal calculations do not produce an exact result because of how computers store numbers:

```js
console.log(0.1 + 0.2); // 0.30000000000000004
```

For display, you can use `toFixed()` to choose the number of decimal places:

```js
const total = 0.1 + 0.2;

console.log(total.toFixed(2)); // "0.30"
```

**Note:** `toFixed()` returns a **string**, not a number.

## Key Takeaway

Use JavaScript numbers for arithmetic, `Math` methods for common calculations, and `Number()` when you need to convert numeric text. Watch for strings when using `+` and for small precision differences in decimal calculations.

# Grasping Booleans in JavaScript

A **boolean** is a value that is either `true` or `false`. Booleans help your code make decisions.

```js
const isLoggedIn = true;
const hasPermission = false;

console.log(typeof isLoggedIn); // "boolean"
```

The words `true` and `false` do not have quotation marks. `"true"` is a string, not a boolean.

## Comparisons Produce Booleans

Comparison operators let you compare values:

| Operator | Meaning                  | Example   | Result  |
| -------- | ------------------------ | --------- | ------- |
| `>`      | Greater than             | `8 > 5`   | `true`  |
| `<`      | Less than                | `8 < 5`   | `false` |
| `>=`     | Greater than or equal to | `8 >= 8`  | `true`  |
| `<=`     | Less than or equal to    | `8 <= 5`  | `false` |
| `===`    | Strictly equal to        | `8 === 8` | `true`  |
| `!==`    | Not strictly equal to    | `8 !== 5` | `true`  |

```js
const age = 20;
const canEnter = age >= 18;

console.log(canEnter); // true
```

## Strict Equality

Use `===` to check whether two values are equal **and have the same type**:

```js
console.log(5 === 5); // true
console.log(5 === "5"); // false
```

Use `!==` to check whether two values are different:

```js
console.log(5 !== "5"); // true
```

You may also see `==` and `!=`. These can convert values to different types before comparing them, which can lead to surprising results. Prefer `===` and `!==` in most cases.

## Key Takeaway

Booleans represent `true` or `false`. Comparisons produce boolean results, and strict equality (`===` and `!==`) avoids automatic type conversion. Later sections explain how to use booleans in conditional statements and combine them with logical operators.

# Manipulating Strings in JavaScript

A **string** is a sequence of characters used to store text. You can create one with single quotes, double quotes, or backticks:

```js
const firstName = "Jadd";
const greeting = "Hello";
const message = `Welcome!`;

console.log(typeof firstName); // "string"
```

## Combining Strings

You can join strings with the `+` operator:

```js
const firstName = "Jadd";
const lastName = "Akkad";

const fullName = firstName + " " + lastName;

console.log(fullName); // "Jadd Akkad"
```

The `" "` adds a space between the names.

### Template Literals

Backticks let you insert values directly into a string using `${}`:

```js
const name = "Jadd";
const score = 95;

const message = `${name} scored ${score} points.`;

console.log(message); // "Jadd scored 95 points."
```

Template literals can also span multiple lines:

```js
const message = `Hello, Jadd!
Welcome back.`;
```

## String Length

The `.length` property gives you the number of characters in a string:

```js
const word = "JavaScript";

console.log(word.length); // 10
```

Spaces and punctuation count as characters:

```js
console.log("Hi!".length); // 3
```

## Accessing Characters

Each character has a position called an **index**. Indexes start at `0`.

```js
const word = "JavaScript";

console.log(word[0]); // "J"
console.log(word[1]); // "a"
```

You can use `.length - 1` to access the last character:

```js
console.log(word[word.length - 1]); // "t"
```

## Changing Letter Case

Use `.toUpperCase()` and `.toLowerCase()` to create uppercase or lowercase versions of a string:

```js
const word = "JavaScript";

console.log(word.toUpperCase()); // "JAVASCRIPT"
console.log(word.toLowerCase()); // "javascript"
```

This can help when you want to compare text without worrying about capitalization:

```js
const answer = "YES";

console.log(answer.toLowerCase() === "yes"); // true
```

## Removing Surrounding Whitespace

`.trim()` removes spaces, tabs, and line breaks from the beginning and end of a string:

```js
const username = "  Jadd  ";

console.log(username.trim()); // "Jadd"
```

It does not remove spaces between words:

```js
console.log("  Hello world  ".trim()); // "Hello world"
```

## Checking Whether Text Is Present

Use `.includes()` to check whether a string contains another string:

```js
const sentence = "I am learning JavaScript.";

console.log(sentence.includes("JavaScript")); // true
console.log(sentence.includes("Python")); // false
```

The check is case-sensitive:

```js
console.log("JavaScript".includes("javascript")); // false
```

You can also check how a string begins or ends:

```js
const filename = "notes.js";

console.log(filename.startsWith("notes")); // true
console.log(filename.endsWith(".js")); // true
```

## Extracting Part of a String

`.slice(start, end)` creates a new string containing characters from `start` up to, but **not including**, `end`:

```js
const word = "JavaScript";

console.log(word.slice(0, 4)); // "Java"
console.log(word.slice(4)); // "Script"
```

A negative index counts backward from the end:

```js
console.log(word.slice(-6)); // "Script"
```

## Replacing Text

`.replace()` replaces the first matching piece of text:

```js
const sentence = "I like cats. Cats are cute.";

console.log(sentence.replace("cats", "dogs"));
// "I like dogs. Cats are cute."
```

String matching is case-sensitive, so `"cats"` and `"Cats"` are different.

Use `.replaceAll()` to replace every exact match:

```js
const message = "Hi, Jadd. Bye, Jadd.";

console.log(message.replaceAll("Jadd", "Alex"));
// "Hi, Alex. Bye, Alex."
```

## Splitting a String

`.split()` breaks a string into an array:

```js
const colors = "red,blue,green";

console.log(colors.split(","));
// ["red", "blue", "green"]
```

## Strings Cannot Be Changed in Place

Strings are **immutable**. Methods such as `.trim()` and `.toUpperCase()` return new strings; they do not change the original string.

```js
const name = "  jadd  ";

name.trim();
console.log(name); // "  jadd  "
```

To keep the result, store it in a variable:

```js
const name = "  jadd  ";
const cleanName = name.trim().toUpperCase();

console.log(cleanName); // "JADD"
```

You can chain string methods because each method returns a string.

## Key Takeaway

Use string properties and methods to inspect, search, extract, and format text. Most string methods return a **new string**, so save the result if you need to use it later.

# Type Conversion in JavaScript

**Type conversion** means changing a value from one data type to another. For example, you might convert the string `"25"` into the number `25` so you can use it in a calculation.

JavaScript can convert values **automatically** (implicit conversion), or you can convert them yourself (explicit conversion).

## Why Type Conversion Matters

Values that look similar may have different types:

```js
const number = 5;
const text = "5";

console.log(typeof number); // "number"
console.log(typeof text); // "string"
```

This difference affects what JavaScript does with them:

```js
console.log(5 + 3); // 8
console.log("5" + 3); // "53"
```

Because one value is a string in the second example, `+` joins the values as text.

## Explicit Conversion

**Explicit conversion** is when you tell JavaScript to convert a value. This makes your intention clear.

### Converting to a Number

Use `Number()` to convert a value to a number:

```js
const input = "25";
const age = Number(input);

console.log(age); // 25
console.log(typeof age); // "number"
```

Some common results are:

```js
console.log(Number("42")); // 42
console.log(Number("3.5")); // 3.5
console.log(Number("")); // 0
console.log(Number("hello")); // NaN
```

`NaN` means **Not a Number**. It represents the result of an invalid numeric conversion or calculation.

You can check for it with `Number.isNaN()`:

```js
const result = Number("hello");

console.log(Number.isNaN(result)); // true
```

### Converting to a String

Use `String()` to convert a value to text:

```js
const score = 95;
const scoreText = String(score);

console.log(scoreText); // "95"
console.log(typeof scoreText); // "string"
```

You can also insert values into a template literal:

```js
const score = 95;

console.log(`Your score is ${score}.`);
// "Your score is 95."
```

### Converting to a Boolean

Use `Boolean()` to see whether JavaScript treats a value as true or false:

```js
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("hello")); // true
console.log(Boolean("")); // false
```

Common **falsy** values include:

- `false`
- `0`
- `""` (an empty string)
- `null`
- `undefined`
- `NaN`

Most other values are **truthy**. For example, `"false"` is truthy because it is a nonempty string:

```js
console.log(Boolean("false")); // true
```

## Converting Part of a String to a Number

`parseInt()` reads a whole number from the beginning of a string:

```js
console.log(parseInt("25px", 10)); // 25
```

The `10` tells JavaScript to read the number in base 10.

`parseFloat()` reads a decimal number from the beginning of a string:

```js
console.log(parseFloat("19.99 dollars")); // 19.99
```

If you want the **entire string** to be a valid number, use `Number()` instead:

```js
console.log(Number("25px")); // NaN
```

## Implicit Conversion

**Implicit conversion** happens when JavaScript changes a value's type automatically.

```js
console.log("5" * 2); // 10
console.log("5" - 2); // 3
console.log("5" + 2); // "52"
```

The `*` and `-` operators convert `"5"` to a number. The `+` operator joins `"5"` and `2` as strings.

Because automatic conversion can be surprising, convert values explicitly when you expect a particular type.

## Equality and Type Conversion

The `==` operator may convert values before comparing them, while `===` compares without conversion. As explained in [Grasping Booleans](#grasping-booleans-in-javascript), prefer `===` and `!==` for most comparisons so the result is easier to predict.

## Example: Converting User Input

Values entered into an HTML input are typically read as strings. Convert a numeric input before doing arithmetic:

```js
const inputValue = "10";
const total = Number(inputValue) + 5;

console.log(total); // 15
```

Without the conversion:

```js
const inputValue = "10";
const total = inputValue + 5;

console.log(total); // "105"
```

## Key Takeaway

Use `Number()`, `String()`, and `Boolean()` when you want to control a value's type. Be especially careful when adding numeric strings: `+` might join text instead of adding numbers.

# Conditional Statements in JavaScript

**Conditional statements** let your program choose which code to run based on a condition. A condition is an expression that JavaScript evaluates as `true` or `false`.

For example, you can show one message when a user is logged in and another when they are not.

## The `if` Statement

An `if` statement runs its code only when the condition is true:

```js
const age = 20;

if (age >= 18) {
  console.log("You are an adult.");
}
```

Because `20 >= 18` is `true`, the message is printed.

## The `else` Statement

Use `else` to run code when the `if` condition is false:

```js
const age = 16;

if (age >= 18) {
  console.log("You are an adult.");
} else {
  console.log("You are under 18.");
}
```

Only one of these two blocks runs.

## The `else if` Statement

Use `else if` when you need to check multiple conditions:

```js
const grade = 85;

if (grade >= 90) {
  console.log("A");
} else if (grade >= 80) {
  console.log("B");
} else if (grade >= 70) {
  console.log("C");
} else {
  console.log("Below C");
}
```

JavaScript checks the conditions **from top to bottom**. It runs the first block whose condition is true, then skips the rest.

Here, the output is `"B"` because `85 >= 80` is the first true condition.

## Combining Conditions

Use logical operators when a decision depends on more than one condition. The [Logical Operations](#logical-operations-in-javascript) section explains `&&` (AND), `||` (OR), and `!` (NOT) in detail.

## Truthy and Falsy Conditions

An `if` condition does not have to contain an actual boolean. JavaScript converts the condition to a boolean, following the truthy and falsy rules covered in [Type Conversion](#converting-to-a-boolean):

```js
const username = "Jadd";

if (username) {
  console.log("A username was provided.");
}
```

A nonempty string is truthy, so the code runs. An empty string is falsy, so it would not run.

## The Ternary Operator

The **ternary operator** is a short way to choose between two values:

```js
const age = 20;

const message = age >= 18 ? "Adult" : "Under 18";

console.log(message); // "Adult"
```

Its structure is:

```js
condition ? valueIfTrue : valueIfFalse;
```

Use it for simple choices. For longer decisions, an `if` statement is usually easier to read.

## The `switch` Statement

A `switch` statement compares one value against several possible cases:

```js
const day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of the week");
    break;

  case "Friday":
    console.log("Almost the weekend");
    break;

  default:
    console.log("Another day");
}
```

The `break` statement stops JavaScript from continuing into the next case. The `default` block runs when no case matches.

`switch` uses strict matching, so `5` and `"5"` do not match.

## Common Mistakes

### Using `=` instead of `===`

`=` assigns a value. `===` compares two values:

```js
const score = 10;

if (score === 10) {
  console.log("Perfect match!");
}
```

### Putting Broader Conditions First

Because an `else if` chain stops at the first true condition, check more specific or higher thresholds first:

```js
const grade = 95;

if (grade >= 90) {
  console.log("A");
} else if (grade >= 80) {
  console.log("B");
}
```

If `grade >= 80` came first, a grade of `95` would incorrectly enter that block.

## Key Takeaway

Use `if`, `else if`, and `else` to run code based on conditions. Use a ternary operator for a simple choice between two values, and `switch` when comparing one value against several cases.

# For Loops in JavaScript

A **for loop** repeats a block of code while a condition is true. It is especially useful when you know how many times you want the code to run.

## Basic Syntax

```js
for (initialization; condition; update) {
  // Code to repeat
}
```

A common example is:

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

Output:

```text
0
1
2
3
4
```

The three parts of the loop work like this:

| Part           | Example     | Purpose                                    |
| -------------- | ----------- | ------------------------------------------ |
| Initialization | `let i = 0` | Creates the counter before the loop starts |
| Condition      | `i < 5`     | Keeps the loop running while it is true    |
| Update         | `i++`       | Changes the counter after each repetition  |

## How the Loop Runs

For this loop:

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
}
```

JavaScript follows these steps:

1. Set `i` to `0`.
2. Check whether `i < 3`.
3. If true, run the code inside the loop.
4. Increase `i` by `1`.
5. Repeat from step 2.

When `i` reaches `3`, the condition is false and the loop stops.

## Starting and Stopping at Different Numbers

To count from `1` through `5`:

```js
for (let i = 1; i <= 5; i++) {
  console.log(i);
}
```

To count backward:

```js
for (let i = 5; i >= 1; i--) {
  console.log(i);
}
```

To increase by more than one:

```js
for (let i = 0; i <= 10; i += 2) {
  console.log(i);
}
```

Output:

```text
0
2
4
6
8
10
```

## Looping Through an Array

Arrays use indexes that start at `0`. You can use a for loop to visit each element:

```js
const colors = ["red", "blue", "green"];

for (let i = 0; i < colors.length; i++) {
  console.log(colors[i]);
}
```

Output:

```text
red
blue
green
```

`i < colors.length` keeps the index within the array. Since the array has three elements, its valid indexes are `0`, `1`, and `2`.

## Looping Through a String

You can also use a for loop to access each character of a string:

```js
const word = "Hi";

for (let i = 0; i < word.length; i++) {
  console.log(word[i]);
}
```

Output:

```text
H
i
```

## Controlling Loops with `break` and `continue`

These statements work in any JavaScript loop. Use `break` to stop a loop early:

```js
for (let i = 1; i <= 5; i++) {
  if (i === 3) {
    break;
  }

  console.log(i);
}
```

Output:

```text
1
2
```

Use `continue` to skip the rest of the current repetition and move to the next one:

```js
for (let i = 1; i <= 5; i++) {
  if (i === 3) {
    continue;
  }

  console.log(i);
}
```

Output:

```text
1
2
4
5
```

## Common Mistake: An Infinite Loop

Your update must eventually make the condition false. Otherwise, the loop can run forever:

```js
// Do not run this loop:
for (let i = 0; i < 5; i--) {
  console.log(i);
}
```

Here, `i` keeps decreasing, so `i < 5` stays true.

## Key Takeaway

A for loop has three parts: **initialize a counter**, **check a condition**, and **update the counter**. Use it when you need to repeat code a known number of times or work through values by index.

# While Loops in JavaScript

A **while loop** repeats a block of code as long as its condition is true. It is useful when you do not know in advance how many times the code needs to run.

## Basic Syntax

```js
while (condition) {
  // Code to repeat
}
```

JavaScript checks the condition **before** each repetition. If the condition is false from the start, the code inside the loop never runs.

## Counting with a While Loop

```js
let count = 1;

while (count <= 5) {
  console.log(count);
  count++;
}
```

Output:

```text
1
2
3
4
5
```

The loop works like this:

1. Check whether `count <= 5`.
2. If true, run the code inside the loop.
3. Increase `count` by `1`.
4. Check the condition again.
5. Stop when the condition becomes false.

## When Is a While Loop Useful?

A while loop is useful when you want to keep doing something **until a condition changes**.

For example, you might keep removing items from an array until it is empty:

```js
const tasks = ["Study", "Exercise", "Cook"];

while (tasks.length > 0) {
  const nextTask = tasks.shift();
  console.log(`Completed: ${nextTask}`);
}
```

The loop stops when `tasks.length` becomes `0`.

## The `do...while` Loop

A `do...while` loop runs its code **at least once**, then checks the condition:

```js
let count = 10;

do {
  console.log(count);
  count++;
} while (count < 5);
```

Output:

```text
10
```

Even though `count < 5` is false, the code runs once because the condition is checked afterward.

## Controlling a While Loop

The `break` and `continue` statements work the same way in `while` loops as they do in `for` loops: `break` ends the loop, while `continue` skips to the next condition check.

Make sure the value controlling a `while` loop is updated before a possible `continue`. If the condition never becomes false, the loop will run forever.

## Key Takeaway

A while loop checks a condition **before each repetition** and keeps running while that condition is true. Make sure something in the loop can eventually make the condition false.

# Logical Operations in JavaScript

**Logical operations** combine or reverse conditions. They are commonly used in `if` statements when a decision depends on more than one value.

JavaScript has three main logical operators:

| Operator | Name | Basic meaning                      |
| -------- | ---- | ---------------------------------- |
| `&&`     | AND  | Both conditions must be true       |
| &#124;&#124; | OR   | At least one condition must be true |
| `!`      | NOT  | Reverses a condition               |

## AND: `&&`

The AND operator returns `true` when **both** conditions are true:

```js
const age = 20;
const hasTicket = true;

if (age >= 18 && hasTicket) {
  console.log("You can enter.");
}
```

| First condition | Second condition | Result  |
| --------------- | ---------------- | ------- |
| `true`          | `true`           | `true`  |
| `true`          | `false`          | `false` |
| `false`         | `true`           | `false` |
| `false`         | `false`          | `false` |

## OR: `||`

The OR operator returns `true` when **at least one** condition is true:

```js
const isAdmin = false;
const isOwner = true;

if (isAdmin || isOwner) {
  console.log("You can edit this post.");
}
```

| First condition | Second condition | Result  |
| --------------- | ---------------- | ------- |
| `true`          | `true`           | `true`  |
| `true`          | `false`          | `true`  |
| `false`         | `true`           | `true`  |
| `false`         | `false`          | `false` |

## NOT: `!`

The NOT operator reverses a boolean value:

```js
const isLoggedIn = false;

console.log(!isLoggedIn); // true
```

You can use it in a condition:

```js
if (!isLoggedIn) {
  console.log("Please log in.");
}
```

## Combining Operators

You can combine logical operators to express more detailed conditions:

```js
const age = 20;
const hasTicket = true;
const isBanned = false;

if (age >= 18 && hasTicket && !isBanned) {
  console.log("You can enter.");
}
```

Use parentheses to make longer expressions easier to understand:

```js
const isAdmin = false;
const isOwner = true;
const isLoggedIn = true;

if ((isAdmin || isOwner) && isLoggedIn) {
  console.log("You can edit this page.");
}
```

In this example, the user must be logged in **and** be either an admin or the owner.

## Short-Circuit Evaluation

JavaScript does not always evaluate both sides of a logical operator.

With `&&`, if the left side is falsy, the right side is skipped because the entire condition cannot be true:

```js
const isLoggedIn = false;

isLoggedIn && console.log("Welcome back!");
// Nothing is printed
```

With `||`, if the left side is truthy, the right side is skipped because the entire condition is already true:

```js
const isAdmin = true;

isAdmin || console.log("Check other permissions");
// Nothing is printed
```

This behavior is called **short-circuit evaluation**.

## Logical Operators Can Return Non-Boolean Values

When used with values other than booleans, `&&` and `||` return one of their original values:

- `a && b` returns `a` if `a` is falsy; otherwise, it returns `b`.
- `a || b` returns `a` if `a` is truthy; otherwise, it returns `b`.

```js
console.log("" || "Guest"); // "Guest"
console.log("Jadd" || "Guest"); // "Jadd"

console.log(0 && "Continue"); // 0
console.log(5 && "Continue"); // "Continue"
```

This is why `||` is sometimes used to choose a fallback value:

```js
const username = "";
const displayName = username || "Guest";

console.log(displayName); // "Guest"
```

**Be careful:** `||` also uses the fallback for `0`, `false`, and other falsy values. If you only want a fallback for `null` or `undefined`, use the nullish coalescing operator `??`:

```js
const score = 0;

console.log(score || 10); // 10
console.log(score ?? 10); // 0
```

## Operator Precedence

`!` is evaluated before `&&`, and `&&` is evaluated before `||`:

```js
const result = true || (false && false);

console.log(result); // true
```

JavaScript reads that expression as:

```js
const result = true || (false && false);
```

When in doubt, add parentheses so the order is clear.

## Key Takeaway

Use `&&` when all conditions must be true, `||` when at least one must be true, and `!` to reverse a condition. Remember that `&&` and `||` can return non-boolean values and may skip evaluating their right side.

# Introduction to Arrays in JavaScript

An **array** stores multiple values in a single variable. Arrays are useful when you have a list of related items, such as names, grades, or tasks.

```js
const colors = ["red", "blue", "green"];
```

Instead of creating a separate variable for each color, you can keep them together in one array.

## Creating an Array

Use square brackets `[]` to create an array. Separate its values with commas:

```js
const fruits = ["apple", "banana", "orange"];
const scores = [85, 92, 78];
const emptyArray = [];
```

The values inside an array are called **elements**.

An array can hold different types of values:

```js
const mixed = ["Jadd", 20, true];
```

However, keeping related values together usually makes an array easier to work with.

## Accessing Elements

Each element has a numbered position called an **index**. Array indexes start at `0`.

```js
const fruits = ["apple", "banana", "orange"];

console.log(fruits[0]); // "apple"
console.log(fruits[1]); // "banana"
console.log(fruits[2]); // "orange"
```

If you try to access an index that does not exist, you get `undefined`:

```js
console.log(fruits[3]); // undefined
```

## Array Length

The `.length` property tells you how many elements are in an array:

```js
const fruits = ["apple", "banana", "orange"];

console.log(fruits.length); // 3
```

Since indexes start at `0`, the last element is at index `length - 1`:

```js
console.log(fruits[fruits.length - 1]); // "orange"
```

## Changing an Element

You can update an element by assigning a new value to its index:

```js
const fruits = ["apple", "banana", "orange"];

fruits[1] = "grape";

console.log(fruits); // ["apple", "grape", "orange"]
```

## Adding Elements

Use `.push()` to add an element to the **end** of an array:

```js
const fruits = ["apple", "banana"];

fruits.push("orange");

console.log(fruits); // ["apple", "banana", "orange"]
```

Use `.unshift()` to add an element to the **beginning**:

```js
fruits.unshift("grape");

console.log(fruits); // ["grape", "apple", "banana", "orange"]
```

## Removing Elements

Use `.pop()` to remove and return the **last** element:

```js
const fruits = ["apple", "banana", "orange"];

const lastFruit = fruits.pop();

console.log(lastFruit); // "orange"
console.log(fruits); // ["apple", "banana"]
```

Use `.shift()` to remove and return the **first** element:

```js
const firstFruit = fruits.shift();

console.log(firstFruit); // "apple"
console.log(fruits); // ["banana"]
```

## Checking Whether a Value Is in an Array

Use `.includes()` to check whether an array contains a value:

```js
const fruits = ["apple", "banana", "orange"];

console.log(fruits.includes("banana")); // true
console.log(fruits.includes("grape")); // false
```

## Key Takeaway

Arrays store ordered lists of values. Use indexes to access elements, `.length` to find the number of elements, and methods such as `.push()` and `.pop()` to add or remove items.

# JavaScript Objects

An **object** stores related information in one place. It contains **properties**, and each property has a name (called a **key**) and a value.

```js
const student = {
  name: "Jadd",
  age: 20,
  isEnrolled: true,
};
```

Here, `name`, `age`, and `isEnrolled` are keys. Their values are `"Jadd"`, `20`, and `true`.

## Accessing Properties

Use **dot notation** when you know the property name:

```js
console.log(student.name); // "Jadd"
console.log(student.age); // 20
```

You can also use **bracket notation**:

```js
console.log(student["name"]); // "Jadd"
```

Bracket notation is especially useful when the property name is stored in a variable:

```js
const property = "age";

console.log(student[property]); // 20
```

Notice the difference:

```js
student[property]; // Looks up the value of the variable property: "age"
student["property"]; // Looks for a key literally named "property"
```

Accessing a property that does not exist returns `undefined`:

```js
console.log(student.grade); // undefined
```

## Changing and Adding Properties

Assign a new value to change an existing property:

```js
student.age = 21;

console.log(student.age); // 21
```

Assign a value to a new key to add a property:

```js
student.major = "Computer Science";

console.log(student.major); // "Computer Science"
```

You can do both with bracket notation:

```js
student["isEnrolled"] = false;
student["graduationYear"] = 2027;
```

## Removing a Property

Use `delete` to remove a property:

```js
delete student.graduationYear;

console.log(student.graduationYear); // undefined
```

## Objects Can Contain Other Values

A property's value can be a string, number, boolean, array, or even another object:

```js
const student = {
  name: "Jadd",
  courses: ["JavaScript", "React"],
  address: {
    city: "Toronto",
    country: "Canada",
  },
};
```

Access nested values by following each level:

```js
console.log(student.courses[0]); // "JavaScript"
console.log(student.address.city); // "Toronto"
```

An object can also contain functions. These are called **methods** and are covered in [Object Methods](#object-methods-in-javascript).

## Checking Whether a Property Exists

Use `Object.hasOwn()` to check whether an object has a property:

```js
const student = { name: "Jadd" };

console.log(Object.hasOwn(student, "name")); // true
console.log(Object.hasOwn(student, "age")); // false
```

This is helpful when a property could exist but have the value `undefined`.

## Getting an Object's Keys and Values

```js
const student = {
  name: "Jadd",
  age: 20,
};

console.log(Object.keys(student)); // ["name", "age"]
console.log(Object.values(student)); // ["Jadd", 20]
```

`Object.entries()` gives you an array of key-value pairs:

```js
console.log(Object.entries(student));
// [["name", "Jadd"], ["age", 20]]
```

## Objects Are Compared by Reference

Two separate objects are not strictly equal, even if they contain the same properties:

```js
const first = { name: "Jadd" };
const second = { name: "Jadd" };

console.log(first === second); // false
```

Two variables that refer to the **same object** are strictly equal:

```js
const first = { name: "Jadd" };
const second = first;

console.log(first === second); // true
```

Changing the object through either variable affects the same object:

```js
second.name = "Alex";

console.log(first.name); // "Alex"
```

## Objects vs. Arrays

| Use an object when...                                | Use an array when...              |
| ---------------------------------------------------- | --------------------------------- |
| You want to describe something with named properties | You want an ordered list of items |
| Example: a student with a name and age               | Example: a list of student names  |

```js
const student = { name: "Jadd", age: 20 };
const names = ["Jadd", "Alex", "Sam"];
```

## Key Takeaway

Objects group related values under named keys. Use dot notation for known property names and bracket notation when a property name comes from a variable. You can add, change, and remove properties after creating an object.

# Function Basics in JavaScript

A **function** is a reusable block of code that performs a task. You write the code once and run it whenever you need it.

## Declaring and Calling a Function

Use the `function` keyword to declare a function:

```js
function greet() {
  console.log("Hello!");
}
```

The code inside the function does not run until you **call** it:

```js
greet(); // "Hello!"
greet(); // "Hello!"
```

The parentheses `()` are part of the function call.

## Parameters and Arguments

A **parameter** is a variable listed when you define a function. An **argument** is the value you provide when you call it.

```js
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet("Jadd"); // "Hello, Jadd!"
greet("Alex"); // "Hello, Alex!"
```

Here, `name` is the parameter. `"Jadd"` and `"Alex"` are arguments.

A function can accept multiple parameters:

```js
function introduce(name, age) {
  console.log(`My name is ${name}, and I am ${age} years old.`);
}

introduce("Jadd", 20);
```

Arguments are matched to parameters in order: `"Jadd"` becomes `name`, and `20` becomes `age`.

## Returning a Value

Use `return` when you want a function to produce a value that other code can use:

```js
function add(a, b) {
  return a + b;
}

const total = add(5, 3);

console.log(total); // 8
```

A function call can be used wherever its returned value is needed:

```js
console.log(add(10, 4)); // 14
```

### `return` vs. `console.log()`

`console.log()` displays a value in the console. It does not return that value from the function.

```js
function addAndPrint(a, b) {
  console.log(a + b);
}

const result = addAndPrint(5, 3); // Prints 8

console.log(result); // undefined
```

Use `return` when the result needs to be saved or used in another calculation.

## `return` Stops the Function

Once JavaScript reaches `return`, it exits the function:

```js
function checkAge(age) {
  if (age < 18) {
    return "Under 18";
  }

  return "18 or older";
}

console.log(checkAge(16)); // "Under 18"
console.log(checkAge(20)); // "18 or older"
```

When `age` is `16`, the first `return` runs, so JavaScript never reaches the second one.

If a function does not return a value, its result is `undefined`.

## Default Parameters

A **default parameter** provides a value when an argument is omitted:

```js
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

console.log(greet("Jadd")); // "Hello, Jadd!"
console.log(greet()); // "Hello, Guest!"
```

Without a default value, a missing argument becomes `undefined`:

```js
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet(); // "Hello, undefined!"
```

## Function Expressions

You can store a function in a variable:

```js
const greet = function (name) {
  return `Hello, ${name}!`;
};

console.log(greet("Jadd")); // "Hello, Jadd!"
```

This is called a **function expression**.

## Arrow Functions

An **arrow function** is another way to write a function expression:

```js
const add = (a, b) => {
  return a + b;
};

console.log(add(5, 3)); // 8
```

For a simple expression, you can omit the braces and `return`:

```js
const add = (a, b) => a + b;

console.log(add(5, 3)); // 8
```

Arrow functions have some different behavior from regular functions, especially with `this`. For now, focus on how to define, call, and return values from functions.

## Key Takeaway

Functions let you group and reuse code. **Parameters** receive input, **arguments** provide that input when you call the function, and `return` sends a result back to the code that called it.

# Callback Functions in JavaScript

A **callback function** is a function passed to another function as an argument. The receiving function decides **when** to call it.

Callbacks are useful when you want to tell a function what action to perform.

## A Simple Callback

```js
function greet(name) {
  console.log(`Hello, ${name}!`);
}

function processUser(callback) {
  const username = "Jadd";
  callback(username);
}

processUser(greet); // "Hello, Jadd!"
```

Here:

1. `greet` is passed into `processUser`.
2. `processUser` calls it with `"Jadd"`.
3. The message is printed.

## Pass the Function, Not Its Result

When passing a callback, use the function's name **without** parentheses:

```js
processUser(greet); // Passes the function
```

Adding parentheses calls the function immediately and passes its return value instead:

```js
processUser(greet()); // Calls greet immediately
```

## Writing a Callback Directly

You can define a callback where you pass it:

```js
function processUser(callback) {
  const username = "Jadd";
  callback(username);
}

processUser(function (name) {
  console.log(`Welcome, ${name}!`);
});
```

This is an **anonymous function** because it does not have a name.

You can also use an arrow function:

```js
processUser((name) => {
  console.log(`Welcome, ${name}!`);
});
```

## Callbacks with Arrays

Many array methods accept callbacks. For example, `.forEach()` calls its callback once for each element:

```js
const colors = ["red", "blue", "green"];

colors.forEach((color) => {
  console.log(color);
});
```

Output:

```text
red
blue
green
```

The array method supplies each `color` argument when it calls the callback. You do not call the callback yourself.

A callback can receive additional information. With `.forEach()`, the second argument is the element's index:

```js
const colors = ["red", "blue", "green"];

colors.forEach((color, index) => {
  console.log(`${index}: ${color}`);
});
```

Output:

```text
0: red
1: blue
2: green
```

## Callbacks Can Return Values

Some functions use the value returned by their callback. For example, `.map()` creates a new array from the values its callback returns:

```js
const numbers = [1, 2, 3];

const doubled = numbers.map((number) => {
  return number * 2;
});

console.log(doubled); // [2, 4, 6]
```

Different functions use callback return values differently. For example, `.forEach()` ignores the value returned by its callback.

## Event Listeners Use Callbacks

In a webpage, you can pass a callback that runs when an event happens:

```js
const button = document.querySelector("#save-button");

button.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

The callback runs each time the button is clicked. The browser calls it for you.

## Key Takeaway

A callback is a function you give to another function. Pass it **without calling it**; the receiving function will call it when appropriate. Callbacks are common in array methods, timers, and event listeners.

# Variable Scope in JavaScript

**Scope** determines where a variable can be accessed in your code. A variable may be available throughout a file, only inside a function, or only inside a block of code.

Understanding scope helps you avoid accidentally using or changing the wrong variable.

## Global Scope

A variable declared outside of any function or block is in the **global scope** of that script. Code in that script can access it from inside functions and blocks:

```js
const appName = "StudyFlow";

function showAppName() {
  console.log(appName);
}

showAppName(); // "StudyFlow"
```

Avoid putting every variable in global scope. As your program grows, it becomes harder to keep track of which code uses or changes each value.

## Function Scope

A variable declared inside a function can be accessed inside that function, but not outside it:

```js
function greet() {
  const message = "Hello!";
  console.log(message); // Works
}

greet();

console.log(message); // Error: message is not defined
```

Function parameters are also local to the function:

```js
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet("Jadd"); // "Hello, Jadd!"

console.log(name); // Error: name is not defined
```

Separate function calls get their own local variables:

```js
function double(number) {
  const result = number * 2;
  return result;
}

console.log(double(3)); // 6
console.log(double(5)); // 10
```

## Block Scope

A **block** is code surrounded by curly braces `{}`. Variables declared with `let` or `const` inside a block can only be accessed inside that block.

```js
if (true) {
  const message = "Inside the block";
  let count = 5;

  console.log(message); // Works
  console.log(count); // Works
}

console.log(message); // Error
console.log(count); // Error
```

Loops also create blocks:

```js
for (let i = 0; i < 3; i++) {
  const label = `Item ${i}`;
  console.log(label);
}

console.log(i); // Error
console.log(label); // Error
```

## Inner Code Can Access Outer Variables

A function or block can usually access variables declared in the scopes around it:

```js
const greeting = "Hello";

function greet(name) {
  const punctuation = "!";

  if (name) {
    console.log(`${greeting}, ${name}${punctuation}`);
  }
}

greet("Jadd"); // "Hello, Jadd!"
```

The `if` block can access `greeting`, `name`, and `punctuation`. Code outside the function cannot access `name` or `punctuation`.

JavaScript determines this from **where the function is written**, not where it is called. This is called _lexical scope_.

## Variable Shadowing

An inner scope can declare a variable with the same name as one in an outer scope. The inner variable **shadows** the outer one:

```js
const message = "Outside";

function showMessage() {
  const message = "Inside";
  console.log(message);
}

showMessage(); // "Inside"
console.log(message); // "Outside"
```

Inside `showMessage()`, `message` refers to the local variable. The outer variable still exists.

## `var` Has Different Scope Rules

Unlike `let` and `const`, `var` is **not block-scoped**. A `var` declared inside an `if` block remains accessible outside that block within the same function or script:

```js
if (true) {
  var oldStyle = "I am still accessible";
}

console.log(oldStyle); // "I am still accessible"
```

However, `var` is function-scoped:

```js
function example() {
  var secret = "Inside the function";
}

example();

console.log(secret); // Error
```

Prefer `const` and `let` in new code because their block scope is easier to reason about.

## Common Mistake: Using a Variable Outside Its Scope

```js
function calculateTotal() {
  const total = 10 + 5;
}

console.log(total); // Error
```

If code outside the function needs the value, **return it**:

```js
function calculateTotal() {
  const total = 10 + 5;
  return total;
}

const result = calculateTotal();

console.log(result); // 15
```

## Key Takeaway

A variable's scope controls where you can use it. `let` and `const` are limited to their enclosing block, while variables inside a function are unavailable outside that function. Inner code can access outer variables, but outer code cannot directly access variables declared inside an inner scope.

# Object Methods in JavaScript

An **object method** is a function stored as a property of an object. Methods describe actions that the object can perform.

```js
const user = {
  name: "Jadd",

  greet() {
    console.log("Hello!");
  },
};

user.greet(); // "Hello!"
```

Here, `greet` is a method of the `user` object.

## Properties vs. Methods

A **property** stores a value. A **method** stores a function:

```js
const student = {
  name: "Jadd", // Property

  study() {
    // Method
    console.log("Studying...");
  },
};

console.log(student.name); // "Jadd"
student.study(); // "Studying..."
```

Use parentheses `()` to call a method. Without them, you are referring to the function itself:

```js
student.study; // The function
student.study(); // Calls the function
```

## Using `this` Inside a Method

The `this` keyword lets a regular method access other properties on the object it was called through:

```js
const user = {
  name: "Jadd",

  greet() {
    console.log(`Hello, ${this.name}!`);
  },
};

user.greet(); // "Hello, Jadd!"
```

In the call `user.greet()`, `this` refers to `user`.

This is useful because the method can use the object's current values:

```js
user.name = "Alex";

user.greet(); // "Hello, Alex!"
```

## Methods Can Accept Arguments

Like other functions, methods can receive arguments:

```js
const calculator = {
  add(a, b) {
    return a + b;
  },
};

const result = calculator.add(5, 3);

console.log(result); // 8
```

`a` and `b` are parameters. The values `5` and `3` are the arguments supplied when the method is called.

## Methods Can Update an Object

A method can change its object's properties:

```js
const counter = {
  count: 0,

  increment() {
    this.count++;
  },
};

counter.increment();
counter.increment();

console.log(counter.count); // 2
```

Since `counter.count` changes, the method uses `this.count` to access and update it.

## Method Shorthand

There are two common ways to write a method:

```js
const user = {
  name: "Jadd",

  // Method shorthand
  greet() {
    return `Hello, ${this.name}!`;
  },

  // Function stored in a property
  sayGoodbye: function () {
    return `Goodbye, ${this.name}!`;
  },
};
```

Both methods can be called using dot notation:

```js
console.log(user.greet()); // "Hello, Jadd!"
console.log(user.sayGoodbye()); // "Goodbye, Jadd!"
```

Method shorthand is often easier to read.

## Be Careful with Arrow Functions and `this`

An arrow function does **not** get its own `this` from the object. If a method needs to use `this.name`, write it as a regular method:

```js
const user = {
  name: "Jadd",

  greet() {
    return `Hello, ${this.name}!`;
  },
};
```

Avoid writing that method as an arrow function:

```js
const user = {
  name: "Jadd",

  greet: () => {
    return `Hello, ${this.name}!`; // `this` does not refer to user
  },
};
```

Arrow functions can still be stored in objects when they do not need the object's `this`.

## Key Takeaway

A method is a function associated with an object. Call it with parentheses, pass it arguments when needed, and use `this` in a regular method to access or change the object's properties.

# JSON Handling in JavaScript

**JSON** stands for **JavaScript Object Notation**. It is a text format used to store and exchange data. You will often encounter JSON when working with APIs or saving data.

JSON looks similar to a JavaScript object, but **JSON is text**, while a JavaScript object is a value you can work with directly in your code.

## JavaScript Object vs. JSON

A JavaScript object:

```js
const user = {
  name: "Jadd",
  age: 20,
  isStudent: true,
};
```

The same data written as JSON:

```json
{
  "name": "Jadd",
  "age": 20,
  "isStudent": true
}
```

Notice that JSON requires **double quotes around property names and strings**. It also does not allow trailing commas or comments.

## Converting an Object to JSON

Use `JSON.stringify()` to convert a JavaScript value into a JSON string:

```js
const user = {
  name: "Jadd",
  age: 20,
};

const jsonText = JSON.stringify(user);

console.log(jsonText); // '{"name":"Jadd","age":20}'
console.log(typeof jsonText); // "string"
```

The result is a string. You cannot access its properties with `jsonText.name`.

## Converting JSON to an Object

Use `JSON.parse()` to convert JSON text into a JavaScript value:

```js
const jsonText = '{"name":"Jadd","age":20}';

const user = JSON.parse(jsonText);

console.log(user.name); // "Jadd"
console.log(user.age); // 20
```

`JSON.parse()` does not always return an object. For example, parsing `"[1,2,3]"` returns an array.

## The Conversion Process

```text
JavaScript value --JSON.stringify()--> JSON string
JSON string ------JSON.parse()-------> JavaScript value
```

Example:

```js
const original = { theme: "dark" };

const jsonText = JSON.stringify(original);
const restored = JSON.parse(jsonText);

console.log(restored.theme); // "dark"
```

## JSON Value Rules

JSON supports:

- Strings
- Numbers
- Booleans (`true` and `false`)
- `null`
- Arrays
- Objects

JSON does **not** support JavaScript values such as `undefined` or functions.

For example, `JSON.stringify()` leaves out object properties whose values are `undefined` or functions:

```js
const user = {
  name: "Jadd",
  nickname: undefined,
  greet() {
    console.log("Hello!");
  },
};

console.log(JSON.stringify(user)); // '{"name":"Jadd"}'
```

## Handling Invalid JSON

`JSON.parse()` throws an error if the text is not valid JSON:

```js
JSON.parse("{name: 'Jadd'}"); // Error
```

The property name and string value need double quotes:

```js
JSON.parse('{"name":"Jadd"}'); // Works
```

If the JSON comes from an outside source, use `try...catch` to handle errors:

```js
const input = '{"name":"Jadd"}';

try {
  const user = JSON.parse(input);
  console.log(user.name);
} catch (error) {
  console.log("The JSON is invalid.");
}
```

## Formatting JSON for Readability

By default, `JSON.stringify()` produces compact text. Pass `null` and a number of spaces to format it:

```js
const user = { name: "Jadd", age: 20 };

console.log(JSON.stringify(user, null, 2));
```

Output:

```json
{
  "name": "Jadd",
  "age": 20
}
```

## Example: Saving an Object in `localStorage`

A browser's `localStorage` stores strings. Convert an object to JSON before saving it:

```js
const settings = {
  theme: "dark",
  notifications: true,
};

localStorage.setItem("settings", JSON.stringify(settings));
```

When you read it back, parse the JSON string:

```js
const savedText = localStorage.getItem("settings");

if (savedText !== null) {
  const savedSettings = JSON.parse(savedText);
  console.log(savedSettings.theme); // "dark"
}
```

## Key Takeaway

Use `JSON.stringify()` to turn a JavaScript value into JSON text. Use `JSON.parse()` to turn valid JSON text back into a JavaScript value. Remember that JSON has stricter syntax than a JavaScript object.

# Dates and Time in JavaScript

JavaScript uses the `Date` object to represent a specific moment in time. You can use it to get the current time, work with timestamps, and display dates.

## Creating a Date

Create a `Date` object for the current moment:

```js
const now = new Date();

console.log(now);
```

You can also create one from a date and time:

```js
const birthday = new Date(2026, 8, 26);

console.log(birthday);
```

**Important:** Months in this form are numbered from `0` to `11`. `0` is January, so `8` is September.

The full numeric form is:

```js
new Date(year, monthIndex, day, hour, minute, second);
```

Numbers that create a date this way are interpreted in the computer's **local time zone**.

## Getting Parts of a Date

Use these methods to read parts of a date in local time:

```js
const date = new Date(2026, 8, 26, 14, 30);

console.log(date.getFullYear()); // 2026
console.log(date.getMonth()); // 8 (September)
console.log(date.getDate()); // 26 (day of the month)
console.log(date.getHours()); // 14
console.log(date.getMinutes()); // 30
```

`getDay()` gives the **day of the week**, not the day of the month:

```js
console.log(date.getDay()); // 0–6, where 0 is Sunday
```

Use `getDate()` for the day of the month.

## Changing Parts of a Date

You can update a `Date` object with setter methods:

```js
const date = new Date(2026, 8, 26);

date.setFullYear(2027);
date.setMonth(0); // January
date.setDate(15);

console.log(date);
```

These methods **change the original `Date` object**.

## Timestamps

JavaScript measures time as milliseconds since **January 1, 1970, 00:00:00 UTC**. This number is called a **timestamp**.

```js
const now = new Date();

console.log(now.getTime()); // Timestamp in milliseconds
console.log(Date.now()); // Current timestamp in milliseconds
```

You can also create a date from a timestamp:

```js
const timestamp = Date.now();
const date = new Date(timestamp);
```

## Calculating Time Differences

Subtracting two dates gives the difference in milliseconds:

```js
const start = new Date(2026, 8, 26, 9, 0);
const end = new Date(2026, 8, 26, 11, 30);

const differenceInMilliseconds = end - start;
const differenceInMinutes = differenceInMilliseconds / (1000 * 60);

console.log(differenceInMinutes); // 150
```

Common conversions are:

| Unit   |     Milliseconds |
| ------ | ---------------: |
| Second |           `1000` |
| Minute |      `1000 * 60` |
| Hour   | `1000 * 60 * 60` |

Be careful when treating a calendar day as exactly 24 hours: daylight saving time can make a local day shorter or longer.

## Formatting Dates for People

Use `Intl.DateTimeFormat` to format a date for display:

```js
const date = new Date(2026, 8, 26, 14, 30);

const formatter = new Intl.DateTimeFormat("en-CA", {
  dateStyle: "long",
  timeStyle: "short",
  timeZone: "America/Toronto",
});

console.log(formatter.format(date));
```

The `timeZone` option makes the intended display time zone explicit. Without it, formatting uses the environment's local time zone.

## ISO Strings and UTC

`.toISOString()` produces a standard date and time string in **UTC**:

```js
const date = new Date(2026, 8, 26, 14, 30);

console.log(date.toISOString());
```

The exact output depends on the local time zone used when `date` was created. An ISO string ends in `Z`, which means UTC.

You can create a date from a full ISO date and time:

```js
const date = new Date("2026-09-26T18:30:00Z");
```

This string describes a specific moment: 6:30 p.m. UTC.

## Watch Out for Date-Only Strings

JavaScript interprets a date-only string such as `"2026-09-26"` as midnight **UTC**:

```js
const date = new Date("2026-09-26");
```

Displaying that moment in a time zone behind UTC may show **September 25**. If you mean a local calendar date, construct it with numeric parts instead:

```js
const localDate = new Date(2026, 8, 26);
```

## Invalid Dates

An invalid input can create an `Invalid Date` object:

```js
const date = new Date("not a date");

console.log(Number.isNaN(date.getTime())); // true
```

Check a date before using it when its input may be invalid.

## Key Takeaway

Use `Date` to represent a moment in time and `Intl.DateTimeFormat` to display it. Remember that numeric month indexes start at `0`, timestamps use milliseconds, and time zones can change how a date appears.

# `setTimeout()` and `setInterval()` in JavaScript

`setTimeout()` and `setInterval()` are **timers**. They let you schedule a function to run later.

- `setTimeout()` runs a function **once** after a delay.
- `setInterval()` runs a function **repeatedly** at a specified interval.

Timer delays are measured in **milliseconds**. There are `1,000` milliseconds in one second.

## `setTimeout()`: Run Once Later

```js
setTimeout(() => {
  console.log("One second has passed.");
}, 1000);
```

The callback is scheduled to run after approximately one second.

The basic structure is:

```js
setTimeout(callback, delayInMilliseconds);
```

JavaScript does not pause while waiting for the timer:

```js
console.log("First");

setTimeout(() => {
  console.log("Third");
}, 1000);

console.log("Second");
```

Output:

```text
First
Second
Third
```

## Canceling a Timeout

`setTimeout()` returns a timer ID. Pass that ID to `clearTimeout()` to cancel the timer before it runs:

```js
const timeoutId = setTimeout(() => {
  console.log("This will not run.");
}, 2000);

clearTimeout(timeoutId);
```

This is useful when a scheduled action is no longer needed.

## `setInterval()`: Run Repeatedly

```js
const intervalId = setInterval(() => {
  console.log("Another second has passed.");
}, 1000);
```

The callback runs approximately once every second until you stop the interval.

The basic structure is:

```js
setInterval(callback, intervalInMilliseconds);
```

## Stopping an Interval

Use `clearInterval()` with the ID returned by `setInterval()`:

```js
let count = 0;

const intervalId = setInterval(() => {
  count++;
  console.log(count);

  if (count === 3) {
    clearInterval(intervalId);
  }
}, 1000);
```

Output over time:

```text
1
2
3
```

After printing `3`, the interval stops.

## Pass a Function, Not a Function Call

As explained in [Callback Functions](#pass-the-function-not-its-result), pass a function to a timer without calling it. If the callback needs an argument, wrap the call in an arrow function:

```js
setTimeout(() => {
  greet("Jadd");
}, 1000);
```

## The Timing Is Not Exact

A delay specifies the **earliest approximate time** a callback can run. If JavaScript is busy with other work, the callback may run later:

```js
setTimeout(() => {
  console.log("Runs after at least a short delay.");
}, 0);

console.log("Runs first.");
```

Output:

```text
Runs first.
Runs after at least a short delay.
```

A delay of `0` does not mean the callback runs immediately.

## When to Use Each Timer

| Timer           | Use it when...                             | How to stop it    |
| --------------- | ------------------------------------------ | ----------------- |
| `setTimeout()`  | An action should happen once after a delay | `clearTimeout()`  |
| `setInterval()` | An action should repeat over time          | `clearInterval()` |

For example, use `setTimeout()` to hide a notification after a few seconds. Use `setInterval()` to update a visible clock regularly.

## Key Takeaway

Use `setTimeout()` to schedule a callback once and `setInterval()` to repeat it. Store the returned timer ID when you may need to cancel it, and remember that timer delays are approximate.
