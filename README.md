# Functional Programming in JavaScript 20/80

## What is an application?

Twitter, Facebook, and Gmail are essentially lists of data (text or images).

Common skeleton:
- Base data: tweet, post, email
- A bunch of them in a list
- We can interact with them: e.g., retweet, like a post, reply to email

Most apps at their core are lists of data. To be good at building apps, you need to be good at working with:
- Data
- Lists of data
- Data Transformation

Apps present data in a meaningful and consumable way. They transform data into information and allow interaction with it. In essence, apps are data & transformations of data.

## JavaScript Basics

### A. Primitive types:
1. Strings: use `''` or `""`
2. Numbers: just key them
3. Booleans: type `true` or `false`

Variable Declaration and Assignment: 

- Use `const` for variables that won't be reassigned (preferred for functional programming).
- Use `let` for variables that will be reassigned.

Syntax:
```
const variableName = value;
let variableName = value;
```
You'll mostly use `const` since FP is about "Immutable Data".

### B. Complex Types:
We can combine primitive types into a new custom type using Object Literals. 

Grouping data:
Object Literals -> we'll use as records. One object = one record (row of data)

Syntax:
```
const objectName = {
  key1: value1,
  key2: value2,
  method: function() {
    // method body
  }
};
```

Access:
```
objectName.key
objectName["key"]
objectName.method()
```

Dates in JS: (using date object)
```javascript
date = new Date(2024, 5, 1);
```

### C. Collections: Arrays
- Arrays are zero-indexed.
- Use square brackets for declaration and access.

Syntax:
```
const arrayName = [element1, element2, ...];
const element = arrayName[index];
```

## Immutable Data

Data that never changes once created. You can use it to make new data, but you don't change it. Data that doesn't change is simple. Simple isn't easy, but it pays off in the long term.

`const` prevents "reassignment" but doesn't mean immutability.

### State Changes (functional style):

Add / Update / Delete Objects in an immutable way:

To **Add** a property: copy old then add new using spread operator (`...obj`)
```javascript
const obj = {
    property1: value1,
    property2: value2
};

const updatedObj = {
    ...obj,
    property3: value3
};
```

We can also use the same syntax to **update** existing properties:
```javascript
const updatedObj = {
    ...obj,
    property2: newValue2,
    property3: value3
};
```

To **delete** a property: destructuring + rest syntax:
```javascript
const { property_to_delete, ...updatedObjWithoutProperty } = updatedObject;
```

This extracts the `property_to_delete` by itself, and you can use `updatedObjWithoutProperty` as your new record without it.

Example:
```javascript
const meal = {
  description: 'Dinner',
};

// 1. In an Immutable way, add a property to the
// meal called calories setting its value to 200,
// then log the result to the console
const mealWithCalories = {
  ...meal,
  calories: 200
};
console.log(mealWithCalories);

// 2. In an Immutable way, increase the calories 
// by 100 and print the result to the console
const mealWithIncreasedCalories = {
  ...mealWithCalories,
  calories: mealWithCalories.calories + 100,
};
console.log(mealWithIncreasedCalories);

// 3. In an Immutable way, remove the calories property and log the result to the console
const {calories, ...mealWithOutCalories} = mealWithIncreasedCalories;
console.log(mealWithOutCalories);
```

### Add / Update / Delete from Arrays in immutable way:

- **Add** item to array: use spread syntax (`...`) same as with objects
- **Update** item: using `array.map(fn)`
- **Delete** item: `array.filter(fn)`

Example:
```javascript
// Initial array
const fruits = ['apple', 'banana', 'orange'];

// Adding an item to the array using spread syntax
const addedFruits = [...fruits, 'grape'];
console.log(addedFruits); // Output: ['apple', 'banana', 'orange', 'grape']

// Updating an item using array.map()
const updatedFruits = fruits.map(fruit => {
  if (fruit === 'banana') {
    return 'mango';
  }
  return fruit;
});
console.log(updatedFruits); // Output: ['apple', 'mango', 'orange']

// Deleting an item using array.filter()
const filteredFruits = fruits.filter(fruit => fruit !== 'orange');
console.log(filteredFruits); // Output: ['apple', 'banana']
```

## Summarizing information in arrays using Array.reduce()

The `reduce()` method in JavaScript is a powerful tool for processing arrays. It allows you to take an array and reduce it to a single value.

