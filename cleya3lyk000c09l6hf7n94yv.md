---
title: "Inside JavaScript's Runtime: Callstack, Event Loop, Callback and Job Queues"
datePublished: Tue Mar 07 2023 13:20:13 GMT+0000 (Coordinated Universal Time)
cuid: cleya3lyk000c09l6hf7n94yv
slug: inside-javascripts-runtime-callstack-event-loop-callback-and-job-queues
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Iqi0Rm6gBkQ/upload/a209d253896b2186d83fe7039e446789.jpeg
tags: javascript, event-loop, callstack, callbackqueue, javascript-runtime

---

## Overview

In this article, let's deep dive into how javascript works under the hood, how it executes our asynchronous javascript code, and in what order, how it generates stack trace, and much more...

Let me break it down for you. When you write JavaScript code, it needs to be executed by the computer. The JavaScript engine in your browser or server does the job of executing your code. Inside the JavaScript engine, there's a runtime environment that handles the execution of your code. In this runtime environment, we can execute synchronous as well as asynchronous javascript code. This runtime environment is composed of a few important components, including the call stack, event loop, callback queue, and job queue. Let's get to know them *haule haule*.

## Synchronous VS Asynchronous

You must be confused about what is synchronous and asynchronous behavior of JavaScript. So, let's understand it first.

1. **Synchronous behavior** means that the code is executed one line at a time, in the order that it appears. Each line of code must finish executing before the next line can start. This synchronous behavior can sometimes cause problems if a task takes a long time to complete. Because the program will be blocked until that task is finished. To overcome this, there comes asynchronous behavior.
    
2. **Asynchronous behavior** means that the code can continue executing while a task is being completed. Unlike synchronous, asynchronous code doesn't block the execution of the rest of your program. While the asynchronous task is being executed, the rest of your code can continue to run. This is useful for tasks that take a long time to complete, like fetching data from a server or reading a large file.
    

Here's an example to help you understand asynchronous code in JavaScript:

Imagine you're fetching data from a server. If you were to do this synchronously, your program would stop executing while it waits for the data to be fetched. This could cause your program to become unresponsive, especially if the server takes a long time to respond. With asynchronous code, you can fetch the data in the background, and once it's complete, you can handle the data in a callback function. This way, the rest of your program can continue to execute while the data is being fetched.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678180020196/42be20d2-ddcf-4cd3-955f-1c13cd4b1a26.png align="center")

## JavaScript's Runtime Environment

Now, let's understand how our code works under the hood. When we write JavaScript code, it needs to be executed by the computer. The JavaScript engine present in our browser or server does the job of executing our code. There is a runtime environment inside the Javascript engine which handles the execution of code.

This runtime environment is composed of a few important components such as a call stack, memory heap, event loop, web APIs, callback queue, and job queue. Let's understand these components in more detail.

### CallStack

Callstack is a mechanism in Javascript that keeps track of the function calls in our program. Whenever a function is called, its details are pushed onto the top of the call stack. The call stack follows the Last-In-First-Out (LIFO) principle, which means the most recently added function is the first one to be executed. When a function returns, its details are popped off the stack, and the control is returned to the calling function.

In simple terms, the call stack is a data structure that records where in the program we are. It is used to keep track of function calls, their order of execution, and the context in which they are called.

Consider the following code.

```javascript
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

function calculate(a, b) {
  let sum = add(a, b);
  let product = multiply(a, b);
  return sum + product;
}

console.log(calculate(2, 3));
```

When `calculate(2, 3)` is called, it calls `add(2, 3)` and `multiply(2, 3)` and stores their return values in variables. Finally, it returns the sum of the two variables.

The call stack for the above code would look like this:

```scss
1. calculate(2, 3)
2. add(2, 3)
3. multiply(2, 3)
```

As you can see, the `calculate` function is at the top of the call stack because it was the last function called. When it calls `add` and `multiply`, they are pushed onto the stack, and the stack follows the Last-In-First-Out (LIFO) principle, which means the most recently added function is the first one to be executed. When `multiply` and `add` return, they have popped off the stack, and the control is returned to the calling function.

### Memory Heap

In Javascript runtime environment, the heap is an area of memory used to store dynamically allocated objects. It is a region of memory separate from the call stack, which is used to store primitive data types and function calls. This is where all the memory allocation happens for the variables, that have been defined in the program.

### Event Loop

***JavaScript is a single-threaded language***, which means it can only do one thing at a time. However, it needs to handle multiple things at once, such as user input, network requests, and timeouts. This is where the event loop comes in. We can say that the Event Loop is the secret sauce that helps give Javascript its multi-tasking abilities (almost!)

The event loop is a mechanism that manages the execution of asynchronous code in JavaScript. It constantly checks the callback queue and executes the callbacks one by one. When the call stack is empty, the event loop takes the first callback from the callback queue and pushes it onto the call stack.

The event loop is responsible for keeping track of what code is waiting to be executed, and it ensures that the right code is executed at the right time.

### Web APIs

Web APIs are a part of the JavaScript runtime environment in web browsers. They are a collection of built-in interfaces and methods that provide additional functionality to JavaScript code, allowing it to interact with the browser and the web page.

One important aspect of Web APIs is that they are often asynchronous. Many Web APIs, such as the Fetch API, XMLHttpRequest, and the setTimeout function, allow us to perform tasks that take a certain amount of time to complete, such as sending a network request or setting a timer.

