# JavaScript Reference Guide

## Scope

Scope determines the accessibility and visibility of variables within different parts of your code. JavaScript has global scope, function scope, and block scope.

```javascript
function add() {
    if (true) {
        var a = 1;   // Function-scoped
        let b = 2;   // Block-scoped
        const c = 3; // Block-scoped
    }
    // 'a' is accessible here due to function scope.
    // 'b' and 'c' would throw a ReferenceError if accessed here.
    return a + 2; 
}

console.log(add()); // Output: 3
```

---

## Hoisting

Hoisting is JavaScript's default behavior of moving variable and function declarations to the top of their containing scope during compilation. Variable declarations initialized with `var` are hoisted with a value of `undefined`.

```javascript
console.log(hoisting); // Output: undefined
var hoisting = 'hello';
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

Objects, Arrays, Functions, Dates, Maps, and Sets are reference types. A variable assigned to a reference type holds a memory address (reference) pointing to the object, not the object itself.

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
- Functions, Arrays, Dates, Maps, and Sets are also objects in JavaScript:

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

A shallow copy duplicates top-level properties. However, nested object properties remain shared references.

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

- Spread operator (`...`) creates a shallow duplicate. Nested levels remain linked by reference.

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
console.log(greet(null));      // Output: "Hello null" (null is treated as a explicit value)
```

- Function declarations are fully hoisted, allowing them to be called before their definition appears in the code structure.

```javascript
greet(); // Output: "Hello"

function greet() {
    console.log("Hello");
}
```

### Function Scope

Variables declared inside a function are local to that function body and cannot be accessed externally.

```javascript
function test() {
    const message = "Hello";
    console.log(message);
}

test();
// console.log(message); // Throws ReferenceError: message is not defined
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

``` javascript
const user = {
  name: "John",

  greet: function () {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
};

user.greet()
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

#### Callback Parameters

```javascript
array.map((element, index, array) => {
    // element: The current value being processed
    // index: The index of the current element
    // array: The original array map was called upon
});
```

#### `map()` vs `forEach()`

- `map()` returns a newly created array containing transformed elements.
- `forEach()` executes a callback for each element without returning anything (`undefined`), ignoring any values returned by the callback.

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach executes side effects
numbers.forEach((num) => {
    console.log(num);
});

// forEach ignores callback return values
const result = numbers.forEach((num) => {
    return num * 3;
});

console.log(result);  // Output: undefined
console.log(numbers); // Output: [1, 2, 3, 4, 5] (original unchanged)
```

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

#### Callback Parameters

```javascript
array.filter((element, index, array) => {
    // element: The current value being processed
    // index: The index of the current element
    // array: The original array
    return condition; // Must return a truthy or falsy value
});
```

#### Conceptual Comparison: `map()` vs `filter()`

```text
map()
 ↓
"Transform every item"

filter()
 ↓
"Should I keep this item?"
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

#### Callback Parameters

```javascript
array.reduce((accumulator, currentValue, index, array) => {
    // accumulator: Value accumulated from previous iterations (or initialValue)
    // currentValue: The current element being processed
    // index: Current index
    // array: The original array
}, initialValue);
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

```text
Match found → Returns the matching element
No match    → Returns undefined
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

#### Comparison: `find()` vs `filter()` vs `some()`

```text
find()   → "Which one is the first match?"
filter() → "Give me ALL matching elements"
some()   → "Is there AT LEAST ONE matching element?"
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

#### `some()` vs `every()`

```text
some()  → Is AT LEAST ONE element true?
every() → Are ALL elements true?
```

#### Empty Array Behavior with `some()` and `every()`

```javascript
[].some((num) => num > 0);  // Output: false
[].every((num) => num > 0); // Output: true
```

- `some()` asks: *"Is there at least one element satisfying the condition?"* An empty array has no elements, so it evaluates to `false`.
- `every()` asks: *"Did any element fail the condition?"* In an empty array, no elements failed (vacuous truth), so it evaluates to `true`.

---

### `Array.prototype.sort()`

`sort()` sorts the elements of an array in place and returns the reference to the same array (it mutates the original array).

- By default, elements are converted to strings and compared using their UTF-16 code units (lexicographical sorting).

```javascript
const fruits = ["banana", "apple", "orange"];

fruits.sort();
console.log(fruits); // Output: ["apple", "banana", "orange"]

// Descending alphabetical sort using localeCompare
fruits.sort((a, b) => b.localeCompare(a));
console.log(fruits); // Output: ["orange", "banana", "apple"]
```

#### Sorting Numbers Using a Comparator Function

Default string-based sorting leads to unexpected results for numbers (e.g., `100` is placed before `25` because `"1"` comes before `"2"`). To sort numbers numerically, provide a comparator callback `(a, b) => a - b`:

```javascript
const numbers = [3, 6, 7, 567, 32, 654, 90, 1, 354, 54];

// Ascending Order
numbers.sort((a, b) => a - b);
console.log(numbers); // Output: [1, 3, 6, 7, 32, 54, 90, 354, 567, 654]

// Descending Order
numbers.sort((a, b) => b - a);
console.log(numbers); // Output: [654, 567, 354, 90, 54, 32, 7, 6, 3, 1]
```

#### Why `(a, b) => a - b` Works

```text
Negative value (< 0) → 'a' is placed before 'b'
Positive value (> 0) → 'b' is placed before 'a'
Zero (=== 0)         → Keep original relative order
```

---

### Array Methods Comparison & Cheatsheet

| Method | Purpose | Return Value | Output Length / Shape | Mutates Original? |
| :--- | :--- | :--- | :--- | :--- |
| `map()` | Transforms every element into a new array | New array | Same length as original | No |
| `filter()` | Selects matching elements | New array | 0 to original length | No |
| `forEach()` | Executes side effects for each element | `undefined` | No array returned | No |
| `reduce()` | Combines elements into a single accumulated value | Single value | Any data type (number, object, etc.) | No |
| `find()` | Finds the first matching element | Single element or `undefined` | Single item | No |
| `some()` | Checks if at least one element satisfies condition | `true` or `false` | Boolean | No |
| `every()` | Checks if all elements satisfy condition | `true` or `false` | Boolean | No |
| `sort()` | Sorts elements in place | Reference to original array | Same length | **Yes** |

#### Quick Reference

```text
map()     → CHANGE / TRANSFORM every item into a new array
filter()  → SELECT all matching items into a new array
find()    → FIND the first matching item or return undefined
some()    → CHECK if at least one item satisfies the condition (returns boolean)
every()   → CHECK if all items satisfy the condition (returns boolean)
reduce()  → COMBINE / ACCUMULATE items into a single final value
sort()    → REORDER items in place (mutates original array)
```
