# TypeScript Reference Guide

## What is TypeScript?

JavaScript is flexible and fun to write, but it has one big drawback: it won't warn you about mistakes until you actually run the code. For example, if you accidentally pass a string into a function that expects a number, or misspell a property name, JavaScript won't complain until your app crashes in production or produces unexpected bugs.

**TypeScript solves this problem.** It is a strongly typed layer built directly on top of JavaScript. Think of it as JavaScript with a built-in safety net and spellchecker.

- **Catches errors early:** Finds bugs right in your code editor as you type, long before running the code.
- **Better developer experience:** Gives you rich autocomplete (IntelliSense), instant documentation, and fearless refactoring.
- **Compiles to plain JavaScript:** Browsers and Node.js don't run TypeScript directly. The TypeScript compiler (`tsc`) checks your code, strips away all the type annotations, and outputs clean, normal JavaScript.

```typescript
function add(a: number, b: number): number {
    return a + b;
}

console.log(add(10, 20)); // Output: 30

// TypeScript warns you immediately before you even save or run:
// Argument of type 'string' is not assignable to parameter of type 'number'.
// console.log(add("10", 20));

const age: number = 25;
console.log(age); // Output: 25
```

---

## Type Annotations (Telling TypeScript the Type)

A **type annotation** is how you explicitly tell TypeScript: *"Hey, this variable must always hold this kind of value."*

You do this by adding a colon (`:`) followed by the type name after your variable or parameter name.

```typescript
let username: string = "John";
let age: number = 25;
let isActive: boolean = true;

username = "David"; // Works fine, because "David" is a string

// TypeScript blocks this:
// Error: Type 'number' is not assignable to type 'string'
// username = 100;
```

---

## Type Inference (TypeScript Guessing for You)

The good news is you don't have to write type annotations on every single line of code!

If you initialize a variable with a value, TypeScript is smart enough to figure out (infer) the type automatically.

```typescript
// TypeScript looks at the value 25 and infers: age is a number
let age = 25;

age = 30; // Works fine

// TypeScript stops you:
// Error: Type 'string' is not assignable to type 'number'
// age = "30";

// Inferred as an array of numbers: number[]
const scores = [10, 20, 30];
scores.push(40); // Works fine
// scores.push("50"); // Error: Argument of type 'string' is not assignable to 'number'

// Inferred as an object with name: string, age: number, isDeveloper: boolean
const user = {
    name: "Peter",
    age: 25,
    isDeveloper: true,
};
```

> [!TIP]
> **Best Practice for Beginners & Pros:** Let TypeScript infer types whenever the initial value is obvious (like `let count = 0;`). Only write explicit annotations when declaring an uninitialized variable, typing function parameters, or defining complex object structures.

---

## Basic Types

Here are the everyday types you will use in TypeScript:

| Type | What it represents | Example |
| :--- | :--- | :--- |
| `string` | Text values | `"Peter"`, `'Hello'`, `` `Total: ${count}` `` |
| `number` | Integers, floats, negative numbers, `NaN` | `25`, `10.5`, `-3` |
| `boolean` | True or false values | `true`, `false` |
| `undefined` | Uninitialized variable | `undefined` |
| `null` | Intentionally empty value | `null` |
| `any` | Turns off type checking completely | `let x: any = "anything";` |
| `unknown` | Safe version of `any`; must check type before using | `let x: unknown = "data";` |
| `void` | When a function returns nothing | `function log(): void` |
| `never` | When a function never finishes (throws an error or infinite loop) | `function fail(): never` |

### `any` vs `unknown` (A Very Common Interview Question!)

- **`any` (The "I don't care" escape hatch):** It tells TypeScript: *"Don't check this variable at all."* You can call nonexistent methods, access random properties, or assign it anywhere. It feels convenient, but if you overuse it, you lose all the safety TypeScript offers.
- **`unknown` (The "I don't know yet, but be careful" type):** It can accept any value just like `any`, but TypeScript won't let you call methods or pass it into other functions until you check its type first (called **type narrowing**).

```typescript
// 1. Using 'any' (Dangerous)
let dangerousData: any = "Hello World";
dangerousData.someRandomMethod(); // Compiles fine, but crashes at runtime!

// 2. Using 'unknown' (Safe)
let safeData: unknown = "Hello World";

// safeData.toUpperCase(); // Error: 'safeData' is of type 'unknown'

// You must prove to TypeScript what type it is first:
if (typeof safeData === "string") {
    console.log(safeData.toUpperCase()); // Output: "HELLO WORLD" (Autocomplete works here!)
}
```

---

## Contextual Typing

Sometimes TypeScript figures out types from the **surrounding context** where an expression lives. You don't need to specify types because TypeScript already knows what should be there.

