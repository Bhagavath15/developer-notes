# JavaScript Reference Guide

## Scope

Scope determines the accessibility and visibility of variables within different parts of your code. JavaScript has global scope, function scope, block scope, and lexical scope.

### Global Scope

Variables or functions declared outside any function or block are in the global scope and are accessible throughout your code.

```javascript
const appName = "Amazon";

function showAppName() {
    console.log(appName); // Output: "Amazon"
}

showAppName();
console.log(appName); // Output: "Amazon"
```

### Function Scope

Variables declared inside a function are local to that function body and cannot be accessed externally.

```javascript
function calculateTotal() {
    const price = 200;
    const quantity = 3;
    console.log(price * quantity); // Output: 600
}

calculateTotal();
// console.log(price); // Throws ReferenceError: price is not defined
```

### Block Scope

A block is code enclosed in curly braces `{ }`, such as inside an `if` statement or a loop. Variables declared with `let` and `const` are block-scoped, whereas `var` is function-scoped or globally scoped.

```javascript
if (true) {
    var a = 10;   // Function-scoped / Global
    let b = 20;   // Block-scoped
    const c = 30; // Block-scoped
}

console.log(a); // Output: 10
// console.log(b); // ReferenceError: b is not defined
// console.log(c); // ReferenceError: c is not defined
```

### Lexical Scope

Lexical scope means that where a function is physically written in the source code determines which surrounding variables that function can access. Inner functions have access to variables in their parent lexical environments.

```javascript
const name = "Peter";

function outer() {
    const place = "Chennai";

    function inner() {
        const age = 25;
        console.log(name);  // Output: "Peter"
        console.log(place); // Output: "Chennai"
        console.log(age);   // Output: 25
    }

    inner();
}

outer();
```

---

## Temporal Dead Zone (TDZ)

The Temporal Dead Zone (TDZ) is the period between entering a scope and the point where a variable declared with `let` or `const` is initialized. Accessing a variable during TDZ throws a `ReferenceError`.

```javascript
// console.log(price); // ReferenceError: Cannot access 'price' before initialization

let price = 200;

console.log(c); // Output: undefined (var is hoisted and initialized to undefined)
var c = 30;
```

---

## Hoisting

Hoisting is JavaScript's default behavior of moving variable and function declarations to the top of their containing scope during compilation. Variable declarations initialized with `var` are hoisted with a value of `undefined`.

```javascript
console.log(hoisting); // Output: undefined
var hoisting = 'hello';
```

Function declarations are fully hoisted with their definitions available:

```javascript
greet(); // Output: "Hello"

function greet() {
    console.log("Hello");
}
```

---

## Loops and Asynchronous Execution

Variable declarations inside loops affect how asynchronously scheduled tasks (like `setTimeout`) capture values across iterations.

### Using `var`

Because `var` is function-scoped and not block-scoped, a single shared variable `i` is mutated across loop iterations. By the time the callbacks execute, the loop has completed.

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i); // Output: 3, 3, 3
    }, 1000);
}
```

### Using `let`

Because `let` is block-scoped, a new binding for `i` is created for every loop iteration, preserving the expected value in each callback closure.

```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i); // Output: 0, 1, 2
    }, 1000);
}
```

---

## Shadowing

Variable shadowing occurs when a variable declared within an inner scope (such as a block or function) shares the same name as a variable in an outer scope, temporarily overriding access to the outer variable within that block.

```javascript
let name = "John";

{
    let name = "David"; // Shadows the outer 'name' variable inside this block
    console.log(name);  // Output: "David"
}

console.log(name);      // Output: "John"
```

---

## Primitive and Reference Types

### Primitive Types

Primitive data types represent immutable, single values stored directly in memory. JavaScript has 7 primitive types:

1. `string`
2. `number`
3. `bigint`
4. `boolean`
5. `undefined`
6. `null`
7. `symbol`

```javascript
const name = "John";     // string
const age = 25;          // number
const price = 100.5;     // number
const isActive = true;   // boolean
const value = undefined; // undefined
const data = null;       // null
const big = 123n;        // bigint
const id = Symbol("id"); // symbol
```

### Primitive Assignment

When copying a primitive value, JavaScript copies the actual value itself (copy-by-value). Modifying one variable does not affect another.

```javascript
let a = 10;
let b = a;

b = 20;

console.log(a); // Output: 10
console.log(b); // Output: 20
```

- Changing `b` does not affect `a`.
- The primitive value was copied independently.

---

## Reference / Object Types

A variable assigned to a reference type holds a memory address (reference) pointing to the object, not the object itself.

1. `Objects`
2. `Arrays`
3. `Functions`
4. `Dates`
5. `Maps`
6. `Sets`

```javascript
const user1 = {
    name: "John",
    age: 25
};
const user2 = user1; // 'user2' copies the memory reference of 'user1'

user2.name = 'peter';

console.log(user1.name); // Output: "peter"
console.log(user2.name); // Output: "peter"
```

- Both variables reference the exact same object in memory.
- JavaScript passes argument values as values, but for objects, that value is the reference itself.

```javascript
const greet = function () {
    console.log("Hello");
};
const map = new Map();
const set = new Set();
const date = new Date();
```

### Difference Between Primitive and Reference Types

- **Primitives:** Assigned and copied by value.
- **Reference Types:** Assigned and copied by reference pointer.

### Reference Equality

```javascript
const a = { name: "John" };
const b = { name: "John" };
const c = a;

console.log(a === b); // Output: false (two distinct memory references)
console.log(a === c); // Output: true  (points to the exact same reference)
```

---

## Equality Comparison: `==` vs `===`

Strict equality checks ensure both value and reference identity.

```javascript
console.log(10 === 10); // Output: true
```

- Primitive values match both type and value.

```javascript
console.log({} === {}); // Output: false
```

- Two distinct object literal references are never strictly equal, even if their structure is identical.

```javascript
const user1 = {
    name: "John",
    age: 25
};
const user2 = user1;

console.log(user1 === user2); // Output: true
```

- Evaluates to `true` because both variables point to the exact same reference location.

> **Note:** `null` is a primitive type, but `typeof null` returns `"object"` due to a historical JavaScript bug.

---

## Copying Objects

### Reference Assignment

```javascript
const user = {
    name: "John",
    address: {
        city: "Chennai"
    }
};

const copy = user;
```

- Modifying `copy` mutates `user` because no duplicate object was created.

### Shallow Copy

A shallow copy duplicates top-level properties. However, nested object properties remain shared references. Shallow copies can be created using `{ ...obj }`, `Object.assign()`, `[...arr]`, or `slice()`.

```javascript
const user = {
    name: "John",
    address: {
        city: "Chennai"
    }
};

const copy = { ...user };
copy.name = "David";

console.log(user.name); // Output: "John"
```

#### Nested Object Behavior in Shallow Copy

```javascript
const user = {
    name: "John",
    address: {
        city: "Chennai"
    }
};

const copy = { ...user };

copy.address.city = "Bangalore";
console.log(user.address.city); // Output: "Bangalore"
```

- Spread operator (`...`) creates a shallow duplicate. Top-level primitive properties are separated, but nested levels remain linked by reference.

### Deep Copy

A deep copy recursively duplicates every level of an object structure, breaking all shared memory references.

```javascript
const user = {
    name: "John",
    address: {
        city: "Chennai"
    }
};

const copy = structuredClone(user);
copy.address.city = "Bangalore";

console.log(user.address.city); // Output: "Chennai"
```

---

## Type Coercion

Type coercion is the automatic or manual conversion of values from one data type to another.

### Implicit Coercion

JavaScript automatically converts values behind the scenes based on context:

```javascript
console.log("5" + 2);    // Output: "52" (String concatenation)
console.log("5" - 2);    // Output: 3    (Numeric subtraction)
console.log("5" * 2);    // Output: 10   (Numeric multiplication)
console.log("5" / 2);    // Output: 2.5  (Numeric division)
console.log(5 + "2");    // Output: "52" (String concatenation)
console.log(5 - "2");    // Output: 3    (Numeric subtraction)
console.log(true + 1);   // Output: 2    (true becomes 1)
console.log(false + 1);  // Output: 1    (false becomes 0)
```

### Explicit Coercion

Explicit coercion occurs when type conversions are written manually using built-in constructors or functions:

```javascript
// String coercion
console.log(String(123));      // "123"
console.log(String(true));     // "true"
// Number coercion
console.log(Number("123"));    // 123
console.log(Number("10.5"));   // 10.5
console.log(Number("hello"));  // NaN
// Boolean coercion
console.log(Boolean(1));       // true
console.log(Boolean(0));       // false
console.log(Boolean("hello")); // true
console.log(Boolean(""));      // false
```

---

## Abstract Equality (`==`) vs Strict Equality (`===`)

- `==` (Loose Equality): Performs implicit type coercion before comparing values.
- `===` (Strict Equality): Compares both value and data type without performing coercion.

```javascript
console.log(10 == "10");     // Output: true  (coerces string "10" to number 10)
console.log(10 === "10");    // Output: false (compares number vs string)

