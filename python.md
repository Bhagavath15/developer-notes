# Python – Learning Notes

# Day 1 – Python Introduction

## 1. What is Python?

- Python is a high-level, general-purpose, dynamically typed, and strongly typed programming language.
- Created by Guido van Rossum and first released in 1991.
- Known for its readable syntax and ease of development.
- Used in backend development, automation, data science, and AI/ML.

### A. High-Level Language

A high-level language allows developers to write human-readable instructions instead of working directly with machine instructions.

- Machine-level instructions are represented using binary values (`0` and `1`).
- Python handles low-level operations such as memory management, allowing developers to focus on application logic.

### B. General-Purpose Language

Python can be used for different types of applications:

- **Backend:** Build REST APIs using Django, Flask, and FastAPI.
- **AI/ML:** Develop machine learning models and AI applications.
- **Automation:** Automate repetitive tasks and file operations.
- **Data Science:** Analyze data and create visualizations.

### C. Dynamically Typed Language

Python determines variable types at runtime. We don't need to declare the type explicitly.

Python is also strongly typed, meaning it doesn't automatically mix incompatible types.

```python
result = "10" + 5

# TypeError
```

Here, Python doesn't automatically convert the string into an integer.

---

## 2. Features of Python

| Feature | Explanation |
| --- | --- |
| Easy to read | Simple and readable syntax |
| Dynamically typed | No explicit type declaration |
| Strongly typed | Incompatible types aren't silently mixed |
| Object-oriented | Supports classes and objects |
| Cross-platform | Runs on Windows, Linux, and macOS |
| Open-source | Freely available |
| Large ecosystem | Supports thousands of libraries |
| Automatic memory management | Handles object memory and garbage collection |

### Is Python purely object-oriented?

No. Python supports multiple programming paradigms:

1. **Procedural:** Executes instructions step by step.
2. **Object-oriented:** Uses classes and objects.
3. **Functional:** Uses functions, lambda expressions, and higher-order functions.

---

## 3. How Python Executes Code

An important interview topic.

Before understanding execution, let's understand three terms.

### A. Source Code

The code written by a developer.

```python
print("Hello World")
```

Saved in a Python file with the `.py` extension, such as `main.py`.

### B. Bytecode

- An intermediate representation of Python instructions.
- In CPython, source code is compiled into bytecode before execution.
- Bytecode is not machine code.

### C. Python Virtual Machine (PVM)

The execution component of CPython that interprets and executes Python bytecode.

### Python Execution Flow (CPython)

```text
Python Source Code (.py)
          |
          v
       Compiler
          |
          v
     Python Bytecode
          |
          v
 Python Virtual Machine
          |
          v
    Program Output
```

### Execution Steps

**Step 1 – Write Source Code**

```python
a = 10
b = 20

print(a + b)
```

**Step 2 – Compilation**

CPython compiles the source code into bytecode and checks for syntax errors.

**Step 3 – Bytecode Generation**

Python generates intermediate instructions from the source code.

We can inspect them using the `dis` module.

```python
import dis

def add_numbers():
    a = 10
    b = 20
    return a + b

dis.dis(add_numbers)
```

Common bytecode instructions include:

- `LOAD_CONST`
- `STORE_FAST`
- `BINARY_OP`
- `RETURN_VALUE`

Exact instructions depend on the Python version.

**Step 4 – PVM Execution**

The Python Virtual Machine executes the bytecode instructions.

**Step 5 – Output**

```text
30
```

---

## 4. Day 1 – Interview Questions

1. **What is Python?**  
   A high-level, general-purpose, dynamically typed, and strongly typed programming language.

2. **Who created Python?**  
   Guido van Rossum, first released in 1991.

3. **Why is Python high-level?**  
   Developers can write readable code without directly managing CPU registers, memory addresses, or machine instructions.

4. **What is dynamic typing?**  
   Variable types are determined at runtime without explicit type declarations.

5. **Is Python dynamically typed or strongly typed?**  
   Both. Dynamic typing determines types at runtime, while strong typing prevents silently mixing incompatible types.

