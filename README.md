# TypeScript

## Getting started

1. **Install Node.js and npm**

   Before you can start a TypeScript project, you need to have Node.js and npm
   (Node Package Manager) installed on your computer. If you do not, you can
   download them from the [official website](https://nodejs.org/en). A download
   of Node.js comes with npm without needing a separate download.

2. **Initialize a new npm project**

   ```bash
   npm init -y
   ```

3. **Install Typescript**

   a. **Globally (optional, but recommended)**

   You can install TypeScript globally on your system to access the TypeScript
   compiler (tsc) from the command line:

   ```bash
   npm install -g typescript
   ```

   b. **As a project dependency**

   Alternatively, you can install it as a local dependency to your project.

   ```bash
   npm install typescript --save-dev
   ```

4. **Create a Typescript configuration file (tsconfig.json)**

   The TypeScript compiler uses a configuration file to understand how to
   compile your TypeScript code. You can create this configuration file manually
   or use TypeScript's interactive setup:

   ```bash
   npx tsc --init
   ```

   This command will generate a tsconfig.json file in your project directory.
   We will revisit this file later.

5. **Make a TypeScript file**

   Now you're ready to start writing TypeScript code in your project directory.
   Create a TypeScript file with the .ts extension, for example, `app.ts`.

## Understanding static typing

TypeScript is a statically typed superset of JavaScript.

In JavaScript, you declare variables without specifying their types and the type
is determined dynamically at runtime. For example:

```js
let x = 10;
```

TypeScript, on the other hand, allows you to specify the type of a variable when
you declare it. This means you explicitly tell the TypeScript compiler what kind
of data a variable will hold, and it checks that you're using it correctly
throughout your code.

```ts
let x: number = 10; // x can only hold numbers
```

TypeScript includes most of the data types that you find in regular JavaScript,
but it also introduces additional data types to provide better type checking and
annotations. For a full list of available types, see the
[docs](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html).

## Building a TypeScript file

1. Open the `app.ts` file you made earlier and add the following code:

   ```ts
   function greet(name) {
     return `Hello, ${name}!`;
   }

   console.log(greet("John"));
   ```

2. Next, run the Typescript compiler to turn this code into JavaScript.

   ```bash
   tsc app.ts
   ```

   You should notice a new `app.js` file appear. Take a look in this file. Does
   anything look different?

3. Pat yourself on the back. You just compiled your first TypeScript file!

## Hey wait that was just JavaScript

You probably noticed that the code we added to `app.ts` was no different than
ordinary JavaScript code. And yet, it still considered valid by the TypeScript
compiler. What gives?

TypeScript is designed to be a **superset** of JavaScript, which means that any
valid JavaScript code is also valid TypeScript code. TypeScript extends
JavaScript by adding static typing and additional features, but it does not
remove or change any existing JavaScript features or syntax.

If you wanted, you could take an existing JavaScript file (with a .js
extension), rename it to have a .ts extension, and it would work as TypeScript
without modification. TypeScript will treat it as having the `any` type for all
variables and expressions, effectively opting out of static type checking for
that code.

However, to take full advantage of TypeScript's benefits, you can gradually add
type annotations to your JavaScript code. By doing so, you can enhance code
readability and catch type-related errors at compile time, even in existing
JavaScript files.

Let's do that now. Modify your app.ts file to include static type checking, like
so:

```ts
function greet(name: string) {
  return `Hello, ${name}!`;
}

console.log(greet("John"));
```

Compile the file again and take a look at `app.js`. You'll notice that it still
looks the same. So what's the point?

Change the value `"John"` in the console.log function to the number `5`. You
will see that VSCode is quick to point out the problem you've just introduced to
your code. The types don't match! The parameter `name` was expecting to receive
a string argument, but it got a number instead.

![console-error](./Images/console-error.png)

If you try to compile this code, the compiler will yell at you as well.

![compiler-error](./Images/compiler-error.png)

**This** is the power of TypeScript. Static typing is added to your JavaScript
code so that you can avoid type-related errors.\
Your code is now more explicit and predictable. 🎉🎉

## Interfaces

Interfaces define the structure of **objects**, which can be used to describe
the shape of classes, function parameters, and object literals.

### Declaring interfaces

You declare an interface using the _interface_ keyword, followed by the
interface name and a set of properties or method signatures enclosed in curly
braces. For example:

```ts
interface Person {
  name: string;
  age: number;
}
```

Here, we've declared an interface named Person with two properties: name of type
_string_ and age of type _number_.

### Using interfaces

You can use interfaces to define the shape of objects, and TypeScript will check
that objects conform to the interface's structure. Using the interface we just
declared as an example:

```ts
let person: Person = {
  name: "John",
  age: 30,
};
```

Here the person object must have both name and age properties of the correct
types to satisfy the Person interface. You can try this out by putting the
interface and person variables in a `person.ts` file. If we change the value of
age from `30` to `"30"` we can see this error.

![person code](./Images/person-code.png)
![person error](./Images/person-error.png)

### Optional properties

You can make properties optional in an interface by adding a `?` after the
property name:

```ts
interface Person {
  name: string;
  age?: number;
}
```

With age marked as optional, an object can have or omit the age property when
implementing the Person interface.

### Readonly properties

You can mark properties as readonly, meaning they can't be modified once
assigned:

```ts
interface Point {
  readonly x: number;
  readonly y: number;
}

let point: Point = { x: 0, y: 0 };
// point.x = 10;
// Error: Cannot assign to 'x' because it is a read-only property.
```

### Function signatures

Interfaces can also describe function signatures. For example, you can define an
interface for a function that takes two numbers and returns a number:

```ts
interface MathFunction {
  (x: number, y: number): number;
}

let add: MathFunction = function (a, b) {
  return a + b;
};
```

### Extending interfaces

You can extend interfaces by using the `extends` keyword to create a new
interface that inherits properties and methods from one or more existing
interfaces. For example, if we wanted to have an `Employee` interface that
contianed `name`, `age`, and `employeeId` properties, we may notice that we have
already defined an interface that includes `name` and `age`. We could extend our
`Employee` interface off of our `Person` interface and avoid repetition.

```ts
interface Employee extends Person {
  employeeId: number;
}

let employee: Employee = {
  name: "Alice",
  age: 25,
  employeeId: 12345,
};
```

In this example, Employee extends Person, inheriting the name and age
properties.

### Implementing interfaces with classes

You can `implement` an interface in a class by ensuring the class provides
implementations for all the properties and methods declared in the interface:

```ts
class Student implements Person {
  constructor(public name: string, public age: number) {}
}

let student: Student = new Student("Bob", 21);
```

### Function interfaces and call signatures

You can also use interfaces to describe the shape of functions. This is
particularly useful when defining callback functions or higher-order functions:

```ts
interface OnClick {
  (event: MouseEvent): void;
}

function registerClickHandler(callback: OnClick) {
  // ...
}
```

### Index signatures

Interfaces can define index signatures for objects with dynamic property names:

```ts
interface Dictionary {
  [key: string]: string;
}

let colors: Dictionary = {
  red: "#FF0000",
  green: "#00FF00",
  blue: "#0000FF",
};
```

This allows you to create objects with arbitrary property names and values of a
specific type.
