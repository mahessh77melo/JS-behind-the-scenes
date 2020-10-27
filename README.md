# Working of JavaScript behind the scenes

---

## Features Of JavaScript :

<br>

### <u>High Level Language</u>

JavaScript is a high level language like python. Unlike c, c++ which are low level languages, to run JS, a developer doesn't need have the need to manage the resources. Everything kind of happens automatically in JS and in python. As you may know, the downside is that these programs are not very optimized like the low level languages.

### <u>Garbage collection</u>

These languages have inbuilt memory management, it removes used objects from the runtime memory and we don't need to do it manually. It's like these langagues have a cleaning guy

### <u>Just in time interpreted</u>

It is not compiled before running, and that is why typescript is used for. JavaScript is converted into machine understandable binary code before running. All objects, functions, variable assignments happen on the go in JS.

### <u>Multi paradigm </u>

Wait, what ???? what the heck is multi whatever! JavaScript has many methods and styles of programming.

1. Procedural Programming

2. Object oriented Programming

3. Functional Programming

Many languages come under only one of those paradigms. But JS does it all. More like **Lebron James**.

### <u>Prototype based and object Oriented</u>

Anything and everything in JS in an object. For example :  Functions, arrays etc.

Look at the following block of code.....

```js
let arr = [0,1,2,3]
arr.push(4)
const newArr = [7,8,9]
arr = arr.concat(...newArr)
console.log(arr)
// [0,1,2,3,4,7,8,9]
```

The methods `concat` and `push` are available from `Array.prototype` object. Any array ever created inherits this object thereby gaining access to these methods.

### <u>First class functions</u>

All functions are treated as variables in Python and JavaScript. This gives us the luxury of passing them as variables and calling functions inside functions. A simple and popular example of this would be the function that you pass into the Event Listeners.

Look at this block of code....

```js
const handleClick = ()=>{
    console.log('clicked')
}
document.querySelector('.div').addEventListener('click',handleClick)
```

Notice how the function `handleClick` is not called there, but instead it  is just passed into the event listener and it does the rest. Another way of using it would be...

```js
document.querySelector('.div').addEventListener('click',()=>handleClick())
```

here, the second argument of the event listener is just a function which in turn calls another function --> `handleClick()`

### <u>Dynamically interpreted</u>

Python people may find this word familiar. In JS, dataTypes are not fixed by the compiler or whatever and we have the advantage of assigning the types of the variables on the go. Variable types like `var` and `let` in ESnext allow us to keep changing the values and datatypes.

```js
let a='3'
a=45;
a = a+'3'
console.log(a)
// 48 or '453' depending on the browser.
```

this example explains why many people want JS to be a strongly typed language, (compiled and then executed) so that it runs the code as we expect every single time.

This reduces numerous possible bugs. Languages like Java, c++, ruby are strongly typed languages. If u want to experience strongly typed JS, then go take a look at **TypeScript** (TS).

### <u>Single threaded</u>

This is a complicated one. If you want to go full on in this topic, check out something called **Concurrency model** . The way in which JS executes multiple tasks at a time.

By default, JavaScript is capable if running a single task at a time and thats where the name *Single threaded* comes from.

So what happens when a tasks runs for a considerably long time ....say 5 seconds or something, it would block  the whole execution. 

```js
fetch(`${apiHeader}/movie?id=${current.id}`).then(res=>res.json())
.then(res=>{
    console.log(res.data)
}).catch(err=>{
    console.log(err.message)
})
```

 this is something we would have come across a lot. These fetch api calls usually take a particular amount of time to finish executing which is non-negligible. What happens to our **execution context** meanwhile??.

The execution context pushes these type of long running functions to the background and the *execution stack* keeps running the other functions. So it doesn't delay anything. After  the execution of the long running function in the background, it returns back to the main thread. This whole process is handled by something called an **Event Loop**.

**Note :** the name *long running* is kind of weird, so the correct word is **Asynchronous function**