console.log(true == 1);      // Output: true  (coerces true to 1)
console.log(true === 1);     // Output: false (boolean vs number)

console.log(false == 0);     // Output: true  (coerces false to 0)
console.log(false === 0);    // Output: false (boolean vs number)

console.log(null == undefined);  // Output: true  (special rule in JS specification)
console.log(null === undefined); // Output: false (null vs undefined)

console.log(null == 0);      // Output: false
console.log(null === 0);     // Output: false

console.log("" == false);    // Output: true  (coerces "" and false to 0)
console.log("" === false);   // Output: false (string vs boolean)

const userId = "100";
const selectedId = 100;
console.log(userId == selectedId);  // Output: true
console.log(userId === selectedId); // Output: false

console.log([] == false);    // Output: true  ([] converts to "" then to 0)
console.log([] === false);   // Output: false (object/array vs boolean)

console.log([1] == 1);       // Output: true  ([1] converts to "1" then 1)
console.log([1] === 1);      // Output: false (object/array vs number)

console.log("0" == false);   // Output: true  ("0" converts to 0)
console.log("0" === false);  // Output: false (string vs boolean)

console.log(" " == 0);       // Output: true  (" " converts to 0)
console.log(" " === 0);      // Output: false (string vs number)
```

---

## Truthy and Falsy Values

### Falsy Values

Values that evaluate to `false` when converted to a boolean:

- `false`
- `0`
- `-0`
- `0n`
- `""` (empty string)
- `null`
- `undefined`
- `NaN`

```javascript
console.log(Boolean(0));         // Output: false
console.log(Boolean("0"));       // Output: true
console.log(Boolean(""));        // Output: false
console.log(Boolean(" "));       // Output: true
console.log(Boolean(null));      // Output: false
console.log(Boolean(undefined)); // Output: false
console.log(Boolean([]));        // Output: true
console.log(Boolean({}));        // Output: true
console.log(Boolean(NaN));       // Output: false
```

### Logical NOT Operator (`!`)

Converts a value to its opposite boolean equivalent.

```javascript
console.log(!"hello"); // Output: false
console.log(!0);       // Output: true
console.log(!null);    // Output: true
console.log(![]);      // Output: false
```

### Double NOT Operator (`!!`)

Converts a value explicitly to its corresponding boolean evaluation.

```javascript
console.log(!!"hello"); // Output: true
console.log(!!0);       // Output: false
console.log(!![]);      // Output: true
console.log(!!null);    // Output: false
```

---

## Logical Short-Circuit Operators and Nullish Coalescing

### Logical OR (`||`)

Evaluates operands from left to right and returns the first **truthy** operand. If all are falsy, returns the last operand.

```javascript
const name = "";
const result = name || "Guest";
console.log(result); // Output: "Guest"
```

```javascript
const count = 0;
const result = count || 10;
console.log(result); // Output: 10 (because 0 is falsy)
```

### Nullish Coalescing (`??`)

Evaluates operands from left to right and returns the right-hand operand only if the left-hand operand is `null` or `undefined`.

```javascript
const count = 0;
console.log(count ?? 10); // Output: 0
```

### Logical OR (`||`) vs Nullish Coalescing (`??`)

- `||`: Checks for truthiness (rejects `0`, `""`, `false`, `null`, `undefined`, `NaN`).
- `??`: Checks specifically for `null` or `undefined`.

### Logical AND (`&&`)

Evaluates operands from left to right and returns the first **falsy** operand. If all operands are truthy, returns the last operand.

```javascript
const user = true;
user && console.log("Logged in"); // Output: "Logged in"
```

#### Behavior Summary

- `truthyValue && operand` $\rightarrow$ `operand`
- `falsyValue && operand` $\rightarrow$ `falsyValue`

```javascript
console.log(true && "Hello");  // Output: "Hello"
console.log(false && "Hello"); // Output: false
```

---

## Operators

### Arithmetic Operators

Used to perform mathematical calculations: `+`, `-`, `*`, `/`, `%`, `**`.

#### Postfix Increment / Decrement

Returns the original value before performing the increment or decrement operation.

```javascript
let a = 10;
console.log(a++); // Output: 10
console.log(a);   // Output: 11

let b = 10;
console.log(b--); // Output: 10
console.log(b);   // Output: 9
```

#### Prefix Increment / Decrement

Increments or decrements the variable value first, then returns the updated value.

```javascript
let a = 10;
console.log(++a); // Output: 11
console.log(a);   // Output: 11

let b = 10;
console.log(--b); // Output: 9
console.log(b);   // Output: 9
```

### Assignment & Compound Assignment Operators

Assign values to variables (`=`) or perform an arithmetic operation and assignment simultaneously (`+=`, `-=`, `*=`, `/=`).

```javascript
let score = 10;
score += 5; // Equivalent to: score = score + 5
console.log(score); // Output: 15
```

### Comparison Operators

Used to evaluate relationships: `<`, `>`, `<=`, `>=`.

### Equality Operators

Used to check value and type conditions: `==`, `===`.

### Logical Operators

Used to combine logical expressions: `&&`, `||`, `!`.

### Ternary Operator

A shorthand conditional expression.

**Syntax:** `condition ? valueIfTrue : valueIfFalse`

### `typeof` Operator

Returns a string indicating the evaluation type of an operand.

### `in` Operator

Checks whether a property key exists in an object or its prototype chain.

```javascript
const user = {
    name: "John",
    age: 25
};

console.log("name" in user);  // Output: true
console.log("email" in user); // Output: false
```

### `delete` Operator

Removes a property key and its value from an object.

```javascript
const user = {
    name: "John",
    age: 25
};

delete user.age;

console.log(user); // Output: { name: "John" }
```

### Optional Chaining (`?.`)

Reads nested properties safely without causing an error if an intermediate reference is `null` or `undefined`.

```javascript
const user = {
    profile: {
        name: "John"
    }
};

console.log(user.profile?.name); // Output: "John"
console.log(user.profile?.age);  // Output: undefined
```

### Operator Precedence

Simplified order of operations (highest priority to lowest):

1. Grouping: `()`
2. Exponentiation: `**`
3. Multiplication / Division / Modulo: `*`, `/`, `%`
4. Addition / Subtraction: `+`, `-`
5. Relational Comparisons: `<`, `<=`, `>`, `>=`
6. Logical Operators: `&&`, `||`, `??`
7. Conditional Operator: `?:`
8. Assignment: `=`

---

## Destructuring

Destructuring syntax extracts values from arrays or properties from objects into distinct variables.

### Object Destructuring

Object properties are matched by key name; variable order does not matter.

```javascript
const user = {
    name: "John",
    age: 25,
    city: "Chennai"
};

const { age = 18, city, name } = user;

console.log(city); // Output: "Chennai"
console.log(name); // Output: "John"

// Renaming variables during destructuring
const { name: userName, age: userAge } = user;

console.log(userName); // Output: "John"
console.log(userAge);  // Output: 25
```

- Destructuring extracts properties into independent variables without mutating or cloning the original object.
- **Renaming Syntax:** `const { keyName: newVariableName } = object;`

### Nested Object Destructuring

```javascript
const user = {
    name: "John",
    address: {
        city: "Chennai",
        country: "India"
    }
};

const { name, address: { city, country } } = user;

console.log(name);    // Output: "John"
console.log(city);    // Output: "Chennai"
console.log(country); // Output: "India"
```

### Rest Property in Destructuring

Collects remaining unextracted object properties into a separate target object.

```javascript
const user = {
    name: "John",
    age: 25,
    city: "Chennai",
    country: "India"
};

