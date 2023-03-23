---
title: "Deep Dive into Asynchronous JS: Callbacks, Promises & Async-Await"
datePublished: Mon Mar 20 2023 14:00:59 GMT+0000 (Coordinated Universal Time)
cuid: clfgwa4bt001409mo6s5d8ubv
slug: deep-dive-into-asynchronous-js-callbacks-promises-async-await
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/sAGaysaOrHA/upload/d023e3676f9a17a222c61c00cdb3ee0e.jpeg
tags: javascript, promises, callbacks, asyncawait, asynchronous-javascript

---

Hello and welcome to my blog post about callbacks, promises, and async-await!

If you're new to JavaScript programming, or even if you've been working with the language for a while, you may have heard these terms thrown around. They are all related to handling asynchronous code in JavaScript, which is code that runs independently of the main thread and can take some time to complete.

Callbacks were the original way to handle asynchronous code in JavaScript. They involve passing a function as an argument and calling it later. Promises were introduced as a solution to Callback Hell. They return an object that represents the eventual completion or failure of a task. Async-await was introduced in ES2017 to write asynchronous code that looks and behaves more like synchronous code. It helps avoid the potential pitfalls of callbacks and promises.

In this blog post, we'll take a closer look at each of these concepts and explore how they can be used to write clean, readable, and efficient JavaScript code. So, sit back, relax, and let's dive into the world of callbacks, promises, and async-await!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679302007387/36294331-a7e7-4c9f-a951-a6936f2a2c3c.gif align="center")

## Why and What are these Callbacks?

We all know that JavaScript is a synchronous language i.e. code is executed line by line. But, some tasks in JS take longer time to execute. This synchronous behavior can sometimes cause problems if a task takes a long time to complete. Because the program will be blocked until that task is finished. To overcome this, there comes asynchronous behavior.

Callbacks are a way to handle asynchronous behavior in JavaScript with asynchronous code. Asynchronous code is code that runs independently of the main thread and can take some time to complete. For example, making an API request, reading a file, or waiting for a user to click a button. These tasks can take some time.

> Any function that is passed as an argument to another function so that it can be executed in that other function is called a **callback function**.

In JavaScript, functions can be passed as arguments to other functions. A callback is simply a function that is passed as an argument to another function and gets executed after the main function has completed its task. Let's understand callback functions from the examples.

Here's an example of a simple callback:

```javascript
function greet(name, callback) {
  console.log(`Hello, ${name}!`);
  callback();
}
function sayBye() {
  console.log('Goodbye!');
}
greet('John', sayBye);
// Output:
// Hello, John!
// Goodbye!
```

In this example, the `greet` function takes a name argument and a callback argument. It logs a greeting to the console "Hello, name" and then calls the callback function. The `sayBye` function is defined separately and logs 'Goodbye!' to the console. When we call the `greet('John', sayBye)`, it logs 'Hello, John!' and then calls the `sayBye` function, which logs 'Goodbye!'. The final output becomes 'Hello, John!' 'Goodbye!'.

Another example of a callback is using `setTimeout`:

```javascript
console.log('Before setTimeout');
setTimeout(function() {
  console.log('Inside setTimeout');
}, 2000);
console.log('After setTimeout');
// Output:
//Before setTimeout
// After setTimeOut
// Inside setTimeout : after delay of 2 sec
```

In this example, we use the `setTimeout` function to delay the execution of a callback function for 2 seconds. The first console.log statement is executed immediately, then the `setTimeout` function is called with a callback function as its argument. The callback function logs 'Inside setTimeout' to the console after a 2-second delay. Finally, the last console.log statement is executed. The final output becomes: 'Before setTimeout' 'After setTimeout' 'Inside setTimeout'.

The callback function enables us to do async programming in JS. We use this for some functions that are interdependent for execution. For example, orders can be done after adding items to the cart. So, we pass callback functions as arguments to functions, which then these functions call the callback function that has been passed.

### Drawbacks of the Callback Functions

Callbacks can be powerful and useful tools for handling asynchronous code in JavaScript. However, as code becomes more complex, managing callbacks can become difficult and lead to the issue of "Callback Hell" also known as the "*Pyramid of Doom*". Let's see these drawbacks in more detail.

