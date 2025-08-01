
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

---

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




Would you like a **diagram**, or a version of this explanation tailored to **Node.js** instead of the browser?
