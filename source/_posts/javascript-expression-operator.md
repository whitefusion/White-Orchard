---
title: 'Javascript: expression and operator'
date: 2018-07-14 11:29:21
tags:
    - note
    - javascript
categories: language core
---
> This is the reading note for "Chapter 4: expressions and operators, Javascript: The definitive guide 5th edition". <br>

An _expression_ is a phrase of JS that a JS interpreter can _evaluate_ to produce a value. The most common way to build a complex expression out of simpler expression is with an _operator_. </br>

The simplest expressions, known as _primary expressions_, are those that stand alone - they do not include any simpler expressions. Primary expressions in JS are constant or _literal_ values, certain language keywords (reserved words like `true`, `this`, `null`), and variable references. </br>

Object and array _initializers_ are expressions whose value is a newly created object or array. They are not primary expressions since they include subexpressions that specify property and element values.  `Undefined` elements in an array initializer can be included in an array literal by simply omitting a value between commas. </br>

A property access expression evaluates to the value of an object property or an array element. JS defines two syntaxes for property acess:
```javascript
expression . identifier
expression [expression]
```

An _invocation expression_ is JS's syntax for calling (or executing) a function or method. If the function uses a `return` statement to return a value, then that value becomes the value of the invocation expression. Otherwise, the value of the invocation expression is `undefined`. In method invocations, the object or array that is the subject of the property access becomes the value of the `this` parameter while the body of the function is being executed. Invocation expressions that are not method invocations normally use the global object as the value of the `this`. </br>

An _object creation expression_ creates a new object and invokes a function (called a constructor) to initialize the properties of that object. If no arguments are passed to the constructor function in an object creation expression, the empty pair of parenthese can be omitted: `new Object`. </br>

Most operators are represented by punctuation characters such as `+` and `=`. Some, however, are represented by keywords such as `delete` and `instance`. `lvalue` is a historical term that means an expression that can legally appear on the left side of an assignment expression. In JS, variables, properties of objects, and elements of arrays are lvalues. </br>

For arithmetic expressions, 
```javascript
0/0 = NaN;
-5 % 2 = -1;
1 + 2 // => 3
"1" + "2" // => "12"
"1" + 2 // => "12"
1 + {} // => "1[object Object]"
true + true // => 2
2 + null // => 2
2 + undefined // => NaN
```
For self incremental operator `++`, expression `++x` is not always the same as `x = x + 1`. The `++` never performs string concatenation. If `x` is `"1"`, `++x` is `2`, but `x+1` is the string `"11"`. 

For relational operators, the difference between the `===` operator and the `==` operator is that `==` allows type conversion. Note that JS objects are compared by reference, not by value. An object is equal to itself, but not to any other object. if two distinct objects have the same number of properties, with the same names and values, they are still not equal. 
- `NaN` is never equal to any other value, including itself! Use `x!=x` to check `NaN`.
- `0 === -0`
- Two strings may have the same meaning and the same visual apperance, but still be encoded using difference sequences of 16-bit values.
- if both values refer to the same object, array, or function, they are equal. 

For less strict `==`:
- `null == undefined`
- If one value is an object and the other is a number or string, convert the object to a primitive. The built-in classes of core JS attempt `valueOf()` conversion before `toString()` conversion, except for the Date class, which performs `toString()` conversion. If its `valueOf()` method returns a primitive value, that value is used. Otherwise, the return value of its `toString()` method is used.

Remember that JS strings are sequences of 16-bit integer values, and that string comparison is just a numerical comparison of the values in the two strings. Note in particular that string comparison is case-sensitive, and all ASCII letters are "less than" all lowercase ASCII letters. Also, operators behave differently for numeric and string operands. `+` favors strings: it performs concatenation if either operand is a string. The comparison operators favor numbers. 

```javascript
"11" < "3"   // String comparison. Result is true.
"11" < 3     // Numeric comparison. "11" converted to 11. Result is false.
"one" < 3    // Numeric comparison. "one" converted to NaN. Result is false.
```

The `in` operator expects a left-side operand that is or can be converted to a string. It expects a right-side operand that is an object. it evaluates to `true` if the left-side value is the **name of a property** of the right-side object. It can be used to check if a key in an object but not an element in an array since element is not the name of property of array. 
```javascript
var data = [7,8,9];
"0" in data                // => true: array has an element "0"
1 in data                  // => true: numbers are converted to strings
3 in data                  // => false: no element 3
```

The `instanceof` operator expects a left-side operand that is an object and a right-side operand that identifies a class of objects. 
```javascript
var d = new Date();  // Create a new object with the Date() constructor
d instanceof Date;   // Evaluates to true; d was created with Date()
d instanceof Object; // Evaluates to true; all objects are instances of Object
d instanceof Number; // Evaluates to false; d is not a Number object
```
`instanceof` considers the 'superclass' when deciding whether an object is an instance of a class. </br>

For logical operators like `&&`, it may or may not evaluate its right-side operand. The behavior of `&&` is sometimes called short ciruiting. 
```javascript
if (a == b) stop();
(a == b) && stop();
```
Like `&&`, `||` may not evaluate the right side expression. </br>

For assignment expression, the `=` operator expects its left-side operands to be an lvalue: a variable or object property. The value of an assignment expression is the value of the right-side operand. The assignment operator has right-to-left associativity, they are evaluated from right to left. `i = j = k = 0`. </br>

In most cases, the expression `a op =b` is equivalent to the expression `a = a op b`, but two cases differ only if `a` includes side effects such as a function call or an increment operator. </br>

For `typeof` operator, it evaluates objects to nothing but `object` other tan functions, it is only useful to distinguish objects from other prmitive types. </br>

`delete` is unary operator that attempts to delete the object property or array element specified as its operand. Some built-in core and client-side properties are immune from deletion, the user-defined variables declared with the `var` statement cannot be deleted. 
```javascript
var o = {x:1, y:2};  // Define a variable; initialize it to an object
delete o.x;          // Delete one of the object properties; returns true
typeof o.x;          // Property does not exist; returns "undefined"
delete o.x;          // Delete a nonexistent property; returns true
delete o;            // Can't delete a declared variable; returns false.
                     // Would raise an exception in strict mode.
delete 1;            // Argument is not an lvalue: returns true
this.x = 1;          // Define a property of the a global object without var
delete x;            // Try to delete it: returns true in non-strict mode
```