1. **Callback Hell**
    
    When the callback function is kept inside another function, which in turn is kept inside another function, (in short a lot of nested callbacks).
    
    Let's understand this scenario with a real-life dummy example.
    
    ```javascript
    const cart = ['item1', 'item2', 'item3']; // array of items
    createOrder(cart, function (){
        proceedPayment( function (){
            showOrderSummary( function(){   
                updateWallet()
            })
        })
    })
    ```
    
    In the above dummy example, all the functions are interdependent. `proceedPayment` should work only after the order is created by `createOrder`. `showOrderSummary` should only happen after the payment is done by `proceedPayment`. `updateWallet` should only work after we get the order summary from `showOrderSummary`. Each function is taking another function as a callback.
    
    This causes a *pyramid of doom* structure causing our code to grow horizontally, making it tough to manage our code.
    
2. **Inversion of Control**
    
    This happens when control of the program is no longer in our(programmer's) hands.
    
    In nested functions, see the above example, one API calls the callback function that has been received, but we don't know how the code is written inside that API and how it will affect our code. Will our callback function (cb) be called or not? What if it is called twice? What if it has bugs inside it? We have given control of our code to the other code.
    
    This is one of the drawbacks of callback functions, where we cannot control our callback functions when to execute, and how to execute.
    

To overcome these drawbacks, this is where promises and async-await come into the picture to provide a more structured and readable way to handle async code. Let's go to the promises!

## What is a Promise in JS?

We hear people make promises all the time. That your aunt who promised to give you some chocolates after the work is finished, a kid promising to not touch the cookie jar again without permission...but promises in JavaScript are slightly different.

Promises are an essential feature in JavaScript that enables us to write asynchronous code in a more readable and manageable way. Promises provide a way to handle asynchronous operations in a structured and organized manner, making it easier to maintain and scale codebases.

There are three main aspects of promises.

1. Creating a promise
    
2. Handling a promise
    
3. Chaining of promises
    

Let's understand these aspects in more detail.

### Creating a Promise

> A **promise** is an object representing the eventual completion or failure of an asynchronous operation.

A promise is a special type of object in JavaScript that represents a value that may not be available yet.

The constructor syntax of the Promise object is:

```javascript
let myPromise = new Promise(function(resolve, reject) {
  //code
});
```

Promise takes a single callback function as an argument. That callback function takes two arguments that are `resolve` and `reject`.

* `resolve` - If the request is successful promise will be resolved using the resolve() method and the state is changed to fulfilled.
    
* `reject` - If there is an error promise is rejected using the reject() method and the state is rejected.
    

The promise object returned by the `new Promise` constructor has these internal properties:

* `state` — initially `"pending"`, then changes to either `"fulfilled"` when `resolve` is called or `"rejected"` when `reject` is called.
    
* `result` — initially `undefined`, then changes to `value` when `resolve(value)` is called or `error` when `reject(error)` is called.
    

So eventually the promise moves to one of these states:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679312702100/eeb28e17-04f1-45b5-9f1a-a5ca976d7ccc.png align="center")

Here's an example of creating a promise in JavaScript:

```javascript
const conditionFulfilled= true;
let myPromise = new Promise(function (resolve, reject) {
    if (conditionFulfilled) {
        resolve("Promise Resolved");
    } else {
        reject("Promise Rejected");
    }
});
console.log(myPromise)
//output: Promise Resolved
```

When the condition is fulfilled, the promise is resolved, and if not promise is rejected.

### Handling a Promise

At this point, we know that, in JavaScript, a Promise is an object that represents a value that may not be available yet, but will be resolved or rejected at some point in the future.

To handle these resolved or rejected values, there are two methods: `.then` and `.catch`.

* `.then` - This method takes a callback function as its argument. This callback function will be called when the Promise is fulfilled with a value.
    
* `.catch` - This method also takes a callback function as its argument, which will be called when the Promise is rejected with an error.
    

Let's understand it with an example. Imagine you want to fetch some data from a server using JavaScript, but the request may take some time to complete. To handle this asynchronous operation, you can use a Promise.

```javascript
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => {
    const data = { name: "John", age: 25 };
    resolve(data);
  }, 2000);
});

fetchData
  .then((data) => {
    console.log("Data received:", data);
  })
  .catch((error) => {
    console.log("Error:", error);
  });
```

Now that we have our Promise, we can use `.then()` and `.catch()` methods to handle the result of the operation.

In this code, we call the `.then()` method on our `fetchData` Promise. The `.then()` method takes a callback function that is called when the operation is successful. The callback function receives the data object as a parameter.

In this example, we simply log the data to the console. However, you could use the data to update your web page, send it to another server, or do something else with it.

