## Challenges

1. Create an `Vehicle` constructor.

  ```js
  function Vehicle(name, age, color) {
    this.name = name;
    this.age = age;
    this.color = color;
  }
  ```

3. Create a `Bicycle` constructor with the same properties as `Vehicle` (bonus if you use the `call` method). Bicycles should also have a property called `isTandem`.

  ```js
  function Bicycle(name, age, color, isTandem) {
    Vehicle.call(this, name, age, color);
    this.isTandem = isTandem;
  };
  ```

4. Implement prototypal inheritance with `Bicycle` as a subclass of `Vehicle`.

  ```js
  Bicycle.prototype = new Vehicle;
  Bicycle.prototype.constructor = Bicycle;
  ```

5. Create a new instance of `Bicycle`!

  ```js
  myBike = new Bicycle("Built for Two", 3, "blue", true);
  //=> Bicycle {name: "Built for Two", age: 3, color: "blue", isTandem: true}
  ```

6. Check if the tree you just created is an `instanceof` `Bicycle`. How about an `instanceof` `Vehicle`?

  ```js
  myBike instanceof Bicycle
  //=> true

  myBike instanceof Vehicle
  //=> true
  ```