### Basic Syntax

```javascript
array.reduce(reducerFunction, initialValue)
```

Where:
- `reducerFunction` is a function that gets called for each element in the array
- `initialValue` is the starting value (optional)

### The Reducer Function

The reducer function takes four parameters:
1. accumulator: The running result
2. currentValue: The current element being processed
3. currentIndex: The index of the current element (optional)
4. array: The original array (optional)

### Examples

#### 1. Sum of Numbers

```javascript
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // Output: 15
```

Explanation:
- We start with 0 (our initialValue)
- For each number, we add it to our running total (acc)
- The final result is the sum of all numbers

#### 2. Counting Occurrences

```javascript
const fruits = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple'];
const fruitCount = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
console.log(fruitCount);
// Output: { apple: 3, banana: 2, orange: 1 }
```

Explanation:
- We start with an empty object {}
- For each fruit, we increment its count in the object
- The final result is an object with fruit names as keys and their counts as values

#### 3. Grouping Reviews by Score

```javascript
const reviews = [4.5, 4.0, 5.0, 2.0, 1.0, 5.0, 3.0, 4.0, 1.0, 5.0, 4.5, 3.0, 2.5, 2.0];
const groupedReviews = reviews.reduce((acc, score) => {
  acc[score] = (acc[score] || 0) + 1;
  return acc;
}, {});
console.log(groupedReviews);
// Output: { '1': 2, '2': 2, '2.5': 1, '3': 2, '4': 2, '4.5': 2, '5': 3 }
```

Explanation:
- We start with an empty object {}
- For each review score, we increment its count in the object
- The final result is an object with scores as keys and their frequencies as values

### Key Points to Remember

1. If no initialValue is provided, reduce() uses the first element as the initial accumulator value.
2. reduce() is versatile - it can produce a number, string, object, or even another array.
3. It's great for summarizing data or transforming arrays into other data structures.

## Control Flow

### If-Else
Syntax:
```
if (condition) {
  // code block
} else if (anotherCondition) {
  // code block
} else {
  // code block
}
```

### Ternary Operator
Syntax:
```
condition ? expressionIfTrue : expressionIfFalse
```
returns a value based on condition

### Switch Statement

```javascript
switch (expression) {
  case value1:
    // Code to execute if expression === value1
    break;
  case value2:
    // Code to execute if expression === value2
    break;
  // More cases...
  default:
    // Code to execute if no case matches
}
```

Key points:
1. The `expression` is evaluated once.
2. Its value is compared with the values in each `case`.
3. If there's a match, the associated block of code is executed.
4. The `break` statement exits the switch block.
5. `default` is optional and runs if no case matches.

**Here's when you might use each:**

- Use if-else when you have complex conditions or need to execute multiple lines of code based on conditions.
- Use the ternary operator for simple, one-line conditional assignments or returns.
- Use switch when you're comparing a single value against multiple possible discrete values.

Let's create a simple function that returns a greeting based on the time of day: "Good morning" for morning, "Good afternoon" for afternoon, and "Good evening" for evening.


Here's how we could implement this using all three methods:

1. Using if-else:

```javascript
function getGreeting(hour) {
    if (hour < 12) {
        return "Good morning";
    } else if (hour < 18) {
        return "Good afternoon";
    } else {
        return "Good evening";
    }
}
```

2. Using ternary operator:

```javascript
function getGreeting(hour) {
    return hour < 12 ? "Good morning" 
         : hour < 18 ? "Good afternoon" 
         : "Good evening";
}
```

3. Using switch statement:

```javascript
function getGreeting(hour) {
    switch (true) {
        case hour < 12:
            return "Good morning";
        case hour < 18:
            return "Good afternoon";
        default:
            return "Good evening";
    }
}
```

## Functions: Currying, Closure, Partial Application

### Function Declaration

**Function Statement**
Syntax:
```
function functionName(parameter1, parameter2, ...) {
  // function body
  return value; // optional
}
```

**Function Expression**
Syntax:
```
const functionName = function(parameter1, parameter2, ...) {
  // function body
  return value; // optional
};
```
**Arrow Function**

Syntax for multi-line function:
```
const functionName = (parameter1, parameter2, ...) => {
  // function body
  return value; // optional
};
```