If the operation fails, the `.catch()` method is called. The `.catch()` method takes a callback function that is called when the operation fails. The callback function receives the error object as a parameter.

In our example, we simply log the error to the console. However, you could also display an error message to the user or take some other action to handle the error.

> A promise is said to be *settled* if it is either fulfilled or rejected, but not pending.

### Chaining of Promises

Chaining of Promises with `.then()` and `.catch()` is a powerful way to handle multiple asynchronous operations in JavaScript. This is the way we can get rid of Callback Hell. Let me show you an example to explain this concept in more detail. First, check out the example of the callback hell.

Now the same example with promises can be written as:

```javascript
createOrder(cart)
.then(() => proceedPayment())
.then(() => ShowOrderSummary())
.then(() => updateWallet())
.catch((err) => console.log(err));
```

Here, you can see promises can efficiently handle interdependent operations. The code is clean and easy to understand. So, this is how you can chain Promises using `.then()` and `.catch()` to handle multiple asynchronous operations in JavaScript. This technique is widely used in web development to fetch data from multiple servers or perform complex operations that require multiple steps.

**Some tips to remember while using chaining promises:**

1. `.catch` always catches all errors in its above chain.
    
2. The `.then` written after `.catch` will be executed always no matter if the promise is resolved or rejected.
    
3. Always return a promise from the inner chain (i.e. from each `.then`) to keep the flow of the data in the chain.
    
4. Always put a `.catch` statement in your chain to make sure all errors are being handled. i.e. Always terminate the chain with a catch.
    

The thing is, chaining promises together just like callbacks can get pretty bulky and confusing. That's why Async and Await were brought about. Now, sit back, relax, and let's go to our final destination of async-await!

## Why and What is async-await?

> **Async-await** is syntactic sugar for Promises.

Async/await is a feature in modern programming languages, such as JavaScript, that allows developers to write asynchronous code in a synchronous-like way. It was introduced to make it easier to write and manage asynchronous code, which is often used to perform tasks that may take a long time, such as reading data from a file or making an HTTP request.

Before async/await, developers had to use callbacks or Promises to handle asynchronous code. This approach could quickly become complex and difficult to manage when dealing with complex asynchronous operations or multiple asynchronous calls.

Let's see what are these `async` and `await` keywords used for.

* `async` - It ensures that the function returns a promise, and wraps non-promises in it.
    
* `await` - It makes JavaScript wait until that promise settles and returns its result.
    

**Note:** `await` keyword works only inside `async` functions

Let's see the same example we saw for handling promises using `async-await` :

```javascript
const fetchData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const data = { name: "John", age: 25 };
      resolve(data);
    }, 2000);
  });
};

const getData = async () => {
  try {
    const data = await fetchData();
    console.log("Data received:", data);
  } catch (error) {
    console.log("Error:", error);
  }
};

getData();
```

In this code, we first define the `fetchData` function as a promise that resolves after a delay of 2 seconds. Then we define an `async` function called `getData` that calls `fetchData` using the `await` keyword to pause execution until the promise is resolved.

If the promise is resolved, the data is logged into the console. If the promise is rejected, the error is caught and logged to the console using the `catch` method. Finally, we call `getData` to initiate the asynchronous operation.

By using async/await, developers can write asynchronous code that is easier to read, write, and debug, making it a valuable addition to modern programming languages.

Finally!

Doneee!!!!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1679319418748/883693a2-c31c-4b73-8fb6-e9a1928272b7.gif align="center")

## Summary

Today we discussed the concepts of callbacks, promises, and async-await in JavaScript. Understood why these concepts came into the picture and how to use them. Also tried to highlight the drawbacks of using callbacks, such as Callback Hell and Inversion of Control, and explain how promises and async-await can overcome these drawbacks. Finally, discussed some examples and tips for using promises and async-await to write clean, readable, and efficient JavaScript code.

That wraps up our deep dive into Asynchronous JavaScript! We've covered the major components of async javascript with the help of an example.  
I hope this will help you to understand the concepts.  
And I want to thank you for being patient with my writing and reading the article till the end! 🙌

## References

* [MDN Web Docs for promises and callbacks](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
    
* [JavaScript.info for async-await](https://javascript.info/async-await)
    

---

If you got to learn something new today, then do spread the word about the article. I would be very grateful!  
If you have any questions or points to add, comment away or drop me a DM on [**Twitter**](https://twitter.com/pranita0709). I'd love to connect :)

That's it for today!