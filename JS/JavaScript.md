
#### 1. What actually is javascript
* JS runs on JS Engine/Interpreters/Runtime Environment via a browser or NodesJS
* JS lang doesn't need to be installed like java or python, it comes with the Interpreter
* JS is program logic, whereas Interpreters provide features/API (such as console, setTimeout, etc), which can be interacted via JS lang
* LiveScript was created by Brendan Eich within 10 days. It had many loopholes to work on. It was a combination of Java Syntax(to attract Java programmers) + OOP + Functional Programming
* LiveScript was renamed to JavaScript to attract Java programmers. JavaScript had to be renamed to ECMAScript due to copyright issues
* ECMA International decides standards for ECMAScript.
* TC-39 a committee of Browser Vendors who decides JS features and adopt it onto their own JS Engines (V8 - google, spidermonkey - mozilla, webkit - apple)
* Frontend frameworks optimizes UI written in their version of JS and optimizes it reducing the processing loads.

#### 2. Functions in JS
* Functions are treated as variable.
```js
// Normal function
function sayHello() {
  console.log('hello')
}

// Function treated as variable
let functionVar = function fn(){
  console.log('first class citizen)
}
functionVar()

// Anonymous function
let functionVar = function (){
  console.log('first class citizen)
}
functionVar()
```

* IIFE -> Immediately Invoked Function Expression
```js
// running this function directly on start without calling it
// like useEffect, ngOnInit
(function fn(){
  console.log('IIFE')
})()
```
* First Class Citizen
  1. Assignment -> function expression
  2. variable can be passed as parameter, function can be passed as parameter -> fp, hof, callback
  3. variable can be returned from a function, function can be returned from a function -> closure
```js
function sayHello(param){
  console.log('Hello', param)
  param()
  return "str"
}

function smaller(){
  console.log('Insider function executed')
}
sayHello(smaller)


function outer(){
  console.log('Outer function')
  return function() {
    console.log('Inner function')
  }
}
let rval = outer()
console.log(rval)
rval()
```

#### 3. Execution Context
  When the JavaScript engine scans a script file, it makes an environment wrapper called the **Execution Context** that handles the entire execution of the code.

  Inside Execution contexts: there exists **global** and **this** object. 
  ```js
  console.log(global)
  // in browser
  console.log(window)
  ```
Script without a main function is running in Global Execution Context:
  Global: In node - global object, in browser - window object
  This: In node - empty object, in browser - window object

  Apart from global and this object, there exists our code.
  During the runtime, the parser parses the source code and allocates memory for the variables and functions. The source code is generated and gets executed.
  This whole process is **Creation** phase.

#### 4. Hoisting
  ***JavaScript Hoisting*** is the behavior where the interpreter moves function and variable declarations to the top of their respective scope before executing the code.
```js
function real(){  // lets say it is stored in 8k address in heap
  console.log('real 1')
}
function real(){  // 12k
  console.log('real 2')
}
function real(){  // 16k
  console.log('real 3')
}
real()
```
In this above code,
It will store real (8k) in execution stack, then it will be replaced by 12k real and at last it will be replaced by 16k real. Only the last one will be stored and executed.

Even after real() function call is placed anywhere, only the 16k one is going to be executed.

Hoisting is for declarations not initialization

```js
console.log(varName)
let varName
console.log(varName)

varName = 'Helo'
console.log(varName)
fn()
function fn(){
  console.log('fn')
}
fn()

fnContainer() 
let fnContainer = function (){ //this is initialization - in arrow fn as well
  console.log('fnC')
}
fnContainer()
```

#### 5. Lexical Scope and Scope Chain

**Scope** refers to the _area_ where an item (such as a function or variable) is visible and accessible to other code.
**Scope** essentially means where these variables are available for use.
*  **Scope** means area, space, or region.
-  **Global scope** means global space or a public space.
-  **Local scope** means a local region or a restricted region.
**Scope chain** in JavaScript is a mechanism that determines the order in which variables are looked up when a variable is referenced.

