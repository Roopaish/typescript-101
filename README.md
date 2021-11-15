# Typescript

## What is Typescript?

- Alternative to js
- Strictly typed
- support modern features (arrow functions, let, const) # these are new features of js, with typescript, these features will be translated to older version of js, so that older browser can support it
- extra (generics, interfaces, tuples etc)

## Installation

Install Typescript compiler

```bash
npm install typescript
```

Convert ts to js

```bash
tsc sandbox.ts file_name.js
# Or
tsc sandbox.ts
```

To watch changes in typescript and automatically compile to js

```bash
tsc sandbox.ts -w
```

## basics

- Const and Variables

```ts
// Every code is the same as js
// Constants

const inputs = document.querySelectorAll("input");

console.log(inputs);

inputs.forEach((input) => {
  console.log(input);
});

// Variable, types can't be changed
let age = 32;
let name = "Luigi";
let isFamous = true;
```

- Functions

```ts
// Function
func circ = (diameter) => {
  return diameter * Math.PI;
}

circ(10)
circ('hello') // no error in code

// Fix, inferring the type
func circ = (diameter: number) => {
  return diameter * Math.PI;
}

circ('hello') // error will be thrown
```

- Arrays and Objects

```ts
// Arrays
let names = ["sam", "tom"];
names.push("jerry");
names.push(10); // error in code, unlike js

let mixed = ["sam", 1, "tom"];
mixed.push(10); // no err
mixed.push("jerry"); // no err

// Objects
let employee = {
  name: "sam", // this property should always be string
  job: "QA Tester",
  age: 30, // this property should always be number
}; // extra property can't be added
```

- Explicit Types

```ts
let character: string;
let age: number;
let isAdult: boolean;

let ninjas: string[]; // array is declared only
ninjas.push("hey"); // will throw error in console cause, array becomes null as we haven't assigned it anything

let ninjas: string[] = [];
ninjas.push("hey"); // now works

// Union types
let mixed: (string | number)[] = [];
mixed.push("hello");
mixed.push(20);

let uid: string | number;
uid = "123";
uid = 123;

// Objects
let ninjaOne: object;
ninjaOne = { name: "yoshi", age: 30 };

let ninjaTwo: {
  name: string;
  age: number;
  isAdult: boolean;
};

ninjaTwo = { name: "yoshi", age: 30, isAdult: true };
```

- Dynamic/Any Type

```ts
let age: any = 10;
age = true;
age = "hello";
age = { name: "luigi" };

let mixed: any[] = [];
mixed.push(5);
mixed.push("sam");

let ninja: { name: any; age: any };
```

## Better Workflow and tsconfig

Put all deployment code to public and source to src.  
Use this command to build tsconfig.json file.

```bash
tsc --init
```

Update rootDir and outDir in tsconfig

```json
{
  "compilerOptions": {
    "rootDir": "./src", // Input folder
    "outDir": "./public" // Output folder
  },
  "include": ["src"] // To only include ts file in src folder
}
```

Now type in these commands to compile and watch the js file `tsc` or `tsc -W`.

## Function Basis

```ts
// Explicit type
let greet: Function;

greet = (){
  console.log("Hello");
}

// ?optional parameter, default parameter (always at end of parameters)
const add = (a: number, b: number, c?:number|string, d:number|string = 10){
  console.log(a+b);
}

add(5+10);

// return type ():number, but optional as ts automatically infer the type
const minus = (a:number, b:number):number => {
  return a - b;
}

let result = minus(10,5);
```

## Type Aliases

```ts
type StringOrNum = string | number;

const logDetails = (uid: StringOrNum) => {};
const logDetails = (uid: string | number) => {}; // same as above

type objWithNum = { name: string; uid: StringOrNum };
const greet = (user: objWithNum) => {
  console.log(name);
};
```

## Function Signatures

```ts
// eg1
let greet: (a: string, b: string) => void;

greet = (name: string, greeting: string) => {
  console.log(`${name} says ${greeting}`);
};

// eg2
let calc: (a: number, b: number, c: string) => number;

calc = (numOne: number, numTwo: number, action: string) => {
  if (action === "add") {
    return numOne + numTwo;
  }
  return numOne - numTwo; // throws error if we add without this
};

// eg3
let logDetails: (obj: { name: string; age: number }) => void;

type person = { name: string; age: number };

logDetails = (ninja: person) => {
  console.log(`${ninja.name} is ${ninja.age} years old`);
};
```

## DOM & Type casting

```ts
// Typescript defines type for DOM selector(only with element) automatically
const anchor = document.querySelector("a");

if (anchor) {
  console.log(anchor.href); // anchor can be null too, so we have to do it like this else error will appear
}

// Alternative
const anchor = document.querySelector("a")!;

console.log(anchor.href);
```

```ts
const form = document.querySelector("form")!; // Type is defined automatically

const form = document.querySelector(".same-form")!; // Type is not defined

// Manually defining type, good for intellisense
const form = document.querySelector(".same-form") as HTMLFormElement;
```

```ts
amount: valueAsNumber; // turns string value to number when compiled to js
```

