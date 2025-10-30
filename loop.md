Excellent — this is exactly where `Object.keys()` becomes powerful 💪

Let’s explore **how it works with nested objects**, because in your API JSON, the structure is *nested multiple levels deep* (like `paths → methods → details`).

---

## 🧩 1️⃣ Simple Flat Object

```js
const user = { name: 'Alice', age: 25, country: 'Vietnam' };

console.log(Object.keys(user));
```

👉 Output:

```js
["name", "age", "country"]
```

✅ Works because `user` is a **flat (non-nested)** object.

---

## 🧩 2️⃣ Nested Object Example

Let’s add a nested level:

```js
const person = {
  name: 'Alice',
  contact: {
    email: 'alice@example.com',
    phone: '123456789'
  }
};
```

Now try:

```js
console.log(Object.keys(person));
```

👉 Output:

```js
["name", "contact"]
```

> It does **not** automatically go inside `contact`.
> `Object.keys()` only gets the *top-level* keys.

---

### To get **nested keys**, you must explicitly go deeper:

```js
const contactKeys = Object.keys(person.contact);
console.log(contactKeys);
```

👉 Output:

```js
["email", "phone"]
```

✅ So you can combine calls:

```js
Object.keys(person).forEach(key => {
  console.log('Level 1 key:', key);

  if (typeof person[key] === 'object') {
    Object.keys(person[key]).forEach(subKey => {
      console.log('  Level 2 key:', subKey);
    });
  }
});
```

👉 Output:

```
Level 1 key: name
Level 1 key: contact
  Level 2 key: email
  Level 2 key: phone
```

---

## 🧠 How It Works Internally

`Object.keys(obj)` works only on **one object level**.
If you want to go through **nested** objects, you must call it **recursively** (call it again inside the loop).

---

## 🧱 3️⃣ Example: Nested API JSON (your case)

Here’s a simplified version of your `api.json`:

```json
{
  "paths": {
    "/api/v2/admin/customer/companies": {
      "get": {
        "summary": "Get companies list",
        "description": "Retrieve a paginated list"
      },
      "post": {
        "summary": "Create company"
      }
    }
  }
}
```

---

### 🔍 Let’s visualize the structure:

```
jsonData
 └── paths
      └── "/api/v2/admin/customer/companies"
            ├── get
            │    ├── summary
            │    └── description
            └── post
                 └── summary
```

---

### 💻 Using `Object.keys()` Step by Step:

#### Level 1:

```js
Object.keys(jsonData.paths)
// → ["/api/v2/admin/customer/companies"]
```

#### Level 2:

```js
const path = "/api/v2/admin/customer/companies";
Object.keys(jsonData.paths[path])
// → ["get", "post"]
```

#### Level 3:

```js
const method = "get";
Object.keys(jsonData.paths[path][method])
// → ["summary", "description"]
```

✅ You can keep going deeper by nesting calls, each level drilling into the next object.

---

## 🧩 4️⃣ General Recursive Example

Here’s how to handle **any depth** dynamically:

```js
function printAllKeys(obj, level = 0) {
  Object.keys(obj).forEach(key => {
    console.log(' '.repeat(level * 2) + key); // indent based on depth
    if (typeof obj[key] === 'object' && obj[key] !== null) {
      printAllKeys(obj[key], level + 1);
    }
  });
}

const data = {
  a: 1,
  b: { x: 10, y: { m: 100, n: 200 } },
  c: { z: 30 }
};

printAllKeys(data);
```

👉 Output:

```
a
b
  x
  y
    m
    n
c
  z
```

So you can see:

* `Object.keys()` gets one level.
* A **recursive function** keeps calling itself to dive deeper.

---

## 🧩 5️⃣ In Your Case (API JSON)

You’re essentially doing this manually with two levels:

```js
Object.keys(jsonData.paths).forEach(path => {
  const methods = jsonData.paths[path]; // 2nd level
  Object.keys(methods).forEach(method => {
    const details = methods[method]; // 3rd level
    console.log(method.toUpperCase(), path, details.summary);
  });
});
```

If you wanted to go even deeper (e.g., into `parameters`), you’d add another `Object.keys()` loop inside that.

---

## 🧾 Summary Table

| Level | Object.keys() called on        | Example output                             | Meaning        |
| ----- | ------------------------------ | ------------------------------------------ | -------------- |
| 1     | `jsonData.paths`               | `["/api/v2/admin/customer/companies"]`     | API endpoints  |
| 2     | `jsonData.paths[path]`         | `["get", "post"]`                          | HTTP methods   |
| 3     | `jsonData.paths[path][method]` | `["summary", "description", "parameters"]` | method details |

---

Would you like me to show a **recursive version** that automatically prints all nested keys from your JSON (no matter how deep)?