const { name, ...rest } = user;

console.log(name); // Output: "John"
console.log(rest); // Output: { age: 25, city: "Chennai", country: "India" }
```

### Array Destructuring

Array items are assigned by index position.

```javascript
const fruits = ["apple", "banana", "orange"];
const [first, second, third] = fruits;

console.log(first);  // Output: "apple"
console.log(second); // Output: "banana"
console.log(third);  // Output: "orange"

// Skipping values using commas and supplying default values
const [a, , b, c = 20] = fruits;

console.log(a); // Output: "apple"
console.log(b); // Output: "orange"
console.log(c); // Output: 20

// Rest elements in array destructuring
const [d, ...rest] = fruits;
console.log(d);    // Output: "apple"
console.log(rest); // Output: ["banana", "orange"]
```

#### Swapping Variables Without Temporary Storage

```javascript
let a = 10;
let b = 20;

[a, b] = [b, a];

console.log(a); // Output: 20
console.log(b); // Output: 10
```

### Function Parameters Destructuring

```javascript
function displayUser({ name, age }) {
    console.log(name);
    console.log(age);
}

displayUser({
    name: "John",
    age: 25
});
```

```javascript
function printNumbers([first, second]) {
    console.log(first);
    console.log(second);
}

printNumbers([10, 20]);
```

---

## Spread Operator

The spread operator (`...`) unpacks elements of an array or properties of an object into individual components.

```javascript
const fruits = ['apple', 'banana'];
const newFruits = [...fruits]; // Shallow copy of array
console.log(newFruits);

const user = {
    name: "John",
    age: 25
};

const copy = { ...user }; // Shallow copy of object
console.log(copy);
```

- When spreading objects with duplicate keys, subsequent keys override earlier values.

---

## Rest Operator

The rest parameter syntax (`...`) collects multiple loose arguments or elements into a single array structure. It must always be placed last in a parameter list or pattern.

```javascript
const numbers = [10, 20, 30, 40];
const [first, ...rest] = numbers;

const user = {
    name: "John",
    age: 25,
    city: "Chennai"
};

const { name, ...details } = user;

console.log(name);    // Output: "John"
console.log(details); // Output: { age: 25, city: "Chennai" }
```

```javascript
function sum(...numbers) {
    return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(10, 20, 30)); // Output: 60
```

```javascript
function display(first, second, ...rest) {
    console.log(first);  // Output: 10
    console.log(second); // Output: 20
    console.log(rest);   // Output: [30, 40, 50]
}

display(10, 20, 30, 40, 50);
```

---

## Spread vs Rest

- **Spread:** Expands elements or properties out of an iterable/object.
- **Rest:** Bundles multiple individual elements or parameters together into a single structure.

### Spread Example

```javascript
const numbers = [10, 20, 30];
console.log(...numbers); // Output: 10 20 30
```

### Rest Example

```javascript
const [first, ...rest] = numbers; // 'rest' becomes [20, 30]
```

---

## Template Literals

Template literals are string literals delimited by backtick (`` ` ``) characters, providing support for string interpolation and embedded multiline expressions.

**Syntax:** `` `Hello ${variable}` ``

---

## Functions

### Function Declarations

A function declaration defines a reusable block of code that performs specific tasks.

```javascript
// Function Definition
function greet() {
    console.log('Hello World!');
}

// Function Execution
greet(); // Output: "Hello World!"
```

### Parameters and Arguments

- **Parameter:** The named variable defined inside the function declaration signature.
- **Argument:** The concrete value passed to the function when invoked.

```javascript
function greet(name) { // 'name' is the parameter
    console.log(name);
}

greet("John"); // "John" is the argument
```

### Return Statement

```javascript
function add(a, b) {
    return a + b;
}

const result = add(10, 20);
console.log(result); // Output: 30
```

- Calculates and passes the result back to the calling context.
- Execution terminates immediately once JavaScript encounters a `return` statement.

```javascript
function test() {
    console.log("A");
    return;
    console.log("B"); // Unreachable code
}

test(); 
// Output: "A"
```

### Default Parameters

Assigns fallback values to function parameters if arguments are omitted or passed as `undefined`.

```javascript
function greet(name = "Guest") {
    return `Hello ${name}`;
}

console.log(greet("John"));    // Output: "Hello John"
console.log(greet());          // Output: "Hello Guest"
console.log(greet(undefined)); // Output: "Hello Guest"
console.log(greet(null));      // Output: "Hello null" (null is treated as an explicit value)
```

### Outer Scope Access

Functions have access to variables declared in their parent or outer lexical scopes.

```javascript
const name = 'John';

function greet() {
    return `Hello ${name}`;
}

console.log(greet()); // Output: "Hello John"
```

Locally declared variables shadow identically named variables in the outer scope:

```javascript
const name = "John";

function greet() {
    const name = "David";
    console.log(name);
}

greet(); // Output: "David"
```

### Functions Returning Functions

In JavaScript, functions are first-class values and can be returned from other functions.

```javascript
function greet() {
    return function () {
        console.log("Hello World!");
    };
}

const greetUser = greet();
greetUser(); // Output: "Hello World!"
```

### First-Class Functions

Functions can be assigned to variables, passed as arguments, or returned from other functions.

```javascript
function greet() {
    console.log("Hello World!");
}

const greetFn = greet;
greetFn(); // Output: "Hello World!"
```

---

## Function Expressions

A function expression creates a function assigned directly to a variable. Variable declarations with expressions are hoisted, but their assignment values are not initialized until evaluation.

```javascript
const greet = function () {
    console.log("Hello");
};

greet(); // Output: "Hello"
```

### Function Expression as a Callback

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(function (number) {
    return number * 2;
});
```

---

## Arrow Functions

Arrow functions provide a concise syntax for writing function expressions introduced in ES6.

```javascript
// Explicit Return
const add = (a, b) => {
    return a + b;
};

// Implicit Return
const addImplicit = (a, b) => a + b;

// Implicit Return of Object Literal
const getUser = () => ({
    name: "John"
});
```

### Lexical `this` Binding in Arrow Functions

Arrow functions do not define their own `this` context. Instead, they inherit `this` lexically from their enclosing parent execution context.

```javascript
const user = {
    name: "peter",
    greet: function () {
        return `Hello ${this.name}`;
    }
};

console.log(user.greet()); // Output: "Hello peter"
```

- In regular function methods, `this` refers to the invoking object context (`user`).

```javascript
const user = {
    name: "John",
    greet: () => {
        console.log(this.name);
    }
};

