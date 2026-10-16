# TypeScript Reference Guide

## What is TypeScript?

JavaScript is flexible and fun to write, but it has one big drawback: it won't warn you about mistakes until you actually run the code. For example, if you accidentally pass a string into a function that expects a number, or misspell a property name, JavaScript won't complain until your app crashes in production or produces unexpected bugs.

**TypeScript solves this problem.** It is a strongly typed programming language built directly on top of JavaScript. Think of it as JavaScript with a built-in safety net and spellchecker.

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

## Type Annotations & Type Inference

### Type Annotations (Telling TypeScript the Type)

A **type annotation** explicitly tells TypeScript what data type a variable, function parameter, or return value should hold. You do this by writing a colon (`:`) followed by the type name.

```typescript
let username: string = "John";
let age: number = 25;
let isActive: boolean = true;

username = "David"; // Valid: matches string

// TypeScript error: Type 'number' is not assignable to type 'string'
// username = 100;
```

### Type Inference (TypeScript Guessing for You)

You don't need to write explicit annotations everywhere! If you initialize a variable with a value, TypeScript automatically figures out (infers) the type for you.

```typescript
let age = 25; // TypeScript looks at 25 and infers: age is a number

age = 30; // Valid

// TypeScript stops you:
// Error: Type 'string' is not assignable to type 'number'
// age = "30";

const scores = [10, 20, 30]; // Inferred as number[]
scores.push(40); // Valid
// scores.push("50"); // Error: Argument of type 'string' is not assignable to 'number'

const user = {
    name: "Peter",
    age: 25,
    isDeveloper: true,
};
// Inferred shape: { name: string; age: number; isDeveloper: boolean; }
```

> [!TIP]
> **Rule of Thumb:** Let TypeScript infer simple primitive assignments (like `let count = 0;`). Write explicit annotations when declaring an uninitialized variable, typing function parameters, or defining complex object structures.

### Contextual Typing

Contextual typing happens when TypeScript infers the type of an expression from its **surrounding location**:

```typescript
const names = ["Peter", "John", "David"];

// You don't have to write (name: string).
// TypeScript already knows 'names' is string[], so 'name' must be a string!
names.forEach((name) => {
    console.log(name.toUpperCase());
});
```

---

## Basic & Special Types

| Type | What it represents | Example |
| :--- | :--- | :--- |
| `string` | Text values | `"Peter"`, `'Hello'`, `` `Count: ${n}` `` |
| `number` | Integers, floats, negative numbers, `NaN` | `25`, `10.5`, `-3` |
| `boolean` | Logical `true` or `false` | `true`, `false` |
| `undefined` | Uninitialized variable | `undefined` |
| `null` | Intentionally empty value | `null` |
| `any` | Turns off type checking completely | `let x: any = "anything";` |
| `unknown` | Safe counterpart to `any`; check before use | `let x: unknown = "data";` |
| `void` | When a function returns no useful value | `function log(): void` |
| `never` | Values that never occur (errors, infinite loops) | `function fail(): never` |

### `any` vs `unknown`

- **`any` (The "I don't care" escape hatch):** Tells TypeScript to disable all checks. You can access nonexistent methods or assign it anywhere. While convenient, overusing `any` turns TypeScript back into untyped JavaScript and can cause runtime crashes.
- **`unknown` (The "Check first" type):** Accepts any value just like `any`, but TypeScript blocks you from performing operations on it until you check its type first (**type narrowing**).

```typescript
// 1. any (Dangerous)
let dangerousData: any = "Hello World";
dangerousData.toUpperCase(); // Allowed
// dangerousData.toFixed(2); // Compiles fine, but crashes at runtime with TypeError!

// 2. unknown (Safe)
let safeData: unknown = "Hello World";

// safeData.toUpperCase(); // Error: 'safeData' is of type 'unknown'

// You must prove the type to TypeScript first:
if (typeof safeData === "string") {
    console.log(safeData.toUpperCase()); // Output: "HELLO WORLD" (Autocomplete works here!)
}

// Narrowing unknown across different types:
function displayValue(value: unknown): void {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else if (typeof value === "number") {
        console.log(value.toFixed(2));
    } else if (typeof value === "boolean") {
        console.log(value ? "Yes" : "No");
    } else {
        console.log("Unknown type");
    }
}
```

### `void` vs `never`

- **`void`**: Used as a function return type when the function completes normally without returning a useful value to the caller (it returns `undefined`). A `void` function can also use a bare `return;` statement to exit early.
- **`never`**: The function **never returns normally at all**—it either throws an unhandled exception or enters an infinite loop.

```typescript
// Returns a number
function getAge(): number {
    return 25;
}

// Returns a string
function getName(): string {
    return "Peter";
}

// void: does work, but does not return a value
function greet(): void {
    console.log("Hello!");
}

// void functions can exit early using a bare return:
function checkEligibility(userAge: number): void {
    if (userAge < 18) {
        console.log("Not eligible");
        return; // Exits early
    }
    console.log("Eligible");
}

function getValue(): void {
    return undefined; // Allowed: void can return undefined
}

// never: execution never finishes normally
function throwError(msg: string): never {
    throw new Error(msg);
}

function runForever(): never {
    while (true) {
        console.log("Running...");
    }
}

// Quick comparison in execution:
function first(): void {
    console.log("Hello"); // Executes and finishes
}

function second(): never {
    throw new Error("Failed"); // Stops immediately by throwing error
}
```

### Exhaustive Checking with `never`

A powerful pattern using `never` is ensuring that all cases in a `switch` statement are handled:

```typescript
type PaymentStatus = "pending" | "paid" | "failed";

function processPayment(status: PaymentStatus): string {
    switch (status) {
        case "pending":
            return "Payment is pending";
        case "paid":
            return "Payment successful";
        case "failed":
            return "Payment failed";
    }

    // If all cases are handled, 'status' here is of type 'never'
    const unreachable: never = status;
    return unreachable;
}

// If someone later adds "refunded" to PaymentStatus, TypeScript immediately
// flags an error on 'unreachable', reminding you to handle "refunded"!
```

---

## Literal Types

A **literal type** restricts a variable to hold **only one exact value** instead of any general string, number, or boolean.

