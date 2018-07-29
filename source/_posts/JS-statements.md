---
title: 'Javascript: Statements'
date: 2018-07-19 20:18:23
tags:
    - note
    - javascript
categories: language core
---
> This is the reading note for "Chapter 5: Statements, Javascript: The definitive guide 5th edition". </br>

### Expression Statement 
Expressions are JS phrases. Statements are JS sentences or commands. JS statements are terminated with semicolons. Expressions are evaluated to produce a value, but statements are _executed_ to make something happen. "make something happen" is to evaluate an expression that has side effects. Expressions with side effects, such as assignments and function invocations, can stand alone as statements, and when used in this way they are known as _expression statements_. A similar category of statements are the _declaration statement_ that declare new variables and define new functions. </br>

The simplest kinds of statements in JS are expressions with side effects. Assignment statements are one major category of expression statements. </br>
 
A _statement block_ combines multiple statements into a single __compound statement__. A compound statement allows you to use multiple statements where JS syntax expects a single statement. But usually, you need parenthese to group them.
```javascript
{
    x = Math.PI;
    cx = Math.cos(x);
    console.log("cos(π) = " + cx);
}
```

The _empty statement_ is the opposite: it allows you to include no statements where one is expected. 
```javascript
// initialize an array a using an empty statement
for(i = 0; i < a.length; a[i++] = 0) ;
```
The `var` and **function** are _declaration statements_ - they declare or define variables and functions. 
```javascript
var x = 2, // multiple variables..
    f = function(x) { return x*x }, // each on its own line
    y = f(x);
```

## Declaration Statements
If a `var` statement appears within the body of a function, it defines local variables, scoped to that function. When `var` is used in top-level code, it declares global variables, visible throughout the JS program. Global variables are properties of the global object. Unlike other global properties, however, properties created with `var` cannot be deleted. </br>

Like variables declared with `var`, functions defined with function definition statements are implicitly 'hoisted' to the top of the containing script or function. With `var`, only the variable declaration is hoisted - the variable initialization code remains wher you placed it. With function declaration statements, however, both the function name and the function body are hoisted. </br>

The syntax for `function` definition.
``` javascript
function funcname([arg1 [, arg2[..., argn]]]) {
    statements
}
```

## Conditional Statements
For `if` and `if else`, the syntax is
```javascript
if (expression)
    statement1 // could be compound or expression statement
else
    statement2
```

For `switch`, if it does not find a case with a matching value, it look for a statement labeled `default:` it there is no `default:` label, the `switch` statement skips the block of code altogether. </br>

For `while`, omitted. </br>

For `do/while`, this means that the body of the loop is always executed at least once. </br>

For `for`, the syntax is 
```javascript
for(initialize; test; increment)
    statement
```
The _initialize_ expression is evaluated once, before the loop begins. The _test_ expression is evaluated before each iteration and controls whether the body of the loop is executed. Finally, the _increment_ expression is evaluated. 
```javascript
// return the tail of linked list o , traverse while o.next is truthy
function tail(o) {
    for(; o.next; o = o.next) /*empty*/ ;
    return 0;
}
```

For `for/in`, the syntax is 
```javascript
for (variable in object)
    statement
```
_variable_ typically names a variable, but it may be any expression that evaluates to an lvalue or a `var` statement that declares a single variable - it must be something suitable as the left side of an assignment expression. The `for/in` loop makes it easy to do the same for the properties of an object: 
```javascript
for(var p in o)
    console.log(o[p]);
```
Before each iteration, however, the interpreter evaluates the _variable_ expression and assigns the name of the property ( **a string value** ) to it. Note that the _variable_ in the `for/in` loop may be an arbitrary expression, as long as it evaluates to something suitable for the left side of an assignment. For example, you can use code like the following to copy the names of all object properties into an array:
```javascript
var o = {x:1, y:2, z:3};
var a = [], i = 0;
for(a[i++] in o) /* empty */;
```
The `for/in` loop does not actually enumerate all properties of an object, only the _enumerable_ properties. The various built-in methods defined by core JS are not enumerable. All objects have a `toString()` method, for example, but the for/in loop does not enumerate this property. If the body of a `for/in` loop deletes a property that has not yet been enumerated, that property will not be enumerated. If the body of the loop defines new properties on the object, those properties will generally **not** be enumerated. The enumeration order is the order they were defined, with older properties enumerated first. Typically , inherited properties are enumerated after all noninherited 'own' properties of an object. </br> 

