<!--
Creator: Brianna
Location: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Object Oriented Programming in JavaScript

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

Object oriented programming is a common pattern throughout many languages. Its patterns will enable you to write more readable, organized, and declarative programs.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Build practical and useful objects using Javascript constructors
- Design object types in JavaScript using Object Oriented Programming techniques
- Describe prototypal inheritance


### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Create and manipulate JavaScript objects
- Write and interpret JavaScript functions

## Programming Paradigms


![XKCD goto](https://imgs.xkcd.com/comics/goto.png)
*Dev culture reference: 'goto considered harmful.' Google it LATER.*


We've been working with **procedural programming**, putting blocks of code in functions (aka procedures) that we call at various points in the code.

```
data <--> global or local variables

behaviors <--> procedures  (functions)

```


With **object oriented programming**, we organize data and behaviors in objects.  

```
data <--> attributes  ("class" or "instance" variables)

behaviors <--> methods (functions)

```

## OOP features

### Classes and Instances

JavaScript didn't have classes until ES6, and it still uses prototypes behind the scenes. Instead of classes, we can think of "object types" or "kinds of objects" that are defined by the combination of (1) a constructor function (like `Array()`) and (2) the constructor function's prototype (`Array.prototype`).

Objects created with the `new` keyword are cloned "instances" of the "object type" of the constructor.

### Composition

Object types are built up or composed of other data types, including other objects.

### Delegation

Basically, "delegating" tasks to another part of the program. In JS, think of **prototype chain** lookup.

### Inheritance

Basically, letting objects be "a kind of" other objects. The process by which, for example, we can easily create both `Employee` and `Student` object types starting from the features of a base `Person` object type. Tomorrow!


### Encapsulation

Encapsulation is an *overloaded* term - it can mean a few different things.  In the context of OOP, it most often refers to keeping attributes or methods "private," instead of "public" and exposed. JS doesn't have actual private object variables but can simulate them by using a scoping setup known as a **closure**.

Private variables always need *getter* and *setter* methods if they're going to be accessible from outside their scope.

```
function Person (name, realAge, feelsOld){
  this.name = name;
  this.feelsOld = feelsOld;

  var _realAge = realAge;  // this variable will be "private", not accessible
  // can also make "private" function variables if your design calls for it

  this.getAge = function(){
    if (this.feelsOld){
     return _realAge - 10
    } else {
     return _realAge;
    }
  }
}

var grandpa = new Person("Jim", 72, true);

console.log("real age: ", grandpa._realAge);  // undefined

console.log("'age': ",grandpa.getAge());  // 62 :D
```


## Prototypes

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.nice = true;
    this.greet = function() {
      if (this.nice){
        console.log(`Hello, I'm ${this.name}!`);
      } else {
        console.log("GO AWAY!");
      }
    }
}
```



People can now be created with unique attributes, which is awesome. On the other hand, you may notice that all people could share the `greet` method. Right now, though, each person instance has a separate greet method.

```javascript
var lily = new Person("Lily");
var rose = new Person("Role");

lily.greet === rose.greet // false
```

We want their greet methods to be the same! Next, we'll see how to avoid creating an entirely new `greet` method every time we make a new person.

<hr>

By adding the method `greet` to the constructor's **prototype** we can enable all people to share a `greet` method, or any other method for that matter! Shared attributes can also be added to the prototype, but they're less common.  The prototype is simply an object that can be referenced by all the person instances.

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.nice = true;
}

Person.prototype.greet = function() {
  if (this.nice){
    console.log(`Hello, I'm ${this.name}!`);
  } else {
    console.log("GO AWAY!");
  }
}
```

Now try running the same test from above to see if both people share the same `greet` method.

```javascript
var lily = new Person("Lily");
var rose = new Person("Rose");