```typescript
let username: "Peter" = "Peter";
// username = "John"; // Error: Type '"John"' is not assignable to type '"Peter"'

let exactScore: 100 = 100;
// exactScore = 200; // Error: Type '200' is not assignable to type '100'

let mustBeTrue: true = true;
// mustBeTrue = false; // Error: Type 'false' is not assignable to type 'true'
```

### Important: Inferred `string` vs String Literal

When declaring a variable with `let`, TypeScript infers the broad type `string` because `let` variables can change:

```typescript
let theme = "light"; // Inferred as 'string'

function changeTheme(theme: "light" | "dark") {
    console.log(theme);
}

// changeTheme(theme); // Error: Argument of type 'string' is not assignable to parameter of type '"light" | "dark"'

// Fix 1: Use const (inferred as the literal type "light")
const fixedTheme = "light";
changeTheme(fixedTheme); // Valid

// Fix 2: Explicitly annotate the literal type
let explicitTheme: "light" | "dark" = "light";
changeTheme(explicitTheme); // Valid
```

---

## Union Types (`|`)

A **Union type** allows a variable, object property, or function parameter to accept **one of several allowed types** (meaning **OR**).

```typescript
// Union of primitive types
let value: string | number = "Hello";
value = 100;     // Valid
value = "World"; // Valid
// value = true; // Error: Type 'boolean' is not assignable to 'string | number'

let data: string | number | boolean;
data = "Hello"; // Valid
data = 100;     // Valid
data = true;    // Valid
// data = null; // Error: Type 'null' is not assignable

// Allowing null or string
let currentUsername: string | null = "Peter";
currentUsername = null; // Valid
// currentUsername = 100; // Error: Type 'number' is not assignable
```

### Literal Unions (Predefined Word Sets)

Combining literal types with unions lets you create clean, type-safe choices that eliminate typos:

```typescript
type OrderStatus = "pending" | "confirmed" | "delivered";

let status: OrderStatus = "pending";
status = "confirmed"; // Valid
status = "delivered"; // Valid
// status = "cancelled"; // Error: Type '"cancelled"' is not assignable to type 'OrderStatus'

// In function parameters:
function setTheme(theme: "light" | "dark") {
    console.log(`Setting theme to: ${theme}`);
}

setTheme("light"); // Valid
setTheme("dark");  // Valid
// setTheme("blue"); // Error: Argument of type '"blue"' is not assignable

// In object types:
type UserRoleRecord = {
    name: string;
    role: "admin" | "user";
};

const user1: UserRoleRecord = { name: "Peter", role: "admin" };
const user2: UserRoleRecord = { name: "John", role: "user" };
```

---

## Arrays & Tuples

### Arrays

Arrays in TypeScript can be defined using square bracket syntax (`type[]`) or generic syntax (`Array<type>`):

```typescript
// Bracket notation (Preferred)
const numbers: number[] = [10, 20, 30];
const names: string[] = ["Peter", "John"];
const statuses: boolean[] = [true, false, true];

// Generic notation
const scoreList: Array<number> = [1, 2, 3, 4, 5];
const nameList: Array<string> = ["Peter", "John"];

// Array holding multiple allowed types (Union Array)
const mixedItems: (string | number)[] = [1, 2, "Park", 4, "Beach"];
const mixedItemsGeneric: Array<string | number> = [1, 2, "Park", 4, "Beach"];
```

### Array of Objects

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

// 2. Using an interface for reusable clarity (Recommended)
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

### Read-Only Arrays

A `readonly` array prevents mutation methods like `.push()`, `.pop()`, and direct index reassignments:

```typescript
const fruits: readonly string[] = ["Apple", "Mango"];
console.log(fruits[0]); // Output: "Apple"

// TypeScript stops mutations during compile time:
// fruits.push("Orange"); // Error: Property 'push' does not exist on 'readonly string[]'
// fruits[0] = "Banana";  // Error: Index signature only permits reading

// Alternative generic syntax:
const numbersList: ReadonlyArray<number> = [10, 20, 30];
```

> [!NOTE]
> `readonly` is a **compile-time protection**. It prevents accidental modifications in your TypeScript code, but JavaScript arrays remain standard arrays in the final bundle.

### Tuples (Fixed-Pattern Arrays)

A **tuple** is an array with a **fixed length** where each specific index position has a designated type.

```typescript
// Position 0 MUST be a number, Position 1 MUST be a string
const record: [number, string] = [1, "hello"];
console.log(record[0]); // Output: 1
console.log(record[1]); // Output: "hello"

// Readonly tuple prevents mutation
const userRecord: readonly [string, number] = ["Peter", 25];
console.log(userRecord[0]); // Output: "Peter"

// userRecord[0] = "John"; // Error: Cannot assign to read-only property
// userRecord.push(30);    // Error: Property 'push' does not exist on readonly tuple
```

### Union Array vs Tuple (A Crucial Distinction!)

It is easy for beginners to mix up `(string | number)[]` and `[string, number]`. Here is the difference:

```typescript
// 1. Union Array: Any number of items, strings and numbers mixed in ANY order
let a: (string | number)[];
a = [101, "Shoes", 1500, "In Stock"]; // Valid
a = ["Only", "Words"];                // Valid

// 2. Tuple: EXACTLY 2 items, index 0 MUST be string, index 1 MUST be number
let b: [string, number];
b = ["Peter", 25]; // Valid

// b = [25, "Peter"];     // Error: Wrong order!
// b = ["Peter", "25"];   // Error: Index 1 expects number!
// b = ["Peter", 25, 30]; // Error: Expected 2 elements, but got 3!

// Another tuple example with index assignment checks:
const itemValues: [number, string, number] = [101, "Shoes", 1500];
// itemValues[0] = "101"; // Error: Index 0 expects number, not string
```

### Tuples with Optional and Rest Elements

```typescript
// Optional element with '?' (must come at the end)
let employee: [string, number?];
employee = ["Sam"];     // Valid
employee = ["Sam", 28]; // Valid
// employee = ["Sam", "28"]; // Error: Second element must be a number!

// Rest element for variable number of trailing items
let studentScores: [string, ...number[]];
studentScores = ["Peter", 80, 90, 100]; // Valid: 1 string + 3 numbers
studentScores = ["John"];               // Valid: 1 string + 0 numbers
// studentScores = ["David", "A"];       // Error: Trailing elements must be numbers!
```

