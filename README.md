# Patterns in Javascript
Patterns are a better way to write maintainable, readable and reusable code. Code structuring becomes more important when applications become larger. Design patterns prove crucial to solving this challenge. The most important design patterns that are widely used are - 

* Modular
* Revealing Modular
* Constructor
* Singleton
* Factory

Now we know that there are certain patterns that are used in JS, but we still not know WHEN, WHY & HOW of the patterns. Like when a certain pattern should be used, why should we use it and how the implementation will look like. Lets go over each of the patterns mentioned above - 

## 1. Modular Pattern - 
JS modules are the most prevalent used design patterns for keeping particular pieces of code independent of each other. This provides loose coupling to support well structured code.

If you are familiar with OOPS, modules are similar to JS classes. One of the advantages of the classes is encapsulation - protecting state and behaviour from being accessed from other classes. 

It provides a way of wrapping a mix of public and private methods and variables, protecting pieces from leaking into the global scope and accidentally colliding with another developer's interface

Modules should be Immediately-Invoked-Function-Expressions (IIFE) to allow for private scopes - that is, a closure that protect variables and methods (however, it will return an object instead of a function). This is what it looks like:

```
(function() {

    // declare private variables and/or functions

    return {
      // declare public variables and/or functions
    }

})();

```

Here we instantiate the private variables and/or functions before returning our object that we want to return. Code outside of our closure is unable to access these private variables since it is not in the same scope. Let's see another example - 

```
var testModule = (function () {
 
  var counter = 0;
 
  return {
 
    incrementCounter: function () {
      return counter++;
    },
 
    resetCounter: function () {
      console.log( "counter value prior to reset: " + counter );
      counter = 0;
    }
  };
 
})();
 
// Usage:
 
// Increment our counter
testModule.incrementCounter();
 
// Check the counter value and reset
// Outputs: counter value prior to reset: 1
testModule.resetCounter();

```

Looking at another example, below we can see a shopping basket implemented using this pattern. The module itself is completely self-contained in a global variable called basketModule. The basket array in the module is kept private and so other parts of our application are unable to directly read it. It only exists with the module's closure and so the only methods able to access it are those with access to its scope (i.e. addItem(), getItemCount() etc) -

```

var basketModule = (function () {
 
  // privates
 
  var basket = [];
 
  function doSomethingPrivate() {
    //...
  }
 
  function doSomethingElsePrivate() {
    //...
  }
 
  // Return an object exposed to the public
  return {
 
    // Add items to our basket
    addItem: function( values ) {
      basket.push(values);
    },
 
    // Get the count of items in the basket
    getItemCount: function () {
      return basket.length;
    },
 
    // Public alias to a private function
    doSomething: doSomethingPrivate,
 
    // Get the total value of items in the basket
    getTotal: function () {
 
      var q = this.getItemCount(),
          p = 0;
 
      while (q--) {
        p += basket[q].price;
      }
 
      return p;
    }
  };
})();

```

This is how we would use this - 

```

// basketModule returns an object with a public API we can use
 
basketModule.addItem({
  item: "bread",
  price: 0.5
});
 
basketModule.addItem({
  item: "butter",
  price: 0.3
});
 
// Outputs: 2
console.log( basketModule.getItemCount() );
 
// Outputs: 0.8
console.log( basketModule.getTotal() );
 
// However, the following will not work:
 
// Outputs: undefined
// This is because the basket itself is not exposed as a part of our
// public API
console.log( basketModule.basket );
 
// This also won't work as it only exists within the scope of our
// basketModule closure, but not in the returned public object
console.log( basket );

```


	