lily.greet === rose.greet // true
```

#### Benefits

- Less wasted memory
- Single source of truth

>What if we edit the prototype *after* the person instances have been created? Will they update their behavior accordingly?


## Constructor and Prototype Review

**Constructors**

* variables and functions are declared once for each instance
* functions have access to "private" variables declared within the constructor's scope
* when you update the constructor, previously created instances DON'T update
* data is "embedded" in each instance

**Prototypes**

* all instances share the same function and variable declarations
* when you update the prototype, previously created instances DO get the updates
* data is "referenced" from the prototype copy

**Instance variables and functions**

* adds variable or function directly to the instance
* overwrites constructor properties/methods by replacing them
* overwrites prototype properties/methods by being earlier on the lookup chain!

## Modeling with Constructors, Prototypes, Instances

| Place | How it Works | Attribute Example | Method Example |
| :-- | :--- | :--- | :--- |
| constructor | common, each instance gets a copy at creation | usually passed in (name), sometimes calculated | rare, can access "private" variables |
| prototype | all instances share a lookup copy | commonalities (numLegs on Dog), or shared data (numCreated) | common, same behavior across instances |
| instance | only one copy, for this instance | singularities (secretCode with a sibling) or overwriting (3-legged dog)| rare, singularities (interpretSecretCode) |

Remember, when possibly overwriting an existing prototype property or method (especially on built-in objects' prototypes), always use `||`:

`Array.prototype.sort = Array.prototype.sort || mySort;`

## Challenges - Modeling Cars for a Dealership

For each challenge that asks you to add a variable to the Car object type, write a comment that explains why you chose to put it on the constructor, prototype, or individual car instance.

1. List important attributes and methods for a Car object type.

1. Create a constructor for the Car object type.

1. Create a "public" (normal) `location` attribute for the Car object type.  Should this be on the constructor or the prototype?

1. Create a `drive` method for the Car object that takes in a new location and changes the car's location to that place. Should this be on the constructor or the prototype?

1. Create a `numWheels` variable that says how many wheels cars should have. Should this be on the constructor or the prototype?

1. Almost all of your cars are black. Create a `color` variable that stores the color of a car.  Should this be on the constructor or the prototype?

1. Create an instance of a car, and change its `color`.

1. Create a "private" `_priceMarkup` variable for the Car object type that stores the markup above a car's actual price (don't want customers to see this!). For example, `_priceMarkup = 0.5` would mean the final price of the car is 1.5 times its actual price. Should this be on the constructor or the prototype?

1. Create a getter method and a setter for the `_priceMarkup` variable.  Should these methods be on the constructor or the prototype?

1. Create a `getFinalPrice` method that returns the calculated cost of the car (based on the `price` and `_priceMarkup`). Should this be on the constructor or the prototype?

1. Create a `template` method that returns an HTML string that can represent a car in the DOM.  Include the car's `color`, `location`, `numwheels`, and final price! Should this be on the constructor or the prototype?

1. Create an `carCount` variable that counts how many cars the dealership has entered into inventory.  Should this be on the constructor or the prototype?  Or maybe on the `Car` function itself?!

1. Stretch: Use `carCount` to assign an `inventoryID` to each car when it's created. A car's `inventoryID` should be unique to that car. Should this be on the constructor or the prototype?



## Bonus: Persistent data

<!-- ### JavaScript Object Notation

Beyond modeling with objects, we sometimes want to store data in objects or send them across the internet. For these purposes, we'll often use JSON (JavaScript Object Notation), which is a standard text representation of JavaScript Objects.  We have a method called `JSON.stringify` to automatically convert JavaScript objects into a JSON string!

What does JSON get us right now today?

Our first opportunity for **persistent** data! -->

### LocalStorage

`window.localStorage` is a persistent object the browser already has set up for us. Open your developer tools and take a look at your localStorage. (Note: the browser lets us access `window.localStorage` by just typing `localStorage`.)

Try storing an array in localStorage:

`localStorage.setItem(testArr, [2,3,4,5]);`

Now check what `localStorage.getItem(testArr)` returns.  Like other JavaScript objects, localStorage has keys that are strings. BUT! localStorage also converts all values to strings, which can be bad for some of our data.  Let's try again using the JSON.stringify method first:

`localStorage.setItem(testArr, JSON.stringify([2,3,4,5]));`

What is `localStorage.testArr` now?

We're getting closer to keeping our original array values. We need to `JSON.parse` the stored object string to turn it back into JavaScript.

`var originalVals = JSON.parse(localStorage.getItem(testArr));`

Note we still don't actually have the exact original array. Since arrays (and all JS objects) are reference types, their identity is tied to the location they're stored in your computer's memory. We only got back the values.

`originalVals == testArr       // false`


You can use `localStorage.clear()` to get rid of all of your localStorage data.


**What about functions?**

We shouldn't store functions in localStorage, so we'll have to create new instances of objects programmatically from localStorage when we start up our site.

##### Check for Understanding

Write pseudocode to meet this goal:

"When the page loads, add the names and prices of all the cars from localStorage to the page."


### Using LocalStorage

If you're interested in using localStorage for your projects, read [MDN's guide](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API) and pay special attention to their examples.

##### Key points:
- LocalStorage is a prototype function of the `window` global object.
- It allows you to save up to 5MB of data per domain locally on the browser.
- It was meant to help replace functionality that was otherwise accomplished via cookies.
- We will be using it to practice persisting data in our application (before we get on to databases of course!).
- localStorage only accepts data as a string.
- If you want to save arrays or objects you must first use `JSON.stringify()` to convert them into string data.
- You may use `JSON.parse()` to retrieve the data in its original format when you need it back since it will be saved as a string.
- It is possible to convert functions to strings and execute strings as code, but it's more secure to use object types defined in your JavaScript than store functions in localStorage.


## Modeling relationships

```
function Person (first_name, last_name, money){
  this.first_name = first_name;
  this.last_name = last_name;

  this.money = money;
  this.stuff = [];

}

Person.prototype.buyStuff = function(newStuff, cost){
 this.money = this.money - cost;
 this.stuff.push(newStuff);
}
```

```
function CellPhone(make, model, price){
 this.make = make;
 this.model = model;
 this.price = price;
}
```

```
var gal = new Person("Annie", "Oakley", 828);

var iPhone6 = new CellPhone("iPhone", "6", 649.99);

gal.buyStuff(iPhone6);

```