---

<br>

## The JavaScript V8 Engine :

<br>

Every browser has a runtime engine with it. Browsers can't work without that. 

Browsers like *Chrome*, *Apple Safari*, *Microsoft Edge* use **Webkit engine**.

Whereas *Firefox* stands out and uses an engine called **Gecko**.

These are the engine that are used to render the web pages, but each of them has a different JavaScript engine.

| Browser | JS engine      |
| ------- | -------------- |
| Chrome  | Google's V8    |
| Safari  | JavaScriptCore |
| Firefox | SpiderMonkey   |
| Opera   | Carakan        |

<br>

Google's V8 is by far the most popular because it is used by popular frameworks such as **Node.js**, **Deno.js** etc. This is because node and deno run on the machine directly. Those stand out from the traditional front-end client side JavaScript and are capable of running on the machine itself. But that's a topic for a whole other day.

All these engines mentioned above convert the human readable js code into browser readable machine code before execution. For newbies, *engine* is not very different from a *compiler*.

Every engine is split into a *call stack* and a *heap*. The call stack is where the execution stack exists and the objects and the variables are stored in the heap.

### Difference between Compilation and Interpretation :-

In compilation, the whole code is converted into machine code all at one time and the execution occurs wayyy after the compilation. You might have seen two seperate shortcuts for compilation and execution in IDEs for JAVA, C++ etc.

In Interpretation, the whole code however, has to be converted into machine code, but the execution occurs simultaneously. As the compiler or *engine* reads the code line by line, it is converted into machine code just before execution.

As said earlier,dynamically interpreted languages are  slow to be interpreted.

This was acceptable until JavaScript was used only to annoy the users with childlish tricks in the web and color changing buttons etc. But in modern day world  where  ESnext is the trend, this cannot be acceptable. That's why JS is a **Just in time compiled** language. The execution starts as soon as the compilation is over. Execution is not manually triggered by the user. Conclusion......

~~JAVASCRIPT IS AN INTERPRETED LANGUAGE~~

### Hidden processes that occur with compilation and execution :-

1. Parsing : The whole code is read by the engine and something called an **AST** is generated. This represents all the variables declared and assigned values in the code. This *Abstract syntax tree* is then used in the process of compilation. 

2. Optimization: The compilation generates a less optimized version of a machine code initially. But then simulateously during execution, the machine code is again converted into human code and converted back to machine code in an optimized way. This process is like a cycle.

Different engines go through this process in their own ways.

---

<br>

## The JavaScript Runtime

<br>
Obviously, from what we know so far, the js engine  is the heart of the runtime. But js requires some other things to run too.

1. The js engine --> (call stack and the heap)

2. WEB APIs --> (access to the DOM, fetch() function, Timers)

3. Call-back queue --> (every callback function that should occur on a given condition)

All of the above are self explanatory. A point to be noted is that the node js runtime doesn't have access to the WEB APIs, and that's why it doesn't have access to the `document.querySelector()`, `fetch()` functions. Subsequently, it uses something called C++ bindings and a Thread pool. That's why it gains access to the `require()` method.

The call back queue is maintained by the **Event Loop** which is responsible for the non-blocking and concurrent behaviour of the js runtime. 

## Execution Context :-

During compilation, something called a *global execution context* is created. The top-level code is inside this context. Meaning, all the lines of code which aren't inside of a function. This is obvious because we want a function to be executed only when it is called ryt?? yup!

The variables that exist in this global context are *global variables*. JavaScript code always runs inside an Execution context. No matter how big a program is, it will have only **one global context**.

So what happens to functions, methods and callbacks? 

Each have their own and new execution contexts that contain necessary information to run the code block. 

All these execution contexts make up for the call stack. The callback functions are monitored and called by the **EVENT LOOP**.

What  is inside an execution context? 

1. Variable environment
   
   1. `let`  `const`  and `var`  declarations.
   
   2. functions and keywords
   
   3. the `arguments` object

