---
title: Number_String_Boolean_Null_Undefined
date: 2018-06-24 22:06:54
tags:
    - core
    - language
    - note
    - core
categories: language
---
## Numbers
JS does not make a distinction between integer values and floating-point values. **All numbers** in JS are represented as a 64-bit floating-point values. </br>

When a number appears directly in a JS program, it's called a _numeric literal_. 

#### Integer Literals
A hexadecimal literal begins with "0x" or "0X", followed by a string of hexadecimal digits. A hexadecimal digit is one of the digits 0 through 9 or the letters a (or A) through f(or F), which represent values 10 through 15. </br>

Some implementations of JS allow you to specify integer literals in octal(base-8) format. An octal literal begins with the digit 0 and is followed by a sequence of digits, each between 0 and 7. </br>

#### Arithmetic in JS

JS supports compolex mathematical opeartions through a set of functions and constants defined as properties of the `Math` object.</br>

Arithmetic in JS does not raise errors in case of overflow, underflow, or division by zero. The 'overflow' will have a return value `infinity` or `negative infinity`. The 'underflow' will have a return value `0` or `-0`. The '0/0' will have a result as `NaN`. `NaN` also arises if you attempt to divide infinity by infinity, or take the square root of a negative number or use arithmetic operators with non-numeric operands that cannot be converted to numbers. </br>

`NaN` has one unusual feature in JS: it does not compare equal to any other value, including itself. This means you can't write `x == NaN` to determine whether the value of a variable x is `NaN`. Instead you should write `x!=x`. That expression will be true if, and only if, x is `NaN`. The function `isNaN()` is similar. </br>

The negative zero is also somewhat unusual. It compares equal to positive zero, which means that the two values are almost distiguishable, except when used as a divisor: `1/zero === 1/negz // => false: infinity and -infinity are not equal`

#### Binary floating-point and rounding errors
The IEEE-754 floating point representation used by JS is a binary representation, which can exactly represent fractions like `1/2, 1/8 and 1/1024`. For example the decimal number 0.75 is represented as 0.11 in binary representation: 1x2^(-1)+1x2^(-2). Unfortunately, binary floating-point representations cannot exactly represent numbers as simple as 0.1 since it cannot be expanded as a base-2 Taylor series with finite addons.
```javascript
var x = .3 - .2
var y = .2 - .1
x == y // false
x == .1 // false
y == .1 // true
```
It affects any programming language that uses binary floating-point numbers. The computed values are adequate for almost any purposes, the problem arises when we attempt to compare values for equality.  </br>

#### Dates and Times
`Date()` constructor for creating objects that represent dates and times. It comes with a handy API which cannot be referred from docs.

## Text
A _string_ is an immutable ordered sequence of 16-bit values, each of which typically represents a Unicode character. </br>

The most commonly used Unicode characters have codepoints that fit in 16 bits and can be represented by a single element of a string. Unicode characters whose codepoints do not fit in 16 bits are encoded following the rules of UTF-16 as a sequence (known as a 'surrogate pair') of two 16-bit values. This means that a JS string of length 2 (two 16-bit values) might represent only a single Unicode character: 
```javascript
var e = '\ud835\udc52' // look like a length-1 string but it is length 2.
```
In short, we have a set of Unicode character. Each of them has a code point (`0Xxxxx or 0Xxxxxx`). Then we encode(map) them to UTF-16 encoding, but not all of them can fit in 16 bits.

#### Escape sequences in string literals
The backslash character (`\`) has a special purpose in JS strings. Combined with the character that follows it, it represents a character that is not otherwise representable within the string. </br>

the `\'` is the single quote character. Tqo escape sequences are generic and can be used to represent any character by sepcifying its Latin-1 or Unicode character code as a hexadecimal number. For Latin-1 encoding, the sequence `\xA9` represents the copyright symbol, which has the Latin-1 encoding given by the hexadecimal number A9.

```
\xXX: The Latin-1 character specified by the two hexadecimal digits XX
\uXXXX The unicode character specified by the four hexadecimal digits XXXX
```

#### Working with strings
Remember that strings are immutable in JS. Methods like `replace() ` and `toUpperCase()` return new strings: they do not modify the string on which they are invoked. 

#### Pattern Matching
JS defines a `RegExp()` constructor for creating objects that represent texual patterns. JS adopts Perl's syntax for regular expressions. Like Dates, it's simply a specialized kind of object, with a useful API. 
```
var text = "testing: 1, 2, 3";   // Sample text
var pattern = /\d+/g             // Matches all instances of one or more digits
pattern.test(text)               // => true: a match exists
text.search(pattern)             // => 9: position of first match
text.match(pattern)              // => ["1", "2", "3"]: array of all matches
text.replace(pattern, "#");      // => "testing: #, #, #"
text.split(/\D+/);               // => ["","1","2","3"]: split on non-digits
```

## Boolean 
Any JS value can be converted to a boolean value. The following values convert to, and therefore work like, `false`:
```
undefined
null
0
-0
NaN
"" // empty string
```
All other values, including all objects (and arrays) convert to, and work like, `true`. 

## Null and undefined
`null` is the absence of a value. Using the `typeof` operator on `null` returns "object", indicating that `null` can be thought of as a special object value that indicates "no objects". </br>

The `undefined` value represents a deeper kind of absence. It is the value of variables that have not been initialized and the value you get when you query the value of an object property or array element that does not exist. The `undefined` value is also return by functions that have no return value, and the value of function parameters for which no argument is supplied. If you apply the `typeof` operator to the undefined value, it returns 'undefined', indicating this value is the sole member of a special type. Neither `null` or `undefined` have any properties or methods. </br>

You might consider `undefined` to represent a system-level, unexpected, or error-like absence of value and `null` to represent program-level, normal or expected absence of value. 

