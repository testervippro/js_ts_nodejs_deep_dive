
# Node.js vs Browser JavaScript

JavaScript runs in both **Node.js** and **browsers**, but the environments are quite different. Here's a breakdown of their **key differences**, with real examples.

---

## 🌐 Purpose

| Platform | Usage |
|----------|-------|
| **Node.js** | Server-side programming (back-end) |
| **Browser** | Client-side scripting (front-end) |

---

## 🔑 Key Differences

### 1. **APIs Available**

- **Node.js** has access to:
  - File system
  - Operating system
  - TCP/UDP networking

- **Browsers** have access to:
  - DOM
  - fetch API
  - WebSockets
  - UI rendering

---

### 2. **Global Objects**

- **Node.js:**

```javascript
global.myVar = 42;
console.log(global.myVar); // 42
````

* **Browser:**

```javascript
window.myVar = 42;
console.log(window.myVar); // 42
```

---

### 3. **File Access**

* **Node.js (Allowed):**

```javascript
const fs = require('fs');

fs.readFile('myfile.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

* **Browser (Not allowed):**

```javascript
// Browsers block file system access for security.
// Only sandboxed file APIs or FilePicker APIs are allowed.
```

---

### 4. **HTTP Requests**

* **Node.js:**

```javascript
const https = require('https');

https.get('https://example.com', res => {
  let data = '';
  res.on('data', chunk => data += chunk);
  res.on('end', () => console.log(data));
});
```

* **Browser:**

```javascript
fetch('https://example.com')
  .then(response => response.text())
  .then(data => console.log(data));
```

---

### 5. **Modules**

* **Node.js (CommonJS):**

```javascript
const fs = require('fs');
```

* **Node.js & Browser (ES Modules):**

```javascript
// Node.js with ESM
import fs from 'fs'; // Node only (with ESM support)

// Browser with CDN
import _ from 'https://cdn.jsdelivr.net/npm/lodash-es/lodash.js';
```

---

### 6. **Environment Variables**

* **Node.js:**

```javascript
console.log(process.env.NODE_ENV); // Access env vars
```

* **Browser:**

```javascript
// Not supported
// You must use build tools to inject variables (e.g., Vite, Webpack)
```

---

### 7. **Security**

* **Node.js**: Full access to file system and OS.
* **Browser**: Sandboxed for safety.

---

### 8. **Package Management**

* **Node.js**: Uses `npm` or `yarn`
* **Browser**: Uses CDNs or bundlers (like Webpack, Vite, Parcel)

---

## 🧾 Comparison Table

| Feature               | Node.js         | Browser           |
| --------------------- | --------------- | ----------------- |
| File System Access    | ✅ Yes           | ❌ No              |
| TCP/UDP Networking    | ✅ Yes           | ❌ No              |
| DOM Access            | ❌ No            | ✅ Yes             |
| Global Object         | `global`        | `window` / `self` |
| Modules Supported     | CommonJS / ESM  | ESM / `<script>`  |
| Environment Variables | ✅ `process.env` | ❌ No              |
| Security              | Full OS Access  | Sandboxed         |
| Package Management    | npm / yarn      | CDN / Bundler     |

---

## ✅ Summary

| Node.js                         | Browser                            |
| ------------------------------- | ---------------------------------- |
| Server-side                     | Client-side                        |
| Great for building APIs & tools | Great for building user interfaces |
| Access to low-level resources   | Access to the UI and DOM           |

> ⚠️ Use the right tool for the right job — Node.js for back-end logic, and browser JavaScript for front-end interactivity.




# 🚀 Node.js Command Line Usage

Node.js includes a powerful CLI (Command Line Interface) for running JavaScript files, working with packages, debugging apps, and managing environments.

This guide covers all the **essential Node.js CLI features** with examples.

---

## 🏁 Getting Started

Use these terminals:
- **Windows**: Command Prompt / PowerShell / Windows Terminal
- **macOS/Linux**: Terminal

---

## 📂 Running JavaScript Files

```bash
# Basic: Run a JS file
node app.js

# Pass arguments
node app.js arg1 arg2

# Watch mode (auto-restart on changes)
node --watch app.js
````

---

## 🧪 Node.js REPL

REPL = **Read, Eval, Print, Loop**
Launch an interactive Node shell:

```bash
node
```

Example usage:

```js
> const name = 'Node.js';
> console.log(`Hello, ${name}!`);
> .help   // Show commands
> .exit   // Exit REPL
```

---

## 🧵 Command Line Arguments

Access arguments via `process.argv`:

```js
// args.js
console.log('All args:', process.argv);
console.log('First:', process.argv[2]);
console.log('Second:', process.argv[3]);
```

Run:

```bash
node args.js hello world
```

Output:

```bash
All args: [ ... , 'hello', 'world' ]
First: hello
Second: world
```

---

## 🌍 Environment Variables

Access via `process.env`:

```js
// env.js
console.log('ENV:', process.env.NODE_ENV || 'development');
console.log('MY_VARIABLE:', process.env.MY_VARIABLE);
```

Set env variables:

```bash
# Linux/macOS
NODE_ENV=production MY_VARIABLE=test node env.js

# Windows (PowerShell)
$env:NODE_ENV="production"; $env:MY_VARIABLE="test"; node env.js
```

---

## 🐞 Debugging Node.js Apps

### 🔧 Start Debug Mode

```bash
node --inspect app.js
node --inspect-brk app.js       # Break on first line
node --inspect=9222 app.js      # Custom port
node --inspect=0.0.0.0:9229 app.js  # Remote access (be cautious!)
```

### 🧩 Chrome DevTools Debugging

1. Run: `node --inspect app.js`
2. Open Chrome → `chrome://inspect`
3. Click **"Open dedicated DevTools for Node"**
4. Add breakpoints and inspect variables

---

## 🧰 Common CLI Tools

### 📌 Node Version Manager (nvm)

```bash
nvm install 18.16.0    # Install specific version
nvm use 18.16.0        # Use a version
nvm ls                 # List installed versions
```

### 📦 npm (Node Package Manager)

```bash
npm init               # Initialize project
npm install            # Install dependencies
npm update             # Update packages
npm audit              # Check for vulnerabilities
```

---

## 🏷️ Common Node CLI Flags

### Basic Info

```bash
node --version         # Show Node.js version
node --v8-options      # V8 engine options
node --help            # CLI help
```

### Runtime Behavior

```bash
node --check app.js             # Syntax check only
node --trace-warnings app.js    # Trace warning origins
node --max-old-space-size=4096 app.js  # Set memory limit
node --require dotenv/config app.js    # Preload module
```

### Experimental Features

```bash
node --experimental-modules app.mjs      # Use ES modules
node --experimental-repl-await           # Await in REPL
node --experimental-worker worker.js     # Worker threads
```

---

## ✅ Summary Table

| Feature               | Example Command                         |
| --------------------- | --------------------------------------- |
| Run file              | `node app.js`                           |
| Arguments             | `node app.js hello world`               |
| Watch mode            | `node --watch app.js`                   |
| Use REPL              | `node`                                  |
| Environment Variables | `NODE_ENV=prod node env.js`             |
| Debugging             | `node --inspect app.js`                 |
| Use nvm               | `nvm install 18.16.0`                   |
| Package management    | `npm install`                           |
| Memory limit          | `node --max-old-space-size=2048 app.js` |
| Preload a module      | `node --require dotenv/config app.js`   |

---


# ⚙️ Node.js & the V8 JavaScript Engine

## 🔍 What is the V8 Engine?

The **V8 Engine** is Google’s open-source JavaScript engine, used in:
- 🧭 **Google Chrome** (browser)
- 🖥️ **Node.js** (server-side JS runtime)

### 🌐 Origin
- Developed by **Google** in **2008** for Chrome
- Integrated into **Node.js** to enable running JavaScript outside the browser

### 🚀 Key Features
- Just-In-Time (JIT) Compilation
- Hidden Classes & Inline Caching
- Efficient Garbage Collection
- Modern JavaScript (ES6+) support
- WebAssembly support

---

## ⚡ Why V8 Makes Node.js Fast

### 🔄 Just-In-Time Compilation
- Converts JavaScript into **native machine code** for fast execution

### 🧱 Hidden Classes
- Optimizes object property lookups like statically typed languages

### ♻️ Efficient Garbage Collection
- Cleans up unused memory and prevents leaks

### 🧠 Inline Caching
- Remembers object structure and speeds up repeated property access

---

## 🧪 Example: Check V8 Version

```js
// Show the V8 engine version in your Node.js installation
console.log(`V8 version: ${process.versions.v8}`);
````

Sample Output:

```
V8 version: 12.4.254.11-node.24
```

---

## 🔍 V8's Role in Node.js

V8 powers the **JavaScript runtime** in Node.js and enables:

✅ Running JS **outside the browser**
✅ Accessing system resources (FS, network, etc.)
✅ Using the **same engine as Chrome** for consistent behavior

---

## 📊 Example: V8 Heap Memory Usage

```js
const v8 = require('v8');

const heapStats = v8.getHeapStatistics();

console.log('Heap size limit:', (heapStats.heap_size_limit / 1024 / 1024).toFixed(2), 'MB');
console.log('Total heap size:', (heapStats.total_heap_size / 1024 / 1024).toFixed(2), 'MB');
console.log('Used heap size:', (heapStats.used_heap_size / 1024 / 1024).toFixed(2), 'MB');
```

Sample Output:

```
Heap size limit: 2048.00 MB
Total heap size: 102.40 MB
Used heap size: 64.37 MB
```

---

## 🔄 V8 Update Cycle

* V8 is **constantly updated** with:

  * New **ECMAScript** features
  * Performance optimizations
* Node.js **frequently updates V8** with each major release

🔁 As V8 evolves, **Node.js gains access** to newer JS features like:

* Optional Chaining (`?.`)
* Nullish Coalescing (`??`)
* Top-level `await`, etc.

---

## 📌 Summary Table

| Feature                  | V8 Supports? |
| ------------------------ | ------------ |
| Just-in-time compilation | ✅            |
| ES6+ syntax              | ✅            |
| WebAssembly              | ✅            |
| Garbage collection       | ✅            |
| Used in Node.js          | ✅            |

---

## ✅ Summary

* **V8 is the core engine** behind Node.js JavaScript execution.
* It compiles JS to native code, making Node fast.
* Node.js evolves with V8, gaining access to modern language features.

```

# 🏗️ Node.js Architecture

## 📌 What is Node.js Architecture?

Node.js uses a **single-threaded, event-driven architecture** that is:
- 🔁 **Non-blocking** (asynchronous I/O)
- 🎯 **Single-threaded with event loop**
- ⚙️ Efficient at handling many connections simultaneously
- 📤 Delegates heavy work to a background **Thread Pool**

This makes Node.js perfect for:
- Real-time applications
- APIs and microservices
- I/O-heavy workloads

---

## 📊 Node.js Architecture Diagram Overview

### 🔄 1. Client Request Phase
- Clients send requests to the Node.js server
- Requests are added to the **Event Queue**

### 🔁 2. Event Loop Phase
- Event Loop continuously polls the **Event Queue**
- Picks up requests one at a time and processes them

### ⚙️ 3. Request Processing
- Simple (non-blocking) tasks are handled on the **main thread**
- Complex/blocking tasks (e.g., file I/O, DB queries) are sent to the **libuv Thread Pool**

### 📬 4. Response Phase
- Once blocking tasks are done, their **callbacks** are queued
- Event Loop executes the callbacks and **sends responses**

---

## 🧪 Code Examples: Non-blocking vs Blocking

### ✅ Non-blocking Example

```js
const fs = require('fs');

console.log('Before file read');

fs.readFile('myfile.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log('File contents:', data);
});

console.log('After file read');
````

🧠 **Output Order:**

```
Before file read
After file read
File contents: <your file content>
```

> Notice how the last line is printed last, even though it appears earlier in the code.

---

### ❌ Blocking Example

```js
const fs = require('fs');

console.log('Start of blocking code');

const data = fs.readFileSync('myfile.txt', 'utf8'); // ❗ Blocks execution

console.log('Blocking operation completed');
```

🧠 **Blocking Behavior:**

* The code waits (blocks) until the file is read before continuing.

---

### 🆚 Key Difference

| Feature           | Blocking                | Non-blocking                 |
| ----------------- | ----------------------- | ---------------------------- |
| Main thread waits | ✅ Yes                   | ❌ No                         |
| Other tasks run   | ❌ No (halts everything) | ✅ Yes (event loop continues) |
| Scalability       | ❌ Poor                  | ✅ Excellent                  |

---

## 📈 When to Use Node.js

✅ **Ideal For:**

* I/O-bound applications (file system, DB queries, HTTP calls)
* Real-time systems (chat apps, online games, stock tickers)
* APIs (REST, GraphQL)
* Microservices

❌ **Avoid for:**

* Heavy CPU-bound operations (math-heavy computation, image/video processing)

🧠 **Better Alternatives for CPU-bound Tasks:**

* Use **Worker Threads** (`worker_threads` module)
* Offload to **native modules** (C/C++ add-ons)
* Use a different service/language for the CPU-bound logic (Go, Rust, Python, etc.)

---

## ✅ Summary: Why Node.js is Efficient

* Uses **event loop** + **non-blocking I/O**
* Handles **many concurrent connections** efficiently
* Written in **JavaScript**, works on both **client and server**
* Huge ecosystem via **npm**

---

## 📌 Key Benefits

| Feature               | Benefit                             |
| --------------------- | ----------------------------------- |
| Single-threaded model | Simpler concurrency                 |
| Non-blocking I/O      | High throughput                     |
| Event Loop            | Efficient task scheduling           |
| Thread Pool           | Offloads blocking I/O operations    |
| JavaScript Everywhere | Full-stack development with JS      |
| Massive npm ecosystem | Thousands of ready-to-use libraries |

```
# 🔁 Node.js Event Loop

## 📌 What is the Event Loop?

The **event loop** is the core mechanism behind Node.js's **non-blocking**, **asynchronous behavior**.

- Allows handling of **thousands of concurrent operations** with a single thread.
- Delegates long-running tasks (e.g., file I/O, network requests) to the system and processes the results **via callbacks**.
- Keeps the Node.js process alive until all work is complete.

---

## ⚙️ How the Event Loop Works (Steps)

1. Execute main **synchronous code**
2. Process **microtasks**
   - `Promise.then()`, `queueMicrotask()`, `process.nextTick()`
3. Handle **timers**
   - `setTimeout()`, `setInterval()`
4. Run **I/O callbacks**
   - File system, HTTP, DNS, etc.
5. Execute **setImmediate()** callbacks
6. Process **close** events (e.g., sockets, streams)

Between every phase, Node.js checks and runs the **microtask queue**.

---

## 🧪 Example: Basic Execution Order

```js
console.log('First');

setTimeout(() => console.log('Third'), 0);

Promise.resolve().then(() => console.log('Second'));

console.log('Fourth');
````

🧠 **Output:**

```
First
Fourth
Second
Third
```

✅ **Why?**

* `First` and `Fourth`: synchronous
* `Second`: microtask from Promise
* `Third`: timer, runs in next event loop tick

---

## 🔄 Event Loop Phases (in order)

| Phase         | Example Functions               |
| ------------- | ------------------------------- |
| **1. Timers** | `setTimeout()`, `setInterval()` |
| **2. I/O**    | File system, sockets, DNS, etc. |
| **3. Poll**   | Waiting for new I/O             |
| **4. Check**  | `setImmediate()`                |
| **5. Close**  | `socket.on('close')`, etc.      |

🧠 **Between each phase:** Microtasks (Promises, `process.nextTick()`) are run.

---

## 🔍 Example: Phases in Action

```js
console.log('1. Start');

process.nextTick(() => console.log('2. Next tick'));

Promise.resolve().then(() => console.log('3. Promise'));

setTimeout(() => console.log('4. Timeout'), 0);

setImmediate(() => console.log('5. Immediate'));

console.log('6. End');
```

🧠 **Expected Output:**

```
1. Start
6. End
2. Next tick
3. Promise
4. Timeout
5. Immediate
```

✅ **Priority Order**:

1. Synchronous code
2. `process.nextTick()` queue
3. Microtask queue (e.g., Promises)
4. Timers
5. `setImmediate()`

---

## 🚀 Why Is the Event Loop Important?

The event loop enables:

* ⚡ **High concurrency** with minimal resources
* ⚙️ Efficient **non-blocking** I/O operations
* 🔄 Continuous execution of async callbacks
* 🧩 Real-time systems & **scalable** apps

✅ **Best for:**

* Real-time apps (chat, games)
* APIs and microservices
* Streaming data (audio/video)
* Networked applications

---

## ✅ Summary

* Node.js uses an **event loop** to run asynchronous code without blocking the main thread.
* **Microtasks** (Promises, `nextTick`) have **higher priority** than timers and I/O callbacks.
* This architecture is the secret behind Node.js’s **scalability** and performance.



# ⚙️ Node.js Asynchronous Programming

## 📌 What Is Asynchronous Programming?

In Node.js, asynchronous programming allows your program to **continue executing** while waiting for operations like:

- File system access
- Network requests
- Database queries

This non-blocking model enables **high concurrency** and is key to Node.js’s performance.

---

## 🆚 Sync vs Async

| Feature         | Synchronous                      | Asynchronous                        |
|-----------------|----------------------------------|-------------------------------------|
| Execution       | Blocks until complete            | Non-blocking, runs in background    |
| Simplicity      | Easy to understand               | Requires careful flow control       |
| Performance     | Can slow down the event loop     | Better for I/O-heavy tasks          |
| Examples        | `readFileSync()`                 | `readFile()`, Promises, async/await |

---

## 📄 Example: Synchronous File Read

```js
const fs = require('fs');

console.log('1. Start sync read');
const data = fs.readFileSync('myfile.txt', 'utf8');
console.log('2. File contents:', data);
console.log('3. End sync read');
````

🧠 Output:

```
1. Start sync read
2. File contents: ...
3. End sync read
```

Blocks everything until the file is read.

---

## 📄 Example: Asynchronous File Read

```js
const fs = require('fs');

console.log('1. Start async read');
fs.readFile('myfile.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log('2. File contents:', data);
});
console.log('3. End async start');
```

🧠 Output:

```
1. Start async read
3. End async start
2. File contents: ...
```

---

## ⚠️ Callback Hell

**Problem:**

```js
getUser(id, (err, user) => {
  getOrders(user.id, (err, orders) => {
    processOrders(orders, (err) => {
      console.log('Done!');
    });
  });
});
```

🛠️ **Solution: Promises**

```js
getUser(id)
  .then(user => getOrders(user.id))
  .then(orders => processOrders(orders))
  .then(() => console.log('Done!'))
  .catch(err => console.error(err));
```

---

## ✅ Async/Await (Recommended)

```js
async function handleUser(userId) {
  try {
    const user = await getUser(userId);
    const orders = await getOrders(user.id);
    await processOrders(orders);
    console.log('Done!');
  } catch (err) {
    console.error(err);
  }
}
```

---

## 🔄 Promises with fs

```js
const fs = require('fs').promises;

console.log('1. Reading...');
fs.readFile('myfile.txt', 'utf8')
  .then(data => console.log('3. Content:', data))
  .catch(err => console.error(err));

console.log('2. Async operation started');
```

---

## 🧠 Async/Await with Multiple Files

```js
const fs = require('fs').promises;

async function readFiles() {
  try {
    const data1 = await fs.readFile('file1.txt', 'utf8');
    const data2 = await fs.readFile('file2.txt', 'utf8');
    console.log('Files read successfully!');
    return { data1, data2 };
  } catch (error) {
    console.error('Failed to read files:', error);
  }
}
```

---

## ⚡ Parallel Execution with Promise.all

```js
async function fetchAllData() {
  try {
    const [users, products, orders] = await Promise.all([
      User.find(),
      Product.find(),
      Order.find()
    ]);
    return { users, products, orders };
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
}
```

---

## ✅ Best Practices

| Do ✅                              | Don't ❌                        |
| --------------------------------- | ------------------------------ |
| Use `async/await` for readability | Nest too many callbacks        |
| Always `try/catch` for errors     | Forget to `await` a promise    |
| Use `Promise.all` for parallel    | Mix sync & async unnecessarily |
| Use `fs.promises` over callbacks  | Ignore error handling          |

---

## 🧩 Summary

* Node.js uses asynchronous I/O for performance
* Prefer **async/await** for cleaner code
* Avoid **callback hell** with Promises or async/await
* Handle errors properly in async functions
* Use **Promise.all** for concurrent async tasks

---


# 📦 Node.js Promises

## 🔍 Introduction

A **Promise** in Node.js is a modern way to handle asynchronous operations. It represents a value that may be available now, later, or never.

---

## 📊 Promise States

| State     | Description                                  |
|-----------|----------------------------------------------|
| Pending   | Initial state, operation is not completed    |
| Fulfilled | Operation completed successfully             |
| Rejected  | Operation failed                             |

Once **settled** (fulfilled or rejected), a Promise's state is final.

---

## ✅ Benefits of Using Promises

### 🔁 Callbacks (Old Way):
```js
getUser(id, (err, user) => {
  if (err) return handleError(err);
  getOrders(user.id, (err, orders) => {
    if (err) return handleError(err);
    processOrders(orders);
});
````

### 🔗 Promises (Better Way):

```js
getUser(id)
  .then(user => getOrders(user.id))
  .then(orders => processOrders(orders))
  .catch(handleError);
```

### 🎯 Advantages:

* Avoids **callback hell**
* Centralized **error handling**
* Easy to **chain** multiple async operations
* Supports **parallel execution** via `Promise.all`

---

## 🔧 Creating a Basic Promise

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = Math.random() > 0.5;
    success
      ? resolve("Operation completed successfully")
      : reject(new Error("Operation failed"));
  }, 1000);
});

myPromise
  .then(result => console.log('✅', result))
  .catch(error => console.error('❌', error.message));
```

---

## 📁 Example: Read File with Promises

```js
const fs = require('fs').promises;

fs.readFile('myfile.txt', 'utf8')
  .then(data => console.log('File:', data))
  .catch(err => console.error('Error:', err));
```

---

## 🔗 Promise Chaining

```js
function getUser(userId) {
  return new Promise(resolve => {
    setTimeout(() => resolve({ id: userId, name: 'John' }), 1000);
  });
}

function getUserPosts(user) {
  return new Promise(resolve => {
    setTimeout(() => resolve(['Post 1', 'Post 2']), 1000);
  });
}

getUser(123)
  .then(user => {
    console.log('User:', user);
    return getUserPosts(user);
  })
  .then(posts => console.log('Posts:', posts))
  .catch(error => console.error('Error:', error));
```

---

## 🧰 Common Promise Methods

### 🔸 Instance Methods

| Method       | Description                    |
| ------------ | ------------------------------ |
| `.then()`    | Handles success or failure     |
| `.catch()`   | Handles only rejection         |
| `.finally()` | Executes regardless of outcome |

```js
myPromise
  .then(data => console.log(data))
  .catch(err => console.error(err))
  .finally(() => console.log("Finished"));
```

---

### 🔹 Static Methods

| Method                   | Purpose                                |
| ------------------------ | -------------------------------------- |
| `Promise.all([])`        | Waits for all promises to resolve      |
| `Promise.race([])`       | Resolves/rejects with the first one    |
| `Promise.allSettled([])` | Waits for all to settle (no fail-fast) |
| `Promise.resolve(val)`   | Creates a resolved promise             |
| `Promise.reject(err)`    | Creates a rejected promise             |

---

## ⚡ Example: `Promise.all()` for Parallel Execution

```js
const fs = require('fs').promises;

const p1 = Promise.resolve('First result');
const p2 = new Promise(resolve => setTimeout(() => resolve('Second result'), 1000));
const p3 = fs.readFile('data.txt', 'utf8');

Promise.all([p1, p2, p3])
  .then(results => {
    console.log('Results:', results);
  })
  .catch(error => {
    console.error('One failed:', error);
  });
```

---

## 🏁 `Promise.race()` – First Settled Promise

```js
const fast = new Promise(resolve => setTimeout(() => resolve('Fast'), 500));
const slow = new Promise(resolve => setTimeout(() => resolve('Slow'), 1000));

Promise.race([fast, slow])
  .then(result => console.log('Winner:', result)); // Fast
```

---

## ❗ Error Handling in Promises

```js
function fetchData() {
  return new Promise((_, reject) => {
    reject(new Error("Network error"));
  });
}

// Option 1
fetchData()
  .then(data => console.log(data))
  .catch(error => console.error("Caught:", error.message));

// Option 2 (less common)
fetchData()
  .then(
    data => console.log(data),
    error => console.error("Handled:", error.message)
  );
```

✅ **Best Practice:** Always use `.catch()` for error handling to avoid unhandled rejections.

---

## 📌 Summary

* **Promises** improve async handling over callbacks
* Use `.then()`, `.catch()`, and `.finally()` for clean control flow
* Use `Promise.all()` and `Promise.race()` for concurrent tasks
* Always handle errors properly

---

## 🔁 Node.js Async/Await

### ✅ What Is It?

`async/await` is a modern syntax for working with asynchronous code in JavaScript and Node.js.
It’s **built on top of Promises**, and it makes async code look synchronous — making it easier to read, write, and debug.

---

## 📘 Syntax

```js
async function myFunction() {
  const result = await someAsyncOperation();
  console.log(result);
}
```

* `async` — marks a function as asynchronous and makes it return a Promise.
* `await` — pauses the function until the Promise resolves/rejects.

---

## 🚀 Basic Example

```js
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function run() {
  console.log("Start");
  await delay(1000);
  console.log("1 second later");
}

run();
```

---

## 📁 Example: Reading a File

```js
const fs = require('fs').promises;

async function readFile() {
  try {
    const content = await fs.readFile('myfile.txt', 'utf8');
    console.log(content);
  } catch (err) {
    console.error('Error:', err);
  }
}
```

---

## ⚖️ Comparison: Callbacks vs Promises vs Async/Await

| Pattern     | Pros                           | Cons                                    |
| ----------- | ------------------------------ | --------------------------------------- |
| Callbacks   | Simple, widely supported       | Callback hell, hard to manage           |
| Promises    | Clean chaining, error handling | Verbose, nested chaining                |
| Async/Await | Very readable, try/catch       | Requires Promises, can block if misused |

### 🔁 Callback Example

```js
getUser(1, (err, user) => {
  if (err) return console.error(err);
  getPosts(user.id, (err, posts) => {
    if (err) return console.error(err);
    console.log(posts);
  });
});
```

### 🔗 Promise Example

```js
getUser(1)
  .then(user => getPosts(user.id))
  .then(posts => console.log(posts))
  .catch(err => console.error(err));
```

### ✨ Async/Await Example

```js
async function showPosts() {
  try {
    const user = await getUser(1);
    const posts = await getPosts(user.id);
    console.log(posts);
  } catch (err) {
    console.error(err);
  }
}
```

---

## 🧠 Advanced: Parallel vs Sequential

### ❌ Sequential (Slow)

```js
const a = await getDataA();
const b = await getDataB();
```

### ✅ Parallel (Fast)

```js
const [a, b] = await Promise.all([getDataA(), getDataB()]);
```

---

## 🛡️ Error Handling with try/catch

```js
async function fetchData() {
  try {
    const res = await fetch('https://api.example.com/data');
    const json = await res.json();
    return json;
  } catch (err) {
    console.error('Fetch failed:', err.message);
  }
}
```

You can also catch outside:

```js
fetchData().catch(err => console.error(err));
```

---

## 🔧 Best Practices

* ✅ Use `Promise.all` for parallel operations.
* ✅ Always wrap `await` in `try/catch`.
* ✅ Don’t mix `await` with callbacks — **convert callbacks** to Promises using `util.promisify`.

```js
const util = require('util');
const readFile = util.promisify(require('fs').readFile);
```

* ✅ Keep async functions small and focused.
* ✅ Use **top-level await** only in **ESM (ECMAScript Modules)**.

---

## ✅ Summary

| Concept         | Description                                |
| --------------- | ------------------------------------------ |
| `async`         | Declares a function that returns a Promise |
| `await`         | Pauses execution until Promise settles     |
| `try/catch`     | Handles errors inside `async` functions    |
| `Promise.all()` | Runs multiple promises in parallel         |

---

# Node.js Error Handling

## Why Handle Errors?

Errors are inevitable in any program, but how you handle them makes all the difference. In Node.js, proper error handling is crucial because:

- It prevents applications from crashing unexpectedly  
- It provides meaningful feedback to users  
- It makes debugging easier with proper error context  
- It helps maintain application stability in production  
- It ensures resources are properly cleaned up  

---

## Common Error Types in Node.js

### 1. Standard JavaScript Errors

```js
// SyntaxError
JSON.parse('{invalid json}');

// TypeError
null.someProperty;

// ReferenceError
unknownVariable;
````

### 2. System Errors

```js
// ENOENT: No such file or directory
const fs = require('fs');
fs.readFile('nonexistent.txt', (err) => {
  console.error(err.code); // 'ENOENT'
});

// ECONNREFUSED: Connection refused
const http = require('http');
const req = http.get('http://nonexistent-site.com', (res) => {});
req.on('error', (err) => {
  console.error(err.code); // 'ECONNREFUSED' or 'ENOTFOUND'
});
```

---

## Basic Error Handling

### Error-First Callbacks

The most common pattern in Node.js core modules is where the first argument to a callback is an error object (if any occurred).

#### Example: Error-First Callback

```js
const fs = require('fs');

function readConfigFile(filename, callback) {
  fs.readFile(filename, 'utf8', (err, data) => {
    if (err) {
      if (err.code === 'ENOENT') {
        return callback(new Error(`Config file ${filename} not found`));
      } else if (err.code === 'EACCES') {
        return callback(new Error(`No permission to read ${filename}`));
      }
      return callback(err);
    }

    try {
      const config = JSON.parse(data);
      callback(null, config);
    } catch (parseError) {
      callback(new Error(`Invalid JSON in ${filename}`));
    }
  });
}

// Usage
readConfigFile('config.json', (err, config) => {
  if (err) {
    console.error('Failed to read config:', err.message);
    return;
  }
  console.log('Config loaded successfully:', config);
});
```

---

## Modern Error Handling

### Using try...catch with Async/Await

With `async/await`, you can use `try/catch` blocks for both synchronous and asynchronous code:

```js
const fs = require('fs').promises;

async function loadUserData(userId) {
  try {
    const data = await fs.readFile(`users/${userId}.json`, 'utf8');
    const user = JSON.parse(data);

    if (!user.email) {
      throw new Error('Invalid user data: missing email');
    }

    return user;
  } catch (error) {
    if (error.code === 'ENOENT') {
      throw new Error(`User ${userId} not found`);
    } else if (error instanceof SyntaxError) {
      throw new Error('Invalid user data format');
    }
    throw error;
  } finally {
    console.log(`Finished processing user ${userId}`);
  }
}

// Usage
(async () => {
  try {
    const user = await loadUserData(123);
    console.log('User loaded:', user);
  } catch (error) {
    console.error('Failed to load user:', error.message);
  }
})();
```

---

## Global Error Handling

### Uncaught Exceptions

For unexpected errors, use global event handlers to perform cleanup:

```js
// Handle uncaught exceptions
process.on('uncaughtException', (error) => {
  console.error('UNCAUGHT EXCEPTION! Shutting down...');
  console.error(error.name, error.message);

  server.close(() => {
    console.log('Process terminated due to uncaught exception');
    process.exit(1);
  });
});

// Handle unhandled promise rejections
process.on('unhandledRejection', (reason, promise) => {
  console.error('UNHANDLED REJECTION! Shutting down...');
  console.error('Unhandled Rejection at:', promise, 'Reason:', reason);

  server.close(() => {
    process.exit(1);
  });
});

// Examples
Promise.reject(new Error('Something went wrong'));

setTimeout(() => {
  throw new Error('Uncaught exception after timeout');
}, 1000);
```

---

## Error Handling Best Practices

### ✅ Do

* Handle errors at the appropriate level
* Log errors with sufficient context
* Use custom error types for different scenarios
* Clean up resources in `finally` blocks
* Validate input to catch errors early

### ❌ Don’t

* Ignore errors (empty catch blocks)
* Expose sensitive error details to clients
* Use try/catch for flow control
* Swallow errors without logging them
* Continue execution after unrecoverable errors

---

## Custom Error Types

```js
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = 'ValidationError';
    this.field = field;
    this.statusCode = 400;
  }
}