2. The scope chain

3. the `this` object.

The scope chain and the this key word are created in  the *creation phase*.

**NOTE** : Arrow functions don't have an `arguments` object and also they don't have a `this` keyword. Both return undefined.

## The call Stack :-

Initially, the global context sits in the call stack. But things  start to deviate from boring only when a function call happens. This is when the execution context of that specific function is pushed into the call stack. After the call stack finished executing the function, it return back to the global context since that was the one that was being dealt with previously. 

This explains why the call stack is called the call **stack**. It executes the context at the top of the stack. After the execution is over and function returns something..... it is thrown out of the call stack and then again the top most context comes back into deal.

Lets complicate a bit. What  happens when a function is called inside another function.

Look at this code block....

```js
let x;
const first = () =>{
    let a = 2;
    const b = second(5,7);
    a = a+b;
    return a;
}
function second(c,d){
    return c+d;
}
x = first()
// x becomes 14
```

The global context contains the following four variables.

| Name   | Type         |
| ------ | ------------ |
| x      | `<let>`      |
| first  | `<function>` |
| second | `<function>` |
| x      | `<unknown>`  |

<br>

When the `first` function is called, a new execution context is created and seperate, variable environment and scope chain are created for it.

Inside the first function, the `second` function is called. This creates a new execution context. At this point of time the call stack looks like this.....

| Call Stack     |
| -------------- |
|                |
| `second()`     |
| `first()`      |
| Global Context |

<br>

The `first()` function waits for the second function to finish and when that happens, the `second()` functions is *popped* from the call stack. Now the global context waits until the `first()` function returns a value. This is a simple example, but things don't change much even when the scale is increased.

**TAKE-AWAY :** The call stack always executes the top most context it has inside it. The call stack ensures that the order of execution never gets lost.

<br>

---

## Scope

Scope is generally the indicator that where our variables and functions are available for access and where not. *Where do our variables live.*

In JavaScript , **Lexical  Scoping** is used. That is...... scope depends on where we place our functions and variables (location).

One might ask, "What is the difference between the variable environment of the execution stack and the scope??".

Well, for functions, both are the same.

### Three types of Scopes...

1. <u>Global Scope</u>
   
   1. Outside of any function block. Top-level code.
   
   2. Variables declared here are accessible anywhere in the code.

2. <u>Function Scope</u>
   
   1. Variables created here are accessible only inside the function, **Not outside**.
   
   2. Also called as *local scope*.

3. <u>Block Scope</u>
   
   1. New feature in ES6.
   
   2. Any variable inside a code block can be accessed only inside that specific block. 
   
   3. By code blocks, I mean... if blocks , for loop blocks etc.
   
   4. Since this is a new feature in ES6, block scoping only applies for `const` and `let` type variables. However, the `var` is much more flexible.

```js
// Example for global scope
const x = 5;
if(true){
    console.log(x)
}
// 5 is printed to the console
```

```js
var x=5;
const y=6;
const printEveryThing = ()=>{
    let z = 7;
    console.log(x,y,z);
}
printEveryThing();
// 5,6,7 are printed to the console

console.log(z)
// -- this gives Reference error
```

