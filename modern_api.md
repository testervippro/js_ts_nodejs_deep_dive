
# **Node.js Modern API Cheat Sheet (ESM + Promise-based)**

---

## **1. File System (`fs/promises`)**

```ts
import * as fs from "node:fs/promises";

// File operations
await fs.readFile("file.txt", "utf-8");
await fs.writeFile("file.txt", "Hello");
await fs.appendFile("file.txt", "More");
await fs.unlink("file.txt");

// Directory operations
await fs.mkdir("dir", { recursive: true });
await fs.readdir("dir");
await fs.rm("dir", { recursive: true, force: true });
await fs.rename("old.txt", "new.txt");

// File info
const stats = await fs.stat("file.txt");
stats.isFile(); stats.size;
await fs.access("file.txt");

// File handles (advanced)
const fh = await fs.open("file.txt", "r+");
await fh.read(Buffer.alloc(10), 0, 10, 0);
await fh.write("data", 0);
await fh.close();
```

---

## **2. Path (`path`)**

```ts
import path from "path";

const joinPath = path.join("dir", "file.txt");
const resolvePath = path.resolve("dir", "file.txt");
const ext = path.extname("file.txt");
const base = path.basename("/dir/file.txt");
const dir = path.dirname("/dir/file.txt");
```

---

## **3. OS & System Info (`os`)**

```ts
import os from "os";

os.cpus();
os.arch();
os.platform();
os.freemem();
os.totalmem();
os.tmpdir();
os.userInfo();
```

---

## **4. Process**

```ts
console.log(process.env);   // env variables
console.log(process.argv);  // CLI args
console.log(process.cwd()); // current working directory
process.exit(0);            // exit
process.on("uncaughtException", (err) => console.error(err));
process.on("unhandledRejection", (err) => console.error(err));
```

---

## **5. Child Processes (Promise-based)**

```ts
import { exec, execFile, spawn } from "node:child_process";
import { promisify } from "node:util";

const execAsync = promisify(exec);
const { stdout, stderr } = await execAsync("ls -la");

// spawn for streaming output
const child = spawn("ls", ["-la"]);
child.stdout.on("data", console.log);
child.stderr.on("data", console.error);
```

---

## **6. HTTP / HTTPS**

```ts
import http from "node:http";
import https from "node:https";
import { request } from "node:https";

// Async fetch-like (Node 18+)
const res = await fetch("https://example.com");
const text = await res.text();
const json = await res.json();
```

---

## **7. Timers (Promise-based)**

```ts
import { setTimeout as sleep, setInterval } from "node:timers/promises";

// Sleep
await sleep(1000);

// Interval async iterator
for await (const _ of setInterval(1000)) {
  console.log("tick");
}
```

---

## **8. Crypto (modern Promise API)**

```ts
import crypto from "node:crypto";

// Random bytes
const buf = crypto.randomBytes(16);

// Hash
const hash = crypto.createHash("sha256").update("data").digest("hex");

// Key generation (Promise)
const keyPair = await crypto.webcrypto.subtle.generateKey(
  { name: "RSA-PSS", modulusLength: 2048, publicExponent: new Uint8Array([1,0,1]), hash: "SHA-256" },
  true,
  ["sign", "verify"]
);
```

---

## **9. URL & Query String**

```ts
import { URL, URLSearchParams } from "node:url";

const myUrl = new URL("https://example.com/path?x=1&y=2");
myUrl.pathname; myUrl.searchParams.get("x");

const params = new URLSearchParams({ a: "1", b: "2" });
params.toString(); // "a=1&b=2"
```

---

## **10. Streams (Promise-friendly)**

```ts
import { pipeline } from "node:stream/promises";
import fs from "node:fs/promises";
import { createReadStream, createWriteStream } from "node:fs";

// Async pipeline
await pipeline(
  createReadStream("input.txt"),
  createWriteStream("output.txt")
);
```

---

## **11. Events**

```ts
import { EventEmitter } from "node:events";

const emitter = new EventEmitter();
emitter.on("data", console.log);

// Async iterator
for await (const data of emitter) {
  console.log(data);
}
```

---

## **12. Node Utilities**

```ts
import { promisify } from "node:util";

const sleep = promisify(setTimeout);
await sleep(1000);

const debuglog = require("util").debuglog("app");
debuglog("Debug info");
```

---

## **13. Worker Threads**

```ts
import { Worker } from "node:worker_threads";

const worker = new Worker(new URL("./worker.js", import.meta.url));
worker.on("message", console.log);
worker.postMessage({ data: 123 });
```

---

## **14. Modern ES Module Support**

```ts
import { fileURLToPath } from "node:url";
import { dirname } from "node:path";

const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);
```

---

## ⚡ Notes

* Node.js 16+ encourages **Promise-based APIs** (`fs/promises`, `timers/promises`) and **ESM imports**.
* Avoid sync APIs (`readFileSync`, `statSync`) in production code; use async/await instead.
* Use `fetch` (Node 18+) for HTTP requests instead of old `http`/`https` callbacks.

---