user.greet(); // Output: undefined
```

- Arrow functions do not bind their own `this` to `user`; they look up `this` in the outer lexical scope (e.g., global object or module context), where `name` is undefined.

```javascript
const user = {
  name: "John",

  greet: function () {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
};

user.greet(); // Output after 1 second: "John"
```

``` javascript
user.greet()
     │
     ▼
greet()  ← normal function
     │
     │ this = user
     │
     ▼
arrow function
     │
     │ inherits this
     ▼
this = user
```

---

## Callback Functions

A callback function is a function passed as an argument into another function, intended to be executed at a later point in time.

```javascript
const greet = () => console.log('hello');

function execute(callback) {
    callback();
}

execute(greet); // Output: "hello"
```

- When `greet` is defined, it is a standard function.
- When `greet` is passed into `execute`, it acts as a callback.
- `execute` is a Higher-Order Function because it accepts a function as an argument.

```text
execute  → Higher-Order Function (accepts callback)
callback → Parameter receiving the passed function
greet    → Callback function passed into execute
```

### Passing Arguments to Callbacks

```javascript
function processUser(name, callback) {
    callback(name);
}

function greet(name) {
    console.log(`Hello ${name}`);
}

processUser("David", greet); // Output: "Hello David"
```

> **Important:** Do not invoke the function with parentheses `()` when passing it as an argument (pass `greet`, not `greet()`). Adding `()` invokes the function immediately and passes its return value instead of the function reference.

---

## Higher-Order Functions

A Higher-Order Function (HOF) is a function that does at least one of the following:

1. Accepts one or more functions as arguments (e.g., accepting callbacks).
2. Returns a function as its result.

### Accepting a Function as an Argument

```javascript
function execute(callback) {
    callback();
}
```

### Returning a Function

```javascript
function createGreeting() {
    return function () {
        console.log("Hello!");
    };
}

const greet = createGreeting();
greet(); // Output: "Hello!"
```

### Callback vs Higher-Order Function

```text
Callback Function
   ↓
A function passed into another function as an argument

Higher-Order Function
   ↓
A function that accepts another function as an argument, or returns a function
```

---

## Pure and Impure Functions

### Pure Functions

A pure function is a function that satisfies two conditions:

1. **Deterministic:** Always returns the same output for the identical set of inputs.
2. **No Side Effects:** Does not read or modify any state or variables outside its scope (no external mutations, no I/O side effects).

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(10, 20)); // Output: 30
console.log(add(10, 20)); // Output: 30
console.log(add(10, 20)); // Output: 30
```

### Impure Functions

An impure function depends on external state outside its parameters or modifies variables and data structures outside its own scope. Its return value can change even with identical inputs.

```javascript
let tax = 0.18;

function calculatePrice(price) {
    return price + (price * tax);
}

console.log(calculatePrice(100)); // Output: 118

// If the external state changes:
tax = 0.20;
console.log(calculatePrice(100)); // Output: 120
```

### Pure vs Impure Functions Comparison

| Pure Functions | Impure Functions |
| :--- | :--- |
| Same input always produces the same output | Output can vary for the same inputs |
| Depends exclusively on its parameter arguments | Depends on external state / variables |
| Causes no external mutations or side effects | Can modify external state / variables |
| Predictable and deterministic | Less predictable |
| Easy to unit test and reason about | Harder to test due to external dependencies |
| Easy to reuse and cache (memoization) | Can have hidden coupling and dependencies |

---

## Closures

A closure is created when an inner function retains access to variables from its outer (enclosing) lexical scope, even after that outer function has finished executing and was removed from the call stack.

```javascript
function outer() {
    const message = "Hello";
    
    function inner() {
        console.log(message);
    }
    
    return inner;
}

const res = outer(); // 'outer' completes its execution
res(); // Output: "Hello" ('inner' retains access to 'message')
```

### Function Factory Using Closures

```javascript
const calculate = (mul) => {
    return (val) => val * mul;
};

const triple = calculate(3);
console.log(triple(12)); // Output: 36
```

---

## IIFE (Immediately Invoked Function Expressions)

An IIFE is a function that is executed immediately upon its definition.

### Syntax

Wrapping the function inside parentheses `(...)` turns it into an expression, and appending `()` executes it immediately:

```javascript
(function () {
    console.log("Hello");
})();

// Alternative valid syntax:
(function () {
    console.log("Hello");
}());
```

### IIFE with Parameters

```javascript
(function (name) {
    console.log(`Hello ${name}`);
})("John"); // Output: "Hello John"
```

### Returning Values from an IIFE

```javascript
const result = (function () {
    return 10 + 20;
})();

console.log(result); // Output: 30

const res = (function (a) {
    return a * a;
}(3));

console.log(res); // Output: 9
```

### Preventing Scope Pollution

Variables declared inside an IIFE remain contained within its scope and do not pollute or collide with the global namespace:

```javascript
var name = 'Peter';

(function () {
    var name = 'John';
    console.log(name); // Output: "John"
})();

console.log(name); // Output: "Peter"
```

### Arrow Function IIFE

```javascript
var name = 'Peter';

(() => {
    var name = 'John';
    console.log(name); // Output: "John"
})();

console.log(name); // Output: "Peter"
```

---

## Currying

Currying is a functional programming technique where a function that takes multiple arguments is transformed into a sequence of nested functions, each accepting a single argument.

```javascript
function add(a) {
    return (b) => {
        return (c) => a + b + c;
    };
}

console.log(add(1)(2)(3)); // Output: 6
```

---

## Object Methods

A function stored as a property of an object is called a method. When called as an object method, the `this` keyword refers to the object itself.

```javascript
const user = {
    name: 'Peter',
    greet: function () {
        return `Hi ${this.name}!`;
    }
};

console.log(user.greet()); // Output: "Hi Peter!"
```

### ES6 Method Shorthand Syntax

```javascript
const calculator = {
    // Shorthand method syntax
    add(a, b) {
        return a + b;
    },

    multiply(a, b) {
        return a * b;
    }
};

console.log(calculator.add(10, 20));      // Output: 30
console.log(calculator.multiply(10, 20)); // Output: 200
```

### Method Chaining

Methods can return `this` (the object reference), enabling consecutive method calls to be chained together in a single statement.

```javascript
const user = {
    name: 'Peter',
    age: 30,
    greet: function () {
        return `Hi ${this.name}!`;
    },
    setName(name) {
        this.name = name;
        return this; // Return object instance for chaining
    },
    setAge(age) {
        this.age = age;
        return this; // Return object instance for chaining
    }
};

user.setName("John").setAge(25);
console.log(user); // Output: { name: 'John', age: 25, greet: [Function: greet], setName: [Function: setName], setAge: [Function: setAge] }
```

---

## Array Iteration Methods

### `Array.prototype.forEach()`

`forEach()` executes a provided callback function once for each element in an array.

- **Purpose:** Used for side effects (e.g., printing items, updating external state).
- **Return Value:** Returns `undefined`.
- **Note:** It ignores any return values inside its callback and does not return a new array.

```javascript
const numbers = [10, 20, 30, 40];

numbers.forEach((num) => {
    console.log(num);
});

const result = numbers.forEach((num) => num * 2);
console.log(result); // Output: undefined
```

---

### `Array.prototype.map()`

`map()` creates a new array populated with the results of calling a provided callback function on every element in the calling array.

- **Higher-Order Function:** Takes a callback function as an argument.
- **Pure / Non-mutating:** Does not alter the original array; returns a new array.
- **Preserves Length:** The resulting array will always have the exact same length as the original array.

```javascript
const numbers = [1, 2, 3, 4, 5];
const updated = numbers.map((num) => num * 2);

console.log(updated); // Output: [2, 4, 6, 8, 10]
```

#### `map()` vs `forEach()`

- `map()` returns a newly created array containing transformed elements.
- `forEach()` executes a callback for each element without returning anything (`undefined`), ignoring any values returned by the callback.

---

### `Array.prototype.filter()`

`filter()` creates a new array containing all elements that pass the test implemented by the provided callback function (evaluating to a truthy value).

- **Higher-Order Function:** Accepts a condition-checking callback function.
- **Pure / Non-mutating:** Does not modify the original array; returns a new array.
- **Variable Length:** The output array may have the same length or fewer elements than the original array.

```javascript
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter((num) => {
    return num % 2 === 0;
});

console.log(evenNumbers); // Output: [2, 4]
```

---

### `Array.prototype.reduce()`

`reduce()` executes a user-supplied reducer callback function on each element of the array, in order, passing in the return value from the calculation on the preceding element. The final result is a single accumulated value.

- **Higher-Order Function:** Accepts a reducer callback and an optional initial value.
- **Accumulator:** A variable carrying the aggregated result across each iteration.

```javascript
const numbers = [1, 2, 3, 4, 5];

const total = numbers.reduce((accumulator, currentValue) => {
    return accumulator + currentValue;
}, 0); // 0 is the initial value

console.log(total); // Output: 15
```

---

### `Array.prototype.find()`

`find()` returns the **first element** in the array that satisfies the provided testing condition. If no values satisfy the condition, it returns `undefined`.

- **Short-circuiting:** Iterates sequentially and stops immediately once a match is found; it does not evaluate remaining elements.
- **Returns Element or `undefined`:** Does not return an array.

```javascript
const numbers = [1, 2, 3, 4, 5];

const result = numbers.find((num) => num > 3);

console.log(result); // Output: 4
```

---

### `Array.prototype.some()`

`some()` tests whether **at least one** element in the array passes the condition implemented by the provided callback function.

- Returns a boolean (`true` or `false`).
- **Short-circuiting:** Stops iterating immediately as soon as a single matching element is found.

```javascript
const numbers = [1, 3, 5, 8, 9];

const result = numbers.some((num) => num % 2 === 0);

console.log(result); // Output: true
```

---

### `Array.prototype.every()`

`every()` tests whether **all elements** in the array pass the condition implemented by the provided callback function.

- Returns `true` if every element passes, otherwise `false`.
- **Short-circuiting:** Stops iterating immediately upon encountering the first element that fails the condition.

```javascript
const numbers = [2, 4, 6, 8];

const result = numbers.every((num) => num % 2 === 0);

console.log(result); // Output: true
```

#### Empty Array Behavior with `some()` and `every()`

```javascript
[].some((num) => num > 0);  // Output: false
[].every((num) => num > 0); // Output: true
```

---

### `Array.prototype.flat()`

`flat()` creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.

- **Non-mutating:** Does not alter the original array; returns a new flattened array.
- **Default depth:** 1 level.

```javascript
const numbers = [1, 2, [3, 4], 5];
console.log(numbers.flat()); // Output: [1, 2, 3, 4, 5]

const nested = [1, 2, [3, 4, [5, 6]]];
console.log(nested.flat(2));        // Output: [1, 2, 3, 4, 5, 6]
console.log(nested.flat(Infinity)); // Output: [1, 2, 3, 4, 5, 6] (Flattens all nested levels)
```

---

### `Array.prototype.flatMap()`

`flatMap()` combines `map()` and `flat()` (depth 1) into a single method call. It maps each element using a mapping function, then flattens the result into a new array.

- **Higher-Order Function:** Accepts a callback function.
- **Flattens 1 level only:** Cannot flatten deeper than 1 level.

```javascript
const numbers = [1, 2, 3];

const result = numbers.flatMap((num) => [num, num * 2]);

console.log(result); // Output: [1, 2, 2, 4, 3, 6]
```

---

### `Array.prototype.sort()`

`sort()` sorts the elements of an array in place and returns the reference to the same array (it mutates the original array).

```javascript
const numbers = [3, 6, 7, 567, 32, 654, 90, 1, 354, 54];

// Ascending Order
numbers.sort((a, b) => a - b);
console.log(numbers); // Output: [1, 3, 6, 7, 32, 54, 90, 354, 567, 654]

// Descending Order
numbers.sort((a, b) => b - a);
console.log(numbers); // Output: [654, 567, 354, 90, 54, 32, 7, 6, 3, 1]
```

---

### Array Methods Comparison Cheatsheet

| Method | Purpose | Return Value | Mutates Original? |
| :--- | :--- | :--- | :--- |
| `forEach()` | Executes side effects per item | `undefined` | No |
| `map()` | Transforms every item | New array | No |
| `filter()` | Selects matching items | New array | No |
| `reduce()` | Aggregates items into single output | Any value | No |
| `find()` | Finds first matching item | Element / `undefined` | No |
| `some()` | Checks if at least one item matches | `true` or `false` | No |
| `every()` | Checks if all items match | `true` or `false` | No |
| `flat()` | Flattens nested arrays | New array | No |
| `flatMap()` | Transforms then flattens 1 level | New array | No |
| `sort()` | Reorders array elements in place | Array reference | **Yes** |

---

## Execution Context

An Execution Context is an environment created by JavaScript to transform and execute code. It keeps track of variables, functions, outer scope references, and `this` binding.

### Types of Execution Contexts

1. **Global Execution Context (GEC):** Created by default for executing top-level script code.
2. **Function Execution Context (FEC):** Created every time a function is invoked.

### Phases of Execution Context

Each context passes through two phases:

1. **Creation Phase:** Memory space is set up for variables and functions (hoisting occurs; `var` variables are set to `undefined`).
2. **Execution Phase:** Code is executed line by line, assigning values and invoking functions.

---

## Call Stack and Stack Overflow

### Call Stack

The Call Stack is a LIFO (Last-In, First-Out) data structure used by JavaScript to track the execution contexts created by function calls.

```javascript
function first() {
    console.log("First start");
    second();
    console.log("First end");
}

function second() {
    console.log("Second");
}

first();

// Output:
// First start
// Second
// First end
```

### Stack Overflow

Occurs when the call stack exceeds its maximum stack size limit, typically caused by unhandled infinite recursion.

```javascript
function repeat() {
    repeat(); // Throws RangeError: Maximum call stack size exceeded
}

repeat();
```

---

## Asynchronous JavaScript

### Callback Hell

Callback Hell occurs when multiple nested asynchronous callbacks make code hard to read, maintain, and debug.

```javascript
function stepOne(callback) {
    setTimeout(() => {
        console.log("Step 1");
        callback();
    }, 1000);
}

function stepTwo(callback) {
    setTimeout(() => {
        console.log("Step 2");
        callback();
    }, 1000);
}

// Nested call structure (Pyramid of Doom)
stepOne(() => {
    stepTwo(() => {
        console.log("All steps finished");
    });
});
```

---

## Promises

A `Promise` is an object representing the eventual completion or failure of an asynchronous operation.

### Promise States

1. **Pending:** Initial state; operation in progress.
2. **Fulfilled:** Operation completed successfully (`resolve()`).
3. **Rejected:** Operation failed (`reject()`).

```javascript
const paymentPromise = new Promise((resolve, reject) => {
    const paymentSuccess = true;
    if (paymentSuccess) {
        resolve("Payment completed");
    } else {
        reject("Payment failed");
    }
});

paymentPromise
    .then((data) => {
        console.log(data); // Output: "Payment completed"
    })
    .catch((err) => {
        console.error(err);
    })
    .finally(() => {
        console.log("Payment process finished."); // Executes regardless of outcome
    });
```

### Promise Chaining

Promise chaining allows sequential execution of asynchronous operations while avoiding nested callbacks. Each `.then()` returns a new Promise.

```javascript
Promise.resolve(10)
    .then((price) => {
        console.log(price); // Output: 10
        return price * 2;
    })
    .then((result) => {
        console.log(result); // Output: 20
        return result + 5;
    })
    .then((finalResult) => {
        console.log(finalResult); // Output: 25
    })
    .catch((error) => {
        console.error("Error occurred:", error.message);
    });
```

---

## Async / Await

`async/await` is modern ES8 syntax built on top of Promises that allows asynchronous code to be written in a cleaner, synchronous style.

- Marking a function with `async` ensures it always returns a Promise.
- The `await` keyword pauses execution inside an `async` function until the Promise settles (resolves or rejects).

```javascript
function processPayment() {
    return Promise.resolve("Payment processed");
}

async function getOrder() {
    const result = await processPayment();
    console.log(result); // Output: "Payment processed"
}

getOrder();
```

### Error Handling with `async/await`

```javascript
function processPayment() {
    return Promise.reject(new Error("Payment declined"));
}

async function checkout() {
    try {
        const result = await processPayment();
        console.log(result);
    } catch (error) {
        console.error(error.message); // Output: "Payment declined"
    }
}

checkout();
```

### Sequential vs Parallel Execution

- **Sequential Execution:** Calls operations step-by-step using consecutive `await` statements.
- **Parallel Execution:** Initiates multiple operations simultaneously using combinators like `Promise.all()`.

---

## Promise Combinators

Promise combinators allow concurrent handling of multiple Promises simultaneously.

### `Promise.all()`

Waits for **all** input Promises to fulfill. Returns an array of resolved values preserving input order.

- **Short-circuiting:** Rejects immediately if **any** input Promise rejects.

```javascript
const orderStatus = Promise.resolve("Order Placed");
const paymentStatus = Promise.resolve("Payment Completed");
const deliveryStatus = Promise.resolve("Order Delivered");

Promise.all([orderStatus, paymentStatus, deliveryStatus])
    .then((data) => console.log(data));
// Output: ['Order Placed', 'Payment Completed', 'Order Delivered']
```

### `Promise.allSettled()`

Waits until **all** Promises settle (either fulfilled or rejected). Never short-circuits.

- **Return Value:** Returns an array of outcome objects with `{ status: 'fulfilled', value }` or `{ status: 'rejected', reason }`.

```javascript
const restaurant = Promise.resolve("Restaurant loaded");
const menu = Promise.reject(new Error("Menu loading failed"));
const coupons = Promise.resolve("Coupons loaded");

Promise.allSettled([restaurant, menu, coupons])
    .then((results) => console.log(results));
```

### `Promise.race()`

Returns a Promise that settles as soon as the **first** Promise settles (whether fulfilled or rejected).

```javascript
const taskA = new Promise((resolve) => setTimeout(() => resolve("A completed"), 300));
const taskB = new Promise((resolve) => setTimeout(() => resolve("B completed"), 100));

Promise.race([taskA, taskB])
    .then((result) => console.log(result)); // Output: "B completed"
```

### `Promise.any()`

Returns a Promise that resolves as soon as the **first successful** (fulfilled) Promise resolves.

- Ignores rejections unless **all** input Promises reject, in which case it throws an `AggregateError`.

```javascript
const taskA = Promise.reject(new Error("Failed"));
const taskB = new Promise((resolve) => setTimeout(() => resolve("Success B"), 200));

Promise.any([taskA, taskB])
    .then((result) => console.log(result)); // Output: "Success B"
```

### Promise Combinators Summary

| Method | Settles When... | Fulfills When... | Rejects When... |
| :--- | :--- | :--- | :--- |
| `Promise.all()` | All fulfill OR any rejects | All promises fulfill | Any promise rejects |
| `Promise.allSettled()` | All promises settle | All promises settle | Never rejects |
| `Promise.race()` | First promise settles | First promise fulfills | First promise rejects |
| `Promise.any()` | First fulfills OR all reject | First promise fulfills | All promises reject (`AggregateError`) |

---

## Error Handling

Error handling prevents runtime failures from crashing applications using standard `try...catch...finally` blocks or throwing custom errors.

### Common Standard JavaScript Error Types

- **`ReferenceError`:** Occurs when referencing an undeclared variable.
- **`TypeError`:** Occurs when executing an invalid operation on an incompatible type.
- **`SyntaxError`:** Occurs when writing invalid syntax.
- **`RangeError`:** Occurs when a value falls outside allowable numeric limits.

### `try...catch` and Throwing Errors

```javascript
const checkAge = (age) => {
    if (age < 18) {
        throw new Error("You must be at least 18");
    }
    return "Access granted";
};

try {
    console.log(checkAge(16));
} catch (error) {
    console.error(error.message); // Output: "You must be at least 18"
} finally {
    console.log("Age verification completed."); // Always executes
}
```

---

## Event Loop and Concurrency Model

JavaScript executes code in a single-threaded runtime using an Event Loop mechanism to handle non-blocking asynchronous operations.

### Key Components

1. **Call Stack:** Executes synchronous functions sequentially (LIFO).
2. **Web/Node APIs:** Environment features handling background tasks like timers (`setTimeout`), HTTP requests (`fetch`), and DOM events.
3. **Task Queue (Macrotask Queue):** Holds ready callbacks from timers (`setTimeout`, `setInterval`), I/O, and UI events (FIFO).
4. **Microtask Queue:** Holds high-priority callbacks like Promise reactions (`.then()`, `.catch()`, `.finally()`), `queueMicrotask()`, and `async/await` continuations.

### Execution Priority

1. Synchronous code in the Call Stack executes to completion.
2. The Event Loop drains **all items in the Microtask Queue** completely before executing next items.
3. The Event Loop picks the oldest task from the Macrotask Queue and pushes it onto the Call Stack.

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timer Callback (Macrotask)");
}, 0);