**NOTE :** Functions  are also block scoped in 'strict mode'. *Strict mode* can be achieved with typing '`use strict';` in the first line of code.

One important thing to note is that, the scope chain has **nothing to do** with the call stack or the order in which the functions are  called.

Observe the following example.......

```js
function second(){
    const c = 3;
    third();
}
function third(){
    var d = 4;
    console.log(d+c);
}
// this throws a reference error since c is not available to third() func
```

**TAKE-AWAY :** The scope chain is a one way street. The outer scope never have access to the inner scoped variables.

A scope chain is equal to adding all the environment variables of all the parent scopes.

<br>

---

## A simple example :

### Case One :-

```js
'use strict';
function calcAge(birthYear) {
  const age = 2020 - birthYear;
  const printAge = () => {
    const output = ` ${firstName} is ${age} years old 
                        and is born in ${birthYear}`;    
    console.log(output);
  };
  printAge();
  return age;
}
const firstName = 'Mahesh';
const age = calcAge(1999);
```

As you may see, everything goes well and good in this function. 

Because when the function scope can't find a `firstName` variable, it looks outside its scope until it finds one, which it does....in this case.

the output would be...

```
Mahesh is 21 years old and is born in 1999
```

### Case Two :-

but, when we interchange the last two lines...

```js
'use strict';
function calcAge(birthYear) {
  const age = 2020 - birthYear;
  const printAge = () => {
    const output = ` ${firstName} is ${age} years old and 
                     is born in ${birthYear}`;
    console.log(output);
  };
  printAge();
  return age;
}
const age = calcAge(1999);
const firstName = 'Mahesh';
```

This throws a reference error. Eventhough vscode will show u the value of the global variable when u hover over the `firstName` variable inside the `printAge` function, js won't like that. This is because when the calcAge function enters the execution stack, the firstName variable hasn't entered the global scope yet. Therefore, the output will be....

```
ReferenceError: Cannot access 'firstName' before initialization
```

**Note :** You may want to notice that I have declared the `age` variable (const) twice . But Js feels ok with it, that's because const and let are *block scoped*. And moreover, one of them is *function scoped*.

Since we changed topics midway, let us play with scopes...

```js
'use strict';
function calcAge(birthYear) {
  const age = 2020 - birthYear;
  const printAge = () => {
    const output = `${firstName} is ${age} years old and
                     is born in ${birthYear}`;
    if (age > 18) {
      var adult = true;
    }
    console.log(adult ? 'You are an adult' : 'Chota bachaaa!!!');
    console.log(output);
  };
  printAge();
  return age;
}
const firstName = 'Mahesh';
let age = calcAge(1999);
```

As u may think, it says, `You are an adult` xD. JS feels okay with this behaviour. But when u try to declare adult  as a `const` or `let` then JS yells at you. 

**Note :** Starting from ES6, functions are also treated as block scoped.

### Case Three :

```js
'use strict';
function calcAge(birthYear) {
  const age = 2020 - birthYear;
  const printAge = () => {
    const firstName = 'Override';
    const output = `${firstName} is ${age} years
                    old and is born in ${birthYear}`;
    console.log(output);
  };
  printAge();
  return age;
}
const firstName = 'Mahesh';
let age = calcAge(1999);
```

Now the `firstName` declared *inside* the `printAge` function is used, because the js engine searches for a variable in the parent scope, only if it's not in the current scope.

And the name 'Override' is kinda cool.

```
Override is 21 years old and is born in 1999
```

This explains why multiple functions can have the same argument names. Because the scope of the arguments end when the function finishes executing.

**NOTE OF THE DAY:** The `window` object has access to the var global variables. But it does not have access to the global `const` and `let`.

### Closures : A complex application of scopes :-

So we would have come across some weird code blocks wherin functions return functions....WAIT WHATTT????? yes they doo....And vscode shows their prototype like this....

```js
const doubleArrow: () => () => void
```

And they resemble this structure, they are also similar to the `bind`. But let's stick to the point. 

The noob version of that funciton returning function would be

```js
function greet(greeting) {
  return function (name) {
    console.log(`${greeting} ${name}`);
  };
}
greet('hey')('Mahesh');
// can also be written as
const inner = greet('Hi');
inner('Kevin Durant'); // same result
```

Let that sink in........!!!!!!

Now onto closures...

Inner functions have access to the parent function's scope, Even after they are returned. Yes, read that again...*even after they are returned*. To make it really clear... 

A function always has access to the variable environment of the execution context in  which is was created.

Example time...

```js
const secureBooking = () => {
  let passengers = 0;
  return () => {
    passengers++;
    console.log(`${passengers} passengers.`);
  };
};
const booker = secureBooking();
booker();
booker();
booker();
```
Nothing happens even after the secureBooking function is called and is popped out of the execution context. Drama starts after the *booker* function is called...not one, not two, not three, not four, not five...alright cut it.. that was a james reference 😂.
Coming back to cricket, the passengers variable was incremented 3 times. 

```
// Output
1 passengers.
2 passengers.
3 passengers.
```

So even after the secureBooking functions comes out of the execution context , it is   able to access the parent scope variables (passengers).  The returned value which is a  function is stored in the booker variable. So the booker function now has access to the passengers variable which was  initially not in its scope.

<br>

---

<br>

## Hoisting

The process of JS engine scanning thru our code and setting all the `var`s to undefined before they are assigned their respective values is called *hoisting*.

That was a *long time ago*.

Before es6, arrow functions, const , let etc.

Let us cover the easy part first.

1. Hoisting sets all the `var`s to `undefined` at the start of execution, then when the code has started to execute line by line, these values change.
- when u try to use a `var` before it is initialized, it returns `undefined`.
2. The main advantage of *hoisting* is that functions can be called before they are declared. This was the sole purpose of *hoisting*. This was the **main intention** of *hoisting*.
- This makes the code much much cleaner, (general opinion).

However, this causes numerous bugs in the code, when devs try to access the variables before they are initialized. These bugs are hard to locate. That's where const and let come into play. 

- `const` and `let` are identified by the compiler, but not hoisted. Therefore, when the program tries to access it before the initialization, JS yells at you saying *'can't access a variable before initialization'*. However, if you try to access a variable which is never declared in your whole code, then JS yells differently saying *'can't access an undeclared variable'*.

- What happens in the case of function expressions and arrow functions?? DUDE!!!! they are nothing but variables acoording to our guy V8. Obviously they are treated the same way as `const` or `let` (in the weird case, if u declared an arrow func. as let). 

- Or if u declare a function expression as a `var` ..which is highly discouraged, then it is set to `undefined` during hoisting.

This explains why arrow functions can't be accessed before they are declared. 

#### **Nerd point :**

`const` and `let` have something called a *TDZ* - (**Temporal dead zone**). Inside their scope, where they are identified by the compiler, until the declaration occurs, they are inside the *TDZ*. Whenever they are tried to used inside the *TDZ* , the 'uninitialized' error is thrown. *TDZ*  is the reason why we are able to identify these errors. 

---

## The 'this' Keyword

<br>

It is a special variable that is created for the execution context of each and **every function**. `this` is not static, its value keeps changing everytime a different execution context is in top of the call stack. And the value is actually assigned to `this` when the function is actually called. 

The value of `this` is highly affected by the type of the function inside which it exists...

- **Method functions**: the `this` keyword points to the **object** which calls the method.

- **Simple functions**: the `this` keyword is set to `undefined`. May also point to the *window* object of the browser depending on the mode.

- **Arrow functions**: Usually don't have a `this` keyword and returns an empty object. But when surrounded by another function, the `this` keyword of the inner arrow function points to the `this` of the outer function.Therefore, if the outer function is a *method*, then `this` points to that corresponding object.

- **Event listeners**: In this case, the `this` keyword points to the DOM element that is responsible for the *event listener*.

### Examples....

1. **Method :**

```js
const person = {
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: function () {
    const age = 2020 - this.birthYear;
    console.log(this);
    this.age = age;
  },
  age: '',
};
person.calcAge();
console.log(person);
```

and the output  for the above seen code block would be.......

```js
{
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: [Function: calcAge],
  age: ''
}
{
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: [Function: calcAge],
  age: 21
}
```

If u look at the fist time the object is printed, the age object is still `age:''`. This comes from console logging the `this` object from inside the *calcAge* function. At that time, the this keyword points to the object that calls the function, which is the *person* object. 

The second time the object is printed..... that is from the last line of the code wherein the person object gets updated after calling the *calcAge* function.

2. **Simple function :**

```js
const simpleFunc = function () {
  console.log('I am a simple function');
  console.log(this);
};
simpleFunc();
```

this above example is just a simple function definition and the subsequent call of the same. The `this` keyword inside this function returns `undefined`.

```js
'I am a simple function'
undefined
```

3. **Arrow function Case :**

Now this is a bit complicated, or atleast that's how I see it. First u have to understand  that the *example 1 for method function case* can also be written in this form.....

```js
const calcAge = function () {
  const age = 2020 - this.birthYear;
  console.log(this);
  this.age = age;
};