class NotFoundError extends Error {
  constructor(resource) {
    super(`${resource} not found`);
    this.name = 'NotFoundError';
    this.statusCode = 404;
  }
}

// Usage
function getUser(id) {
  if (!id) {
    throw new ValidationError('User ID is required', 'id');
  }
  // ...
}
```

---

## Summary

Effective error handling is a critical aspect of building robust Node.js applications.

By understanding different error types, using appropriate patterns, and following best practices, you can create applications that are:

* More stable
* More maintainable
* More user-friendly

> Good error handling is not just about preventing crashes—it's about providing meaningful feedback, maintaining data integrity, and ensuring a good user experience even when things go wrong.



# Node.js Modules

## What is a Module in Node.js?

Modules are the building blocks of Node.js applications. They help in:

- Organizing code into manageable files  
- Encapsulating functionality  
- Preventing global namespace pollution  
- Improving code maintainability and reusability  

> Node.js supports two module systems:  
> ✅ CommonJS (default)  
> ✅ ES Modules (ECMAScript modules)  

This page covers **CommonJS**.

---

## Core Built-in Modules

Node.js provides several built-in modules compiled into the binary. Common examples include:

- `fs` – File system operations  
- `http` – HTTP server and client  
- `path` – File path utilities  
- `os` – Operating system utilities  
- `events` – Event handling  
- `util` – Utility functions  
- `stream` – Stream handling  
- `crypto` – Cryptographic functions  
- `url` – URL parsing  
- `querystring` – URL query string handling  

### Example: Using Built-in Modules

```js
const http = require('http');

