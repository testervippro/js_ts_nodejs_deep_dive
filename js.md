
# JavaScript: Hoisting

**Hoisting** is JavaScript’s default behavior of **moving declarations to the top** of the current scope (function or script).

---

## 🔼 What Gets Hoisted?

- **`var` declarations** are hoisted and initialized as `undefined`
- **`let` and `const` declarations** are hoisted **but not initialized**
- **Function declarations** are also hoisted (entire function definition)

---

## 📌 Example: `var` Hoisting

These two examples behave the same way due to hoisting:

### ✅ Example 1
```js
x = 5;
console.log(x); // 5
var x;
````

### ✅ Example 2

```js
var x;
x = 5;
console.log(x); // 5
```

Hoisting moves the `var x;` declaration to the top internally.

---

## 🚫 Hoisting & Initialization

Only **declarations** are hoisted – **initializations are not**.

### ⚠️ Example

```js
console.log(y); // undefined
var y = 7;
```

Internally interpreted as:

```js
var y;
console.log(y); // undefined
y = 7;
```

---

## ❌ `let` and `const` Hoisting

These are hoisted but **not initialized**, causing a **ReferenceError** if accessed before declaration.

### Example: `let`

```js
console.log(car); // ❌ ReferenceError
let car = 'Volvo';
```

The variable is in a **temporal dead zone** until the declaration is evaluated.

### Example: `const`

```js
car = 'Volvo';   // ❌ SyntaxError
const car;
```

---

## 🔍 Function Hoisting

Function declarations are hoisted completely.

```js
greet(); // "Hello!"

function greet() {
  console.log("Hello!");
}
```

But function **expressions** (assigned to variables) are only partially hoisted:

```js
sayHi(); // ❌ TypeError: sayHi is not a function

var sayHi = function() {
  console.log("Hi!");
};
```

---

## ✅ Best Practice

Always **declare variables at the top** of their scope to avoid hoisting confusion.

### Recommended:

```js
'use strict'; // Prevents use-before-declare

function myFunc() {
  let count = 0;
  const max = 10;

  // logic...
}
```

---

## 🧠 Summary Table

| Keyword    | Hoisted? | Initialized?     | Access Before Declare?          |
| ---------- | -------- | ---------------- | ------------------------------- |
| `var`      | ✅ Yes    | ✅ As `undefined` | ✅ Allowed (returns `undefined`) |
| `let`      | ✅ Yes    | ❌ No             | ❌ ReferenceError                |
| `const`    | ✅ Yes    | ❌ No             | ❌ ReferenceError / SyntaxError  |
| `function` | ✅ Yes    | ✅ Yes            | ✅ Allowed                       |


# JavaScript: `"use strict"` Directive

The `"use strict"` directive enables **strict mode**, which makes JavaScript behave more securely and predictably by **catching common coding mistakes**.

---

## 🚦 What is `"use strict"`?

- Introduced in **ECMAScript 5**
- A **string literal**, not a statement
- Enables **strict mode** which:
  - Disallows undeclared variables
  - Catches silent errors
  - Prevents unsafe actions

### ✅ Browser Support
| Browser       | Version Supporting `"use strict"` |
|---------------|------------------------------------|
| Chrome        | 13.0+                              |
| Firefox       | 4.0+                               |
| Safari        | 5.1+                               |
| Opera         | 12.1+                              |
| IE            | ❌ Not supported before IE 10      |

---

## 📌 Declaring Strict Mode

### Global Strict Mode
```js
"use strict";
x = 3.14; // ❌ Error: x is not declared
````

### Function Scope Strict Mode

```js
x = 3.14; // ✅ Allowed

function myFunction() {
  "use strict";
  y = 3.14; // ❌ Error: y is not declared
}
```

---

## ❗ Why Use Strict Mode?

* Helps write **cleaner** and **more secure** code
* Turns **silent errors into real errors**
* Prevents usage of unsafe syntax and reserved keywords
* Disallows creation of global variables accidentally

---

## 🚫 Things Not Allowed in Strict Mode

