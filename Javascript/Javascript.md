# JavaScript Basics 

JavaScript is a **dynamic**, **interpreted**, high-level programming language.
It was originally created for browsers to make web pages interactive.

Now, thanks to **Node.js**, JavaScript can also run outside the browser, using the **V8 engine** written in C++.

---

## 🧠 JavaScript Characteristics

* **Dynamic typing**: Variable types can change at runtime
* **Interpreted**: Code runs line by line, no compilation needed
* **Single-threaded**, but can perform async operations via the event loop

---

## 🔡 Declaring Variables

JavaScript provides 3 ways to declare variables:

var a = 1;        // Function-scoped or global
let b = 2;        // Block-scoped
const c = 3;      // Block-scoped and cannot be reassigned

### Comparison Table

| Keyword | Scope    | Reassignment | Hoisted | Notes                     |
| ------- | -------- | ------------ | ------- | ------------------------- |
| var     | Function | Yes          | Yes     | Use only in old code      |
| let     | Block    | Yes          | No      | Recommended for variables |
| const   | Block    | No           | No      | Recommended for constants |

---

## 🔠 Types in JavaScript

JavaScript has **2 categories** of types:

### Primitive Types

* number → 1, 3.14
* string → 'hello', "world"
* boolean → true, false
* undefined → variable declared but not assigned
* null → intentional absence of value
* bigint → very large integers, e.g., 123n
* symbol → unique values (used rarely)

Example:

let age = 42;
let name = 'Oogway';
let isTurtle = true;
let x; // undefined
let y = null;

---

### Reference Types

* Object
* Array
* Function
* Date, RegExp, etc.

---

## 🧱 Objects

Objects store key-value pairs.

let person = {
 name: 'Oogway',
 age: 500
};

### Accessing Properties

console.log(person.name);        // Dot notation (preferred)
console.log(person\['age']);      // Bracket notation

> Use bracket notation when the property name is dynamic.

---

## 🧾 Arrays

Arrays are ordered collections of values.

let colors = \['red', 'blue'];

Arrays can hold mixed types:

let mixed = \['green', 10, false];

Accessing elements:

console.log(colors\[0]);   // 'red'

Length of array:

console.log(colors.length);  // 2

Note: typeof colors → 'object'

---

## 🔁 Functions

### Basic function

function greet() {
 console.log('Hello!');
}

greet(); // Call the function

### Function with return value

function square(number) {
 return number \* number;
}

console.log(square(4)); // 16

---

## ✅ Summary

* JavaScript runs in browsers and on servers (via Node.js)
* Use `let` and `const` for safer code
* Know primitive vs reference types
* Practice using objects, arrays, and functions

---