http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('Hello World!');
}).listen(8080);
````

---

## Creating and Exporting Modules

In Node.js, **any `.js` file is a module**. You can export functionality in multiple ways:

### 1. Exporting Multiple Items

```js
// utils.js

const getCurrentDate = () => new Date().toISOString();

const formatCurrency = (amount, currency = 'USD') => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: currency
  }).format(amount);
};

// Method 1: Using `exports`
exports.getCurrentDate = getCurrentDate;
exports.formatCurrency = formatCurrency;

// Method 2: Using `module.exports`
// module.exports = { getCurrentDate, formatCurrency };
```

### 2. Exporting a Single Item

```js
// logger.js

class Logger {
  constructor(name) {
    this.name = name;
  }

  log(message) {
    console.log(`[${this.name}] ${message}`);
  }

  error(error) {
    console.error(`[${this.name}] ERROR:`, error.message);
  }
}

module.exports = Logger;
```

---

## Using Your Modules

```js
// app.js

const http = require('http');
const path = require('path');

// Import custom modules
const { getCurrentDate, formatCurrency } = require('./utils');
const Logger = require('./logger');

// Create logger instance
const logger = new Logger('App');

const server = http.createServer((req, res) => {
  try {
    logger.log(`Request received for ${req.url}`);

    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.write(`<h1>Welcome to our app!</h1>`);
    res.write(`<p>Current date: ${getCurrentDate()}</p>`);
    res.write(`<p>Formatted amount: ${formatCurrency(99.99)}</p>`);
    res.end();
  } catch (error) {
    logger.error(error);
    res.writeHead(500, { 'Content-Type': 'text/plain' });
    res.end('Internal Server Error');
  }
});

// Start server
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  logger.log(`Server running at http://localhost:${PORT}`);
});
```

---

## Module Loading and Caching

* Node.js **caches** modules after the first load.
* Requiring the same module again returns the cached instance.
* This behavior can optimize performance but can also affect dynamic module behavior.

---

## Module Resolution Order

When using `require()`, Node.js resolves modules in the following order:

1. Core Node.js modules (e.g., `fs`, `http`)
2. `node_modules` directories (packages)
3. Local files (`./`, `../`)

---

## Best Practices

### ✅ Module Organization

* Focus each module on a **single responsibility**
* Use **meaningful file and directory names**
* Group related functionality together
* Use `index.js` as entry point for folders

### ✅ Export Patterns

* Use **named exports** for utility collections
* Use **default exports** for classes or single functions
* Always **document the module's API**

### ⚠️ Handle Initialization

* If your module requires startup configuration or setup, encapsulate it.

---

## Summary

Modules are a **core concept** in Node.js that promote reusable, organized, and maintainable code.

### ✅ Key Takeaways:

* Node.js uses **CommonJS** by default
* Use `require()` to import, `module.exports` or `exports` to export
* Modules are **cached** after first load
* Follow best practices to maintain a clean and scalable architecture


# Node.js ES Modules

## Introduction to ES Modules

**ES Modules (ESM)** is the official standard for packaging JavaScript code. It was introduced in **ES6 (ES2015)** and is now fully supported in Node.js.

### Benefits of ES Modules

- More structured and statically analyzable
- Enables tree-shaking for smaller builds
- Cleaner and modern syntax

> Previously, Node.js used **CommonJS** (`require/exports`) by default.  
> Now, developers can choose **CommonJS** or **ESM** depending on project needs.

---

## CommonJS vs ES Modules

| Feature               | CommonJS               | ES Modules              |
|-----------------------|------------------------|--------------------------|
| File Extension        | `.js` (default)         | `.mjs` or `.js` with `"type": "module"` |
| Import Syntax         | `require()`            | `import`                |
| Export Syntax         | `module.exports`       | `export`, `export default` |
| Import Timing         | Dynamic (runtime)      | Static (parsed before execution) |
| Top-level Await       | ❌ Not supported        | ✅ Supported             |
| File URL in Imports   | ❌ Not required         | ✅ Required for local files |

---

## Example: CommonJS

```js
// math.js (CommonJS)
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = { add, subtract };
````