| Action                                      | Example                                | Error Type     |
| ------------------------------------------- | -------------------------------------- | -------------- |
| Undeclared variables                        | `x = 3.14`                             | ReferenceError |
| Undeclared objects                          | `x = {a:1}`                            | ReferenceError |
| Deleting variables or functions             | `delete x;` or `delete function x(){}` | SyntaxError    |
| Duplicate parameter names                   | `function(x, x){}`                     | SyntaxError    |
| Octal literals                              | `let x = 010;`                         | SyntaxError    |
| Octal escape sequences                      | `let x = "\010";`                      | SyntaxError    |
| Writing to read-only properties             | `obj.x = 3.14;` (if `writable: false`) | TypeError      |
| Using reserved future keywords as variables | `let public = 123;`                    | SyntaxError    |
| Using `eval` or `arguments` as identifiers  | `let eval = 1;`                        | SyntaxError    |
| Using the `with` statement                  | `with (obj) {...}`                     | SyntaxError    |
| `eval()` creating vars in calling scope     | `eval("var x = 2"); alert(x);`         | ReferenceError |

---

## 🧠 `"this"` Behavior in Strict Mode

In strict mode, `this` is `undefined` if not set by the caller (e.g., global function call):

```js
"use strict";
function checkThis() {
  console.log(this); // undefined
}
checkThis();
```

In non-strict mode, `this` would be the global object (`window` in browsers).

---

## 🔐 Security & Future Proofing

Strict mode reserves keywords for **future versions** of JavaScript.

### Reserved Keywords in Strict Mode:

* `implements`
* `interface`
* `let`
* `package`
* `private`
* `protected`
* `public`
* `static`
* `yield`

```js
"use strict";
let interface = "example"; // ❌ Error
```

---

## ⚠️ Gotchas

* `"use strict"` **must be the first line** in a script or function to take effect.
* Not enforced if used in the middle of code or inside a block.

---

## ✅ Best Practices

* Use `"use strict"` in **all scripts and functions**
* Helps prevent hard-to-debug bugs
* Ensures your code is **modern**, **safe**, and **clean**

---

> 🧠 Tip: Combine `"use strict"` with a linter (like ESLint) for better code safety and consistency.


# JavaScript `this` Keyword — Explained

In JavaScript, `this` is a **special keyword** that refers to the **object that is executing the current function**.

It behaves differently depending on **how and where the function is invoked**.

---

## 📌 What is `this`?

- `this` is **not a variable**.
- It is a **keyword** that is automatically defined in every function's execution context.
- You **cannot assign** a new value to `this`.

---

## ⚙️ `this` — Contextual Behavior

| Context                           | `this` refers to...                        |
|-----------------------------------|--------------------------------------------|
| Global scope                      | Global object (`window` in browsers)       |
| Function (non-strict mode)        | Global object                              |
| Function (strict mode)            | `undefined`                                |
| Object method                     | The object the method belongs to           |
| Event handler (HTML DOM)          | The HTML element that received the event   |
| Constructor function (via `new`)  | The new object being created               |
| Arrow function                    | Inherits `this` from surrounding scope     |
| `bind()`, `call()`, `apply()`     | The explicitly bound object                |


## 🌍 `this` in Global Scope

```js
let x = this;
console.log(x); // In browser: [object Window]
````

* In the global context, `this` refers to the **global object** (`window` in browsers).

---

## 🔧 `this` in Regular Functions

### Without strict mode:

```js
function myFunc() {
  return this;
}
console.log(myFunc()); // [object Window]
```

### With strict mode:

```js
"use strict";
function myFunc() {
  return this;
}
console.log(myFunc()); // undefined
```

* In **strict mode**, default binding is disabled. `this` becomes `undefined`.

---

## 🧠 `this` in Object Methods

```js
const person = {
  name: "Alice",
  greet: function() {
    return "Hello, " + this.name;
  }
};
console.log(person.greet()); // "Hello, Alice"
```

* Inside an object method, `this` refers to the **object itself**.

---

## 🖱️ `this` in Event Handlers

```html
<button onclick="this.style.display='none'">Click Me</button>
```

* In HTML event handlers, `this` refers to the **element that received the event**.

---

## 🏗️ `this` in Constructor Functions

```js
function Person(first, last) {
  this.firstName = first;
  this.lastName = last;
}