const person = {
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: calcAge,
  age: '',
};
person.calcAge();
console.log(person);
```

And the output would remain the same. Now let's start the complication process xD. Let me insert a useless *arrow function* inside the calcAge function expression.

```js
const calcAge = function () {
  const age = 2020 - this.birthYear;
  // console.log(this);
  const printAge = () => {
    const firstName = this.name;
    const output = `${firstName} is ${age} years old
                     and is born in ${this.birthYear}`;
    console.log(output);
  };
  printAge();
  this.age = age;
};

const person = {
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: calcAge,
  age: '',
};
person.calcAge();
console.log(person);
```

Now all we are going to talk about is the `printAge` arrow function inside the calcAge function. Cus we are already pretty clear with the other parts of the code. Now the above written code gives the following output.

```js
'Mahesh is 21 years old and is born in 1999'
{
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: [Function: calcAge],
  age: 21
}
```

Everything works perfectly. This is because...

`this.name` inside the arrow func. returns the `name` attribute of the `this` object which is currently pointing to the object that is called the outer function. And it's the same case with `this.birthYear`. But `age` is simply the variable in the outer scope.

This is what happens when an arrow function is inside a method and tries to access the `this` object. This was a new and *insert fire emoji* feature that was introduced in ES6.

**BUT**, (there's always a but)....What happens when we try to use arrow function as a object method and try to use `this` inside it directly.....

```js
const calcAge = function () {
  const age = 2020 - this.birthYear;
  // console.log(this);
  this.age = age;
};

