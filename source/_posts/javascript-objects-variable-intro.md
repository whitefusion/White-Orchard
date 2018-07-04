---
title: 'Javascript: Objects and variable Intro'
date: 2018-07-02 22:54:01
tags: 
    - note 
    - javascript
categories: language core
---
> This is the reading note for "Chapter 3 (3.5 to end): Types, values and variables, Javascript: The definitive guide 5th edition". <br>

## The global object
Previous section has divided the JS types into primitive types and object types - objects, arrays and functions. </br>

Global object serves an important purpose: the properties of this object are the globally defined symbols that ar3e available to a JS program. When JS interpreter starts, it creates a new global object and gives it an initial set of properties that define:
- global properties like `undefiend`, `infinity`, `NaN` ...
- global functions like `isNaN()`, `parseInt()`, `eval()` ...
- constructor functions like `Date()`, `RegExp()`, `Object()`, `Array()` ...
- global objects like `Math` and `JSON`

In top-level code - JS code that is not part of a function - you can use JS keyword `this` to refer to the global object: 
```javascript
var global = this; // define a global variable to refer to the global object.
```

In client-side JS, the `Window` object serves as the global object for all JS code contained in the browser widnows it represents. This global `Window` has a self-referential `window` property that can be used insetad of `this` to refer to the global object. </br>

Actually, if your code declares a global variable, that variable is a property of the global object. </br>

## Wrapper objects
```javascript
// A primitive type string, not an object, could have properties!!
var s = 'hello world'; // a string
var word = s.substring(s.indexOf(" ")+1, s.length); // Use string properties

var s = 'test';
s.len = 4; // create a new object but then discarded. 
var t = s.len; // return undefined. This could be used to tell primitive type from object type.
```
Whenever you try to refer to a property of a string `s`, JS converts the string value to an object as if by calling `new String(s)`. Once the property has been resolved, the newly created object is discarded. </br>

The temporary objects created when you access a property of a string, number, or boolean are known as **wrapper objects**. And it may occasionally be necessary to distinguish a string value from a String object or a number of boolean value from a Number or Boolean object. The `typeof` operator will aslo show the difference.  </br>

## Immutable Primitive Values and Mutable Object References
There is a fundamental difference in JS between primitive values and objects (arrays, objects and functions). Primitives are immutable and are also compared by _value_. Meanwhile, objects are different than primitives. They are _mutable_ and they are compared by reference. Two object values are the same if and only if they refer to the same underlying object. </br>

Assigning an object(or array) to a variable simply assigns the reference: it does not create a new copy of the object. If you want to make a new copy of an object or array, you must explicitly copy the properties of the object or the element like using a `for` loop or something equivalent. </br>

## Type Conversions
![type_conversion][1]
The primitive-to-primitive conversions are relatively straightforward. The primitive-to-object conversions are straightforward: primitive values convert to their wrapper object. </br>

The `==` equality operator is flexible with its notion of equality. 
```javascript
null == undefined // equal.
"0" == false
```
Keep in mind that convertibility of one value to antoher does not imply equality of those two values. If `undefined` is used where a boolean value is expected, for example, it will convert to `false`. But this does not mean that `undefined == false`. 

## Explicit Conversions
The simplest way to perform an explicit type conversion is to use the `Boolean(), Number(), String(), Object() ` functions. Note that any value other than `null` or `undefined` has a `toString()` method and the result of this method is usually the same as that return by the `String()` function. </br>

Cretain JS operators perform implicit type conversions, and are sometimes fused for the purposes of type conversion. The unary `+` operator converts its operand to a number. 

