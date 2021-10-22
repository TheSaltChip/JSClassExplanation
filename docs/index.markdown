# Explanation of JS Classes
## ECMAScript 12

### Table of contents

<ul>
<li>Keywords</li>
<li>Import structure</li>
<li>defer vs type="module"</li>
<li>Private and public variables and functions</li>
<li>Inheritance</li>
<li>jsDoc</li>
<li>Parameters</li>
<li>More examples</li>
</ul>

---

## Keywords

### export:

This is an example of what you would call a "private" class It can only be used in the document where it has been
declared unless you load it in the html file

```js
class ClassName {

}
```

Like this, which in my honest opinion is dumb when you can use modules. This will become ugly
when you are dealing with loads of classes

```html
<script src="here/is/the/Class.js" defer></script>
```

---
If you want to use a class in a different document you need the export keyword

```js
export class ClassName {

}
```

---
---

### default:

If you have a single class in a file or a default class for a file you can use the keyword

```js 
default
```

But its is important to note that you are only allowed to use a single default per file

```js 
//File 1 {A.js}
export default class {

}
```

---
Note that you do not need to use a name, you can if you want

When importing you do this

```js
import Name from "here/is/A.js";
```

Note that you do not have {} around Name, this is important. If you have {} around Name, you are telling js to look for
a named class, that is not default, with the name "Name" in A.js

In that example Name is the name you have given the default class that was in A.js and can now use Name in this document
as you wish

---
---

### import structure

When importing classes

```js
import {A, B, C} from "location/of/abc.js"
```

---
default class

```js
import Name from "location/of/where/the/default/class/is.js"
```

---
Whole document as a variable

```js
import * as VarName from "here/is/a/document/with/loads/of/stuff.js"
```

---
---

### defer vs type="module"

A **really** important thing to know is that **none** of the examples above
that use export is allowed unless the script that is used by
the html page has type="module"

```html

<script src="this/is/the/script.js" type="module"></script>
```

You do not need to load all the exported classes in the HTML file only the main script that uses all the classes (I am
not 100% satisfied with the wording here)

defer is used when you want to load the script in parallel with the html document type="module" has "defer" by standard.
But as previously stated, type="module" is needed when you are using export on classes

---
---

### Private and public variable and functions(methods)

When you are referencing class functions and variables you always use "this."
The only time you do not use it is when you are declaring private variable or are creating local variables.

Technically you could declare public fields where the #privateVariable is but the rule of thumb in js is to just do it
in the constructor with this.name

All the variables that are declared first in the constructor with this.a, this.b are public

```js
export class ClassName {
    #privateVariable

    constructor(parameter) {
        this.publicVariable = parameter;

        let localVariable = "this only exists in the constructor";

        this.#privateVariable = "# at the start equals private";
    }

    publicObjectFunction() {
        console.log("Or a method in java");
        console.log(new ClassName().publicObjectFunction() + "to access");
    }

    static publicStaticFunction() {
        console.log("Or a static method in java");
        console.log(ClassName.publicStaticFunction() + " to access");
    }

    #privateObjectFunction() {
        console.log("Or a method in java");
        console.log(new ClassName().publicObjectFunction() + "to access");
        console.log("Only works in this class");
    }

    static #privateStaticFunction() {
        console.log("Or a static method in java");
        console.log(ClassName.#publicStaticFunction() + " to access");
        console.log("Only works in this class");
    }
}
```

---
---

### Inheritance

As far as I know it is basically the same as java

Only thing is that before you use this. you need to call super()

```js
export class A {
    constructor() {
        this.aField = "something";
        this.anotherField = "something else";
    }
}

export class B extends A {
    constructor() {
        super();

        this.bVal = "value";

        this.aField = "something completely different"
    }
}
```

---
---

### jsDoc

jsDoc is to js what javaDoc is to java. You use it to describe classes and the methods within, and decide what types the
parameters could be.

```js
export class ClassName {
    /**
     * Usefull description of what this class is
     *
     *
     * @param {Paramtype} param1
     * @param {Paramtype2 | paramtype 3}param2
     */
    constructor(param1, param2) {

    }

    /**
     *
     * @param {Number} p1
     * @param {Number} p2
     * @param {String} p3
     */
    method(p1, p2, p3) {

    }
}
```

"{Paramtype} param1" tells the js that param1 should be of the type Paramtype, but it is not enforced at runtime, it
is only for your own help when writing the code

You can specify as many types as you wish with "|",
<br>
Example: {Number | String} means that you expect the parameter to be either a String or a Number

---
---

### Parameters

There is a couple of things you can do with the parameters of a function or class

```js
class ClassName {
    constructor(p1, p2, p3, p4, p5, ...p6) {
    }
}
```

Here p6 is a list containing every single parameter after the first 5, ie a list containing [6,7,8,9,0]

```js
let a = new ClassName(1, 2, 3, 4, 5, 6, 7, 8, 9, 0);
```

The fun/bad/useful/useless thing is that the values you give the constructor does not need to be of the same type

```js
let b = new ClassName(1, 2, 3, 4, 5, "String", 'C', 6, "String", new Object())
```

And all of this is allowed, all get stored in an array for you to access

---

In Js you can give the parameters default values
<br>
Here will p1 be 1 if not given a value and p4 will be null if not given a value

```js
class ClassName {
    constructor(p1 = 1, p2, p3, p4 = null) {

    }
}
```

which brings me on to a useful technic for handling loads of parameters when you only need to modify a few

```js
class ClassName {
    constructor({p1 = 5, p2 = "useful default", p3 = "not useful", p4} = {}) {
        // Code that does something revolutionary
    }
}

let classA = new ClassName({
    p3: "Super useful",
    p4: 42
});

let classB = new ClassName({
    p1: 3,
    p2: "even more useful",
    p4: 9,
    p3: "ta da"
});
```

Note that the parameter now is an object instead of multiple values, which in my opinion is much easier to use since you
have to manually tell js which parameter you want to change or give value to

---
---

### Some more examples

```js
//File 1 {A.js}
export class A {
    constructor() {
        //something usefull
    }
}

// File 2 {main.js}
import {A} from "the/place/where/you/have/A.js"

const a = new A();

// or

function megaUsefull() {
    let a = new A();
}
```

Unlike java, you can have multiple public classes in a single js file

```js
// File 1 {Classes.js}
export class A {

}

export class B {

}

export class C {

}

export function dFunction() {
}

```

and you import them to a file with

```js
//If you need all you do this
import {A, B, C} from "the/place/where/you/have/Classes.js"

//If you only need A and C
import {A, C} from "the/place/where/you/have/Classes.js"

// Or you can import the whole document as a variable
import * as Name from "the/place/where/you/have/Classes.js";

// and then call on the different classes with
let aClass = new Name.A();

// or call a function

Name.dFunction();
```
