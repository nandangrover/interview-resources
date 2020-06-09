# Questions

1. Write poly-fill of Object.create and explain why it worked:

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

2. Implement curry function

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