const printAge = () => {
  const firstName = this.name;
  const output = `${firstName} is ${this.age} years old and is
                    born in ${this.birthYear}`;
  console.log(output);
};

const person = {
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: calcAge,
  printAge: printAge,
  age: '',
};

person.calcAge();
console.log(person);
person.printAge();
```

I have taken the *printAge* arrow guy out of the *calcAge* method and declared it as a seperate method. Well, even after calculating the age and calling the *printAge* function via the *person* object...this is what turns out........

```
{
  name: 'Mahesh',
  birthYear: 1999,
  calcAge: [Function: calcAge],
  printAge: [Function: printAge],
  age: 21
}
undefined is undefined years old and is born in undefined
```

Enough said !!!!!

4. **Event Listeners :**

This is pretty straight forward.

The `this` keyword inside the event listener points to the DOM element attached with the event listener.

```js
// the event.js file
const header = document.querySelector('h1');
header.addEventListener('click', function () {
  console.log(this);
});
```

```html
 <!-- The html file -->
  <body>
    <h1>Try clicking the header</h1>
    <script src="event.js"></script>
  </body>
```

```
// Browser console after clicking the header mulitple times

<h1>Try clicking the header</h1>                      event.js:3
<h1>Try clicking the header</h1>                      event.js:3
<h1>Try clicking the header</h1>                      event.js:3
```

**Quick blooper :** After finishing the first 3 parts, I went on to code the 4th part. Since I am an arrow lover, (not the Arrowverse)... i tried using an arrow function as the *callback* for the *event listener* in *event.js* and that eventually logged the `window` object to the console. YIKES  !!!!!

**Take-away :** When in a browser, the `this` object returns the *window* object of the browser. But I find it comfortable to run plain js in my system with **node**.

```
node event.js
```

The `this` keyword inside the arrow function now returns an empty object `{}`.

<br>

---
