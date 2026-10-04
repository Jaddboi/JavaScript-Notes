# Table of Contents

- [Template Strings](#template-strings-in-javascript)
- [Arrow Functions](#arrow-functions-in-javascript)
- [Enhanced Object Literals](#enhanced-object-literals-in-javascript)
- [Default Parameters](#default-parameters-in-javascript)
- [The Spread Operator](#the-spread-operator-in-javascript)
- [The Rest Operator](#the-rest-operator-in-javascript)
- [Array Destructuring](#array-destructuring-in-javascript)
- [Object Destructuring](#object-destructuring-in-javascript)
- [Function Destructuring](#function-destructuring-in-javascript)
- [The Ternary Operator](#the-ternary-operator-in-javascript)
- [The `for...in` Loop](#the-forin-loop-in-javascript)
- [The `for...of` Loop](#the-forof-loop-in-javascript)
- [The `forEach()` Method](#the-foreach-method-in-javascript)
- [The `map()`, `filter()`, and `find()` Methods](#the-map-filter-and-find-array-methods)
- [The `every()` and `some()` Methods](#the-every-and-some-array-methods)
- [The `reduce()` Method](#the-reduce-array-method-in-javascript)
- [The `Map` Data Structure](#the-map-data-structure-in-javascript)
- [The `Set` Data Structure](#the-set-data-structure-in-javascript)
- [Symbols](#symbols-in-javascript)

# Template Strings in JavaScript

**Template strings**, also called **template literals**, are strings written with backticks (`` ` ``) instead of single or double quotes.

```js
const message = `Hello, world!`;
```

They make it easier to insert values into text and write strings across multiple lines.

## Inserting Variables with `${}`

Place a variable inside `${}` to include its value in a template string:

```js
const name = "Jadd";

const greeting = `Hello, ${name}!`;

console.log(greeting); // "Hello, Jadd!"
```

This is called **interpolation**.

With regular strings, you would need to join the pieces using `+`:

```js
const greeting = "Hello, " + name + "!";
```

## Inserting Expressions

You can put expressions inside `${}`, not just variable names:

```js
const price = 10;
const quantity = 3;

console.log(`Total: $${price * quantity}`); // "Total: $30"
```

Here, `${price * quantity}` evaluates to `30`. The `$` immediately before it is ordinary text.

You can also call a function:

```js
function getName() {
  return "Jadd";
}

console.log(`Welcome, ${getName()}!`); // "Welcome, Jadd!"
```

## Writing Multiple Lines

Template strings can span multiple lines:

```js
const message = `Hello, Jadd!
Welcome to JavaScript.`;

console.log(message);
```

Output:

```text
Hello, Jadd!
Welcome to JavaScript.
```

The line break becomes part of the string. Spaces added at the start of a line are preserved too.

## Using Quotes Inside a Template String

Because backticks mark the beginning and end, you can use single and double quotes inside without escaping them:

```js
const message = `She said, "It's working!"`;

console.log(message); // She said, "It's working!"
```

To include an actual backtick, put a backslash before it:

```js
const message = `Use \`backticks\` for template strings.`;

console.log(message); // Use `backticks` for template strings.
```

## Showing `${}` as Text

Normally, `${}` inserts a value. Put a backslash before the `$` if you want to display the characters literally:

```js
const example = `Write \${name} to insert a variable.`;

console.log(example); // Write ${name} to insert a variable.
```

## Key Takeaway

Use backticks to create a template string. Insert variables or expressions with `${}`, and write across multiple lines when needed.

# Arrow Functions in JavaScript

An **arrow function** is a shorter way to write a function expression. It uses the arrow `=>` between its parameters and its body.

```js
const greet = (name) => {
  return `Hello, ${name}!`;
};

console.log(greet("Jadd")); // "Hello, Jadd!"
```

## Comparing Function Syntax

A regular function expression:

```js
const add = function (a, b) {
  return a + b;
};
```

The same function written as an arrow function:

```js
const add = (a, b) => {
  return a + b;
};
```

Both examples return the sum of `a` and `b`.

## Parameters

With **one parameter**, the parentheses are optional:

```js
const double = (number) => number * 2;

console.log(double(4)); // 8
```

With **zero or multiple parameters**, parentheses are required:

```js
const sayHello = () => "Hello!";
const add = (a, b) => a + b;
```

Arrow functions can also use default parameters, which are covered in [Default Parameters](#default-parameters-in-javascript).

## Implicit Return

If the function body is a **single expression**, you can omit the curly braces and the `return` keyword:

```js
const square = (number) => number * number;

console.log(square(5)); // 25
```

This is called an **implicit return**. JavaScript returns the expression's result automatically.

These two functions do the same thing:

```js
const doubleA = (number) => {
  return number * 2;
};

const doubleB = (number) => number * 2;
```

If you use curly braces for the function body, you must write `return` when you want to return a value:

```js
const double = (number) => {
  number * 2;
};

console.log(double(5)); // undefined
```

## Multiple Statements

Use curly braces when the function needs more than one statement:

```js
const calculateTotal = (price, quantity) => {
  const subtotal = price * quantity;
  const tax = subtotal * 0.13;

  return subtotal + tax;
};

console.log(calculateTotal(10, 2)); // 22.6
```

## Returning an Object

To implicitly return an object, wrap the object in parentheses:

```js
const createUser = (name) => ({ name });

console.log(createUser("Jadd")); // { name: "Jadd" }
```

Without the parentheses, JavaScript treats the curly braces as the function body rather than an object:

```js
const createUser = (name) => {
  return { name };
};
```

The `{ name }` syntax is property shorthand, which is explained in the next chapter.

## Arrow Functions and `this`

Arrow functions handle `this` differently from regular functions. An arrow function does **not** create its own `this`; it uses `this` from the surrounding code.

Because of this, use a regular method when you want `this` to refer to the object calling it:

```js
const user = {
  name: "Jadd",

  greet() {
    return `Hello, ${this.name}!`;
  },
};

console.log(user.greet()); // "Hello, Jadd!"
```

Avoid using an arrow function for that method:

```js
const user = {
  name: "Jadd",

  greet: () => `Hello, ${this.name}!`,
};

// Here, `this` does not refer to user.
```

The same difference matters with some event handlers and other functions that provide their own `this`.

## Declaration Timing

An arrow function assigned to a `const` variable must be called **after** its declaration:

```js
greet("Jadd"); // Error: greet is not initialized yet

const greet = (name) => `Hello, ${name}!`;
```

This differs from a function declaration, which can generally be called earlier in the same scope:

```js
greet("Jadd"); // Works

function greet(name) {
  console.log(`Hello, ${name}!`);
}
```

## Key Takeaway

Arrow functions provide concise syntax for writing functions, especially short callbacks. Use an implicit return for a single expression, add `return` when using a block body, and remember that arrow functions do not have their own `this`.

# Enhanced Object Literals in JavaScript

An **object literal** is an object you create with curly braces `{}`:

```js
const user = {
  name: "Jadd",
  age: 20,
};
```

JavaScript has several shortcuts that make object literals easier to write: **property shorthand**, **method shorthand**, and **computed property names**.

## Property Shorthand

When a variable has the same name as the property you want to create, you can write its name once:

```js
const name = "Jadd";
const age = 20;

const user = {
  name,
  age,
};

console.log(user); // { name: "Jadd", age: 20 }
```

This is shorthand for:

```js
const user = {
  name: name,
  age: age,
};
```

The property's key is the variable's name, and its value comes from the variable.

## Method Shorthand

You can define a method without writing `function`:

```js
const user = {
  name: "Jadd",

  greet() {
    return `Hello, ${this.name}!`;
  },
};

console.log(user.greet()); // "Hello, Jadd!"
```

This is shorter than writing:

```js
const user = {
  name: "Jadd",

  greet: function () {
    return `Hello, ${this.name}!`;
  },
};
```

## Computed Property Names

Use square brackets `[]` when you want to calculate a property name while creating an object:

```js
const field = "email";

const user = {
  name: "Jadd",
  [field]: "jadd@example.com",
};

console.log(user.email); // "jadd@example.com"
```

Without the brackets, the key would literally be `"field"`:

```js
const field = "email";

const user = {
  field: "jadd@example.com",
};

console.log(user.field); // "jadd@example.com"
console.log(user.email); // undefined
```

You can also build a property name from an expression:

```js
const number = 1;

const scores = {
  [`round${number}`]: 95,
};

console.log(scores.round1); // 95
```

## Key Takeaway

Use **property shorthand** when a key and variable have the same name, **method shorthand** to define methods, and **computed property names** when a key comes from a variable or expression. The [Spread Operator](#spreading-an-object) section explains how to copy properties into a new object.

# Default Parameters in JavaScript

A **default parameter** gives a function parameter a value to use when no argument is provided.

```js
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

console.log(greet("Jadd")); // "Hello, Jadd!"
console.log(greet()); // "Hello, Guest!"
```

In this example, `"Guest"` is the default value for `name`.

## Why Use Default Parameters?

Without a default, a missing argument has the value `undefined`, which can produce an unwanted result:

```js
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet()); // "Hello, undefined!"
```

## Multiple Default Parameters

A function can have more than one default parameter:

```js
function introduce(name = "Guest", city = "Toronto") {
  return `${name} lives in ${city}.`;
}

console.log(introduce());
// "Guest lives in Toronto."

console.log(introduce("Jadd"));
// "Jadd lives in Toronto."

console.log(introduce("Jadd", "Mississauga"));
// "Jadd lives in Mississauga."
```

You can mix parameters with and without defaults:

```js
function calculateTotal(price, taxRate = 0.13) {
  return price + price * taxRate;
}

console.log(calculateTotal(100)); // 113
console.log(calculateTotal(100, 0.05)); // 105
```

## When Does a Default Apply?

A default is used when the argument is **missing** or is explicitly `undefined`:

```js
console.log(greet()); // "Hello, Guest!"
console.log(greet(undefined)); // "Hello, Guest!"
```

Other values do **not** trigger the default:

```js
console.log(greet("")); // "Hello, !"
console.log(greet(null)); // "Hello, null!"
console.log(greet(false)); // "Hello, false!"
```

A default parameter does not mean “use this value whenever the argument is falsy.”

## Skipping a Parameter

If you want to use the default for one parameter while providing a later argument, pass `undefined` in its place:

```js
console.log(introduce(undefined, "Ottawa"));
// "Guest lives in Ottawa."
```

Arguments still match parameters by position.

## Defaults Can Use Expressions

A default value can be calculated:

```js
function createUsername(name, number = Math.floor(Math.random() * 1000)) {
  return `${name}${number}`;
}

console.log(createUsername("Jadd", 42)); // "Jadd42"
```

The default expression runs only when that parameter needs its default.

A default can also use an **earlier** parameter:

```js
function calculateTotal(price, tax = price * 0.13) {
  return price + tax;
}

console.log(calculateTotal(100)); // 113
```

## Key Takeaway

Set a default parameter with `parameter = value` in the function definition. JavaScript uses that value only when the corresponding argument is omitted or `undefined`.

# The Spread Operator in JavaScript

The **spread syntax** uses three dots: `...`. It expands values from an array, string, or object in places where JavaScript expects individual values or properties.

What `...` does depends on where you use it.

## Spreading and Combining Arrays

Use `...` to put the elements of one array into a new array:

```js
const first = [1, 2, 3];
const combined = [...first, 4, 5];

console.log(combined); // [1, 2, 3, 4, 5]
```

Without `...`, the original array would become one nested element:

```js
const first = [1, 2, 3];

console.log([first, 4, 5]); // [[1, 2, 3], 4, 5]
```

You can spread multiple arrays into one:

```js
const morningTasks = ["Study", "Exercise"];
const eveningTasks = ["Cook", "Read"];

const allTasks = [...morningTasks, ...eveningTasks];

console.log(allTasks);
// ["Study", "Exercise", "Cook", "Read"]
```

You can insert values anywhere in the new array:

```js
const numbers = [2, 3];

const updated = [1, ...numbers, 4];

console.log(updated); // [1, 2, 3, 4]
```

## Copying an Array

Spreading an array creates a **new array**:

```js
const original = ["red", "blue"];
const copy = [...original];

copy.push("green");

console.log(original); // ["red", "blue"]
console.log(copy); // ["red", "blue", "green"]
```

This is a **shallow copy**. If an element is an object, the original and new arrays still refer to that same inner object:

```js
const original = [{ name: "Jadd" }];
const copy = [...original];

copy[0].name = "Alex";

console.log(original[0].name); // "Alex"
```

## Spreading Arguments into a Function Call

Spread an array when a function expects separate arguments:

```js
const numbers = [4, 8, 2];

console.log(Math.max(...numbers)); // 8
```

This is like calling `Math.max(4, 8, 2)`.

## Spreading a String

A string can be spread into individual characters:

```js
const letters = [..."hello"];

console.log(letters); // ["h", "e", "l", "l", "o"]
```

## Spreading an Object

Object spread copies properties into a new object:

```js
const user = {
  name: "Jadd",
  age: 20,
};

const updatedUser = {
  ...user,
  city: "Toronto",
};

console.log(updatedUser);
// { name: "Jadd", age: 20, city: "Toronto" }
```

You can use it to create an updated copy:

```js
const user = {
  name: "Jadd",
  age: 20,
};

const updatedUser = {
  ...user,
  age: 21,
};

console.log(updatedUser); // { name: "Jadd", age: 21 }
console.log(user); // { name: "Jadd", age: 20 }
```

When keys repeat, the **last value wins**:

```js
const user = { name: "Jadd" };

const first = { ...user, name: "Alex" };
const second = { name: "Alex", ...user };

console.log(first.name); // "Alex"
console.log(second.name); // "Jadd"
```

Like array spread, object spread creates a **shallow copy**. Nested objects are still shared unless you copy those separately.

## Key Takeaway

Use spread syntax to expand arrays or strings into individual values, or to copy properties into a new object. Spread copies are **shallow**, so nested objects can still be shared. The next section compares spread with rest syntax.

# The Rest Operator in JavaScript

The **rest syntax** uses three dots: `...`. It collects multiple values into one array or object.

You will most often see rest syntax in **function parameters** and **destructuring**.

## Rest Parameters in Functions

A rest parameter collects any remaining arguments into an array:

```js
function listItems(...items) {
  console.log(items);
}

listItems("apple", "banana", "orange");
// ["apple", "banana", "orange"]
```

Here, `...items` collects all three arguments into the `items` array.

You can then use array properties and methods:

```js
function countItems(...items) {
  return items.length;
}

console.log(countItems("apple", "banana")); // 2
console.log(countItems()); // 0
```

When no arguments are collected, the rest parameter is an empty array.

## Combining Regular and Rest Parameters

You can put regular parameters before a rest parameter:

```js
function introduce(name, ...hobbies) {
  console.log(name);
  console.log(hobbies);
}

introduce("Jadd", "gaming", "coding", "reading");
```

Output:

```text
Jadd
["gaming", "coding", "reading"]
```

The first argument goes into `name`. All remaining arguments go into the `hobbies` array.

## The Rest Parameter Must Come Last

A rest parameter must be the **last parameter** in a function:

```js
function example(first, ...remaining) {
  console.log(first, remaining);
}
```

This is invalid:

```js
function example(...remaining, last) {
  // Syntax error
}
```

A function can have only one rest parameter.

## Example: Adding Any Number of Values

```js
function sum(...numbers) {
  let total = 0;

  for (const number of numbers) {
    total += number;
  }

  return total;
}

console.log(sum(2, 3)); // 5
console.log(sum(2, 3, 4, 5)); // 14
console.log(sum()); // 0
```

The function works with any number of arguments because they are collected into `numbers`.

## Rest vs. Spread

Rest and spread use the same `...` syntax, but do opposite jobs:

| Syntax | What it does                                                             |
| ------ | ------------------------------------------------------------------------ |
| Rest   | Collects multiple values into one array or object                        |
| Spread | Expands an array, string, or object into individual values or properties |

```js
function showNumbers(...numbers) {
  // Rest collects the arguments into an array.
  console.log(numbers);
}

const values = [1, 2, 3];

showNumbers(...values);
// Spread expands the array when calling the function.
```

In this example, spread turns `values` into three arguments. Rest then collects those arguments into the `numbers` array.

## Key Takeaway

Use rest syntax to collect remaining function arguments or values during destructuring. In a function, the rest parameter becomes an array and must be the **last parameter**. The following destructuring sections show rest with arrays and objects.

# Array Destructuring in JavaScript

**Array destructuring** lets you take values from an array and assign them to variables in one statement.

```js
const colors = ["red", "blue", "green"];

const [first, second, third] = colors;

console.log(first); // "red"
console.log(second); // "blue"
console.log(third); // "green"
```

The variables receive values based on their **positions** in the array.

## Without Destructuring

You could assign each value using its index:

```js
const colors = ["red", "blue", "green"];

const first = colors[0];
const second = colors[1];
```

Destructuring is a shorter way to do the same thing:

```js
const [first, second] = colors;
```

## You Do Not Need Every Value

You can take only the values you need:

```js
const scores = [85, 92, 78];

const [firstScore] = scores;

console.log(firstScore); // 85
```

To skip a value, leave an empty position between commas:

```js
const scores = [85, 92, 78];

const [firstScore, , thirdScore] = scores;

console.log(firstScore); // 85
console.log(thirdScore); // 78
```

## Missing Values and Defaults

If there are more variables than array elements, the extra variables receive `undefined`:

```js
const colors = ["red"];

const [first, second] = colors;

console.log(first); // "red"
console.log(second); // undefined
```

You can provide a default value:

```js
const colors = ["red"];

const [first, second = "blue"] = colors;

console.log(second); // "blue"
```

The default is used when the value is **missing or `undefined`**, not when it is `null` or `false`:

```js
const values = [null, false];

const [first = "default", second = true] = values;

console.log(first); // null
console.log(second); // false
```

## Collecting Remaining Values with Rest

Use `...` to collect the remaining elements into a new array:

```js
const numbers = [10, 20, 30, 40];

const [first, ...remaining] = numbers;

console.log(first); // 10
console.log(remaining); // [20, 30, 40]
```

The rest element must come last:

```js
const [first, ...remaining] = numbers; // Works
```

```js
const [...remaining, last] = numbers; // Syntax error
```

## Nested Array Destructuring

You can destructure an array inside another array:

```js
const student = ["Jadd", [85, 92]];

const [name, [firstGrade, secondGrade]] = student;

console.log(name); // "Jadd"
console.log(firstGrade); // 85
console.log(secondGrade); // 92
```

The destructuring pattern follows the shape of the array.

## Swapping Variables

Destructuring lets you swap two variables without creating a temporary variable:

```js
let first = "apple";
let second = "banana";

[first, second] = [second, first];

console.log(first); // "banana"
console.log(second); // "apple"
```

The variables already exist, so you do not write `const` or `let` again.

## Destructuring Does Not Change the Original Array

```js
const colors = ["red", "blue"];

const [first, second] = colors;

console.log(colors); // ["red", "blue"]
```

Destructuring reads values from the array; it does not remove them.

## Key Takeaway

Array destructuring assigns array elements to variables by **position**. You can skip elements, provide defaults, collect the rest, and unpack nested arrays.

# Object Destructuring in JavaScript

**Object destructuring** lets you take properties from an object and assign their values to variables in one statement.

```js
const student = {
  name: "Jadd",
  age: 20,
  major: "Computer Science",
};

const { name, age } = student;

console.log(name); // "Jadd"
console.log(age); // 20
```

JavaScript looks for properties named `name` and `age`, then creates variables with those names.
Unlike array destructuring, object destructuring matches **property names**, not positions. The order does not matter, and you can take only the properties you need.

## Without Destructuring

You could access and assign each property separately:

```js
const student = {
  name: "Jadd",
  age: 20,
};

const name = student.name;
const age = student.age;
```

Destructuring combines those assignments:

```js
const { name, age } = student;
```

## Renaming Variables

Use `propertyName: variableName` when you want the new variable to have a different name:

```js
const student = {
  name: "Jadd",
  age: 20,
};

const { name: studentName, age: studentAge } = student;

console.log(studentName); // "Jadd"
console.log(studentAge); // 20
```

In `name: studentName`, `name` is the **object property**, and `studentName` is the **new variable**.

This is useful when a variable with the original name already exists:

```js
const name = "Alex";
const student = { name: "Jadd" };

const { name: studentName } = student;

console.log(name); // "Alex"
console.log(studentName); // "Jadd"
```

## Missing Properties and Defaults

If a property does not exist, its variable receives `undefined`:

```js
const student = { name: "Jadd" };

const { name, age } = student;

console.log(name); // "Jadd"
console.log(age); // undefined
```

You can provide a default value:

```js
const student = { name: "Jadd" };

const { name, age = 18 } = student;

console.log(age); // 18
```

The default is used when the property is **missing or `undefined`**. It is not used for values such as `null`, `0`, or `false`.

You can rename a property and provide a default at the same time:

```js
const student = { name: "Jadd" };

const { age: studentAge = 18 } = student;

console.log(studentAge); // 18
```

## Nested Object Destructuring

You can take properties from an object inside another object:

```js
const student = {
  name: "Jadd",
  address: {
    city: "Toronto",
    country: "Canada",
  },
};

const {
  address: { city, country },
} = student;

console.log(city); // "Toronto"
console.log(country); // "Canada"
```

This creates variables named `city` and `country`. It does **not** create a variable named `address`.

If the `address` property might be missing, this pattern needs additional handling; otherwise, JavaScript will throw an error when it tries to destructure it.

## Collecting Remaining Properties with Rest

Use `...` to collect the properties you did not select:

```js
const student = {
  name: "Jadd",
  age: 20,
  major: "Computer Science",
};

const { name, ...details } = student;

console.log(name); // "Jadd"
console.log(details); // { age: 20, major: "Computer Science" }
```

The rest object is a **shallow copy**. Nested objects within it can still be shared with the original object.

## Assigning to Existing Variables

When declaring **new** variables, use `const` or `let`:

```js
const { name } = student;
```

To assign to a variable that already exists, put the destructuring assignment in parentheses:

```js
let name = "Alex";
const student = { name: "Jadd" };

({ name } = student);

console.log(name); // "Jadd"
```

The parentheses prevent JavaScript from treating `{ name }` as a block of code.

## Key Takeaway

Object destructuring assigns properties to variables by **property name**, regardless of their order. You can rename variables, set defaults, unpack nested objects, and collect the remaining properties with rest syntax.

# Function Destructuring in JavaScript

**Function destructuring** means unpacking values from an array or object when a function receives an argument or returns a result.

It can make functions easier to read when they work with several related values.

## Destructuring an Object Parameter

Suppose a function needs a student's name and major:

```js
function introduce(student) {
  return `${student.name} studies ${student.major}.`;
}
```

You can destructure the object in the parameter list:

```js
function introduce({ name, major }) {
  return `${name} studies ${major}.`;
}

const student = {
  name: "Jadd",
  major: "Computer Science",
  age: 20,
};

console.log(introduce(student));
// "Jadd studies Computer Science."
```

The function takes **one object argument**. It creates local variables for the `name` and `major` properties and ignores properties it does not need. Renaming, defaults, and rest syntax work here just as they do in any object destructuring pattern.

## Destructuring an Array Parameter

You can also unpack an array in a parameter list:

```js
function formatCoordinates([latitude, longitude]) {
  return `${latitude}, ${longitude}`;
}

const coordinates = [43.6, -79.4];

console.log(formatCoordinates(coordinates)); // "43.6, -79.4"
```

Array destructuring uses **position**: the first element becomes `latitude`, and the second becomes `longitude`.

## Defaults and Omitted Arguments

Defaults inside a destructuring pattern handle missing properties. To also handle a completely omitted argument, default the entire parameter to an empty object:

```js
function greet({ name = "Guest" } = {}) {
  return `Hello, ${name}!`;
}

console.log(greet()); // "Hello, Guest!"
console.log(greet({ name: "Jadd" })); // "Hello, Jadd!"
```

Here, `= {}` supplies an empty object when the argument is omitted. Then `name = "Guest"` supplies the missing property's value. Without `= {}`, calling `greet()` would try to destructure `undefined` and throw an error.

## Returning an Array and Destructuring It

A function can return an array that the caller unpacks:

```js
function getCoordinates() {
  return [43.6, -79.4];
}

const [latitude, longitude] = getCoordinates();

console.log(latitude); // 43.6
console.log(longitude); // -79.4
```

The caller must know what each **position** represents.

## Returning an Object and Destructuring It

A function can return an object with named properties:

```js
function calculateRectangle(width, height) {
  return {
    area: width * height,
    perimeter: 2 * (width + height),
  };
}

const { area, perimeter } = calculateRectangle(5, 3);

console.log(area); // 15
console.log(perimeter); // 16
```

Because objects use property names, the caller can take only the values it needs:

```js
const { area } = calculateRectangle(5, 3);

console.log(area); // 15
```

## Passing Several Options as One Object

Object destructuring is useful when a function has several optional settings:

```js
function createMessage({ name, greeting = "Hello", punctuation = "!" } = {}) {
  return `${greeting}, ${name}${punctuation}`;
}

console.log(createMessage({ name: "Jadd" }));
// "Hello, Jadd!"

console.log(
  createMessage({
    name: "Jadd",
    punctuation: ".",
    greeting: "Welcome",
  })
);
// "Welcome, Jadd."
```

The order of properties in the argument does not matter. The function matches them by name.

## Key Takeaway

Destructure **parameters** to unpack the parts of an argument a function needs. Destructure a **return value** to assign its parts to variables. Arrays match by position; objects match by property name.

# The Ternary Operator in JavaScript

The **ternary operator** chooses between two values based on a condition. It is a shorter way to write a simple `if...else` decision.

It is called _ternary_ because it has three parts:

```js
condition ? valueIfTrue : valueIfFalse;
```

JavaScript evaluates the condition, then uses one of the two values.

## Basic Example

```js
const age = 20;

const message = age >= 18 ? "Adult" : "Under 18";

console.log(message); // "Adult"
```

Because `age >= 18` is `true`, `message` receives `"Adult"`.

If `age` were `16`, the condition would be false and `message` would receive `"Under 18"`.

## Comparing It with `if...else`

These examples produce the same result:

```js
const age = 20;
let message;

if (age >= 18) {
  message = "Adult";
} else {
  message = "Under 18";
}
```

This is equivalent to the ternary in the basic example above. The ternary works well when each branch provides a simple value.

## Using the Result Directly

A ternary produces a value, so you do not have to assign it to a variable:

```js
const isLoggedIn = true;

console.log(isLoggedIn ? "Welcome back!" : "Please log in.");
// "Welcome back!"
```

You can also use it inside a template string:

```js
const score = 85;

console.log(`Result: ${score >= 50 ? "Pass" : "Fail"}`);
// "Result: Pass"
```

## Using Functions in Each Branch

Each branch can contain an expression, including a function call:

```js
function showWelcome() {
  console.log("Welcome back!");
}

function showLoginPrompt() {
  console.log("Please log in.");
}

const isLoggedIn = true;

isLoggedIn ? showWelcome() : showLoginPrompt();
```

Only the function in the selected branch is called. In this example, `showWelcome()` runs.

For actions like these, an `if...else` statement is often clearer:

```js
if (isLoggedIn) {
  showWelcome();
} else {
  showLoginPrompt();
}
```

## Conditions Use Truthy and Falsy Values

The condition does not have to be an actual boolean:

```js
const username = "";

const displayName = username ? username : "Guest";

console.log(displayName); // "Guest"
```

An empty string is **falsy**, so the second value is chosen. A nonempty string is truthy.

## Nested Ternaries

You can put one ternary inside another:

```js
const score = 85;

const grade =
  score >= 90 ? "A" : score >= 80 ? "B" : score >= 70 ? "C" : "Below C";

console.log(grade); // "B"
```

This works, but several branches can be difficult to read. For longer decisions, use `if...else if...else`:

```js
let grade;

if (score >= 90) {
  grade = "A";
} else if (score >= 80) {
  grade = "B";
} else if (score >= 70) {
  grade = "C";
} else {
  grade = "Below C";
}
```

## Common Mistake: Forgetting a Branch

A ternary needs **both** the true and false expressions:

```js
const message = age >= 18 ? "Adult" : "Under 18";
```

This is incomplete:

```js
const message = age >= 18 ? "Adult"; // Syntax error
```

If you only want code to run when a condition is true, use an `if` statement.

## Key Takeaway

Use `condition ? valueIfTrue : valueIfFalse` for a short choice between two values. The ternary operator produces a value, making it useful for assignments and expressions. Use `if...else` when the decision involves several steps or becomes hard to read.

# The `for...in` Loop in JavaScript

A **`for...in` loop** goes through the enumerable property names (keys) of an object.

On each repetition, `key` holds a **property name**, not the property's value.

## Accessing Each Value

Use bracket notation to access the value associated with a key:

```js
const student = {
  name: "Jadd",
  age: 20,
  major: "Computer Science",
};

for (const key in student) {
  console.log(`${key}: ${student[key]}`);
}
```

Output:

```text
name: Jadd
age: 20
major: Computer Science
```

You need `student[key]` because `key` is a variable containing a property name.

```js
student[key]; // Looks up the property whose name is stored in key
student.key; // Looks for a property literally named "key"
```

## Inherited Properties

A `for...in` loop can also visit enumerable properties that an object **inherits**. If you only want properties belonging directly to the object, check with `Object.hasOwn()`:

```js
const student = {
  name: "Jadd",
  age: 20,
};

for (const key in student) {
  if (Object.hasOwn(student, key)) {
    console.log(`${key}: ${student[key]}`);
  }
}
```

For a simple object literal like this one, you will usually see its own properties. The check becomes more useful when objects inherit properties.

## Choosing `for...in` or `for...of`

`for...in` can loop through an array's keys:

```js
const colors = ["red", "blue"];

for (const index in colors) {
  console.log(index);
}
```

Output:

```text
0
1
```

However, those indexes are **strings**, and `for...in` can also visit other enumerable properties added to the array. If you want the array's **values**, use `for...of` instead:

```js
for (const color of colors) {
  console.log(color);
}
```

Output:

```text
red
blue
```

| Loop       | Gives you             | Common use                                       |
| ---------- | --------------------- | ------------------------------------------------ |
| `for...in` | Property names (keys) | Inspecting an object's properties                |
| `for...of` | Values                | Looping through arrays and other iterable values |

A regular object is not directly iterable with `for...of`. To loop through its own keys, you can combine `Object.keys()` with `for...of`:

```js
const student = { name: "Jadd", age: 20 };

for (const key of Object.keys(student)) {
  console.log(`${key}: ${student[key]}`);
}
```

## Key Takeaway

Use `for...in` to loop through an object's property **names**, and use bracket notation to access their values. Remember that it can include inherited enumerable properties, so check `Object.hasOwn()` when you only want the object's own properties.

# The `for...of` Loop in JavaScript

A **`for...of` loop** goes through the values of an **iterable**. Arrays and strings are common iterables.

```js
const colors = ["red", "blue", "green"];

for (const color of colors) {
  console.log(color);
}
```

Output:

```text
red
blue
green
```

On each repetition, `color` receives the next value from the array.

You can use `const` because each repetition gets its own loop variable. Use `let` if you need to reassign that variable within a repetition.

## Looping Through a String

A string is iterable, so `for...of` can go through its characters:

```js
const word = "Hi";

for (const character of word) {
  console.log(character);
}
```

Output:

```text
H
i
```

## Getting Both Index and Value

If you need an array element's index too, use `.entries()` with destructuring:

```js
const colors = ["red", "blue", "green"];

for (const [index, color] of colors.entries()) {
  console.log(`${index}: ${color}`);
}
```

Output:

```text
0: red
1: blue
2: green
```

Each value from `colors.entries()` contains an index and its corresponding element.

## Using `break` and `continue`

Use `break` to stop the loop early:

```js
const numbers = [2, 4, 7, 8];

for (const number of numbers) {
  if (number === 7) {
    break;
  }

  console.log(number);
}
```

Output:

```text
2
4
```

Use `continue` to skip the rest of the current repetition:

```js
const numbers = [1, 2, 3, 4];

for (const number of numbers) {
  if (number % 2 === 0) {
    continue;
  }

  console.log(number);
}
```

Output:

```text
1
3
```

## Can You Use `for...of` with an Object?

A regular object is **not directly iterable**:

```js
const student = { name: "Jadd", age: 20 };

for (const value of student) {
  console.log(value);
}
// TypeError
```

Use `Object.values()` to loop through its values:

```js
for (const value of Object.values(student)) {
  console.log(value);
}
```

Or use `Object.entries()` to get both keys and values:

```js
for (const [key, value] of Object.entries(student)) {
  console.log(`${key}: ${value}`);
}
```

Output:

```text
name: Jadd
age: 20
```

## Key Takeaway

Use `for...of` when you want to work directly with the **values** in an array, string, or other iterable. If you also need array indexes, use `.entries()`.

# The `forEach()` Method in JavaScript

`forEach()` is an array method that runs a **callback function once for each element** in an array.

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

The array calls the callback and passes each element to it. You do not call the callback yourself.

## Getting the Index

The callback can also receive the element's **index** as its second parameter:

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

The callback parameters are supplied in this order:

```js
array.forEach((element, index, array) => {
  // ...
});
```

The third parameter is the array being visited. You can leave out any parameters you do not need.

## Using a Named Callback

You can define a function separately and pass it to `forEach()`:

```js
function printColor(color) {
  console.log(color);
}

const colors = ["red", "blue", "green"];

colors.forEach(printColor);
```

Pass `printColor` **without parentheses**. `forEach()` will call it for each element.

## Example: Calculating a Total

You can use `forEach()` to perform an action for every value:

```js
const prices = [10, 15, 5];
let total = 0;

prices.forEach((price) => {
  total += price;
});

console.log(total); // 30
```

## `forEach()` Does Not Create a New Array

`forEach()` returns `undefined`. A value returned from its callback is ignored:

```js
const numbers = [1, 2, 3];

const result = numbers.forEach((number) => {
  return number * 2;
});

console.log(result); // undefined
```

If you want to create a **new array** from the callback's returned values, use `.map()` instead. The next section explains it in detail.

## You Cannot Stop `forEach()` with `break`

`break` and `continue` cannot be used to control a `forEach()` callback. A `return` only ends the **current callback call**; it does not stop the whole loop:

```js
const numbers = [1, 2, 3];

numbers.forEach((number) => {
  if (number === 2) {
    return; // Skips the rest of this callback call
  }

  console.log(number);
});
```

Output:

```text
1
3
```

If you need to stop looping early with `break`, use `for...of`:

```js
for (const number of numbers) {
  if (number === 2) {
    break;
  }

  console.log(number);
}
```

Output:

```text
1
```

## Key Takeaway

Use `forEach()` to run a callback once for each array element. It can provide the element, its index, and the array, but it does not produce a new array or support `break`.

# The `map()`, `filter()`, and `find()` Array Methods

`map()`, `filter()`, and `find()` use callback functions to work with arrays. Each method answers a different question:

| Method     | Question                            | Result                            |
| ---------- | ----------------------------------- | --------------------------------- |
| `map()`    | What should each element become?    | A new array of transformed values |
| `filter()` | Which elements should be kept?      | A new array of matching elements  |
| `find()`   | What is the first matching element? | One element, or `undefined`       |

Each callback can receive the current element, its index, and the original array. When an arrow callback uses curly braces, it must explicitly `return` the value that controls the method.

## `map()`: Transform Every Element

`map()` calls a callback for each element and puts each **returned value** into a new array:

```js
const numbers = [1, 2, 3];

const doubled = numbers.map((number) => {
  return number * 2;
});

console.log(doubled); // [2, 4, 6]
console.log(numbers); // [1, 2, 3]
```

The new array has the same number of elements as the original array.

For a short callback, you can use an arrow function's implicit return:

```js
const doubled = numbers.map((number) => number * 2);
```

Another example:

```js
const names = ["jadd", "alex", "sam"];

const uppercaseNames = names.map((name) => name.toUpperCase());

console.log(uppercaseNames); // ["JADD", "ALEX", "SAM"]
```

## `filter()`: Keep Matching Elements

`filter()` keeps an element when its callback returns a **truthy** value. A boolean condition is the clearest way to use it:

```js
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter((number) => {
  return number % 2 === 0;
});

console.log(evenNumbers); // [2, 4, 6]
console.log(numbers); // [1, 2, 3, 4, 5, 6]
```

The new array may have fewer elements than the original. If nothing matches, `filter()` returns an empty array:

```js
const largeNumbers = numbers.filter((number) => number > 10);

console.log(largeNumbers); // []
```

## `find()`: Get the First Match

`find()` returns the **first element** whose callback returns a truthy value:

```js
const numbers = [3, 7, 12, 18];

const firstEven = numbers.find((number) => number % 2 === 0);

console.log(firstEven); // 12
```

It stops searching once it finds a match. If no element matches, it returns `undefined`:

```js
const result = numbers.find((number) => number > 100);

console.log(result); // undefined
```

## Using Them with Objects

These methods are especially useful for arrays of objects:

```js
const students = [
  { name: "Jadd", grade: 85 },
  { name: "Alex", grade: 92 },
  { name: "Sam", grade: 68 },
];
```

Use `map()` to get an array of names:

```js
const names = students.map((student) => student.name);

console.log(names); // ["Jadd", "Alex", "Sam"]
```

Use `filter()` to get **all** students with a grade of at least `80`:

```js
const passingStudents = students.filter((student) => student.grade >= 80);

console.log(passingStudents);
// [
//   { name: "Jadd", grade: 85 },
//   { name: "Alex", grade: 92 }
// ]
```

Use `find()` to get the **first** student with a grade of at least `90`:

```js
const topStudent = students.find((student) => student.grade >= 90);

console.log(topStudent); // { name: "Alex", grade: 92 }
```

## Chaining Methods

Since `filter()` and `map()` return arrays, you can chain them:

```js
const students = [
  { name: "Jadd", grade: 85 },
  { name: "Alex", grade: 92 },
  { name: "Sam", grade: 68 },
];

const passingNames = students
  .filter((student) => student.grade >= 80)
  .map((student) => student.name);

console.log(passingNames); // ["Jadd", "Alex"]
```

First, `filter()` keeps the matching students. Then, `map()` takes their names.

## Do These Methods Change the Original Array?

The methods themselves do not remove or replace elements in the original array. `map()` and `filter()` return new arrays, while `find()` returns an element.

However, if the elements are **objects**, the returned results can still refer to those same objects:

```js
const students = [{ name: "Jadd" }];
const matches = students.filter((student) => student.name === "Jadd");

matches[0].name = "Alex";

console.log(students[0].name); // "Alex"
```

## Key Takeaway

Use `map()` to **transform every element**, `filter()` to **keep all matches**, and `find()` to **get the first match**. Remember that the callback's returned value controls what each method does.

# The `every()` and `some()` Array Methods

`every()` and `some()` check whether elements in an array meet a condition. Both call a callback for array elements and return a **boolean**.

| Method    | Returns `true` when...               |
| --------- | ------------------------------------ |
| `every()` | All elements pass the test           |
| `some()`  | At least one element passes the test |

## `every()`: Do All Elements Pass?

`every()` returns `true` only if the callback returns a truthy value for **every** element:

```js
const scores = [85, 92, 78];

const allPassed = scores.every((score) => score >= 50);

console.log(allPassed); // true
```

If even one element fails, the result is `false`:

```js
const scores = [85, 42, 78];

const allPassed = scores.every((score) => score >= 50);

console.log(allPassed); // false
```

## `some()`: Does Any Element Pass?

`some()` returns `true` if the callback returns a truthy value for **at least one** element:

```js
const scores = [85, 42, 78];

const anyoneFailed = scores.some((score) => score < 50);

console.log(anyoneFailed); // true
```

If no elements pass the test, the result is `false`:

```js
const scores = [85, 92, 78];

const anyoneFailed = scores.some((score) => score < 50);

console.log(anyoneFailed); // false
```

## Using Them with Objects

These methods also work with arrays of objects:

```js
const tasks = [
  { title: "Read notes", completed: true },
  { title: "Practice JavaScript", completed: false },
  { title: "Review answers", completed: true },
];

const allCompleted = tasks.every((task) => task.completed);
const anyIncomplete = tasks.some((task) => !task.completed);

console.log(allCompleted); // false
console.log(anyIncomplete); // true
```

## They Stop When the Result Is Known

`every()` stops checking as soon as an element fails:

```js
const numbers = [2, 4, 5, 8];

const allEven = numbers.every((number) => number % 2 === 0);

console.log(allEven); // false
```

Once it reaches `5`, the result must be `false`, so it does not need to check `8`.

`some()` stops checking as soon as an element passes:

```js
const numbers = [1, 3, 4, 6];

const hasEven = numbers.some((number) => number % 2 === 0);

console.log(hasEven); // true
```

Once it reaches `4`, the result must be `true`.

## What Happens with an Empty Array?

For an empty array:

```js
console.log([].every((value) => value > 0)); // true
console.log([].some((value) => value > 0)); // false
```

`every()` returns `true` because **no element failed** the test. `some()` returns `false` because **no element passed** it. Neither callback runs.

If your rule also requires at least one element, check the length:

```js
const scores = [];

const allPassed = scores.length > 0 && scores.every((score) => score >= 50);

console.log(allPassed); // false
```

## Key Takeaway

Use `every()` to ask **“Do all elements pass?”** and `some()` to ask **“Does at least one pass?”** Both return a boolean and stop checking once they know the answer.

# The `reduce()` Array Method in JavaScript

`reduce()` processes an array and combines its elements into a **single result**. That result can be a number, string, object, or even another array.

A common use is adding numbers:

```js
const numbers = [2, 3, 5];

const total = numbers.reduce((accumulator, number) => {
  return accumulator + number;
}, 0);

console.log(total); // 10
```

## Basic Syntax

```js
array.reduce((accumulator, currentValue) => {
  return updatedAccumulator;
}, initialValue);
```

- **`accumulator`** holds the result built so far.
- **`currentValue`** is the array element being processed.
- **`initialValue`** is the accumulator's starting value.
- The callback's returned value becomes the accumulator for the next element.

After the last element, `reduce()` returns the final accumulator.

As with other arrow callbacks, you may omit braces for an implicit return. If you use braces, explicitly return the updated accumulator.

## Following Each Step

For the opening example, the accumulator changes like this:

| Current number | Accumulator before | Value returned |
| -------------: | -----------------: | -------------: |
|            `2` |                `0` |            `2` |
|            `3` |                `2` |            `5` |
|            `5` |                `5` |           `10` |

The final result is `10`.

## Choosing the Initial Value

Choose an initial value that makes sense for the result you want:

| Goal             | Typical initial value |
| ---------------- | --------------------- |
| Add numbers      | `0`                   |
| Multiply numbers | `1`                   |
| Build a string   | `""`                  |
| Build an array   | `[]`                  |
| Build an object  | `{}`                  |

For example, to multiply numbers:

```js
const numbers = [2, 3, 4];

const product = numbers.reduce(
  (accumulator, number) => accumulator * number,
  1
);

console.log(product); // 24
```

## Example: Adding Prices from Objects

```js
const items = [
  { name: "Notebook", price: 5 },
  { name: "Pen", price: 2 },
  { name: "Backpack", price: 30 },
];

const totalPrice = items.reduce((total, item) => {
  return total + item.price;
}, 0);

console.log(totalPrice); // 37
```

Here, the accumulator is named `total` because that describes what it holds.

## Example: Counting Values

The accumulator can be an object:

```js
const colors = ["red", "blue", "red", "green", "blue", "red"];

const counts = colors.reduce((result, color) => {
  result[color] = (result[color] || 0) + 1;
  return result;
}, {});

console.log(counts);
// { red: 3, blue: 2, green: 1 }
```

The callback updates the result object and returns it so the next repetition can keep using it.

## What If You Omit the Initial Value?

If you do not provide an initial value, `reduce()` uses the array's first element as the accumulator and starts the callback with the second element:

```js
const numbers = [2, 3, 5];

const total = numbers.reduce((accumulator, number) => accumulator + number);

console.log(total); // 10
```

However, calling `reduce()` on an empty array **without** an initial value throws an error:

```js
[].reduce((accumulator, number) => accumulator + number);
// TypeError
```

Providing an initial value handles an empty array:

```js
const total = [].reduce((accumulator, number) => accumulator + number, 0);

console.log(total); // 0
```

For clarity, it is usually a good idea to provide the initial value.

## Key Takeaway

`reduce()` carries an **accumulator** through an array. On each element, your callback returns the accumulator's next value. Provide an initial value and make sure the callback returns the result you want to carry forward.

# The `Map` Data Structure in JavaScript

A **`Map`** stores pairs of **keys and values**. Like an object, it lets you look up a value by its key.

**Note:** The `Map` data structure is different from the array `.map()` method. An array's `.map()` transforms elements; a `Map` stores key-value pairs.

## Creating a Map

Use `new Map()` to create an empty map:

```js
const scores = new Map();
```

You can also provide an array of `[key, value]` pairs:

```js
const scores = new Map([
  ["Jadd", 95],
  ["Alex", 87],
]);

console.log(scores.get("Alex")); // 87
```

## Adding and Updating Entries

Use `.set(key, value)` to add an entry:

```js
const scores = new Map();

scores.set("Jadd", 95);
```

Calling `.set()` with an existing key updates its value:

```js
scores.set("Jadd", 98);

console.log(scores.get("Jadd")); // 98
```

`.set()` returns the map, so you can chain calls:

```js
const scores = new Map().set("Jadd", 95).set("Alex", 87);
```

## Reading a Value

Use `.get(key)` to retrieve a value:

```js
const scores = new Map([["Jadd", 95]]);

console.log(scores.get("Jadd")); // 95
console.log(scores.get("Sam")); // undefined
```

A missing key returns `undefined`. A key can also exist with `undefined` as its value, so use `.has()` when you need to distinguish those cases.

## Checking Whether a Key Exists

```js
const scores = new Map([["Jadd", 95]]);

console.log(scores.has("Jadd")); // true
console.log(scores.has("Sam")); // false
```

## Counting Entries

Use the `.size` property to find the number of entries:

```js
const scores = new Map([
  ["Jadd", 95],
  ["Alex", 87],
]);

console.log(scores.size); // 2
```

`size` is a property, so do not add parentheses.

## Removing Entries

Use `.delete(key)` to remove one entry. It returns `true` if the key existed:

```js
const scores = new Map([["Jadd", 95]]);

console.log(scores.delete("Jadd")); // true
console.log(scores.has("Jadd")); // false
```

Use `.clear()` to remove every entry:

```js
scores.clear();

console.log(scores.size); // 0
```

## Keys Can Be Different Types

Object property keys are strings or symbols. A `Map` can use values of many types as keys, including numbers and objects:

```js
const results = new Map();

results.set(1, "First place");
results.set("1", "The text '1'");

console.log(results.get(1)); // "First place"
console.log(results.get("1")); // "The text '1'"
```

The number `1` and string `"1"` are different keys.

### Object Keys Use Identity

An object used as a key must be looked up with the **same object reference**:

```js
const student = { name: "Jadd" };
const grades = new Map();

grades.set(student, 95);

console.log(grades.get(student)); // 95
console.log(grades.get({ name: "Jadd" })); // undefined
```

The second lookup creates a different object, even though its contents look the same.

## Looping Through a Map

A `Map` keeps entries in insertion order. Use `for...of` to visit its `[key, value]` pairs:

```js
const scores = new Map([
  ["Jadd", 95],
  ["Alex", 87],
]);

for (const [name, score] of scores) {
  console.log(`${name}: ${score}`);
}
```

Output:

```text
Jadd: 95
Alex: 87
```

You can loop through just the keys or just the values:

```js
for (const name of scores.keys()) {
  console.log(name);
}

for (const score of scores.values()) {
  console.log(score);
}
```

`.entries()` gives the key-value pairs explicitly:

```js
for (const [name, score] of scores.entries()) {
  console.log(name, score);
}
```

## `Map` vs. Object

| Feature           | `Map`                             | Object                                           |
| ----------------- | --------------------------------- | ------------------------------------------------ |
| Keys              | Can be many value types           | Strings or symbols                               |
| Number of entries | `.size`                           | `Object.keys(object).length` for own string keys |
| Direct iteration  | Works with `for...of`             | Requires a method such as `Object.entries()`     |
| Typical use       | A collection of key-value entries | A value with named properties, such as a user    |

Use an object to describe something with known fields:

```js
const student = {
  name: "Jadd",
  age: 20,
};
```

Use a `Map` when you need a collection of entries that you will add, look up, or remove by key:

```js
const scoresByStudent = new Map();

scoresByStudent.set("Jadd", 95);
scoresByStudent.set("Alex", 87);
```

## Key Takeaway

A `Map` stores key-value pairs and preserves their insertion order. Use `.set()`, `.get()`, `.has()`, `.delete()`, and `.size` to work with its entries. Unlike object properties, map keys can be numbers, objects, and other types.

# The `Set` Data Structure in JavaScript

A **`Set`** stores a collection of **unique values**. If you add the same value more than once, it appears only once.

A `Set` keeps values in the order they were first added.

## Creating a Set

Use `new Set()` to create an empty set:

```js
const numbers = new Set();
```

You can also create one from an array or another iterable:

```js
const numbers = new Set([1, 2, 2, 3, 3]);

console.log(numbers); // Set { 1, 2, 3 }
console.log(numbers.size); // 3
```

The repeated values are stored only once.

## Adding Values

Use `.add()` to insert a value:

```js
const fruits = new Set();

fruits.add("apple");
fruits.add("banana");
fruits.add("apple");

console.log(fruits.size); // 2
```

`.add()` returns the set, so you can chain calls:

```js
const fruits = new Set().add("apple").add("banana").add("orange");
```

## Checking Whether a Value Exists

Use `.has()` to check whether a set contains a value:

```js
const fruits = new Set(["apple", "banana"]);

console.log(fruits.has("apple")); // true
console.log(fruits.has("grape")); // false
```

## Removing Values

Use `.delete()` to remove a value. It returns `true` if the value was present:

```js
const fruits = new Set(["apple", "banana"]);

console.log(fruits.delete("apple")); // true
console.log(fruits.has("apple")); // false
```

Use `.clear()` to remove all values:

```js
fruits.clear();

console.log(fruits.size); // 0
```

## Counting Values

Use the `.size` property to find how many unique values a set contains:

```js
const fruits = new Set(["apple", "banana"]);

console.log(fruits.size); // 2
```

`.size` is a property, so do not write `fruits.size()`.

## Looping Through a Set

A `Set` is iterable, so you can use `for...of`:

```js
const fruits = new Set(["apple", "banana", "orange"]);

for (const fruit of fruits) {
  console.log(fruit);
}
```

Output:

```text
apple
banana
orange
```

You can also use `.forEach()`:

```js
fruits.forEach((fruit) => {
  console.log(fruit);
});
```

## Removing Duplicates from an Array

A common use for a set is creating an array of unique values:

```js
const numbers = [1, 2, 2, 3, 3, 3];

const uniqueNumbers = [...new Set(numbers)];

console.log(uniqueNumbers); // [1, 2, 3]
```

`new Set(numbers)` removes the duplicates. Spread syntax `...` then puts the set's values into a new array.

## Sets Do Not Use Indexes

Unlike an array, a set does not give its values numbered positions:

```js
const colors = new Set(["red", "blue"]);

console.log(colors[0]); // undefined
```

If you need to access values by index, convert the set to an array:

```js
const colorArray = [...colors];

console.log(colorArray[0]); // "red"
```

## Objects Are Compared by Identity

Two separate objects count as different values, even if their contents look the same:

```js
const users = new Set();

users.add({ name: "Jadd" });
users.add({ name: "Jadd" });

console.log(users.size); // 2
```

To avoid adding the same object twice, use the same object reference:

```js
const user = { name: "Jadd" };
const users = new Set();

users.add(user);
users.add(user);

console.log(users.size); // 1
```

## `Set` vs. Array

| Feature           | `Set`                   | Array              |
| ----------------- | ----------------------- | ------------------ |
| Duplicate values  | Keeps one of each value | Allows duplicates  |
| Access by index   | No                      | Yes                |
| Check for a value | `.has(value)`           | `.includes(value)` |
| Number of values  | `.size`                 | `.length`          |

Use a set when you need a collection of unique values. Use an array when duplicates or positions matter.

## Key Takeaway

A `Set` stores unique values in insertion order. Use `.add()`, `.has()`, `.delete()`, `.clear()`, and `.size` to work with it. Convert an array to a set when you want to remove duplicate primitive values.

# Symbols in JavaScript

A **symbol** is a primitive value that is always unique. Symbols are often used as object property keys when you want to avoid name collisions.

```js
const id = Symbol();

console.log(typeof id); // "symbol"
```

Create symbols with `Symbol()`, **without** the `new` keyword.

## Every Symbol Is Unique

Even symbols with the same description are different values:

```js
const first = Symbol("id");
const second = Symbol("id");

console.log(first === second); // false
```

The text `"id"` is a **description** that helps identify the symbol when debugging. It does not determine its identity.

## Using a Symbol as an Object Key

Use square brackets when adding a property whose key is a symbol:

```js
const id = Symbol("id");

const user = {
  name: "Jadd",
  [id]: 123,
};

console.log(user.name); // "Jadd"
console.log(user[id]); // 123
```

Here, `[id]` uses the symbol stored in the `id` variable as the property key.

Without the brackets, the key would be the ordinary string `"id"`:

```js
const user = {
  id: 123,
};
```

## Why Use a Symbol Key?

String property names can conflict. A symbol key lets you add a property without overwriting a property that happens to have the same string name:

```js
const internalId = Symbol("id");

const user = {
  id: "public-123",
  [internalId]: "internal-456",
};

console.log(user.id); // "public-123"
console.log(user[internalId]); // "internal-456"
```

The string key `"id"` and the symbol key `internalId` are different properties.

## Symbol Properties and Object Methods

Methods such as `Object.keys()` and `Object.entries()` return string-keyed properties, so they leave out symbol keys:

```js
const secretKey = Symbol("secret");

const user = {
  name: "Jadd",
  [secretKey]: 42,
};

console.log(Object.keys(user)); // ["name"]
```

You can retrieve an object's own symbol keys with `Object.getOwnPropertySymbols()`:

```js
console.log(Object.getOwnPropertySymbols(user));
// [Symbol(secret)]
```

**Symbols do not make data private or secure.** Other code can still find the symbol key and read its value.

## Symbols and JSON

`JSON.stringify()` leaves out properties whose keys are symbols:

```js
const id = Symbol("id");

const user = {
  name: "Jadd",
  [id]: 123,
};

console.log(JSON.stringify(user)); // '{"name":"Jadd"}'
```

Keep this in mind if you plan to save or send an object as JSON.

## Shared Symbols with `Symbol.for()`

Normally, each call to `Symbol()` creates a new symbol. `Symbol.for()` instead looks up a symbol by a string key in a shared symbol registry. If it does not exist, JavaScript creates it:

```js
const first = Symbol.for("app.userId");
const second = Symbol.for("app.userId");

console.log(first === second); // true
```

Use `Symbol()` when you want a unique symbol. Use `Symbol.for()` when separate parts of a program intentionally need to retrieve the same symbol by name.

## Built-In Symbols

JavaScript has built-in symbols that let objects participate in certain language features. For example, `Symbol.iterator` identifies the method used to iterate over a value:

```js
const numbers = [1, 2, 3];

console.log(typeof numbers[Symbol.iterator]); // "function"
```

This is part of why arrays work with `for...of`. You do not need to create your own iterator to use ordinary arrays.

## Key Takeaway

`Symbol()` creates a unique primitive value. Use symbols as object property keys when you need to avoid naming collisions; ordinary string keys are simpler for typical data. Symbols are omitted by common string-key methods and JSON serialization, but they do **not** provide privacy.