```js
// Define a variable in the global scope:
const fullName = "Oluwatobi Sofela";

// Define nested functions:
function profile() {
  function sayName() {
    function writeName() {
      return fullName;
    }
    return writeName();
  }
  return sayName();
}
```
**writeName() scope ---> sayName() scope ---> profile() scope ---> global scope**

1. Firstly, the computer will check if `fullName` got defined locally within the `writeName()` function. But it will find no `fullName` definition there, so it moves up to the next scope.
2. Secondly, the computer will search for `fullName`'s definition in `sayName()` (the next space in the scope chain). Still, it doesn't find it there, so it climbs up the ladder to the next scope.
3. Thirdly, the computer will search for `fullName`'s definition in the `profile()` function. Yet still, `fullName` is not found there. So the computer goes forward to seek `fullName`'s lexical scope in the next region of the scope chain.
4. Fourthly, the computer goes to the _global scope_ (the following scope after `profile()`). Fortunately, it finds fullName's definition there! Therefore, it gets its content (`"Oluwatobi Sofela"`) and returns it.

```js
console.log(varName) // undefined
var varName = 10
console.log(varName) // 10
function fn(){
    console.log(varName) // undefined
    var varName = 20
    console.log(varName) // 20
}
fn() // when function calls, it has its own execution context on a stack
```

```js
console.log(varName)
var varName = 10
console.log(varName)
function fn(){
    console.log('line 5: ' + varName) // undefined
    var varName = 20
    function b() {
	  console.log('line 8:', varName) // 20 - accesses the var of fn() that will be lexical scope
    }
    b()
    console.log(varName) // 20
}
fn()
```

```js
var varName = 10
// 
function b() {
  console.log('line 3:', varName) // 10 - it will look for variable outside the function definition, not outside the function call
}
console.log(varName)
function fn(){
    console.log('line 9: ' + varName) // undefined
    var varName = 20
    b()
    console.log(varName) // 20
}
fn()
```

#### 6. var keyword & function scope
```js
console.log('line1:', varName)
// declare
var varName
// assign
varName = 10
console.log('line7:', varName)
// reassign
varName = 20
console.log('line10:', varName)

var varName
console.log('line13:', varName) //20 - as varName address in memory stores 20 already

```

```js
function fn(){
  console.log('line 2:', a) // undefined
  var a = 10
  console.log('line 4:', a) // 10
  
  if(a == 10){
    var a // a already has a value 10
    console.log('line 8:', a) // 10 
  }
  console.log('line 10:', a) // 10
}

fn()
```

```js
var a = 10
console.log('line 2:', a) // 10

function fn(){
  console.log('line 5:', a) // undefined
  var a = 20
  a++
  console.log('line 7:', a) // 21
  if(a){
    var a = 30
    a++
    console.log('line 12:', a) // 31
  }
}

fn()
console.log('line 17:', a) // 10
```


#### 6. var, let and const
Temporal dead zone (TDZ) is a block of code where a variable is inaccessible until it is initialized with a value.

* `var` declarations are globally scoped or function/locally scoped.

```js
let fruit = 'apple'
console.log('line 2:', fruit) // apple

{
  // console.log(fruit) //tdz - cannot access before initialization
  let fruit
  console.log('line 7:', fruit) // undefined
  fruit = 'orange'
  console.log('line 9:', fruit) // orange
  {
    console.log('line 11:', fruit) // orange
  }
}
console.log('line 14', fruit) // apple
```

Variable Shadowing - occurs when an inner scope declares a variable with the same name as an outer scope. It happens with let and const both.
```js
const fruit = 'apple'
console.log('line 2:', fruit)

{
  const fruit = 'orange' // variable shadowing -> legal
  console.log('line 6:', fruit)
}
console.log('line 8', fruit)
```

Illegal Variable Shadowing -  create a variable in an outer scope with the let keyword and another variable with the var keyword in a block scope but with the same name, it will throw an error.
```js
let fruit = 'apple'
console.log('line 2:', fruit)

{
  var fruit = 'orange' // variable shadowing -> illegal. var is not block scoped and is accessible to the parent
  console.log('line 6:', fruit)
}
console.log('line 8', fruit)
```

![[var_let_const.PNG]]