```js
// app.js
const math = require('./math');
console.log(math.add(5, 3)); // 8
```

---

## Example: ES Module

```js
// math.mjs
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

```js
// app.mjs
import { add, subtract } from './math.mjs';
console.log(add(5, 3)); // 8
```

---

## Enabling ES Modules in Node.js

### 1. Use `.mjs` file extension

Node will treat `.mjs` files as ES Modules automatically.

### 2. Use `"type": "module"` in `package.json`

```json
{
  "type": "module"
}
```

This allows `.js` files to be treated as ES Modules.

### 3. Use the CLI flag

```bash
node --input-type=module script.js
```

---

## Import and Export Syntax

### Exporting

#### Named Exports

```js
export function sayHello() { console.log('Hello'); }
export function sayGoodbye() { console.log('Goodbye'); }

// OR
function add(a, b) { return a + b; }
function subtract(a, b) { return a - b; }
export { add, subtract };
```

#### Default Export

```js
export default function() {
  console.log('I am the default export');
}

// OR
function mainFunction() { return 'Main functionality'; }
export default mainFunction;
```

#### Mixed Exports

```js
export const VERSION = '1.0.0';
function main() { console.log('Main function'); }
export { main as default };
```

---

### Importing

#### Named Imports

```js
import { sayHello, sayGoodbye } from './greetings.mjs';
import { add as sum, subtract as minus } from './math.mjs';
import * as math from './math.mjs';
```

#### Default Imports

```js
import mainFunction from './main.mjs';
import anyNameYouWant from './main.mjs';
```

#### Default + Named

```js
import main, { VERSION } from './main.mjs';
```

---

## Dynamic Imports

Dynamic import lets you conditionally load a module at runtime.

```js
// app.mjs
async function loadModule(moduleName) {
  try {
    const module = await import(`./${moduleName}.mjs`);
    return module;
  } catch (err) {
    console.error(`Failed to load ${moduleName}:`, err);
  }
}

