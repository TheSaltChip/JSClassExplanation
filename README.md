# Explanation of JS Classes

### Table of contents
<ul>
<li>Keywords</li>
<li>Import structure</li>
<li>defer vs type="module"</li>
<li>private and public variables and functions</li>
<li>inheritance</li>
<li>some more examples</li>
</ul>

---
### Keywords

##### export:

This is an example of what you would call a "private" class
It can only be used in the document where it has been declared
```js
class ClassName{
    
}
```
---
If you want to use a class in a different document you need the export keyword
```js
export class ClassName{
    
}
```
---
---
#### default:
If you have a single class in a file or a default class for a file you can use
the keyword 
```js 
default
```
But its is important to note that you are only allowed to use a single default
per file

```js 
//File 1 {A.js}
export default class {
    
}
```
---
Note that you do not need to use a name, you can if you want
<br>
When importing you do this
```js
import Name from "here/is/A.js";
```
note that you do not have {} around Name, this is important. If you have {}
around Name, you are telling js to look for a named class, that is not default,
with the name "Name" in A.js
<br><br>
in that example Name is the name you have given the default class that was in A.js
and can now use Name in this document as you wish

---
---
#### import structure
When importing classes
```js
import {A,B,C} from "location/of/abc.js"
```
---
default class
```js
import Name from "location/of/where/the/default/class/is.js"
```
---
whole document as a variable
```js
import * as VarName from "here/is/a/document/with/loads/of/stuff.js"
```

---
---


#### defer vs type="module"
A **really** important thing to know is that **none** of the examples above
is allowed unless the script that is used by the html page has type="module"

```html
<script src="this/is/the/script.js" type="module"></script>
```
You do not need to load all the exported classes in the HTML file only the
main script that uses all the classes (I am not 100% satisfied with the wording here)

---
---

#### private and public variable and functions(methods)

When you are referencing class functions and variables you always use "this."
The only time you do not use it is when you are declaring private variable or
are creating local variables.

Technically you could declare public fields where the #privateVariable is
but the rule of thumb in js is to just do it in the constructor with this.name

All the variables that are declared first in the constructor with 
this.a, this.b are public

```js
export class ClassName{
    #privateVariable
    
    constructor(parameter){
        this.publicVariable = parameter;
        
        let localVariable = "this only exists in the constructor";
        
        this.#privateVariable = "# at the start equals private";
    }
    
    publicObjectFunction(){
        console.log("Or a method in java");
        console.log(new ClassName().publicObjectFunction() + "to access");
    }
    
    static publicStaticFunction(){
        console.log("Or a static method in java");
        console.log(ClassName.publicStaticFunction() + " to access");
    }

    #privateObjectFunction(){
        console.log("Or a method in java");
        console.log(new ClassName().publicObjectFunction() + "to access");
        console.log("Only works in this class");
    }

    static #privateStaticFunction(){
        console.log("Or a static method in java");
        console.log(ClassName.#publicStaticFunction() + " to access");
        console.log("Only works in this class");
    }
}
```
---
---

#### Inheritance
As far as I know it is basically the same as java
Only thing is that before you use this. you need to
call super()
```js
export class A{
    constructor(){
        this.aField = "something";
        this.anotherField = "something else";
    }
}

export class B extends A{
    constructor() {
        super();
        
        this.bVal = "value";
        
        this.aField = "something completely different"
    }
}
```

---
---

Some more examples
```js
//File 1 {A.js}
export class A{
    constructor(){
        //something usefull
    }
}

// File 2 {main.js}
import {A} from "the/place/where/you/have/A.js"

const a = new A();

// or

function megaUsefull(){
    let a = new A();
}
```

Unlike java, you can have multiple public classes in a single js file

```js
// File 1 {Classes.js}
export class A{
    
}

export class B{

}

export class C{

}

export function dFunction(){}

```

and you import them to a file with

```js
//If you need all you do this
import {A,B,C} from "the/place/where/you/have/Classes.js"

//If you only need A and C
import {A,C} from "the/place/where/you/have/Classes.js"

// Or you can import the whole document as a variable
import * as Name from "the/place/where/you/have/Classes.js";

// and then call on the different classes with
let aClass = new Name.A();

// or call a function

Name.dFunction();
```