```typescript
const names = ["Peter", "John", "David"];

// You don't have to write (name: string).
// TypeScript already knows 'names' is string[], so 'name' must be a string!
names.forEach((name) => {
    console.log(name.toUpperCase());
});
```

Because TypeScript understands array methods like `.forEach()` and `.map()`, it provides instant autocompletion for string methods on `name` automatically.

---

## Arrays

In TypeScript, arrays usually contain items of the same type. You can declare array types in two ways:

1. **Square bracket syntax (`type[]`):** Clean and most popular.
2. **Generic syntax (`Array<type>`):** Does the exact same thing.

```typescript
// Square bracket notation (Preferred)
const numbers: number[] = [10, 20, 30];
const names: string[] = ["Peter", "John"];
const statuses: boolean[] = [true, false, true];

// Generic notation
const scoreList: Array<number> = [85, 90, 95];
const nameList: Array<string> = ["Peter", "John"];

// Array containing multiple types (Union type)
const mixedItems: (string | number)[] = [1, 2, 3, "Park", 4, "Beach"];
const mixedItemsGeneric: Array<string | number> = [1, 2, 3, "Park", 4, "Beach"];
```

### Array of Objects

In real-world applications (like fetching data from an API), you deal with lists of objects all the time:

```typescript
// 1. Inline object type array
const inlineRestaurants: {
    id: number;
    name: string;
    rating: number;
}[] = [
    { id: 1, name: "Banana Leaf", rating: 4.8 },
    { id: 2, name: "Kerala Mess", rating: 4.7 },
];

// 2. Using an interface for readability and reuse (Recommended)
interface Restaurant {
    id: number;
    name: string;
    rating: number;
}

const restaurants: Restaurant[] = [
    { id: 1, name: "Pizza House", rating: 4.5 },
    { id: 2, name: "Burger Point", rating: 4.2 },
];
```

### Read-Only Arrays (Preventing accidental mutations)

If you have a list of values that should never be altered or pushed to, mark it as `readonly`:

```typescript
const fruits: readonly string[] = ["Apple", "Mango"];

console.log(fruits[0]); // Output: "Apple"

// TypeScript stops you from accidentally modifying the data:
// fruits.push("Orange"); // Error: Property 'push' does not exist on 'readonly string[]'
// fruits[0] = "Banana";  // Error: Index signature only permits reading

// Alternative generic syntax:
const vegetables: ReadonlyArray<string> = ["Apple", "Mango"];
```

---

## Tuples (Fixed-Pattern Arrays)

A normal array can have any length. A **tuple**, on the other hand, is an array with a **fixed length** and a **specific type at each position**.

Think of coordinates `[x, y]` or React's `useState()` hook which always returns `[value, setterFunction]`.

```typescript
// Position 0 MUST be a number, position 1 MUST be a string
const userRecord: [number, string] = [1, "hello"];

console.log(userRecord[0]); // Output: 1
console.log(userRecord[1]); // Output: "hello"

// Readonly tuple: prevents pushing or changing values
const user: readonly [string, number] = ["Peter", 25];

console.log(user[0]); // Output: "Peter"

// TypeScript errors on mutation:
// user[0] = "John"; // Error: Cannot assign to read-only property
// user.push(30);    // Error: Property 'push' does not exist on type 'readonly [string, number]'
```

### Syntax

```typescript
let variableName: [Type1, Type2, Type3];
```

### Union Array vs Tuple (A Common Confusion!)

It's easy for beginners to confuse `(string | number)[]` with `[string, number]`. Here is the difference:

```typescript
// 1. Union Array: Can hold strings AND numbers in ANY order and ANY quantity
let unionArray: (string | number)[];
unionArray = [1, 2, "hello", 4, "world"]; // Valid
unionArray = ["only", "strings"];         // Valid

// 2. Tuple: Fixed length of 2, element 0 MUST be string, element 1 MUST be number
let strictTuple: [string, number];
strictTuple = ["Peter", 25]; // Valid
// strictTuple = [25, "Peter"];     // Error: Wrong order!
// strictTuple = ["Peter", 25, 30]; // Error: Too many elements!
```

### Tuples with Optional Elements

You can mark elements at the end of a tuple as optional using `?`:

```typescript
let user: [string, number?];

user = ["Peter"];     // Valid (number omitted)
user = ["Peter", 25]; // Valid (number provided)
// user = ["Peter", "28"]; // Error: Second element must be a number!
```

### Tuples with Rest Elements

When you know the first element but want any number of trailing items:

```typescript
// First element is a student name, followed by any number of test scores
let scores: [string, ...number[]];

scores = ["Peter", 80, 90, 100]; // Valid: string followed by three numbers
scores = ["John"];               // Valid: string followed by zero numbers

// TypeScript error: Type 'string' is not assignable to type 'number'
// scores = ["David", "A"];
```

---

## Working with Objects

Objects are at the heart of JavaScript and TypeScript. TypeScript lets you define exactly what properties an object should have.

