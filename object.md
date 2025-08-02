
## 📘 **1. Static Methods of `Object`**

*(Used like `Object.methodName(obj)`)*

| Method                                        | Description                                           |
| --------------------------------------------- | ----------------------------------------------------- |
| `Object.assign(target, ...sources)`           | Copies properties from sources to target              |
| `Object.create(proto)`                        | Creates a new object with the given prototype         |
| `Object.defineProperty(obj, key, descriptor)` | Adds/updates a property with descriptor               |
| `Object.defineProperties(obj, props)`         | Adds multiple properties with descriptors             |
| `Object.entries(obj)`                         | Returns key-value pairs as array                      |
| `Object.fromEntries(arr)`                     | Converts array of key-value pairs to object           |
| `Object.freeze(obj)`                          | Makes object immutable (no changes allowed)           |
| `Object.seal(obj)`                            | Prevents adding/removing properties                   |
| `Object.getOwnPropertyDescriptor(obj, prop)`  | Gets a property descriptor                            |
| `Object.getOwnPropertyDescriptors(obj)`       | Gets all property descriptors                         |
| `Object.getOwnPropertyNames(obj)`             | Returns all property names (including non-enumerable) |
| `Object.getOwnPropertySymbols(obj)`           | Gets symbol properties                                |
| `Object.getPrototypeOf(obj)`                  | Gets prototype of an object                           |
| `Object.setPrototypeOf(obj, proto)`           | Sets prototype                                        |
| `Object.hasOwn(obj, prop)`                    | Checks if prop exists (like `hasOwnProperty`)         |
| `Object.is(value1, value2)`                   | Like `===` but handles `NaN` and `-0` correctly       |
| `Object.isExtensible(obj)`                    | Checks if object is extensible                        |
| `Object.preventExtensions(obj)`               | Disallows adding new properties                       |
| `Object.keys(obj)`                            | Returns enumerable keys                               |
| `Object.values(obj)`                          | Returns enumerable values                             |

---

## 📘 **2. Instance Methods of Object (from `Object.prototype`)**

| Method                           | Description                            |
| -------------------------------- | -------------------------------------- |
| `obj.hasOwnProperty(prop)`       | Checks if prop is own (not inherited)  |
| `obj.isPrototypeOf(obj2)`        | Checks if object is in prototype chain |
| `obj.propertyIsEnumerable(prop)` | Checks if a property is enumerable     |
| `obj.toString()`                 | Returns string representation          |
| `obj.valueOf()`                  | Returns the primitive value            |
| `obj.toLocaleString()`           | Locale-aware version of `toString()`   |

---

## 🧪 **Example Usage**

```js
const person = {
  name: "Alice",
  age: 30
};

console.log(Object.keys(person));         // ['name', 'age']
console.log(Object.values(person));       // ['Alice', 30]
console.log(Object.entries(person));      // [['name', 'Alice'], ['age', 30]]

Object.freeze(person);
person.name = "Bob";                      // No effect
console.log(person.name);                 // 'Alice'
```

---

## 🧠 Pro Tip: Enumerating All Methods in an Object

To get all property names and methods (including from the prototype):

```js
let allProps = new Set();
let obj = yourObject;
do {
  Object.getOwnPropertyNames(obj).forEach(p => allProps.add(p));
} while (obj = Object.getPrototypeOf(obj));

console.log([...allProps]);
```

---

Would you like a PDF cheatsheet or visual diagram of the object method hierarchy?
