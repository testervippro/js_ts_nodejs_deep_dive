Excellent question 💡 — comparing **native JavaScript `Object` methods** (like `Object.keys()`, `Object.entries()`, etc.) with **Lodash** equivalents reveals some very important differences in behavior, especially when working with **nested objects**, **arrays**, or **non-enumerable properties**.

Let’s go step-by-step.

---

# ⚖️ **Comparison: Native `Object` vs Lodash (`_.`)**

| **Feature / Use Case**               | **Native JS (`Object`)**                              | **Lodash (`_`)**                                                              | **Key Differences / Notes**                                                            |
| ------------------------------------ | ----------------------------------------------------- | ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| 🔹 **Get Keys (Shallow)**            | `Object.keys(obj)`                                    | `_.keys(obj)`                                                                 | Both return **enumerable** own property names only. No difference for shallow objects. |
| 🔹 **Get Keys (Deep)**               | ❌ Manual recursion required                           | ✅ `_.keysIn()` (includes inherited) or `_.flatMapDeep()` for nested traversal | Lodash offers deeper and easier recursion options (`_.flatMapDeep`, `_.mapKeys`).      |
| 🔹 **Get Values**                    | `Object.values(obj)`                                  | `_.values(obj)`                                                               | Both return enumerable values (only first level).                                      |
| 🔹 **Get Entries (key-value pairs)** | `Object.entries(obj)`                                 | `_.toPairs(obj)`                                                              | Same output format. Lodash supports array-like and objects interchangeably.            |
| 🔹 **Convert back to object**        | `Object.fromEntries(pairs)`                           | `_.fromPairs(pairs)`                                                          | Equivalent functionality. Lodash is slightly more flexible with arrays.                |
| 🔹 **Deep Keys**                     | ❌ Requires recursion (manual)                         | ✅ `_.keysIn()` + `_.forOwn` / `_.forIn` / custom `_.flatMapDeep`              | Lodash simplifies recursive key extraction.                                            |
| 🔹 **Own vs Inherited Keys**         | `Object.keys()` = own only                            | `_.keysIn()` = own + inherited                                                | Lodash adds the inherited keys (like `for...in`).                                      |
| 🔹 **Has Property**                  | `Object.hasOwn(obj, key)` / `obj.hasOwnProperty(key)` | `_.has(obj, path)`                                                            | Lodash supports **deep paths** like `'user.info.name'`, native does not.               |
| 🔹 **Get Property Value**            | `obj[prop]`                                           | `_.get(obj, 'path.to.prop', defaultValue)`                                    | Lodash safely handles **undefined/null paths** and provides default fallback.          |
| 🔹 **Set Property Value**            | `obj.prop = value`                                    | `_.set(obj, 'a.b.c', value)`                                                  | Lodash supports **deep assignment** — auto-creates nested structure.                   |
| 🔹 **Check Equality**                | `Object.is(a, b)` / `a === b`                         | `_.isEqual(a, b)`                                                             | Lodash does **deep comparison** for objects, arrays, nested structures.                |
| 🔹 **Clone Object**                  | `structuredClone(obj)` / `{...obj}`                   | `_.clone()` / `_.cloneDeep()`                                                 | Lodash provides **deep cloning**, which native JS lacks (except `structuredClone`).    |
| 🔹 **Merge Objects**                 | `Object.assign(target, src)`                          | `_.merge(target, src)`                                                        | Lodash performs **deep merge**; native does only shallow copy.                         |
| 🔹 **Pick Properties**               | `{ key: obj.key }` (manual)                           | `_.pick(obj, ['key1', 'key2'])`                                               | Lodash simplifies selective key extraction.                                            |
| 🔹 **Omit Properties**               | Manual destructuring                                  | `_.omit(obj, ['key'])`                                                        | Lodash makes removing specific keys easy.                                              |
| 🔹 **Map Over Object**               | `Object.entries(obj).map(...)`                        | `_.mapValues(obj, fn)` / `_.mapKeys(obj, fn)`                                 | Lodash directly supports object mapping.                                               |
| 🔹 **Flatten Keys (Deep)**           | Custom recursive function                             | `_.flatMapDeep(obj)`                                                          | Lodash provides built-in recursion utilities.                                          |

---

## 🧠 **Example: `Object.keys()` vs `_.keys()` vs `_.keysIn()`**

```js
const data = Object.create({ inherited: true });
data.name = "Alice";
data.info = { age: 30, city: "Tokyo" };

// Native
console.log(Object.keys(data));   // ['name', 'info']

// Lodash _.keys() - same as Object.keys
console.log(_.keys(data));        // ['name', 'info']

// Lodash _.keysIn() - includes inherited
console.log(_.keysIn(data));      // ['name', 'info', 'inherited']
```