Syntax for single-expression function:
```
const functionName = (parameter1, parameter2, ...) => expression;
```

### Currying:
Currying is the process of transforming a function that takes multiple arguments into a sequence of functions, each taking a single argument. It allows you to partially apply a function by fixing some of its arguments.

Example:
```javascript
function multiply(a) {
  return function(b) {
    return a * b;
  };
}

const double = multiply(2);
console.log(double(5)); // Output: 10
```

### Partial Application:
Partial application is the process of fixing some arguments of a function, creating a new function with fewer arguments. It's like creating a specialized version of the function.

Example:
```javascript
function greet(greeting, name) {
  return `${greeting}, ${name}!`;
}

const sayHello = greet.bind(null, 'Hello');
console.log(sayHello('John')); // Output: Hello, John!
```

Currying is done when creating a function, with no data. Partial application is when we use a function with data.

We can do partial application without currying:

```javascript
function multiply(a, b) {
  return a * b;
}

const multiplyByFive = multiply.bind(null, 5);

console.log(multiplyByFive(3)); // Output: 15
console.log(multiplyByFive(7)); // Output: 35
```

### Currying with Ramda:

```javascript
const curriedAdd = R.curry((x, y) => x + y);
```

### Closure:
A closure is a function that has access to variables in its outer (enclosing) lexical scope, even after the outer function has returned. It allows a function to "remember" and access variables from its outer scope.

Example:
```javascript
function outerFunction(x) {
  return function innerFunction(y) {
    return x + y;
  };
}

const addFive = outerFunction(5);
console.log(addFive(3)); // Output: 8
```

## (Pure) Functions vs (Impure) Procedures

Functions create and return a value based on input and cause NO side effects.

(Pure) Function rules:
1. Must have input parameters
2. Must NOT rely on stateful values (not rely on values outside themselves that could change over time)
3. Return value determined only by its input parameters
4. Must NOT cause side effects (a change outside itself)

Contrast these two:

```javascript
function add(x, y) {
    return x + y; 
}
// has all 4 rules

let counter = 0;
function increment() {
    counter++; 
}
// breaks all 4 rules so it's a procedure
```

Why use (Pure) Functions?
- Reusable
- Composable
- Easy to test (just check output)
- Easy to cache expensive function calls

FP doesn't say NO state, that would be impractical. FP says use as little state as possible and if state is necessary, tightly control it.

## Composing functions using Ramda R.pipe:

Syntax:
```javascript
const composedFn = R.pipe(f1, f2, f3);
```

## Loops

### For Loop
Syntax:
```
for (initialization; condition; update) {
  // code block
}
```

### While Loop
Syntax:
```
while (condition) {
  // code block
}
```

## Ramda:
Importing Ramda:
```javascript
import * as R from 'ramda';
```

Now we can access Ramda functions as `R.funcName`

### R.pipe:
`R.pipe` is a function from the Ramda library that allows you to compose functions together in a left-to-right manner.

```javascript
const R = require('ramda');

const addOne = x => x + 1;
const double = x => x * 2;

const addOneAndDouble = R.pipe(
  addOne,
  double
);

console.log(addOneAndDouble(3)); // Output: 8
```

## Other Useful JavaScript Functions

### parseFloat:
A built-in JavaScript function that parses a string and returns the first floating-point number encountered in the string.

```javascript
console.log(parseFloat("3.14")); // Output: 3.14
console.log(parseFloat("-2.71")); // Output: -2.71
console.log(parseFloat("1.23abc")); // Output: 1.23
console.log(parseFloat("abc1.23")); // Output: NaN
```

### number.toFixed:
Used to round a number to a specified number of decimal places.

## Template Literals

Syntax:
```
`String with ${variableName} or ${expression}`
```

## Extra bits not in FPJS

## Modules (for Organizing Code)

Modules in JavaScript allow you to organize your code into separate files, making it more manageable and reusable. They help in creating a clear structure for your projects and enable you to encapsulate related functionality.

### Exporting

There are two main ways to export from a module:

1. Named exports: Allow you to export multiple values from a module.
2. Default export: Allows you to export a single main value from a module.

Syntax

```
export const functionName = () => { ... };
export default mainFunction;
```

Example

```javascript
// myModule.js

// Named exports
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// Default export
export default function multiply(a, b) {
  return a * b;
}

// You can also export variables
export const PI = 3.14159;
```