## Jumps
`break` statement makes the interpreter jump to the end of a loop or other statement. `continue` makes the interpreter skip the rest of the body of a loop and jump back to the top of a loop to begin a new iteration. The `return` statement makes the interpreter jump from a function invocation back to the code that invoked it and also supplies the value for the invocation. The `throw` statement raises, or `throws`, an exception and is designed to work with `try/catch/finally` statement, which establishes a block of exception handling code. When an exception is thrown, the interpreter jumps to the nearest enclosing exception handler, which may be in the same function or up the call stack in an invoking function. </br>

Any statement may be `labled` by preceding it with an identifier and a colon: 
```javascript
identifier: statement
```
It is only useful to label statements that have bodies, such as loops and conditions.. `break` and `continue` are the only JS statements that use statement labels.
```javascript
mainloop: while(token != null) {
    // code omitted
    continue mainloop; // Jump to the next iteration of the named loop
    // more code omitted
}
```
Identifies could be anything but reserved words.The namespace for labels is different than the namespace for variables and functions, so you can use the same identifier as a statement label and as a variable or function name. A statement may not have the same label as a statement that contains it, but two statements may have the same label as long as neither one is nesetd within the other. </br>

The `break` statement, used alone, causes the innermost enclosing loop or `switch` statement to exit immediately. Because it causes a loop or `switch`  to exit. this form of the `break` statement is legal only if it appears inside one of these statements. JS also allows the break keyword to be followed by a statement label
```javascript 
break labelname;
```
When `break` is used with a label, it jumps to the end of, or terminates, the enclosing statement that has the specified label. It is a syntax error to use `break` in this form if there is no enclosing statement with the specified label. A newline is not allowed between the `break` keyword and the `labelname`. JS assumes you meant to use the simple, unlabeled form of the statement and treats the line terminator as a semicolon. 
```javascript
var matrix = getData();

var sum = 0, success = false;

compute_sum: if (matrixe) {
    for ( var x = 0; x < matrix.length; x++ ) {
        var row = matrix[x];
        if (!row) break compute_sum;
        for(var y = 0; y < row.length; y++) {
            var cell = row[y];
            if(isNan(cell)) break compute_sum;
            sum+=cell;
        }
    }
    success = true;
}
// The break statements jump here, if we arrive here with success == false
// then there was something wrong with the matrix we were given.
// otherwise sum contains the sum of all cells of the matrix. 
```
`continue` can be used only within the body of a loop.
- in a `while` loop, the specified `expression` at the beginning of the loop is tested again. 
- in a `do/while` loop, execution skips to the bottom of the loop, where the loop condition is tested again before restarting the loop at the top.
- in a `for` loop, the _increment_ expression is evaluated, and the _test_ expression is tested again to determine if another iteration should be done.
- in a `for/in` loop, the loop starts over with the next property name being assigned to the specified variable. 

Note the difference in behavior of the `continue` statement in the `while` and `for` loops: a `while` loop returns directly to its condition, but a `for` loop first evaluates its `increment` expression and then returns to its condition. </br>

Recall that function invocations are expressions and that all expressions have values. A `return` statement within a function specifies the value of invocations of that function. </br>

An _exception_ is a signal that indicates that some sort of exceptional condition or error has occurred. You might throw a number that represents an error code or a string that contains a human-readable error message. An Error object has a `name` property that specifies the type of error and a `message` property that holds the string passed to the constructor function.

```javascript
function factorial(x) {
    // if the input argument is invalid, throw an exception
    if (x < 0) throw new Error('x must not be negative');
    // otherwise, compute a value and return normally
    for (var f = 1; x > 1; f*=x, x--) /* empty */ ;
    return f; 
}
```

`try` clause of this statement simply defines the block of code whose exceptions are to be handled. The `try` block is followed by a catch clause, which is a block of statements that are invoked when an exception occurs anywhere within the `try` block. The `catch` clause is followed by a `finally` block containing cleanup code that is guaranteed to be executed, regardless of what happens in the `try` block. Both the `catch` and `finally` blocks are optional, but a `try` block must be accompanied by at least one of these blocks. 

