# TypeScript
>TypeScript, developed by Microsoft Corp., is a typed superset of JavaScript that compiles to plain JavaScript.

## Why use TypeScript

TypeScript syntax is a superset of ECMAScript 2015 (ES2015). Types are completely optional. We can rename all .js files to .ts file and TypeScript will still give us back valid .js file. Additionally it provides some extra features that help us make a robust software . Like -

1. Enables IDEs to provide intelligent hints as we type.
2. Spots common errors as we type the code or at compile time.
3. Makes code easier to read and understand.

## Compile it to javaScript

We can use TypeScript npm package to compile it to javaScript.

1. npm install -g typescript . (It will make command tsc available to the console).

2. Create a file example.ts in your Desktop.

```javaScript
	function greeter(person: string) {
	    return "Hello, " + person;
	}
	var user = "Mark";
	document.body.innerHTML = greeter(user);
```
3. Compile it with `tsc ~/Desktop/example.ts`.

4.An example.js file will get generated at the same location.

```javaScript
	function greeter(person) {
	    return "Hello, " + person;
	}
	var user = "Mark";
	document.body.innerHTML = greeter(user);

```
>Alternatively we can use *[PlayGround](http://www.typescriptlang.org/Playground)* .


## TypeScript features.

### Basic Types

#### String

```javaScript
let language: string = "Spanish";
language = 65 ; //Causes compile time error. Type 'number' is not assignable to type 'string'
```

#### Number
```javaScript
let score: number = 87;
```

#### Boolean
```javaScript
let isChecked: boolean = false;
```

#### Array
```javaScript
let players:string[] = ['Mark', 'John', 'Amy'];
```

#### Any
 We can assign any type of values to a variable with type 'any'

```javaScript
let dynamicVar: any = 'Now it holds String';
dynamicVar = 42;
dynamicVar = ['Mark', 'John', 'Amy'];
```

#### Void
Generally used in return dataType of a function which returns nothing.

```javaScript
function noReturnVal(): void {
    alert("I return nothing");
	//return 2;  //error number is not assignable to type void
}
```

#### Enum

Enum is used to provide user-friendly names to numeric values.
It's often convenient to developers.

```javaScript
//TypeScript

enum Lang {Js, Go, Scala};
let myLang: Lang = Lang.Go;
```
Compiled JavaScript
```
var Lang;
(function (Lang) {
    Lang[Lang["Js"] = 0] = "Js";
    Lang[Lang["Go"] = 1] = "Go";
    Lang[Lang["Scala"] = 2] = "Scala";
})(Lang || (Lang = {}));

var myLang = Lang.Go;

```

structure of Object `Lang` looks like

```javaScript
Object {0: "Js", 1: "Go", 2: "Scala", Js: 0, Go: 1, Scala: 2}
```
So instead of directly doing `let myLang = 2;`, enum helps us do `myLang = Lang.Go;`


### Interfaces

In the previous section we have seen how the type safety works for primitive data types like number, string etc.

Interfaces help us to ensure type safety of the entire object. Let's say there is a `function printStudentInfo`, it takes a `student object` . How do we ensure each `property` of that object has a desired `type`? Answer is Interfaces.

```javaScript
interface Customer {
  name: string;
  phone?:string;
  address?:string;
}

function printCustomer(custObj: Customer) {
  console.log(custObj);
}

let myCust = {phone: "66661111", name: 'Mark', income: "high" };
// let myCust = {phone: "66661111", address: '1st strret'}; // error property name is missing.
printCustomer(myCust);

```

`Customer` object has only one required property `name` . Both `phone` and `address`
properties are optional so denoted with a `?` after property name.  Type-checker only ensures that the required properties are present (here only `name`) and they match the types required. So our additional propery `income` are not throwing any errors.

Interfaces are also capable of describing `function types`.We can also describe methods in an interface that are implemented in the class.

```javaScript
//TypeScript

interface StudentStruct {
    name: string;
    isSenior(age: number): boolean;
}

class Student implements StudentStruct  {
    name: string;
    isSenior(age: number) {
       return age > 15
    }
    constructor(name: string) {
      this.name = name;
   }
}

```
Compiled JavaScript

```javaScript
var Student = (function () {
    function Student(name) {
        this.name = name;
    }
    Student.prototype.isSenior = function (age) {
        return age > 15;
    };
    return Student;
}());

```

`isSenior` is a method that takes **one argument of type number** and **returns `boolean`**.

### Classes

 Classes are introduced in ECMAScript 6 to facilitate JavaScript's existing prototype-based inheritance.Before we deep dive into ES6 classes in details let's discuss a bit about prototypical inheritance of javaScript.

In a nutshell, prototypal inheritance is when an object inherits from another object.

```javaScript
 // Function object acts as a combination of a prototype
 // to use for the new object and a constructor function to invoke.

 var Answer = function(quest){
   this.question = quest
 }
 //We can only add prototype to a function object.
 Answer.prototype.say = function(){
   return 'Answer to — " ' + this.question + ' " is 42';
 }
 // `new` creates an instance of the specified Function.

 var myAnswer = new Answer('How big is the universe');
 myAnswer.say();

```

Interestingly all `this` properties (`question`) and `prototype` properties are available to
all the instances of `Answer` (here `myAnswer`).

#### Why use `prototype` over `this`?

 Prototype methods are shared among all the instances. Functions declared as properties of `this` inside a constructor *copies* them across all instances, so *consumes more memory*.

#### How method overriding works in javaScript ?

```javaScript
function BaseClass() {
};

BaseClass.prototype.method1 = function () {
    alert('Base method 1');
};

BaseClass.prototype.method2 = function () {
    alert('Base method 2');
};


function ChildClass() {
}

// Function ChildClass extending BaseClass.
// This is how a function inherits all prototype properties of another Function.
// By assigning new instance of the BaseClass to it's own prototype.

ChildClass.prototype = new BaseClass();
//fixing prototype pointer. Recommended but not always necessary.
ChildClass.prototype.constructor = ChildClass;


ChildClass.prototype.method2 = function () {
    alert('Child method 2');
};

let myClass = new ChildClass();
myClass.method1(); // Base method 1
myClass.method2(); // Child method 2

```



#### ES6 classes.

ES6 classes facilitate same prototypical inheritance discussed above . Under the hood classes are `special functions`, where all the `instance methods` of class get translated to Function prototype methods and `this` properties of the class are translated to `this` properties of the function.


```javaScript
class Rect {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  area(){
    return  this.height *  this.width;
  }
}
```
Compiled version (using [Closure Compiler](https://closure-compiler.appspot.com/home))

```javaScript
var Rect = function(height, width) {
  this.height = height;
  this.width = width;
};

Rect.prototype.area = function() {
  return this.height * this.width;
};

```

### Classes in TypeScript and static property

TypeScript compiler works slightly different, but underline principles are same.
In TypeScript we should declare the `this` variables outside constructor before initializing them in constructor.

```javaScript
class SayHi {
    who: string;
    constructor(name: string) {
        this.who = name;
    }
    greet() {
        return "Hello, " + this.who;
    }
    static purpose(): string{
        let purposeText:string = 'I greet people';
        return purposeText;
    }
}

let greeter = new SayHi("world");
greeter.greet();
SayHi.purpose();

```

here's the compiled code (using TypeScript compiler)

```javaScript
var SayHi = (function () {
    function SayHi(name) {
        this.who = name;
    }
    SayHi.prototype.greet = function () {
        return "Hello, " + this.who;
    };
    SayHi.purpose = function () {
        var purposeText = 'I greet people';
        return purposeText;
    };
    return SayHi;
}());
var greeter = new SayHi("world");
greeter.greet(); // "Hello, world"
SayHi.purpose(); // "I greet people"
```

Notice the difference between TypeScript compiler and Closure Compiler output
code. TypeScript compiler assigns the output of an immediate function invocation
`(function(){}())` to the className variable. And that invoked function returns a function object.

* `instance methods` of class (`greet`) get translated to Function prototype methods
* `this` properties of the class (`who`) are translated to `this` properties of the function.
* `static` properties of the class become direct properties of the function (`SayHi.purpose = function(){...}`) and can be accessed directly without creating any instance (`SayHi.purpose`).

### Inheritance

In TypeScript/ES6 we use `extends` keyword to inherit from a parent class.

```javaScript
class WebAPP {
    theme:string;
    constructor(color: string) { this.theme = color; }
    getTheme(): void {
        alert("Theme of the app is " + this.theme);
    }
}

class FB extends WebAPP {
    purpose:string;
    constructor(color: string, usage: string ) {
        super(color); // call constructor of parent class from child.
        this.purpose = usage;
    }
    getPurpose():void{
        alert("It's purpose is " + this.purpose);
    }
}

let myFB = new FB('blue', 'social networking');
myFB.getTheme(); // Theme of the app is blue
myFB.getPurpose(); // It's purpose is social networking

```

#### Private/Public modifiers

In TypeScript, each member is public by default.Private members are not visible outside the class.

```javaScript
class Animal {
    private name:string;
    constructor(theName: string) { this.name = theName; }
    whoAmI() {
        alert('I am ' + this.name);
    }
}

let cheetah = new Animal('cheetah');
cheetah.whoAmI();
// cheetah.name(); not accessible here.

```

#### Accessors

It's used as a way of intercepting accesses to a member of an object. Let's say we have a private property `gems` of a class . `gems` can only be modified via `content` property, provided secret key `R@nd0m` is passed while creating the instance.

```javaScript
class ReadOnly {
    private secret: string;
    private gems: string;

    constructor(code:string){
        this.secret = code;
    }

    get content(): string {
        return this.gems || 'Not set';
    }

    set content(newGem: string) {
        this.secret === "R@nd0m" ? this.gems = newGem : alert("Unauthorized access");
    }

}

let reader = new ReadOnly("R@nd0m");
reader.content = "Pearl"; // this will call accessor `set content(newGem: string)`
console.log(reader.content);
let noaccess = new ReadOnly("xyz");
noaccess.content = "Diamond";
console.log(noaccess.content);

```
This gets compiled to

```javaScript
var ReadOnly = (function () {
    function ReadOnly(code) {
        this.secret = code;
    }
    Object.defineProperty(ReadOnly.prototype, "content", {
        get: function () {
            return this.gems || 'Not set';
        },
        set: function (newGem) {
            this.secret === "R@nd0m" ? this.gems = newGem : alert("Unauthorized access");
        },
        enumerable: true,
        configurable: true
    });
    return ReadOnly;
}());
var reader = new ReadOnly("R@nd0m");
reader.content = "Pearl";
console.log(reader.content); // Pearl
var noaccess = new ReadOnly("xyz");
noaccess.content = "Diamond"; //Unauthorized access
console.log(noaccess.content); //Not set

```
### Modules

Instead of putting lots of different names into the global namespace, we can wrap up our objects into a module.

#### Internal module

Let's say we have two generators. A hex color code generator (courtesy [Paul Irish](http://www.paulirish.com/2009/random-hex-color-code-snippets/)), and a prefilled number array generator.We will put them in different file , wrapped in `module`.We'll preface the methods with `export` so that they are visible outside the module.

`hexColorCode.ts`

```javaScript
module Generators{
    export function hexColor():string{
        return '#'+Math.floor(Math.random()*16777215).toString(16);
    }
}
```

`prefilledArray.ts`

```javaScript
module Generators{
    export function prefilledArray(len: number):number[]{
        return (n=>{let a=[];while(n--)a[n]=n+1;return a})(len);
    }
}
```
`main.ts`
To refer external file we use triple slash `///` followed by`<reference/>` tag with file `path`

```javaScript
/// <reference path="hexColorCode.ts" />
/// <reference path="prefilledArray.ts" />
console.log(Generators.prefilledArray(10));
console.log(Generators.hexColor());

```
Compiling the code `tsc --out combined.js main.ts` will generate following `combined.js`

```javaScript
var Generators;
(function (Generators) {
    function hexColor() {
        return '#' + Math.floor(Math.random() * 16777215).toString(16);
    }
    Generators.hexColor = hexColor;
})(Generators || (Generators = {}));
var Generators;
(function (Generators) {
    function prefilledArray(len) {
        return (function (n) { var a = []; while (n--)
            a[n] = n + 1; return a; })(len);
    }
    Generators.prefilledArray = prefilledArray;
})(Generators || (Generators = {}));
/// <reference path="hexColorCode.ts" />
/// <reference path="prefilledArray.ts" />
console.log(Generators.prefilledArray(10));
console.log(Generators.hexColor());

```

#### External modules.

To use external module in type script there are 2 options -

* Use nodeJs (Server side javaScript)
* Use require.js (mainly used for Client side javaScript)

Here, we will use only demonstrate executing server side Js with external module here.

First, we will export our class/function/variable etc. directly from respective .ts file.

```javaScript
// filledArray.ts

export function prefilledArray(len: number):number[]{
  return (n=>{let a=[];while(n--)a[n]=n+1;return a})(len);
}

// hexColor.ts

export function hexColor():string{
  return '#'+Math.floor(Math.random()*16777215).toString(16);
}

// We will import these in file where we want to use them .
// main.ts

import * as hexColor from "./hexColor";
import * as prefilledArray from "./filledArray";

console.log(prefilledArray.prefilledArray(10));
console.log(hexColor.hexColor());
```
While compiling the files use `--module` flag `commonjs` (for node) or `amd` for client side (require.js) . So running `tsc --module commonjs main.ts` would yeild in three different files.

```javaScript
//filledArray.js

function prefilledArray(len) {
    return (function (n) { var a = []; while (n--)
        a[n] = n + 1; return a; })(len);
}
exports.prefilledArray = prefilledArray;

//hexColor.js

function hexColor() {
    return '#' + Math.floor(Math.random() * 16777215).toString(16);
}
exports.hexColor = hexColor;

//main.js

var hexColor = require("./hexColor");
var prefilledArray = require("./filledArray");
console.log(prefilledArray.prefilledArray(10));
console.log(hexColor.hexColor());

/* And when we execute the final `main.js`
 *  with `node main.js` we get following output.
	*
 *[ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]
 * #a42b37
*/
```
Alternatively running `tsc --module amd main.ts` yeild in three different javaScript files
which we can use in client side javaScript using require.js.

### Generics

Generics helps us create components that can work over a variety of types rather than a single one. Let's say we want to ensure that if I pass a function an array of integer (or an array of string) then that function's return dataType should be an array of integer(or an array of string).

```javaScript
function reverse<T>(items: T[]): T[] {
    return items.reverse();
}

var source = [0, 1, 2];
var backward = reverse(source);
console.log(source); // 2,1,0

// Returned data is an array of integer .
// setting array element to non integer value causes error.
source[0] = '1';     // Error!
```

## Conclusion
