# Javascript Questions and Explanations

### Table of Contents

| No. | Questions |
| --- | --------- |
|1  | [Write poly-fill of Object.create and explain why it worked:](#1) |
|2  | [Implement curry function](#2) |
|3  | [Memory leaks in closures?](#3) |
|4  | [Where closure variables are stored?](#4) |
|5  | [What is functional programming?](#5) |
|6  | [Why is JS called single threaded?](#6) |
|7  | [Polyfill for bind: prerequisite to understand below code(call, apply, bind, closure, prototype, rest & spread operator)](#7) |

### <a name="1">1.</a> Write poly-fill of Object.create and explain why it worked:

```js
if( typeof Object.create !== "function") {
    Object.create = function(param) {
        var Fun = function(){};
        Fun.prototype = param;
        return new Fun();
    }
}
```
What `new` does when we create instance, what if we do not add it etc.? Explain the following for adding new.

Here is the main idea behind using new keyword in JavaScript - creating new objects with the specified prototype. And you can create objects with new only through a constructor function. 

`Object.create` lets you initialize object properties using its second argument,e.g.:

```js
var userB = {
  sayHello: function() {
    console.log('Hello '+ this.name);
  }
};

var bob = Object.create(userB, {
  'id' : {
    value: MY_GLOBAL.nextId(),
    enumerable:true // writable:false, configurable(deletable):false by default
  },
  'name': {
    value: 'Bob',
    enumerable: true
  }
});
```
 **[⬆ Back to Top](#table-of-contents)**

### <a name="2">2.</a> Implement curry function

```js
var temp = curry(avg, 1, 2, 3);
temp(10); //4 - stores 1, 2, 3 in closures and adds 10 for average
temp(1, 2); //1.8 - stores 1, 2, 3 in closures and add 1, 2 for average

//Average function:
var avg = function (...param) {
    var sum = 0;
    for(var i = 0; i < param.length; i++) {
        sum += param[i];
    }
    return param.length > 0 ? sum / param.length : 0;
}

var curry = function (fn, ...m) {
    return function (...n) {
        return fn.apply(this, m.concat(n));
    }
}
```
Another Example
```js
function curry(f) { // curry(f) does the currying transform
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

// usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

alert( curriedSum(1)(2) ); // 3
```
A curried function is a function that takes multiple arguments one at a time. Given a function with 3 parameters, the curried version will take one argument and return a function that takes the next argument, which returns a function that takes the third argument.

 **[⬆ Back to Top](#table-of-contents)**

### <a name="3">3.</a> Memory leaks in closures?

A memory leak occurs in a closure if a variable is declared in outer function becomes automatically available to the nested inner function and continues to reside in memory even if it is not being used/referenced in the nested function.

```js
 var newElem;
 
 function outer() {
     var someText = new Array(1000000);
     var elem = newElem;
 
      function inner() {
         if (elem) return someText;
      }
 
      return function () {};
  }
 
  setInterval(function () {
      newElem = outer();
  }, 5);
```
In the above example, function inner is never called but keeps a reference to elem. But as all inner functions in a closure share the same context, inner(line 7) shares the same context as function(){} (line 12)which is returned by outer function. Now in every 5ms we make a function call to outer and assign its new value(after each call) to newElem which is a global variable. As long a reference is pointing to this function(){}, the shared scope/context is preserved and someText is kept because it is part of the inner function even if inner function is never called. Each time we call outer we save the previous function(){} in elem of the new function. Therefore again the previous shared scope/context has to be kept. So in the nth call of outer function, someText of the (n-1)th call of outer cannot be garbage collected. This process continues until your system runs out of memory eventually.

`SOLUTION`: The problem in this case occurs because the reference to function(){} is kept alive. There will be no javascript memory leak if the outer function is actually called(Call the outer function in line 15 like newElem = outer()();). A small isolated javascript memory leak resulting from closures might not need any attention. However a periodic leak repeating and growing with each iteration can seriously damage the performance of your code.

 **[⬆ Back to Top](#table-of-contents)**

### <a name="4">4.</a> Where closure variables are stored?

A closure is just an evolution of the concept of the stack.

The stack is used to separate/isolate scope when functions are called. When a function returns the stack frame (activation record) is popped off the call stack thus freeing the used memory allowing the next function call to reuse that RAM for its stack frame.

What a closure does is that instead of actually freeing that stack frame, if there's any object/variable in that stack frame that's referenced by anything else then it keeps that stack frame for future use.

Most languages implement this by implementing the stack as a linked list or hash table instead of a flat array. That way, the stack can be re-ordered at runtime and is not constrained by physical memory layout.

So. With this in mind, the answer is that variables in a closure are stored in the stack and heap. Depending on your point of view.
From the point of view of the language, it's definitely the stack. Since that's what closures are in theory - a modified stack.

### <a name="5">5.</a> What is functional programming?

Functional programming (often abbreviated FP) is the process of building software by composing `pure functions`, `avoiding shared state`, `mutable data`, and `side-effects`. Functional programming is declarative rather than imperative, and application state flows through `pure functions`. Contrast with object oriented programming, where application state is usually shared and colocated with methods in objects.

Functional programming is a programming paradigm, meaning that it is a way of thinking about software construction based on some fundamental, defining principles (listed above). Other examples of programming paradigms include object oriented programming and procedural programming.

Examples of functional programming includes: 
.map()
.filter()
.reduce()

To know more check out this [blog](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0)

 **[⬆ Back to Top](#table-of-contents)**

### <a name="6">6.</a> Why is JS called single threaded?

Javascript is a single threaded language. This means it has one call stack and one memory heap. As expected, it executes code in order and must finish executing a piece code before moving onto the next. It's synchronous, but at times that can be harmful. For example, if a function takes awhile to execute or has to wait on something, it freezes everything up in the meanwhile.

```js
console.log("first")
setTimeout(() => {
    console.log("second")
}, 1000)
console.log("third")
```
Output:
```
first
third
second // After one second
```

 **[⬆ Back to Top](#table-of-contents)**

### <a name="7">7.</a> Polyfill for bind: prerequisite to understand below code(call, apply, bind, closure, prototype, rest & spread operator)
```js
let name = {
  firstname: "Amit",
  lastname: "Mondal"
}

let name2 = {
  firstname: "Nandan",
  lastname: "Gorver"
}

let printName = function (hometown, state, country) {
  console.log(`${this.firstname} ${this.lastname}, ${hometown}, ${state}, ${country}`);
}

// Bind polyfill (let's call it mybind)
Function.prototype.mybind = function(...args){
  // Store `printName` in obj and later pass the context and parameters
  let obj = this,
    params = args.slice(1);
  
  // Returning a function as bind returns a function and doesn't call it
  return function (...args2) {
    obj.apply(args[0], [...params, ...args2]);
  }
}

// Testing above code:
let printMyName = printName.mybind(name, "Maharashtra", "Mumbai");
printMyName( "India");

let printMyName2 = printName.mybind(name2, "Maharashtra", "Navi Mumbai");
printMyName2( "India");
```
 **[⬆ Back to Top](#table-of-contents)**
