
# TypeScript Getting Started

TypeScript is transpiled into JavaScript using a compiler.

## Installing the Compiler

TypeScript has an official compiler which can be installed through **npm**.

Within your npm project, run the following command to install the compiler:

```bash
npm install typescript --save-dev
````

You should see an output similar to:

```
added 1 package, and audited 2 packages in 2s
found 0 vulnerabilities
```

The compiler is installed in the `node_modules` directory and can be run with:

```bash
npx tsc
```

Which should give you an output similar to:

```
Version 4.5.5
tsc: The TypeScript Compiler - Version 4.5.5
```

Followed by a list of all the common commands.

---

## Configuring the Compiler

By default, the TypeScript compiler will print a help message when run in an empty project.

The compiler can be configured using a `tsconfig.json` file.

You can have TypeScript create `tsconfig.json` with the recommended settings using:

```bash
npx tsc --init
```

Which should give you an output similar to:

```
Created a new tsconfig.json with:
  target: es2016
  module: commonjs
  strict: true
  esModuleInterop: true
  skipLibCheck: true
  forceConsistentCasingInFileNames: true
```

You can learn more at [https://aka.ms/tsconfig.json](https://aka.ms/tsconfig.json)

Here is an example of additional options you could add to the `tsconfig.json` file:

```json
{
  "include": ["src"],
  "compilerOptions": {
    "outDir": "./build"
  }
}
```

This will configure the TypeScript compiler to transpile TypeScript files located in the `src/` directory of your project into JavaScript files in the `build/` directory.


# TypeScript Simple Types

TypeScript supports some simple types (primitives) you may already know.

## Main Primitives

There are **three main primitives** in JavaScript and TypeScript:

- `boolean` – true or false values  
- `number` – whole numbers and floating-point values  
- `string` – text values like `"TypeScript Rocks"`

### Additional Primitives

There are also two **less common primitives** introduced in later versions:

- `bigint` – similar to `number`, but supports arbitrarily large values
- `symbol` – used to create a globally unique identifier

---

## Type Assignment

When creating a variable, there are two main ways TypeScript assigns a type:

- **Explicit**
- **Implicit**

In both examples below, `firstName` is of type `string`.

### Explicit Type

**Explicit** – writing out the type:

```ts
let firstName: string = "Dylan";
````

Explicit type assignments are easier to read and more intentional.

---

### Implicit Type

**Implicit** – TypeScript will *infer* the type based on the assigned value:

```ts
let firstName = "Dylan";
```

> 📝 **Note:** Having TypeScript "guess" the type of a value is called **inference**.

Implicit type assignments are shorter, faster to type, and often used during development or testing.

---

## Error in Type Assignment

TypeScript will throw an error if the data type does not match.

### Example (Explicit):

```ts
let firstName: string = "Dylan"; // type string
firstName = 33; // ❌ Error: Type 'number' is not assignable to type 'string'
```

### Example (Implicit):

```ts
let firstName = "Dylan"; // inferred as string
firstName = 33; // ❌ Error: Type 'number' is not assignable to type 'string'
```

JavaScript, in contrast, will not throw an error for mismatched types.

---

## Unable to Infer

TypeScript may not always properly infer the type of a variable. In such cases, it will assign the type as `any`, disabling type checking.

### Example:

```ts
// Implicit any, as JSON.parse can return any type
const json = JSON.parse("55"); 
console.log(typeof json); // "number"
```

> 🔧 This behavior can be avoided by enabling `noImplicitAny` in the `tsconfig.json` file.

---

## Capitalized Types: Avoid Confusion

You may sometimes see primitive types capitalized:

* `boolean !== Boolean`
* `string !== String`

For this tutorial, **always use lowercase primitive types** (`string`, `number`, `boolean`).
The uppercase versions are rarely used and reserved for very specific use cases.


# TypeScript Special Types

TypeScript has **special types** that may not refer to any specific type of data.


## `any` Type

The `any` type **disables type checking** and effectively allows *all* types to be used.

### ❌ Without `any`

```ts
let u = true;
u = "string"; // ❌ Error: Type 'string' is not assignable to type 'boolean'.
Math.round(u); // ❌ Error: Argument of type 'boolean' is not assignable to parameter of type 'number'.
````

### ✅ With `any`

```ts
let v: any = true;
v = "string"; // ✅ No error
Math.round(v); // ✅ No error
```

The `any` type is **useful for bypassing errors**, but:

* You lose **type safety**
* Tools like **autocomplete** and **type inference** will not work

> ⚠️ `any` should be avoided at *"any"* cost!

---

## `unknown` Type

The `unknown` type is a **safer alternative** to `any`.

It allows assignment of any type, but **prevents unsafe operations** without proper type checking or casting.

### Example:

```ts
let w: unknown = 1;
w = "string"; // ✅ OK
w = {
  runANonExistentMethod: () => {
    console.log("I think therefore I am");
  }
} as { runANonExistentMethod: () => void };

