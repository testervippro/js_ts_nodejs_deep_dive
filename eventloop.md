
##  All Components of the JavaScript Event Loop System

```
+----------------------------+
|        JavaScript Engine   |
|                            |
|  +----------------------+  |
|  |      Call Stack       | <--- Executes all synchronous code
|  +----------------------+  |
|                            |
|  +----------------------+  |
|  |   Microtask Queue     | <--- e.g. Promise.then(), async/await
|  +----------------------+  |
|                            |
|  +----------------------+  |
|  |   Event Loop          | <--- Watches call stack, runs queues
|  +----------------------+  |
+----------------------------+

        ↓ interacts with

+----------------------------+
|   Browser / Node APIs      | <--- Timers, DOM events, HTTP, etc.
|                            |
|  e.g., setTimeout, fetch,  |
|        DOM event listener  |
+----------------------------+

        ↓ sends to

+----------------------------+
|    Macrotask Queue         | <--- setTimeout, setInterval, I/O
+----------------------------+
```

---

## 🧱 Breakdown of Each Component

| **Component**                             | **Purpose**                                                                 |
| ----------------------------------------- | --------------------------------------------------------------------------- |
| **Call Stack**                            | Executes all synchronous code, function-by-function                         |
| **Web APIs (Browser APIs)**               | Handles async actions like `setTimeout`, `fetch`, `addEventListener`        |
| **Event Loop**                            | Monitors the call stack and queues; pushes tasks into stack when it's empty |
| **Microtask Queue**                       | High-priority queue: `Promise.then`, `catch`, `async/await` results         |
| **Macrotask Queue**                       | Lower priority: `setTimeout`, `setInterval`, DOM events, I/O                |
| **Job Queue** (alias for Microtask Queue) | Used in specs to refer to microtasks                                        |

## What the Event Loop Does
The event loop is a mechanism that:

Monitors the Call Stack – the stack of functions currently being executed.

Monitors the Task Queues – like the Microtask Queue and Macrotask Queue.

Keeps checking:

If the call stack is empty, meaning JavaScript isn't executing anything right now...

It will take the next task from one of the queues and push it onto the call stack to be executed.

 So, the event loop acts like a traffic controller — it manages what code runs when, especially after asynchronous events.
`

## 🕒 Order of Execution

1. Run all synchronous code in **Call Stack**
2. Run all **Microtasks**

   * `.then()`, `catch`, `finally`, `queueMicrotask`
3. Run **one task** from Macrotask Queue
4. Repeat the loop

---

## 🧪 Code to Demonstrate All Components

```javascript
console.log('1 - sync');

setTimeout(() => {
  console.log('2 - setTimeout'); // macrotask
}, 0);

Promise.resolve().then(() => {
  console.log('3 - promise'); // microtask
});

queueMicrotask(() => {
  console.log('4 - microtask'); // microtask
});

console.log('5 - sync end');
```

---

### 🧾 Output:

```
1 - sync
5 - sync end
3 - promise
4 - microtask
2 - setTimeout
```

---

### 📊 Execution Flow:

1. `1 - sync` → call stack
2. `setTimeout` → goes to **Web API**, then **macrotask queue**
3. `Promise.then` → goes to **microtask queue**
4. `queueMicrotask` → also goes to **microtask queue**
5. `5 - sync end` → call stack finishes
6. Event Loop:

   * Executes **microtasks** first → 3, 4
   * Then executes **macrotask** → 2

---

## 🚀 Summary Table

| **Component**   | **Examples**                     | **Executes when**             |
| --------------- | -------------------------------- | ----------------------------- |
| Call Stack      | All synchronous functions        | Immediately                   |
| Microtask Queue | `Promise.then`, `async/await`    | After sync, before macrotasks |
| Macrotask Queue | `setTimeout`, `setInterval`, I/O | After microtasks              |
| Web APIs        | `setTimeout`, `fetch`, `DOM`     | Browser handles it            |
| Event Loop      | Scheduler of task execution      | Always running                |


###  🌀 All Components in the Node.js Event Loop

The Node.js Event Loop has **6 main phases**, plus a **microtask queue**. Here's how it works:

---

### 🔸 1. **Timers Phase**

* Executes callbacks scheduled by:

  * `setTimeout()`
  * `setInterval()`
* These callbacks run **once the timer has expired** (not immediately after the delay).

```js
setTimeout(() => {
  console.log('Timers Phase: setTimeout');
}, 0);
```

---

### 🔸 2. **Pending Callbacks Phase**

* Executes I/O callbacks **deferred to the next loop iteration**.
* Includes:

  * Some TCP errors
  * `setTimeout()`/`setImmediate()` in specific edge cases

> Not commonly used directly in user code.

---

### 🔸 3. **Idle, Prepare Phase** *(Internal Use Only)*

* Internal phase used by Node.js.
* Prepares the system for the next poll phase.

> You won’t interact with this phase directly.

---

### 🔸 4. **Poll Phase**

* Retrieves new I/O events.
* Executes callbacks for I/O like:

  * `fs.readFile()`
  * `net`, `http`, `DNS` callbacks
* If there are no timers or `setImmediate()`, it **waits (blocks)** here for I/O.

```js
const fs = require('fs');
fs.readFile(__filename, () => {
  console.log('Poll Phase: fs.readFile');
});
```

---

### 🔸 5. **Check Phase**

* Executes `setImmediate()` callbacks.

```js
setImmediate(() => {
  console.log('Check Phase: setImmediate');
});
```

---

### 🔸 6. **Close Callbacks Phase**

* Executes callbacks for closed resources:

  * `socket.on('close', ...)`
  * `stream.on('close', ...)`

```js
const net = require('net');
const server = net.createServer();
server.on('close', () => {
  console.log('Close Callbacks Phase: server closed');
});
server.close();
```

---

## 🔹 Microtasks Queue (Runs After Each Phase)

Includes:

* `process.nextTick()` → runs **before all other microtasks**
* `Promise.then()` / `await` → runs **after `process.nextTick()`**

```js
process.nextTick(() => {
  console.log('Microtask: process.nextTick');
});

Promise.resolve().then(() => {
  console.log('Microtask: Promise.then');
});
```

---

## 🔸 Diagram: Event Loop Phases (Simplified Flow)

```text
| Start of Tick |
      ↓
[process.nextTick()]
      ↓
[Promise.then() / await]
      ↓
1. Timers
2. Pending Callbacks
3. Idle, Prepare
4. Poll
5. Check
6. Close Callbacks
      ↓
| Next Tick |
```

---

## ✅ Summary Table

| Phase             | Callback Types                         | Priority Order          |
| ----------------- | -------------------------------------- | ----------------------- |
| Microtasks        | `process.nextTick()`, `Promise.then()` | 🔝 Highest              |
| Timers            | `setTimeout()`, `setInterval()`        | Normal (based on delay) |
| Pending Callbacks | Internal deferred I/O                  | Rarely used manually    |
| Idle/Prepare      | Internal (V8/Node.js prep work)        | -                       |
| Poll              | I/O (fs, net, http...)                 | Medium (may block)      |
| Check             | `setImmediate()`                       | After Poll              |
| Close Callbacks   | `.on('close')`                         | Last phase              |