### Type Annotations and Optional Properties (`?`)

Not every object has all values filled in. If a field might be missing (like a user's bio or secondary email), mark it as optional using `?`:

```typescript
// 1. Inferred type
const userInferred = {
    name: "Peter",
    age: 25,
    isActive: true,
};

// 2. Explicit type annotation
let user: {
    name: string;
    age: number;
    email?: string; // The '?' means email is optional
};

user = {
    name: "Peter",
    age: 25,
    // email is optional, so omitting it is completely valid!
};

user = {
    name: "John",
    age: 30,
    email: "john@example.com",
};
```

### `readonly` Object Properties

If a property should never be reassigned after creation (like a database ID), mark it with `readonly`:

```typescript
const account: {
    readonly id: number;
    name: string;
} = {
    id: 101,
    name: "Peter",
};

account.name = "John"; // Allowed: name is editable

// TypeScript stops you from overwriting the id:
// account.id = 102; // Error: Cannot assign to 'id' because it is a read-only property
```

> [!NOTE]
> `readonly` is a compiler safety check. It helps you catch accidental overwrites in your code, but it doesn't freeze the object at runtime like `Object.freeze()`.

### Reusable Object Structures (`type` and `interface`)

When the same object structure needs to be reused across multiple places:

```typescript
// Using type alias
type User = {
    id: number;
    name: string;
    age: number;
};

const user1: User = { id: 1, name: "Peter", age: 25 };
const user2: User = { id: 2, name: "John", age: 30 };

// Using interface
interface Product {
    id: number;
    name: string;
    price: number;
    inStock: boolean;
    description?: string; // Optional property
}

const product: Product = {
    id: 101,
    name: "T-Shirt",
    price: 350,
    inStock: true,
};
```

### Nested Objects

You can nest object types to match real data shapes:

```typescript
interface Address {
    city: string;
    pincode: number;
}

interface UserWithAddress {
    id: number;
    name: string;
    address: Address; // Nested interface
}

const customer: UserWithAddress = {
    id: 1,
    name: "Peter",
    address: {
        city: "Chennai",
        pincode: 600001,
    },
};

console.log(customer.address.city); // Output: "Chennai"
```

### Objects Containing Arrays and Union Properties

```typescript
// Array inside an object
interface RestaurantProfile {
    id: number;
    name: string;
    cuisines: string[];
    rating: number;
}

const restaurant: RestaurantProfile = {
    id: 101,
    name: "Pizza House",
    cuisines: ["Italian", "Fast Food"],
    rating: 4.5,
};

// Union property: allows string OR number
interface ContactUser {
    name: string;
    phone: string | number;
}

const contact1: ContactUser = { name: "Peter", phone: "9876543210" };
const contact2: ContactUser = { name: "John", phone: 9876543210 };
```

---

## Interfaces (Blueprints for Objects)

An **interface** defines the structure that an object must follow. It specifies what properties an object needs, their types, and whether each property is required, optional, or a function method.

```typescript
interface User {
    id: number;
    name: string;
    age: number;
    isActive: boolean;
    email?: string;  // Optional property
    greet(): string; // Function signature (must return a string)
}

const user: User = {
    id: 1,
    name: "Peter",
    age: 24,
    isActive: true,
    greet() {
        return `Hello, ${this.name}`;
    },
};

console.log(user.greet()); // Output: "Hello, Peter"
```

### Extending Interfaces (`extends`)

Instead of copying and pasting fields across multiple interfaces, one interface can inherit properties from another using `extends`.

```typescript
interface Person {
    name: string;
    age: number;
}

// Single extension
interface Employee extends Person {
    salary: number;
    employeeId: number;
}

// Avoiding repetition by sharing a base interface
interface BasePerson {
    name: string;
    email: string;
}

interface Admin extends BasePerson {
    permissions: string[];
}

interface Customer extends BasePerson {
    customerId: number;
}
```

### Declaration Merging (A Unique Interface Feature)

If you declare an interface with the **same name multiple times in the same scope**, TypeScript automatically merges their properties together into a single interface.

```typescript
interface User {
    name: string;
}

interface User {
    age: number;
}

// 'User' now requires BOTH 'name' and 'age':
const user: User = {
    name: "Peter",
    age: 24,
};
```

> [!TIP]
> Declaration merging is handy when writing plugins or third-party libraries where users need to add extra properties to an existing global type (like adding custom properties to `window` or `Express.Request`).

---

## Type Aliases (`type`)

A **type alias** lets you give a custom, reusable name to any type so you can reuse it throughout your code.

Unlike `interface`, which is primarily designed for object shapes, a `type` alias can represent **anything**: primitives, objects, union types, tuples, arrays, or functions.

```typescript
// 1. Aliasing primitive types
type Username = string;

let userName: Username = "Peter";
userName = "John"; // Valid

// TypeScript error: Type 'number' is not assignable to type 'string'
// userName = 123;

type Age = number;
type Email = string;
type IsActive = boolean;

let age: Age = 25;
let email: Email = "peter@example.com";
let isActive: IsActive = true;

// 2. Aliasing object shapes
type Product = {
    readonly id: number;
    name: string;
    price: number;
    description?: string;
};

const product: Product = {
    id: 1,
    name: "T-shirt",
    price: 350,
};

// 3. Aliasing array types
type RestaurantItem = {
    id: number;
    name: string;
    rating: number;
};

type RestaurantList = RestaurantItem[];

const restaurants: RestaurantList = [
    { id: 1, name: "Pizza House", rating: 4.5 },
    { id: 2, name: "Burger Point", rating: 4.2 },
];
```

### Combining Types with Intersection (`&`)

Just like interfaces use `extends` to inherit properties, type aliases use the **intersection operator (`&`)** to merge multiple type requirements into one.

Think of it as: *"This new type must satisfy Type A **AND** Type B."*

```typescript
type Person = {
    name: string;
    age: number;
};

// Employee combines all properties of Person plus employeeId and salary
type Employee = Person & {
    employeeId: number;
    salary: number;
};

const employee: Employee = {
    name: "Peter",
    age: 25,
    employeeId: 101,
    salary: 50000,
};
```

### Union Types and String Literal Types (Super Useful!)

One of the most practical everyday uses of `type` is creating string literal unions. This prevents typos and limits variables to specific allowed words:

```typescript
type OrderStatus = "pending" | "confirmed" | "delivered";

let status: OrderStatus = "pending";

status = "confirmed"; // Valid
status = "delivered"; // Valid

// TypeScript catches the typo immediately!
// Error: Type '"cancelled"' is not assignable to type 'OrderStatus'
// status = "cancelled";
```

---

## When to Use `interface` vs `type`?

Both `interface` and `type` can describe object shapes and can often be used interchangeably. However, they have distinct differences and strengths:

| Feature | `interface` | `type` Alias |
| :--- | :--- | :--- |
| **Describe object structure** | Yes | Yes |
| **Optional & readonly properties** | Yes | Yes |
| **Extend / combine object shapes** | `extends` (`interface B extends A`) | Intersection `&` (`type B = A & B`) |
| **Define unions (e.g. `"pending" \| "done"`)** | No, not directly | Yes |
| **Alias a primitive (e.g. `string`, `number`)** | No | Yes (`type Age = number;`) |
| **Tuples support** | Difficult to express | Clean (`type Point = [number, number];`) |
| **Declaration merging** | Yes (merges multiple declarations with same name) | No (throws duplicate identifier error) |

### Simple Rule of Thumb

- **Use `interface`** when modeling objects, components, or API data structures where you might want to extend them or benefit from declaration merging.
- **Use `type`** when you need union types (`"light" | "dark"`), primitives, tuples, or combining types together using `&` (intersections).

> Both are suitable for object structures. Type aliases are additionally useful for unions, primitive aliases, and other type combinations.

---

## Functions in TypeScript

Typing functions ensures that callers pass the correct arguments and receive the expected return value.

### Parameter and Return Type Annotations

```typescript
function calculateDiscount(price: number, discountRate: number): number {
    return price - (price * discountRate);
}

const finalPrice = calculateDiscount(1000, 0.1); // Output: 900
```

### Optional and Default Parameters

- **Optional parameters (`?`)** must always come **after** required parameters.
- **Default parameters** automatically provide a fallback value and infer the parameter type.

```typescript
// 1. Optional parameter greeting?
function greetUser(name: string, greeting?: string): string {
    return greeting ? `${greeting}, ${name}!` : `Hello, ${name}!`;
}

console.log(greetUser("Peter"));          // Output: "Hello, Peter!"
console.log(greetUser("Peter", "Welcome")); // Output: "Welcome, Peter!"

// 2. Default parameter role = "User"
function assignRole(username: string, role: string = "User"): string {
    return `${username} is assigned to role: ${role}`;
}

console.log(assignRole("John"));          // Output: "John is assigned to role: User"
console.log(assignRole("AdminUser", "Admin")); // Output: "AdminUser is assigned to role: Admin"
```

### Rest Parameters

When a function accepts an unknown number of arguments, type the rest parameter as an array:

```typescript
function sumAllNumbers(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sumAllNumbers(10, 20, 30, 40)); // Output: 100
```

### Function Type Signatures

You can define the signature of a function using a `type` alias, making it easy to pass functions as callbacks:

```typescript
type MathOperation = (a: number, b: number) => number;

const multiply: MathOperation = (x, y) => x * y;
const divide: MathOperation = (x, y) => x / y;

console.log(multiply(5, 4)); // Output: 20
```