## Classes

```ts
class Invoice {
  client: string;
  details: string;
  amount: number;

  constructor(c: string, d: string, a: number) {
    this.client = c;
    this.details = d;
    this.amount = a;
  }

  format() {
    return `${this.client} owes \$${this.amount} for ${this.details}`;
  }
}

const invOne = new Invoice("mario", "work on a website", 250);

// We can also define array of Invoice objects
let invoices: Invoice[] = [];
invoices.push(invOne);

// We can also modify object property
invOne.client = "Yoshi";
invOne.amount = 100;
```

## Access Modifiers

```ts
class Invoice {
  readonly client: string; // value can't be changed, accessible everywhere
  private details: string; // accessible only in class
  public amount: number; // accessible everywhere

  constructor(c: string, d: string, a: number) {
    this.client = c;
    this.details = d;
    this.amount = a;
  }

  format() {
    // this.client = 'something'; not allowed
    return `${this.client} owes \$${this.amount} for ${this.details}`;
  }
}

const invOne = new Invoice("mario", "work on a website", 250);

// invOne.client = "Yoshi"; // not allowed
invOne.amount = 300;
// console.log(invOne.details); // not allowed
```

```ts
// ShortHand, only if using access modifiers
class Invoice {
  constructor(
    readonly client: string,
    private details: string,
    public amount: number
  ) {}

  format() {
    return `${this.client} owes \$${this.amount} for ${this.details}`;
  }
}
```

## Modules

Splitting code to different modular files.
Change tsconfig to target modern browsers cause it only works on modern browsers.

```json
{
  "compilerOptions": {
    ...
     "target": "es6",
     "module": "es2015"
    ...
  }
}
```

```html
<!-- Specifying the type as module in html while importing the module -->
<script type="module" src="sandbox.js"></script>
```

```ts
// a module
// use export to let Invoice class import in other file
export class Invoice {
  constructor(
    readonly client: string,
    private details: string,
    public amount: number
  ) {}

  format() {
    return `${this.client} owes \$${this.amount} for ${this.details}`;
  }
}
```

```ts
// Import the file, .js is required cause the final build will be .js file
import { Invoice } from "./classes/invoice.js";
```

Drawbacks: Have to make multiple requests for multiple files, older browser doesn't support it  
Solution: Use Webpack

## Interfaces

Used to infer certain type of structures for objects or classes.

```ts
interface isPerson {
  name: string;
  age: number;
  speak(a: string): void;
  spend(a: number): number;
}

// me should contain all the property of isPerson interface, and extra property can't be added
const me: isPerson = {
  name: "roopaish",
  age: 20,
  speak(text: string): void {
    console.log(text);
  },
  spend(amount: number): number {
    console.log(`I spent ${amount}`);
    return amount;
  },
};
```

Interfaces with classes

```ts
// Interface for a class
export interface HasFormatter {
  format(): string;
}
```

```ts
// class Invoice must follow implementation mentioned in HasFormatter
import { HasFormatter } from "../interfaces/HasFormatter.js";

export class Invoice implements HasFormatter {
  constructor(
    readonly client: string,
    private details: string,
    public amount: number
  ) {}

  format() {
    return `${this.client} owes \$${this.amount} for ${this.details}`;
  }
}
```

```ts
// Now we can define the type of variable
import { Invoice } from "./classes/invoice.js";
import { HasFormatter } from "./interfaces/HasFormatter.js";

let docOne: HasFormatter; // defining the type of variable

docOne = new Invoice("yoshi", "web work", 150); // works cause, Invoice implements HasFormatter

// Same with array
let docs: HasFormatter[] = [];
docs.push(docOne);
```

## Generics

```ts
// any type can be passed
const addUID = <T>(obj: T) => {
  let uid = Math.floor(Math.random() * 100);
  return { ...obj, uid };
};

let docOne = addUID({ name: "yoshi", age: 40 }); // {name: 'yoshi', age: 40, uid: 7}
let docTwo = addUID("hello"); // {0: 'h', 1: 'e', 2: 'l', 3: 'l', 4: 'o', uid: 88}
```

```ts
const addUID = <T extends object>(obj: T) => {
  let uid = Math.floor(Math.random() * 100);
  return { ...obj, uid };
};

let docOne = addUID({ name: "yoshi", age: 40 });
// let docTwo = addUID("hello"); // error, even if its generic it must be object/generic object
console.log(docTwo);
```

```ts
interface Resource<T> {
  uid: number;
  resourceName: string;
  data: T;
}

const doc1: Resource<object> = {
  uid: 1,
  resourceName: "person",
  data: { name: "roopaish" },
};

const doc2: Resource<string[]> = {
  uid: 2,
  resourceName: "shoppingList",
  data: ["shirt", "table"],
};
```

## Enums

```ts
enum Resource {
  person, // 0
  shoppingList, // 1
}
```

## Tuple

Order of type of variable in an array is same.

```ts
let tup: [string, number, boolean] = ["ryu", 40, true];
// let tup2: [string, number, boolean] = [ 40, 'ryu', true]; // not allowed
```