const user = new Person("John", "Doe");
console.log(user.firstName); // "John"
```

* When a function is invoked with `new`, `this` refers to the **newly created object**.

---

## ➡️ Arrow Functions and `this`

```js
const person = {
  name: "Bob",
  greet: () => {
    return "Hi, " + this.name;
  }
};

console.log(person.greet()); // "Hi, undefined"
```

* Arrow functions **do not have their own `this`**.
* They inherit `this` from their **lexical (parent) scope**.

---

## 🧲 `this` with `call()`, `apply()`, and `bind()`

```js
function greet() {
  return this.name;
}

const user = { name: "Jane" };

console.log(greet.call(user)); // "Jane"
console.log(greet.apply(user)); // "Jane"

const boundGreet = greet.bind(user);
console.log(boundGreet()); // "Jane"
```

| Method    | Behavior                                               |
| --------- | ------------------------------------------------------ |
| `call()`  | Calls a function with a given `this` and arguments     |
| `apply()` | Same as `call()`, but arguments are passed as an array |
| `bind()`  | Returns a new function with a permanently bound `this` |

---

## 🔢 `this` Precedence Order

When multiple rules apply, the following **order of precedence** determines `this`:

1. `bind()`
2. `apply()` / `call()`
3. Object method
4. Global scope

---

## ✅ Summary: Always Know How You're Calling

* The value of `this` is **determined at call time**, not definition time.
* Avoid surprises by understanding **how the function is invoked**.

---

> 💡 Debug tip: use `console.log(this)` inside any function to inspect the value at runtime.



## ✅ 1. `call()`, `apply()`, `bind()` – Change `this` context

In JavaScript, functions are first-class objects. `call`, `apply`, and `bind` are methods that let you **explicitly set `this`** inside a function.

---

### 🔹 `call()`

Calls the function **immediately**, with a given `this` and **comma-separated arguments**.

#### 📌 Example:

```javascript
function greet(lang1, lang2) {
  console.log(`Hello, I am ${this.name}. I speak ${lang1} and ${lang2}.`);
}

const person = { name: "Anna" };

greet.call(person, "English", "French");
// → Hello, I am Anna. I speak English and French.
```

---

### 🔹 `apply()`

Same as `call`, but takes arguments **as an array**.

```javascript
greet.apply(person, ["English", "French"]);
// → Hello, I am Anna. I speak English and French.
```

---

### 🔹 `bind()`

Returns a **new function** with the `this` context bound. You can call it **later**.

```javascript
const greetAnna = greet.bind(person, "English", "French");

greetAnna(); // call later
// → Hello, I am Anna. I speak English and French.
```

---

## ✅ 2. Closures – Remembering scope

A **closure** is created when a function remembers and accesses variables from its **lexical scope**, even when called outside that scope.

---

### 📌 Example of closure:

```javascript
function outer() {
  let count = 0;

  return function inner() {
    count++;
    console.log(`Count is ${count}`);
  };
}

const counter = outer();

counter(); // Count is 1
counter(); // Count is 2
counter(); // Count is 3
```

### 🔍 Why is this a closure?

* `inner()` still has access to `count` even though `outer()` has finished running.
* This is **closure** — it "closes over" the `count` variable.

---

## ✅ Summary Table

| Method      | Calls Immediately? | Accepts Args | Changes `this` | Returns New Function |
| ----------- | ------------------ | ------------ | -------------- | -------------------- |
| `call`      | ✅                  | Comma list   | ✅              | ❌                    |
| `apply`     | ✅                  | Array        | ✅              | ❌                    |
| `bind`      | ❌ (call later)     | Comma list   | ✅              | ✅                    |
| **Closure** | ❌ (executes later) | n/a          | n/a            | ✅ (remembers scope)  |


# JavaScript Callbacks

> _"I will call back later!"_

A **callback** is a function passed as an argument to another function.  
This allows one function to execute another **after** it finishes.

---

## 🔁 Function Sequence

JavaScript functions are executed in the **order they are called**, not in the order they are defined.

### 📌 Example 1

```javascript
function myFirst() {
  myDisplayer("Hello");
}

function mySecond() {
  myDisplayer("Goodbye");
}