6. **Is Python purely object-oriented?**  
   No. It supports procedural, object-oriented, and functional programming.

7. **What is bytecode?**  
   An intermediate representation of Python source code.

8. **What is PVM?**  
   The execution component of CPython that executes bytecode.

9. **Is bytecode machine code?**  
   No. Machine code is directly executable by the processor, whereas Python bytecode requires the Python runtime.

10. **How can we inspect bytecode?**  
    Using Python's built-in `dis` module.

---

## 5. Day 1 – Practice Tasks

1. Print your name, age, and learning goal.
2. Assign different data types to a variable and check them using `type()`.
3. Try `"10" + 5`, `"10" + str(5)`, and `int("10") + 5`. Understand the results.
4. Create a multiplication function and inspect its bytecode using `dis`.
5. Explain Python execution flow without looking at the notes.

**Day 1 Completed: Python Introduction and Execution Flow.**

---

# Day 2 – Variables and Dynamic Typing

## 1. What is a Variable?

A variable is a name that refers to an object stored in memory.

It allows us to store and access values in our programs.

```python
name = "Bhagavath"
age = 25
salary = 45000.50
```

Let's understand the assignment:

- `name`, `age`, and `salary` are variable names.
- `=` is the assignment operator.
- `"Bhagavath"` is a string object.
- `25` is an integer object.
- `45000.50` is a float object.

Python binds each variable name to its corresponding object. We don't need to declare variable types explicitly.

---

## 2. Multiple Variable Assignment

Python supports different ways of assigning values.

### A. Separate Assignment

```python
name = "Bhagavath"
age = 25
role = "Developer"
```

Each variable is assigned independently.

### B. Multiple Assignment

```python
name, age, role = "Bhagavath", 25, "Developer"

print(name)
print(age)
print(role)
```

**Output:**

```text
Bhagavath
25
Developer
```

Python unpacks the values on the right and assigns them in order.

| Variable | Value |
| --- | --- |
| name | "Bhagavath" |
| age | 25 |
| role | "Developer" |

### C. Same Value to Multiple Variables

```python
first = second = third = 100

print(first, second, third)
```

**Output:**

```text
100 100 100
```

All three names refer to the same integer object.

### D. Swapping Variables

Python allows us to swap values without a temporary variable.

```python
a = 10
b = 20

a, b = b, a

print(a)
print(b)
```

**Output:**

```text
20
10
```

Python evaluates the right-hand side first and then assigns the values to the left-hand variables.

---

## 3. Variable Naming Rules and Conventions

Meaningful variable names make code easier to understand and maintain.

### A. Valid Variable Names

```python
name = "Python"
user_name = "Developer"
age = 25
_total = 100
number1 = 10
```

### B. Invalid Variable Names

```python
# 1name = "Python"       # Cannot start with a number
# user-name = "Python"   # Hyphen is not allowed
# user name = "Python"   # Spaces are not allowed
# class = "Developer"    # Reserved keyword
```

**Naming Rules:**

| Rule | Explanation |
| --- | --- |
| Cannot start with a number | `1name` is invalid |
| No spaces | Use `_` instead |
| No hyphens | `-` is an operator |
| No reserved keywords | `class`, `if`, `for`, etc. |
| Case-sensitive | `name` and `Name` are different |

### C. Case Sensitivity

Python treats uppercase and lowercase names differently.

```python
name = "Bhagavath"
Name = "Python"
NAME = "Developer"

print(name)
print(Name)
print(NAME)
```

These are three separate variables.

### D. Naming Conventions

| Naming style | Example | Usage |
| --- | --- | --- |
| snake_case | `user_name` | Variables and functions |
| PascalCase | `UserProfile` | Classes |
| UPPER_CASE | `MAX_RETRIES` | Constants |
| _leading_underscore | `_internal_value` | Internal-use convention |

```python
user_name = "Bhagavath"

MAX_RETRIES = 3

class UserProfile:
    pass

def calculate_total():
    pass
```

**Important:**