const moduleName = process.env.NODE_ENV === 'production' ? 'prod' : 'dev';
loadModule(moduleName).then((module) => {
  module.default(); // call default export
});
```

```js
// Or simpler:
const mathModule = await import('./math.mjs');
console.log(mathModule.add(10, 5)); // 15
```

> ✅ Dynamic imports are great for **code splitting**, **lazy-loading**, or **conditional loading**.

---

## Top-level Await

```js
// data-loader.mjs
console.log('Loading data...');
const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
const data = await response.json();
console.log('Data loaded!');
export { data };
```

> Useful for:

* Loading config files
* DB connections
* Initialization logic

---

## Best Practices

### ✅ 1. Be Clear About File Extensions

```js
// Good
import { someFunction } from './utils.mjs';

// Bad
import { someFunction } from './utils';
```

---

### ✅ 2. Use Directory Indexes

```js
// utils/index.mjs
export * from './string-utils.mjs';
export * from './number-utils.mjs';

// app.mjs
import { formatString, add } from './utils/index.mjs';
```

---

### ✅ 3. Choose the Right Export Style

* Use **named exports** for multiple utility functions.
* Use **default exports** for single main functionality.

---

### ✅ 4. Handle the CommonJS Transition

* ESM **can import** CommonJS (via default import).
* CommonJS **must use dynamic import** to load ESM.

```js
// Importing CommonJS from ESM
import fs from 'fs'; // default import

