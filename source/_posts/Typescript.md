---
title: "Angular: Typescript"
date: 2018-06-04 20:10:03
tags:
    - language
    - core
    - note
    - typescript
---
> This is the reading note for "Typescript, ng-book, the complete book on Angular 5 ". <br>

### Overview
There are five big improvements that Typescript bring over ES5: 
- types
- classes
- decorators
- imports
- language utilities (e.g. destructuring)

### Types
TS can optionally provide the variable type along with its name as we declare it. 

```typescript
var fullname: string;

function greetText(name: string): string {
    return 'Hello' + name;
}
```
The declaration of variable type as well as the return type of a function could prevent bugs caused by type error.

### Built-in types
- string
- number
- boolean
- array<type>
- enums : A bi-direction dictionary, which could be used for authorization management. 
- any
- void

### Classes
A class could contain properties and methods like
```typescript
class Persion {
    first_name: string,
    last_name: string,
    age: number;

    greet() {
        console.log('hello');
    }
}
```
The type of property is optinal. <br>
When methods don't declare an explicit returning type and return a value, it's assumed they can return anything. <br>
However, in the case above, we are returning *void*, since there's no explicit return statement, which is also a valid *any* value.<br>

### Constructors
A constructor is a special method that is executed when a *new* instance of the class is being created. <br>
Constructor methods **must** be named constructor, They can optionally take parameters but they cannot return any values. <br>
A class can only have one constructor. <br>
Constructor can take parameters when we want to parameterize our new instance creation.<br>
When a class has no constructor defined explicityly, one will be created automatically.

### Inheritance
Inheritance is a way to indicate that a class receives behavior from a parent class. Then we can override, modify or augment those behaviors on the new class. <br>
It is achieved through the **extends** keyword.

```typescript
class Report{
    data: Array<string>;
    constructor(data: Array<string>) {
        this.data = data;
    }

    run() {
        this.data.forEach(function(line){console.log(line)});
    }
}


class TabbedReport extends Report {
    headers: Array<string>;
    constructor(headers: string[], values: string[]) {
        super(values)
        this.headers = headers;
    }

    run() {
        console.log(this.headers);
        super.run();
    }
}
```

### Other features
- Interfaces
- Generics
- Importing and exporting modules
- Decorators
- Destructuring