---

## 🧩 **Deep Path Handling Example**

### ✅ Lodash

```js
const user = { profile: { address: { city: "Osaka" } } };

console.log(_.get(user, "profile.address.city"));       // "Osaka"
console.log(_.get(user, "profile.phone", "No phone"));  // "No phone"

_.set(user, "profile.address.zip", "540-0000");
console.log(user.profile.address.zip); // "540-0000"
```

### ❌ Native JS

```js
// Causes error if path doesn’t exist:
console.log(user.profile.phone.number); // ❌ TypeError

// Manual check required:
console.log(user?.profile?.phone ?? "No phone");
```

---

## 🧩 **Deep Keys Example**

### Native (Manual Recursive)

```js
function deepKeys(obj, prefix = '') {
  return Object.keys(obj).flatMap(k =>
    typeof obj[k] === 'object' && obj[k] !== null
      ? deepKeys(obj[k], `${prefix}${k}.`)
      : `${prefix}${k}`
  );
}

const data = { user: { name: 'Bob', address: { city: 'Tokyo' } }, active: true };
console.log(deepKeys(data));
// ['user.name', 'user.address.city', 'active']
```

### Lodash (Simpler)

```js
function lodashDeepKeys(obj, prefix = '') {
  return _.flatMap(_.keys(obj), k =>
    _.isObject(obj[k]) ? lodashDeepKeys(obj[k], `${prefix}${k}.`) : `${prefix}${k}`
  );
}

console.log(lodashDeepKeys(data));
// ['user.name', 'user.address.city', 'active']
```

---

## ⚡ **Performance & Behavior Summary**

| **Feature**               | **Native `Object`**    | **Lodash (`_`)**                  |
| ------------------------- | ---------------------- | --------------------------------- |
| Shallow operations        | Fastest (built-in C++) | Slightly slower                   |
| Deep operations           | Requires recursion     | Easier with built-ins             |
| Handles nested paths      | ❌ No                   | ✅ Yes (`_.get`, `_.set`, `_.has`) |
| Safe for `undefined/null` | ❌ No                   | ✅ Yes                             |
| Supports default values   | ❌ No                   | ✅ Yes                             |
| Works with arrays too     | Partially              | ✅ Fully                           |
| Includes inherited props  | ❌ Only via manual      | ✅ `_.keysIn()`                    |
| Library dependency        | None                   | Requires Lodash import            |

---

## 🧭 **Quick Visual Summary**

| Task                           | Native                 | Lodash                     |
| ------------------------------ | ---------------------- | -------------------------- |
| Get shallow keys               | `Object.keys(obj)`     | `_.keys(obj)`              |
| Get all keys (own + inherited) | manual                 | `_.keysIn(obj)`            |
| Get deep value safely          | manual                 | `_.get(obj, 'a.b.c')`      |
| Set deep value                 | manual                 | `_.set(obj, 'a.b.c', 123)` |
| Check nested path              | manual                 | `_.has(obj, 'a.b.c')`      |
| Deep clone                     | `structuredClone(obj)` | `_.cloneDeep(obj)`         |
| Deep merge                     | manual recursion       | `_.merge(obj1, obj2)`      |
| Pick keys                      | manual                 | `_.pick(obj, ['x', 'y'])`  |
| Omit keys                      | manual                 | `_.omit(obj, ['x', 'y'])`  |

---

## 🧩 **When to Use Which**

| Use Case                                         | Recommendation                             |
| ------------------------------------------------ | ------------------------------------------ |
| Simple, shallow key/value extraction             | ✅ **Native `Object` methods**              |
| Working with nested structures                   | ✅ **Lodash (`_.get`, `_.set`, `_.merge`)** |
| Defensive programming (avoid `undefined` errors) | ✅ **Lodash**                               |
| High-performance operations (tight loops)        | ✅ **Native**                               |
| Readability & maintainability                    | ✅ **Lodash**                               |

---

### TL;DR Summary

> * Use **`Object.keys()`** for **simple objects**.
> * Use **`_.get()`**, **`_.set()`**, or **`_.merge()`** for **nested or unpredictable data**.
> * Lodash simplifies deep work; native JS is faster for shallow operations.

---

Would you like me to show a **side-by-side visual comparison table** (JS vs Lodash) for *deep object manipulation* (like nested merge, clone, and get/set paths)? It’s great for QA/test automation reference sheets.