---

## Objects, Optional Properties & `readonly`

### Type Annotations and Optional Properties (`?`)

When defining object types, mark properties that might not be provided with `?`:

```typescript
let user: {
    name: string;
    age: number;
    email?: string; // Optional property
};

user = {
    name: "Peter",
    age: 25,
    // email is omitted, completely valid!
};

user = {
    name: "John",
    age: 30,
    email: "john@example.com",
};
```

### `readonly` Object Properties

`readonly` ensures that a property cannot be reassigned once initialized:

```typescript
interface Employee {
    readonly id: number;
    name: string;
    salary?: number;
}

const employee: Employee = {
    id: 101,
    name: "Peter",
    salary: 50000,
};

// Trying to overwrite the id causes an error:
// employee.id = 102; // Error: Cannot assign to 'id' because it is a read-only property

// Modifying other mutable properties is completely allowed:
employee.name = "John";  // Valid
employee.salary = 60000; // Valid (salary is optional, but mutable and expects number)
```

> [!NOTE]
> - `readonly` is shallow by default: making an object property `readonly` does not automatically make its nested object properties read-only.
> - `readonly` only applies to reassigning the property itself. Non-readonly fields on the same object can still be updated.

### Nested Objects & Objects with Arrays

```typescript
// Nested object interface
interface Address {
    city: string;
    pincode: number;
}

interface UserProfile {
    id: number;
    name: string;
    address: Address; // Nested interface
}

const userProfile: UserProfile = {
    id: 1,
    name: "Peter",
    address: {
        city: "Chennai",
        pincode: 600001,
    },
};

console.log(userProfile.address.city); // Output: "Chennai"

// Object containing an array
interface RestaurantProfile {
    id?: number;
    name: string;
    rating: number;
    cuisines: string[];
}

const restaurant: RestaurantProfile = {
    id: 101,
    name: "Pizza House",
    rating: 4.5,
    cuisines: ["Italian", "Fast Food"],
};

// Object with union property
interface Contact {
    name: string;
    phone: string | number; // Accepts string or number
}

const contact1: Contact = { name: "Peter", phone: "9876543210" };
const contact2: Contact = { name: "John", phone: 9876543210 };
```

---

## Interfaces (Blueprints for Objects)

An **interface** defines the structural contract that an object must follow—specifying which properties it needs, their types, and any method signatures.

```typescript
interface User {
    id: number;
    name: string;
    age: number;
    isActive: boolean;
    email?: string;  // Optional property
    greet(): string; // Method signature
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

Interfaces can inherit properties from other interfaces using the `extends` keyword, avoiding duplicate code:

```typescript
// Base interface
interface Person {
    name: string;
    age: number;
}

// Single extension
interface Employee extends Person {
    salary: number;
    employeeId: number;
}

const dev: Employee = {
    name: "Peter",
    age: 25,
    employeeId: 101,
    salary: 50000,
};

// Reusing a common base across multiple roles
interface BaseAccount {
    name: string;
    email: string;
}

interface Admin extends BaseAccount {
    permissions: string[];
}

interface Customer extends BaseAccount {
    customerId: number;
}
```

### Declaration Merging

When multiple interfaces are declared with the **exact same name in the same scope**, TypeScript automatically merges their property definitions into a single combined interface:

```typescript
interface User {
    name: string;
}

interface User {
    age: number;
}

// The resulting 'User' interface requires both 'name' and 'age':
const mergedUser: User = {
    name: "Peter",
    age: 24,
};
```

> [!TIP]
> Declaration merging is helpful when extending existing third-party libraries (e.g., adding custom properties to `window` or `Express.Request`).

---

## Type Aliases (`type`) & Intersection (`&`)

A **type alias** lets you give a custom, reusable name to any type so you can reuse it throughout your codebase.

Unlike `interface`, which is tailored to object shapes, a `type` alias can represent **anything**: primitives, objects, union types, tuples, arrays, or function signatures.

```typescript
// 1. Primitive aliases
type Username = string;
let myUser: Username = "Peter";
myUser = "John"; // Valid
// myUser = 123; // Error: Type 'number' is not assignable to 'string'

type Age = number;
type Email = string;
type IsActive = boolean;

let userAge: Age = 25;
let userEmail: Email = "peter@example.com";
let activeStatus: IsActive = true;

// 2. Object shape alias
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

// 3. Array type alias
type RestaurantItem = {
    id: number;
    name: string;
    rating: number;
};

type RestaurantList = RestaurantItem[];

const foodPlaces: RestaurantList = [
    { id: 1, name: "Pizza House", rating: 4.5 },
    { id: 2, name: "Burger Point", rating: 4.2 },
];
```

### Combining Types with Intersection (`&`)

While interfaces use `extends`, type aliases use the **intersection operator (`&`)** to combine multiple type definitions into one (meaning **AND**).

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

### Union (`|`) vs Intersection (`&`)

| Feature | Union (`\|`) | Intersection (`&`) |
| :--- | :--- | :--- |
| **Logic** | **OR** (alternatives) | **AND** (requirements) |
| **Meaning** | Value can be one of the specified types | Value must satisfy all combined types |
| **Example** | `string \| number` | `Person & Employee` |
| **Best used for** | Alternative values or state variants | Combining multiple object shapes |

### Important: Conflicting Property Types in Intersections

If two types define the exact same property with incompatible types, the intersection creates `never`:

```typescript
type A = { value: string };
type B = { value: number };