```javascript
x + "" // same as String()
+x // same as Nubmer(x)
!!x // same as Boolean(x)
```
The `toString()` method defined by the Number class accepts an optional argument that specifies a radix or base. 
```javascript
var n = 17;
binary_string = n.toString(2); // evaluates to '10001'
octal_string = '0'+n.toString(8); // evaluates to '021'
hex_string = '0x' + n.toString(16); // evaluates to '0x11'
```
To get a better control when working with financial or scientific data, there other number-to-string classes may help here. `toFixed()` converts a number to a string with a specified number of digits after the decimal point. `toExponential()` converts a number to a string using exponential notation, with one digit before the decimal point and a specified number of digits after the decimal point. `toPrecision()` converts a number to a string with the number of significant digits you specify. 

```javascript
var n =123456.789;
n.toFixed(2); // '123456.79'
n.toExponential(3);// '1.235e+5'
n.toPrecision(4); // '1.235e+5'
```
We also have `parseInt()` and `parseFloat()` functions to convert string to number. If a string begins with "0x" or "0X", `parseInt()` interprets it as a hexadecimal number. It also accepts a second argument specifying the radix. </br>
```javascript
parseInt("3 blind mice") // => 3
parseFloat(" 3.14 meters") // => 3.14
parseInt("0xFF") // => 255
parseInt("-0xFF") // => -255
parseInt(".1") // => NaN
parseFlaot("$72.47") // => NaN
parseInt("11",2) // => 3
parseInt("zz", 36); // => 1295
```

## Object to primitive conversions
All objects inherit two conversion methods. The first is `toString()` and the second is `valueOf()`. </br>
Many classes define more specific versions of `toString()` method, like `Date(), RegExp` that convert object to a human-readable format. Similar things happened for `valueOf()`. </br>
With those two functions, we can take a further look on object-to-string and object-to-number conversions. To convert an object to a string, JS takes to these steps:
- look at `toString()`, if not defined or no primitive returned,
- look at `valueOf()`, if no primitive returned.
- `TypeError`
To convert an object to a number, JS takes to these steps:
- `valueOf()`
- `toString()`
- `TypeError`

## Variable Declaration and Scope
If the repeated declaration has an initializer, it acts as if it were simply an assignment statement, and a JS variable can hold a value of any type. </br>

The _scope_ of a variable is the region of your program source code in which it is defined. A _global_ variable has global scope; it is defined everywhere in your JS code. JS uses _function_ scope: variables are visible within the function in which they are defined and within any functions that are nested within that function. <br/>
JS's function scope means that all variables declared within a function are visible throughout the body of the function. This feature of JS is informally known as **hoisting**: JS code behaves as if all variable declarations in a function (but not any associated assignments) are 'hoisted' to the top of the function. 
```javascript
var scope = 'global';
function f() {
    console.log(scope); // undefined
    var scope = 'local';
    console.log(scope); // local
}
```
When you declare a global JS variable, what you are actually doing is defining a property of the global object. If you use `var` to declare the variable, the property that is created is nonconfigurable, which means that it cannot be deleted with the `delete` operator. </br>
```javascript
var truevar = 1;
delete truevar; // => false
this.fakevar = 3; 
delete fakevar; // => true
```
JS is a __lexically scoped__ language: the scope of a variable ca nbe thought of as the set of source code lines for which the variable is defined. Global variables are defined throughout the program. Local variables are defined throughout the function. Every chunk of JS code has a __scope chain__ associated with it. This scope chain is a list or chain of objects that defines that variables that are 'in scope' for that code. When JS needs to look up the value of a variable `x`, it looks up the object in the chain one by one.</br>
In top-level JS code, the scope chain consists of a single object, the global object. In a non-nested function, the scope chain consists of two objects. The first is the object that defines the functions's parameters and local variables, and the second is global object. In a nested function, the scope chain has three or more objects. It is important to understand how this chain of objects is created. When a function is defined, it stores the scope chain then in effect. When that function is invoked, it creates a new object to store its local vars, and adds that new object to the stored scope chain to create a new, longer, chain that represetnts the scope for that function invocation. </br>
It becomes more interesting for nested functions since each time the outer function is called, the inner function is defined again. A new scope object will be created for each invocation of the same function, even thoght the code are the same. 





  [1]: ../images/type_conversion.png