- Python doesn't enforce constants. `MAX_RETRIES` can still be reassigned.
- A leading underscore indicates internal use; it doesn't make a variable truly private.

### E. Avoid Built-in Names

Avoid using existing built-in function names as variables.

Common built-ins:

`sum()`, `list()`, `str()`, `int()`, `type()`, `id()`, `max()`, `min()`.

**Bad:**

```python
list = [10, 20, 30]
```

**Better:**

```python
numbers = [10, 20, 30]
```

Using `list` as a variable shadows the built-in `list()` function in that scope.

---

## 4. Dynamic Typing in Detail

Python allows a variable name to refer to objects of different types during runtime.

### A. Reassigning Different Types

```python
value = 100

print(value)
print(type(value))

value = "Python"

print(value)
print(type(value))
```

**Output:**

```text
100
<class 'int'>
Python
<class 'str'>
```

**What happens?**

1. Initially, `value` refers to an integer object.
2. Python returns `int` when checking its type.
3. After reassignment, `value` refers to a string object.
4. Its type is now `str`.

Python doesn't convert the integer into a string. It simply rebinds the variable name to another object.

### B. Different Variables, Different Types

```python
name = "Python"
age = 25
price = 99.99
is_active = True

print(type(name))
print(type(age))
print(type(price))
print(type(is_active))
```

**Output:**

```text
<class 'str'>
<class 'int'>
<class 'float'>
<class 'bool'>
```

### C. Dynamic Typing vs Strong Typing

Dynamic typing allows reassignment to different types. Strong typing prevents incompatible types from being silently combined.

```python
number = 10
text = "20"

result = number + text
```

**Output:**

```text
TypeError
```

To perform the addition, explicitly convert the string:

```python
result = number + int(text)

print(result)
```

**Output:**

```text
30
```

Remember:

- **Dynamic typing:** A variable name can refer to objects of different types at runtime.
- **Strong typing:** Incompatible types aren't silently mixed.

---

## 5. `is` vs `==`

Both operators look similar, but they check different things.

- `==` checks whether two objects have equal values.
- `is` checks whether two names refer to the exact same object.

### A. Different Objects, Same Values

```python
first_list = [10, 20, 30]
second_list = [10, 20, 30]

print(first_list == second_list)
print(first_list is second_list)
```

**Output:**

```text
True
False
```

Both lists contain the same values, but they are separate objects.

### B. Same Object

```python
first_list = [10, 20, 30]
second_list = first_list

print(first_list == second_list)
print(first_list is second_list)
```

**Output:**

```text
True
True
```

Both names refer to the same list object.

| Operator | Purpose |
| --- | --- |
| `==` | Value equality |
| `is` | Object identity |

**Important:** Use `is None` when checking for `None`.

```python
if result is None:
    print("No result")
```

---

## 6. Comments and Documentation

Comments help explain code when additional information is needed.

### A. Single-Line Comments

Use `#` to write comments.

```python
# Store the user's name
name = "Bhagavath"

print(name)
```

Python ignores comments during execution.

### B. Inline Comments

```python
age = 25  # User's age
```

Use inline comments only when they add useful information.

### C. Multiline Comments

Python doesn't have a dedicated multiline comment syntax. Use multiple `#` lines.

```python
# Calculate the total price
# of the selected products.

price = 100
quantity = 3

total = price * quantity
print(total)
```

### D. Docstrings

Docstrings document modules, classes, and functions.

They are usually written using triple quotes.

```python
def calculate_total():
    """
    Calculates the total amount.
    """
    return 100
```

Access the documentation using `__doc__`.

```python
print(calculate_total.__doc__)
```

**Comments vs Docstrings**

| Comments | Docstrings |
| --- | --- |
| Explain code | Document modules, classes, and functions |
| Use `#` | Usually use triple quotes |
| Not stored as documentation objects | Accessible through `__doc__` |

---

## 7. Day 2 – Interview Questions

1. **What is a variable?**  
   A name that refers to an object stored in memory.

2. **Does Python require variable declarations?**  
   No. Values can be assigned directly without declaring their types.

