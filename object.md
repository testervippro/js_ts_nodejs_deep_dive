Excellent point 💡 — `Object.keys()` behaves differently when dealing with **nested objects** or **non-enumerable properties**.
Here’s the **updated and extended JavaScript `Object` Methods Cheat Sheet**, including **special cases for `Object.keys()`** (with examples that cover deep and nested structures).

---

# 💎 **JavaScript `Object` Methods Cheat Sheet (with Nested Object Examples)**

---

## 📘 **1. Static Methods of `Object`**

*(Used like `Object.methodName(obj)`)*

| **Method**                                        | **Description**                                                 | **Example**                                                                                                                                       |
| ------------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`Object.assign(target, ...sources)`**           | Copies properties from one or more sources into a target object | `js const target = {a: 1}; const source = {b: 2}; Object.assign(target, source); console.log(target); // {a: 1, b: 2} `                           |
| **`Object.create(proto)`**                        | Creates a new object with the specified prototype               | `js const proto = {greet() {return "Hi";}}; const obj = Object.create(proto); console.log(obj.greet()); // "Hi" `                                 |
| **`Object.defineProperty(obj, key, descriptor)`** | Defines or modifies a property with custom settings             | `js const obj = {}; Object.defineProperty(obj, 'age', {value: 25, writable: false}); console.log(obj.age); // 25 `                                |
| **`Object.defineProperties(obj, props)`**         | Defines multiple properties at once                             | `js const user = {}; Object.defineProperties(user, { name: {value: 'Alice'}, age: {value: 30} }); console.log(user.name, user.age); // Alice 30 ` |
| **`Object.entries(obj)`**                         | Returns an array of `[key, value]` pairs                        | `js const person = {name: 'Alice', age: 30}; console.log(Object.entries(person)); // [['name','Alice'], ['age',30]] `                             |
| **`Object.fromEntries(arr)`**                     | Converts an array of `[key, value]` pairs into an object        | `js const entries = [['x', 10], ['y', 20]]; console.log(Object.fromEntries(entries)); // {x:10, y:20} `                                           |
| **`Object.freeze(obj)`**                          | Freezes an object (no add/remove/change allowed)                | `js const car = {brand: 'Toyota'}; Object.freeze(car); car.brand = 'Honda'; console.log(car.brand); // Toyota `                                   |
| **`Object.seal(obj)`**                            | Seals an object (no add/remove, but existing values can change) | `js const book = {title: 'JS'}; Object.seal(book); book.title = 'Python'; console.log(book.title); // Python `                                    |
| **`Object.getOwnPropertyDescriptor(obj, prop)`**  | Gets descriptor for a property                                  | `js const obj = {x: 5}; console.log(Object.getOwnPropertyDescriptor(obj, 'x')); `                                                                 |
| **`Object.getOwnPropertyDescriptors(obj)`**       | Gets all property descriptors                                   | `js const obj = {a: 1, b: 2}; console.log(Object.getOwnPropertyDescriptors(obj)); `                                                               |
| **`Object.getOwnPropertyNames(obj)`**             | Returns all property names, even non-enumerable                 | `js const obj = Object.create({}, {hidden: {value: 42, enumerable: false}}); console.log(Object.getOwnPropertyNames(obj)); // ['hidden'] `        |
| **`Object.getOwnPropertySymbols(obj)`**           | Returns all symbol properties                                   | `js const sym = Symbol('id'); const obj = {[sym]: 123}; console.log(Object.getOwnPropertySymbols(obj)); `                                         |
| **`Object.getPrototypeOf(obj)`**                  | Returns the prototype of an object                              | `js const arr = []; console.log(Object.getPrototypeOf(arr) === Array.prototype); // true `                                                        |
| **`Object.setPrototypeOf(obj, proto)`**           | Sets the prototype of an object                                 | `js const proto = {sayHi() {return 'Hi';}}; const obj = {}; Object.setPrototypeOf(obj, proto); console.log(obj.sayHi()); // Hi `                  |
| **`Object.hasOwn(obj, prop)`**                    | Checks if property exists directly on the object                | `js const user = {name: 'Bob'}; console.log(Object.hasOwn(user, 'name')); // true `                                                               |
| **`Object.is(value1, value2)`**                   | Safer alternative to `===` (handles `NaN` and `-0`)             | `js console.log(Object.is(NaN, NaN)); // true console.log(Object.is(-0, 0)); // false `                                                           |
| **`Object.isExtensible(obj)`**                    | Checks if object can be extended                                | `js const obj = {}; console.log(Object.isExtensible(obj)); // true `                                                                              |
| **`Object.preventExtensions(obj)`**               | Prevents adding new properties                                  | `js const obj = {a: 1}; Object.preventExtensions(obj); obj.b = 2; console.log(obj.b); // undefined `                                              |
| **`Object.values(obj)`**                          | Returns enumerable property values                              | `js const obj = {x: 1, y: 2}; console.log(Object.values(obj)); // [1, 2] `                                                                        |

