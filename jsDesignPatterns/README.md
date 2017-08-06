# Patterns in Javascript
Patterns are a better way to write maintainable, readable and reusable code. Code structuring becomes more important when applications become larger. Design patterns prove crucial to solving this challenge. The most important design patterns that are widely used are - 

* Modular
* Revealing Modular
* Constructor
* Singleton
* Factory

Now we know that there are certain patterns that are used in JS, but we still not know WHEN, WHY & HOW of the patterns. Like when a certain pattern should be used, why should we use it and how the implementation will look like. Lets go over each of the patterns mentioned above - 

## 1. Modular Pattern
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


## 2. Revealing Modular Pattern

The Revealing Module pattern came about as Heilmann was frustrated with the fact that he had to repeat the name of the main object when we wanted to call one public method from another or access public variables.  He also disliked the Module pattern’s requirement for having to switch to object literal notation for the things he wished to make public.

An example of how to use the Revealing Module pattern can be found below:

```

var myRevealingModule = (function () {
 
        var privateVar = "some random channel, please change the name! :(",
            publicVar = "Hey there!";
 
        function privateFunction() {
            console.log( "Youtube channel:" + privateVar );
        }
 
        function publicSetName( strName ) {
            privateVar = strName;
        }
 
        function publicGetName() {
            privateFunction();
        }
 
 
        // Reveal public pointers to
        // private functions and properties
 
        return {
            setName: publicSetName,
            greeting: publicVar,
            getName: publicGetName
        };
 
    })();
 
myRevealingModule.setName( "jsTrends" );

```

## 3. Constructor Pattern	

In classical object-oriented programming languages, a constructor is a special method used to initialize a newly created object once memory has been allocated for it. In JavaScript, as almost everything is an object, we're most often interested in object constructors.

Object constructors are used to create specific types of objects - both preparing the object for use and accepting arguments which a constructor can use to set the values of member properties and methods when the object is first created.

```

function Car( model, year, miles ) {
 
  this.model = model;
  this.year = year;
  this.miles = miles;
 
  this.toString = function () {
    return this.model + " has done " + this.miles + " miles";
  };
}
 
// Usage:
 
// We can create new instances of the car
var civic = new Car( "Honda Civic", 2009, 20000 );
var mondeo = new Car( "Ford Mondeo", 2010, 5000 );
 
// and then open our browser console to view the
// output of the toString() method being called on
// these objects
console.log( civic.toString() );
console.log( mondeo.toString() );

```

Constructor with Prototype - 


In above example just replace the toString method with the following method - 

```

Car.prototype.toString = function () {
  return this.model + " has done " + this.miles + " miles";
};

```


## 4. Singleton Pattern

The Singleton pattern is thus known because it restricts instantiation of a class to a single object. Classically, the Singleton pattern can be implemented by creating a class with a method that creates a new instance of the class if one doesn't exist. In the event of an instance already existing, it simply returns a reference to that object.

An example of Singleton usage is use of an office printer. If there are ten people in an office, and they all use one printer, ten computers share one printer (instance). By sharing one printer, they share the same resources.

```

var printer = (function () {

  var printerInstance;

  function create () {

    function print() {
      // underlying printer mechanics
    }

    function turnOn() {
      // warm up
      // check for paper
    }

    return {
      // public + private states and behaviors
      print: print,
      turnOn: turnOn
    };
  }

  return {
    getInstance: function() {
      if(!printerInstance) {
        printerInstance = create();
      }
      return printerInstance;
    }
  };

})();

```

Usage

```

var officePrinter = printer.getInstance();

```

In AngularJS, Singletons are prevalent, the most notable being services, factories, and providers. Since they maintain state and provides resource accessing, creating two instances defeats the point of a shared service/factory/provider.


## 5. Factory Pattern

A Factory can provide a generic interface for creating objects, where we can specify the type of factory object we wish to be created.

Imagine that we have a UI factory where we are asked to create a type of UI component. Rather than creating this component directly using the new operator or via another creational constructor, we ask a Factory object for a new component instead. We inform the Factory what type of object is required (e.g "Button", "Panel") and it instantiates this, returning it to us for use.

Examples of this pattern can be found in UI libraries such as ExtJS where the methods for creating objects or components may be further subclassed.

The following is an example that builds upon our previous snippets using the Constructor pattern logic to define cars. It demonstrates how a Vehicle Factory may be implemented using the Factory pattern:

```

// Types.js - Constructors used behind the scenes
 
// A constructor for defining new cars
function Car( options ) {
 
  // some defaults
  this.doors = options.doors || 4;
  this.state = options.state || "brand new";
  this.color = options.color || "silver";
 
}
 
// A constructor for defining new trucks
function Truck( options){
 
  this.state = options.state || "used";
  this.wheelSize = options.wheelSize || "large";
  this.color = options.color || "blue";
}
 
 
// FactoryExample.js
 
// Define a skeleton vehicle factory
function VehicleFactory() {}
 
// Define the prototypes and utilities for this factory
 
// Our default vehicleClass is Car
VehicleFactory.prototype.vehicleClass = Car;
 
// Our Factory method for creating new Vehicle instances
VehicleFactory.prototype.createVehicle = function ( options ) {
 
  switch(options.vehicleType){
    case "car":
      this.vehicleClass = Car;
      break;
    case "truck":
      this.vehicleClass = Truck;
      break;
    //defaults to VehicleFactory.prototype.vehicleClass (Car)
  }
 
  return new this.vehicleClass( options );
 
};
 
// Create an instance of our factory that makes cars
var carFactory = new VehicleFactory();
var car = carFactory.createVehicle( {
            vehicleType: "car",
            color: "yellow",
            doors: 6 } );
 
// Test to confirm our car was created using the vehicleClass/prototype Car
 
// Outputs: true
console.log( car instanceof Car );
 
// Outputs: Car object of color "yellow", doors: 6 in a "brand new" state
console.log( car );

```

Modify the VehicleFactory instance to use the Truck class 

```

var movingTruck = carFactory.createVehicle( {
                      vehicleType: "truck",
                      state: "like new",
                      color: "red",
                      wheelSize: "small" } );
 
// Test to confirm our truck was created with the vehicleClass/prototype Truck
 
// Outputs: true
console.log( movingTruck instanceof Truck );
 
// Outputs: Truck object of color "red", a "like new" state
// and a "small" wheelSize
console.log( movingTruck );

```

### When To Use The Factory Pattern

The Factory pattern can be especially useful when applied to the following situations:

* When our object or component setup involves a high level of complexity.
* When we need to easily generate different instances of objects depending on the environment we are in.


## Conclusions

It's important for us to be aware of these patterns but it's also essential to know how and when to use them. Study the pros and cons of each pattern before employing them. Take the time out to experiment with patterns to fully appreciate what they offer and make usage judgements based on a pattern's true value to your application.


### References

1. [Learning JavaScript Design Patterns - Addy Osmani](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)
2. https://toddmotto.com/mastering-the-module-pattern/
3. https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know#toc-singleton
 