Promise.resolve().then(() => {
    console.log("Promise Callback (Microtask)");
});

console.log("End");

// Execution Output:
// Output: "Start"
// Output: "End"
// Output: "Promise Callback (Microtask)"
// Output: "Timer Callback (Macrotask)"
```

---

## Prototypes

A prototype is an object from which other objects inherit properties and methods. In JavaScript, every object has an internal link to another object called its prototype.

If an object does not possess a requested property or method directly on itself, JavaScript searches for it on the object's prototype.

### Own Properties vs Prototype Properties

```javascript
const user = {
    name: "John"
};

console.log(user.name);       // Output: "John" (Own property)
console.log(user.toString()); // Output: "[object Object]" (Inherited from Object.prototype)
```

In the example above:
- `name` is an **own property** defined directly on the `user` object.
- `toString()` is not defined directly on `user`, so JavaScript looks up the prototype link to `Object.prototype` where `toString()` exists.

### Prototype Link Chain Representation

```text
user object
  └── Own property: name: "John"
        │
        ▼ (prototype link)
Object.prototype
  └── Methods: toString(), valueOf(), hasOwnProperty(), ...
        │
        ▼ (prototype link)
null (End of the prototype chain)
```

### Inspecting Prototypes

You can inspect an object's prototype using `Object.getPrototypeOf()` (recommended) or the legacy `__proto__` accessor property:

```javascript
const user = {
    name: "John"
};

