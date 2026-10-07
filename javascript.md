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
