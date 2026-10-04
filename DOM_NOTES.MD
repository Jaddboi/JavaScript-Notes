# Table of Contents

- [Accessing DOM Elements](#accessing-dom-elements-in-javascript)
- [`innerHTML`](#innerhtml-in-javascript)
- [Managing CSS Classes](#managing-css-classes-with-javascript)
- [Working with HTML Attributes](#working-with-html-attributes-in-javascript)
- [Navigating Siblings](#navigating-siblings-in-the-dom)
- [Modifying Styles](#modifying-styles-with-javascript)
- [Creating DOM Elements](#creating-dom-elements-with-javascript)
- [Removing DOM Elements](#removing-dom-elements-with-javascript)
- [DOM Events](#dom-events-in-javascript)
- [Keyboard Events](#keyboard-events-in-javascript)

# Accessing DOM Elements in JavaScript

The **Document Object Model (DOM)** is JavaScript's representation of an HTML page. Each HTML element becomes an object that JavaScript can find and work with.

## What Is `document`?

In a browser, `document` represents the current webpage. You use its methods to find HTML elements:

```js
document.getElementById("page-title");
document.querySelector(".card");
document.querySelectorAll("button");
```

## Selecting an Element by ID

Use `getElementById()` to find an element by its `id`:

```html
<h1 id="page-title">Welcome</h1>
```

```js
const heading = document.getElementById("page-title");

console.log(heading);
```

Write the ID without `#`:

```js
document.getElementById("page-title"); // Correct
document.getElementById("#page-title"); // Does not find this ID
```

An ID should identify one element on the page.

## Selecting the First Match with `querySelector()`

`querySelector()` finds the **first element** that matches a CSS selector:

```html
<p class="message">First message</p>
<p class="message">Second message</p>
```

```js
const message = document.querySelector(".message");

console.log(message.textContent); // "First message"
```

It uses the same selector syntax you use in CSS:

```js
document.querySelector("#page-title"); // Element with this ID
document.querySelector(".message"); // First element with this class
document.querySelector("button"); // First button
document.querySelector(".card p"); // First paragraph inside a .card
```

Unlike `getElementById()`, you **do** include `#` when selecting an ID with `querySelector()`.

## Selecting All Matches with `querySelectorAll()`

`querySelectorAll()` returns a **NodeList** containing every element that matches the selector:

```html
<button class="action">Save</button> <button class="action">Cancel</button>
```

```js
const buttons = document.querySelectorAll(".action");

console.log(buttons.length); // 2
console.log(buttons[0]); // The Save button
```

You can loop through the results:

```js
buttons.forEach((button) => {
  console.log(button.textContent);
});
```

Output:

```text
Save
Cancel
```

`querySelectorAll()` returns an empty NodeList if nothing matches.

## Other Selection Methods

You may also see these methods:

```js
document.getElementsByClassName("action");
document.getElementsByTagName("button");
```

They return **HTMLCollections**, which update automatically as matching elements are added or removed from the page. In contrast, the NodeList returned by `querySelectorAll()` does not automatically add new matches.

For many tasks, `querySelector()` and `querySelectorAll()` are convenient because they accept CSS selectors.

## Searching Within an Element

You can select an element first, then search inside it:

```html
<section class="profile">
  <h2>Jadd</h2>
  <p>Computer Science student</p>
</section>
```

```js
const profile = document.querySelector(".profile");
const description = profile.querySelector("p");

console.log(description.textContent);
// "Computer Science student"
```

This searches for a paragraph **inside `profile`**, rather than searching the whole document.

## What If No Element Matches?

Methods that select one element return `null` when they find nothing:

```js
const button = document.querySelector("#missing-button");

console.log(button); // null
```

Trying to use a property on `null` causes an error:

```js
button.textContent = "Click me"; // Error
```

Check that the element exists first:

```js
if (button) {
  button.textContent = "Click me";
}
```

## Make Sure the HTML Has Loaded

If your script runs before the browser has read an element, it cannot find that element yet.

A simple solution is to use `defer` when linking your JavaScript file:

```html
<head>
  <script src="script.js" defer></script>
</head>
```

With `defer`, the script runs after the HTML has been parsed:

```html
<body>
  <button id="save-button">Save</button>
</body>
```

```js
// script.js
const saveButton = document.querySelector("#save-button");

console.log(saveButton); // The button element
```

## Choosing a Method

| Method                         | What it selects            | If nothing matches |
| ------------------------------ | -------------------------- | ------------------ |
| `getElementById("id")`         | The element with that ID   | `null`             |
| `querySelector("selector")`    | The first matching element | `null`             |
| `querySelectorAll("selector")` | All matching elements      | Empty NodeList     |

## Key Takeaway

Use `document` to access elements in a webpage. `querySelector()` finds the first element matching a CSS selector, while `querySelectorAll()` finds all matches. Check for `null` when selecting one element, and make sure the HTML has loaded before your script runs.

# `innerHTML` in JavaScript

The **`innerHTML`** property lets you read or replace the HTML **inside** a DOM element.

```html
<div id="message">
  <strong>Hello!</strong>
</div>
```

```js
const message = document.querySelector("#message");

console.log(message.innerHTML); // Includes "<strong>Hello!</strong>"
```

Unlike `textContent`, `innerHTML` works with HTML markup as well as text.

## Setting `innerHTML`

Assign an HTML string to create elements inside a selected element:

```html
<div id="message"></div>
```

```js
const message = document.querySelector("#message");

message.innerHTML = "<strong>Hello, Jadd!</strong>";
```

The browser parses the string and creates a `<strong>` element:

```html
<div id="message">
  <strong>Hello, Jadd!</strong>
</div>
```

## Creating Several Elements

You can use a template string to write longer HTML:

```js
const card = document.querySelector("#profile-card");

card.innerHTML = `
  <h2>Jadd</h2>
  <p>Computer Science student</p>
  <button>View profile</button>
`;
```

Template strings make multiline HTML easier to read.

## `innerHTML` vs. `textContent`

These properties treat the same string differently:

```js
const message = document.querySelector("#message");

message.textContent = "<strong>Hello!</strong>";
```

`textContent` displays the characters `<strong>Hello!</strong>` as plain text.

```js
message.innerHTML = "<strong>Hello!</strong>";
```

`innerHTML` creates a `<strong>` element, so **Hello!** appears bold.

| Property      | Treats `<strong>Hello!</strong>` as... |
| ------------- | -------------------------------------- |
| `textContent` | Plain text                             |
| `innerHTML`   | HTML markup                            |

## Setting `innerHTML` Replaces Existing Content

Assigning to `innerHTML` removes the element's current children and replaces them with the new content:

```html
<ul id="tasks">
  <li>Study</li>
</ul>
```

```js
const tasks = document.querySelector("#tasks");

tasks.innerHTML = "<li>Exercise</li>";
```

The list now contains **Exercise**; **Study** has been removed.

If JavaScript references or event listeners were attached to elements inside the list, replacing those elements also removes them from the page.

## Adding HTML Without Replacing Existing Children

Use `insertAdjacentHTML()` to insert trusted HTML at a specific position:

```js
const tasks = document.querySelector("#tasks");

tasks.insertAdjacentHTML("beforeend", "<li>Cook</li>");
```

`"beforeend"` inserts the new element at the end of `tasks`, inside the `<ul>`.

You may see code that uses `innerHTML +=` to add content:

```js
tasks.innerHTML += "<li>Cook</li>";
```

Be careful: this reads and rebuilds all the existing child HTML, which can remove event listeners attached to those child elements.

## Keep Untrusted Text Out of `innerHTML`

Do **not** insert text supplied by a user directly into `innerHTML`:

```js
const username = '<img src="invalid" onerror="alert(1)">';

message.innerHTML = `Hello, ${username}`; // Unsafe
```

The browser interprets the string as HTML, which can allow unwanted code to run. This is called **cross-site scripting (XSS)**.

If the value should appear as text, use `textContent`:

```js
message.textContent = `Hello, ${username}`;
```

The browser will display the characters instead of interpreting them as markup.

If you need to combine HTML elements with untrusted text, create the elements and assign the text separately:

```js
const strong = document.createElement("strong");

strong.textContent = username;
message.replaceChildren("Hello, ", strong);
```

## Key Takeaway

`innerHTML` reads or replaces an element's inner HTML. Setting it parses markup and replaces existing child elements. Use `textContent` for plain or untrusted text.

# Managing CSS Classes with JavaScript

CSS classes control how elements look. JavaScript can add, remove, or change those classes to update a page in response to user actions.

```html
<p id="message" class="notice">Hello!</p>
```

```css
.notice {
  padding: 12px;
}

.highlight {
  background-color: yellow;
}
```

```js
const message = document.querySelector("#message");

message.classList.add("highlight");
```

The paragraph now has both classes:

```html
<p id="message" class="notice highlight">Hello!</p>
```

## What Is `classList`?

Every HTML element has a `classList` property. It provides methods for working with the element's CSS classes:

| Method        | Purpose                                       |
| ------------- | --------------------------------------------- |
| `.add()`      | Adds one or more classes                      |
| `.remove()`   | Removes one or more classes                   |
| `.toggle()`   | Adds a class if absent; removes it if present |
| `.contains()` | Checks whether a class is present             |
| `.replace()`  | Replaces one class with another               |

Pass a **class name**, without a leading dot, to these methods:

```js
message.classList.add("highlight"); // Correct
message.classList.add(".highlight"); // Incorrect for this purpose
```

The dot belongs in CSS selectors such as `document.querySelector(".highlight")`.

## Adding Classes

Use `.add()` to apply a class:

```js
const card = document.querySelector("#card");

card.classList.add("active");
```

You can add multiple classes at once:

```js
card.classList.add("active", "featured");
```

Adding a class that is already present does not create a duplicate.

## Removing Classes

Use `.remove()` to remove a class:

```js
card.classList.remove("active");
```

You can remove multiple classes:

```js
card.classList.remove("active", "featured");
```

Removing a class that is not present does not cause an error.

## Toggling a Class

Use `.toggle()` when an action should switch a class on or off:

```html
<button id="theme-button">Toggle dark mode</button>
```

```css
.dark-mode {
  background-color: #222;
  color: white;
}
```

```js
const button = document.querySelector("#theme-button");

button.addEventListener("click", () => {
  document.body.classList.toggle("dark-mode");
});
```

The first click adds `dark-mode`. The next click removes it.

`.toggle()` returns a boolean indicating whether the class is present **after** the call:

```js
const isDarkMode = document.body.classList.toggle("dark-mode");

console.log(isDarkMode);
```

You can also pass a second argument to set the class to a specific state:

```js
document.body.classList.toggle("dark-mode", true); // Ensure it is added
document.body.classList.toggle("dark-mode", false); // Ensure it is removed
```

## Checking for a Class

Use `.contains()` when your code needs to know whether a class is present:

```js
if (card.classList.contains("active")) {
  console.log("The card is active.");
}
```

It returns `true` or `false`.

## Replacing a Class

Use `.replace()` to swap one class for another:

```js
const status = document.querySelector("#status");

status.classList.replace("pending", "complete");
```

If `pending` is not present, nothing is replaced. The method returns `true` when it makes the replacement.

## `classList` vs. `className`

`className` is a string containing an element's classes:

```js
const card = document.querySelector("#card");

console.log(card.className);
```

Assigning to `className` **replaces all existing classes**:

```js
card.className = "active";
```

If the card previously had `class="card featured"`, those classes are removed. Use `classList.add()` when you want to keep existing classes:

```js
card.classList.add("active");
```

## Updating Several Elements

Select all matching elements, then update each one:

```html
<p class="notification">First message</p>
<p class="notification">Second message</p>
```

```js
const notifications = document.querySelectorAll(".notification");

notifications.forEach((notification) => {
  notification.classList.add("visible");
});
```

## Key Takeaway

Use `element.classList` to manage individual CSS classes. `.add()` and `.remove()` make specific changes, `.toggle()` switches a class on or off, and `.contains()` checks whether it is present.

# Working with HTML Attributes in JavaScript

**Attributes** provide information about an HTML element. They appear in the element's opening tag:

```html
<a id="profile-link" href="/profile" target="_blank">View profile</a>
```

This link has `id`, `href`, and `target` attributes. JavaScript can read, add, change, and remove them.

## Reading an Attribute

Use `getAttribute()` to read an attribute's value:

```html
<a id="profile-link" href="/profile">View profile</a>
```

```js
const link = document.querySelector("#profile-link");

console.log(link.getAttribute("href")); // "/profile"
```

If the attribute is missing, `getAttribute()` returns `null`:

```js
console.log(link.getAttribute("target")); // null
```

## Adding or Changing an Attribute

Use `setAttribute(name, value)`:

```js
link.setAttribute("href", "/settings");
link.setAttribute("target", "_blank");
```

If the attribute already exists, its value is replaced. If it does not exist, it is added.

## Checking Whether an Attribute Exists

Use `hasAttribute()` to get a boolean:

```js
console.log(link.hasAttribute("href")); // true
console.log(link.hasAttribute("target")); // true
```

This checks whether the attribute is present, regardless of its value.

## Removing an Attribute

Use `removeAttribute()` to remove it:

```js
link.removeAttribute("target");

console.log(link.hasAttribute("target")); // false
```

## Attributes vs. DOM Properties

Many HTML attributes have corresponding JavaScript **properties**:

```js
link.setAttribute("href", "/settings");
console.log(link.href);
```

However, an attribute and its property do not always return the same text. For example:

```html
<a id="profile-link" href="/profile">View profile</a>
```

```js
const link = document.querySelector("#profile-link");

console.log(link.getAttribute("href")); // "/profile"
console.log(link.href); // Full URL resolved by the browser
```

Use `getAttribute()` when you need the value written in the HTML attribute. Use the DOM property when you need the element's current JavaScript value.

## Boolean Attributes

Some attributes represent an on/off state. Their **presence** means the state is on, regardless of the text assigned to them.

For example, a button with a `disabled` attribute is disabled:

```html
<button id="save-button" disabled>Save</button>
```

The simplest way to control this in JavaScript is through the `.disabled` property:

```js
const button = document.querySelector("#save-button");

button.disabled = true; // Disable the button
button.disabled = false; // Enable the button
```

You can also add or remove the attribute:

```js
button.setAttribute("disabled", "");
button.removeAttribute("disabled");
```

**Be careful:** `setAttribute("disabled", "false")` still disables the button because the attribute is present.

## Form Input Values

For an input, the `.value` property gives you its **current value**:

```html
<input id="username" value="Jadd" />
```

```js
const input = document.querySelector("#username");

console.log(input.value);
```

If the user types a different name, `input.value` updates. `input.getAttribute("value")` usually still reflects the initial value specified by the HTML attribute.

Use `.value` when you want to read or change what is currently in the input:

```js
input.value = "Alex";
```

## Custom `data-*` Attributes

Custom `data-*` attributes let you store small pieces of information on an HTML element:

```html
<button id="course-button" data-course-id="csc369">Open course</button>
```

Read the attribute with `getAttribute()`:

```js
const button = document.querySelector("#course-button");

console.log(button.getAttribute("data-course-id")); // "csc369"
```

Or use the element's `.dataset` property:

```js
console.log(button.dataset.courseId); // "csc369"
```

With `.dataset`, a name such as `data-course-id` becomes `courseId`. Dataset values are strings:

```js
button.dataset.courseId = "csc343";

console.log(button.getAttribute("data-course-id")); // "csc343"
```

## Choosing the Right Tool

| Task                              | Common choice       |
| --------------------------------- | ------------------- |
| Read an HTML attribute            | `getAttribute()`    |
| Add or change an attribute        | `setAttribute()`    |
| Check whether an attribute exists | `hasAttribute()`    |
| Remove an attribute               | `removeAttribute()` |
| Read a user's current input       | `.value`            |
| Enable or disable a button        | `.disabled`         |
| Add or remove a CSS class         | `.classList`        |
| Work with a `data-*` attribute    | `.dataset`          |

## Key Takeaway

Use attribute methods to read and change HTML attributes. For current states such as an input's value or a button's disabled state, the matching DOM property is often more useful.

# Navigating Siblings in the DOM

**Sibling elements** are elements that share the same parent. JavaScript lets you move from one element to the element before or after it.

```html
<ul id="tasks">
  <li id="first-task">Study</li>
  <li id="second-task">Exercise</li>
  <li id="third-task">Cook</li>
</ul>
```

All three `<li>` elements are siblings because they have the same parent `<ul>`.

## Finding the Next Element Sibling

Use `.nextElementSibling` to get the next element under the same parent:

```js
const firstTask = document.querySelector("#first-task");

console.log(firstTask.nextElementSibling.textContent); // "Exercise"
```

If there is no next element sibling, the result is `null`:

```js
const thirdTask = document.querySelector("#third-task");

console.log(thirdTask.nextElementSibling); // null
```

## Finding the Previous Element Sibling

Use `.previousElementSibling` to get the element immediately before the selected element:

```js
const thirdTask = document.querySelector("#third-task");

console.log(thirdTask.previousElementSibling.textContent); // "Exercise"
```

The first element has no previous element sibling:

```js
const firstTask = document.querySelector("#first-task");

console.log(firstTask.previousElementSibling); // null
```

## Element Siblings vs. Node Siblings

The DOM contains different types of **nodes**, including elements, text, and comments.

Compare these properties:

| Property                  | What it returns                     |
| ------------------------- | ----------------------------------- |
| `.nextElementSibling`     | The next **element**, or `null`     |
| `.previousElementSibling` | The previous **element**, or `null` |
| `.nextSibling`            | The next **node**, or `null`        |
| `.previousSibling`        | The previous **node**, or `null`    |

In formatted HTML, spaces and line breaks between elements often become **text nodes**:

```html
<ul>
  <li id="first-task">Study</li>
  <li>Exercise</li>
</ul>
```

```js
const firstTask = document.querySelector("#first-task");

console.log(firstTask.nextElementSibling); // The Exercise <li>
console.log(firstTask.nextSibling); // Often a whitespace text node
```

When you want to move between HTML **elements**, use `nextElementSibling` and `previousElementSibling`.

## Checking Before Using a Sibling

A sibling property can return `null`, so check it before accessing its properties:

```js
const thirdTask = document.querySelector("#third-task");
const nextTask = thirdTask.nextElementSibling;

if (nextTask) {
  console.log(nextTask.textContent);
} else {
  console.log("There is no next task.");
}
```

You can also use optional chaining when you only need to read a value:

```js
console.log(thirdTask.nextElementSibling?.textContent);
// undefined
```

## Moving Through Multiple Siblings

You can follow sibling links more than once:

```js
const firstTask = document.querySelector("#first-task");

const secondTask = firstTask.nextElementSibling;
const thirdTask = secondTask.nextElementSibling;

console.log(thirdTask.textContent); // "Cook"
```

For an unknown number of siblings, use a loop:

```js
let currentTask = document.querySelector("#first-task");

while (currentTask !== null) {
  console.log(currentTask.textContent);
  currentTask = currentTask.nextElementSibling;
}
```

Output:

```text
Study
Exercise
Cook
```

## Going to the Parent

A sibling is another element under the **same parent**. You can access that parent with `.parentElement`:

```js
const secondTask = document.querySelector("#second-task");

console.log(secondTask.parentElement.id); // "tasks"
```

If you need all element children of that parent, use `.children`:

```js
const taskList = secondTask.parentElement;

console.log(taskList.children.length); // 3
```

## Key Takeaway

Use `.nextElementSibling` and `.previousElementSibling` to move between elements that share a parent. They return `null` at the beginning or end, so check the result before using it.

# Modifying Styles with JavaScript

JavaScript can change an element's CSS using its **`.style`** property. This adds or updates an **inline style** on that element.

## Setting Style Properties

Assign a CSS value to a property on `element.style`:

```js
const message = document.querySelector("#message");

message.style.color = "purple";
message.style.backgroundColor = "lavender";
message.style.fontSize = "20px";
```

CSS property names with hyphens become **camelCase** in JavaScript:

| CSS                | JavaScript        |
| ------------------ | ----------------- |
| `background-color` | `backgroundColor` |
| `font-size`        | `fontSize`        |
| `border-radius`    | `borderRadius`    |
| `margin-top`       | `marginTop`       |

Properties without hyphens, such as `color`, keep the same name.

## Include Units When Needed

Many CSS values need a unit:

```js
message.style.fontSize = "20px";
message.style.marginTop = "1rem";
message.style.width = "50%";
```

For these properties, assigning `"20"` without a unit will not give you the intended `20px` style. Some CSS properties, such as `opacity`, use unitless values:

```js
message.style.opacity = "0.5";
```

## Using Variables to Set Styles

You can build a style value with a template string:

```js
const fontSize = 24;

message.style.fontSize = `${fontSize}px`;
```

This is useful when a style depends on a calculation or user action.

## Removing an Inline Style

Assign an empty string to remove a style set through `.style`:

```js
message.style.color = "purple";
message.style.color = "";
```

After the inline `color` is removed, the element can use a color from a CSS rule or an inherited style.

You can also use `.removeProperty()` with the CSS property name:

```js
message.style.removeProperty("background-color");
```

## Reading Styles

`element.style` reads **inline styles**. It does not necessarily show a value applied by a CSS class or stylesheet.

```html
<p id="message" class="notice">Hello!</p>
```

```css
.notice {
  color: purple;
}
```

```js
const message = document.querySelector("#message");

console.log(message.style.color); // ""
```

The inline `color` is empty even though the text appears purple.

Use `getComputedStyle()` to read the style the browser has calculated for an element:

```js
const styles = getComputedStyle(message);

console.log(styles.color); // The computed color value
```

The browser may return colors in a format such as `rgb(...)`, even if your CSS used a named color.

## Using `setProperty()`

`.setProperty()` lets you use a CSS property name exactly as it appears in CSS:

```js
message.style.setProperty("background-color", "lavender");
message.style.setProperty("font-size", "20px");
```

It is especially useful for CSS custom properties:

```css
.card {
  background-color: var(--card-color);
}
```

```js
const card = document.querySelector(".card");

card.style.setProperty("--card-color", "lavender");
```

## Inline Styles vs. CSS Classes

For a single value that changes dynamically, `.style` is convenient:

```js
const progress = 70;

progressBar.style.width = `${progress}%`;
```

For a group of related style changes, define a CSS class and toggle it:

```css
.active {
  background-color: purple;
  color: white;
  border-radius: 8px;
}
```

```js
const button = document.querySelector("#action-button");

button.classList.toggle("active");
```

A class keeps the visual rules together in CSS. Also remember that an inline style often takes precedence over a style from an ordinary CSS class:

```js
button.style.color = "red";
button.classList.add("active"); // The inline red color may still be used
```

## Key Takeaway

Use `element.style` to set inline CSS values. Write hyphenated CSS properties in camelCase, include units where required, and use `getComputedStyle()` when you need to read styles applied by CSS rules.

# Creating DOM Elements with JavaScript

JavaScript can create new HTML elements and add them to a webpage. This is useful when displaying new messages, tasks, search results, or other content that changes while the page is open.

## Creating an Element

Use `document.createElement()` with an HTML tag name:

```js
const paragraph = document.createElement("p");
```

This creates a `<p>` element, but it does **not** put it on the page yet. The element exists in JavaScript until you insert it into the DOM.

## Adding Text and Attributes

You can set up the element before inserting it:

```js
const paragraph = document.createElement("p");

paragraph.textContent = "Hello, Jadd!";
paragraph.classList.add("message");
paragraph.id = "welcome-message";
```

This prepares an element equivalent to:

```html
<p id="welcome-message" class="message">Hello, Jadd!</p>
```

Use `textContent` for plain text, especially text that may come from a user.

## Adding an Element to the Page

Suppose the HTML contains a container:

```html
<div id="messages"></div>
```

Select the container and use `.append()` to add the new element at the end:

```js
const messages = document.querySelector("#messages");
const paragraph = document.createElement("p");

paragraph.textContent = "Hello, Jadd!";

messages.append(paragraph);
```

The page now contains:

```html
<div id="messages">
  <p>Hello, Jadd!</p>
</div>
```

You can also use `.appendChild()`:

```js
messages.appendChild(paragraph);
```

Both methods add a node at the end. `.append()` can also add text and accept multiple items:

```js
messages.append("New message: ", paragraph);
```

## Adding an Element at the Beginning

Use `.prepend()` to insert an element as the container's first child:

```js
const messages = document.querySelector("#messages");
const paragraph = document.createElement("p");

paragraph.textContent = "Newest message";

messages.prepend(paragraph);
```

| Method                       | Where it adds content      |
| ---------------------------- | -------------------------- |
| `container.append(element)`  | End of the container       |
| `container.prepend(element)` | Beginning of the container |

## Adding an Element Beside Another Element

Use `.before()` or `.after()` to insert an element next to an existing element:

```html
<ul id="tasks">
  <li id="exercise-task">Exercise</li>
</ul>
```

```js
const exerciseTask = document.querySelector("#exercise-task");

const studyTask = document.createElement("li");
studyTask.textContent = "Study";

exerciseTask.before(studyTask);
```

The result is:

```html
<ul id="tasks">
  <li>Study</li>
  <li id="exercise-task">Exercise</li>
</ul>
```

Using `exerciseTask.after(studyTask)` would place the new item after **Exercise**.

## Example: Add an Item to a List

```html
<ul id="task-list"></ul>
<button id="add-task-button">Add task</button>
```

```js
const taskList = document.querySelector("#task-list");
const addButton = document.querySelector("#add-task-button");

addButton.addEventListener("click", () => {
  const task = document.createElement("li");

  task.textContent = "Study JavaScript";
  task.classList.add("task");

  taskList.append(task);
});
```

Each click creates a **new** `<li>` and adds it to the list.

## Creating Nested Elements

Create each element, assemble them, and then insert the result:

```js
const card = document.createElement("article");
const heading = document.createElement("h2");
const description = document.createElement("p");

heading.textContent = "JavaScript";
description.textContent = "Practice DOM manipulation.";

card.classList.add("card");
card.append(heading, description);

document.body.append(card);
```

The result is:

```html
<article class="card">
  <h2>JavaScript</h2>
  <p>Practice DOM manipulation.</p>
</article>
```

## An Existing Element Moves When Inserted Again

Inserting the **same element** somewhere else moves it; it does not create a second copy:

```js
const paragraph = document.createElement("p");
paragraph.textContent = "Hello!";

firstContainer.append(paragraph);
secondContainer.append(paragraph);
```

After the second call, `paragraph` is inside `secondContainer`. If you need two elements, call `document.createElement()` twice or clone the first element.

## Key Takeaway

Use `document.createElement()` to make an element, set its text and attributes, then insert it with methods such as `.append()`, `.prepend()`, `.before()`, or `.after()`. Creating an element alone does not display it on the page.

# Removing DOM Elements with JavaScript

JavaScript can remove elements from a webpage after they are no longer needed. The most direct way is the element's **`.remove()`** method.

## Removing One Element

```html
<p id="message">This message will be removed.</p>
```

```js
const message = document.querySelector("#message");

message.remove();
```

The `<p>` element is removed from the page.

As explained in [Accessing DOM Elements](#what-if-no-element-matches), a selector may return `null`. Optional chaining lets you remove an element only when it exists:

```js
document.querySelector("#message")?.remove();
```

## Removing an Element After a Click

```html
<p id="notice">Your changes have been saved.</p>
<button id="dismiss-button">Dismiss</button>
```

```js
const notice = document.querySelector("#notice");
const dismissButton = document.querySelector("#dismiss-button");

dismissButton.addEventListener("click", () => {
  notice.remove();
});
```

Clicking **Dismiss** removes the notice from the page.

## Removing a Child Through Its Parent

You may also see `.removeChild()`. This method is called on the **parent**, with the child you want to remove:

```html
<ul id="tasks">
  <li id="old-task">Old task</li>
</ul>
```

```js
const taskList = document.querySelector("#tasks");
const oldTask = document.querySelector("#old-task");

taskList.removeChild(oldTask);
```

For most simple cases, calling `oldTask.remove()` is shorter. `.removeChild()` is useful when you are already working with the parent. The child must belong to that parent.

## Removing All Children

Use `.replaceChildren()` with no arguments to remove every child node from an element:

```html
<ul id="tasks">
  <li>Study</li>
  <li>Exercise</li>
</ul>
```

```js
const taskList = document.querySelector("#tasks");

taskList.replaceChildren();
```

The `<ul>` remains on the page, but both `<li>` elements are removed.

You can also replace the children with new content:

```js
const newTask = document.createElement("li");
newTask.textContent = "Cook";

taskList.replaceChildren(newTask);
```

Now the list contains only **Cook**.

## Removing All Matching Elements

Use `querySelectorAll()` to find several elements, then remove each one:

```html
<p class="notification">First notification</p>
<p class="notification">Second notification</p>
```

```js
const notifications = document.querySelectorAll(".notification");

notifications.forEach((notification) => {
  notification.remove();
});
```

`querySelectorAll()` returns a static NodeList, so removing the elements while looping through that list works as expected.

## What Happens to the JavaScript Variable?

Removing an element from the DOM does **not** automatically erase a variable that refers to it:

```js
const message = document.querySelector("#message");

message.remove();

console.log(message); // Still refers to the detached element
```

You can insert that same element again:

```js
document.body.append(message);
```

It reappears on the page. Calling `.remove()` detaches an element; it does not destroy the JavaScript object immediately.

## Removing an Element vs. Hiding It

Removing an element takes it out of the DOM:

```js
message.remove();
```

Hiding an element keeps it in the DOM but prevents it from appearing:

```js
message.style.display = "none";
```

If you plan to show the same element again, hiding it or toggling a CSS class may be more convenient.

## Key Takeaway

Use `.remove()` to detach one element and `.replaceChildren()` to clear a container. Check that a selected element exists before removing it, and remember that a JavaScript variable can still refer to an element after it leaves the DOM.

# DOM Events in JavaScript

A **DOM event** is something that happens in a webpage, such as a button click, a key press, or a form submission. JavaScript can listen for events and run code in response.

For example, you can change a message when someone clicks a button:

```html
<button id="greet-button">Greet me</button>
<p id="message"></p>
```

```js
const button = document.querySelector("#greet-button");
const message = document.querySelector("#message");

button.addEventListener("click", () => {
  message.textContent = "Hello, Jadd!";
});
```

## Using `addEventListener()`

The basic syntax is:

```js
element.addEventListener("eventName", callback);
```

- `element` is the DOM element you want to watch.
- `"eventName"` is the event type, such as `"click"`.
- `callback` is the function that runs when the event occurs.

The callback does not run when you add the listener. It runs later, when the event happens.

## Common DOM Events

| Event       | When it happens                                     |
| ----------- | --------------------------------------------------- |
| `click`     | An element is clicked                               |
| `input`     | A form control's value changes as the user edits it |
| `change`    | A form control's change is committed                |
| `submit`    | A form is submitted                                 |
| `keydown`   | A key is pressed                                    |
| `mouseover` | The pointer moves onto an element                   |
| `focus`     | An element receives focus                           |
| `blur`      | An element loses focus                              |

### Input Example

The `input` event is useful when you want to react as someone types:

```html
<input id="name-input" type="text" />
<p id="preview"></p>
```

```js
const input = document.querySelector("#name-input");
const preview = document.querySelector("#preview");

input.addEventListener("input", () => {
  preview.textContent = input.value;
});
```

For text inputs, `input` runs as the user types. The `change` event usually runs when the edited value is committed, such as when the input loses focus.

## The Event Object

When an event occurs, the browser passes an **event object** to the callback. It contains information about what happened:

```js
const button = document.querySelector("#save-button");

button.addEventListener("click", (event) => {
  console.log(event.type); // "click"
  console.log(event.target); // Element where the event originated
});
```

You can name the parameter `event` or use another name such as `e`.

### `target` vs. `currentTarget`

- `event.target` is the element where the event originated.
- `event.currentTarget` is the element whose listener is currently running.

They can differ when a child element inside the listening element is clicked:

```html
<button id="save-button">
  <span>Save</span>
</button>
```

```js
const button = document.querySelector("#save-button");

button.addEventListener("click", (event) => {
  console.log(event.currentTarget); // The button

  // If the span was clicked, event.target may be the span.
  console.log(event.target);
});
```

## Preventing a Default Action

Some events have a browser action associated with them. For example, submitting a form normally navigates or reloads the page.

Use `event.preventDefault()` when you want to handle the submission with JavaScript:

```html
<form id="signup-form">
  <input name="username" required />
  <button type="submit">Sign up</button>
</form>
```

```js
const form = document.querySelector("#signup-form");

form.addEventListener("submit", (event) => {
  event.preventDefault();

  console.log("Form submitted with JavaScript.");
});
```

`preventDefault()` stops the browser's default action for that event. It does not stop your callback from running.

## Named and Removable Handlers

You can define the callback separately:

```js
function handleClick() {
  console.log("Button clicked!");
}

const button = document.querySelector("#save-button");

button.addEventListener("click", handleClick);
```

Pass `handleClick` without parentheses. Writing `handleClick()` would call it immediately and pass its result instead.

Use `removeEventListener()` when you no longer want the listener to run. Pass the same event name and function reference:

```js
button.removeEventListener("click", handleClick);
```

A new arrow function with identical code is still a different function, so it will not remove the original listener.

If a listener should run only once, use the `once` option:

```js
button.addEventListener("click", handleClick, { once: true });
```

## Adding Multiple Handlers

`addEventListener()` can register more than one handler for the same event. Each handler runs when the event occurs:

```js
button.addEventListener("click", () => console.log("First handler"));
button.addEventListener("click", () => console.log("Second handler"));
```

Assigning a function to `button.onclick` works, but a later assignment replaces the earlier handler. `addEventListener()` is generally more flexible.

## Event Bubbling

Many events **bubble** from the element where they begin through its ancestors:

```html
<div id="panel">
  <button id="action-button">Click me</button>
</div>
```

```js
const panel = document.querySelector("#panel");
const actionButton = document.querySelector("#action-button");

panel.addEventListener("click", () => console.log("Panel handler"));
actionButton.addEventListener("click", () => console.log("Button handler"));
```

Clicking the button runs its handler first and then the panel's handler.

## Event Delegation

Bubbling allows one parent listener to handle events from many children, including children added later. This pattern is called **event delegation**:

```html
<ul id="task-list">
  <li><button type="button" class="remove-task">Remove Study</button></li>
  <li><button type="button" class="remove-task">Remove Exercise</button></li>
</ul>
```

```js
const taskList = document.querySelector("#task-list");

taskList.addEventListener("click", (event) => {
  if (!(event.target instanceof Element)) {
    return;
  }

  const removeButton = event.target.closest(".remove-task");

  if (!removeButton || !taskList.contains(removeButton)) {
    return;
  }

  removeButton.closest("li").remove();
});
```

The listener is attached to the `<ul>`, rather than to each button. `closest()` also handles a click on an element nested inside a remove button.

## `preventDefault()` vs. `stopPropagation()`

These methods solve different problems:

- `event.preventDefault()` stops a browser action, such as normal form submission.
- `event.stopPropagation()` stops an event from continuing to ancestor handlers.

Neither method automatically does the other's job.

## Key Takeaway

DOM events let JavaScript respond to user actions. Use `addEventListener()` to register handlers, read the event object for details, and use event delegation when one parent should handle events from many children. `preventDefault()` stops a browser action, while `stopPropagation()` stops bubbling.

# Keyboard Events in JavaScript

**Keyboard events** let JavaScript respond when a user presses or releases a key. They are useful for shortcuts, games, and actions such as closing a dialog with Escape.

## `keydown` and `keyup`

The two main keyboard events are:

| Event     | When it fires          |
| --------- | ---------------------- |
| `keydown` | When a key is pressed  |
| `keyup`   | When a key is released |

```js
document.addEventListener("keydown", (event) => {
  console.log(`Pressed: ${event.key}`);
});

document.addEventListener("keyup", (event) => {
  console.log(`Released: ${event.key}`);
});
```

You may see `keypress` in older code. Use `keydown` or `keyup` for new code.

## Identifying the Key with `event.key`

`event.key` tells you what key the user pressed:

```js
document.addEventListener("keydown", (event) => {
  if (event.key === "Escape") {
    console.log("Escape was pressed.");
  }
});
```

Common values include:

| Key pressed | `event.key`                                    |
| ----------- | ---------------------------------------------- |
| Enter       | `"Enter"`                                      |
| Escape      | `"Escape"`                                     |
| Space       | `" "`                                          |
| Arrow up    | `"ArrowUp"`                                    |
| Arrow down  | `"ArrowDown"`                                  |
| Letter A    | Usually `"a"` or `"A"`, depending on modifiers |

The strings are case-sensitive. For example, `"Enter"` is different from `"enter"`.

## `event.key` vs. `event.code`

- `event.key` describes the **value or meaning** of the key press.
- `event.code` describes the **physical key position** on the keyboard.

```js
document.addEventListener("keydown", (event) => {
  console.log(event.key);
  console.log(event.code);
});
```

For example, pressing the A key on a typical English keyboard might produce `"a"` for `event.key` and `"KeyA"` for `event.code`. Keyboard layout and modifier keys can affect `event.key`.

Use `event.key` for actions such as “close when Escape is pressed.” Use `event.code` when the physical position matters, such as some game controls.

## Listening on an Input

You can attach a keyboard listener to a specific element:

```html
<input id="search-input" type="text" />
```

```js
const input = document.querySelector("#search-input");

input.addEventListener("keydown", (event) => {
  if (event.key === "Enter") {
    console.log(`Search for: ${input.value}`);
  }
});
```

The input must have focus for its keyboard listener to receive the event.

**Note:** In a `keydown` handler, the input's `.value` may not yet include the character from the current key press. Use the `input` event when you need the updated text as the user types.

## Modifier Keys

The event object tells you whether keys such as Control, Shift, Alt, or Command are held:

```js
document.addEventListener("keydown", (event) => {
  if (event.ctrlKey && event.key.toLowerCase() === "s") {
    console.log("Control + S was pressed.");
  }
});
```

| Property         | Modifier                                                    |
| ---------------- | ----------------------------------------------------------- |
| `event.ctrlKey`  | Control                                                     |
| `event.shiftKey` | Shift                                                       |
| `event.altKey`   | Alt / Option                                                |
| `event.metaKey`  | Command on macOS, or the system key on some other platforms |

For a shortcut that works with Control on Windows/Linux and Command on macOS:

```js
document.addEventListener("keydown", (event) => {
  if ((event.ctrlKey || event.metaKey) && event.key.toLowerCase() === "s") {
    event.preventDefault();
    console.log("Run the app's save action.");
  }
});
```

`preventDefault()` stops the browser's usual action for that shortcut. Use it only when your application provides an appropriate replacement.

## Holding a Key Down

When a key is held, `keydown` may fire repeatedly. Check `event.repeat` if you want to respond only to the first press:

```js
document.addEventListener("keydown", (event) => {
  if (event.key === "ArrowDown" && !event.repeat) {
    console.log("Arrow Down was pressed once.");
  }
});
```

## Finding Where the Event Started

`event.target` identifies the element that received the keyboard event:

```js
document.addEventListener("keydown", (event) => {
  console.log(event.target);
});
```

This can help you avoid triggering a page shortcut while someone is typing:

```js
document.addEventListener("keydown", (event) => {
  const isTyping =
    event.target instanceof HTMLElement &&
    (event.target.matches("input, textarea, select") ||
      event.target.isContentEditable);

  if (isTyping) {
    return;
  }

  if (event.key.toLowerCase() === "n") {
    console.log("Create a new item.");
  }
});
```

## Example: Close a Message with Escape

```html
<div id="message">Press Escape to close this message.</div>
```

```js
const message = document.querySelector("#message");

document.addEventListener("keydown", (event) => {
  if (event.key === "Escape") {
    message.remove();
  }
});
```

## Key Takeaway

Use `keydown` and `keyup` to respond to keyboard actions. Check `event.key` for the key's meaning, `event.code` for its physical position, and modifier properties for shortcuts. Use the `input` event when you need the current text in a form control.
