Here’s your comprehensive list of **JavaScript string methods** with **descriptions and examples** for each one. This is a powerful reference if you're learning or reviewing string operations.

---

### ✅ **Core String Methods with Examples**

| **Method**                    | **Description**                           | **Example**                                                               |
| ----------------------------- | ----------------------------------------- | ------------------------------------------------------------------------- |
| `at(index)`                   | Returns the character at a specific index | `"hello".at(1)` → `'e'`                                                   |
| `charAt(index)`               | Returns character at given index          | `"hello".charAt(1)` → `'e'`                                               |
| `charCodeAt(index)`           | Unicode of char at index                  | `"A".charCodeAt(0)` → `65`                                                |
| `codePointAt(index)`          | Returns Unicode code point                | `"💻".codePointAt(0)` → `128187`                                          |
| `concat()`                    | Concatenates strings                      | `"Hello ".concat("World")` → `"Hello World"`                              |
| `constructor`                 | Returns constructor function              | `"abc".constructor === String` → `true`                                   |
| `endsWith(value)`             | Checks if ends with value                 | `"chatGPT".endsWith("PT")` → `true`                                       |
| `fromCharCode(code)`          | Unicode to char                           | `String.fromCharCode(65)` → `'A'`                                         |
| `includes(value)`             | Checks if string contains value           | `"JavaScript".includes("Script")` → `true`                                |
| `indexOf(value)`              | First occurrence index                    | `"banana".indexOf("a")` → `1`                                             |
| `isWellFormed()`              | Valid Unicode format (ES2024)             | `"abc".isWellFormed()` → `true`                                           |
| `lastIndexOf(value)`          | Last occurrence index                     | `"banana".lastIndexOf("a")` → `5`                                         |
| `length`                      | Returns string length                     | `"hello".length` → `5`                                                    |
| `localeCompare()`             | Compares strings based on locale          | `"a".localeCompare("b")` → `-1`                                           |
| `match(regex)`                | Returns match array                       | `"abc123".match(/\d+/)` → `['123']`                                       |
| `matchAll(regex)`             | Returns all matches as iterator           | `Array.from("abc123abc".matchAll(/abc/g))` → `[['abc'], ['abc']]`         |
| `padEnd(len, str)`            | Pads string at end                        | `"5".padEnd(3, "0")` → `"500"`                                            |
| `padStart(len, str)`          | Pads string at start                      | `"5".padStart(3, "0")` → `"005"`                                          |
| `prototype`                   | Extend String with custom methods         | `String.prototype.shout = function() { return this.toUpperCase() + "!" }` |
| `repeat(count)`               | Repeats string                            | `"ha".repeat(3)` → `"hahaha"`                                             |
| `replace(search, replace)`    | Replaces first match                      | `"foo bar".replace("bar", "baz")` → `"foo baz"`                           |
| `replaceAll(search, replace)` | Replaces all matches                      | `"aabb".replaceAll("a", "z")` → `"zzbb"`                                  |
| `search(regex)`               | Index of match                            | `"abc123".search(/\d/)` → `3`                                             |
| `slice(start, end)`           | Returns substring (excludes end)          | `"abcdef".slice(1, 4)` → `"bcd"`                                          |
| `split(separator)`            | Splits string into array                  | `"a,b,c".split(",")` → `["a", "b", "c"]`                                  |
| `startsWith(value)`           | Checks if starts with value               | `"JavaScript".startsWith("Java")` → `true`                                |
| `substr(start, length)`       | Returns substring (deprecated)            | `"abcdef".substr(1, 3)` → `"bcd"`                                         |
| `substring(start, end)`       | Like slice, but no negative indexes       | `"abcdef".substring(1, 4)` → `"bcd"`                                      |
| `toLocaleLowerCase()`         | Lowercase with locale                     | `"İ".toLocaleLowerCase("tr")` → `"i"`                                     |
| `toLocaleUpperCase()`         | Uppercase with locale                     | `"i".toLocaleUpperCase("tr")` → `"İ"`                                     |
| `toLowerCase()`               | Converts to lowercase                     | `"HELLO".toLowerCase()` → `"hello"`                                       |
| `toString()`                  | Returns string object as string           | `(123).toString()` → `"123"`                                              |
| `toUpperCase()`               | Converts to uppercase                     | `"hello".toUpperCase()` → `"HELLO"`                                       |
| `toWellFormed()`              | Fixes lone surrogates (ES2024)            | `"abc\uD800".toWellFormed()` → `"abc�"`                                   |
| `trim()`                      | Removes spaces from both ends             | `"  hi  ".trim()` → `"hi"`                                                |
| `trimEnd()`                   | Removes spaces from end                   | `"hi   ".trimEnd()` → `"hi"`                                              |
| `trimStart()`                 | Removes spaces from start                 | `"   hi".trimStart()` → `"hi"`                                            |
| `valueOf()`                   | Primitive value of string                 | `"hello".valueOf()` → `"hello"`                                           |