- They are all hoisted to the top of their scope. Unlike `var` which is initialized as `undefined`, the `let` keyword is not initialized. So if you try to use a `let`, `const` variable before declaration, you'll get a `Reference Error`.
- While `var` and `let` can be declared without being initialized, `const` must be initialized during declaration.

#### 7. Everything is object or primitive
* JS => primitive or object
* everything is object => Dates, errors, modules, objects, array etc
* 6 primitive types - string, number, boolean, undefined, null, symbol
```js

// object - key-value type
let object = {
    name: 'Person',
    age: 90,
    sayHi: function() {
        console.log('Hi')
    }
}
// loop
// for(let key in object){
//     console.log(key, object[key])
// }
// object.sayHi()

// Strange behavior of array
const arr = [6,65,4,43]
arr.demoProp = 'prop in arr'
arr.func = function () {
    console.log('func in arr')
}
arr[95] = 100
// for(let key in arr){
//   console.log(key, arr[key])
// }
console.log(arr.length)

// function -> object -> key
// extra feature -> code property that can be executed when invoke
function fn() {
  console.log('demo function')
}
fn()
fn.demoProp = 'Demo prop for function'
fn.insideFunc = function () {
  console.log('function inside function obj')
}
fn.insideFunc()
console.log(fn)
```

##### Pure functions
A pure function in JavaScript is a function that returns the same output for the same input, and doesn't have any side effects. 
- **No side effects**: Pure functions don't change any values or states outside of their scope. 
- **No external dependencies**: Pure functions don't depend on any values outside of their scope. 
- **Predictable**: Pure functions are reliable and predictable because they always return the same result for the same input.

**Mutable** data types are those whose values can be changed, whereas **Immutable** data types are ones in which the values can't be changed.

```js
const person1 = {
    name: 'person1',
    age: 90
}

const person2 = person1 // stores reference of person1

person1.name = 'steve' // both person1, person2 name will be same as it is referencing to one variable

console.log(person1)
console.log(person2)

const person3 = Object.assign({}, person2) // creates a new copy of variable instead of referencing it
person3.name = 'JJ'
console.log(person3)

// another way to create a new copy
const person4 = {...person2}
person4.name = 'Jordan'
console.log(person4)
```

#### 8. High Order Functions
A higher order function is a function that takes one or more functions as arguments, or returns a function as its result.
```js
// logic to calculate area
const area = function(radius){
  return Math.PI * radius * radius;
}

// logic to calculate radius
const diameter = function(radius){
  return 2 * radius
}

const calculate = function(radii, logic){
  const output = []
  for(let i = 0; i < radii.length; i++){
    output.push(logic(radii[i]))
  }
  return output
}

console.log(calculate([1,2,3,4,5], area))
```

* When working with arrays, you can use the `map()`, `reduce()`, `filter()`, and `sort()` functions to manipulate and transform data in an array.

* When working with objects, you can use the `Object.entries()` function to create a new array from an object.

* When working with functions, you can use the `compose()` function to create complex functions from simpler ones

![[hof_cheatsheat_1.jpg]]

Polyfills of map, filter and reduce
```js
function polyfillMap(arr, cb) {
  let newArr = []
  for(let i = 0; i < arr.length; i++){
      newArr.push(cb(arr[i]))
  }
  return newArr
}
console.log(polyfillMap([1,2,3,4,5], (x) => x*x ))

function polyfillFilter(arr, cb) {
  let newArr = []
  for(let i = 0; i < arr.length; i++){
    if(cb(arr[i])){
      newArr.push(arr[i])
    }
  }
  return newArr
}
console.log(polyfillFilter([1,2,3,4,5], (x) => {
  if(x % 2 === 0){
    return x
  }
}))

function polyfillReduce(arr, acc, cb) {
  for(let i = 0; i < arr.length; i++){
    acc = cb(acc, arr[i])
  }
  return acc
}
console.log(polyfillReduce([1,2,3,4,5,9], 0, (acc, x) => acc + x))
```
using Prototype
```js
Array.prototype.polyfillMap = function(callback){
  let newArr = []
  for(let i = 0; i < this.length; i++){
    newArr.push(callback(this[i]))
  }
  return newArr
}

Array.prototype.polyfillFilter = function(callback){
  let newArr = []
  for(let i = 0; i < this.length; i++){
    if(callback(this[i])){
      newArr.push(this[i])
    }
  }
  return newArr
}

Array.prototype.polyfillReduce = function(acc, callback){
  for(let i = 0; i < this.length; i++){
    acc = callback(acc, this[i])
  }
  return acc
}

const arr = [1,2,3,4,5,6]

console.log(arr.polyfillMap((x) => x*x))
console.log(arr.polyfillFilter((x) => x % 2 === 0))
console.log(arr.polyfillReduce(0, (acc, x) => acc + x))
```
#### 9. Closure
A closure in JavaScript is created when a function is defined within another function. It allows the inner function to access the variables and parameters of the outer function, even after the outer function has finished executing.
A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**).