console.log(Object.getPrototypeOf(user) === Object.prototype); // Output: true
console.log(user.__proto__ === Object.prototype);              // Output: true
```

---

## Prototypal Inheritance

Prototypal inheritance allows an object to inherit properties and methods directly from another object. You can explicitly set the prototype of an object using `Object.setPrototypeOf()` or `Object.create()`.

```javascript
const person = {
    greet() {
        return "Hello Everyone!";
    }
};

const user = {
    name: "Peter"
};

// Set person as the prototype of user
Object.setPrototypeOf(user, person);

console.log(user.name);    // Output: "Peter"
console.log(user.greet()); // Output: "Hello Everyone!"

console.log(user.hasOwnProperty("name"));  // Output: true (own property)
console.log(user.hasOwnProperty("greet")); // Output: false (inherited property)
console.log("greet" in user);              // Output: true (exists on object or prototype chain)
```

### Property Existence Checks

| Check | Meaning | Description |
| :--- | :--- | :--- |
| `user.hasOwnProperty("name")` | Own Property | Checks if the property belongs directly to `user`. Returns `false` for inherited properties. |
| `user.hasOwnProperty("greet")` | Not an Own Property | Returns `false` because `greet` resides on `person`, not directly on `user`. |
| `"greet" in user` | Prototype Chain Search | Returns `true` if `greet` exists directly on `user` or anywhere up its prototype chain. |

---

## Prototypes in Constructor Functions

When defining constructor functions, methods are typically attached to the function's `.prototype` property. This ensures all instances share a single copy of the method in memory rather than creating a separate function copy for each instance.

```javascript
function User(name) {
    this.name = name;
}

// Add a shared method to the prototype
User.prototype.greet = function () {
    return "Hello " + this.name;
};

const user1 = new User("John");
const user2 = new User("Peter");

console.log(user1.greet()); // Output: "Hello John"
console.log(user2.greet()); // Output: "Hello Peter"
```

### Dynamic Prototype Updates

Because instances share the same prototype object reference, updating or adding methods to `User.prototype` immediately reflects across all existing and future instances:

```javascript
function User(name) {
    this.name = name;
}

User.prototype.greet = function () {
    return "Hello " + this.name;
};

const user1 = new User("John");
console.log(user1.greet()); // Output: "Hello John"

// Updating the prototype method dynamically
User.prototype.greet = function () {
    return "Welcome " + this.name;
};

console.log(user1.greet()); // Output: "Welcome John"
```

---

## Prototype Chain

The **prototype chain** is the series of linked prototype objects JavaScript traverses when resolving a property or method that does not exist directly on the target object.

JavaScript inspects the object itself first. If the property is not found, it traverses upward to the object's prototype, then that prototype's prototype, and so on until the property is found or `null` is reached (which evaluates to `undefined`).

```javascript
const animal = {
    eat() {
        return "Eating";
    }
};

// Create dog inheriting from animal
const dog = Object.create(animal);

dog.bark = function () {
    return "Barking";
};

console.log(dog.bark());     // Output: "Barking" (Found on dog)
console.log(dog.eat());      // Output: "Eating" (Found on animal)
console.log(dog.toString()); // Output: "[object Object]" (Found on Object.prototype)
console.log(dog.wings);      // Output: undefined (Not found anywhere in chain)

// Verifying the prototype hierarchy:
console.log(Object.getPrototypeOf(dog) === animal); // Output: true
console.log(Object.getPrototypeOf(Object.getPrototypeOf(dog)) === Object.prototype); // Output: true
console.log(Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(dog)))); // Output: null
```

---

## The `this` Keyword

The `this` keyword refers to the execution context in which the current function is executed. How `this` is evaluated depends on how the function is invoked, not where it is defined (with the exception of arrow functions).

### Method Invocation

When a regular function is invoked as a method of an object, `this` refers to the object preceding the dot:

```javascript
const user = {
    name: "John",
    greet() {
        console.log(this.name);
    }
};