// Importing ESM from CommonJS
(async () => {
  const { default: myEsmModule } = await import('./my-esm-module.mjs');
})();
```

---

### ✅ 5. Dual Package Hazard

To support both ESM and CommonJS in your npm package:

```json
{
  "exports": {
    ".": {
      "import": "./index.mjs",
      "require": "./index.cjs"
    }
  }
}
```

---

## Node.js Version Support

* ✅ Node.js v12+ supports ESM
* ✅ Full support in v14+ and later
* ❌ For older Node versions, use **transpilers** like Babel

---

## Summary

* ES Modules offer a modern, static, and powerful module system in Node.js
* Use `.mjs` or `"type": "module"` in `package.json`
* Prefer `import`/`export` over `require`/`module.exports`
* Take advantage of top-level `await`, tree-shaking, and dynamic imports

> Choosing ES Modules helps future-proof your applications and aligns with the modern JavaScript ecosystem.


# **Node.js NPM**

## **What is NPM?**

* **NPM (Node Package Manager)** is the default package manager for Node.js.
* It helps developers **install**, **share**, and **manage dependencies** (modules/libraries).
* The NPM registry ([https://www.npmjs.com](https://www.npmjs.com)) hosts **thousands of free packages**.
* Installed automatically when you install Node.js.

---

## **What is a Package?**

A **package** is a collection of files (code, metadata, etc.) used as a reusable module in a Node.js project.

Typical uses:

* Add utilities or frameworks (e.g., Express, Lodash)
* Share custom code across projects
* Use third-party APIs or libraries

---

## **Installing a Package**

### **Local Installation (default)**

```bash
npm install <package-name>
```

* Installs package in a local `node_modules/` folder.
* Example:

```bash
npm install upper-case
```

Creates:

```
project-folder/
  └── node_modules/
        └── upper-case/
```

### **Using the Package**

```js
const uc = require('upper-case');
console.log(uc.upperCase("Hello World!")); // Outputs: HELLO WORLD!
```

### **Example: Simple HTTP Server with NPM Package**

```js
const http = require('http');
const uc = require('upper-case');

http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.write(uc.upperCase("Hello World!"));
  res.end();
}).listen(8080);
```

Run the server:

```bash
node demo_uppercase.js
```

Visit: `http://localhost:8080`

---

## **Global Packages**

Global packages are used for CLI tools.

### **Install Globally**

```bash
npm install -g <package-name>
```

Example:

```bash
npm install -g http-server
```

### **Use the CLI Tool Anywhere**

```bash
http-server
```

> ℹ️ On Unix systems, you may need `sudo`:

```bash
sudo npm install -g http-server
```

---

## **Managing Packages**

### **Update Packages**

* Update one package:

  ```bash
  npm update <package-name>
  ```
* Update all:

  ```bash
  npm update
  ```
* Check outdated packages:

  ```bash
  npm outdated
  ```

### **Update NPM itself**

```bash
npm install -g npm@latest
```

---

## **Uninstalling Packages**

### **Uninstall Local Package**

```bash
npm uninstall <package-name>
```

### **Uninstall Global Package**

```bash
npm uninstall -g <package-name>
```

### **Uninstall and Update `package.json`**

```bash
npm uninstall --save <package-name>
```

---

## **Best Practices**

✅ Use a `package.json` file to manage project dependencies:

```bash
npm init
```

✅ Use `--save-dev` for development dependencies:

```bash
npm install --save-dev nodemon
```

✅ Use `.gitignore` to exclude `node_modules` from version control

✅ Use semantic versioning (`^`, `~`) in `package.json` for better update control

✅ Avoid installing unnecessary global packages unless needed for CLI use

---

## **Summary**

| Feature            | Description                                      |
| ------------------ | ------------------------------------------------ |
| **NPM**            | Node.js package manager                          |
| **Local Install**  | `npm install <name>` adds package to project     |
| **Global Install** | `npm install -g <name>` installs system-wide     |
| **Use in Code**    | `require('<package>')` to use installed package  |
| **Uninstall**      | `npm uninstall <name>` to remove                 |
| **Update**         | `npm update` to keep packages current            |
| **CLI Tools**      | Use global installs for tools like `http-server` |


# **Node.js: Dependency Management & `package.json`**

---

## ✅ What Is Dependency Management?

Dependency management is the process of:

* Tracking
* Installing
* Updating
* Removing external packages your app relies on

### **Why it's important:**

* Keeps your application **stable**, **secure**, and **maintainable**
* Handles versioning and compatibility
* Enables collaboration and consistent environments

**Default tool:** `npm` (Node Package Manager)
**Alternatives:** `Yarn`, `pnpm`

---

## 📦 `package.json` — The Dependency Manifest

The `package.json` file defines:

* App metadata (name, version, description, etc.)
* Scripts for automation
* Dependencies (`dependencies`, `devDependencies`, etc.)

Created with:

```bash
npm init           # Interactive
npm init -y        # Defaults
```

---

## 🔒 Lock Files

Lock files ensure **consistent installations** by freezing exact versions of installed packages and their sub-dependencies.

| Tool | File                |
| ---- | ------------------- |
| npm  | `package-lock.json` |
| yarn | `yarn.lock`         |

✅ **Always commit lock files to version control**

---

## 📐 Understanding Semantic Versioning (SemVer)

Version format: `MAJOR.MINOR.PATCH`

| Segment | Meaning                      |
| ------- | ---------------------------- |
| MAJOR   | Breaking changes             |
| MINOR   | Backward-compatible features |
| PATCH   | Bug fixes                    |

### Version specifiers in `package.json`:

| Symbol  | Example   | Meaning                       |
| ------- | --------- | ----------------------------- |
| `^`     | `^2.8.1`  | Any 2.x.x version             |
| `~`     | `~2.8.1`  | Any 2.8.x version             |
| `>=`    | `>=2.8.1` | 2.8.1 or higher               |
| None    | `2.8.1`   | Exact version only            |
| `*`     | `*`       | Any version (not recommended) |
| `"2.x"` | `2.x`     | Any version with major 2      |

---

## 📥 Installing Dependencies

### 🔹 Install All

```bash
npm install
```

* Installs everything in `package.json`

### 🔹 Install a Specific Package

```bash
npm install express
```

### 🔹 Install Specific Version

```bash
npm install express@4.17.1
```

### 🔹 Install Without Saving

```bash
npm install express --no-save
```

### 🔹 Install Globally

```bash
npm install -g nodemon
```

---

## 📂 Types of Dependencies

| Type     | Install Command                           | Usage                                                               |
| -------- | ----------------------------------------- | ------------------------------------------------------------------- |
| Regular  | `npm install express`                     | Needed in **production**                                            |
| Dev      | `npm install jest --save-dev` or `-D`     | Only for **development/testing**                                    |
| Peer     | Defined in `package.json`                 | Used in **plugins/libraries** to declare required external packages |
| Optional | `npm install pkg --save-optional` or `-O` | Enhances features, not required                                     |

### 🔸 Example: `peerDependencies`

```json
"peerDependencies": {
  "react": "^17.0.0"
}
```

---

## 🧩 Example: Mixed Dependency Types