// w.runANonExistentMethod(); // ❌ Error: Object is of type 'unknown'
```

### Type Guard + Casting

```ts
if (typeof w === 'object' && w !== null) {
  (w as { runANonExistentMethod: Function }).runANonExistentMethod();
}
```

✅ This is **type-safe** because we first check `typeof w === 'object'`.

> ℹ️ **Casting** is done with the `as` keyword to treat a value as a specific type.

---

## `never` Type

The `never` type represents a value that **should never occur**. TypeScript throws an error if a value is assigned to it.

```ts
let x: never = true; // ❌ Error: Type 'boolean' is not assignable to type 'never'.
```

* `never` is **rarely used directly**
* Commonly used in **exhaustiveness checks** and **advanced generics**

---

## `undefined` and `null` Types

These types refer to the JavaScript primitives:

```ts
let y: undefined = undefined;
let z: null = null;
```

They are not often used unless you enable `strictNullChecks` in `tsconfig.json`.

> 🔧 Enabling `strictNullChecks` makes `null` and `undefined` values more type-safe.


# TypeScript Arrays

TypeScript has a specific syntax for typing arrays.


## Array Type Annotation

You can explicitly declare the type of elements in an array using `type[]` syntax.

### Example:

```ts
const names: string[] = [];
names.push("Dylan"); // ✅ No error
// names.push(3); // ❌ Error: Argument of type 'number' is not assignable to parameter of type 'string'.
````

---

## `readonly` Arrays

The `readonly` keyword can prevent arrays from being modified.

### Example:

```ts
const names: readonly string[] = ["Dylan"];
names.push("Jack"); // ❌ Error: Property 'push' does not exist on type 'readonly string[]'
```

> 💡 Try removing the `readonly` modifier and the code will work again.

---

## Type Inference

TypeScript can infer the type of an array if it’s initialized with values.

### Example:

```ts
const numbers = [1, 2, 3]; // inferred as number[]
numbers.push(4); // ✅ No error

// numbers.push("2"); // ❌ Error: Argument of type 'string' is not assignable to parameter of type 'number'.

let head: number = numbers[0]; // ✅ No error
```

> ✅ Inferred arrays help you write less code while still getting type safety.

# TypeScript Tuples

A **tuple** is a typed array with a pre-defined length and types for each index.

Tuples allow each element in the array to have a known, specific type.

---

## Defining a Tuple

To define a tuple, specify the type for each element in order:

```ts
// define our tuple
let ourTuple: [number, boolean, string];

// initialize correctly
ourTuple = [5, false, 'Coding God was here'];
````

### ❌ Incorrect Initialization

Even if the types are present, **order matters**:

```ts
let ourTuple: [number, boolean, string];

// incorrect order — throws an error
ourTuple = [false, 'Coding God was mistaken', 5];
// ❌ Error: Type 'boolean' is not assignable to type 'number', etc.
```

---

## Readonly Tuples

A good practice is to make your tuple `readonly`.

Tuples only have strong type definitions for their initial positions — not beyond that.

### Mutable Tuple

```ts
let ourTuple: [number, boolean, string];
ourTuple = [5, false, 'Coding God was here'];

// This will NOT cause a compile-time error:
ourTuple.push('Something new and wrong'); 
console.log(ourTuple);
// Output: [5, false, 'Coding God was here', 'Something new and wrong']
```

### Readonly Tuple

```ts
const ourReadonlyTuple: readonly [number, boolean, string] = [5, true, 'The Real Coding God'];

// ❌ Error: Property 'push' does not exist on type 'readonly [...]'
ourReadonlyTuple.push('Coding God took a day off');
```

> 📌 Use `readonly` for stricter type safety in tuples.

---

## Tuples in Real Life: React `useState`

If you’ve used **React**, you’ve probably used tuples:

```ts
const [firstName, setFirstName] = useState('Dylan');
```

This is a tuple:

* First element: a `string`
* Second element: a setter function

---

## Named Tuples

Named tuples provide **semantic meaning** to each position in the tuple.

### Example:

```ts
const graph: [x: number, y: number] = [55.2, 41.3];
```

> 🧠 Named tuples make your code more readable and self-documenting.

---

## Destructuring Tuples

Since tuples are arrays, they can be **destructured**.

### Example:

```ts
const graph: [number, number] = [55.2, 41.3];
const [x, y] = graph;

console.log(x); // 55.2
console.log(y); // 41.3
```

> 🧩 Destructuring is useful when working with tuples returned from functions.


# TypeScript Object Types

TypeScript has a specific syntax for typing objects.

> 📚 You can read more about objects in the [JavaScript Objects](#) chapter.

---

## Defining Object Types

You can directly define object types using inline type annotations.

### Example:

```ts
const car: { type: string, model: string, year: number } = {
  type: "Toyota",
  model: "Corolla",
  year: 2009
};
````

> 💡 These types can also be written separately and reused using **interfaces**.

---

## Type Inference

TypeScript can infer the types of object properties from their initial values.

### Example:

```ts
const car = {
  type: "Toyota",
};

car.type = "Ford"; // ✅ No error
car.type = 2;      // ❌ Error: Type 'number' is not assignable to type 'string'.
```

> 🧠 Inferred types reduce boilerplate, but still provide type safety.

---

## Optional Properties

Optional properties are not required in the object definition.

### ❌ Without Optional Property:

```ts
const car: { type: string, mileage: number } = {
  type: "Toyota",
};
// ❌ Error: Property 'mileage' is missing
car.mileage = 2000;
```