user.greet(); // Output: "John"
```

Sharing functions across multiple objects binds `this` to the object currently calling the function:

```javascript
function greet() {
    console.log(this.name);
}

const user1 = { name: "John", greet };
const user2 = { name: "Sam", greet };

user1.greet(); // Output: "John"
user2.greet(); // Output: "Sam"
```

### `this` in Regular Functions

For a plain, standalone function call (`greet()`), `this` depends on the execution mode:

- **Non-Strict Mode (Classic script):** `this` defaults to the global object (`window` in browsers, `global` in Node.js).
- **Strict Mode (`"use strict"`):** `this` remains `undefined`.
- **ES Modules:** Modules run in strict mode by default, so `this` is `undefined`.

```javascript
function showThis() {
    console.log(this);
}

showThis(); // Output: window (non-strict browser) or undefined (strict mode / ES modules)
```

#### Losing `this` Context (Detached Methods)

Extracting an object method and calling it as a standalone function disconnects `this` from the original object:

```javascript
const user = {
    name: "John",
    greet() {
        console.log(this.name);
    }
};

const greetFunction = user.greet;

// greetFunction();
// In strict mode: TypeError: Cannot read properties of undefined (reading 'name')
// In non-strict mode: undefined (looks up window.name)
```

### `this` in Arrow Functions

Arrow functions do **not** have their own `this` binding. Instead, they capture `this` lexically from their surrounding enclosing scope at definition time.

#### Arrow Function on Object Literal

```javascript
const user = {
    name: "John",
    greet: () => {
        console.log(this.name);
    }
};

user.greet(); // Output: undefined (inherits this from surrounding global/module scope)
```

#### Arrow Function Inside a Regular Method

When nested inside a method or callback, an arrow function retains the outer method's `this`:

```javascript
const user = {
    name: "John",
    greet() {
        const sayName = () => {
            console.log(this.name);
        };
        sayName();
    }
};

user.greet(); // Output: "John"
```

#### Arrow Function in Asynchronous Callbacks

```javascript
const user = {
    name: "John",
    greetLater() {
        setTimeout(() => {
            console.log(this.name);
        }, 100);
    }
};

user.greetLater(); // Output: "John" (after 100ms)
```

### `this` in Constructor Functions

When invoking a function with the `new` keyword, JavaScript automatically creates a fresh blank object and binds `this` to that new instance:

```javascript
function User(name) {
    this.name = name;
}

const user1 = new User("John");
console.log(user1.name); // Output: "John"
```

---

## Explicit Binding: `call()`, `apply()`, and `bind()`

JavaScript provides explicit binding methods on `Function.prototype` to control what `this` refers to during function execution.

### `call()`

Calls a function immediately with a specified `this` value and arguments provided individually (comma-separated).

**Syntax:**
```javascript
func.call(thisArg, arg1, arg2, /* ..., */ argN);
```

```javascript
function introduce(city, role) {
    console.log(`${this.name} is from ${city} and works as ${role}`);
}

const user = {
    name: "John"
};

introduce.call(user, "Bangalore", "Developer");
// Output: "John is from Bangalore and works as Developer"
```

#### Reusing Methods Across Objects

```javascript
function greet() {
    console.log(`Hello, ${this.name}`);
}

const user1 = { name: "John" };
const user2 = { name: "Sam" };

greet.call(user1); // Output: "Hello, John"
greet.call(user2); // Output: "Hello, Sam"
```

#### Calling Prototype Methods Explicitly

```javascript
function User(name) {
    this.name = name;
}

User.prototype.greet = function () {
    console.log(`Hello, ${this.name}`);
};

const user1 = new User("John");
const user2 = new User("Sam");

User.prototype.greet.call(user1); // Output: "Hello, John"
User.prototype.greet.call(user2); // Output: "Hello, Sam"
```

### `apply()`

Calls a function immediately with a specified `this` value, but accepts arguments as a single array (or array-like object).

**Syntax:**
```javascript
func.apply(thisArg, [argsArray]);
```

```javascript
function introduce(city, role) {
    console.log(`${this.name} is from ${city} and works as ${role}`);
}

const user = {
    name: "John"
};

introduce.apply(user, ["Bangalore", "Developer"]);
// Output: "John is from Bangalore and works as Developer"
```

### `bind()`

Does **not** execute the function immediately. Instead, it returns a **new function** with its `this` context permanently bound to the provided object, along with optional preset arguments.

**Syntax:**
```javascript
const boundFunc = func.bind(thisArg, arg1, /* ..., */ argN);
```

```javascript
const user = {
    name: "Alex",
    greet() {
        console.log(`Hello, ${this.name}`);
    }
};

const greetFn = user.greet;
// greetFn(); // Throws TypeError in strict mode because 'this' is lost

// Permanently binding 'user' as 'this'
const boundGreet = user.greet.bind(user);
boundGreet(); // Output: "Hello, Alex"
```

#### Partial Application with `bind()`

You can preset initial arguments when binding:

```javascript
function introduce(city, role) {
    console.log(`${this.name} - ${city} - ${role}`);
}

const user = { name: "Sam" };

// Preset city as "Chennai"
const introduceSam = introduce.bind(user, "Chennai");

// Supply remaining argument later
introduceSam("Developer"); // Output: "Sam - Chennai - Developer"
```

### Comparison: `call()` vs `apply()` vs `bind()`

| Method | Execution | Argument Passing | Return Value |
| :--- | :--- | :--- | :--- |
| `call()` | Executes immediately | Comma-separated list (`arg1, arg2`) | Result of the function call |
| `apply()` | Executes immediately | Array or array-like (`[arg1, arg2]`) | Result of the function call |
| `bind()` | Does not execute immediately | Comma-separated list (preset or subsequent) | A new function with bound `this` |

---

## Classes

Introduced in ES6, classes provide clean, declarative syntax for creating object blueprints and implementing prototypal inheritance under the hood.

### Basic Class Syntax

```javascript
class User {
    constructor(name, role) {
        this.name = name;
        this.role = role;
    }

    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
}

const user1 = new User("David", "React Developer");
const user2 = new User("Peter", "PHP Developer");

user1.greet(); // Output: "Hello, I'm David"
user2.greet(); // Output: "Hello, I'm Peter"
```

- **`class User`**: Declares the class template.
- **`constructor()`**: Special initialization method called automatically when creating an instance with `new`.
- **`this.name = name`**: Attaches instance properties directly to the newly created instance.
- **`greet()`**: Method defined on `User.prototype`, shared across all instances.

### Class Method Binding

Like regular methods, class methods can lose their `this` context if extracted without binding:

```javascript
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(this.name);
    }
}

const user = new User("Alex");
user.greet(); // Output: "Alex"

const detachedGreet = user.greet;
// detachedGreet(); // Throws TypeError: Cannot read properties of undefined (reading 'name')

const boundGreet = user.greet.bind(user);
boundGreet(); // Output: "Alex"
```

### Classes vs Constructor Functions

| Feature | Constructor Function | Class |
| :--- | :--- | :--- |
| **Declaration** | Standard `function User(name) { ... }` | `class User { ... }` |
| **Methods** | Attached manually via `User.prototype.method` | Declared directly within the class body |
| **Invocation** | Can be invoked without `new` (may pollute global scope) | Must be invoked with `new` (throws `TypeError` otherwise) |
| **Strict Mode** | Follows ambient script strictness | Class body always executes in strict mode |
| **Hoisting** | Function declarations are hoisted | Classes are not hoisted (subject to TDZ) |

### Class Inheritance: `extends` and `super`

The `extends` keyword establishes a prototype link between parent and child classes. The `super` keyword calls the parent constructor or invokes parent methods.

```javascript
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello ${this.name}`);
    }
}

class Admin extends User {
    constructor(name, permission) {
        super(name); // Invokes the parent constructor (must be called before accessing 'this')
        this.permission = permission;
    }

    showPermission() {
        console.log(this.permission);
    }
}

const admin = new Admin("Peter", "Manage users");

admin.greet();          // Output: "Hello Peter" (Inherited from User)
admin.showPermission(); // Output: "Manage users" (Own method on Admin)
```

