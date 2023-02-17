# Loose equality (==) VS Strict equality (===)

You came across this equality comparison which means you must be a programmer who works in JavaScript. And this is awesome!

Double equals (==) and triple equals (===) in JavaScript often make beginners think. But once we get to know how it works then JavaScript will feel even more beautiful.

## What are == and === in JavaScript?

Double equals (==) means "loose equality" and triple equals (===) means "strict equality". What do they do? They both check the equality between the two operands. But you can ask, to check the equality we can just use one operator but why we are using two of them?

Because there is some difference between them.

1. `==` is used to check values of the both operands.
    
2. `===` is used to check both values and datatype of the operands.
    

Earlier I thought this is the only difference between both `==` and `===` operators. But there is more to it. We have to know how these two equalities work. Let's dive more into it!

## Loose Equality (==)

Double equals `==` often referred to as loose equality because it performs type coercion before making any comparison between the operands.

This means if the operands that are being compared have different data types, the JavaScript Engine automatically converts one of the operands to match the other operands to make the comparison possible.

Let's understand this with the help of an example.

```javascript
const x = 10;
const y = "10";

console.log(x == y);  // true
```

Here, we have two variables `x` and `y`. The type of variable `x` is `number` and the type of variable `y` is `string`. Now, when we compare the two variables using double equals (`==`) we get `true` as our output.

This is because the type of the variable `y` is converted to the `number` before making the comparison. And after the comparison, the value is checked in both variables. If it's the same, we will get `true`, and we'll get `false` otherwise. Here we got `true`.

Note this while comparing using `==` actual values of operands remain the same or unchanged. Its datatype gets converted implicitly.

### Type Coercion

In loose equality `==`, we heard of type coercion. There are some rules that JavaScript follows for this conversion. Let's go through them one by one.

* If both operands are of the same type, they are compared with the `===` operator and the result is returned.
    
* If the type is different, it gets converted and then compared using the `===` operator. And this 'type coercion' also follows some rules. Let's understand them:
    
    1. **Number and String** : In this conversion, the string is converted to the type number and then it gets compared. If the string can't be converted to a number, then the conversion gives `NaN`. When `NaN` is compared with the number it returns `false`.
        
    2. **Number and Boolean** : Here, the boolean gets converted to a number like this: true = 1 and false = 0. And then both are compared and the result is returned.
        
    3. **Number and Bigint** : Numerical value of both operands gets compared and the result is returned. If any one of the operands is `NaN`, `+∞`, or `-∞`, it will return `false`.
        
    4. **String and Boolean** : Here, the boolean gets converted to the type number and the string gets converted to the type number. And then both get compared and the result is returned.
        
    5. **String and Bigint** : Here string is converted to bigint. And then compared and the result is returned.
        
    6. **null and undefined** : When null and undefined are compared, loose equality returns `true`.
        
    7. **Object and another type** : If one operand is an `object` and the other is a primitive type, the object will be converted to a primitive type before the comparison and result get returned.
        
    
    Let's understand these rules with some examples:
    

```javascript
console.log(20 == "20"); // true
console.log(20 == "abc"); // false => Number and NaN(String to Number)
console.log(1 == true); // true => 1 and 1 (true=1)
console.log(40 == false); // false => 40 and 0 (false=0)
console.log("mystring" == true); // false => NaN (string converts into number) and 1 (true=1)
console.log(null == undefined); // true
```

> **Point to remember!**
> 
> Loose equality compares the type of the operands, if they are not the same, it converts both of them to the same type and then compares them with the strict equality operator.

## Strict Equality (===)

Triple equals (===) is also referred to as "strict equality". It works similarly to how double equals (==) works, with one important difference: it does not convert the types of the operands before comparing.

It only compares two operands and checks if they have the same data type and if both of them are similar or not.

It checks the similarity between both operands as follows:

1. **For integer:** check for the same number.
    
2. **For string:** both must have the same character in similar order.
    
3. **For boolean:** both must have the same value. (True or False).
    
4. **For object:** must have the same reference to the objects.
    

Explore the below example for a better understanding:

```javascript
console.log(5 === "5"); // false => value is same but type is different
console.log("25" === "50"); // false => type is same but value is different
console.log([1,2,3] === [1,2,3]); // true => both type and values are same with same order
```

> **Point to remember!**
> 
> Strict equality strictly compares the values of both operands and only returns true if the type and value are the same. It does not do any changes to the type of the operand.

## Conclusion

* The `==` operator performs a loose equality comparison that performs type coercion if necessary to make the comparison possible.
    
* The `===` operator, on the other hand, performs a strict equality comparison that does not perform type coercion and requires the operands to have the same type (as well as the same value).
    

Type coercion in JavaScript can sometimes lead to unexpected results, so it's mostly recommended to use the strict equality operator `===` instead of the loose equality operator `==`.

Refer to [this](https://abs-eq.emnudge.dev/#) as an excellent tool to see the behind-the-scenes during loose equality comparison. You will have fun!

Hope this blog will help you in the learning process!