type C = A & B;
// C["value"] is (string & number). No value can be both at once, so it becomes 'never'!
```

---

## When to Use `interface` vs `type`?

Both `interface` and `type` can describe object structures and are often interchangeable. Here is a quick reference:

| Feature | `interface` | `type` Alias |
| :--- | :--- | :--- |
| **Describe object structure** | Yes | Yes |
| **Optional & readonly properties** | Yes | Yes |
| **Extend / combine object shapes** | `extends` (`interface B extends A`) | Intersection `&` (`type B = A & B`) |
| **Define unions (e.g. `"pending" \| "done"`)** | No, not directly | Yes |
| **Alias a primitive (e.g. `string`, `number`)** | No | Yes (`type Age = number;`) |
| **Tuples support** | Difficult to express | Clean (`type Point = [number, number];`) |
| **Declaration merging** | Yes (merges same-name interfaces) | No (duplicate identifier error) |

### Simple Rule of Thumb

- **Use `interface`** when modeling object models, components, class contracts, or public APIs where inheritance (`extends`) or declaration merging is useful.
- **Use `type`** when you need union types (`"light" | "dark"`), primitives, tuples, function signatures, or combining shapes using `&`.

> Both are suitable for object structures. Type aliases are additionally useful for unions, primitive aliases, and other type combinations.

---

## Enums (Enumeration)

An **enum** allows you to define a group of named constants instead of repeating hardcoded values:

### 1. Numeric Enums

By default, numeric enum members start at `0` and auto-increment by `1`:

```typescript
enum Status {
    Pending, // 0
    Success, // 1
    Failed,  // 2
}

let currentStatus: Status = Status.Pending;
currentStatus = Status.Success; // Valid (value: 1)

// Custom start values:
enum StatusCode {
    Pending = 10,
    Success = 20,
    Failed = 30,
}
```

### Numeric Enum Reverse Mapping

Numeric enums automatically support **reverse mapping**, meaning you can look up the member name using its numeric value:

```typescript
enum Status {
    Pending, // 0
    Success, // 1
    Failed,  // 2
}

console.log(Status.Pending); // Output: 0
console.log(Status[0]);      // Output: "Pending"
```

> [!NOTE]
> Under the hood, TypeScript compiles numeric enums into an object like `{ "0": "Pending", "Pending": 0 }`. That is why accessing `Status[0]` retrieves the member name.

### 2. String Enums

String enums require explicit string values for each member and do not provide reverse mapping:

```typescript
enum PaymentStatus {
    Pending = "PENDING",
    Paid = "PAID",
    Failed = "FAILED",
}

function processPayment(status: PaymentStatus) {
    console.log(status);
}

processPayment(PaymentStatus.Paid); // Output: "PAID"
// processPayment("PAID"); // Error: Argument must be PaymentStatus.Paid
```

### Enums vs String Literal Unions

While enums generate a JavaScript object at runtime, string literal unions disappear completely upon compilation:

```typescript
// Modern TypeScript alternative: zero runtime overhead
type UserRole = "ADMIN" | "EDITOR" | "VIEWER";

let role: UserRole = "ADMIN"; // Clean, simple, and zero extra JS bundle size
```

---

## Functions in TypeScript

Typing functions guarantees that callers supply the expected arguments and receive the correct return type.

### Parameter & Return Types

```typescript
function add(a: number, b: number): number {
    return a + b;
}

console.log(add(10, 20)); // Output: 30
// add("10", 20); // Error: Argument of type 'string' is not assignable to 'number'

function calculateDiscount(price: number, discountRate: number): number {
    return price - (price * discountRate);
}

const finalPrice = calculateDiscount(1000, 0.1); // Output: 900
```

### Assigning Function Types to Variables

```typescript
let calculate: (a: number, b: number) => number;

calculate = (a, b) => a + b;
console.log(calculate(2, 3)); // Output: 5

// Functions with different return types:
let getName: () => string;
getName = () => "Peter";
console.log(getName()); // Output: "Peter"

let isAdult: (age: number) => boolean;
isAdult = (age) => age >= 18;
console.log(isAdult(20)); // Output: true
```

### Function Type Alias vs Implementation

```typescript
// 1. Define the type signature using 'type'
type MathOperation = (a: number, b: number) => number;

// 2. Implement functions matching that type signature
const multiply: MathOperation = (a, b) => a * b;
const subtract: MathOperation = (a, b) => a - b;
const divide: MathOperation = (a, b) => a / b;

console.log(multiply(4, 5)); // Output: 20
```

### Optional & Default Parameters

- **Optional parameter (`?`)**: Must always come **after** required parameters.
- **Default parameter**: Provides a fallback value if none is passed.

```typescript
// Optional parameter greeting? / message?
function greetUser(name: string, greeting?: string): string {
    return greeting ? `${greeting}, ${name}!` : `Hello, ${name}!`;
}

console.log(greetUser("Peter"));          // Output: "Hello, Peter!"
console.log(greetUser("Peter", "Welcome")); // Output: "Welcome, Peter!"

// Default parameter role = "User" / multiplier = 2
function assignRole(username: string, role: string = "User"): string {
    return `${username} is assigned to role: ${role}`;
}

console.log(assignRole("John"));               // Output: "John is assigned to role: User"
console.log(assignRole("AdminUser", "Admin")); // Output: "AdminUser is assigned to role: Admin"

function multiplyBy(a: number, multiplier: number = 2): number {
    return a * multiplier;
}

console.log(multiplyBy(5));    // Output: 10
console.log(multiplyBy(5, 3)); // Output: 15
```

### Rest Parameters

When a function accepts an unknown number of arguments, type the rest parameter as an array:

```typescript
function sumAllNumbers(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sumAllNumbers(10, 20, 30, 40)); // Output: 100
```

### Callback Functions & `void` in Callbacks

```typescript
function execute(callback: (value: number) => void): void {
    callback(100);
}

execute((value) => {
    console.log(value); // Output: 100
});
```

> [!NOTE]
> In callback typing (`callback: () => void`), `void` means the calling function (`execute`) **will not use** the callback's return value. It does not forbid the callback from returning something; it simply means whatever is returned will be ignored.

---

## Type Narrowing & Type Guards

When working with union types (like `string | number` or `unknown`), TypeScript requires you to narrow down the broad type into a specific one before calling type-specific methods.

### 1. Narrowing Using `typeof`

Used for primitive types (`"string"`, `"number"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"`, `"function"`):

```typescript
function printValue(value: string | number): void {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else {
        console.log(value.toFixed(2));
    }
}

printValue("hello"); // Output: "HELLO"
printValue(25);      // Output: "25.00"
```

### 2. Narrowing Using Truthiness Checks

Checks whether a value is present (`truthy`) or absent (`null`/`undefined`):

```typescript
function printName(name: string | null): void {
    if (name) {
        console.log(name.toUpperCase());
    } else {
        console.log("No name provided");
    }
}

function printAge(age: number | null): void {
    if (age !== null) {
        console.log(age);
    }
}
```