3. **What is multiple assignment?**  
   Assigning values to multiple variables in one statement.

4. **How do you swap two variables?**  
   Using `a, b = b, a`.

5. **Is Python case-sensitive?**  
   Yes. `name`, `Name`, and `NAME` are different variables.

6. **What are Python naming conventions?**  
   `snake_case` for variables/functions, `PascalCase` for classes, and `UPPER_CASE` for constants.

7. **Can keywords be used as variable names?**  
   No. Keywords such as `class`, `if`, and `for` are reserved.

8. **What happens when we use a built-in name as a variable?**  
   It shadows the original built-in name in that scope.

9. **Difference between `is` and `==`?**  
   `==` checks values; `is` checks object identity.

10. **Difference between comments and docstrings?**  
    Comments explain code, while docstrings provide documentation for modules, classes, and functions.

---

## 8. Day 2 – Practice Tasks

1. Create variables for your name, age, salary, and role. Print their values and types.
2. Assign multiple variables in a single statement.
3. Assign the same value to three variables.
4. Swap two variables without using a temporary variable.
5. Identify valid and invalid variable names.
6. Reassign an integer variable with a string and check its type.
7. Try adding an integer and string. Fix the error using type conversion.
8. Compare two lists using `is` and `==`.
9. Write a program using single-line and inline comments.
10. Create a function with a docstring and access it using `__doc__`.

**Day 2 Completed: Variables, Naming Rules, Dynamic Typing, Object Identity, and Documentation.**

---

# Day 3 – Python Data Types and Type Conversion

## 1. Integer (`int`)

An integer represents a whole number without a decimal component.

Examples: `10`, `-50`, `100`, `0`.

```python
age = 25
temperature = -10
quantity = 100

print(type(age))
print(type(temperature))
print(type(quantity))
```

All three values are of type `int`.

### A. Arbitrary Precision

Python integers can grow beyond the usual 32-bit or 64-bit limits, depending mainly on available memory.

```python
big_number = 999999999999999999999999999999999999

print(big_number)
print(type(big_number))
```

Python handles large integers without fixed-width integer overflow.

### B. Integer Operations

```python
a = 20
b = 6

print(a + b)
print(a - b)
print(a * b)
print(a / b)
print(a // b)
print(a % b)
print(a ** b)
```

| Operator | Meaning | Example |
| --- | --- | --- |
| `+` | Addition | `20 + 6` |
| `-` | Subtraction | `20 - 6` |
| `*` | Multiplication | `20 * 6` |
| `/` | Regular division | `20 / 6` |
| `//` | Floor division | `20 // 6` |
| `%` | Remainder | `20 % 6` |
| `**` | Exponentiation | `20 ** 6` |

**Important: `/` vs `//`**

- `/` returns the regular division result.
- `//` rounds the result down toward negative infinity.

```python
print(20 / 6)   # 3.3333333333333335
print(20 // 6)  # 3

print(-7 // 2)  # -4
```

Notice that `-7 // 2` returns `-4`, not `-3`, because floor division rounds down toward negative infinity.

---

## 2. Floating-Point (`float`)

A float represents numbers using floating-point precision. It is commonly used for decimal values.

```python
price = 99.99
temperature = 36.5
percentage = 85.75
value = 10.0

print(type(price))
print(type(value))
```

Even `10.0` is a float because it contains a decimal point.

### A. Floating-Point Arithmetic

```python
a = 10.5
b = 2.5

print(a + b)
```

**Output:**

```text
13.0
```

### B. Floating-Point Precision Problem

One important concept in Python is floating-point precision.

```python
a = 0.1
b = 0.2

print(a + b)
```

**Output:**

```text
0.30000000000000004
```

Why?

Computers represent floating-point numbers using binary. Some decimal values, such as `0.1` and `0.2`, cannot be represented exactly in binary.

This can produce small precision differences during calculations.

### C. Comparing Float Values

Avoid directly comparing calculated float values using `==` when precision matters.

Python provides `math.isclose()`.