---


### ✅ **JavaScript Object Methods and Properties with Examples**

| **Method / Property**                         | **Description**                                                      | **Example**                                                     |
| --------------------------------------------- | -------------------------------------------------------------------- | --------------------------------------------------------------- |
| `Object.assign(target, source)`               | Copies properties from one or more source objects to a target object | `Object.assign({}, {a: 1, b: 2})` → `{a: 1, b: 2}`              |
| `constructor`                                 | Returns the constructor function of the object                       | `({}).constructor === Object` → `true`                          |
| `Object.create(proto)`                        | Creates a new object with specified prototype                        | `let obj = Object.create({x: 1})`                               |
| `Object.defineProperties(obj, props)`         | Defines multiple properties at once                                  | `Object.defineProperties(obj, {prop1: {...}, prop2: {...}})`    |
| `Object.defineProperty(obj, key, descriptor)` | Defines or modifies a single property                                | `Object.defineProperty(obj, 'x', {value: 10})`                  |
| `Object.entries(obj)`                         | Returns key/value pairs as array                                     | `Object.entries({a: 1, b: 2})` → `[["a", 1], ["b", 2]]`         |
| `Object.freeze(obj)`                          | Makes object immutable (no changes allowed)                          | `Object.freeze(obj)`                                            |
| `Object.fromEntries(iterable)`                | Converts key/value pairs into an object                              | `Object.fromEntries([['a', 1]])` → `{a: 1}`                     |
| `Object.getOwnPropertyDescriptor(obj, prop)`  | Gets descriptor info for a property                                  | `Object.getOwnPropertyDescriptor({a: 1}, 'a')`                  |
| `Object.getOwnPropertyDescriptors(obj)`       | Gets all descriptors of object properties                            | `Object.getOwnPropertyDescriptors({a: 1})`                      |
| `Object.getOwnPropertyNames(obj)`             | Gets all own (non-symbol) property names                             | `Object.getOwnPropertyNames({a: 1})` → `["a"]`                  |
| `Object.groupBy(array, callback)`             | Groups array of objects by callback return (ES2024)                  | `Object.groupBy([{type: 'fruit'}, {type: 'veg'}], i => i.type)` |
| `Object.isExtensible(obj)`                    | Checks if new properties can be added                                | `Object.isExtensible({})` → `true`                              |
| `Object.isFrozen(obj)`                        | Checks if object is frozen                                           | `Object.isFrozen(Object.freeze({}))` → `true`                   |
| `Object.isSealed(obj)`                        | Checks if object is sealed (no add/delete)                           | `Object.isSealed(Object.seal({}))` → `true`                     |
| `Object.keys(obj)`                            | Gets an array of keys                                                | `Object.keys({a: 1, b: 2})` → `["a", "b"]`                      |
| `Object.preventExtensions(obj)`               | Prevents adding new properties                                       | `Object.preventExtensions(obj)`                                 |
| `prototype`                                   | Used to extend object blueprints                                     | `MyConstructor.prototype.greet = () => "hi"`                    |
| `Object.seal(obj)`                            | Prevents adding/removing properties (but values can be changed)      | `Object.seal(obj)`                                              |
| `toString()`                                  | Converts object to string                                            | `({a:1}).toString()` → `"[object Object]"`                      |
| `valueOf()`                                   | Returns primitive value                                              | `(42).valueOf()` → `42`                                         |
| `Object.values(obj)`                          | Returns array of property values                                     | `Object.values({a: 1, b: 2})` → `[1, 2]`                        |

---



