---
title: Understanding currying in JavaScript
description: A simple explanation of how currying in javascript works.
tags: javascript redux
---

I've had a theoretical understanding of currying for some time but have struggled to think of real-world applications
for the concept.

Currying is a functional programming concept. It describes turning a function which accepts multiple arguments into a
series of functions which each accept some of the arguments.

A simple example would be a function which adds two numbers:

```js
function add(a, b) {
  return a + b;
}

add(1, 2); // -> 3
```

Currying this function would result in the following:

```js
function add(a) {
  return function (b) {
    return a + b;
  }
}

add(1)(2); // -> 3

const add1 = add(1);
add1(2); // -> 3
add1(3); // -> 4
```

In the second example the function is also used to create a reusable function, `add1`. This is made possible because
the first function creates a closure when it is called, ensuring that the inner function maintains access to the outer
function's properties.

ES2015 arrow functions let us write curried functions much more succinctly, although they can look quite confusing 
until you break them down.

```js
// step 1: transformed to arrow functions
const add = (a) => {
  return (b) => {
    return a + b;
  }
}

// step 2: this can be further shortened. 
// the explicit return statements are not necessary
const add = (a) => (b) => {
  return a + b;
}

// step 3: and again
const add = (a) => (b) => (a + b);
```

I hadn't come across a real-world use for currying until I started using Redux in my React applications.

Redux enforces functional programming concepts, such as the requirement for reducers to be pure functions.

Redux itself can be extended with custom functionality in the form of middleware. These are functions which perform
additional actions when the Redux store is in use. Common examples include logging and functions that handle 
asynchronous logic such as [redux-promise](https://www.npmjs.com/package/redux-promise).

API compliant middleware functions are written as curried functions. More examples can be found in the official 
[API docs](https://redux.js.org/docs/advanced/Middleware.html), where you'll see the arrow function cascade explained
above put to better use.