```python
import math

a = 0.1
b = 0.2

result = a + b

print(math.isclose(result, 0.3))
```

**Output:**

```text
True
```

It checks whether two numbers are sufficiently close within a specified tolerance.

### D. Decimal for Financial Calculations

For money-related calculations, use `decimal.Decimal` when exact decimal arithmetic is required.

```python
from decimal import Decimal

price = Decimal("0.10")
tax = Decimal("0.20")

total = price + tax

print(total)
```

**Output:**

```text
0.30
```

**Why use strings?**

`Decimal("0.10")` preserves the intended decimal value. Creating a Decimal directly from a float can carry the float's binary approximation.

Common uses:

- Payment systems.
- Billing.
- Financial reports.
- Currency calculations.

---

## 3. Complex (`complex`)

Python supports complex numbers containing real and imaginary components.

Mathematically:

\[
z = a + bj
\]

- `a` represents the real part.
- `b` represents the imaginary part.
- `j` represents the imaginary unit.

Python uses `j` instead of `i`.

```python
number = 3 + 5j

print(number)
print(type(number))
```

**Output:**

```text
(3+5j)
<class 'complex'>
```

### A. Accessing Components

```python
number = 3 + 5j

print(number.real)
print(number.imag)
```

**Output:**

```text
3.0
5.0
```

- `.real` returns the real component.
- `.imag` returns the imaginary component.

### B. Complex Number Operations

```python
a = 3 + 5j
b = 2 + 4j

print(a + b)
print(a - b)
print(a * b)
```

**Output:**

```text
(5+9j)
(1+1j)
(-14+22j)
```

Common applications include mathematics, electrical engineering, signal processing, and scientific computing.

---

## 4. Boolean (`bool`)

Boolean represents one of two logical values:

- `True`
- `False`

Python uses capital `T` and `F`.

```python
is_logged_in = True
is_admin = False

print(type(is_logged_in))
print(type(is_admin))
```

### A. Boolean Expressions

A Boolean expression evaluates to either `True` or `False`.

```python
age = 25

print(age > 18)
print(age < 18)
print(age == 25)
print(age != 25)
```

**Output:**

```text
True
False
True
False
```

Comparison operators:

| Operator | Meaning |
| --- | --- |
| `==` | Equal |
| `!=` | Not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

### B. Logical Operators

Python provides three main logical operators:

- `and` – Both conditions must be true.
- `or` – At least one condition must be true.
- `not` – Reverses the Boolean result.

```python
age = 25
has_license = True

print(age >= 18 and has_license)
print(age >= 18 or has_license)
print(not has_license)
```

**Output:**

```text
True
True
False
```

### C. Boolean Is a Subclass of Integer

This is a common interview question.

```python
print(isinstance(True, int))
print(isinstance(False, int))

print(True + True)
print(True + False)
print(False + False)
```

**Output:**

```text
True
True
2
1
0
```

Why?

Python's `bool` type is a subclass of `int`.

- `True` behaves like `1`.
- `False` behaves like `0`.

However, use Boolean values for logical conditions in application code.

### D. Truthy and Falsy Values

Python provides the `bool()` function to check the truth value of an object.

```python
print(bool(1))          # True
print(bool(0))          # False
print(bool(""))         # False
print(bool("Python"))   # True
print(bool([]))         # False
print(bool([10, 20]))   # True
```

**Common falsy values:**

- `False`
- `None`
- `0`
- `0.0`
- `""`
- `[]`
- `{}`
- `()`
- `set()`

Most other values are truthy.

Example:

```python
username = ""

if username:
    print("Username provided")
else:
    print("Username is empty")
```

Python automatically checks the truth value of `username`.

This is similar to JavaScript:

```javascript
if (username) {
    console.log("Username provided");
}
```

---

## 5. None (`NoneType`)

`None` represents the absence of a value. It is similar to `null` in JavaScript.

```python
result = None

print(result)
print(type(result))
```

**Output:**

```text
None
<class 'NoneType'>
```

### A. Why Do We Use None?

