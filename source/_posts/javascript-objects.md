---
title: 'Javascript: Objects'
date: 2018-07-27 22:31:56
tags: 
    - note
    - javascript
categories: language core
---
> This is the reading note for "Chapter 6: Objects, values and variables, Javascript: The definitive guide 5th edition". <br>

Objects map strings to values. In addition to maintaining its own set of properties, a JS object also inherits the properties of another projects, known as its 'prototype'. Objects can also be used (by ignoring the value part of the string-to-value mapping) to represent sets of strings. Any value in JS that is not a string, a number, boolean, `null` or `undefined` is an object. Even though strings, numbers and booleans are not objects, they behave like immutable objects. </br>

Objects are immutable and are manipulated by reference rather than by value. The most common things to do with objects are create them and to set, query, delete, test and enumerate their properties. </br>

A _property_ has a name and a value. A property name may be any string, including the empty string, but no object may have two properties with the same name. The value simply evaluates to the last value if there are multiple properties with the same name defined. In addition to its name and value, each property has associated values that we'll call _property attributes_:
- The _writable_ attribute specifies whether the value of the property can be set.
- The _enumerable_ attribute specifies whether the property name is returned by a `for/in` loop.
- The _configurable_ attribute specifies whether the property can be deleted and whether its attributes can be altered.

In addition to its properties, every object has three associated _object attributes_ :
- An object's _prototype_ is a reference to another object from which properties are inherited. 
- An object's _class_ is a string that categorizes the type of an object. 
- An object's _extensible_ flag specifies whether new properties may be added to the object. 

Few terms:
- A _native object_ is an object or class of objects defined by the ECMAScript specification. 
- A _host object_ is an object defined by the host environment (such as web browsere) within which the HS interpreter is embeded. The HTMLElement objects that represents the structure of a web page in client-side JS are host objects. Host objects may also be native object, as when the host environment defines methods that are normal JS Functions objects. 
- A _user-defined_ object is any object created by the execution of JS code.
- An _own property_ is a property defined directly on an object. 
- An _inherited property_ is a property defined by an object's prototype object. 

## Creating Objects 
Objects can be created with object literals, with the `new` keyword, and with the `Object.create()` function.
A property name is a JS identifier or a string literal. A property value is any JS expression ; the value of the expression becomes the value of the property. </br>

The `new` operator creates and initializes a new object. The `new` keyword must be followed by a function invocation. A function used in this way is called a _constructor_ and serves to initialize a newly created object. Every JS object has a second JS object associated with it. The second object is known as a prototype, and the first object inherits properties from the prototype.  </br>

All objects created by object literals have the same prototype object, and we can refer to this prototype object in JS code as `Object.prototype`. Objects created using the `new` keyword and a constructor invocation use the value of the `prototype` property of the constructor function as their prototype. So the object created by new Object() inherits from `Object.prototype` just as the object created by `{}` does. Similarly, the object created by `new Array()` uses Array.prototype as its prototype, and the object created by `new Date()` uses `Date.prototype` as its prototype. </br>

`Object.prototype` is one of the rare objects that has no prototype. All of the built-in constructors have a prototype that inherits from `Object.prototype`. </br>