```js
function add(b) {
  let a = 5
  
  function addChild(){
    console.log(a + 5 + b)
  }
  return addChild
}

let childAdd = add()
childAdd()
```

#### 10. this keyword

* Nodejs non-strict mode
```js
console.log(this) // returns empty object

function showThis(){
  console.log(this) // returns global object
}
showThis()

const obj = {
  name: 'John',
  func: function (){
    console.log(this) // return its own object which is obj
    
    function insideFunc(){
      console.log(this) // returns global object
    }
    insideFunc()
  }
}
obj.func()
```

* Nodejs strict mode
```js
'use strict'

console.log(this) // returns empty object

function showThis(){
  console.log(this) // returns undefined
}
showThis()

const obj = {
  name: 'John',
  func: function (){
    console.log(this) // return its own object which is obj
    
    function insideFunc(){
      console.log(this) // returns undefined
    }
    insideFunc()
  }
}
obj.func()
```

* Browser non-strict mode
```js
console.log(this) // returns window object

function showThis(){
  console.log(this) // returns window object
}
showThis()

const obj = {
  name: 'John',
  func: function (){
    console.log(this) // return its own object which is obj
    
    function insideFunc(){
      console.log(this) // returns window object
    }
    insideFunc()
  }
}
obj.func()
```

* Browser strict mode
```js
console.log(this) // returns window object

function showThis(){
  console.log(this) // returns undefined
}
showThis()

const obj = {
  name: 'John',
  func: function (){
    console.log(this) // return its own object which is obj
    
    function insideFunc(){
      console.log(this) // returns undefined
    }
    insideFunc()
  }
}
obj.func()
```

Call, apply and bind:

```js
let person1 = {
  name: 'Adam',
  age: 45,
  showDetails: function(){
    console.log(`${this.name} is ${this.age} years old`)
  }
}

person1.showDetails()

let person2 = {
  name: 'Steve',
  age: 32
}

// function borrowing
person1.showDetails.call(person2)
```

```js
function showDetails (){
  console.log(`${this.name} is ${this.age} years old`)
}
  
let person1 = {
  name: 'Adam',
  age: 45,
}

let person2 = {
  name: 'Steve',
  age: 32
}

showDetails.call(person1)
showDetails.call(person2)
```
with parameters:
```js
function showDetails (city, car){
  console.log(`${this.name} is ${this.age} years old\nLives in ${city} and drives ${car}\n`)
}
  
let person1 = {
  name: 'Adam',
  age: 45,
}

let person2 = {
  name: 'Steve',
  age: 32
}

showDetails.call(person1, 'Delhi', 'BMW')
showDetails.call(person2, 'Mumbai', 'Mercedes')

showDetails.apply(person1, ['Delhi', 'BMW'])
showDetails.apply(person2, ['Mumbai', 'Mercedes'])

const showDetailsBind = showDetails.bind(person1, 'Delhi', 'BMW') // returns a function which bounds the showDetails function
showDetailsBind()
```

Function Currying: It is a functional programming technique where a function with multiple arguments is transformed into a series of functions, each taking a single argument.
```js
function add(a, b){
  console.log(a + b)
}

const add2 = add.bind(this, 4) // this refers to add function
add2(4)

function add(x){
  return function(y){
    console.log(x + y)
  }
}

const addWith2 = add(2)
addWith2(4)
```