### 3. Narrowing Using Equality Checks (`===`, `!==`)

```typescript
function displayStatus(value: string | number | null): void {
    if (value === null) {
        console.log("Value is missing");
    } else if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else {
        console.log(value.toFixed(2));
    }
}
```

### 4. Narrowing Using `Array.isArray()`

Distinguishes between arrays and non-arrays:

```typescript
function processItems(value: string | number[]): void {
    if (Array.isArray(value)) {
        console.log(`Array length: ${value.length}`);
    } else {
        console.log(`String value: ${value.toUpperCase()}`);
    }
}

processItems([10, 20, 30]); // Output: "Array length: 3"
processItems("hello");      // Output: "String value: HELLO"
```

### 5. Narrowing Using the `in` Operator

Checks whether a specific property exists on an object:

```typescript
type Admin = { name: string; permissions: string[] };
type Customer = { name: string; loyaltyPoints: number };

function showUserDetails(user: Admin | Customer): void {
    if ("permissions" in user) {
        console.log(`Admin permissions: ${user.permissions.join(", ")}`);
    } else {
        console.log(`Customer loyalty points: ${user.loyaltyPoints}`);
    }
}
```

### 6. Narrowing Using `instanceof`

Checks whether an object was created by a specific class or constructor function:

```typescript
function formatTimestamp(value: Date | string): string {
    if (value instanceof Date) {
        return value.toISOString();
    } else {
        return new Date(value).toISOString();
    }
}
```

### 7. Custom Type Guards Using `is` (Type Predicates)

A custom type guard is a function that returns a **type predicate** in the form `parameter is Type`. When the function returns `true`, TypeScript narrows the variable inside the conditional block:

```typescript
// 1. Primitive custom type guard
function isString(value: unknown): value is string {
    return typeof value === "string";
}

function printCustomValue(value: unknown): void {
    if (isString(value)) {
        console.log(value.toUpperCase()); // TypeScript knows value is a string!
    }
}

// 2. Object custom type guard
interface UserAccount {
    name: string;
    age: number;
}

function isUser(value: unknown): value is UserAccount {
    return (
        typeof value === "object" &&
        value !== null &&
        "name" in value &&
        "age" in value &&
        typeof (value as UserAccount).name === "string" &&
        typeof (value as UserAccount).age === "number"
    );
}

function printUser(value: unknown): void {
    if (isUser(value)) {
        console.log(`User: ${value.name}, Age: ${value.age}`);
    } else {
        console.log("Invalid user object");
    }
}

printUser({ name: "Peter", age: 25 }); // Output: "User: Peter, Age: 25"
printUser({ name: "Peter", age: "25" }); // Output: "Invalid user object"
```

### Narrowing Methods Summary Table

| Narrowing Method | How It Works | Example |
| :--- | :--- | :--- |
| **`typeof`** | Checks primitive types (`string`, `number`, `boolean`, etc.) | `if (typeof x === "string")` |
| **Truthiness** | Checks whether a value is not `null`, `undefined`, `""`, or `0` | `if (name) { ... }` |
| **Equality** | Uses comparisons like `===` or `!==` | `if (value === null)` |
| **`Array.isArray()`** | Checks whether a value is an array | `if (Array.isArray(x))` |
| **`in` operator** | Checks whether an object contains a property key | `if ("permissions" in user)` |
| **`instanceof`** | Checks if an object is an instance of a class/constructor | `if (val instanceof Date)` |
| **Custom type guard** | Uses a function returning `parameter is Type` | `function isUser(x): x is User` |
| **Discriminated union** | Uses a shared literal property to distinguish variants | `if (res.status === "success")` |

---

## Discriminated Unions (Tagged Unions)

A **discriminated union** is a pattern where every object variant in a union shares a common literal property (called a **tag** or **discriminant**). It is the cleanest way to handle state machines, Redux actions, or API responses.

### 1. Payment Methods Example

```typescript
type CashPayment = {
    method: "cash";
    amount: number;
    cashReceived: number;
};

type CardPayment = {
    method: "card";
    amount: number;
    cardNumber: string;
};

type Payment = CashPayment | CardPayment;

function processPaymentMethod(payment: Payment): void {
    if (payment.method === "cash") {
        console.log(`Cash received: $${payment.cashReceived}`);
    } else {
        console.log(`Card charged: ****-****-****-${payment.cardNumber.slice(-4)}`);
    }
}

processPaymentMethod({ method: "cash", amount: 500, cashReceived: 600 });
processPaymentMethod({ method: "card", amount: 1000, cardNumber: "1234567890123456" });
```

### 2. Multi-State Orders with `switch`

```typescript
type Order =
    | { status: "placed"; orderId: number }
    | { status: "shipped"; orderId: number; trackingId: string }
    | { status: "delivered"; orderId: number; deliveredAt: string };

function trackOrder(order: Order): void {
    switch (order.status) {
        case "placed":
            console.log(`Order ${order.orderId} placed.`);
            break;
        case "shipped":
            console.log(`Tracking ID: ${order.trackingId}`);
            break;
        case "delivered":
            console.log(`Delivered at: ${order.deliveredAt}`);
            break;
    }
}
```

### 3. API Response States

```typescript
type ApiResponse =
    | { status: "loading" }
    | { status: "success"; data: string[] }
    | { status: "error"; errorMessage: string };

function renderUI(response: ApiResponse): string {
    switch (response.status) {
        case "loading":
            return "Loading...";
        case "success":
            return `Data: ${response.data.join(", ")}`;
        case "error":
            return `Error: ${response.errorMessage}`;
    }
}
```

---

## Type Assertions (`as`) & Non-Null Assertion (`!`)

### Type Assertions (`as`)

A **type assertion** tells TypeScript: *"I know more about this value's type than you do."*

- Type assertion means telling TypeScript what type you believe a value has.

```typescript
// TypeScript thinks getElementById returns HTMLElement | null
const searchInput = document.getElementById("search-box") as HTMLInputElement;

// Now accessing .value is completely valid:
console.log(searchInput.value);

let value: unknown = "Hello";

let result = (value as string).toUpperCase();

let result = (<string>value).toUpperCase();
```

### Non-Null Assertion (`!`)

Appending `!` asserts that a value is neither `null` nor `undefined`:

```typescript
const liveButton = document.getElementById("submit-btn")!;
liveButton.addEventListener("click", () => console.log("Clicked!"));
```

> [!CAUTION]
> Type assertions bypass compiler checks. If your assumption is incorrect at runtime, your application will crash.

---

## Generics (Reusable, Type-Safe Code)

Generics allow you to write reusable functions, interfaces, and types that work with multiple data types while preserving full type safety.

- Think of `<T>` as a **type variable / placeholder**. The actual type is provided when the function or interface is used.

### Generic Functions

```typescript
function getValue<T>(value: T): T {
    return value;
}

// Explicit type arguments:
console.log(getValue<number>(100));     // Output: 100
console.log(getValue<string>("Hello")); // Output: "Hello"

// TypeScript can infer T automatically:
const num = getValue(50);      // Inferred as number
const str = getValue("Peter"); // Inferred as string
```

### Generic with Arrays

```typescript
function getFirst<T>(items: T[]): T {
    return items[0];
}

const firstNumber = getFirst([10, 20, 30]);          // Returns number (10)
const firstName = getFirst(["Peter", "John", "Sam"]); // Returns string ("Peter")
```

### Generics with Multiple Type Parameters

```typescript
function pair<T, U>(first: T, second: U): [T, U] {
    return [first, second];
}

const result = pair("Peter", 25); // Result type: [string, number]
console.log(result); // Output: ["Peter", 25]
```

### Generic Interfaces & Type Aliases

```typescript
// Generic Interface
interface Box<T> {
    value: T;
}

const numberBox: Box<number> = { value: 20 };
const stringBox: Box<string> = { value: "Hello" };

interface Book<T, U> {
    name: T;
    id: U;
}

const book: Book<string, number> = {
    name: "Harry Potter",
    id: 101,
};

// Real-world API Response wrapper:
interface ApiResponseWrapper<T> {
    statusCode: number;
    success: boolean;
    data: T;
}

type UserData = { id: number; name: string };
type ProductData = { id: number; title: string; price: number };

const userResponse: ApiResponseWrapper<UserData> = {
    statusCode: 200,
    success: true,
    data: { id: 1, name: "Peter" },
};

const productResponse: ApiResponseWrapper<ProductData> = {
    statusCode: 200,
    success: true,
    data: { id: 101, title: "Laptop", price: 65000 },
};

// Generic Type Alias with Arrow Functions
type GetFirstFn<T> = (items: T[]) => T;

const getFirstItem: GetFirstFn<number> = (items) => items[0];
console.log(getFirstItem([1, 2, 3])); // Output: 1
```

### Generic Constraints (`extends`)

#### Why Do We Need Constraints?

Consider a generic function without a constraint:

```typescript
function getLength<T>(value: T): number {
    // return value.length; // Error: Property 'length' does not exist on type 'T'
}
```

Why does TypeScript complain? Because `T` could be a `number` or a `boolean`, which do not have a `.length` property.

#### The Solution: Restrict `T` Using `extends`

```typescript
function getLength<T extends { length: number }>(value: T): number {
    return value.length;
}

console.log(getLength("Hello"));       // Output: 5 (string has .length)
console.log(getLength([10, 20, 30])); // Output: 3 (array has .length)
// getLength(100); // Error: Argument of type 'number' does not have 'length'!

function logLength<T extends { length: number }>(item: T): void {
    console.log(`Length: ${item.length}`);
}

logLength("Hello World"); // Valid: string has .length
logLength([1, 2, 3, 4]);  // Valid: array has .length
```

### Constraints with Object Properties & Interfaces

```typescript
interface Person {
    name: string;
}

function printName<T extends Person>(user: T): void {
    console.log(user.name);
}

printName({ name: "Peter" }); // Valid
printName({ name: "John", age: 25 }); // Valid: extra properties are permitted!

function showLength<T extends { length: number }>(value: T): void {
    console.log(value.length);
}

showLength({ length: 10, name: "Example" }); // Valid
```

> [!TIP]
> **Key Rule:** A generic constraint (`<T extends Person>`) represents the **minimum requirement** that the type must satisfy. It does not require exact equality; passing extra properties is completely valid.

---

## Built-in Utility Types

Utility types let you transform and reuse existing types without retyping everything from scratch.

### 1. `Partial<T>` (Make all properties optional)

```typescript
type User = {
    name: string;
    age: number;
    salary: number;
};

// All properties become optional: { name?: string; age?: number; salary?: number; }
function updateUser(user: Partial<User>): void {
    console.log(user);
}

updateUser({ name: "John" }); // Valid
updateUser({ age: 30 });      // Valid
updateUser({});               // Valid

interface UserProfile {
    id: number;
    name: string;
    email: string;
    age: number;
}

function updateUserProfile(id: number, updates: Partial<UserProfile>): void {
    console.log(`Updating user ${id} with:`, updates);
}

updateUserProfile(1, { email: "newemail@example.com" }); // Valid
```

### 2. `Required<T>` (Make all properties mandatory)

Opposite of `Partial`; removes all optional `?` markers:

```typescript
type Config = {
    host: string;
    port?: number;
};

const fullConfig: Required<Config> = {
    host: "localhost",
    port: 8080, // Now required!
};

type FullEmployee = {
    name: string;
    age: number;
    salary?: number;
};

const requiredEmployee: Required<FullEmployee> = {
    name: "John",
    age: 24,
    salary: 54000, // Mandatory
};
```

### 3. `Readonly<T>` (Make all properties immutable)

```typescript
interface Post {
    id: number;
    title: string;
}

const post: Readonly<Post> = {
    id: 1,
    title: "TypeScript Guide",
};

// post.title = "New Title"; // Error: Cannot assign to read-only property
```

### 4. `Pick<T, Keys>` (Select specific properties)

```typescript
interface FullUser {
    id: number;
    name: string;
    email: string;
    passwordHash: string;
}

// Picks only 'id' and 'name'
type UserPreview = Pick<FullUser, "id" | "name">;

const preview: UserPreview = {
    id: 1,
    name: "Peter",
};
```

- The | means either property name can be included in the set of keys to select.

### 5. `Omit<T, Keys>` (Exclude specific properties)

```typescript
// Excludes 'passwordHash'
type PublicUser = Omit<FullUser, "passwordHash">;

const publicUser: PublicUser = {
    id: 1,
    name: "Peter",
    email: "peter@example.com",
};
```