We use `None` to represent missing values, optional results, or values that haven't been provided.

```python
email = None

if email is None:
    print("Email is not provided")
```

### B. None vs False vs 0

These values are different.

```python
a = None
b = False
c = 0

print(a == b)
print(b == c)
print(a is None)
print(b is False)
```

**Output:**

```text
False
True
True
True
```

Important:

- `False == 0` returns `True`.
- `False is 0` returns `False`.
- `None` is different from both `False` and `0`.

Use `is None` when checking for `None`.

```python
if result is None:
    print("No result")
```

### C. Functions That Return None

If a function doesn't explicitly return a value, Python automatically returns `None`.

```python
def display_message():
    print("Hello Python")

result = display_message()

print(result)
print(type(result))
```

**Output:**

```text
Hello Python
None
<class 'NoneType'>
```

Remember the difference:

- `print()` displays a value.
- `return` sends a value back to the caller.

---

## 6. Type Conversion in Python

Type conversion means changing a value from one data type to another.

Python provides built-in conversion functions.

| Function | Purpose |
| --- | --- |
| `int()` | Convert to integer |
| `float()` | Convert to float |
| `str()` | Convert to string |
| `bool()` | Convert to Boolean |
| `complex()` | Convert to complex |

### A. Integer Conversion – `int()`

```python
value = "100"

result = int(value)

print(result)
print(type(result))
```

**Output:**

```text
100
<class 'int'>
```

Invalid conversion:

```python
value = "Python"
result = int(value)
```

This raises `ValueError` because `"Python"` cannot be converted into an integer.

**Important:** `int()` truncates the decimal portion toward zero. It doesn't round.

```python
print(int(10.9))   # 10
print(int(-10.9))  # -10
```

### B. Float Conversion – `float()`

```python
value = "99.50"

result = float(value)

print(result)
print(type(result))
```

**Output:**

```text
99.5
<class 'float'>
```

Integer to float:

```python
print(float(25))  # 25.0
```

### C. String Conversion – `str()`

```python
age = 25

result = str(age)

print(result)
print(type(result))
```

**Output:**

```text
25
<class 'str'>
```

Useful when constructing messages or preparing values for APIs.

```python
user_id = 101

message = "User ID: " + str(user_id)

print(message)
```

Python also supports f-strings:

```python
message = f"User ID: {user_id}"

print(message)
```

We'll explore f-strings in more detail on Day 4.

### D. Boolean Conversion – `bool()`

```python
print(bool(0))       # False
print(bool(1))       # True
print(bool(""))      # False
print(bool("Hello")) # True
print(bool(None))    # False
```

These conversions follow the truthy and falsy rules.

### E. Implicit vs Explicit Type Conversion

**Implicit Conversion**

Python automatically converts compatible values during certain operations.

```python
number = 10
price = 5.5

result = number + price

print(result)
print(type(result))
```

**Output:**

```text
15.5
<class 'float'>
```

Python promotes the integer to a float during the operation.

**Explicit Conversion**

The developer manually converts a value using functions such as `int()`, `float()`, and `str()`.

```python
age = "25"

converted_age = int(age)

print(converted_age)
print(type(converted_age))
```

**Output:**

```text
25
<class 'int'>
```

### Python vs JavaScript

| Operation | Python | JavaScript |
| --- | --- | --- |
| Integer conversion | `int("25")` | `parseInt("25")` |
| Float conversion | `float("25.5")` | `parseFloat("25.5")` |
| String conversion | `str(25)` | `String(25)` |
| Boolean conversion | `bool(1)` | `Boolean(1)` |
| Null-like value | `None` | `null` |

**Important difference:**

Python doesn't automatically concatenate strings and numbers using `+`.

```python
age = 25

# TypeError
# print("Age: " + age)

# Correct
print("Age: " + str(age))
```

---

## 7. Day 3 – Interview Questions

1. **What is an integer in Python?**  
   A whole number without a decimal component.

2. **What is arbitrary precision?**  
   Python integers can grow beyond the usual fixed-width integer limits, depending on available memory.

