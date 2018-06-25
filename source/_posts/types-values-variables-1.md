---
title: 'Javascript: Types, Values and Variables'
date: 2018-06-18 23:44:07
tags:
    - javascript
    - note
categories: language core
---
> This is the reading note for " Chapter 3: Overview, Javascript: The definitive guide 5th edition". <br>

Javascript types can be divided into two categories: **primitive** types and **object** types. The primitive types include numbers, strings of text and boolean truth values. <br>

The special Javascript **types** and **values** are _null_ and _undefined_. Each value is typically considered to be the sole number of its own special type. <br>

Any Javascript value that is not a number, a string, a boolean or null or undefined is an **object**. <br>

An ordinary Javascript object is an **unordered** collection of named values. _Array_ is a special kind of object which is an ordered collection of numbered values. <br>.

_function_ is another special kind of object. The most important thing about functions in JS is that they are true values and that JS programs can treat them like regular objects. <br>

Functions that are written to be used to initialize a newly created object are known as _constructor_. Each constructor defines a _class_ of objects. Classes can be thought of as subtypes of the object type. Core JS defines three other useful classes: _Date_, _RegExp_ and _Error_. <br>

The Javascript interpreter performs automatic garbage collection for memory management. When an object is no longer reachable - when a program no longer has any way to refer to it - the interperter knows it can never be used again and automatically reclaims the memory it was occupying. <br>

Javascript is an object-oriented language. It means that rather than having globally defined functions to operate on values of various types, the types themselves define methods for working with values. Technically, it is only Javascript objects that have methods. But numbers, strings and boolean values behave as if they had methods. In JS, _null_ and _undefined_ are the only values that methods cannot be invoked on. <br>

JS types can also be categorized as mutable and immutable types. _Objects_ and _arrays_ are mutable. _Numbers_, _booleans_, _strings_, _null_ and _undefined_ are immutable. For _strings_, one can access the text at any index of a string, but JS provides no way to alter the text of an existing string. <br>

JS converts values liberally from one type to another. If a program expects a _string_, for example, and you give it a _number_, it will automatically convert the _number_ to a _string_ for you. <br>

JS variables are **untyped**. JS uses _lexical scoping_. Variables declared outside of a function are _global variables_ and are visible everywhere in a JS program. Variables declared inside a function have _function scope_ and are visible only to code that appears inside that function. 
