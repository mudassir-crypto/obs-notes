
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