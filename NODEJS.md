# Nodejs Questions and Explanations

### 1. What is Node.js? What is it used for?

Node.js is a run-time JavaScript environment built on top of Chrome’s V8 engine. It uses an event-driven, non-blocking I/O model. It is lightweight and so efficient. Node.js has a package ecosystem called npm.
Node.js can be used to build different types of applications such as web application, real-time chat application, REST API server etc. However, it is mainly used to build network programs like web servers, similar to PHP, Java, or ASP.NET. Node.js was developed by Ryan Dahl in 2009.

### 2. What is Event-driven programming?

Event-driven programming is building our application based on and respond to events. When an event occurs, like click or keypress, we are running a callback function which is registered to the element for that event.
Event driven programming follows mainly a publish-subscribe pattern.

```js
function addToCart(productId){
  event.send("cart.add", {id: productId});
}

event.on("cart.add", function(event){
  show("Adding product " + event.id);
});
```

### 3. What is Event loop in Node.js work? And How does it work?

The Event loop handles all async callbacks. Node.js (or JavaScript) is a single-threaded, event-driven language. This means that we can attach listeners to events, and when a said event fires, the listener executes the callback we provided.

Whenever we call setTimeout, http.get and fs.readFile, Node.js runs this operations and further conitnue to run other code without waiting for the output. When the operation is finished, it receives the output and runs our callback function.

So all the callback functions are queued in an loop, and will run one-by-one when the response has been received.

### 4. What is REPL in Node.js?

REPL means Read-Eval-Print-Loop. It is a virtual environment that comes with Node.js. We can quickly test our JavaScript code in the Node.js REPL environment.

To launch the REPL in Node.js, just opne the command prompt and type `node`. It will change the prompt to > in Windows and MAC.

Now we can type and run our JavaScript easily. For example, if we type `10 + 20`, it will print `30` in the next line.


### 5. The below code runs faster in nodejs than google chrome. Why node.js is faster than google chrome both are using chrome v8 engine ?
```js
console.time("Test");
  for(var i=0; i <2500000; i +=1 ){
    // loop around
  }
console.timeEnd("Test");
```
Answer: In a web browser(Chrome), declaring the variable i outside of any function scope makes it global and therefore binds to **window object**. As a result, running this code in a web browser requires repeatedly resolving the property within the heavily populated window namespace in each iteration of the for loop.

In Node.js however, declaring any variable outside of any function’s scope binds it only to the **module scope** (not the window object) which therefore makes it much easier and faster to resolve.

We will get more or less same execution speed when we wrap the above code in function.
**References:** [1](https://stackoverflow.com/questions/29387950/performance-of-google-chrome-vs-nodejs-v8), [2](https://stackoverflow.com/questions/39904835/why-is-node-js-runtime-slower-than-google-chrome-console/39904955)

### 6. What is the difference between Asynchronous and Non-blocking?

Asynchronous literally means not synchronous. We are making HTTP requests which are asynchronous, means we are not waiting for the server response. We continue with other block and respond to the server response when we received.

The term Non-Blocking is widely used with IO. For example non-blocking read/write calls return with whatever they can do and expect caller to execute the call again. Read will wait until it has some data and put calling thread to sleep.

```js
/ Blocking
const fs = require('fs');
const data = fs.readFileSync('/file.md'); // blocks here until file is read
console.log(data);
moreWork(); // will run after console.log

// Non-blocking
const fs = require('fs');
fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
  console.log(data);
});
moreWork(); // will run before console.log
```
### 7. Difference between setImmediate() vs setTimeout()?

`setImmediate()` and `setTimeout()` are similar, but behave in different ways depending on when they are called.

- `setImmediate()` is designed to execute a script once the current poll (event loop) phase completes.
- `setTimeout()` schedules a script to be run after a minimum threshold in ms has elapsed.

The order in which the timers are executed will vary depending on the context in which they are called. If both are called from within the main module, then timing will be bound by the performance of the process.

### 8. What is process.nextTick()?

`setImmediate()` and `setTimeout()` are based on the event loop. But `process.nextTick()` technically not part of the event loop. Instead, the `nextTickQueue` will be processed after the current operation completes, regardless of the current phase of the event loop.

Thus, any time you call `process.nextTick()` in a given phase, all callbacks passed to `process.nextTick()` will be resolved before the event loop continues.

Basically, `process.nextTick()` has the highest priority and will be executed first. Ex:

Input:

```js
function fn(name){
   return f;

   function f(){
       var n = name;
       console.log("Next TICK "+n+", ");
   }
}

function myTimeout(time,msg){
   setTimeout(function(){
       console.log("TIMEOUT "+msg);
   },time);
}

process.nextTick(fn("ONE"));
myTimeout(0,"AFTER-ONE");// set timeout to execute in 0 seconds for all
process.nextTick(fn("TWO"));
myTimeout(0,"AFTER-TWO");
process.nextTick(fn("THREE"));
myTimeout(0,"AFTER-THREE");
process.nextTick(fn("FOUR"));
```
Output:

```
ext TICK ONE, 
Next TICK TWO, 
Next TICK THREE, 
Next TICK FOUR, 
TIMEOUT AFTER-ONE
TIMEOUT AFTER-TWO
TIMEOUT AFTER-THREE
```
When you use process.nextTick you basically ensure that the function that you pass as a parameter will be called immediately in the next tick ie. start of next event loop. So that's why all your function in next tick executes before your timer ie. setTimeout next tick doesn't mean next second it means next loop of the nodejs eventloop. Also next tick ensures that the function you are trying to call is executed asynchronously. And next tick has higher priority than your timers, I/O operations etc queued in the eventloop for execution. You should use nextTick when you want to ensure that your code is executed in next event loop instead of after a specified time. nextTick is more efficient than timers and when you want to ensure the function you call is executed asynchronously . 