---

### 🌟 **Special Focus: `Object.keys(obj)`**

| **Description**                                                            | **Example**                                                                                                                                                                                                                                                                                                                                                                 |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Returns **enumerable** property names of an object (only first-level keys) | `js const obj = { a: 1, b: 2 }; console.log(Object.keys(obj)); // ['a', 'b'] `                                                                                                                                                                                                                                                                                              |
| ❗ **Nested objects:** It does **not** go inside nested levels              | `js const nested = { user: { name: 'Alice', age: 30 }, active: true }; console.log(Object.keys(nested)); // ['user', 'active'] console.log(Object.keys(nested.user)); // ['name', 'age'] `                                                                                                                                                                                  |
| 🚫 **Non-enumerable properties** are **ignored**                           | `js const obj = {}; Object.defineProperty(obj, 'hidden', { value: 42, enumerable: false }); console.log(Object.keys(obj)); // [] `                                                                                                                                                                                                                                          |
| ✅ Works with arrays (returns indices as strings)                           | `js const arr = ['a', 'b', 'c']; console.log(Object.keys(arr)); // ['0', '1', '2'] `                                                                                                                                                                                                                                                                                        |
| ✅ Can be used recursively to flatten nested keys                           | ``js function deepKeys(obj, prefix = '') {   return Object.keys(obj).flatMap(k =>     typeof obj[k] === 'object' && obj[k] !== null       ? deepKeys(obj[k], `${prefix}${k}.`)       : `${prefix}${k}`   ); } const data = { user: { name: 'Alice', info: { city: 'Tokyo' } }, active: true }; console.log(deepKeys(data)); // ['user.name', 'user.info.city', 'active'] `` |

---

## 📗 **2. Instance Methods of `Object` (from `Object.prototype`)**

| **Method**                       | **Description**                                | **Example**                                                                                                     |
| -------------------------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `obj.hasOwnProperty(prop)`       | Checks if a property is directly on the object | `js const obj = {a: 1}; console.log(obj.hasOwnProperty('a')); // true `                                         |
| `obj.isPrototypeOf(obj2)`        | Checks if `obj` is in `obj2`’s prototype chain | `js const parent = {}; const child = Object.create(parent); console.log(parent.isPrototypeOf(child)); // true ` |
| `obj.propertyIsEnumerable(prop)` | Checks if a property is enumerable             | `js const obj = {a: 1}; console.log(obj.propertyIsEnumerable('a')); // true `                                   |
| `obj.toString()`                 | Returns string representation                  | `js const arr = [1,2,3]; console.log(arr.toString()); // "1,2,3" `                                              |
| `obj.valueOf()`                  | Returns primitive value                        | `js const num = new Number(5); console.log(num.valueOf()); // 5 `                                               |
| `obj.toLocaleString()`           | Locale-aware string                            | `js const num = 123456.789; console.log(num.toLocaleString('de-DE')); // "123.456,789" `                        |

---

## 🧪 **Example: Using `Object.keys()` with Nested Objects**

```js
const company = {
  name: 'TechCorp',
  departments: {
    dev: { employees: 25 },
    hr: { employees: 5 }
  }
};

console.log(Object.keys(company));        // ['name', 'departments']
console.log(Object.keys(company.departments)); // ['dev', 'hr']

// Recursively extract all keys:
function getAllKeys(obj) {
  let keys = [];
  for (const key of Object.keys(obj)) {
    keys.push(key);
    if (typeof obj[key] === 'object' && obj[key] !== null) {
      keys = keys.concat(getAllKeys(obj[key]).map(k => `${key}.${k}`));
    }
  }
  return keys;
}

console.log(getAllKeys(company));
// ['name', 'departments.dev.employees', 'departments.hr.employees']
```

---

## 🧠 **Pro Tip: Enumerate All Properties (Including Prototypes)**

```js
let allProps = new Set();
let obj = yourObject;

do {
  Object.getOwnPropertyNames(obj).forEach(p => allProps.add(p));
} while (obj = Object.getPrototypeOf(obj));

console.log([...allProps]);
```

---

## ⚡ **Quick Recap**

| Concept                       | Summary                                               |
| ----------------------------- | ----------------------------------------------------- |
| 🧱 **Static methods**         | Used with `Object.` prefix                            |
| 🔗 **Instance methods**       | Used on object instances                              |
| 🧊 **`freeze()` vs `seal()`** | `freeze()` = fully locked; `seal()` = no add/remove   |
| 🧩 **`is()`**                 | Safer than `===` for `NaN` and `-0`                   |
| 🧭 **`hasOwn()`**             | Modern alternative to `hasOwnProperty()`              |
| 🔍 **`Object.keys()`**        | Shallow by default — use recursion for nested objects |

---

Would you like me to generate a **visual diagram** (tree view) showing how `Object.keys()` expands across nested objects, or a **PDF version** of this full cheatsheet?