```json
"dependencies": {
  "express": "^4.18.2",
  "lodash": "~4.17.21"
},
"devDependencies": {
  "jest": "^29.5.0",
  "nodemon": "^2.0.22"
},
"peerDependencies": {
  "react": "^18.0.0"
},
"optionalDependencies": {
  "fancy-feature": "^1.2.3"
}
```

---

## 🔁 Updating Dependencies

### Check for outdated packages

```bash
npm outdated
```

### Update specific package

```bash
npm update express
```

### Update all packages

```bash
npm update
```

### Update `npm` itself

```bash
npm install -g npm@latest
```

### Use `npm-check-updates` for advanced updating:

```bash
npm install -g npm-check-updates
ncu       # Check updates
ncu -u    # Update package.json
npm install
```

---

## 🔐 Security & Auditing

### Audit for vulnerabilities

```bash
npm audit
```

### Automatically fix issues

```bash
npm audit fix
```

### Force fix (can break things!)

```bash
npm audit fix --force
```

---

## 🧰 Troubleshooting Common Issues

| Issue            | Solution                                                          |
| ---------------- | ----------------------------------------------------------------- |
| Corrupt cache    | `npm cache clean --force`                                         |
| Broken installs  | Delete `node_modules` and `package-lock.json`, then `npm install` |
| Peer conflicts   | `npm ls`                                                          |
| Rebuild packages | `npm rebuild`                                                     |

---

## 📌 Best Practices

✅ Use **exact versions** in production
✅ Regularly **audit** for vulnerabilities
✅ Commit **lock files** (`package-lock.json`)
✅ Separate **dev** vs. **prod** dependencies
✅ Avoid unnecessary dependencies
✅ Document why dependencies are used
✅ Use scoped packages for internal modules

---

## ✅ Summary Table

| Task                | Command                            |
| ------------------- | ---------------------------------- |
| Install all deps    | `npm install`                      |
| Add dependency      | `npm install <package>`            |
| Add dev dependency  | `npm install --save-dev <package>` |
| Remove package      | `npm uninstall <package>`          |
| Update package      | `npm update <package>`             |
| Audit security      | `npm audit`                        |
| Fix vulnerabilities | `npm audit fix`                    |
| Global install      | `npm install -g <package>`         |
| Check outdated      | `npm outdated`                     |
| Clean cache         | `npm cache clean --force`          |




# 📁 Node.js File System (fs) Module


## 📌 Introduction

The **File System (fs)** module in Node.js allows interaction with the file system:

* Read, write, update, delete files and directories
* Work with file metadata
* Watch for file changes
* Use synchronous, callback-based, or promise-based APIs

✅ **No installation required** — it's a **core module**.

---

## 📥 Importing the `fs` Module

### ✅ CommonJS (default in Node.js)

```js
const fs = require('fs');
```

### ✅ ES Modules (Node.js 14+)

```js
import fs from 'fs'; // Full import
import { readFile, writeFile } from 'fs/promises'; // Named imports
```

---

## 🔁 Async & Promise-based APIs

For modern applications, prefer the **Promise-based API** under `fs/promises`:

```js
// CommonJS
const fs = require('fs').promises;
const { readFile, writeFile } = require('fs').promises;

// ES Modules
import { readFile, writeFile } from 'fs/promises';
```

---

## 📌 Common Use Cases

### 📄 File Operations

* Read & write files
* Append to files
* Rename or move files
* Delete files
* Change file permissions

### 📁 Directory Operations

* Create/remove directories
* List directory contents
* Check file/directory existence
* Watch for changes (`fs.watch`)
* Get stats (`fs.stat`)

### ⚙️ Advanced

* File streams (`fs.createReadStream`)
* Symbolic links
* File descriptors

💡 **Performance Tip:** For large files, use **streams** to avoid loading entire files into memory.

---

## 📚 Reading Files — 3 Approaches

---

### 1️⃣ Callback-based (Traditional)

```js
const fs = require('fs');

fs.readFile('myfile.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  console.log('File content:', data);
});
```

🔹 For binary files (like images), omit encoding:

```js
fs.readFile('image.png', (err, data) => {
  if (err) throw err;
  console.log('Image size:', data.length, 'bytes');
});
```

---

### 2️⃣ Promise-based (Modern & Recommended)

#### Using `fs.promises`

```js
const fs = require('fs').promises;

async function readFileExample() {
  try {
    const data = await fs.readFile('myfile.txt', 'utf8');
    console.log('File content:', data);
  } catch (err) {
    console.error('Error reading file:', err);
  }
}

readFileExample();
```

#### Or use `util.promisify` (legacy approach)

```js
const { promisify } = require('util');
const readFileAsync = promisify(require('fs').readFile);

async function readWithPromisify() {
  try {
    const data = await readFileAsync('myfile.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}

readWithPromisify();
```

---

### 3️⃣ Synchronous (Blocking — Avoid in Production)

```js
const fs = require('fs');

try {
  const data = fs.readFileSync('myfile.txt', 'utf8');
  console.log('File content:', data);
} catch (err) {
  console.error('Error reading file:', err);
}
```

⚠️ **Synchronous methods block the event loop** — use them only for small scripts or CLI tools.

---

## ✅ Best Practices

| Practice                            | Why                                               |
| ----------------------------------- | ------------------------------------------------- |
| Prefer `fs.promises` or async/await | Cleaner, avoids callback hell                     |
| Avoid sync methods in servers       | Blocks event loop, reduces performance            |
| Always handle errors                | File access can fail (permissions, missing files) |
| Use streams for large files         | Lower memory usage                                |
| Specify encoding (`utf8`)           | Prevents `Buffer` output                          |

---

## 🧪 Sample File: `myfile.txt`

```
This is the content of myfile.txt
```

Here’s a polished, well-organized guide for **Node.js Path Module**, structured similarly to your previous summaries:

---

# Node.js Path Module


## What Is the Path Module?