### 6. `Record<Keys, ValueType>` (Construct key-value dictionary)

```typescript
// Arbitrary string keys with numeric values:
type Scores = Record<string, number>;

const userScores: Scores = {
    Peter: 90,
    John: 85,
    Sam: 95,
};

// Restricting keys to specific union members:
type RoleNames = "admin" | "editor" | "guest";

interface RoleDetails {
    level: number;
    canDelete: boolean;
}

const rolePermissions: Record<RoleNames, RoleDetails> = {
    admin: { level: 1, canDelete: true },
    editor: { level: 2, canDelete: false },
    guest: { level: 3, canDelete: false },
};
```

### 7. `Exclude<T, U>` (Remove types from a union)

`Exclude<T, U>` strips away members from union `T` that are assignable to `U`:

```typescript
type StatusOptions = "pending" | "success" | "failed";

type ActiveOptions = Exclude<StatusOptions, "failed">; // "pending" | "success"

let currentStatus: ActiveOptions = "pending";
// currentStatus = "failed"; // Error: "failed" has been excluded!

type PrimitiveOptions = Exclude<string | number | boolean, string>; // number | boolean
```

> [!NOTE]
> `Exclude` works on **unions**, not object properties. If you want to omit a property from an object, use `Omit<T, Keys>`.

### 8. `Extract<T, U>` (Keep matching types from a union)

`Extract<T, U>` does the opposite of `Exclude`: it keeps only the members of union `T` that match `U`:

```typescript
type FileStatus = "pending" | "success" | "failed";

type FinishedStatus = Extract<FileStatus, "success" | "failed">; // "success" | "failed"

let fileUpload: FinishedStatus = "success"; // Valid
// fileUpload = "pending"; // Error: "pending" was not extracted!
```

### 9. `NonNullable<T>` (Remove `null` and `undefined`)

Removes `null` and `undefined` from a union type:

```typescript
type NullableString = string | null | undefined;

type CleanString = NonNullable<NullableString>; // string

let validName: CleanString = "Peter"; // Valid
// validName = null;      // Error: null is not assignable!
// validName = undefined; // Error: undefined is not assignable!
```

---

## The `keyof` Type Operator

The `keyof` operator produces a string or numeric literal union of all known public property names (keys) of an object type.

```typescript
type UserAccount = {
    name: string;
    age: number;
};

// Equivalent to: type UserKeys = "name" | "age"
type UserKeys = keyof UserAccount;

let allowedKey: UserKeys = "name"; // Valid
allowedKey = "age";               // Valid
// allowedKey = "email";          // Error: Type '"email"' is not assignable to 'UserKeys'
```

### Safely Accessing Object Properties with `keyof`

```typescript
function getObjectValue(user: UserAccount, key: keyof UserAccount) {
    return user[key];
}

const sampleUser: UserAccount = { name: "Peter", age: 25 };
console.log(getObjectValue(sampleUser, "name")); // Output: "Peter"
console.log(getObjectValue(sampleUser, "age"));  // Output: 25
```

### Combining `keyof` with Generics

Using `K extends keyof T` guarantees that `key` is a valid property on object `T`, and returns `T[K]` with exact type inference:

```typescript
function getProperty<T, K extends keyof T>(object: T, key: K): T[K] {
    return object[key];
}

const developer = {
    name: "Peter",
    age: 25,
    isAvailable: true,
};

const devName = getProperty(developer, "name"); // Inferred type: string ("Peter")
const devAge = getProperty(developer, "age");   // Inferred type: number (25)

// getProperty(developer, "salary"); // Error: Argument of type '"salary"' is not assignable!
```

---

## The `typeof` Type Operator (Value-to-Type)

In JavaScript, `typeof` evaluates a value at runtime and returns a string (`"string"`, `"number"`, etc.).

In TypeScript, **`typeof` in a type position** extracts the static type of an existing variable, constant, or object:

```typescript
const appConfig = {
    appName: "CraveGo",
    maxItems: 10,
    isActive: true,
};

// Extracts the type shape directly from the variable:
type AppConfig = typeof appConfig;
/*
Equivalent to:
type AppConfig = {
    appName: string;
    maxItems: number;
    isActive: boolean;
};
*/

const clientConfig: AppConfig = {
    appName: "MyApp",
    maxItems: 20,
    isActive: false,
};
```

### Combining `typeof` with `keyof`

You can combine `keyof typeof` to extract the keys of an existing JavaScript object without declaring a separate interface first:

```typescript
const colors = {
    primary: "#007bff",
    secondary: "#6c757d",
    danger: "#dc3545",
};

// type ColorKeys = "primary" | "secondary" | "danger"
type ColorKeys = keyof typeof colors;

let myColor: ColorKeys = "primary"; // Valid
// myColor = "accent"; // Error: Not a valid key!
```

### `typeof` with `as const` (Literal Types from Objects)

Without `as const`, properties are inferred as generic types like `string` or `number`. Adding `as const` turns every property into an immutable literal type:

```typescript
const routes = {
    home: "/",
    dashboard: "/dashboard",
    settings: "/settings",
} as const;

// type RouteKeys = "home" | "dashboard" | "settings"
type RouteKeys = keyof typeof routes;

// type RouteValues = "/" | "/dashboard" | "/settings"
type RouteValues = typeof routes[RouteKeys];
```

---

## Indexed Access Types (Lookup Types)

Indexed access types let you look up the specific type of a property on another type using bracket notation (`Type["property"]`), just like accessing object values in JavaScript.

```typescript
type Member = {
    name: string;
    age: number;
    isActive: boolean;
};

type MemberName = Member["name"];     // string
type MemberAge = Member["age"];       // number
type MemberStatus = Member["isActive"]; // boolean

// Looking up multiple properties creates a union:
type NameOrAge = Member["name" | "age"]; // string | number

// Looking up all values dynamically:
type MemberAllValues = Member[keyof Member]; // string | number | boolean
```

### Indexed Access with Arrays & Tuples

```typescript
type ColorsTuple = ["red", "green", "blue"];

type FirstColor = ColorsTuple[0]; // "red"

// Using [number] extracts the union of all elements in the array/tuple:
type AnyColor = ColorsTuple[number]; // "red" | "green" | "blue"
```