```javascript
try {
// throw an exception, either directly, with a throw statement, or indirectly, by calling a method that throws an exception
}
catch (e) {
// The statements in this block are executed if, and only if, the try block throws an exception. These statements can use the local variable e to refer to the Error object or other value that was thrown
}
finally {
// This block contains statements that are always execuated, regardless of what happens in the try block. They are executed whether the try block terminates:
// 1) normally, after reaching the bottom of the block
// 2) because of a break, continue or return statement
// 3) with an exception that is handled by a catch caluse above
// 4) with an uncaught exception that is still propagating
}
```
Note that the `catch` keyword is followed by an identifier in parenthese. This identifier is like a function parameter. When an exception is caught, the value associated with the exception (an Error object, for example) is assigned to this parameter. Unlike regular variables, the identifier associated with a `catch` clause has block scope -- it is only defined within the `catch` block. </br>

The `finally` clause is guaranteed to be executed if any portion of the try block is executed, regardless of how the code in the `try` block completes. If there is no local `catch` block to handle the exception, the interpreter first executes the `finally` block and then jumps to the nearest containing `catch` clause. If a `finally` clause issues a `return` statement, the method returns normally, even if an exception has been thrown and has not yet been handled. </br>

We cannot completely simulate a `for` loop with a `while` loop because the continue statement behaves differently for the two loops. If we add a `try/finally` statement, we can write a `while` loop that works like a `for` loop and that handles `continue` statements correctly:
```javascript
// simulate for(initialize; test; increment) body;
initialize;
while(test) {
    try {body;}
    finally {increment; }
}
```
## Miscellaneous statements

The `with` statement is used to temporarily extend the scope chain. 
```javascript
with (object) 
    statement
```
The statement adds `object` to the front of the scope chain, executes `statement`, and then restores the scope chain to its original state. JS code that uses `with` is difficult to optimize and is likely to run more slowly than the equivalent code written without the `with` statement. The common use of the `with` statement is to make it easier to work with deeply nested object hierarchies.

```javascript
with (document.forms[0]) {
    // Access form elements directly here. For example:
    name.value = "";
    address.value = "";
    email.value = "";
}
```
Keep in mind that the scope chain is used only when looking up identifiers, not when creating new ones. A `with` statement provides a shortcut for reading properties of `o`, but not for creating new properties of `o`. </br>

The `debugger` statement normally does nothing. In practice, this statement acts like a breakpoint: execution of JS code stops and you can use the debugger to print variables' values, examine the call stack, and so on. </br>

"use strict" is a _directive_. The purpose of a 'use strict' directive is to indicate that the code that follows (in the script or functione) is _strict code_. 
- The top-level code of a script is strict code if the script has 'use strict' directive. 
- A function body is strict code if it is defined within strict code or if it has a 'use strict' directive. 
- Code passed to the `eval()` method is strict code if `eval()` is called from strict code or if the string of code includes a `use strict` directive.

The Rules: 
- The `with` statement is not allowed in strict mode.
- functions invoked as functions (rather than methods) have a `this` value of `undefined`. ( In non-strict mode, functions invoked as functions are always passed the global object as their `this` value.) The difference can be used to determine whether an implementation supports strict mode:
```javascript
var hasStrictMode = ( function() { 'use strict'; return this === undefined }());
```
when a function is invoked with `call()` or `apply()`, the `this` value is exactly the value passed as the first argument to `call()` or `apply()`. (In nonstrict mode, `null` and `undefined` values are replaced with the global object and non-object values are converted to objects.)

- code passed to `eval()` cannot declare variables or define functions in the caller's scope as it can in non-strict mode. Instead, variable and function definitions live in a new scope created for the `eval()`. This scope is discarded when the `eval()` returns.

- In strict mode, the `arguments` object in a function holds a static copy of the values passed to the function. In non-strict mode, the arguments object has 'magic' behavior in which elements of the array and named function parameters both refer to the same value.

- In strict mode, a SyntaxError is thrown if the `delete` operator is followed by an unqualified identifier such as a variable, function, or function parameter. (does nothing in non-strict mode)
- It is a syntax error for an object literal to define two or more properties by the same name. In non-strict, only last definition stay.
- It is a syntax error for a function declaration to have two or more parameters with the same name. 
- Octal integer literals (beginning with a 0 that is not followed by an x are not allowed).
- The identifiers `eval` and `arguments` are treated like keywords, and you are not allowed to change their value. You cannot assign a value to these identifiers, declare them as variables, use them as function names, use them as function parameter names, or use them as the identifier of a `catch` block.
- the ability to examine the call stack is restricted. arguments.caller and arguments.callee both throw a TypeError within a strict mode function.

## Summary

![statement1][1]
![statement2][2]

[1]: ../images/statement1.png
[2]: ../images/statement2.png


