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