---

## Conditional Types

A **conditional type** chooses between two possible types based on a condition, following the ternary operator syntax:

```typescript
type Result = SomeType extends TargetType ? TrueType : FalseType;
```

```typescript
type CheckType<T> = T extends string ? "Text" : "Non-Text";

type A = CheckType<string>;  // "Text"
type B = CheckType<number>;  // "Non-Text"
type C = CheckType<boolean>; // "Non-Text"
```

### Generic Constraint vs Conditional Type

| Concept | Syntax | Purpose |
| :--- | :--- | :--- |
| **Generic Constraint** | `<T extends string>` | **Restricts** which types are accepted into a function or type |
| **Conditional Type** | `T extends string ? A : B` | **Selects** a type depending on whether `T` satisfies the condition |

### Distributive Conditional Types (With Unions)

When a conditional type operates on a generic parameter `T` that receives a union, TypeScript automatically **distributes** the check across every member of the union:

```typescript
type ToArray<T> = T extends any ? T[] : never;

// Distributes to: (string[]) | (number[])
type StrOrNumArray = ToArray<string | number>;
```

---

## The `infer` Keyword (Type Extraction)

The `infer` keyword is used inside conditional types to **declare and extract a type on the fly** from within another type.

Think of `infer` as saying: *"TypeScript, deduce this inner type and bind it to a variable name for me."*

```typescript
type Example<T> = T extends SomeStructure<infer U> ? U : Fallback;
```

### 1. Extracting Array Element Types

```typescript
type GetArrayElement<T> = T extends (infer ElementType)[] ? ElementType : T;

type Item1 = GetArrayElement<string[]>;  // string
type Item2 = GetArrayElement<number[]>;  // number
type Item3 = GetArrayElement<boolean>;   // boolean (not an array, returns fallback T)
```

### 2. Extracting Function Return Types (How `ReturnType<T>` Works)

```typescript
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function getUserAge() {
    return 25;
}

function getUserName() {
    return "Peter";
}

type AgeReturnType = MyReturnType<typeof getUserAge>;   // number
type NameReturnType = MyReturnType<typeof getUserName>; // string
```

### 3. Extracting Property Types from Objects

```typescript
type GetNameField<T> = T extends { name: infer N } ? N : never;

type Developer = { name: "Peter"; level: "Senior" };

type ExtractedName = GetNameField<Developer>; // "Peter"
```

---

## Mapped Types

A **mapped type** iterates over the keys of an existing type using the `in` keyword and transforms each property into a new shape.

```typescript
type UserShape = {
    name: string;
    age: number;
};

// Syntax: [Key in keyof Type]: NewType
type UserFlags = {
    [Key in keyof UserShape]: boolean;
};
/*
Result:
type UserFlags = {
    name: boolean;
    age: boolean;
};
*/
```

### How the Syntax Works

`[Key in keyof UserShape]: boolean`
1. `keyof UserShape` gets the union of keys: `"name" | "age"`.
2. `Key in ...` loops through each key one by one.
3. `: boolean` specifies the new type for every property.

### Replicating Existing Property Types with `T[Key]`

```typescript
type CopyType<T> = {
    [Key in keyof T]: T[Key];
};

type CopiedUser = CopyType<UserShape>; // Exactly matches UserShape
```

### Adding & Removing Modifiers (`?` and `readonly`)

You can add or strip away modifiers like `?` (optional) and `readonly` using `+` (default) or `-`:

```typescript
type BaseItem = {
    readonly id: number;
    title: string;
    description?: string;
};

// 1. Make all properties optional (+?)
type MakeOptional<T> = {
    [K in keyof T]?: T[K];
};

// 2. Make all properties mandatory by removing optional (-?)
type MakeRequired<T> = {
    [K in keyof T]-?: T[K];
};

// 3. Make all properties editable by removing readonly (-readonly)
type MakeMutable<T> = {
    -readonly [K in keyof T]: T[K];
};
```

---

## TypeScript Declaration Files (`.d.ts`)

A declaration file (`.d.ts`) describes the **types and shapes** of JavaScript libraries, modules, or global variables **without providing any executable runtime code**.

- It tells TypeScript what functions, classes, and variables exist in external JavaScript files.
- Uses the `declare` keyword to define ambient variables and functions.

```typescript
// math.d.ts
export declare function add(a: number, b: number): number;
export declare function subtract(a: number, b: number): number;

// globals.d.ts
declare const APP_VERSION: string;
declare function trackAnalytics(event: string): void;
```

When you install npm packages that include types (or `@types/...`), you are consuming `.d.ts` declaration files.

---

## `tsconfig.json` & TypeScript Compilation

The `tsconfig.json` file is the configuration file for a TypeScript project. It tells the compiler how to inspect your code, which files to include/exclude, and how to compile TypeScript into JavaScript.

### Creating a `tsconfig.json`

Run the following command in your terminal to generate a default configuration file:

```bash
tsc --init
```

### Essential `tsconfig.json` Options

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "module": "CommonJS",
        "rootDir": "./src",
        "outDir": "./dist",
        "strict": true,
        "esModuleInterop": true,
        "forceConsistentCasingInFileNames": true,
        "skipLibCheck": true
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "dist"]
}
```

- **`"target"`**: Specifies which JavaScript ECMAScript version to output (e.g., `"ES6"`, `"ES2020"`, `"ESNext"`).
- **`"module"`**: Sets the module system (e.g., `"CommonJS"` for Node.js, `"ESNext"` for modern bundlers).
- **`"rootDir"`**: Specifies the root folder where your TypeScript source code lives.
- **`"outDir"`**: Specifies the output directory where compiled `.js` files are placed.
- **`"strict"`**: Enables a broad suite of strict type-checking flags (e.g., `noImplicitAny`, `strictNullChecks`).
- **`"include"` / `"exclude"`**: Controls which folders and files TypeScript should compile or ignore.

### Compiling Your Project

```bash
# Compiles the whole project using the local tsconfig.json:
tsc

# Checks for type errors without emitting any .js files (great for CI/CD pipelines):
tsc --noEmit

# Compiles using a specific configuration file:
tsc --project tsconfig.json

# Watches for file changes and recompiles automatically:
tsc --watch
```