3. **Difference between `/` and `//`?**  
   `/` performs regular division, whereas `//` performs floor division.

4. **What is a float?**  
   A number represented using floating-point precision.

5. **Why does `0.1 + 0.2` produce a precision difference?**  
   Some decimal fractions cannot be represented exactly in binary floating-point.

6. **How do you compare floating-point values?**  
   Use `math.isclose()` when approximate equality is required.

7. **Why use Decimal?**  
   It provides decimal arithmetic suitable for financial calculations.

8. **What is a complex number?**  
   A number containing real and imaginary components.

9. **What is Boolean in Python?**  
   A data type representing `True` or `False`.

10. **Is bool a subclass of int?**  
    Yes. `True` behaves like `1` and `False` behaves like `0`.

11. **What are truthy and falsy values?**  
    Values that evaluate to `True` or `False` when used in conditions.

12. **What is None?**  
    It represents the absence of a value.

13. **Difference between None, False, and 0?**  
    They are different values, even though `False == 0` evaluates to `True`.

14. **What happens when a function has no return statement?**  
    It returns `None` automatically.

15. **What is type conversion?**  
    Changing a value from one data type to another.

16. **Difference between implicit and explicit conversion?**  
    Implicit conversion happens automatically; explicit conversion is performed manually by the developer.

---

## 8. Day 3 – Practice Tasks

### Task 1 – Integer Operations

- Create two integer variables.
- Perform addition, subtraction, multiplication, division, floor division, modulus, and exponentiation.
- Try floor division with negative numbers.

### Task 2 – Integer Precision

- Create a very large integer.
- Print its value and type.
- Understand how Python handles large integers.

### Task 3 – Floating-Point Precision

- Add `0.1` and `0.2`.
- Check the result.
- Use `math.isclose()` to compare it with `0.3`.

### Task 4 – Decimal

- Import `Decimal`.
- Create two decimal values using strings.
- Add them together.
- Compare the result with float arithmetic.

### Task 5 – Complex Numbers

- Create two complex numbers.
- Perform addition, subtraction, and multiplication.
- Print their real and imaginary components.

### Task 6 – Boolean Operations

- Create variables for age and license availability.
- Check driving eligibility using `and`, `or`, and `not`.
- Check the result types.

### Task 7 – Truthy and Falsy Values

```python
print(bool(0))
print(bool(-1))
print(bool(""))
print(bool("Python"))
print(bool([]))
print(bool([1]))
print(bool(None))
print(bool({}))
```

Identify which values are truthy and falsy.

### Task 8 – None

- Create a variable with `None`.
- Check its type.
- Compare `None`, `False`, and `0`.
- Write a condition using `is None`.

### Task 9 – Type Conversion

- Convert a string to an integer.
- Convert a string to a float.
- Convert an integer to a string.
- Convert a float to an integer.
- Try converting a non-numeric string into an integer.
- Understand the errors.

### Task 10 – Implicit vs Explicit Conversion

- Add an integer and a float.
- Check the result and its type.
- Convert a numeric string into an integer.
- Explain the difference between implicit and explicit conversion.

---

## 9. Day 3 – Important Points to Remember

- `int` represents whole numbers and supports arbitrary precision.
- `/` performs regular division; `//` performs floor division.
- `%` returns the remainder; `**` performs exponentiation.
- `float` represents numbers using floating-point precision.
- Floating-point calculations can have precision differences.
- Use `math.isclose()` for approximate float comparisons.
- Use `Decimal` for decimal-based financial calculations.
- `complex` contains real and imaginary components.
- Python uses `j` for imaginary numbers.
- `bool` represents `True` and `False`.
- Boolean is a subclass of integer.
- Truthy and falsy values are used in conditional expressions.
- `None` represents the absence of a value.
- Use `is None` to check for `None`.
- Functions without an explicit return value return `None`.
- Type conversion can be implicit or explicit.
- Common conversion functions are `int()`, `float()`, `str()`, `bool()`, and `complex()`.

**Day 3 Completed: Python Data Types and Type Conversion.**