myFirst();
mySecond();
// Output: Hello Goodbye
````

### 📌 Example 2

```javascript
mySecond();
myFirst();
// Output: Goodbye Hello
```

---

## 🧭 Sequence Control

Sometimes you want **better control** over when to execute a function. For example, doing a **calculation** and then **displaying** the result.

### ✅ Option 1: Return Value and Call Display Separately

```javascript
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

function myCalculator(num1, num2) {
  let sum = num1 + num2;
  return sum;
}

let result = myCalculator(5, 5);
myDisplayer(result);
```

### ✅ Option 2: Display Inside Calculator

```javascript
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

function myCalculator(num1, num2) {
  let sum = num1 + num2;
  myDisplayer(sum);
}

myCalculator(5, 5);
```

> ❌ Problem: You **cannot reuse** the result before displaying in Option 2.
> ❌ In Option 1, you need to call two functions manually.

---

## ✅ Using Callbacks

You can pass the **display function as a callback**, letting the calculator decide **when to call it**.

```javascript
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

function myCalculator(num1, num2, myCallback) {
  let sum = num1 + num2;
  myCallback(sum);
}

myCalculator(5, 5, myDisplayer);
```

* `myDisplayer` is a **callback function**.
* It is passed as an **argument** to `myCalculator`.

> 💡 **Note:** When passing a function as an argument, do **not use parentheses**.

```javascript
// ✅ Correct
myCalculator(5, 5, myDisplayer);

// ❌ Wrong
myCalculator(5, 5, myDisplayer());
```

---

## 🧪 Example with Arrays and Callbacks

```javascript
// Create an Array
const myNumbers = [4, 1, -20, -7, 5, 9, -6];

// Keep only positive numbers using a callback
const posNumbers = removeNeg(myNumbers, (x) => x >= 0);

// Display Result
document.getElementById("demo").innerHTML = posNumbers;

// Filter Function
function removeNeg(numbers, callback) {
  const myArray = [];
  for (const x of numbers) {
    if (callback(x)) {
      myArray.push(x);
    }
  }
  return myArray;
}
```

* `(x) => x >= 0` is a **callback function**.
* It is used to **filter** only positive numbers.

---

## 💡 When to Use Callbacks?

These examples are simple to teach the **syntax** of callbacks.

### Where Callbacks Shine:

* In **asynchronous functions**, like:

  * File loading
  * API requests
  * Timers
* When **waiting** for a task to finish before running the next function

Asynchronous functions are covered in the next chapter.

---

# Asynchronous JavaScript

> _"I will finish later!"_

In JavaScript, functions that run **in parallel** with other functions are called **asynchronous**.

A classic example of asynchronous behavior is `setTimeout()`.

---

## 🔁 From Synchronous to Asynchronous

Previously, we used callbacks in **synchronous** functions, like:

```javascript
function myDisplayer(something) {
  document.getElementById("demo").innerHTML = something;
}

function myCalculator(num1, num2, myCallback) {
  let sum = num1 + num2;
  myCallback(sum);
}

myCalculator(5, 5, myDisplayer);
````

* `myDisplayer` is passed as a **callback function**.
* It is called **after** the calculation is completed.

---

## ⏳ Asynchronous with `setTimeout()`

With `setTimeout()`, you can run a function **after a delay**.

### ✅ Example 1: Named Callback

```javascript
setTimeout(myFunction, 3000);

function myFunction() {
  document.getElementById("demo").innerHTML = "I love You !!";
}
```

* `myFunction` is the callback.
* `3000` milliseconds = 3 seconds.
* It will execute after the timeout.

> ⚠️ Important: **Do not use parentheses** when passing the function.

```javascript
// ✅ Correct
setTimeout(myFunction, 3000);

// ❌ Incorrect
setTimeout(myFunction(), 3000); // Executes immediately!
```

---

### ✅ Example 2: Inline Anonymous Function

You can also pass a **complete function** directly:

```javascript
setTimeout(function() {
  myFunction("I love You !!!");
}, 3000);

