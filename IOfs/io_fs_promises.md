

## 📂 Import

```ts
import { promises as fs } from "fs"; // or
import * as fs from "fs/promises";
```

---

## 📄 File Read/Write

```ts
// Read file (utf-8 text)
const data = await fs.readFile("file.txt", "utf8");

// Write (overwrite) file
await fs.writeFile("file.txt", "Hello world");

// Append to file
await fs.appendFile("file.txt", "\nAnother line");
```

---

## 📁 Directory Operations

```ts
// Make directory (recursive = true -> create parents)
await fs.mkdir("myDir", { recursive: true });

// Read directory
const files = await fs.readdir("myDir");
console.log(files);

// Remove directory (Node 14.14+)
await fs.rm("myDir", { recursive: true, force: true });
```

---

## 🔍 File Info

```ts
// Check if file exists (wrap in try/catch)
try {
  await fs.access("file.txt");
  console.log("File exists");
} catch {
  console.log("File does not exist");
}

// Get file stats
const stats = await fs.stat("file.txt");
console.log(stats.isFile(), stats.size);
```

---

## 📝 File Rename / Delete / Copy

```ts
// Rename / move
await fs.rename("old.txt", "new.txt");

// Delete file
await fs.unlink("file.txt");

// Copy file
await fs.copyFile("source.txt", "dest.txt");
```

---

## 🔒 File Handles (advanced)

```ts
// Open file
const handle = await fs.open("file.txt", "r+");

// Read from position
const buffer = Buffer.alloc(10);
await handle.read(buffer, 0, 10, 0);

// Write at position
await handle.write("Hello", 0);

// Close file
await handle.close();
```

---

## ⚡ Differences vs `fs` (callback API)

| `fs` (callback)               | `fs/promises` (async/await)      |
| ----------------------------- | -------------------------------- |
| `fs.readFile("f", cb)`        | `await fs.readFile("f")`         |
| `fs.writeFile("f", data, cb)` | `await fs.writeFile("f", data)`  |
| Harder to chain               | Easy to chain with `async/await` |
| Legacy style                  | Modern, recommended              |

---

👉 Best practice: always wrap `fs/promises` in `try/catch` to handle errors.