You can pass `null` to create a new object that does not have a prototype, but if you do this, the newly created object will not inherit anything, not even basic methods like `toString()` (which means it won't work with the `+` operator). If you want to create an ordinary empty object (like the object returned by `{}` or `new Object()`), pass `Object.prototype`:
```javascript
var o = Object.create(Object.prototype);

/ ** Creating a new object that inherits from a prototype ** /

// inherit() returns a newly created object  that inherits properties from the
// prototype object p. It uses the ECMAScript 5 function Object.create() if 
// it is defined, and otherwise falls back to an older technique.

function inherit(p) {
    if (p == null) throw TypeError(); // p must be a non-null object

    if(Object.create)
        return Object.create(p);
    var t = typeof p; 
    if (t !== 'object' && t !== 'function') throw TypeError();

    function f() {}; // define a dummy constructor function
    f.prototype = p;
    return new f();
}
```
One use for our `inherit()` function is when you want to guard against unintended modification of an object by a library function that you don't have control over. Instead of passing the object directly to the function, you can pass an heir. 
```javascript
var o = { x: "don't change" };
library_function(inherit(o)); // guard against accidental modifications of o.
```
## Querying and Setting Properties
The left-hand side should be an expression whose value is an object. If using the dot operator, the right-hand must be a simple identifier that names the property. If using square brackets, the value within the brackets must be an expression that evaluates to a string that contains the desired property name. A more precise statement is that the expression must evaluate to a string or a value that can be converted to a string. An identifier is static and must be hardcoded in the program. </br>

JS objects have a set of 'own properties', and they also inherit a set of properties from their prototype object. Suppose you assign to the property `x` of the object `o`. If `o` previously inherited the property `x`, that inherited property is now hidden by the newly created own property with the same name. The fact that inheritance occurs when querying properties but not when setting them is a key feature of JS because it allows us to selectively override inherited properties. If `o` inherits the property `x`, and that property is an accessor property with a setter method, then that setter method is called rather than creating a new property `x` in `o`. Note, however, that the setter method is called on the object `o`, not on the prototype object that defines the property, so if the setter method defines any properties, it do so on `o`, and it will again leave the prototype chain unmodified. </br>
An attempt to set a property `p` of an object `o` fails in these circusmstances, curiously, these attemplts fail silently.
- `o` has an own property `p` that is read-only.
- `o` has an inherited property `p` that is read-only. 

## Deleting Properties
Surprisingly, `delete` does not operate on the value of the property but on the property itself (both name and value). The `delete` operator only deletes own properties, not inherited ones. A `delete` expression evaluates to `true` if the delete succeeded or if the delete had no effect (such as deleting a nonexist property). `delete` also evaluates to `true` when used (meaninglessly) with an expression that is not a property access expression:
```javascript
o = {x:1}; 
delete o.x; // delete x, return true.
delete o.x; // delete nothing, and return true
delete o.toString(); // not its own property, do nothing
delete 1; // nonsense, but evaluates to true
```
`delete` does not remove properties that have a `configurable` attribute of false. (Though itv will remove configurable properties of nonextensible objects.) Certain properties of built-in objects are nonconfigurable, as are properties of the global object created by variable declaration and function declaration. 
```javascript
delete Object.prototype // can't delete, property is non-configurable
var x = 1; // declare a global variable
delete this.x // can't delete this property
function f() {} // declare a global function
delete this.f; // can't delete this property either
```

## Testing properties
You can do this with the `in` operator, with the `hasOwnProperty()e` and `propertyIsEnumerable()` methods, or simply by querying the property.

The `in` operator expects a property name ( as a string ) on its left side and an object on its right. It returns `true` if the object has an own property or an inherited property by that name.
The `hasOwnProperty()` method of an object has an own property with the given name. It returns `false` for inherited properties. The `propertyIsEnumerable()` refines the `hasOwnProperty()`. It returns `true` only if the named property is an own property and its `enumerable` attribute is `true`. </br>

Instead of using the `in` operator it is often sufficient to simply query the property and use `!==` to make sure it is not `undefined`.
```javascript
var o = { x:1 }
o.x !== undefined; // true
o.y !== undefined; // false
o.toString != undefined; // true: o inherits toString
```

However, `in` can distinguish between properties that do not exist and properties that exist but have been set to `undefined`. 
```javascript
var o = { x: undefined }   // Property is explicitly set to undefined
o.x !== undefined          // false: property exists but is undefined
"x" in o                   // true: the property exists
delete o.x;                // Delete the property x
"x" in o                   // false: it doesn't exist anymore
```

## Enumerating Properties
The `for/in` runs the body of the loop once for each enumerable property (own or inherited) of the specified object. Built-in methods that objects inherit are not enumerable, but the properties that your code adds to the objects are enumerable. 
```javascript
/* object utility functions that enumerable properties */

// copy the enumerable properties of p to o 
// if o and p have a property by the same name, o's property is overwritten.
// this function does not handle getters and setters or copy attributes. 
function extend(o, p) {
    for (prop in p) {
        o[prop] = p[prop];
    }
    return o;
}


// copy enumerable properties and won't overridden owned property
function merge(o, p) {
    for(prop in p) {
        if (o.hasOwnProperty(prop)) continue;
        o[prop] = p[prop];
    }
    return o;
}


// remove properties from o if there is not a property with the same name in p
function restrict(o, p) {
    for (prop in o) {
        if (!(prop in p)) delete o[prop];
    }
    return o;
}

// for each property of p, delete the property with the same name from o

function substract(o, p) {
    for (prop in p){
        delete o[prop];
    }
    return o;
}

// union properties
// property from o are preserved
function union (o, p) {
    return extend(extend({}, o), p);
}

// properties from p are discarded. 
function intersection(o, p) {return restrict(extend({}, o), p); }

// return an array that holds the names of the enumerable own properties of o

function keys(o) {
    if (typeof o !== 'object') throw TypeError();
    var result = [];
    for (var prop in o) {
        if (o.hasOwnProperty(prop))
            result.push(prop);
    }
    return result;
}
```
ECMAScript 5 defines two functions that enumerates property names. The first is `Object.keys()`, which returns an array of the names of the enumerable own properties of an object. Another is `Object.getOwnPropertyNames()`. It returns the names of all the own properties of the specified object, not just the enumerable properties. 

## Property Getters and Setters
It is said that an object property is a name, a value, and a set of attributes. The value may be replaced by one or two methods, known as getter and a setter. Properties defined by getters and setters are sometimes known as _accessor properties_ to distinguish them from data properties that have a simple value.</br> 
When a program queries the value of an accessor property, JS invokes the getter method(passing no arguments). The return value of this method becomes the value of the property access expression. When a program sets the value of an accessor property, JS invokes the setter method, passing the value of the right-hand side of the assignment. This method is responsible for 'setting', in some sense, the property value. The return value of the setter method is ignored. </br>

Accessor properties do not have a _writable_ attribute as data properties do. If a property has both a getter and a setter method, it is a read/write property. If it has only a getter method, it is a read-only property. And if it has only a setter method, it is a write-only property (something that is not possible with data propertiese) and attempts to read it alwasys evaluates to `undefined`. </br>

Accessor properties are defined as one or two functions whose name is the same as the property name, and with the `function` keyword replaced with `get` and `set`.
```javascript
var p = {
    x : 1.0,
    y: 1.0,
    get r() { return Math.sqrt(this.x*this.x + this.y*this.y); },
    set r(newvalue) {
        var oldvalue = Math.sqrt(this.x*this.x + this.y*this.y);
        var ratio = newvalue/oldvalue;
        this.x*= ratio;
        this.y*=ratio;
    },
    get theta() {return Math.atan2(this.y, this.x);}
}; 
```
Note the use of the keyword `this` in the getters and setter above. JS invokes these functions as methods of the object on which they are defined, which means that within the body of the function `this` refers to the point object.  Accessor properties are inherited, just as data properties are, so you can use the object `p` defined above as a prototype for other points. 
```javascript
var q = inherit(p);  // create a new object that inherits getters and setters
q.x = 0, q.y = 0;
console.log(q.r); 
console.log(q.theta);
```
The code above uses accessor properties to define an API. Other reasons to use accessor properties include sanity checking of property writes and returning different values on each property read: 
```javascript
var serialnum = {
    // THe data property holds the next serial number
    // The $ in the property name hints that it is a private property
    $n: 0,

    // Return the current value and increment it
    get next() {return this.$n++},

    // Set a new value of n, but only if it is larger than current
    set next(n) {
        if ( n >= this.$n ) this.$n = n;
        else throw "serial number can only be set to a larger value";
    }
}
```
## Property Attributes
In addition to a name and value, properties have attributes that specify whether they can be written, enumerated, and configured. </br>

We are going to consider getter and setter methods of an accessor property to be property attributes. Following this logic, we'll even say that the value of a data property is an attribute as well. Following this logic, we say that the value of a data property is an attribute as well. Thus, we can say that a property has a name and four attributes. The four attributes of a data property are _value, writable, enumerable, and configurable_. Accessor properties don't have a _value_ attribute or a _writable_: their writability is determined by the presence or absence of a setter. So the four attributes of an accessor property are _get, set, enumerable, and confiugrable_. </br>

The ECMAScript 5 methods for querying and setting the attributes of a property use an object called a _property descriptor_ to represent the set of four attributes. Thus, the property descriptor object of a data property has properties named `value, writable, enumerable` and `configurable`. And the descriptor for an accessor property has `get` and `set` properties instead of `value` and `writable`. The `writable, enumerable` and `configurable` properties are boolean values, and the `get` and `set` properties are function values, of course. To obtain descriptor, ew can call `Object.getOwnPropertyDescriptor():`
```javascript
// Returns {value: 1, writable:true, enumerable:true, configurable:true}
Object.getOwnPropertyDescriptor({x:1}, "x");
// Now query the octet property of the random object defined above.
// Returns { get: /*func*/, set:undefined, enumerable:true, configurable:true}
Object.getOwnPropertyDescriptor(random, "octet");
// Returns undefined for inherited properties and properties that don't exist.
Object.getOwnPropertyDescriptor({}, "x");         // undefined, no such prop
Object.getOwnPropertyDescriptor({}, "toString");  // undefined, inherited
```
As its name implies, `Object.getOwnPropertyDescriptor()` works only for own properties. To query the attributes of inherited properties, you must explicitly traverse the prototype chain. To set the attribute of a property, or to create a new property with the specified attributes, call `Object.defineProperty()`.
```javascript
var o = {};  // Start with no properties at all
// Add a nonenumerable data property x with value 1.
Object.defineProperty(o, "x", { value : 1,
                                writable: true,
                                enumerable: false,
                                configurable: true});
// Check that the property is there but is nonenumerable
o.x;           // => 1
Object.keys(o) // => []
// Now modify the property x so that it is read-only
Object.defineProperty(o, "x", { writable: false });
// Try to change the value of the property
o.x = 2;      // Fails silently or throws TypeError in strict mode
o.x           // => 1
// The property is still configurable, so we can change its value like this:
Object.defineProperty(o, "x", { value: 2 });
o.x           // => 2
// Now change x from a data property to an accessor property
Object.defineProperty(o, "x", { get: function() { return 0; } });
o.x           // => 0
```
If you'are creating a new property, then omitted attributes are taken to be `false` or `undefined`. if you're modifying an existing property, then the attributes you omit are simply left unchanged. Note that this method alters an existing own property or creates a new own property, but it will not alter an inherited property. </br>
If you want to create or modify more than one property at a time, use `Object.defineProperties()`.
```javascript
// this code starts with an empty object, then adds two data properties and one read-only accessor property to it. 
var p = Object.defineProperties({}, {
    x: { value: 1, writable: true, enumerable:true, configurable:true }, 
    y: { value: 1, writable: true, enumerable:true, configurable:true },
    r: {
        get: function() { return Math.sqrt(this.x*this.x + this.y*this.y) },
        enumerable:true,
        configurable:true
    }
});
```
If you pass a set of property descriptors to the second argument of `Object.create()`, then they are used to add properties to the newly created object. The _writable_ attribute governs attempts to change the _value_ attribute. And the _configurable_ attribute governs attempts to change the other attributes (and also specifies whether a property can be deleted).
- If an object is not extensible, you can edit its existing own properties, but you cannot add new properties to it. 
- If a property is not configurable, you cannot change its configurable or enumerable attributes. 
- If an accessor property is not configurable, you cannot change its getter or setter method, and you cannot change it to a data property.
- If a data property is not configurable, you cannot change it to an accessor property.
- If a data property is not configurable, you cannot change its _writable_ attribute from `false` to `true`, but you can change it from `true` to `false`.
- If a data property is not configurable and not writable, you cannot change its value. You can change the value of a property that is configurable but nonwritable, however ( because that would be the same as making it writable, then changing the value, then converting it back to nonwritable ). 
```javascript
/*
 * Add a nonenumerable extend() method to Object.prototype.
 * This method extends the object on which it is called by copying properties
 * from the object passed as its argument.  All property attributes are
 * copied, not just the property value.  All own properties (even non
* enumerable ones) of the argument object are copied unless a property

* with the same name already exists in the target object.

*/
Object.defineProperty(Object.prototype,

   "extend",                  // Define Object.prototype.extend

   {

       writable: true,

       enumerable: false,     // Make it nonenumerable

       configurable: true,

       value: function(o) {   // Its value is this function

           // Get all own props, even nonenumerable ones

           var names = Object.getOwnPropertyNames(o);

           // Loop through them

           for(var i = 0; i < names.length; i++) {

               // Skip props already in this object

               if (names[i] in this) continue;

               // Get property description from o

               var desc = Object.getOwnPropertyDescriptor(o,names[i]);

               // Use it to create property on this

               Object.defineProperty(this, names[i], desc);

           }

       }

   });
```

## Objec Attributes
An object's prototype attribute specifies the object from which it inherits properties. Objects created from object literals use `Object.prototype` as their prototype. Object created with `new` use the value of the `prototype` property of their constructor function as their prototype. And objects created with `Object.create()` use the first argument to that function as their prototype.  You can query the prototype of any object by passing that object to `Object.getPrototypeOf()`. Objects created by object literals or by `Object.create()` have a `constructor` prototype that refers to the `Object()` constructor. To determine whether one object is the prototype of another object, use the `isPrototype()` method. 
```javascript
var p = {x:1};                    // Define a prototype object.
var o = Object.create(p);         // Create an object with that prototype.
p.isPrototypeOf(o)                // => true: o inherits from p
Object.prototype.isPrototypeOf(o) // => true: p inherits from Object.prototype
```
The prototype is exposed through the specially named `__proto__` property.  </br>

An object's `class` attribute is a string that provides information about the type of the object. The default `toString()` method (inherited from `Object.prototype` ) returns a string of the form: `[object class]`.
```javascript
// the tricky part is that may objects inherit other, more useful toString() methods,
// and to invoke the correct version of toString(), we must use Function.call() method.
function classof(o) {
    if (o === null) return 'Null';
    if (o === undefined) return `Undefined`;
    return Object.prototype.toString.call(o).slice(8,-1);
}
```
If you define your own constructor function, any objects you create with it will have a class attribute of "Object": there is not way to specify the _class_ attribute for you own classes of objects. </br>

The _extensible_ attribute of an object specifies whether new properties can be added to the object or not.
ECMAScript5 defines functions for querying and setting the extensibility of an object. To determine whether an object is extensible, pass it to `Object.isExtensible()`. To make an object nonextensible, pass it to `Object.preventExtensions()`. Calling `preventExtensions()` only affects the extensibility of the object itself. If new properties are added to the prototype of a nonextensible object, the nonextensible object will inherit those new properties. </br>
The purpose of the _extensible_ attribute to be able to 'lock down' objects into a known state and prevent outside tampering. 
- `Object.seal()` works like `Object.preventExtensions()`, but in ddition to making the object nonextensible, it also make all of the own properties of that object nonconfigurable. This means that new properties cannot be added to the object, and existing properties cannot be deleted or configured. Existing properties that are writable can still be set. There is not way to unseal a sealed object. 
- `Object.freeze()` locks objects down even more tightly. In addition to making the object nonextensible and its properties nonconfigurable, it also makes all of the object's own data properties read-only. But the setter method left unaffected. 
  
Both methods won't affect on the prototype of the passed object.
`Object.preventExtensions()`, `Object.seal()` and `Object.freeze()` all return the object that they are passed, which means that you can use them in nested function invocations: 
```javascript
var o = Object.seal(Object.create(Object.freeze({x:1}),
                     {y: {value: 2, writable:true}}));
```

## Serializing Objects
Object _serialization_ is the process of converting an object's state to a string from which it can be restored. ECMA5 provides native functions `JSON.stringify()` and `JSON.parse()` to serialize and restore JS objects. </br>
```javascript
o = {x:1, y:{z:[false,null,""]}}; // Define a test object
s = JSON.stringify(o);            // s is '{"x":1,"y":{"z":[false,null,""]}}'
p = JSON.parse(s);                // p is a deep copy of o
```
`NaN, Infinity` and `-Infinity` are serialized to null. Date objects are serialized to ISO-formatted date strings. but `JSON.parse()` leaves these in string form and does not restore the original Date object. Function, RegExp, adn Error objects and the `undefined` value cannot be serialized or restored. `JSON.stringify()` serializes only the enumerable own properties of an object. 

## Object methods
In addition to the basic `toString()` method, objects all have a `toLocalString()`. The Data and Number classes define customized versions of `toLocaleString()` that attempts to format numbers, dates and times according to local conventions. </br>

`Object.prototype` does not actually define a `toJSON()` method, but the `JSON.stringify()` method looks for a `toJSON()` method on any object it is asked to serialize. </br>

The `valueOf()` method is much like the `toString()` method, but it is called when JS needs to convert an object to some primitive type other than a string -- typically, a number. 