function myFunction(value) {
  document.getElementById("demo").innerHTML = value;
}
```

* Here, an **anonymous function** is passed.
* It calls `myFunction("I love You !!!")` after 3 seconds.

---

## ⏲️ Asynchronous with `setInterval()`

`setInterval()` is similar to `setTimeout()`, but it runs the function **repeatedly** at specified intervals.

### 🕒 Example: Clock

```javascript
setInterval(myFunction, 1000);

function myFunction() {
  let d = new Date();
  document.getElementById("demo").innerHTML =
    d.getHours() + ":" +
    d.getMinutes() + ":" +
    d.getSeconds();
}
```

* `myFunction` is called every **1000ms** (1 second).
* It displays the **current time** continuously.

---

## ⚠️ Callback Challenges

Asynchronous programming allows JavaScript to:

* Start long-running tasks
* Keep the program **responsive**
* Continue other tasks in parallel

### But... ❗

* Callbacks in async code can be **hard to read** and **hard to debug**
* This leads to **callback hell** (deeply nested callbacks)

---

## ✅ Modern Alternative: Promises

To simplify asynchronous programming:

* Modern JavaScript uses **Promises**
* Promises make code **cleaner**, **easier to manage**, and **easier to chain**

> Promises and async/await are covered in the next chapter.


# JavaScript Promises

> _"I promise a result!"_

In JavaScript, **Promises** are used to handle asynchronous operations in a more elegant way compared to callbacks.

---

## 🤝 What Is a Promise?

- **Producing code** is the code that _does something_ (like fetching data) — it **may take time**.
- **Consuming code** is the code that _uses the result_ — it **must wait**.

A **Promise** is a JavaScript object that links these two parts.

---

## 📦 Promise Syntax

```javascript
let myPromise = new Promise(function(myResolve, myReject) {
  // Producing code (takes time)
  myResolve(); // Call when successful
  myReject();  // Call when error occurs
});

// Consuming code
myPromise.then(
  function(value) { /* on success */ },
  function(error) { /* on error */ }
);
````

---

## 🧠 Understanding Promises

When the producing code finishes, it must call one of these:

| When    | Callback           |
| ------- | ------------------ |
| Success | `myResolve(value)` |
| Error   | `myReject(error)`  |

---

## 📈 Promise States and Result

A Promise can be in one of three **states**:

| State     | Result      |
| --------- | ----------- |
| pending   | `undefined` |
| fulfilled | a value     |
| rejected  | an error    |

> Note: You **cannot directly access** these properties. You use `.then()` or `.catch()`.

---

## ✅ Using `.then()` and `.catch()`

```javascript
myPromise.then(
  function(value) { /* success */ },
  function(error) { /* failure */ }
);
```

* `.then(success, failure)` — both are optional.
* Use `.catch()` for error handling (preferred for cleaner syntax).

---

## 📌 Example: Basic Promise

```javascript
function myDisplayer(some) {
  document.getElementById("demo").innerHTML = some;
}

let myPromise = new Promise(function(myResolve, myReject) {
  let x = 0;

  if (x == 0) {
    myResolve("OK");
  } else {
    myReject("Error");
  }
});

myPromise.then(
  function(value) { myDisplayer(value); },
  function(error) { myDisplayer(error); }
);
```
Example with state and status
```
function asyncSum(numbers) {
  return new Promise((resolve) => {
    setTimeout(() => {
      const sum = numbers.reduce((a, b) => a + b, 0);
      resolve(sum);
    }, 1000);
  });
}

const resultPromise = asyncSum([1, 2, 3, 4, 5]);
console.log("Async sum of numbers:", resultPromise);  // Promise { <pending> }

resultPromise.then(result => {
  console.log("Sum result after await:", result);  // Sum result after await: 15
});

VM515:11 Async sum of numbers: Promise {<pending>}[[Prototype]]: Promise[[PromiseState]]: "fulfilled"[[PromiseResult]]: 15
Promise {<pending>}[[Prototype]]: Promise[[PromiseState]]: "fulfilled"[[PromiseResult]]: undefined
VM515:14 Sum result after await: 15
```
---

## ⏳ Waiting for a Timeout

### Using Callback:

```javascript
setTimeout(function() {
  myFunction("I love You !!!");
}, 3000);

function myFunction(value) {
  document.getElementById("demo").innerHTML = value;
}
```