When we use an asynchronous Web API in our JavaScript code, the API will typically return immediately, while the task is being executed in the background. This means that the rest of your JavaScript code can continue to run, without being blocked by the asynchronous task.

To handle the results of the asynchronous task, Web APIs typically use callbacks or promises. When the task is complete, the callback function or promise resolves with the result of the task. The callback or promise is then added to the callback queue or job queue, respectively, to be executed by the event loop. Check out the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/API) website to learn more about different types of Web APIs.

### Callback Queue

The callback queue is a data structure that holds all the callbacks that are waiting to be executed. When the task is completed by the Web API using a callback function, it goes to the callback queue. This means, when a function that requires a callback is called, the callback is added to the callback queue. The event loop checks the callback queue and if the call stack is empty it executes the callbacks one by one.

In simple terms, the callback queue is a list of functions waiting to be executed after a certain event has occurred, such as a network request completing or a timer running out.

### Job Queue

The job queue is a data structure that is similar to the callback queue. When the task is completed by the Web API using promises, it goes to the job queue. The **job queue is used for scheduling tasks that have** **higher priority over regular callbacks**. These tasks are called "jobs" and are executed before any regular callbacks.

Some examples of tasks that are placed in the job queue include promises, microtasks, and queueMicrotask. When a completed task or job is added to the job queue, it is executed after the current task on the call stack has been completed.

So, here's a conclusive image of the total runtime environment of Javascript which includes all the components that we have seen above:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678186323446/f665036c-c811-4b16-bbc1-d374cc14a553.png align="center")

Hushh...!! So much theory and so much to understand!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678187003903/ac8c785f-c6b7-4b46-95de-1e00e0e7437c.gif align="center")

# **How Do These Components Work Together?**

Now let's understand all these components we have seen above, with the help of an example.

We will write 3 functions to understand this:

```javascript
function print_first_statement(){
    console.log("First statement");
};

function print_second_statement(){
    setTimeout(() => {
    console.log("Second statement");
}, 3000);
};

function print_third_statement(){
    console.log("Third statement");
};
```

These will be executed as follows:

```javascript
print_first_statement();
print_second_statement();
print_third_statement();
```

The output will be:

```scss
Output:
First statement
Third statement
Second statement
```

Did you see the delay in printing the ‘Second statement’?

Let’s see how this code works in the background:

**Step I**

* `print_first_statement()` is called, its function execution context is placed in the call stack and the function is executed.
    
* This outputs printing `First statement` on the screen.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678189237741/dc7794ff-4625-442b-a062-b271d2378119.png align="center")

* `print_first_statement(`’ function execution context is popped off the call stack.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678189658065/2293029f-3929-4317-9408-30a267717640.png align="left")
    
    **Step II**
    
* `print_second_statement()` is called, its function execution context is placed in the call stack and the function is executed.
    
* This creates a call to the Web API (due to the `setTimeout()` function).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678189955073/a993e23a-3b3e-4466-8c2a-e2b0ee0d3042.png align="left")
    
* Once Web API is done executing this (for 3000ms), it places the anonymous callback function in the Callback queue.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678190255638/ba9885fd-6590-4e59-962b-57a617694402.png align="left")
    
* This will be pushed to the call stack once it is found to be empty by the Event loop.
    
* `print_second_statement()` function execution context is popped off the call stack
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678190523224/10aede57-ba35-43cb-a497-15920c586e47.png align="left")
    

**Step III**

* `print_third_statement()` is called, its function execution context is placed in the call stack and the function is executed.
    
* This outputs printing `Third statement` on the screen.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678190813206/77f35cc9-59dd-40f2-8320-950167138d23.png align="left")
    
* `print_third_statement()` function execution context is popped off the call stack.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678190960753/7490f726-15ce-4315-92cb-45eab1e39b26.png align="left")
    
* When the call stack becomes empty, the anonymous callback function in the callback queue will be pushed into this empty call stack by the Event Loop.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678191237886/886e820a-8ab9-468e-8ec0-2d8cbdd2e257.png align="left")
    
* This callback function’s function execution context is placed in the call stack and the function is executed.
    
* This outputs printing `Second statement` on the screen.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678191469516/f70048a6-9a0b-4431-9603-d46b8e83f028.png align="left")
    
* The callback function’s function execution context is popped off the call stack.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678191548682/069d85c7-8ce5-429b-8d4b-b6e8eb28dbc5.png align="center")
    

So, this is the execution of a piece of code in a Javascript runtime environment with asynchronous behavior.

Check out the site [JavaScript Visualizer](https://www.jsv9000.app/) website to understand the execution of your code. It will be fun to see and understand the line-by-line execution!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678193002325/6a3784e5-a64c-4bd0-827a-cd6470aae71e.gif align="center")

## Summary

Understanding the inner workings of JavaScript's runtime is crucial for writing efficient and error-free code. The call stack, event loop, callback functions, and job queues are all essential components that work together to make JavaScript a powerful and versatile language for web development. A javascript developer must know all these components.

That wraps up the JavaScript Runtime Environment. We've covered the major components that are important for the async behavior of javascript with the help of an example. I hope this will help you to understand the topic.

---

If you got to learn something new today, then do spread the word about the article. I would be very grateful!  
If you have any questions or points to add, comment away or drop me a DM on [**Twitter**](https://twitter.com/pranita0709). I'd love to connect :)

That's it for today!