### ✅ With Optional Property:

```ts
const car: { type: string, mileage?: number } = {
  type: "Toyota"
};

car.mileage = 2000; // ✅ No error
```

> 📝 Use `?` to mark a property as optional.

---

## Index Signatures

Index signatures allow you to define objects with **dynamic property keys**.

### Example:

```ts
const nameAgeMap: { [index: string]: number } = {};
nameAgeMap.Jack = 25; // ✅ No error
nameAgeMap.Mark = "Fifty"; // ❌ Error: Type 'string' is not assignable to type 'number'
```

> 🛠️ Index signatures are helpful when the object has unknown or dynamic keys.

You can also write the same using a utility type:

```ts
const nameAgeMap: Record<string, number> = {};
```

> 🔍 Learn more about this in the [TypeScript Utility Types](#) chapter.


# TypeScript Enums

An `enum` is a special "class" that represents a group of **constants** (unchangeable variables).

TypeScript enums come in two main flavors:

- **Numeric enums**
- **String enums**

---

## Numeric Enums – Default Behavior

By default, enums initialize the **first value to `0`**, and increment by `1` for each subsequent value.

### Example:

```ts
enum CardinalDirections {
  North,
  East,
  South,
  West
}

let currentDirection = CardinalDirections.North;

// logs: 0
console.log(currentDirection);

// ❌ Error: 'North' is not assignable to type 'CardinalDirections'
currentDirection = 'North';
````

---

## Numeric Enums – Initialized First Value

You can manually initialize the first value. The rest will increment from there.

### Example:

```ts
enum CardinalDirections {
  North = 1,
  East,
  South,
  West
}

// logs: 1
console.log(CardinalDirections.North);

// logs: 4
console.log(CardinalDirections.West);
```

---

## Numeric Enums – Fully Initialized

You can assign custom numeric values to each enum member.

### Example:

```ts
enum StatusCodes {
  NotFound = 404,
  Success = 200,
  Accepted = 202,
  BadRequest = 400
}

// logs: 404
console.log(StatusCodes.NotFound);

// logs: 200
console.log(StatusCodes.Success);
```

> 🧠 This gives full control over the values associated with each member.

---

## String Enums

String enums are more **readable** and more commonly used than numeric enums.

### Example:

```ts
enum CardinalDirections {
  North = "North",
  East = "East",
  South = "South",
  West = "West"
}

// logs: "North"
console.log(CardinalDirections.North);

// logs: "West"
console.log(CardinalDirections.West);
```

> ✅ String enums make logs and debugging easier by preserving human-readable values.

---

## Mixing Enum Types

> ⚠️ Technically, enums can mix **string** and **numeric** values.
> However, this is **not recommended** as it can lead to confusing behavior.

---

## Summary

| Enum Type         | Behavior                          |
| ----------------- | --------------------------------- |
| Numeric (default) | Starts at 0, increments by 1      |
| Numeric (custom)  | You define exact numeric values   |
| String            | You assign readable string values |

Enums provide a clear and organized way to handle **constant values** in TypeScript.



# TypeScript Type Aliases and Interfaces

TypeScript allows you to define types **separately** from the variables or objects that use them.

This is done using:

- **Type Aliases**
- **Interfaces**

These are especially useful when you need to **reuse type definitions** across multiple places.

---

## ✅ Type Aliases

Type aliases let you **create custom names** (aliases) for types — both **primitive** and **complex**.

### Example:

```ts
type CarYear = number;
type CarType = string;
type CarModel = string;

type Car = {
  year: CarYear;
  type: CarType;
  model: CarModel;
};

const carYear: CarYear = 2001;
const carType: CarType = "Toyota";
const carModel: CarModel = "Corolla";

const car: Car = {
  year: carYear,
  type: carType,
  model: carModel
};
````

> 📌 Use `type` when you need to create a reusable name for a type (especially for unions, intersections, or primitives).

---

## 🧩 Interfaces

Interfaces are similar to type aliases, but are used **only for object shapes**.

### Example:

```ts
interface Rectangle {
  height: number;
  width: number;
}

const rectangle: Rectangle = {
  height: 20,
  width: 10
};
```

> 🔸 Interfaces describe the **structure** of an object.

---

## 🔄 Extending Interfaces

Interfaces can be **extended**, which means creating a new interface that inherits from another one.

This is useful for **adding new properties** to an existing interface.

### Example:

```ts
interface Rectangle {
  height: number;
  width: number;
}

interface ColoredRectangle extends Rectangle {
  color: string;
}

const coloredRectangle: ColoredRectangle = {
  height: 20,
  width: 10,
  color: "red"
};
```

> 🧠 Think of `extends` like inheritance in object-oriented programming.

---

## 🆚 Type Alias vs Interface

| Feature             | `type`                   | `interface`       |
| ------------------- | ------------------------ | ----------------- |
| Use with primitives | ✅ Yes                    | ❌ No              |
| Use with objects    | ✅ Yes                    | ✅ Yes             |
| Extendable          | ✅ (via intersection `&`) | ✅ (via `extends`) |
| Declaration merging | ❌ No                     | ✅ Yes             |

> Use `interface` for object shapes, especially when extending is required.
> Use `type` for primitives, unions, and more complex combinations.

---

## Summary

* **Type Alias** is flexible and can be used for any kind of type.
* **Interface** is designed for object type definitions and better supports extension.

Both are powerful tools to write **clean, reusable, and maintainable** TypeScript code.


# TypeScript Union Types

In TypeScript, **union types** are used when a value can be **one of multiple types**.

This is useful when you expect a value to be either a `string`, `number`, or another type — but **not** a fixed single type.

---

## 🔗 Syntax: Union (`|`)

Use the **pipe operator `|`** to define a union type.

### Example:

```ts
function printStatusCode(code: string | number) {
  console.log(`My status code is ${code}.`);
}

printStatusCode(404);
printStatusCode('404');
````

In this example, the function `printStatusCode` accepts **either** a `string` or a `number`.

---

## ⚠️ Union Type Errors

When using union types, TypeScript doesn't allow calling type-specific methods without narrowing the type first.

### Problematic Example:

```ts
function printStatusCode(code: string | number) {
  console.log(`My status code is ${code.toUpperCase()}`); 
  // ❌ Error: Property 'toUpperCase' does not exist on type 'string | number'
}
```

### Why this happens:

The method `.toUpperCase()` exists on **strings**, but not on **numbers**.

Since the parameter could be **either**, TypeScript gives an error unless you **narrow** the type.

---

## ✅ Type Narrowing Fix

You can fix this using **type guards** (like `typeof`) to narrow the type before using type-specific methods.

### Corrected Example:

```ts
function printStatusCode(code: string | number) {
  if (typeof code === 'string') {
    console.log(`My status code is ${code.toUpperCase()}`);
  } else {
    console.log(`My status code is ${code}`);
  }
}
```

---

## Summary

* Use `|` to define union types like `string | number`.
* Be careful with type-specific methods — always **narrow the type** before using them.
* Union types are helpful when dealing with **flexible inputs** or **API responses** that may return different types.


# TypeScript Functions

TypeScript provides a specific syntax for typing **function parameters** and **return values**, improving code clarity and reducing errors.

---

## 🏁 Return Type

You can explicitly define the type of value that a function returns.

### Example:

```ts
function getTime(): number {
  return new Date().getTime();
}
````

If not specified, TypeScript will attempt to **infer** the return type based on the return value.

---

## 🚫 Void Return Type

Use `void` to indicate that a function **does not return any value**.

### Example:

```ts
function printHello(): void {
  console.log('Hello!');
}
```

---

## 🧮 Parameter Types

Function parameters are typed just like variables.

### Example:

```ts
function multiply(a: number, b: number) {
  return a * b;
}
```

If no type is declared, TypeScript defaults to `any`, unless more type information is available.

---

## ❓ Optional Parameters

Use `?` to mark a parameter as **optional**.

### Example:

```ts
function add(a: number, b: number, c?: number) {
  return a + b + (c || 0);
}
```

Here, `c` is optional and defaults to `0` if not provided.

---

## 🧩 Default Parameters

Default values can be assigned to parameters. TypeScript can also **infer** their types.

### Example:

```ts
function pow(value: number, exponent: number = 10) {
  return value ** exponent;
}
```

---

## 🧱 Named Parameters

You can type named parameters (destructured objects) like this:

### Example:

```ts
function divide({ dividend, divisor }: { dividend: number, divisor: number }) {
  return dividend / divisor;
}
```

---

## 📦 Rest Parameters

Rest parameters are always arrays. Type them accordingly.

### Example:

```ts
function add(a: number, b: number, ...rest: number[]) {
  return a + b + rest.reduce((p, c) => p + c, 0);
}
```

---

## 🏷️ Type Aliases for Functions

Function types can be defined separately using **type aliases**, often written like arrow functions.

### Example:

```ts
type Negate = (value: number) => number;

const negateFunction: Negate = (value) => value * -1;
```

This approach improves reusability and readability for complex function signatures.

---

## ✅ Summary

* Use `: type` after parameters and return values to define types.
* Use `void` for functions that don’t return anything.
* Use `?` for optional parameters.
* Use default values to simplify function calls.
* Type rest and named parameters properly.
* Use type aliases to define reusable function types.

# TypeScript Casting

In TypeScript, **casting** allows you to **override the type** of a variable. This can be helpful when dealing with types from external libraries or uncertain data.

---

## 🔁 What is Casting?

**Casting** tells the TypeScript compiler to treat a value as a specific type. It does **not change** the actual runtime type, only how TypeScript interprets it.

---

## 🔤 Casting with `as`

The most common way to cast is with the `as` keyword.

### Example:

```ts
let x: unknown = 'hello';
console.log((x as string).length); // logs 5
````

Note: This does **not** change the runtime type of `x`. If `x` was actually a number:

```ts
let x: unknown = 4;
console.log((x as string).length); // prints undefined
```

---

## ❌ Invalid Casts

TypeScript will try to protect you from incorrect casts:

```ts
console.log((4 as string).length);
// ❌ Error: Conversion of type 'number' to type 'string' may be a mistake...
```

You must convert to `unknown` first if you really want to override the check.

---

## 🔁 Casting with `<>`

You can also use angle brackets (`<>`) for casting — this is an alternative syntax to `as`.

### Example:

```ts
let x: unknown = 'hello';
console.log((<string>x).length); // logs 5
```

> ⚠️ **Note**: This syntax is **not allowed** in `.tsx` files (e.g., in React), because of JSX parsing conflicts.

---

## 💪 Force Casting

When TypeScript blocks a cast, you can force it by casting to `unknown` first, then to the desired type.

### Example:

```ts
let x = 'hello';
console.log(((x as unknown) as number).length); 
// ❗ Warning: Will log undefined, because x is still a string at runtime
```

Force casting should only be used when you’re absolutely sure about the runtime type — otherwise, it can lead to runtime errors.

---

## ✅ Summary

| Method     | Syntax                       | Use Case                                   |
| ---------- | ---------------------------- | ------------------------------------------ |
| `as`       | `value as Type`              | Common, readable, works in all .ts files   |
| `<>`       | `<Type>value`                | Alternative to `as`, **not for TSX files** |
| Force Cast | `(value as unknown) as Type` | When TypeScript blocks a cast              |

---

## 🧠 Reminder

Casting in TypeScript **only affects type-checking**, not the actual data at runtime. Always double-check your assumptions to avoid runtime bugs.


# TypeScript Classes

TypeScript extends JavaScript classes with **types** and **visibility modifiers**, enabling safer and more structured object-oriented code.

---

## 🔧 Members: Types

Class properties and methods can be typed just like variables:

```ts
class Person {
  name: string;
}

const person = new Person();
person.name = "Jane";
````

---

## 🔐 Members: Visibility

There are **three visibility modifiers** in TypeScript:

* `public` (default) – accessible from anywhere
* `private` – accessible **only within the class**
* `protected` – accessible within the class and subclasses

### Example:

```ts
class Person {
  private name: string;

  public constructor(name: string) {
    this.name = name;
  }

  public getName(): string {
    return this.name;
  }
}

const person = new Person("Jane");
console.log(person.getName()); // ✅ OK
// console.log(person.name); // ❌ Error: Property 'name' is private
```

---

## 🧱 Parameter Properties

TypeScript allows defining properties directly in the constructor using visibility modifiers:

```ts
class Person {
  public constructor(private name: string) {}

  public getName(): string {
    return this.name;
  }
}
```

---

## 🚫 Readonly Properties

Use `readonly` to make class properties immutable after initialization:

```ts
class Person {
  private readonly name: string;

  public constructor(name: string) {
    this.name = name;
  }

  public getName(): string {
    return this.name;
  }
}
```

---

## 📐 Implements: Using Interfaces with Classes

You can enforce a class structure using an `interface` and the `implements` keyword:

```ts
interface Shape {
  getArea: () => number;
}

class Rectangle implements Shape {
  public constructor(protected readonly width: number, protected readonly height: number) {}

  public getArea(): number {
    return this.width * this.height;
  }
}
```

> ✅ A class can implement multiple interfaces:
> `class Rectangle implements Shape, Colored { ... }`

---

## 🔗 Inheritance: `extends`

Use `extends` to create a subclass from another class.

```ts
class Square extends Rectangle {
  public constructor(width: number) {
    super(width, width);
  }
}
```

### Inheriting Methods

```ts
const square = new Square(5);
console.log(square.getArea()); // Inherits getArea from Rectangle
```

---

## 🔁 Method Override

Override parent class methods using the **same method name**. Use `override` keyword (recommended) for clarity and safety:

```ts
class Rectangle {
  public toString(): string {
    return `Rectangle`;
  }
}

class Square extends Rectangle {
  public override toString(): string {
    return `Square`;
  }
}
```

> 🔒 Tip: Enable `noImplicitOverride` in your config to force use of `override`.

---

## 🧱 Abstract Classes

Use `abstract` to define base classes that **cannot be instantiated directly**. They can define methods that **must** be implemented by subclasses.

```ts
abstract class Polygon {
  public abstract getArea(): number;

  public toString(): string {
    return `Polygon[area=${this.getArea()}]`;
  }
}

class Rectangle extends Polygon {
  public constructor(protected readonly width: number, protected readonly height: number) {
    super();
  }

  public getArea(): number {
    return this.width * this.height;
  }
}
```

> 🔒 Abstract classes **must be extended** before use.

---

## 🧪 Exercise

Specify that `Person.name` is private, but `getName()` is public:

```ts
class Person {
  private name: string;

  public constructor(name: string) {
    this.name = name;
  }

  public getName(): string {
    return this.name;
  }
}
```

---

## ✅ Summary Table

| Feature                  | Syntax                              | Description                                 |
| ------------------------ | ----------------------------------- | ------------------------------------------- |
| Public Member            | `public name: string`               | Accessible anywhere                         |
| Private Member           | `private name: string`              | Accessible only inside the class            |
| Protected Member         | `protected name: string`            | Accessible inside class & subclasses        |
| Parameter Property       | `constructor(private name: string)` | Declares and assigns a property in one step |
| Readonly Property        | `readonly id: number`               | Can’t be changed after construction         |
| Interface Implementation | `implements Shape`                  | Enforces class structure                    |
| Inheritance              | `extends Rectangle`                 | Inherits class members                      |
| Method Override          | `override toString()`               | Replaces parent method                      |
| Abstract Class           | `abstract class Shape`              | Must be extended to be used                 |


# TypeScript: Basic Generics

Generics allow you to create **reusable and flexible components** (functions, classes, and types) without specifying exact types up front.

---

## 💡 Why Use Generics?

Generics create *type variables* that can adapt to the actual data types used in code, making your code:

- **Reusable**
- **Type-safe**
- **Easier to maintain**

---

## 🔁 Generics in Functions

Use generics in functions to make them work with different types while preserving type safety.

### Example

```ts
function createPair<S, T>(v1: S, v2: T): [S, T] {
  return [v1, v2];
}

console.log(createPair<string, number>('hello', 42)); // ['hello', 42]
````

> 🧠 Tip: TypeScript can infer generic types from parameters, so `<string, number>` is often optional.

---

## 🧱 Generics in Classes

You can create generic classes to operate on different data types:

```ts
class NamedValue<T> {
  private _value: T | undefined;

  constructor(private name: string) {}

  public setValue(value: T) {
    this._value = value;
  }

  public getValue(): T | undefined {
    return this._value;
  }

  public toString(): string {
    return `${this.name}: ${this._value}`;
  }
}

const value = new NamedValue<number>('myNumber');
value.setValue(10);
console.log(value.toString()); // myNumber: 10
```

> 📌 TypeScript infers `T` if passed in the constructor or methods.

---

## 🏷️ Generics in Type Aliases

Generics can be used in **type aliases** for reusable data shapes:

```ts
type Wrapped<T> = { value: T };

const wrappedValue: Wrapped<number> = { value: 10 };
```

This also works with **interfaces**:

```ts
interface Wrapped<T> {
  value: T;
}
```

---

## ⚙️ Default Generic Types

You can assign a **default type** for a generic parameter:

```ts
class NamedValue<T = string> {
  private _value: T | undefined;

  constructor(private name: string) {}

  public setValue(value: T) {
    this._value = value;
  }

  public getValue(): T | undefined {
    return this._value;
  }

  public toString(): string {
    return `${this.name}: ${this._value}`;
  }
}

const value = new NamedValue('myName');
value.setValue('myValue');
console.log(value.toString()); // myName: myValue
```

> 💡 If no type is passed, `T` defaults to `string`.

---

## 🚧 Generic Constraints with `extends`

Use `extends` to **restrict** what types are allowed as generic parameters:

```ts
function createLoggedPair<
  S extends string | number,
  T extends string | number
>(v1: S, v2: T): [S, T] {
  console.log(`creating pair: v1='${v1}', v2='${v2}'`);
  return [v1, v2];
}
```

This ensures only `string` or `number` types can be used.

---

## 🔄 Combining Constraints and Defaults

You can combine `extends` with default values:

```ts
class Store<T extends number | string = string> {
  private _data: T[] = [];

  public addItem(item: T) {
    this._data.push(item);
  }

  public getItems(): T[] {
    return this._data;
  }
}
```

---

## 🧪 Summary Table

| Feature           | Example                                | Description                    |
| ----------------- | -------------------------------------- | ------------------------------ |
| Function Generics | `function<T>(value: T): T { ... }`     | Works for multiple data types  |
| Class Generics    | `class Box<T> { ... }`                 | Create reusable class logic    |
| Type Alias        | `type Wrapped<T> = { value: T }`       | Reusable object shape          |
| Default Types     | `class Box<T = string> { ... }`        | Provide fallback type          |
| Constraints       | `function<T extends string>(x: T) {}`  | Restrict type parameter input  |
| Combine Both      | `class Box<T extends number = number>` | Restrict & default type in one |

---

## ✅ Exercise

Define a generic class `Storage<T>` that stores an array of items of type `T`, with `addItem` and `getItems` methods.

```ts
class Storage<T> {
  private data: T[] = [];

  public addItem(item: T): void {
    this.data.push(item);
  }

  public getItems(): T[] {
    return this.data;
  }
}

const numberStore = new Storage<number>();
numberStore.addItem(5);
numberStore.addItem(10);
console.log(numberStore.getItems()); // [5, 10]
```

# TypeScript: Utility Types

TypeScript provides built-in utility types that make it easier to **transform** or **constrain** existing types.

---

## 🧩 `Partial<T>`

Makes all properties in a type **optional**.

### Example

```ts
interface Point {
  x: number;
  y: number;
}

let pointPart: Partial<Point> = {}; 
pointPart.x = 10; // y is not required
````

---

## ✅ `Required<T>`

Makes all properties **required**, even if they were optional originally.

### Example

```ts
interface Car {
  make: string;
  model: string;
  mileage?: number;
}

let myCar: Required<Car> = {
  make: 'Ford',
  model: 'Focus',
  mileage: 12000 // must be provided now
};
```

---

## 🗺️ `Record<K, T>`

Creates an object type with keys of type `K` and values of type `T`.

### Example

```ts
const nameAgeMap: Record<string, number> = {
  'Alice': 21,
  'Bob': 25
};
```

Equivalent to:

```ts
{ [key: string]: number }
```

---

## 🚫 `Omit<T, K>`

Removes specified keys `K` from type `T`.

### Example

```ts
interface Person {
  name: string;
  age: number;
  location?: string;
}

const bob: Omit<Person, 'age' | 'location'> = {
  name: 'Bob'
};
```

---

## 🎯 `Pick<T, K>`

Keeps only the specified keys `K` from type `T`.

### Example

```ts
interface Person {
  name: string;
  age: number;
  location?: string;
}

const bob: Pick<Person, 'name'> = {
  name: 'Bob'
};
```

---

## 🔻 `Exclude<T, U>`

Removes from `T` those types that are assignable to `U`.

### Example

```ts
type Primitive = string | number | boolean;

const value: Exclude<Primitive, string> = true; 
// only number or boolean allowed now
```

---

## 🔁 `ReturnType<T>`

Extracts the return type of a function type.

### Example

```ts
type PointGenerator = () => { x: number; y: number; };

const point: ReturnType<PointGenerator> = {
  x: 10,
  y: 20
};
```

---

## 📥 `Parameters<T>`

Extracts the parameter types of a function type as a tuple.

### Example

```ts
type PointPrinter = (p: { x: number; y: number; }) => void;

const point: Parameters<PointPrinter>[0] = {
  x: 10,
  y: 20
};
```

---

## 🔒 `Readonly<T>`

Makes all properties in type `T` **readonly** (immutable).

> ⚠️ Prevents changes at compile-time, but not at runtime in plain JavaScript.

### Example

```ts
interface Person {
  name: string;
  age: number;
}

const person: Readonly<Person> = {
  name: "Dylan",
  age: 35,
};

person.name = 'Israel'; // ❌ Error: Cannot assign to 'name'
```

---

## 🧪 Summary Table

| Utility Type    | Purpose                          | Example Use Case                       |
| --------------- | -------------------------------- | -------------------------------------- |
| `Partial<T>`    | Make all fields optional         | Form drafts                            |
| `Required<T>`   | Make all fields required         | Validation enforcement                 |
| `Record<K, T>`  | Map keys to specific value types | Dictionary or map                      |
| `Omit<T, K>`    | Remove fields from a type        | API response shaping                   |
| `Pick<T, K>`    | Keep only specific fields        | Minimal UI representation              |
| `Exclude<T, U>` | Remove union types               | Narrowing allowed values               |
| `ReturnType<T>` | Get return type of function      | Deriving types from existing functions |
| `Parameters<T>` | Get parameters of a function     | Function mocking or overloading        |
| `Readonly<T>`   | Make properties immutable        | Prevent accidental changes             |

---

## 🧠 Practice

Define a type that uses `Omit` to create a `PublicUser` type that removes `password` from the `User` type:

```ts
interface User {
  username: string;
  email: string;
  password: string;
}

type PublicUser = Omit<User, 'password'>;

const user: PublicUser = {
  username: 'john_doe',
  email: 'john@example.com'
};
```

# TypeScript: `keyof` Operator

The `keyof` keyword in TypeScript is used to extract the **key names** from an object type and form a **union of string literal types**.

---

## 🧱 Basic Usage: `keyof` with Explicit Keys

When used with a type that has explicitly defined keys, `keyof` produces a **union of those keys**.

### Example

```ts
interface Person {
  name: string;
  age: number;
}

// keyof Person becomes "name" | "age"

function printPersonProperty(person: Person, property: keyof Person) {
  console.log(`Printing person property ${property}: "${person[property]}"`);
}

let person = {
  name: "Max",
  age: 27
};

printPersonProperty(person, "name"); // ✅ OK
printPersonProperty(person, "age");  // ✅ OK
// printPersonProperty(person, "email"); ❌ Error: "email" is not in keyof Person
````

---

## 🔁 `keyof` with Index Signatures

When used on a type with an index signature, `keyof` evaluates to the **index type**, usually `string` or `number`.

### Example

```ts
type StringMap = {
  [key: string]: unknown;
};

// keyof StringMap is string

function createStringPair(property: keyof StringMap, value: string): StringMap {
  return { [property]: value };
}

const pair = createStringPair("username", "admin");
// Valid: "username" is a string, which matches keyof StringMap
```

---

## 🔎 `keyof` + `typeof`

You can use `keyof` along with `typeof` to dynamically infer keys from a value:

### Example

```ts
const COLORS = {
  red: "#FF0000",
  green: "#00FF00",
  blue: "#0000FF"
};

type ColorName = keyof typeof COLORS; // "red" | "green" | "blue"

function getColorCode(color: ColorName): string {
  return COLORS[color];
}

getColorCode("red"); // ✅ OK
// getColorCode("purple"); ❌ Error: "purple" is not a key in COLORS
```

---

## 🧠 Use Cases

* Validating keys dynamically.
* Creating generic utilities like object property accessors.
* Working with mapped types or type-safe lookups.

---

## 🧪 Practice

Create a function that returns a value from a `Settings` object using a key that must exist:

```ts
interface Settings {
  volume: number;
  brightness: number;
  theme: string;
}

function getSetting(settings: Settings, key: keyof Settings) {
  return settings[key];
}

const userSettings: Settings = {
  volume: 10,
  brightness: 70,
  theme: "dark"
};

console.log(getSetting(userSettings, "volume")); // ✅ 10
// console.log(getSetting(userSettings, "mode")); ❌ Error: "mode" is not a valid key
```

---

## 🧠 Summary

| Concept                        | Result Type             |         |
| ------------------------------ | ----------------------- | ------- |
| `keyof Person`                 | \`"name"                | "age"\` |
| `keyof { [key: string]: any }` | `string`                |         |
| `keyof typeof obj`             | Keys of the object type |         |

---

# TypeScript: Null & Undefined

TypeScript provides robust support for handling `null` and `undefined`, especially when `strictNullChecks` is enabled in your `tsconfig.json`.

---

## ✅ Enabling Strict Null Checks

To fully leverage TypeScript’s null safety, enable:

```json
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
````

---

## 🔢 Types: `null` and `undefined`

Both `null` and `undefined` are primitive types and can be combined with other types.

### Example

```ts
let value: string | undefined | null = null;
value = "hello";
value = undefined;
```

---

## 🔍 Optional Chaining (`?.`)

Safely access deeply nested properties without needing to check if each reference in the chain is null or undefined.

### Example

```ts
interface House {
  sqft: number;
  yard?: {
    sqft: number;
  };
}

function printYardSize(house: House) {
  const yardSize = house.yard?.sqft;
  if (yardSize === undefined) {
    console.log("No yard");
  } else {
    console.log(`Yard is ${yardSize} sqft`);
  }
}

let home: House = { sqft: 500 };
printYardSize(home); // 👉 Prints "No yard"
```

---

## ❓ Nullish Coalescing (`??`)

Provide fallback values **only** if the original value is `null` or `undefined`.

### Example

```ts
function printMileage(mileage: number | null | undefined) {
  console.log(`Mileage: ${mileage ?? "Not Available"}`);
}

printMileage(null); // 👉 Mileage: Not Available
printMileage(0);    // 👉 Mileage: 0 (0 is valid, not nullish)
```

---

## ❗ Null Assertion Operator (`!`)

Tells TypeScript that a value is **definitely not** `null` or `undefined`, bypassing safety checks.

⚠️ Use with caution!

### Example

```ts
function getValue(): string | undefined {
  return "hello";
}

let value = getValue();
console.log("Value length: " + value!.length); // ✅ OK at compile time, ⚠️ can throw at runtime
```

---

## 📦 Array Bounds Handling

Even with `strictNullChecks`, TypeScript does **not** treat out-of-bounds access as `undefined` by default.

To enable this behavior, set:

```json
{
  "compilerOptions": {
    "noUncheckedIndexedAccess": true
  }
}
```

### Example

```ts
let array: number[] = [1, 2, 3];
let first = array[0]; // 🔹 number
// With `noUncheckedIndexedAccess`: type is `number | undefined`
```

---

## 🧠 Summary Table

| Feature            | Operator                   | Purpose                                   |
| ------------------ | -------------------------- | ----------------------------------------- |
| Strict Null Checks | -                          | Require explicit null/undefined types     |
| Optional Chaining  | `?.`                       | Safe nested access                        |
| Nullish Coalescing | `??`                       | Fallback when `null` or `undefined`       |
| Null Assertion     | `!`                        | Assume value is not `null` or `undefined` |
| Array Index Check  | `noUncheckedIndexedAccess` | Adds undefined to array indexing          |

---

# TypeScript: Definitely Typed

JavaScript’s vast ecosystem includes many packages that **do not ship with TypeScript types**. This can limit type safety and autocompletion when using them in TypeScript projects.

---

## 🤷‍♂️ Using Untyped NPM Packages

Many NPM packages:
- Are unmaintained
- Do not include TypeScript types
- May not want to support TypeScript

When using such packages, TypeScript has **no type information**, which leads to unsafe code and reduced editor support.

---

## ✅ Definitely Typed

**Definitely Typed** is a **community-maintained project** that provides **TypeScript type definitions** for popular NPM packages that don't include them.

- It hosts thousands of type definitions
- Each package is available as `@types/your-package-name`

🔗 Repository: [DefinitelyTyped GitHub](https://github.com/DefinitelyTyped/DefinitelyTyped)

---

## 📦 Installing Types from Definitely Typed

Install using `npm`:

```bash
npm install --save-dev @types/jquery
````

📌 Replace `jquery` with any package name you need types for.

---

## 🛠️ After Installing

No additional setup is required:

* TypeScript will **automatically detect** and use the types
* Editors like **VS Code** will enable autocompletion and type-checking instantly

### Example

```ts
import $ from 'jquery';

$('#app').hide(); // ✅ Full type safety & autocompletion
```

---

## 🧠 Pro Tip

If you're unsure whether a type definition exists:

* Try installing it via:
  `npm install --save-dev @types/<package-name>`
* VS Code may even **prompt** you to install it when missing!

---

## 📚 Summary Table

| Scenario                            | Solution                                         |
| ----------------------------------- | ------------------------------------------------ |
| Using untyped NPM package           | Check for `@types/` package                      |
| Types available on DefinitelyTyped? | Install with `npm install --save-dev @types/pkg` |
| Editor support after install        | TypeScript will pick up types auto.              |

---





