### Using Promise:

```javascript
let myPromise = new Promise(function(myResolve, myReject) {
  setTimeout(function() {
    myResolve("I love You !!");
  }, 3000);
});

myPromise.then(function(value) {
  document.getElementById("demo").innerHTML = value;
});
```

---

## 📂 Waiting for a File

### Using Callback:

```javascript
function getFile(myCallback) {
  let req = new XMLHttpRequest();
  req.open('GET', "mycar.html");
  req.onload = function() {
    if (req.status == 200) {
      myCallback(req.responseText);
    } else {
      myCallback("Error: " + req.status);
    }
  };
  req.send();
}

getFile(myDisplayer);
```

### Using Promise:

```javascript
let myPromise = new Promise(function(myResolve, myReject) {
  let req = new XMLHttpRequest();
  req.open('GET', "mycar.html");
  req.onload = function() {
    if (req.status == 200) {
      myResolve(req.response);
    } else {
      myReject("File not Found");
    }
  };
  req.send();
});

myPromise.then(
  function(value) { myDisplayer(value); },
  function(error) { myDisplayer(error); }
);
```

---

## ✨ Summary

* Promises simplify asynchronous code.
* Use `.then()` for success and `.catch()` for errors.
* Promises prevent **callback hell** and make code **more readable**.

> Up next: Learn about `async` and `await` for even cleaner async handling.

---

# JavaScript `async` and `await`

> _"async and await make promises easier to write"_

---

## 🚀 What is `async`?

- The `async` keyword is used to declare **asynchronous functions**.
- An `async` function **always returns a Promise**.

### Example:

```javascript
async function myFunction() {
  return "Hello";
}
````

This is the same as:

```javascript
function myFunction() {
  return Promise.resolve("Hello");
}
```

---

## ✅ Consuming an `async` Function

You can handle the result of an `async` function using `.then()`:

```javascript
myFunction().then(
  function(value) { myDisplayer(value); },
  function(error) { myDisplayer(error); }
);
```

Or simply:

```javascript
myFunction().then(function(value) {
  myDisplayer(value);
});
```

---

## 🕓 What is `await`?

* The `await` keyword can **only be used inside an async function**.
* It tells JavaScript to **wait for a Promise to resolve** before continuing.

### Syntax:

```javascript
let result = await somePromise;
```

---

## 🔤 Example: Basic `await`

```javascript
async function myDisplay() {
  let myPromise = new Promise(function(resolve, reject) {
    resolve("I love You !!");
  });
  document.getElementById("demo").innerHTML = await myPromise;
}

myDisplay();
```

> 🔹 `resolve` and `reject` are callback functions used to control the Promise state.

---

## 💡 Simplified Example (Without `reject`)

```javascript
async function myDisplay() {
  let myPromise = new Promise(function(resolve) {
    resolve("I love You !!");
  });
  document.getElementById("demo").innerHTML = await myPromise;
}

myDisplay();
```

---

## ⏳ Waiting with `setTimeout`

This example waits 3 seconds using a Promise inside an async function:

```javascript
async function myDisplay() {
  let myPromise = new Promise(function(resolve) {
    setTimeout(function() {
      resolve("I love You !!");
    }, 3000);
  });
  document.getElementById("demo").innerHTML = await myPromise;
}

myDisplay();
```

---

## 📂 Waiting for a File (Using `XMLHttpRequest`)

```javascript
async function getFile() {
  let myPromise = new Promise(function(resolve) {
    let req = new XMLHttpRequest();
    req.open('GET', "mycar.html");
    req.onload = function() {
      if (req.status == 200) {
        resolve(req.response);
      } else {
        resolve("File not Found");
      }
    };
    req.send();
  });

  document.getElementById("demo").innerHTML = await myPromise;
}

getFile();
```

---

## 🧠 Summary

| Feature    | Description                                               |
| ---------- | --------------------------------------------------------- |
| `async`    | Makes a function return a Promise                         |
| `await`    | Waits for a Promise to resolve before continuing          |
| ✅ Benefits | Cleaner syntax, better readability than chained `.then()` |

> `async`/`await` is built on top of Promises — not a replacement, but a simplification.











