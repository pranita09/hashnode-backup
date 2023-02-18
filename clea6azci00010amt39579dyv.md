# Why do we get TypeError and ReferenceError in JavaScript?

While coding we encounter so many different types of errors. These errors often make us scratch our heads. But if we understand why we get those errors and how to solve or avoid them, then our life will become much more easier!

In today's blog, let's understand what exactly `TypeError` and `ReferenceError` are? Why do we get those errors? And how we can avoid them?

## What is TypeError?

`TypeError` is thrown when an operand or argument passed to a function is incompatible with the type expected by that operator or function

In other words, the `TypeError` object shows an error when an operation could not be performed when a value is not of the expected type.

### Cases that give TypeError:

1. When we have declared a variable with a `const` keyword, and again when we try to reassign the same variable. In this case, `TypeError` will be thrown.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676725945573/76c0bc6e-eabd-47d1-a172-09a3201d0ac8.png align="left")
    
    `const` variable cannot be reassigned.
    
2. When we try to call a non-function value as a function.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676725802922/ded1e4e6-f343-4744-adb8-529968eb1b7a.png align="left")
    
    Here, the `num` variable is a non-function value. It can not be accessed as a function.
    
3. `TypeError` occurs when we use a value outside the scope of its data type.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676726255125/6b788b1a-c870-47b6-b68f-e2345cf1058d.png align="left")
    
    We can see, both `x` and `y` variables are of type `boolean` and `number` respectively. We cannot access them using built-in functions of different data types.
    
4. When we use an object that does not have the property or function that we are trying to access, then `TypeError` will be thrown.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676726636387/89634a5c-6f8c-4d82-9fff-32403af4b84c.png align="left")
    
    Here, the `person` object does not contain `address` property. Thus, it gave the `TypeError`.
    

### How to avoid getting TypeError:

The simplest way to avoid type errors is to avoid using the same name for different variables and functions. If we use different names, it will remove the confusion and it will be easier to keep track of the variables. This can easily avoid errors.

Another way is to make sure we are using the variable as they are intended. When we need to reassign a value, use `let` (allowing such changes) instead of `const`.

The next way is always trying to check the `type` of variable or value we are using. We cannot access variables or values outside the scope of their `type`.

## What is ReferenceError?

`ReferenceError` occurs when we are trying to refer to or use something that does not exist.

In other words, the `ReferenceError` occurs when JavaScript tries to access a variable that doesn’t exist, hasn’t been defined, or doesn’t exist in the current scope from which we are trying to access it.

When we create or declare a variable, what we are doing is simply creating a reference to an object with an associated value. Here’s an example:

```javascript
const name = "John";
```

The code tells the JavaScript compiler that it sees the variable `name`, it should interpret it as its value, which means it should see `name` as the string with the value of `John`. When JavaScript encounters a variable that doesn’t have a declaration, though, it doesn’t know what to do, so it throws a `ReferenceError`.

### Cases that give ReferenceError:

1. When a variable is undeclared and undefined.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676731385247/62adb580-7c5a-4b89-90a1-37389963c682.png align="left")
    
2. When we access a variable before its declaration.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676731484689/5d31d6b5-dd82-4043-850c-cd7250197026.png align="left")
    
3. **Wrong Scope**: When we try to access a variable inside a function scope that is declared with `let` or `const` keywords. These keywords are block scoped. Hence, it throws `ReferenceError`.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676731741999/68a333a9-d80a-446e-9349-34d371bf8f2f.png align="left")
    
4. **Strict Mode (Silent Errors)**: When we assign a value to a variable, JavaScript requires that the variable be declared using `var`, `let`, or `const`. If we use JavaScript in its default mode (without strict mode), declaring a variable without these keywords will result in a silent error. It means that JavaScript will ignore it even though it is an error. Here’s an example:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676730951268/5a40adab-240f-4323-b1a0-c578e80c9f80.png align="left")
    
    In strict mode, JavaScript will catch this error and throw a reference error. Like this:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676731120341/389ad628-f0ff-4d9a-9f5c-c402a0766402.png align="left")
    
    To avoid silent errors, it is recommended to use JavaScript in the strict mode so it will not miss similar errors that are typically ignored in JavaScript’s default mode.
    

### How to avoid getting ReferenceError:

The easiest way to avoid getting reference errors is to refer to or access only defined variables. Make sure that variables are always declared before calling them.

We can also use conditional statements and error handling to avoid the error if a variable or a function does not exist.

## Summary

While coding, coming across errors is something we can not deny. But the best strategy is to be aware of what triggers these errors so that we can avoid these errors in the code. This will save time searching for solutions online and wasting hours trying to find a solution when a little bit of knowledge and awareness about these things can help.

---

I hope you got to learn something new today. If you did, do consider sharing this post. If you have any queries or feedback do comment or connect with me on [**Twitter**](https://twitter.com/pranita0709)!