The **Path module** is a built-in Node.js utility for handling and manipulating file paths in a platform-agnostic manner. Since different operating systems use different path separators (e.g., `/` on POSIX vs `\` on Windows), the Path module helps ensure code runs reliably everywhere.

**Key Capabilities:**

* Cross-platform path handling
* Path manipulation and normalization
* Extension extraction and parsing
* Path resolution and joining
* Safe treatment of relative and absolute paths

---

## Importing the Path Module

### CommonJS (Default)

```js
const path = require('path');
const { join, resolve, basename } = require('path');
```

### ES Modules (Node.js 14+ with `"type": "module"`)

```js
import path from 'path';
import { join, resolve, basename } from 'path';
```

---

## Core Path Methods

* **`path.basename(path[, ext])`** – Gets filename from path.

  Example:

  ```js
  path.basename('/users/docs/file.txt');           // => 'file.txt'
  path.basename('/users/docs/file.txt', '.txt');   // => 'file'
  ```

---

### `__dirname` & `__filename`

#### CommonJS

```js
console.log(__dirname);  // Current directory
console.log(__filename); // Current file path
const configPath = path.join(__dirname, 'config', 'app-config.json');
```

#### ES Modules

```js
import { fileURLToPath } from 'url';
import { dirname } from 'path';

const __filename = fileURLToPath(import.meta.url);
const __dirname  = dirname(__filename);
```

---

### Other Essential Methods

| Method              | Description                                              |
| ------------------- | -------------------------------------------------------- |
| `path.extname()`    | Returns file extension (e.g., `.txt`)                    |
| `path.join()`       | Joins segments, normalizing separators                   |
| `path.resolve()`    | Produces absolute path from segments, right-to-left      |
| `path.parse()`      | Splits path into `{ root, dir, base, name, ext }` object |
| `path.format()`     | Builds a path string from parsed object                  |
| `path.normalize()`  | Cleans up path (`../`, `./`, duplicate slashes)          |
| `path.relative()`   | Gives relative path from one location to another         |
| `path.isAbsolute()` | Checks if a path is absolute                             |

---

### Examples at a Glance

```js
path.join('/users', '..', 'data', 'file.txt');
// Use resolve to get absolute paths relative to current module
path.resolve(__dirname, 'config/app.json');

// Normalize messy paths
path.normalize('/users//docs/../data/file.txt');

// Find relative path
path.relative('/var/www', '/var/www/static/logo.png');  // => 'static/logo.png'

// Check absoluteness
path.isAbsolute('/etc/passwd');  // true (POSIX)
path.isAbsolute('C:\\Windows');  // true (Windows)
```

---

### Platform-Specific Tools

* **`path.sep`** – OS-specific path separator (`\` or `/`)
* **`path.delimiter`** – Delimiter for PATH-like env vars (`;` on Windows, `:` on POSIX)
* **`path.win32` / `path.posix`** – Force Windows or POSIX behavior regardless of OS

---

## Best Practices & Use Cases

* Use `join()`, `resolve()`, etc., instead of string concatenation for reliability.
* On ES Modules, recreate `__dirname` using `import.meta.url`.
* Resolve possible path-traversal risks:

  ```js
  function safeJoin(base, ...paths) {
    const final = path.normalize(path.join(base, ...paths));
    if (!final.startsWith(path.resolve(base))) throw new Error('Invalid path');
    return final;
  }
  ```
* Handle cross-platform file management (e.g., user data directories, config paths).

---

## Real-World Example: Directory Setup & Logging

```js
const path = require('path');
const fs   = require('fs/promises');

const dirs = ['logs', 'public', 'uploads'].map(d =>
  path.join(__dirname, '..', d)
);

async function ensureDirs() {
  await Promise.all(dirs.map(d => fs.mkdir(d, { recursive: true })));
}

async function log(message) {
  const logFile = path.join(dirs[0], `${new Date().toISOString().slice(0, 10)}.log`);
  await fs.appendFile(logFile, `[${new Date().toISOString()}] ${message}\n`, 'utf8');
}
```

---

## Summary

The Path module is a powerhouse for managing file paths safely and cross-platform:

* Parse, format, normalize, and resolve paths
* Safe file operations with correct path resolution
* Compatible across different OS platforms
* Recommend avoiding manual concatenation; lean on built-in Path APIs instead


# Node.js OS Module


## 📦 Getting Started

### Importing the Module

```js
// CommonJS (default in Node.js)
const os = require('os');

// ES Modules (Node.js 14+ or with "type": "module" in package.json)
import os from 'os';
// or
import { arch, platform, cpus } from 'os';
```

---

## 🔧 Key Features

* Retrieve system information (CPU, memory, platform, etc.)
* Access user and network information
* Work with file paths and directories
* Monitor system resources
* Handle OS signals and errors

---

## 🖥️ System Information

### `os.arch()`

Returns the CPU architecture:

```js
console.log(`CPU Architecture: ${os.arch()}`);
```

Common values: `x64`, `arm`, `arm64`, `ia32`, `mips`

---

### `os.platform()`

Returns the OS platform:

```js
console.log(`Platform: ${os.platform()}`);
```

Examples: `darwin`, `win32`, `linux`

---

### `os.type()`

Returns the OS name:

```js
console.log(`OS Type: ${os.type()}`);
```

Examples: `Linux`, `Darwin`, `Windows_NT`

---

### `os.release()`

Returns the OS version:

```js
console.log(`OS Release: ${os.release()}`);
```

---

### `os.version()`

Returns kernel version:

```js
console.log(`Kernel Version: ${os.version()}`);
```

---

## 👤 User and Environment

### `os.userInfo()`

Returns current user info:

```js
const user = os.userInfo();
console.log(`Username: ${user.username}`);
```

---

### `os.homedir()`

Returns the user's home directory:

```js
console.log(`Home Directory: ${os.homedir()}`);
```

---

### `os.hostname()`

Returns the system hostname:

```js
console.log(`Hostname: ${os.hostname()}`);
```

---

### `os.tmpdir()`

Returns the system temp directory:

```js
console.log(`Temporary Directory: ${os.tmpdir()}`);
```

---

## ⚙️ System Resources

### `os.cpus()`

Returns details of each logical CPU core:

```js
const cpus = os.cpus();
console.log(`CPU Cores: ${cpus.length}`);
```

---

### `os.totalmem()` and `os.freemem()`

Returns total and free system memory (in bytes):

```js
function formatBytes(bytes) {
  const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
  if (bytes === 0) return '0 Bytes';
  const i = Math.floor(Math.log(bytes) / Math.log(1024));
  return `${(bytes / Math.pow(1024, i)).toFixed(2)} ${sizes[i]}`;
}
```

---

### `os.loadavg()`

Returns system load averages:

```js
console.log(os.loadavg()); // [1min, 5min, 15min]
```

Note: Only available on Unix-like systems.

---

## 🌐 Network Information

### `os.networkInterfaces()`

Returns network interfaces and their details:

```js
const networkInterfaces = os.networkInterfaces();
console.log(JSON.stringify(networkInterfaces, null, 2));
```

Get primary non-internal IPv4 address:

```js
function getPrimaryIPv4Address() {
  for (const name in os.networkInterfaces()) {
    for (const iface of os.networkInterfaces()[name]) {
      if (iface.family === 'IPv4' && !iface.internal) return iface.address;
    }
  }
  return '127.0.0.1';
}
```

---

## ⏱️ Uptime

### `os.uptime()`

Returns system uptime (in seconds):

```js
const uptime = os.uptime();
```

Format into readable time:

```js
function formatUptime(seconds) {
  const days = Math.floor(seconds / 86400);
  const hours = Math.floor((seconds % 86400) / 3600);
  const minutes = Math.floor((seconds % 3600) / 60);
  const secs = seconds % 60;
  return `${days}d ${hours}h ${minutes}m ${secs}s`;
}
```

---

## 🧭 Constants and Utilities

### `os.constants`

Useful for signals and error codes:

```js
console.log(os.constants.signals);
```

Handle shutdown signals:

```js
process.on('SIGINT', () => process.exit(0));
process.on('SIGTERM', () => process.exit(0));
```

---

### `os.EOL`

Returns the EOL character for current OS:

```js
console.log(JSON.stringify(os.EOL));
```

---

## ✅ Best Practices

### 1. Use `path.join()` for file paths

```js
const path = require('path');
const configPath = path.join(os.homedir(), 'app', 'config.json');
```

---

### 2. Use `os.EOL` when writing files

```js
const content = `Line1${os.EOL}Line2`;
fs.writeFileSync('file.txt', content);
```

---

### 3. Check Memory Availability

```js
if (os.freemem() < 500 * 1024 * 1024) {
  console.warn('Low memory!');
}
```

---

## 📊 Practical Examples

### 🖥️ System Info Dashboard

```js
function getSystemInfo() {
  return {
    os: {
      type: os.type(),
      platform: os.platform(),
      arch: os.arch(),
      release: os.release(),
      hostname: os.hostname(),
      uptime: formatUptime(os.uptime()),
    },
    user: {
      username: os.userInfo().username,
      homedir: os.homedir(),
    },
    memory: {
      total: formatBytes(os.totalmem()),
      free: formatBytes(os.freemem()),
      usage: `${((1 - os.freemem() / os.totalmem()) * 100).toFixed(2)}%`,
    },
    cpu: {
      model: os.cpus()[0].model,
      cores: os.cpus().length,
    },
  };
}

console.log(JSON.stringify(getSystemInfo(), null, 2));
```

---

### 📈 Resource Monitor (Live)

```js
setInterval(() => {
  console.clear();
  console.log(`Time: ${new Date().toLocaleTimeString()}`);
  console.log(`Memory Usage: ${(os.totalmem() - os.freemem()) / 1024 / 1024} MB`);
}, 1000);
```

---

### 🎯 Platform-Specific Behavior

```js
function getAppDataPath(appName) {
  switch (os.platform()) {
    case 'win32': return path.join(process.env.APPDATA, appName);
    case 'darwin': return path.join(os.homedir(), 'Library', 'Application Support', appName);
    case 'linux': return path.join(os.homedir(), '.config', appName);
    default: return path.join(os.homedir(), `.${appName}`);
  }
}
```

---

## 📝 Summary

The `os` module helps your Node.js applications:

* Get detailed system and user info
* React to system load, memory, and CPU state
* Adapt behavior based on platform
* Stay portable across Windows/macOS/Linux

This is especially useful for:

* Diagnostic tools
* System monitors
* Cross-platform utilities