#### Calling Parent Methods with `super`

```javascript
class User {
    greet() {
        console.log("Hello from User");
    }
}

class Admin extends User {
    greet() {
        console.log("Hello from Admin");
        super.greet(); // Calls User.prototype.greet()
    }
}

const admin = new Admin();
admin.greet();
// Output: "Hello from Admin"
// Output: "Hello from User"
```

---

## Generator Functions

Generators are special functions that can pause execution midway and resume later, maintaining their internal execution state between invocations.

- Declared using the `function*` syntax.
- Pauses execution and emits a value using the `yield` keyword.
- Resumes execution by calling the `.next()` method on the generator object.

```javascript
function* generateNumbers() {
    console.log("First");
    yield 1;

    console.log("Second");
    yield 2;

    console.log("Third");
    yield 3;
}

const generator = generateNumbers();

console.log(generator.next()); // Output: First -> { value: 1, done: false }
console.log(generator.next()); // Output: Second -> { value: 2, done: false }
console.log(generator.next()); // Output: Third -> { value: 3, done: false }
console.log(generator.next()); // Output: { value: undefined, done: true }
```

### Generator Iterator Result Object

Every call to `.next()` returns an object with two properties:
- **`value`**: The yielded value (or the value returned by `return`).
- **`done`**: A boolean flag (`false` if more values remain, `true` when execution is complete).

### Generator Operations

- **`function*`**: Declares a generator function.
- **Calling generator**: Returns a generator object without executing the body immediately.
- **`yield`**: Produces a value and suspends execution.
- **`.next()`**: Resumes execution until the next `yield` or `return`.
- **`.next(value)`**: Resumes execution and passes a value back into the generator where it was paused.
- **`return(value)`**: Immediately finishes the generator, setting `done: true`.

---

## Iterators and Iterables

The iteration protocol defines a standard way to produce and consume sequences of values in JavaScript.

### Iterable

An **iterable** is any data structure that provides an iterator method accessible via the well-known symbol `Symbol.iterator`. Built-in iterables include `Array`, `String`, `Map`, `Set`, and `TypedArray`.

### Iterator

An **iterator** is an object with a `.next()` method that produces iteration results in the format `{ value: any, done: boolean }`.

```javascript
const numbers = [10, 20, 30];

// Obtain the iterator from the iterable array
const iterator = numbers[Symbol.iterator]();

console.log(iterator.next()); // Output: { value: 10, done: false }
console.log(iterator.next()); // Output: { value: 20, done: false }
console.log(iterator.next()); // Output: { value: 30, done: false }
console.log(iterator.next()); // Output: { value: undefined, done: true }
```

### Iterable vs Iterator

| Term | Definition | Built-in Examples |
| :--- | :--- | :--- |
| **Iterable** | An object implementing `[Symbol.iterator]()` that produces an iterator. | `Array`, `String`, `Map`, `Set`, `NodeList` |
| **Iterator** | An object containing a `.next()` method that yields consecutive sequence values. | The object returned by `iterable[Symbol.iterator]()` |

```javascript
const fruits = ["Apple", "Banana", "Mango"];

// Check method types
console.log(typeof fruits[Symbol.iterator]); // Output: "function"

// Get iterator and step through
const iterator = fruits[Symbol.iterator]();
console.log(iterator.next()); // Output: { value: "Apple", done: false }
console.log(iterator.next()); // Output: { value: "Banana", done: false }
console.log(iterator.next()); // Output: { value: "Mango", done: false }
console.log(iterator.next()); // Output: { value: undefined, done: true }
```

---

## ES Modules (ESM)

ES Modules organize code into discrete, reusable files with strict scope boundaries. Modules export features using `export` and import them in other files using `import`.

### Named Exports

A module can export multiple variables, functions, or classes by name.

```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
```

```javascript
// app.js
import { add, subtract } from "./math.js";

console.log(add(10, 5));      // Output: 15
console.log(subtract(10, 5)); // Output: 5
```

#### Aliasing Named Imports

You can rename imports using the `as` keyword:

```javascript
// user.js
export const username = "John";
export function greet() {
    return `Hello, ${username}`;
}

// app.js
import { username as name, greet } from "./user.js";

console.log(name);    // Output: "John"
console.log(greet()); // Output: "Hello, John"
```

### Default Exports

A module can designate a single primary export using `export default`. Default exports can be imported without curly braces and under any custom name.

```javascript
// greet.js
export default function greet() {
    return "Hello!";
}
```

```javascript
// app.js
import greetUser from "./greet.js";

console.log(greetUser()); // Output: "Hello!"
```

### Named Exports vs Default Exports

| Feature | Named Export | Default Export |
| :--- | :--- | :--- |
| **Quantity per module** | Multiple allowed | Only one default export per module |
| **Import Syntax** | Imported with curly braces: `import { name }` | Imported without curly braces: `import customName` |
| **Naming Restrictions** | Must match export name (unless aliased with `as`) | Can be given any identifier name by importer |

### Benefits of Modular JavaScript

1. **Code Reusability:** Write shared logic once and import it across multiple modules.
2. **Namespace Isolation:** Declarations are file-scoped, preventing global scope pollution.
3. **Explicit Dependencies:** Import statements clearly declare the external dependencies of each file.
4. **Maintainability:** Isolates code into focused, manageable units.

---

## Memory Management and Garbage Collection

JavaScript automatically manages memory allocation, usage, and deallocation through runtime memory mechanisms and garbage collection.

### Memory Lifecycle

1. **Allocation:** JavaScript allocates system memory when values, objects, or variables are declared.
2. **Usage:** The application reads, writes, and operates on values in memory.
3. **Release:** Memory that is no longer needed or reachable is released back to the operating system.

```javascript
let username = "John"; // Allocates memory for string
username = "David";    // Allocates memory for new string; previous string becomes candidate for GC
```

> [!NOTE]
> Reassigning a variable does not instantly purge the old value from memory; it simply removes that specific reference.

### Memory Architecture: Stack vs Heap

- **Call Stack:** Stores function execution frames, local execution contexts, and primitive values or object memory addresses (references). Managed via LIFO.
- **Memory Heap:** Unstructured pool of memory where objects, arrays, and functions are dynamically allocated.

### Garbage Collection (GC)

Garbage collection is an automatic runtime background process that identifies and reclaims memory allocated to objects that are no longer **reachable** by the application.

- **Reachable Objects:** Values accessible directly from root references (global variables, local variables in the current call stack) or indirectly through other reachable objects.
- **Unreachable Objects:** Objects that cannot be reached through any active reference chain from root references. These are safely reclaimed by the garbage collector.

```javascript
let user = { name: "John" };
let anotherUser = user;

user = null; // Removes reference from 'user'
```

- Both `user` and `anotherUser` initially referenced the same object in the heap.
- Setting `user = null` removes only one reference.
- Because `anotherUser` still references the object, it remains **reachable** and is **not** garbage collected.

### Memory Leaks

A **memory leak** occurs when an application retains references to memory that is no longer needed, preventing the garbage collector from reclaiming it.

#### Common Causes of Memory Leaks

1. **Unbounded Data Accumulation (Global Caches/Arrays):**
   ```javascript
   const data = [];

   function storeData() {
       data.push(new Array(10000).fill("Hello"));
   }

   storeData();
   storeData();
   storeData();
   ```

2. **Forgotten Event Listeners:**
   ```javascript
   function setup() {
       const user = { name: "John" };

       // If listener is never removed, 'user' remains retained in memory through the callback closure
       window.addEventListener("resize", function () {
           console.log(user.name);
       });
   }
   ```

3. **Uncleared Timers and Intervals:** Unstopped `setInterval()` or `setTimeout()` callbacks holding closures over outer variables.

#### Preventing Memory Leaks

- **Remove Event Listeners:** Call `removeEventListener()` when components unmount or event listeners are no longer needed.
- **Clear Timers:** Always cancel timers using `clearTimeout()` or `clearInterval()`.
- **Avoid Global Scope Pollution:** Scope variables locally and avoid unneeded long-lived collections.
- **Nullify Obsolete References:** Clean up entries from caches, arrays, maps, and subscriptions when done.
