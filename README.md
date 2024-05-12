# js-obj-arr-fn-techniques

```js
compare objects:
let a = { name: 'Dionysia', age: 29};
let b = {name: 'Dionysia', age:29};

console.log(a === b); // false
console.log(a == b); // false
Both a and b are seemingly the same, yet the objects are not equal when I use === or ==.
The reason for this result has to do with how JavaScript approaches testing for equality when it comes to comparing primitive and non-primitive data types.
The difference between primitive and non-primitive data types is that:
- primitive data types are compared by value.
- non-primitive data types are compared by reference.

let a = { name: 'Dionysia', age: 29};
let b = a;

console.log(a === b); // true
In the example above, thanks to the line let b = a;, both variables have the same reference and point to the same object instance, so the result is true.


let a = { name: 'Dionysia', age: 29};
let b = { name: 'Dionysia', age: 29};
JSON.stringify():
console.log(JSON.stringify(a) === JSON.stringify(b)); // true
_.isEqual()
console.log(_.isEqual(a, b)); // true

===========

compare array:
let a = [11, 22, 33];
let b = [11, 22, 33];
JSON.stringify():
console.log(JSON.stringify(a) === JSON.stringify(b)); // true
.toString():
console.log(a.toString() === b.toString()); //true
_.isEqual()
console.log(_.isEqual(a, b)); // true
every(), loops, new Array() // create new array from array

===========

clone: shallow copy / deep copy 
There are two ways to clone an object in Javascript:
Shallow copy: means that only the first level of the object is copied. Deeper levels are referenced.
Deep copy: means that all levels of the object are copied. This is a true copy of the object.

Shallow copy:
A shallow copy can be achieved using the spread operator (…) or using Object.assign():
const obj = { name: 'Version 1', additionalInfo: { version: 1 } };

const shallowCopy1 = { ...obj };
const shallowCopy2 = Object.assign({}, obj);

shallowCopy1.name = 'Version 2';
shallowCopy1.additionalInfo.version = 2;

shallowCopy2.name = 'Version 2';
shallowCopy2.additionalInfo.version = 2;

console.log(obj); // { name: 'Version 1', additionalInfo: { version: 2 } }
console.log(shallowCopy1); // { name: 'Version 2', additionalInfo: { version: 2 } }
console.log(shallowCopy2); // { name: 'Version 2', additionalInfo: { version: 2 } }

As you can see in this code snippet:

After updating a property in the first level of the cloned objects, the original property is not updated.
After updating a property in a deeper level, the original property is also updated. This happens because, in this case, deeper levels are referenced, not copied.

Deep copy:
A deep copy can be achieved using JSON.parse() + JSON.stringify():

const obj = { name: 'Version 1', additionalInfo: { version: 1 } };

const deepCopy = JSON.parse(JSON.stringify(obj));

deepCopy.name = 'Version 2';
deepCopy.additionalInfo.version = 2;

console.log(obj); // { name: 'Version 1', additionalInfo: { version: 1 } }
console.log(deepCopy); // { name: 'Version 2', additionalInfo: { version: 2 } }

As you can see in this code snippet:

After updating a property in the first level of the cloned objects, the original property is not updated.
After updating a property in a deeper level, the original property is neither updated. This happens because, in this case, deeper levels are also copied.

===========

different ways to create object
1. Object Literals
const car = {
  make: 'Toyota',
  model: 'Corolla',
  year: 2021
};

2. The new Object() Syntax
const person = new Object();
person.name = 'John';
person.age = 30;
person.isEmployed = true;

3. Constructor Functions
function Smartphone(brand, model, year) {
  this.brand = brand;
  this.model = model;
  this.year = year;
}

const myPhone = new Smartphone('Apple', 'iPhone 13', 2021);

4. The Object.create() Method
const animal = {
  type: 'Animal',
  displayType: function() {
    console.log(this.type);
  }
};

const dog = Object.create(animal);
dog.type = 'Dog';
dog.displayType(); // Output: Dog

5. ES6 Class Syntax

===========

promise, async/await, try/catch/finally

Promise:
https://www.programiz.com/javascript/promise
Promise handle asynchronous operations with three state: pending, fulfilled, rejected. Promise() constructor takes a function as an argument. The function also accepts two functions resolve() and reject()
Promise chaining, to handle more than one asynchronous task, one after another.You can perform an operation after a promise is resolved using methods such as then(), catch(), finally()

let promise = new Promise(function(resolve, reject){
     //do something
     if (true) return resolve('fulfilled')
     reject('rejected')
});

promise.then((a) => a).then((b) => b).catch((e) => e).finally(() => 'executed')

Promise Methods:
all([iterable])	Waits for all promises to be resolved or any one to be rejected. will reject immediately upon any of the input promises rejecting.
allSettled([iterable]) will wait for all input promises to complete, regardless of whether or not one rejects
any([iterable])	Returns the promise value as soon as any one of the promises is fulfilled
race([iterable])	Wait until any of the promises is resolved or rejected
reject(reason)	Returns a new Promise object that is rejected for the given reason
resolve(value)	Returns a new Promise object that is resolved with the given value
catch()	Appends the rejection handler callback
then()	Appends the resolved handler callback
finally()	Appends a handler to the promise

Callback:
A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.
JavaScript callback functions can also be used to perform synchronous tasks.
SYNCHRONOUSLY executing the callback function. Callbacks can also be executed ASYNCHRONOUSLY
Promises are similar to callback functions in a sense that they both can be used to handle asynchronous tasks.

api(function(result){
    api2(function(result2){
        api3(function(result3){
             // do work
            if(error) {
                // do something
            }
            else {
                // do something
            }
        });
    });
});

callback hell, pyramid of doom

async/await:
https://www.programiz.com/javascript/async-await

We use the async keyword with a function to represent that the function is an asynchronous function. The async function returns a promise.
The await keyword is used inside the async function to wait for the asynchronous operation.

// a promise
let promise = new Promise(function (resolve, reject) {
    setTimeout(function () {
    resolve('Promise resolved')}, 4000); 
});

// async function
async function asyncFunc() {

    // wait until the promise resolves 
    let result = await promise; 

    console.log(result);
    console.log('hello');
}

// calling the async function
asyncFunc();

let promise1;
let promise2;
let promise3;

async function asyncFunc() {
    let result1 = await promise1;
    let result2 = await promise2;
    let result3 = await promise3;

    console.log(result1);
    console.log(result1);
    console.log(result1);
}

// a promise
let promise = new Promise(function (resolve, reject) {
  setTimeout(function () {
  reject('Promise rejected')}, 4000); 
});

// async function
async function asyncFunc() {
  try {
      // wait until the promise resolves 
      let result = await promise; 

      console.log(result);
  }   
  catch(error) {
      console.log(error, 'asd');
  }
}

// calling the async function
asyncFunc(); // Promise resolved

try/catch/finally:
https://www.programiz.com/javascript/try-catch-finally

The try, catch and finally blocks are used to handle exceptions (a type of an error)
In programming, there can be two types of errors in the code:
Syntax Error: Error in the syntax. For example, if you write consol.log('your result');, the above program throws a syntax error. The spelling of console is a mistake in the above code.
Runtime Error: This type of error occurs during the execution of the program. For example, calling an invalid function or a variable.

These errors that occur during runtime are called exceptions. Now, let's see how you can handle these exceptions.

const numerator= 100, denominator = 'a';

try {
     console.log(numerator/denominator);
     console.log(a);
}
catch(error) {
    console.log('An error caught'); 
    console.log('Error message: ' + error);  
    throw 'Error'
}
finally {
     console.log('Finally will execute every time');
}

===========

JavaScript functional programming techniques
https://www.syncfusion.com/blogs/post/7-functional-programming-techniques-for-javascript-developers
- pure/regular functions
function greet() {
  console.log("Hello World!");
}

- arrow functions/function expressions(store into a variable)
const greet = () => console.log("Hello World!");

- default parameters
const greet = (name = "Guest", hi = "Hello") => console.log(hi, name);

- function chaining: Chaining is the concept one function returns a current(this) object and another function will use values in that current(this) object.
function main(num) {
  let i = num;
  return {
    add: function (num) {
      i += num;
      return this;
    },
    subtract: function (num) {
      i -= num;
      return this;
    },
    divide: function (num) {
      i /= num;
      return this;
    },
    multiple: function (num) {
      i *= num;
      return this;
    },
    print() {
      return i;
    },
  };
}

const x = main(10)
const res = x.add(6).subtract(4).divide(3).multiple(2).print();
console.log(res)

- partial application: Partial application starts with a function. We take this function and create a new one with one or more of its arguments already “set” or partially applied.
Ramda:
// Assuming we're in a node environment
const R = require('ramda')
// R.partial returns a new function (!)
const buildHttpsUri = R.partial(buildUri, ['https'])
const twitterFavicon = buildHttpsUri('twitter.com', 'favicon.ico')

function sum1(x, y, z) {
  return sum2(x,y,z)
}
// to
function sum1(x) {
  return (y,z) => {
      return sum2(x,y,z)
  }
}

- composition: Function composition is the pointwise application of one function to the result of another. Developers do it in a manual manner every day when they nest functions
// Bread Slicing Function
const sliceBread = bread => `${bread} is sliced`;
// Spreading Function
const spreadButter = bread => `Butter spread on ${bread}`;
// Filling Function
const addFilling = bread => `Filling added to ${bread}`;
// Composing Functions to make a Sandwich
const makeSandwich = bread => addFilling(spreadButter(sliceBread(bread)));
console.log(makeSandwich("Whole Wheat")); 
// Outputs: "Filling added to Butter spread on Whole Wheat is sliced"

- recursion: is a programming technique where a function calls itself repeatedly to solve a problem
// recursive function
function counter(count) {
  // display count
  console.log(count);
  // condition for stopping
  if(count > 1) {
      // decrease count
      count = count - 1;
      // call counter with new value of count
      counter(count);
  } else {
      // terminate execution
      return;
  };
};
// access function
counter(5);

- nested function: a function can also contain another function
// outer function
function greet(name) {

  // inner function
  function displayName() {
      console.log('Hi' + ' ' + name);
  }

  // calling inner function
  displayName();
}

// calling outer function
greet('John'); // Hi John

- returning function: you can also return a function within a function
function greet(name) {
  function displayName() {
      console.log('Hi' + ' ' + name);
  }

  // returning a function
  return displayName;
}

const g1 = greet('John');
console.log(g1); // returns the function definition
g1(); // invoking/calling the function

- closures(cool abstractions): closure provides access to the outer scope of a function from inside the inner function, even after the outer function has closed, should learn first Nested Function and Returning a function
// outer function
function greet() {
  // variable defined outside the inner function
  let name = 'John';
  // inner function
  function displayName() {
      // accessing name variable, the outer scope of a function
      return 'Hi' + ' ' + name;
  }

  return displayName;
}

const g1 = greet(); // g1 is closure
console.log(g1); // returns the function definition
console.log(g1()); // returns the value

// another example of closure
function calculate(x) {
  function multiply(y) {
      return x * y;
  }
  return multiply;
}

const multiply3 = calculate(3); // multiply3 is closure
const multiply4 = calculate(4); // multiply4 is closure

console.log(multiply3); // returns calculate function definition
console.log(multiply3()); // NaN

console.log(multiply3(6)); // 18
console.log(multiply4(2)); // 8

- currying(cool abstractions): transforms a function with multiple arguments into a sequence/series of functions, each taking a single argument
function sum(a, b, c) {
  return a + b + c;
}
sum(1,2,3); // 6

function sum(a) {
  return (b) => {
      return (c) => {
          return a + b + c
      }
  }
}
console.log(sum(1)(2)(3)) // 6

We could even separate this sum(1)(2)(3) to understand it better:
const sum1 = sum(1);
const sum2 = sum1(2);
const result = sum2(3);
console.log(result); // 6

function sum1(x, y, z) {
  return sum2(x,y,z)
}
// to
function sum1(x) {
  return (y) = > {
      return (z) = > {
          return sum2(x,y,z)
      }
  }
}

- hoc: A higher order function is a function that takes one or more functions as arguments, or returns a function as its result
// Callback function, passed as a parameter in the higher order function
function callbackFunction(){
  console.log('I am  a callback function');
}

// higher order function
function higherOrderFunction(func){
  console.log('I am higher order function')
  func()
}

higherOrderFunction(callbackFunction);

You can use higher order functions in a variety of ways.
When working with arrays, you can use the map(), reduce(), filter(), and sort() functions to manipulate and transform data in an array.
When working with objects, you can use the Object.entries() function to create a new array from an object.
When working with functions, you can use the compose() function to create complex functions from simpler ones

- coupling: Object-Oriented (OO) design that play a crucial role in creating robust and maintainable software systems, efers to the degree of interdependence between modules or classes, Low coupling implies minimal interactions between different components, High coupling denotes strong dependencies between components

- cohesion: Object-Oriented (OO) design that play a crucial role in creating robust and maintainable software systems, refers to the degree to which the elements within a module or class are related to each other and focused on a single task or responsibility

- generator: provide a new way to work with functions and iterators
Using a generator,
- you can stop the execution of a function from anywhere inside the function
- and continue executing code from a halted position
// generator function
function* generatorFunc() {
  // returns 'hello' at first next()
  let x = yield 'hello';
  // returns passed argument on the second next()
  console.log(x);
  console.log('some code');
  // returns 5 on second next()
  yield 5; 
}
const generator = generatorFunc();

console.log(generator.next());
console.log(generator.next(6));
console.log(generator.next());
// { value: 'hello', done: false }
// 6
// some code
// { value: 5, done: false }
// { value: undefined, done: true }

- constructor: used to create and initialize objects
function Person () {
  this.name = "John",
  this.age = 23,

   this.greet = function () {
      console.log("hello");
  }
}

// create objects
const person1 = new Person();
const person2 = new Person();

// access properties
console.log(person2)
console.log(person1.name);
console.log(person2.age);
console.log(person1.greet());
// Person { name: 'John', age: 23, greet: [Function (anonymous)] }
// John
// 23
// hello
// undefined

- immediately invoked: (IIFEs) immediately invoked function expressions
You can name your IIFEs or leave them anonymous. Be aware that naming them does not mean that they will be invoked after they are executed. Naming can be useful, especially if you have several IIFEs that perform different operations close to each other.

const greeting = "Hello world";

(function myFunc() {
  // Beginning of function
  console.log(greeting);
})(); // End of function //calls the function execution e.g. myFunc()

(function () {
  console.log(greeting);
})();

(() => {
  console.log(greeting);
})();

- hoisting: function hoisting allows us to call the function before its declaration
// function call
greeting(); 
// function declaration
function greeting() {
  console.log("Welcome to Programiz.");
}

- singleton pattern: ensures that a class can only have a single instance throughout the lifetime of an application. By restricting the instantiation of a class to one object, you can ensure that shared resources or data are managed consistently across different parts of your application.

const Singleton = (function () {
  let instance;
  function createInstance() {
    return {
      message: "I am the only instance!",
      showMessage: function () {
        console.log(this.message);
      },
    };
  }
  return {
    getInstance: function () {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    },
  };
})();
const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();
console.log(singleton1 === singleton2); // true
In this example, we create an immediately-invoked function expression (IIFE) that serves as our Singleton. Inside the IIFE, we define a private variable instance and a private function createInstance(). The getInstance() method creates a new instance of the object if it doesn't already exist or returns the existing instance.

===========

let p1= new Promise((resolve,reject)=>{
  resolve('foo');
})
let p2= new Promise((resolve,reject)=>{
  reject('bar');
})

console.log('bip')

setTimeout(() => console.log('wew'), 0)

p1.then(val=>{
  console.log(val)
  return p2
})
.then(
  console.log('baz')
)
.catch(err=>{
  console.log(err)
})
console.log('bop')

// bip
// baz
// bop
// foo
// bar
// wew
```
