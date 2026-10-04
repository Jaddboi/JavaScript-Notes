# Table of Contents

- [Introduction to Object-Oriented Programming](#introduction-to-object-oriented-programming-in-javascript)
- [The `this` Keyword](#demystifying-the-this-keyword-in-javascript)
- [Factory Functions](#factory-functions-in-javascript)
- [Constructor Functions](#constructor-functions-in-javascript)
- [Built-In Constructors](#built-in-constructors-in-javascript)
- [The `Object.create()` Method](#the-objectcreate-method-in-javascript)
- [The Prototype Model](#the-prototype-model-in-javascript)
- [Prototypal Inheritance](#prototypal-inheritance-in-javascript)
- [Introduction to Classes](#introduction-to-classes-in-javascript)
- [Class Inheritance](#inheritance-in-javascript-classes)
- [Modifiers](#modifiers-in-javascript-oop)
- [Encapsulation](#encapsulation-in-javascript)
- [Abstraction](#abstraction-in-javascript)
- [Polymorphism](#polymorphism-in-javascript)
- [Synchronous and Asynchronous Programming](#synchronous-and-asynchronous-programming-in-javascript)
- [Callback Hell](#callback-hell-in-javascript)
- [Promises](#promises-in-javascript)
- [Async Functions](#async-functions-in-javascript)
- [Reading Textual Data](#reading-textual-data-in-javascript)
- [Handling JSON Data](#handling-json-data-in-javascript)
- [Fetching Data from APIs](#fetching-data-from-apis-in-javascript)
- [Error Handling](#error-handling-in-javascript)

# Introduction to Object-Oriented Programming in JavaScript

**Object-oriented programming (OOP)** is a way to organize code around **objects**. An object can hold data in properties and behavior in methods.

For example, a `Student` object could store a name and grade and provide a method that checks whether the student passed.

## Objects Combine Data and Behavior

```js
const student = {
  name: "Jadd",
  grade: 85,

  hasPassed() {
    return this.grade >= 50;
  },
};

console.log(student.name); // "Jadd"
console.log(student.hasPassed()); // true
```

- `name` and `grade` are **properties**.
- `hasPassed()` is a **method**.
- `this.grade` accesses the grade of the object calling the method.

## Why Use a Class?

Creating several similar objects by hand repeats the same structure:

```js
const firstStudent = { name: "Jadd", grade: 85 };
const secondStudent = { name: "Alex", grade: 92 };
```

A **class** puts that shared definition in one place. It describes how to create objects with consistent properties and methods. Each object created from the class is called an **instance**. The dedicated classes chapter covers the syntax.

## Main OOP Ideas

OOP is commonly described with these ideas:

1. **Encapsulation:** Keep related data and behavior together and control access to internal state.
2. **Inheritance:** Let a more specific class reuse and extend a more general class.
3. **Polymorphism:** Let different objects respond to the same method call in their own ways.

Later chapters explain each idea in detail.

## Classes and Prototypes

JavaScript classes use **prototypes** behind the scenes. Methods such as `hasPassed()` are shared through a class's prototype rather than copied into every instance.

You can start using classes without knowing all the details of prototypes, but understanding them later will help explain how inheritance works in JavaScript.

## When Is OOP Useful?

OOP can help when your program has many related objects that share behavior, such as users, products, tasks, or game characters. A class gives those objects a consistent structure.

You do not need a class for every object. A simple object literal is often enough for one collection of related values:

```js
const settings = {
  theme: "dark",
  notifications: true,
};
```

## Key Takeaway

OOP organizes code into objects containing properties and methods. A class defines how to create similar objects, `new` creates an instance, and `this` lets its methods work with that instance's data.

# Demystifying the `this` Keyword in JavaScript

`this` is a value that a function can use to refer to its **calling context**. Its value depends on **how the function is called**. Arrow functions are an exception: they take `this` from the surrounding code.

## `this` in an Object Method

When you call a regular function as an object's method, `this` refers to the object before the dot:

```js
const user = {
  name: "Jadd",

  greet() {
    return `Hello, ${this.name}!`;
  },
};

console.log(user.greet()); // "Hello, Jadd!"
```

In `user.greet()`, `this` is `user`.

The same function can use a different `this` when called through another object:

```js
function introduce() {
  return `I am ${this.name}.`;
}

const firstUser = { name: "Jadd", introduce };
const secondUser = { name: "Alex", introduce };

console.log(firstUser.introduce()); // "I am Jadd."
console.log(secondUser.introduce()); // "I am Alex."
```

The function is the same, but the object making the method call changes.

## `this` Is About the Call, Not the Variable Name

Consider this:

```js
const user = {
  name: "Jadd",

  greet() {
    return `Hello, ${this.name}!`;
  },
};

const anotherName = user;

console.log(anotherName.greet()); // "Hello, Jadd!"
```

The variable does not need to be named `user`. In `anotherName.greet()`, `this` refers to the object stored in `anotherName`.

## A Detached Method Can Lose Its `this`

Saving a method in a separate variable removes the object from the call:

```js
const user = {
  name: "Jadd",

  greet() {
    return `Hello, ${this.name}!`;
  },
};

const greet = user.greet;

greet(); // Does not call the function with `user` as `this`
```

In strict mode, `this` is `undefined` in that plain function call, so accessing `this.name` throws an error. In other contexts, the value may differ, but it still will not automatically be `user`.

One solution is to wrap the method call:

```js
const greetUser = () => user.greet();

console.log(greetUser()); // "Hello, Jadd!"
```

Another solution is `.bind()`, covered below.

## `this` in a Plain Function

A regular function called without an object does not get an object-specific `this`:

```js
"use strict";

function showThis() {
  console.log(this);
}

showThis(); // undefined
```

Without strict mode, a plain function call may use the global object instead. The details of top-level `this` also differ between browser scripts, modules, and other JavaScript environments. Do not assume it always refers to the current object.

## `this` in an Arrow Function

An arrow function does **not** create its own `this`. It uses the `this` value from where the arrow function was created.

This makes an arrow function useful inside a regular method:

```js
const user = {
  name: "Jadd",

  greetLater() {
    setTimeout(() => {
      console.log(`Hello, ${this.name}!`);
    }, 1000);
  },
};

user.greetLater(); // Logs "Hello, Jadd!" after about one second
```

`greetLater()` gets `this` from `user.greetLater()`. The arrow callback uses that same `this`.

### Avoid Arrow Functions for Methods That Need the Object

```js
const user = {
  name: "Jadd",

  greet: () => {
    return `Hello, ${this.name}!`;
  },
};
```

The arrow function's `this` does **not** become `user` just because the function is stored in `user`. Use a regular method:

```js
const user = {
  name: "Jadd",

  greet() {
    return `Hello, ${this.name}!`;
  },
};
```

## `this` in a Class

Inside a class method called on an instance, `this` refers to that instance:

```js
class Counter {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count++;
  }
}

const counter = new Counter();

counter.increment();
counter.increment();

console.log(counter.count); // 2
```

In the constructor, `this` refers to the new instance being created.

A class method can also lose its instance's `this` if you detach it:

```js
const increment = counter.increment;

increment(); // Error: `this` is undefined in a class method
```

## Choosing `this` with `call()`, `apply()`, and `bind()`

These methods let you supply a particular `this` value to a regular function:

```js
function introduce(greeting) {
  return `${greeting}, I am ${this.name}.`;
}

const user = { name: "Jadd" };

console.log(introduce.call(user, "Hello"));
// "Hello, I am Jadd."

console.log(introduce.apply(user, ["Hi"]));
// "Hi, I am Jadd."

const introduceUser = introduce.bind(user);

console.log(introduceUser("Welcome"));
// "Welcome, I am Jadd."
```

| Method                            | What it does                                              |
| --------------------------------- | --------------------------------------------------------- |
| `.call(thisValue, arg1, arg2)`    | Calls the function immediately with separate arguments    |
| `.apply(thisValue, [arg1, arg2])` | Calls the function immediately with arguments in an array |
| `.bind(thisValue)`                | Returns a new function with that `this` value fixed       |

`call()`, `apply()`, and `bind()` cannot replace an arrow function's inherited `this`.

## `this` in DOM Event Handlers

For a regular function registered with `addEventListener()`, `this` refers to the element running the listener:

```js
const button = document.querySelector("#save-button");

button.addEventListener("click", function (event) {
  console.log(this === event.currentTarget); // true
});
```

An arrow handler does not receive that `this`. Use `event.currentTarget` when you want the listening element without relying on `this`:

```js
button.addEventListener("click", (event) => {
  console.log(event.currentTarget); // The button
});
```

## A Quick Guide

| How a function is called                        | `this` in a regular function |
| ----------------------------------------------- | ---------------------------- |
| `object.method()`                               | `object`                     |
| `functionName()` in strict mode                 | `undefined`                  |
| `new ClassName()` in a constructor              | The new instance             |
| `functionName.call(object)`                     | `object`                     |
| DOM listener registered with a regular function | The listening element        |

An **arrow function** takes `this` from its surrounding scope instead of following these call rules.

## Key Takeaway

For regular functions, look at **how the function is called** to understand `this`. For arrow functions, look at **where the function was created**. When a method must keep a specific `this`, call it through its object or bind it.

# Factory Functions in JavaScript

A **factory function** is a function that creates and returns an object. It lets you make multiple objects with the same structure while giving each object its own values.

```js
function createUser(name, age) {
  return {
    name,
    age,
  };
}

const firstUser = createUser("Jadd", 20);
const secondUser = createUser("Alex", 22);

console.log(firstUser); // { name: "Jadd", age: 20 }
console.log(secondUser); // { name: "Alex", age: 22 }
```

Each call returns a **new object**.

## Adding Methods

A factory function can return objects with methods:

```js
function createStudent(name, grade) {
  return {
    name,
    grade,

    introduce() {
      return `Hi, I am ${this.name}.`;
    },

    hasPassed() {
      return this.grade >= 50;
    },
  };
}

const student = createStudent("Jadd", 85);

console.log(student.introduce()); // "Hi, I am Jadd."
console.log(student.hasPassed()); // true
```

In `student.introduce()`, `this` refers to the object calling the method.

## Updating an Object Through Its Methods

Methods can change the properties of the object returned by a factory:

```js
function createCounter() {
  return {
    count: 0,

    increment() {
      this.count++;
    },

    reset() {
      this.count = 0;
    },
  };
}

const counter = createCounter();

counter.increment();
counter.increment();

console.log(counter.count); // 2

counter.reset();

console.log(counter.count); // 0
```

Each new counter has its own `count`:

```js
const firstCounter = createCounter();
const secondCounter = createCounter();

firstCounter.increment();

console.log(firstCounter.count); // 1
console.log(secondCounter.count); // 0
```

## Using Default Parameters

Factory functions can give properties default values:

```js
function createTask(title, completed = false) {
  return {
    title,
    completed,
  };
}

console.log(createTask("Study JavaScript"));
// { title: "Study JavaScript", completed: false }

console.log(createTask("Read notes", true));
// { title: "Read notes", completed: true }
```

## Private Data with a Closure

A factory function can keep a variable inside the function and return methods that use it:

```js
function createCounter() {
  let count = 0;

  return {
    increment() {
      count++;
    },

    getCount() {
      return count;
    },
  };
}

const counter = createCounter();

counter.increment();
counter.increment();

console.log(counter.getCount()); // 2
console.log(counter.count); // undefined
```

The methods can access `count` because they were created inside `createCounter()`. Code outside the function cannot access `count` directly. This is an example of a **closure**.

## Factory Functions Do Not Use `new`

Call a factory function like a regular function:

```js
const user = createUser("Jadd", 20);
```

You do not need `new` because the function creates and returns the object itself.

## Factory Function vs. Class

Both factory functions and classes can create similar objects:

```js
// Factory function
function createUser(name) {
  return {
    name,

    greet() {
      return `Hello, ${this.name}!`;
    },
  };
}

const userA = createUser("Jadd");
```

```js
// Class
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello, ${this.name}!`;
  }
}

const userB = new User("Alex");
```

| Factory function                                            | Class                                               |
| ----------------------------------------------------------- | --------------------------------------------------- |
| Returns an object directly                                  | Creates an instance with `new`                      |
| Can keep variables private through closures                 | Can use private fields and shared prototype methods |
| Methods written inside the factory are created on each call | Class methods are shared through the prototype      |

Choose the approach that makes the objects and their behavior easiest to understand for your program.

## Key Takeaway

A factory function creates and returns a new object each time you call it. It is useful for making objects with a consistent structure, adding methods, setting defaults, and keeping data inside a closure.

# Constructor Functions in JavaScript

A **constructor function** creates objects with a shared structure. It is a regular function intended to be called with the **`new`** keyword.

```js
function User(name, age) {
  this.name = name;
  this.age = age;
}

const firstUser = new User("Jadd", 20);
const secondUser = new User("Alex", 22);

console.log(firstUser.name); // "Jadd"
console.log(secondUser.name); // "Alex"
```

By convention, constructor function names start with a capital letter, such as `User` or `Student`.

## How `new` Works

When you call a constructor function with `new`, JavaScript:

1. Creates a new object.
2. Sets `this` inside the function to that object.
3. Connects the object to the constructor's prototype.
4. Runs the function's code.
5. Returns the new object, unless the function explicitly returns another object.

```js
function Student(name, grade) {
  this.name = name;
  this.grade = grade;
}

const student = new Student("Jadd", 85);
```

Here, the constructor assigns `name` and `grade` to the newly created object.

## Each Instance Has Its Own Properties

```js
function Counter(start) {
  this.count = start;
}

const firstCounter = new Counter(0);
const secondCounter = new Counter(10);

firstCounter.count++;

console.log(firstCounter.count); // 1
console.log(secondCounter.count); // 10
```

Changing one instance's `count` does not change the other's.

## Adding Methods

You can define a method inside the constructor:

```js
function Student(name, grade) {
  this.name = name;
  this.grade = grade;

  this.hasPassed = function () {
    return this.grade >= 50;
  };
}

const student = new Student("Jadd", 85);

console.log(student.hasPassed()); // true
```

This works, but it creates a separate `hasPassed` function for every new student.

## Sharing Methods with the Prototype

Put methods on the constructor's `.prototype` when all instances should share them:

```js
function Student(name, grade) {
  this.name = name;
  this.grade = grade;
}

Student.prototype.hasPassed = function () {
  return this.grade >= 50;
};

const firstStudent = new Student("Jadd", 85);
const secondStudent = new Student("Alex", 92);

console.log(firstStudent.hasPassed()); // true
console.log(secondStudent.hasPassed()); // true

console.log(firstStudent.hasPassed === secondStudent.hasPassed); // true
```

The students have their own `name` and `grade` properties, but they use the **same method** through the prototype.

Inside a regular prototype method, `this` refers to the instance calling it:

```js
firstStudent.hasPassed(); // `this` is firstStudent
secondStudent.hasPassed(); // `this` is secondStudent
```

Avoid an arrow function for a prototype method that needs the calling instance's `this`.

## Checking an Instance

Use `instanceof` to check whether an object was created through a constructor's prototype:

```js
const student = new Student("Jadd", 85);

console.log(student instanceof Student); // true
```

## Remember to Use `new`

A constructor function should be called with `new`:

```js
const student = new Student("Jadd", 85);
```

Calling it as a plain function gives `this` different behavior. In strict mode, `this` is `undefined`, so assigning `this.name` causes an error:

```js
"use strict";

Student("Jadd", 85); // Error
```

The capitalized name signals to other developers that `new` is expected.

## Constructor Functions vs. Classes

Modern `class` syntax expresses the same constructor-and-prototype pattern more directly. Both approaches create objects with `new` and share methods through a prototype. The next class chapter covers the modern syntax.

## Key Takeaway

A constructor function uses `this` to set up a new object when called with `new`. Give each instance its own data in the constructor, and put shared methods on the constructor's prototype.

# Built-In Constructors in JavaScript

JavaScript provides **built-in constructors** for creating common kinds of objects, such as arrays, dates, maps, and sets.

You have already used some of them:

```js
const today = new Date();
const scores = new Map();
const uniqueNames = new Set();
```

The `new` keyword creates an instance using the constructor.

## Common Built-In Constructors

| Constructor | Creates                         | Example                             |
| ----------- | ------------------------------- | ----------------------------------- |
| `Object`    | An ordinary object              | `new Object()`                      |
| `Array`     | An array                        | `new Array()`                       |
| `Date`      | A date and time object          | `new Date()`                        |
| `Map`       | A collection of key-value pairs | `new Map()`                         |
| `Set`       | A collection of unique values   | `new Set()`                         |
| `RegExp`    | A regular expression            | `new RegExp("cat")`                 |
| `Error`     | An error object                 | `new Error("Something went wrong")` |

## Creating Arrays and Objects

You _can_ create an array or ordinary object with a constructor:

```js
const colors = new Array("red", "blue");
const user = new Object();

user.name = "Jadd";
```

In most cases, array and object **literals** are shorter and clearer:

```js
const colors = ["red", "blue"];
const user = { name: "Jadd" };
```

### A Pitfall with `new Array()`

A single numeric argument creates an array with that **length**, not an array containing the number:

```js
const numbers = new Array(3);

console.log(numbers.length); // 3
console.log(numbers); // Array with 3 empty slots
```

To create an array containing the value `3`, use:

```js
const numbers = [3];
```

## Creating Dates

`Date` creates an object representing a moment in time:

```js
const now = new Date();

console.log(now);
```

You can supply date parts:

```js
const date = new Date(2026, 9, 4);
```

In this form, month indexes start at `0`, so `9` means **October**.

## Creating Maps and Sets

A `Map` stores key-value pairs:

```js
const scores = new Map([
  ["Jadd", 95],
  ["Alex", 87],
]);

console.log(scores.get("Jadd")); // 95
```

A `Set` stores unique values:

```js
const colors = new Set(["red", "blue", "red"]);

console.log(colors.size); // 2
```

## Creating Regular Expressions

A `RegExp` object can search for patterns in text:

```js
const pattern = new RegExp("cat");

console.log(pattern.test("My cat is sleeping.")); // true
```

For a fixed pattern, a regular expression literal is often simpler:

```js
const pattern = /cat/;
```

Both forms create regular expression objects.

## Creating Errors

`Error` creates an object that describes a problem:

```js
const error = new Error("Something went wrong");

console.log(error.message); // "Something went wrong"
```

You can throw it to stop normal execution and signal a problem:

```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }

  return a / b;
}
```

## Primitive Values vs. Wrapper Objects

JavaScript also has `String`, `Number`, and `Boolean` constructors. Be careful: calling them **with** `new` creates objects, not primitive values.

```js
const text = "hello";
const wrappedText = new String("hello");

console.log(typeof text); // "string"
console.log(typeof wrappedText); // "object"
```

This can produce surprising results with booleans:

```js
const wrappedFalse = new Boolean(false);

if (wrappedFalse) {
  console.log("This runs!");
}
```

Objects are truthy, so the code runs even though the wrapped value is `false`.

Use literals for ordinary primitive values:

```js
const name = "Jadd";
const age = 20;
const isStudent = true;
```

Without `new`, `String()`, `Number()`, and `Boolean()` are useful for **type conversion**:

```js
console.log(String(20)); // "20"
console.log(Number("20")); // 20
console.log(Boolean(0)); // false
```

## Checking What Was Created

`instanceof` can check whether an object was created through a constructor's prototype:

```js
const date = new Date();
const scores = new Map();

console.log(date instanceof Date); // true
console.log(scores instanceof Map); // true
```

For arrays, use `Array.isArray()` when you want to check whether a value is an array:

```js
console.log(Array.isArray([1, 2, 3])); // true
console.log(Array.isArray({ 0: 1 })); // false
```

## Key Takeaway

Built-in constructors create many of JavaScript's standard objects. Use `new Date()`, `new Map()`, or `new Set()` when you need those objects. For ordinary arrays, objects, strings, numbers, and booleans, literals are usually the clearest choice.

# The `Object.create()` Method in JavaScript

`Object.create()` creates a **new object** and lets you choose that object's **prototype**.

A prototype is another object that JavaScript can look at when a property is not found directly on the new object.

```js
const animal = {
  eat() {
    return `${this.name} is eating.`;
  },
};

const dog = Object.create(animal);
dog.name = "Buddy";

console.log(dog.eat()); // "Buddy is eating."
```

`dog` does not have its own `eat` method. JavaScript finds `eat` on `animal`, which is `dog`'s prototype.

## How Property Lookup Works

When you read a property, JavaScript:

1. Looks for it directly on the object.
2. If it is not there, looks on the object's prototype.
3. Continues up the prototype chain until it finds the property or reaches the end.

```js
const person = {
  species: "human",
};

const user = Object.create(person);
user.name = "Jadd";

console.log(user.name); // "Jadd": own property
console.log(user.species); // "human": inherited property
```

This connection is called **prototype inheritance**.

## Own Properties vs. Inherited Properties

Use `Object.hasOwn()` to check whether a property belongs directly to an object:

```js
console.log(Object.hasOwn(user, "name")); // true
console.log(Object.hasOwn(user, "species")); // false
```

The `in` operator checks the object **and** its prototype chain:

```js
console.log("name" in user); // true
console.log("species" in user); // true
```

## Methods Use the Calling Object as `this`

An inherited regular method uses the object that calls it as `this`:

```js
const person = {
  introduce() {
    return `I am ${this.name}.`;
  },
};

const firstUser = Object.create(person);
firstUser.name = "Jadd";

const secondUser = Object.create(person);
secondUser.name = "Alex";

console.log(firstUser.introduce()); // "I am Jadd."
console.log(secondUser.introduce()); // "I am Alex."
```

Both objects inherit the same method, but each call uses that object's `name`.

## Overriding an Inherited Property

Assigning a property on the new object creates an **own property** that takes precedence over the inherited one:

```js
const person = {
  role: "Member",
};

const user = Object.create(person);

console.log(user.role); // "Member"

user.role = "Admin";

console.log(user.role); // "Admin"
console.log(person.role); // "Member"
```

The assignment did not change `person.role`. The new `user.role` hides it for lookups on `user`. This is called **shadowing**.

## The Prototype Is Not a Copy

`Object.create()` does not copy the prototype's properties into the new object. It links the new object to that prototype:

```js
const defaults = {
  theme: "light",
};

const settings = Object.create(defaults);

console.log(settings.theme); // "light"

defaults.theme = "dark";

console.log(settings.theme); // "dark"
```

Because `settings` inherits `theme`, a change to `defaults.theme` is visible through `settings` until `settings` gets its own `theme` property.

## Checking the Prototype

Use `Object.getPrototypeOf()` to see an object's direct prototype:

```js
const person = { species: "human" };
const user = Object.create(person);

console.log(Object.getPrototypeOf(user) === person); // true
```

## Creating an Object with No Prototype

Pass `null` to create an object with no prototype:

```js
const dictionary = Object.create(null);

dictionary.apple = "A fruit";

console.log(dictionary.apple); // "A fruit"
```

This can be useful for a dictionary of string keys. Unlike an ordinary object, it does not inherit methods such as `toString()`:

```js
console.log(dictionary.toString); // undefined
```

Use `Object.hasOwn(dictionary, key)` to check its own properties:

```js
console.log(Object.hasOwn(dictionary, "apple")); // true
```

## Providing Properties When Creating the Object

`Object.create()` accepts an optional second argument containing **property descriptors**:

```js
const person = {
  greet() {
    return `Hello, ${this.name}!`;
  },
};

const user = Object.create(person, {
  name: {
    value: "Jadd",
    writable: true,
    enumerable: true,
    configurable: true,
  },
});

console.log(user.name); // "Jadd"
console.log(user.greet()); // "Hello, Jadd!"
```

Property descriptors control whether a property can be changed, listed, or deleted. For basic use, creating the object and assigning properties afterward is simpler:

```js
const user = Object.create(person);
user.name = "Jadd";
```

## Key Takeaway

`Object.create(prototype)` makes a new object linked to the prototype you choose. The new object can use properties and methods from that prototype without copying them. Use `Object.hasOwn()` when you need to distinguish an object's own properties from inherited ones.

# The Prototype Model in JavaScript

JavaScript objects can inherit properties and methods from other objects through **prototypes**.

The previous chapters explain property lookup and constructor prototypes. Classes use the same model: instance methods are stored on the class's prototype and shared. This section shows that built-in objects work the same way.

## Built-In Objects Have Prototypes

Arrays also inherit methods through a prototype:

```js
const numbers = [1, 2, 3];

console.log(Object.getPrototypeOf(numbers) === Array.prototype); // true

console.log(Object.hasOwn(numbers, "map")); // false
console.log(typeof numbers.map); // "function"
```

The array does not have its own `map` method. JavaScript finds it through `Array.prototype`.

## Key Takeaway

JavaScript's prototype model lets objects inherit and share behavior. Property lookup starts on the object and follows its prototype chain until it finds a match or reaches `null`. Constructor functions and classes both use this model.

# Prototypal Inheritance in JavaScript

**Prototypal inheritance** lets one object use properties and methods from another object. The object it inherits from is its **prototype**.

The earlier prototype chapters explain how to create a direct link and how property lookup works. Here, the focus is on longer chains and constructor-function inheritance.

## Building a Prototype Chain

A prototype can have its own prototype:

```js
const livingThing = {
  breathe() {
    return "Breathing";
  },
};

const animal = Object.create(livingThing);

animal.eat = function () {
  return "Eating";
};

const dog = Object.create(animal);

dog.bark = function () {
  return "Woof!";
};

console.log(dog.bark()); // "Woof!" — found on dog
console.log(dog.eat()); // "Eating" — found on animal
console.log(dog.breathe()); // "Breathing" — found on livingThing
```

For `dog.breathe()`, JavaScript looks through this chain:

```text
dog → animal → livingThing
```

If it reaches the end of the chain without finding a property, reading that property returns `undefined`.

## Inheritance with Constructor Functions

Constructor functions can link their prototypes to create an inheritance chain:

```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.eat = function () {
  return `${this.name} is eating.`;
};

function Dog(name, breed) {
  Animal.call(this, name); // Set up Animal's instance properties
  this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function () {
  return `${this.name} says woof!`;
};

const dog = new Dog("Buddy", "Golden Retriever");

console.log(dog.eat()); // "Buddy is eating."
console.log(dog.bark()); // "Buddy says woof!"
```

There are two separate steps:

1. `Animal.call(this, name)` sets up the new dog's `name`.
2. `Object.create(Animal.prototype)` lets dogs inherit methods such as `eat()`.

Resetting `Dog.prototype.constructor` records `Dog` as the constructor on the new prototype object.

Class inheritance, method overriding, `super`, and `instanceof` are covered in [Inheritance in JavaScript Classes](#inheritance-in-javascript-classes).

## Be Careful with Shared Prototype Data

Properties placed on a prototype are shared. This is useful for methods, but mutable data such as arrays can cause surprising behavior:

```js
const animal = {
  toys: [],
};

const firstDog = Object.create(animal);
const secondDog = Object.create(animal);

firstDog.toys.push("ball");

console.log(secondDog.toys); // ["ball"]
```

Both dogs found the **same array** on `animal`. Give each instance its own array when its data should be independent:

```js
firstDog.toys = [];
secondDog.toys = [];

firstDog.toys.push("ball");

console.log(secondDog.toys); // []
```

## Key Takeaway

Prototypal inheritance links objects so they can share properties and methods. JavaScript searches up the prototype chain when a property is missing, while an object's own properties take priority. `Object.create()`, constructor functions, and classes all use this underlying model.

# Introduction to Classes in JavaScript

A **class** is a way to define how similar objects are created and what they can do. Objects created from a class are called **instances**.

```js
class Student {
  constructor(name, grade) {
    this.name = name;
    this.grade = grade;
  }

  introduce() {
    return `Hi, I am ${this.name}.`;
  }
}
```

This class defines the properties and behavior of a student. It does not create a student until you use `new`.

## Creating an Instance

Use `new` to create an object from a class:

```js
const student = new Student("Jadd", 85);

console.log(student.name); // "Jadd"
console.log(student.grade); // 85
console.log(student.introduce()); // "Hi, I am Jadd."
```

You can create multiple independent instances:

```js
const firstStudent = new Student("Jadd", 85);
const secondStudent = new Student("Alex", 92);

console.log(firstStudent.name); // "Jadd"
console.log(secondStudent.name); // "Alex"
```

Each instance has its own `name` and `grade`.

## The `constructor()` Method

The `constructor()` method in the opening `Student` class runs automatically when you create an instance with `new`.

When you write:

```js
const student = new Student("Jadd", 85);
```

JavaScript passes `"Jadd"` and `85` into the constructor. Inside it, `this` refers to the new instance.

The constructor is commonly used to set up an instance's initial properties.

## Adding Methods

Write methods inside the class body:

```js
class Student {
  constructor(name, grade) {
    this.name = name;
    this.grade = grade;
  }

  hasPassed() {
    return this.grade >= 50;
  }

  updateGrade(newGrade) {
    this.grade = newGrade;
  }
}

const student = new Student("Jadd", 85);

console.log(student.hasPassed()); // true

student.updateGrade(45);

console.log(student.grade); // 45
console.log(student.hasPassed()); // false
```

Call a method on an instance with parentheses, such as `student.hasPassed()`.

**Do not put commas between class methods.**

## Default Values in a Constructor

Constructor parameters can have defaults:

```js
class Task {
  constructor(title, completed = false) {
    this.title = title;
    this.completed = completed;
  }
}

const task = new Task("Study JavaScript");

console.log(task.title); // "Study JavaScript"
console.log(task.completed); // false
```

A class without an explicit constructor can still be instantiated:

```js
class Empty {}

const instance = new Empty();
```

## Class Methods Are Shared

Methods declared in a class are placed on the class's **prototype**. Instances share those methods:

```js
class Student {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `I am ${this.name}.`;
  }
}

const first = new Student("Jadd");
const second = new Student("Alex");

console.log(first.introduce === second.introduce); // true
```

Each student has its own `name`, but both use the same `introduce()` method.

## Checking an Instance

Use `instanceof` to check whether an object was created through a class's prototype:

```js
const student = new Student("Jadd");

console.log(student instanceof Student); // true
```

## Class Names and Declaration Order

Class names are usually written in **PascalCase**, with each word capitalized:

```js
class UserAccount {}
class CourseManager {}
```

Declare a class before you use it:

```js
class UserAccount {}

const account = new UserAccount(); // Works
```

```js
const account = new UserAccount(); // Error

class UserAccount {}
```

## Key Takeaway

A class defines a structure for creating similar objects. Use `new` to create an instance, `constructor()` to set its initial properties, and methods to give it behavior. Inside an instance method, `this` refers to the instance that called it.

# Inheritance in JavaScript Classes

**Inheritance** lets one class build on another. A class can reuse the properties and methods of a more general class while adding its own behavior.

The original class is called the **parent class** or **superclass**. The class that inherits from it is the **child class** or **subclass**.

## Using `extends`

Use `extends` to create a subclass:

```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  eat() {
    return `${this.name} is eating.`;
  }
}

class Dog extends Animal {
  bark() {
    return `${this.name} says woof!`;
  }
}

const dog = new Dog("Buddy");

console.log(dog.name); // "Buddy"
console.log(dog.eat()); // "Buddy is eating."
console.log(dog.bark()); // "Buddy says woof!"
```

`Dog` inherits `Animal`'s constructor and `eat()` method. It adds its own `bark()` method.

## Adding a Subclass Constructor

If the subclass needs to set up additional properties, define its own constructor and call `super()`:

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
}

const dog = new Dog("Buddy", "Golden Retriever");

console.log(dog.name); // "Buddy"
console.log(dog.breed); // "Golden Retriever"
```

`super(name)` calls the parent class's constructor, which sets `this.name`. The subclass constructor then sets `this.breed`.

In a subclass constructor, you must call `super()` **before using `this`**:

```js
class Dog extends Animal {
  constructor(name, breed) {
    this.breed = breed; // Error: cannot use this before super()
    super(name);
  }
}
```

If the subclass does not define a constructor, JavaScript supplies one that passes its arguments to the parent constructor.

## Inherited Methods Use the Subclass Instance

An inherited method can access the subclass instance's properties through `this`:

```js
class Animal {
  introduce() {
    return `My name is ${this.name}.`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super();
    this.name = name;
    this.breed = breed;
  }
}

const dog = new Dog("Buddy", "Golden Retriever");

console.log(dog.introduce()); // "My name is Buddy."
```

When `dog.introduce()` runs, `this` is `dog`, even though `introduce()` was defined in `Animal`.

## Overriding a Parent Method

A subclass can define a method with the same name as an inherited method. Its version takes priority:

```js
class Animal {
  speak() {
    return "An animal makes a sound.";
  }
}

class Dog extends Animal {
  speak() {
    return "Woof!";
  }
}

const animal = new Animal();
const dog = new Dog();

console.log(animal.speak()); // "An animal makes a sound."
console.log(dog.speak()); // "Woof!"
```

This is called **method overriding**.

## Calling the Parent Method with `super`

Inside a subclass method, use `super.methodName()` to call the parent class's method:

```js
class Animal {
  speak() {
    return "An animal makes a sound.";
  }
}

class Dog extends Animal {
  speak() {
    return `${super.speak()} Specifically, a dog says woof!`;
  }
}

const dog = new Dog();

console.log(dog.speak());
// "An animal makes a sound. Specifically, a dog says woof!"
```

Here, `super.speak()` runs `Animal`'s version, and the subclass adds more information.

## Multiple Levels of Inheritance

A subclass can itself be extended:

```js
class Animal {
  eat() {
    return "Eating";
  }
}

class Dog extends Animal {
  bark() {
    return "Woof!";
  }
}

class GuideDog extends Dog {
  guide() {
    return "Guiding a person";
  }
}

const guideDog = new GuideDog();

console.log(guideDog.eat()); // "Eating"
console.log(guideDog.bark()); // "Woof!"
console.log(guideDog.guide()); // "Guiding a person"
```

JavaScript searches for a method on the instance, then continues up the prototype chain until it finds it.

## Checking Inheritance with `instanceof`

`instanceof` checks whether a class's prototype appears in an object's prototype chain:

```js
const guideDog = new GuideDog();

console.log(guideDog instanceof GuideDog); // true
console.log(guideDog instanceof Dog); // true
console.log(guideDog instanceof Animal); // true
```

A `GuideDog` instance is also an instance of its parent classes.

## Static Methods Are Inherited Too

Subclasses can access public static methods defined on a parent class:

```js
class Animal {
  static category() {
    return "Living creature";
  }
}

class Dog extends Animal {}

console.log(Dog.category()); // "Living creature"
```

Static methods are called on the class, not on an instance.

## Private Fields Are Not Directly Accessible in Subclasses

A field declared with `#` can only be accessed within the class that declares it:

```js
class Animal {
  #energy = 100;

  getEnergy() {
    return this.#energy;
  }
}

class Dog extends Animal {
  showEnergy() {
    return this.getEnergy(); // Use the parent's public method
  }
}

const dog = new Dog();

console.log(dog.showEnergy()); // 100
```

Writing `this.#energy` directly inside `Dog` would cause a syntax error.

## When Should You Use Inheritance?

Inheritance works when the child class is a more specific kind of the parent class, such as a `Dog` being an `Animal`. If two classes only share a small piece of functionality, combining smaller objects or functions may be clearer than creating a long inheritance chain.

## Key Takeaway

Use `extends` to inherit from a parent class. Use `super()` to call its constructor, override methods to change inherited behavior, and use `super.methodName()` when you want to build on a parent method.

# Modifiers in JavaScript OOP

In object-oriented programming, **modifiers** control how properties and methods can be accessed. JavaScript classes support public members by default, private members with `#`, and static members with `static`.

Other languages may use keywords such as `protected` or `readonly`. JavaScript does not have those class modifiers.

## Public Members

Properties and methods are **public by default**. Code outside the class can access and change them:

```js
class Student {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `I am ${this.name}.`;
  }
}

const student = new Student("Jadd");

console.log(student.name); // "Jadd"
console.log(student.introduce()); // "I am Jadd."

student.name = "Alex";

console.log(student.name); // "Alex"
```

Here, both `name` and `introduce()` are public.

## Private Members with `#`

Prefix a field or method name with `#` to make it **private to the class**:

```js
class BankAccount {
  #balance;

  constructor(initialBalance) {
    this.#balance = initialBalance;
  }

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount(100);

account.deposit(50);

console.log(account.getBalance()); // 150
```

Code outside the class cannot access the private field:

```js
console.log(account.#balance); // Syntax error
```

The class must declare a private field before using it:

```js
class BankAccount {
  #balance;
}
```

Private fields let the class control how its internal data is read or changed.

## Private Methods

Methods can also be private:

```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    if (!this.#isValidAmount(amount)) {
      return false;
    }

    this.#balance += amount;
    return true;
  }

  getBalance() {
    return this.#balance;
  }

  #isValidAmount(amount) {
    return typeof amount === "number" && amount > 0;
  }
}

const account = new BankAccount();

console.log(account.deposit(25)); // true
console.log(account.getBalance()); // 25
```

`#isValidAmount()` can be called inside `BankAccount`, but not directly from outside it.

## Private Is Different from an Underscore

You may see names beginning with `_`:

```js
class Student {
  constructor(name) {
    this._name = name;
  }
}
```

The underscore is only a **naming convention**. JavaScript does not prevent outside code from accessing `_name`:

```js
const student = new Student("Jadd");

console.log(student._name); // "Jadd"
```

Use `#name` when you need language-enforced privacy.

## Static Members

A **static** method or field belongs to the class itself, rather than to each instance:

```js
class Student {
  static school = "University of Toronto";

  constructor(name) {
    this.name = name;
  }

  static describeSchool() {
    return `School: ${Student.school}`;
  }
}

console.log(Student.school);
// "University of Toronto"

console.log(Student.describeSchool());
// "School: University of Toronto"

const student = new Student("Jadd");

console.log(student.name); // "Jadd"
console.log(student.school); // undefined
```

Call static methods on the class, such as `Student.describeSchool()`, not on an instance.

Built-in methods such as `Array.isArray()` are examples of static methods:

```js
console.log(Array.isArray([1, 2, 3])); // true
```

## Private Static Members

You can combine `static` and `#` when data should belong to the class but remain private:

```js
class Student {
  static #count = 0;

  constructor(name) {
    this.name = name;
    Student.#count++;
  }

  static getCount() {
    return Student.#count;
  }
}

new Student("Jadd");
new Student("Alex");

console.log(Student.getCount()); // 2
```

`#count` is shared at the class level and cannot be accessed directly from outside `Student`.

## Getters and Setters

A **getter** lets you read a value using property syntax while running a method. A **setter** runs when you assign a value:

```js
class Student {
  #grade;

  constructor(grade) {
    this.grade = grade;
  }

  get grade() {
    return this.#grade;
  }

  set grade(value) {
    if (typeof value !== "number" || value < 0 || value > 100) {
      throw new Error("Grade must be between 0 and 100.");
    }

    this.#grade = value;
  }
}

const student = new Student(85);

console.log(student.grade); // 85

student.grade = 92;

console.log(student.grade); // 92
```

Even though `grade` looks like a property from outside, the getter and setter control access to the private `#grade` field.

## Does JavaScript Have `protected` or `readonly`?

JavaScript does **not** provide `protected` or `readonly` class modifiers:

- A **protected** member, in languages that support it, is typically available to the class and its subclasses. JavaScript's `#private` members are accessible only within the class that declares them, not directly within subclasses.
- A **readonly** member cannot be reassigned after initialization. JavaScript has no `readonly` keyword for class members.

You can design a class API to limit changes, but those words are not JavaScript class syntax.

## Quick Reference

| Syntax            | Meaning               | Access                                        |
| ----------------- | --------------------- | --------------------------------------------- |
| `name`            | Public member         | From inside or outside the class              |
| `#name`           | Private member        | Within the declaring class                    |
| `static name`     | Public static member  | Through the class                             |
| `static #name`    | Private static member | Within the declaring class, through the class |
| `get name()`      | Getter                | Read with property syntax                     |
| `set name(value)` | Setter                | Assign with property syntax                   |

## Key Takeaway

JavaScript class members are public unless you use `#` to make them private. Use `static` for values and methods that belong to the class itself. Getters and setters let you control how public-facing properties are read and changed.

# Encapsulation in JavaScript

**Encapsulation** means keeping an object's data and the methods that work with it together. It often also means controlling how other code can read or change that data.

For example, a counter should increase through an `increment()` method instead of requiring other code to manage its internal value.

The previous chapter explains public members, `#private` fields and methods, getters, setters, and the difference between `#name` and `_name`. Encapsulation is the design goal those features help achieve.

## Protecting Valid State

Encapsulation is useful when an object must follow rules. For example, an account should not accept negative deposits or allow withdrawals larger than its balance:

```js
class Account {
  #balance;

  constructor(initialBalance = 0) {
    if (!Number.isFinite(initialBalance) || initialBalance < 0) {
      throw new Error("Initial balance must be a nonnegative number.");
    }

    this.#balance = initialBalance;
  }

  deposit(amount) {
    if (!Number.isFinite(amount) || amount <= 0) {
      throw new Error("Deposit must be a positive number.");
    }

    this.#balance += amount;
  }

  withdraw(amount) {
    if (!Number.isFinite(amount) || amount <= 0) {
      throw new Error("Withdrawal must be a positive number.");
    }

    if (amount > this.#balance) {
      throw new Error("Insufficient balance.");
    }

    this.#balance -= amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new Account(100);

account.deposit(50);
account.withdraw(30);

console.log(account.getBalance()); // 120
```

The methods validate changes before updating `#balance`. Other code cannot directly assign a new value to the private field.

Classes are not the only option: the factory-function chapter shows how a closure can keep state private without a class.

## Key Takeaway

Encapsulation groups data with the operations that use it and lets an object control changes to its internal state. In JavaScript, use public methods with `#private` fields or closures when data should be managed through a defined interface.

# Abstraction in JavaScript

**Abstraction** means giving other code a simple way to use an object while keeping the details of how it works inside the object.

For example, you can call `playlist.playNext()` without knowing how the playlist stores songs or tracks its current position.

## A Simple Example

```js
class Playlist {
  #songs = [];
  #nextIndex = 0;

  addSong(title) {
    this.#songs.push(title);
  }

  playNext() {
    if (this.#songs.length === 0) {
      return "The playlist is empty.";
    }

    const song = this.#songs[this.#nextIndex];

    this.#nextIndex = (this.#nextIndex + 1) % this.#songs.length;

    return `Playing: ${song}`;
  }
}

const playlist = new Playlist();

playlist.addSong("First Song");
playlist.addSong("Second Song");

console.log(playlist.playNext()); // "Playing: First Song"
console.log(playlist.playNext()); // "Playing: Second Song"
console.log(playlist.playNext()); // "Playing: First Song"
```

Code using `Playlist` only needs to know about `addSong()` and `playNext()`. The class handles the array, the current position, and returning to the first song.

## The Public Interface

An object's **public interface** is the set of properties and methods other code is meant to use.

For the playlist:

```js
playlist.addSong("First Song");
playlist.playNext();
```

Those calls describe **what** the code wants to do. The private fields and calculations describe **how** the class does it.

A good public method usually has a name that describes a useful action:

```js
cart.addItem(product);
cart.removeItem(productId);
cart.getTotal();
```

The caller does not need to perform the cart's calculations itself.

## Why Is Abstraction Useful?

Abstraction can make code easier to use and change:

- **Less to remember:** Call a method instead of repeating several implementation steps.
- **Fewer mistakes:** The object handles its own rules in one place.
- **Easier changes:** You can change the internal implementation while keeping the same public methods.

For example, `Playlist` could later store songs differently. Code that calls `addSong()` and `playNext()` would not need to change as long as those methods keep behaving as expected.

## Abstraction Without Private Fields

Abstraction does not require `#private` fields. A function can also hide a calculation behind a clear name:

```js
function calculateFinalPrice(price, taxRate) {
  return price + price * taxRate;
}

const total = calculateFinalPrice(100, 0.13);

console.log(total); // 113
```

The caller asks for the final price without repeating the calculation.

Private fields strengthen the boundary by preventing outside code from directly using an object's internal state.

## Abstraction vs. Encapsulation

These ideas work together, but they focus on different questions:

| Idea          | Main question                                                                       |
| ------------- | ----------------------------------------------------------------------------------- |
| Abstraction   | What does the object let me do?                                                     |
| Encapsulation | How does the object keep its data and behavior together and control access to them? |

In the `Playlist` example, `playNext()` provides a simple action for callers. The `#songs` and `#nextIndex` fields keep the internal state private.

## Does JavaScript Have an `abstract` Keyword?

JavaScript does **not** have an `abstract` class keyword. You can still design classes and functions around clear public interfaces. The next chapter shows how unrelated objects can implement the same interface through polymorphism.

## Key Takeaway

Abstraction gives code a clear, simple way to perform an action while leaving the steps behind that action inside a function or object. Design methods around what callers need to do, and keep implementation details out of their way.

# Polymorphism in JavaScript

**Polymorphism** means different objects can respond to the **same method call** in different ways.

For example, every shape might have an `area()` method, but a circle and a rectangle calculate their areas differently.

## A Basic Example

```js
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  area() {
    return this.width * this.height;
  }
}

class Circle {
  constructor(radius) {
    this.radius = radius;
  }

  area() {
    return Math.PI * this.radius ** 2;
  }
}

const shapes = [new Rectangle(4, 5), new Circle(2)];

for (const shape of shapes) {
  console.log(shape.area());
}
```

Output:

```text
20
12.566370614359172
```

The loop calls `area()` the same way for each object. JavaScript runs the method belonging to the particular object.

## Why Is This Useful?

Code that uses the objects does not need separate instructions for every type:

```js
function printArea(shape) {
  console.log(`Area: ${shape.area()}`);
}

printArea(new Rectangle(4, 5));
printArea(new Circle(2));
```

`printArea()` works with either shape because both provide an `area()` method.

Subclass overriding is one way to achieve polymorphism. Its syntax, including `super`, is covered in the class-inheritance chapter.

## Inheritance Is Not Required

JavaScript can use polymorphism even when classes do not share a parent class. The objects only need to provide the method that the caller uses:

```js
class EmailNotifier {
  send(message) {
    console.log(`Email: ${message}`);
  }
}

class ConsoleNotifier {
  send(message) {
    console.log(`Console: ${message}`);
  }
}

function notify(notifier, message) {
  notifier.send(message);
}

notify(new EmailNotifier(), "Task complete");
notify(new ConsoleNotifier(), "Task complete");
```

`notify()` does not check which class it received. It calls `send()` and relies on the object to provide that method.

In JavaScript, this style is sometimes called **duck typing**: code cares about what an object can do rather than which class created it.

If an object does not have a callable `send` method, `notify()` will throw an error when it tries to call it.

## Avoid Repeated Type Checks When a Shared Method Fits

Without polymorphism, you might write:

```js
function getArea(shape) {
  if (shape.type === "rectangle") {
    return shape.width * shape.height;
  }

  if (shape.type === "circle") {
    return Math.PI * shape.radius ** 2;
  }
}
```

You would need to edit `getArea()` whenever you added a new shape type.

If each shape has its own `area()` method, the calling code stays simple:

```js
function getArea(shape) {
  return shape.area();
}
```

A new shape can provide its own `area()` method without adding another branch to `getArea()`.

## Polymorphism vs. Method Overloading

Some languages support **method overloading**, where multiple methods have the same name but different parameter lists. JavaScript does not select among class methods based on the number or types of arguments:

```js
class Greeter {
  greet() {
    return "Hello!";
  }

  greet(name) {
    return `Hello, ${name}!`;
  }
}

const greeter = new Greeter();

console.log(greeter.greet("Jadd")); // "Hello, Jadd!"
console.log(greeter.greet()); // "Hello, undefined!"
```

The second `greet()` definition replaces the first. If you want different behavior based on arguments, use one method with conditionals or default parameters.

## Key Takeaway

Polymorphism lets code call the same method on different objects while each object supplies its own behavior. In JavaScript, this can happen through subclass overrides or simply through unrelated objects that provide the same method.

# Synchronous and Asynchronous Programming in JavaScript

**Synchronous code** runs one step at a time. JavaScript finishes the current step before moving to the next one.

**Asynchronous code** lets JavaScript start an operation, continue with other work, and handle the result later. Common asynchronous operations include timers, network requests, and reading files.

## Synchronous Code

```js
console.log("First");
console.log("Second");
console.log("Third");
```

Output:

```text
First
Second
Third
```

Each statement finishes before the next one runs.

A slow synchronous task holds up the code after it:

```js
function doLotsOfWork() {
  for (let i = 0; i < 1_000_000_000; i++) {
    // Repeated work
  }
}

console.log("Before");
doLotsOfWork();
console.log("After");
```

`"After"` cannot be printed until `doLotsOfWork()` finishes. In a browser, a long task like this can also make the page temporarily unresponsive.

## Asynchronous Code with a Timer

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

`setTimeout()` schedules its callback. JavaScript continues to `console.log("Second")` while the timer waits.

The delay is approximate: the callback can run **after at least** the requested delay, once JavaScript is available to run it.

## The Event Loop: The Basic Idea

JavaScript runs the current synchronous code first. When that code finishes, it can handle callbacks and promise reactions that are ready to run.

```js
console.log("A");

setTimeout(() => {
  console.log("C");
}, 0);

console.log("B");
```

Output:

```text
A
B
C
```

A delay of `0` does not interrupt the code already running.

This scheduling process is commonly described using the **call stack**, **queues**, and **event loop**. You do not need to memorize every scheduling detail to start writing asynchronous code, but you should remember that a scheduled callback runs later.

## Common Asynchronous Patterns

JavaScript commonly represents future results with **Promises**. You can handle them with `.then()` and `.catch()`, or use `await` inside an `async` function. The following chapters explain callbacks, Promises, and async functions in detail.

## Asynchronous Does Not Mean Parallel

Starting an asynchronous operation lets other JavaScript code run while that operation waits. It does **not** mean every piece of JavaScript code runs at the same time.

For example, a long synchronous loop can still delay timer callbacks and make a browser page unresponsive. Timers and `await` do not automatically move heavy calculations off the main thread.

## Key Takeaway

Synchronous code finishes each step before continuing. Asynchronous operations produce results later, allowing other code to run while they wait. JavaScript commonly handles those results with callbacks, Promises, and `async`/`await`.

# Callback Hell in JavaScript

**Callback hell** happens when asynchronous operations depend on one another and their callbacks become deeply nested. The code can become difficult to read, change, and handle errors in.

Callbacks themselves are useful. The problem is usually the growing **nesting**.

## A Nested Callback Example

Suppose each step must finish before the next one begins:

```js
function runStep(name, callback) {
  setTimeout(() => {
    console.log(`Finished: ${name}`);
    callback();
  }, 1000);
}

runStep("Load user", () => {
  runStep("Load courses", () => {
    runStep("Load assignments", () => {
      console.log("All steps finished.");
    });
  });
});
```

Output over time:

```text
Finished: Load user
Finished: Load courses
Finished: Load assignments
All steps finished.
```

The later steps are nested inside earlier callbacks because their order matters. With more steps and error handling, the code keeps moving farther to the right.

## Why Is It a Problem?

Deeply nested callbacks can make it harder to:

- See the overall order of operations
- Reuse individual steps
- Add a new step
- Handle errors consistently
- Understand which variables are available at each level

For example, an operation with both success and failure callbacks may introduce even more branches.

## Improvement 1: Use Named Functions

Moving callbacks into named functions reduces indentation:

```js
function loadAssignments() {
  runStep("Load assignments", finish);
}

function loadCourses() {
  runStep("Load courses", loadAssignments);
}

function finish() {
  console.log("All steps finished.");
}

runStep("Load user", loadCourses);
```

The steps still use callbacks, but their bodies are no longer nested. Choose descriptive function names so the order remains easy to follow.

## Improvement 2: Use Promises

A Promise represents a result that will be available later. You can chain `.then()` calls to run dependent steps in order:

```js
function runStep(name) {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(`Finished: ${name}`);
      resolve();
    }, 1000);
  });
}

runStep("Load user")
  .then(() => runStep("Load courses"))
  .then(() => runStep("Load assignments"))
  .then(() => {
    console.log("All steps finished.");
  })
  .catch((error) => {
    console.error("A step failed:", error);
  });
```

Each `.then()` returns the Promise for the next step. The chain waits for that Promise before moving on.

This example only resolves. In real code, an operation can also reject its Promise when it fails.

## Improvement 3: Use `async` and `await`

`async` and `await` let you write the same sequence in a form that reads from top to bottom:

```js
async function runAllSteps() {
  try {
    await runStep("Load user");
    await runStep("Load courses");
    await runStep("Load assignments");

    console.log("All steps finished.");
  } catch (error) {
    console.error("A step failed:", error);
  }
}

runAllSteps();
```

`await` pauses `runAllSteps()` until each step's Promise settles. It does not block the rest of JavaScript.

## Sequential vs. Independent Steps

Nest or await operations in sequence only when the next step depends on the previous one. Independent Promise-based operations can start together with `Promise.all()`, covered in the next chapter.

## When Are Callbacks Still Fine?

A callback is a good fit for many tasks, such as responding to a click:

```js
button.addEventListener("click", () => {
  console.log("Button clicked.");
});
```

There is no callback hell here. The concern is a hard-to-follow chain of nested asynchronous steps.

## Key Takeaway

Callback hell is deeply nested callback code, often caused by several dependent asynchronous operations. Named functions can reduce nesting, while Promises and `async`/`await` make the sequence and its error handling easier to follow.

# Promises in JavaScript

A **Promise** represents the eventual result of an asynchronous operation. It gives you a way to handle a value that may become available later or an error that may occur.

For example, a network request may succeed with data or fail with an error. A Promise represents either outcome.

## Promise States

A Promise has one of three states:

| State         | Meaning                                      |
| ------------- | -------------------------------------------- |
| **Pending**   | The operation has not finished               |
| **Fulfilled** | The operation succeeded and produced a value |
| **Rejected**  | The operation failed and produced a reason   |

A Promise starts pending. Once it is fulfilled or rejected, it is **settled** and cannot change to another state.

## Creating a Promise

Use `new Promise()` and provide a function with `resolve` and `reject` parameters:

```js
const promise = new Promise((resolve, reject) => {
  const succeeded = true;

  if (succeeded) {
    resolve("Operation completed!");
  } else {
    reject(new Error("Operation failed."));
  }
});
```

- Call `resolve(value)` when the operation succeeds.
- Call `reject(reason)` when the operation fails.

The function passed to `new Promise()` starts running **immediately**. A Promise does not automatically make that function's work asynchronous.

## An Asynchronous Promise Example

This function returns a Promise that fulfills after a timer:

```js
function wait(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("Finished waiting!");
    }, milliseconds);
  });
}
```

Calling `wait(1000)` returns a Promise immediately. Its value becomes available after approximately one second.

## Handling Success with `.then()`

Pass a callback to `.then()` to use a fulfilled Promise's value:

```js
wait(1000).then((message) => {
  console.log(message); // "Finished waiting!"
});
```

The `.then()` callback runs later, after the Promise fulfills.

The Promise itself is not the final message:

```js
const result = wait(1000);

console.log(result); // A Promise, not "Finished waiting!"
```

## Handling Failure with `.catch()`

Use `.catch()` to handle a rejection:

```js
function checkAge(age) {
  return new Promise((resolve, reject) => {
    if (age >= 18) {
      resolve("Access granted.");
    } else {
      reject(new Error("You must be at least 18."));
    }
  });
}

checkAge(16)
  .then((message) => {
    console.log(message);
  })
  .catch((error) => {
    console.error(error.message); // "You must be at least 18."
  });
```

Errors thrown inside a `.then()` callback also become rejections that a later `.catch()` can handle.

## Running Code After Either Outcome with `.finally()`

A `.finally()` callback runs when the Promise settles, whether it fulfills or rejects:

```js
checkAge(20)
  .then((message) => {
    console.log(message);
  })
  .catch((error) => {
    console.error(error.message);
  })
  .finally(() => {
    console.log("Age check finished.");
  });
```

Use `.finally()` for cleanup that should happen after either outcome. It does not receive the fulfilled value or rejection reason.

## Chaining Promises

`.then()` returns a **new Promise**, so you can chain operations:

```js
Promise.resolve(5)
  .then((number) => {
    return number * 2;
  })
  .then((number) => {
    console.log(number); // 10
  });
```

The value returned from one `.then()` becomes the fulfillment value passed to the next one.

If a callback returns another Promise, the chain waits for it:

```js
wait(1000)
  .then(() => {
    return wait(1000);
  })
  .then(() => {
    console.log("Both waits finished.");
  });
```

You can shorten the first callback:

```js
wait(1000)
  .then(() => wait(1000))
  .then(() => console.log("Both waits finished."));
```

Always return a Promise from `.then()` when the next step must wait for it. Otherwise, the chain continues without waiting for that operation.

## Running Independent Promises Together

Use `Promise.all()` to wait for several independent Promises:

```js
Promise.all([wait(1000), wait(1500)]).then((results) => {
  console.log(results);
  // ["Finished waiting!", "Finished waiting!"]
});
```

`Promise.all()` fulfills with an array of results in the **same order as its input Promises**. It rejects if any input Promise rejects.

## Key Takeaway

A Promise represents a future success or failure. Use `.then()` for fulfilled values, `.catch()` for errors, and `.finally()` for cleanup. Return Promises from `.then()` when later steps must wait for them.

# Async Functions in JavaScript

An **async function** is a function declared with the `async` keyword. It always returns a **Promise**, and it can use `await` to work with Promise results.

```js
async function getGreeting() {
  return "Hello, Jadd!";
}
```

Even though the function returns a string, calling it returns a Promise:

```js
const result = getGreeting();

console.log(result); // A Promise
```

Handle its value with `.then()`:

```js
getGreeting().then((message) => {
  console.log(message); // "Hello, Jadd!"
});
```

## Using `await`

`await` pauses an async function until a Promise settles. If the Promise fulfills, the expression evaluates to its result.

Using the `wait()` function from the Promises chapter:

```js
async function showMessage() {
  console.log("Waiting...");

  const message = await wait(1000);

  console.log(message);
}

showMessage();
```

Output over time:

```text
Waiting...
Finished waiting!
```

## `await` Does Not Block All JavaScript

`await` pauses the **current async function**, allowing other code to continue:

```js
async function example() {
  console.log("First");

  await wait(1000);

  console.log("Third");
}

example();

console.log("Second");
```

Output:

```text
First
Second
Third
```

The call to `example()` returns a Promise while the function waits.

## Handling Errors

If an async function throws, its returned Promise is rejected. Use `try...catch` around awaited operations or attach `.catch()` to the returned Promise. The error-handling chapter covers both patterns in detail.

## Running Steps in Order

Use separate `await` expressions when one step needs to finish before the next:

```js
async function runSteps() {
  await wait(1000);
  console.log("First step finished.");

  await wait(1000);
  console.log("Second step finished.");
}

runSteps();
```

The second timer starts after the first one finishes.

## Async Arrow Functions

Arrow functions can be async too:

```js
const getGreeting = async (name) => {
  return `Hello, ${name}!`;
};

getGreeting("Jadd").then((message) => {
  console.log(message); // "Hello, Jadd!"
});
```

For a single expression, you can use an implicit return:

```js
const getGreeting = async (name) => `Hello, ${name}!`;
```

## Where Can You Use `await`?

You can use `await` inside an async function:

```js
async function load() {
  const result = await wait(1000);
  console.log(result);
}
```

You cannot put `await` inside an ordinary function:

```js
function load() {
  const result = await wait(1000); // Syntax error
}
```

Top-level `await` is also available in JavaScript **modules**, but not in ordinary scripts.

## Common Mistake: `forEach()` Does Not Wait for Async Callbacks

`forEach()` does not wait for Promises returned by its callback:

```js
const delays = [1000, 500];

delays.forEach(async (delay) => {
  await wait(delay);
  console.log(`Finished ${delay} ms wait`);
});

console.log("This runs before the waits finish.");
```

Use `for...of` for steps that must run in sequence:

```js
async function runInOrder() {
  for (const delay of delays) {
    await wait(delay);
    console.log(`Finished ${delay} ms wait`);
  }
}

runInOrder();
```

## Key Takeaway

An async function always returns a Promise. Use `await` inside it to get a Promise's result, `try...catch` to handle rejections, and `Promise.all()` when independent operations can start together.

# Reading Textual Data in JavaScript

A webpage can load text from a file or server using the **Fetch API**. `fetch()` sends a request and returns a Promise for the response. You can then use `response.text()` to read the response body as a string.

## Example: Reading a Text File

Suppose your project contains:

```text
project/
├── index.html
├── script.js
└── message.txt
```

And `message.txt` contains:

```text
Welcome to StudyFlow!
```

You can load it from `script.js`:

```js
async function loadMessage() {
  const response = await fetch("./message.txt");
  const text = await response.text();

  console.log(text); // "Welcome to StudyFlow!"
}

loadMessage();
```

Both `fetch()` and `response.text()` return Promises, so both are awaited.

## What Does `fetch()` Return?

`fetch()` does **not** directly give you the file's text:

```js
const result = fetch("./message.txt");

console.log(result); // A Promise
```

After awaiting it, you get a **Response** object:

```js
const response = await fetch("./message.txt");
```

The Response contains information about the request and a body you can read:

```js
const text = await response.text();

console.log(typeof text); // "string"
```

## Checking for HTTP Errors

A response such as `404 Not Found` does not automatically cause `fetch()` to reject. Check `response.ok` before reading the body:

```js
async function loadMessage() {
  try {
    const response = await fetch("./message.txt");

    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`);
    }

    const text = await response.text();
    console.log(text);
  } catch (error) {
    console.error("Could not load the message:", error);
  }
}

loadMessage();
```

`response.ok` is true for successful HTTP status codes in the `200`–`299` range. The `catch` block handles network failures and the error thrown for an unsuccessful response.

## Displaying the Text on a Page

Use `textContent` when the loaded data should appear as plain text:

```html
<p id="message">Loading...</p>
```

```js
async function loadMessage() {
  const messageElement = document.querySelector("#message");

  try {
    const response = await fetch("./message.txt");

    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`);
    }

    const text = await response.text();
    messageElement.textContent = text;
  } catch (error) {
    messageElement.textContent = "Could not load the message.";
    console.error(error);
  }
}

loadMessage();
```

`textContent` displays the data as text. It does not interpret it as HTML.

## Text vs. JSON

Use `response.text()` for plain text and `response.json()` for JSON. A response body is generally consumed once, so choose the method that matches its content. The next chapter explains JSON responses in detail.

## File Paths and Local Development

`fetch("./message.txt")` uses a path relative to the page's URL. Make sure the file is available at that URL.

Run your project through a local development server. Fetching local files from a page opened directly with a `file://` URL may be blocked by the browser.

Requests to another website may also be restricted by the browser's **CORS** rules. The server must allow your webpage's origin to read its response.

## Reading a File Chosen by the User

If the user selects a local text file through an `<input>`, you can read the selected `File` with its `.text()` method:

```html
<input id="file-input" type="file" accept=".txt,text/plain" />
<pre id="output"></pre>
```

```js
const fileInput = document.querySelector("#file-input");
const output = document.querySelector("#output");

fileInput.addEventListener("change", async () => {
  const file = fileInput.files[0];

  if (!file) {
    return;
  }

  try {
    const text = await file.text();
    output.textContent = text;
  } catch (error) {
    output.textContent = "Could not read the file.";
    console.error(error);
  }
});
```

Use `fetch()` for a resource available at a URL. Use `file.text()` for a file the user selected.

## Key Takeaway

To load text from a URL, await `fetch()`, check `response.ok`, and then await `response.text()`. The result is a string. To read a user-selected file, await the file's `.text()` method.

# Handling JSON Data in JavaScript

**JSON** is a text format commonly used to exchange data between a webpage and a server. When you request JSON with `fetch()`, you can use `response.json()` to parse it into a JavaScript value.

That value is often an object or array.

## Example JSON File

Suppose `student.json` contains:

```json
{
  "name": "Jadd",
  "major": "Computer Science",
  "courses": ["JavaScript", "React"]
}
```

You can fetch and use it:

```js
async function loadStudent() {
  const response = await fetch("./student.json");
  const student = await response.json();

  console.log(student.name); // "Jadd"
  console.log(student.courses[0]); // "JavaScript"
}

loadStudent();
```

`response.json()` returns a **Promise**, so it needs `await`. The result is a JavaScript object that you can access with normal property syntax.

As with text responses, check `response.ok` before reading the body and handle network or parsing errors.

## JSON Can Contain an Array

A JSON response does not have to be one object. For example, `students.json` could contain:

```json
[
  { "name": "Jadd", "grade": 85 },
  { "name": "Alex", "grade": 92 }
]
```

After parsing, you can use array methods:

```js
async function loadStudents() {
  const response = await fetch("./students.json");

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }

  const students = await response.json();

  students.forEach((student) => {
    console.log(`${student.name}: ${student.grade}`);
  });
}

loadStudents().catch((error) => {
  console.error(error);
});
```

Output:

```text
Jadd: 85
Alex: 92
```

## Displaying JSON Data on a Page

After parsing the JSON, create DOM elements to show its values:

```html
<ul id="student-list"></ul>
```

```js
async function showStudents() {
  const list = document.querySelector("#student-list");

  try {
    const response = await fetch("./students.json");

    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`);
    }

    const students = await response.json();

    students.forEach((student) => {
      const item = document.createElement("li");
      item.textContent = `${student.name}: ${student.grade}`;
      list.append(item);
    });
  } catch (error) {
    list.textContent = "Could not load students.";
    console.error(error);
  }
}

showStudents();
```

Use `textContent` for values from outside your code so they are displayed as text rather than interpreted as HTML.

## `response.json()` vs. `JSON.parse()`

Both parse JSON, but they start with different inputs:

```js
const data = await response.json();
```

`response.json()` reads the body of a **Fetch Response** and parses its JSON.

```js
const data = JSON.parse('{"name":"Jadd"}');
```

`JSON.parse()` parses a **JSON string you already have**.

You generally should not do this:

```js
const data = JSON.parse(await response.json()); // Incorrect
```

`response.json()` has already parsed the text.

## Sending JSON to a Server

To send a JavaScript object as JSON, convert it with `JSON.stringify()`:

```js
const newStudent = {
  name: "Jadd",
  grade: 85,
};

const response = await fetch("/api/students", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(newStudent),
});

if (!response.ok) {
  throw new Error(`Request failed: ${response.status}`);
}
```

`JSON.stringify()` turns the object into JSON text for the request body. The `"Content-Type"` header tells the server that the body contains JSON.

The server may return JSON as well, but only call `response.json()` if the response actually has a JSON body.

## Validating External Data

A successful response might still lack a property your code expects. Check important values when the data comes from an external source:

```js
const student = await response.json();

if (typeof student.name !== "string") {
  throw new Error("Student data is missing a name.");
}
```

## Key Takeaway

Use `await response.json()` to turn a JSON response into a JavaScript value. Check `response.ok`, handle parsing and network errors, and use `JSON.stringify()` when sending a JavaScript value as JSON in a request.

# Fetching Data from APIs in JavaScript

An **API** lets one program request data or actions from another. In a webpage, you can use the **Fetch API** to send an HTTP request to an API endpoint.

```js
const response = await fetch("/api/courses");
```

`fetch()` returns a Promise for a **Response** object. You then check whether the request succeeded and read its body.

The previous two chapters cover GET requests, `response.ok`, and reading text or JSON bodies. This chapter focuses on patterns specific to working with APIs.

## Handling Loading, Success, and Failure

A webpage often needs to show what is happening while data loads:

```html
<p id="status">Ready</p>
<ul id="course-list"></ul>
```

```js
async function showCourses() {
  const status = document.querySelector("#status");
  const list = document.querySelector("#course-list");

  status.textContent = "Loading courses...";

  try {
    const response = await fetch("/api/courses");

    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`);
    }

    const courses = await response.json();

    if (!Array.isArray(courses)) {
      throw new Error("Expected an array of courses.");
    }

    list.replaceChildren();

    courses.forEach((course) => {
      const item = document.createElement("li");
      item.textContent = course.name;
      list.append(item);
    });

    status.textContent = `Loaded ${courses.length} courses.`;
  } catch (error) {
    status.textContent = "Could not load courses.";
    console.error(error);
  }
}

showCourses();
```

This example expects the API to return an array like:

```json
[{ "name": "JavaScript" }, { "name": "React" }]
```

The API's actual response format determines how you access its data.

## Sending Query Parameters

APIs often accept options in the URL, such as a search term or page number. Use `URL` and `URLSearchParams` to build them safely:

```js
const url = new URL("/api/courses", window.location.origin);

url.searchParams.set("search", "computer science");
url.searchParams.set("page", "2");

const response = await fetch(url);
```

The request URL includes encoded query parameters, such as `search=computer+science&page=2`.

Sending JSON with POST is covered in [Handling JSON Data](#sending-json-to-a-server). Remember that a successful response may have an empty body, so only parse a body when the API documents one.

## CORS and API Access

A browser may restrict requests to an API on a different origin through **CORS**. The API server must allow your webpage's origin to read its response. Changing JavaScript code alone cannot grant that permission.

Also, do not place secret API keys in browser JavaScript. People can inspect code and requests sent by their browser. Requests requiring a secret key should be made through your own server.

## Key Takeaway

Use `fetch()` to request API data, check `response.ok`, and read the response with `.json()` or `.text()`. Handle network and parsing errors, and follow the API's expected URL, method, headers, request body, and response format.

# Error Handling in JavaScript

**Error handling** lets your program respond when something goes wrong. Instead of letting an error end an operation without explanation, you can catch it, show a useful message, or take another action.

JavaScript commonly uses `try`, `catch`, `finally`, and `throw` for this.

## `try...catch`

Put code that might throw an error inside `try`. If it throws, the `catch` block runs:

```js
try {
  const data = JSON.parse('{"name":"Jadd"}');
  console.log(data.name);
} catch (error) {
  console.error("Could not parse the data:", error);
}
```

If the code succeeds, `catch` is skipped.

Here is an invalid JSON example:

```js
try {
  const data = JSON.parse("{invalid json}");
  console.log(data);
} catch (error) {
  console.error("The JSON is invalid.");
}
```

When `JSON.parse()` throws, JavaScript stops executing the remaining statements in `try` and moves to `catch`.

## The Error Object

The `catch` parameter contains the value that was thrown. JavaScript errors usually have a `message` property:

```js
try {
  JSON.parse("{invalid json}");
} catch (error) {
  console.log(error.message);
}
```

You can choose a different parameter name, but `error` is descriptive.

## Throwing Your Own Error

Use `throw` when your code detects a problem it cannot handle normally:

```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero.");
  }

  return a / b;
}
```

The caller can handle the error:

```js
try {
  const result = divide(10, 0);
  console.log(result);
} catch (error) {
  console.error(error.message); // "Cannot divide by zero."
}
```

Although JavaScript lets you throw other values, using an `Error` object provides a message and useful debugging information.

## Different Error Types

JavaScript has built-in error types:

| Error type       | Common meaning                                  |
| ---------------- | ----------------------------------------------- |
| `Error`          | A general error                                 |
| `TypeError`      | A value was used in an incompatible way         |
| `ReferenceError` | Code refers to a variable that is not available |
| `SyntaxError`    | Code or parsed text has invalid syntax          |
| `RangeError`     | A value is outside an allowed range             |

You can create a more specific error:

```js
function setAge(age) {
  if (!Number.isInteger(age) || age < 0) {
    throw new RangeError("Age must be a nonnegative integer.");
  }

  return age;
}
```

## The `finally` Block

`finally` runs after `try` and any applicable `catch`, whether the operation succeeds or throws:

```js
try {
  console.log("Start operation");
  JSON.parse("{invalid json}");
} catch (error) {
  console.log("Operation failed");
} finally {
  console.log("Operation finished");
}
```

Output:

```text
Start operation
Operation failed
Operation finished
```

Use `finally` for cleanup or status changes that must happen after either outcome.

## Errors in Asynchronous Code

Use `try...catch` around awaited Promises, or attach `.catch()` to a Promise chain. An error thrown inside an async function or `.then()` callback becomes a rejected Promise. The Promises, async-functions, and fetch chapters show both patterns in context.

## A Synchronous `try...catch` Does Not Catch Later Timer Errors

This does **not** catch the error inside the timer callback:

```js
try {
  setTimeout(() => {
    throw new Error("Timer failed");
  }, 1000);
} catch (error) {
  console.error("Caught:", error);
}
```

The `try` block finishes before the callback runs. Put the `try...catch` **inside** the callback:

```js
setTimeout(() => {
  try {
    throw new Error("Timer failed");
  } catch (error) {
    console.error("Caught:", error.message);
  }
}, 1000);
```

## Handle Errors Where You Can Respond Usefully

Catching an error should lead to a useful action, such as displaying a message, retrying an operation, or reporting it:

```js
try {
  const settings = JSON.parse(savedSettings);
  applySettings(settings);
} catch (error) {
  console.error("Could not restore settings:", error);
  useDefaultSettings();
}
```

An empty `catch` block hides the problem and can make debugging difficult:

```js
try {
  doSomething();
} catch (error) {
  // Nothing happens, so failures are hard to find.
}
```

## Key Takeaway

Use `try...catch` to handle thrown errors, `throw new Error(...)` to signal a problem, and `finally` for work that must run after either outcome. For asynchronous code, handle errors where the Promise is awaited or with `.catch()`.