### Importing

When importing, you can use named imports, default imports, or a combination of both.

Syntax

Importing:
```
import { functionName } from './module';
import mainFunction from './module';
```
Example

```javascript
// main.js

// Named imports
import { add, subtract, PI } from './myModule';

// Default import
import multiply from './myModule';

// Combining named and default imports
import multiply, { add, subtract } from './myModule';

// Importing all exports as an object
import * as math from './myModule';

console.log(add(5, 3));        // Output: 8
console.log(subtract(10, 4));  // Output: 6
console.log(multiply(2, 3));   // Output: 6
console.log(PI);               // Output: 3.14159

console.log(math.add(2, 2));   // Output: 4
```

## Promises and Async/Await (for Asynchronous Operations)

Promises and async/await are used to handle asynchronous operations in JavaScript, making it easier to work with tasks that don't complete immediately (like API calls or file operations).

### Promises

Promises represent a value that may not be available immediately but will be resolved at some point in the future. They have three states: pending, fulfilled, or rejected.

Promise syntax:
```
const promiseName = new Promise((resolve, reject) => {
  // asynchronous operation
  if (/* operation successful */) {
    resolve(value);
  } else {
    reject(error);
  }
});
```

Example

```javascript
function fetchData(url) {
  return new Promise((resolve, reject) => {
    fetch(url)
      .then(response => response.json())
      .then(data => resolve(data))
      .catch(error => reject(error));
  });
}

fetchData('https://api.example.com/data')
  .then(data => console.log('Data:', data))
  .catch(error => console.error('Error:', error));
```

### Async/Await

Async/await is syntactic sugar built on top of Promises, making asynchronous code look and behave more like synchronous code.

Async/Await syntax:

```
async function functionName() {
  try {
    const result = await asyncOperation();
    // handle result
  } catch (error) {
    // handle error
  }
}
```

Example

```javascript
async function fetchUserData(userId) {
  try {
    const response = await fetch(`https://api.example.com/users/${userId}`);
    const userData = await response.json();
    console.log('User data:', userData);
    return userData;
  } catch (error) {
    console.error('Error fetching user data:', error);
    throw error;
  }
}

// Usage
fetchUserData(123)
  .then(data => {
    // Process the data
  })
  .catch(error => {
    // Handle any errors
  });
```

## Classes (for Object-Oriented Programming)

Classes in JavaScript provide a way to create objects with shared properties and methods, enabling object-oriented programming patterns.

Syntax:
```
class ClassName {
  constructor(param1, param2) {
    this.property1 = param1;
    this.property2 = param2;
  }

  methodName() {
    // method body
  }
}
```

Usage:
```
const instance = new ClassName(arg1, arg2);
```

Example

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  sayHello() {
    console.log(`Hello, my name is ${this.name} and I'm ${this.age} years old.`);
  }

  // Static method
  static isAdult(age) {
    return age >= 18;
  }

  // Getter
  get birthYear() {
    const currentYear = new Date().getFullYear();
    return currentYear - this.age;
  }

  // Setter
  set birthYear(year) {
    const currentYear = new Date().getFullYear();
    this.age = currentYear - year;
  }
}

// Usage
const john = new Person('John', 30);
john.sayHello();  // Output: Hello, my name is John and I'm 30 years old.

console.log(Person.isAdult(20));  // Output: true

console.log(john.birthYear);  // Output: 1994 (assuming current year is 2024)

john.birthYear = 1990;
console.log(john.age);  // Output: 34 (assuming current year is 2024)

// Inheritance
class Employee extends Person {
  constructor(name, age, position) {
    super(name, age);  // Call the parent constructor
    this.position = position;
  }

  introduce() {
    console.log(`I'm ${this.name}, a ${this.position} at our company.`);
  }
}

const jane = new Employee('Jane', 28, 'Developer');
jane.sayHello();   // Output: Hello, my name is Jane and I'm 28 years old.
jane.introduce();  // Output: I'm Jane, a Developer at our company.
```

## Imperative vs Declarative Programming

- Imperative: A program describes how to do something: do x then y then z
- Declarative (functional): Describes "What" to do using functions: move then draw then ...

Analogy:
- Imperative: Giving step-by-step detailed directions from where you are to my house
- Declarative: Giving you the address

