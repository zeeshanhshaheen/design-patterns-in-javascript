<h1 align="center"> :computer: Design Patterns in JavaScript :computer: </h1>
<p align="center">
 20 Design Patterns explaination in JavaScript
</p>
<p align="center">
We will discuss implementation of Design Patternns in JavaScript by using ES6 classes.
</p>

## :rocket: What are Design Patterns?
Design Patterns are the solutions to commonly occuring problems in software design. These patterns are easily re-usable and are expressive.

According to Wikipedia

> In software engineering, a software design pattern is a general reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. It is a description or template for how to solve a problem that can be used in many different situations.

## Types of Design Patterns
* [Creational](#creational-design-patterns)
* [Structural](#structural-design-patterns)
* [Behavioral](#behavioral-design-patterns)

## Creational Design Patterns
Creational Design Patterns will create objects for you instead of instantiating an object directly.

According to Wikipedia
> In software engineering, creational design patterns are design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. The basic form of object creation could result in design problems or added complexity to the design. Creational design patterns solve this problem by somehow controlling this object creation.

 * [Factory Method](#-factory-method)
 * [Abstract Factory](#-abstract-factory)
 * [Builder](#-builder)
 * [Prototype](#-prototype)
 * [Singleton](#-singleton)
 
 ## Factory Method
 It defines an interface for creating a single object and let childclasses to decide which class to instantiate.
 
 According to Wikipedia
> In class-based programming, the factory method pattern is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created. This is done by creating objects by calling a factory method—either specified in an interface and implemented by child classes, or implemented in a base class and optionally overridden by derived classes—rather than by calling a constructor.
 
#### Example
Let's take an example of a point. We have a class of point and we have to create Cartesian point and Polar point. We will define a Point factory that will do this work

```JavaScript
CoordinateSystem = {
  CARTESIAN: 0,
  POLAR: 1,
};

class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  static get factory() {
    return new PointFactory();
  }
}
```
Now we will create Point Factory

```JavaScript
class PointFactory {
  
  static newCartesianPoint(x, y) {
    return new Point(x, y);
  }

  static newPolarPoint(rho, theta) {
    return new Point(rho * Math.cos(theta), rho * Math.sin(theta));
  }
}
```
We will use our factory now,
```JavaScript
let point = PointFactory.newPolarPoint(5, Math.PI/2);
let point2 = PointFactory.newCartesianPoint(5, 6)
console.log(point);
console.log(point2);

```

## Abstract Factory
It creates families or groups of common objects without specifying their concrete classes.

According to Wikipedia
> The abstract factory pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes

#### Example
We will be using the example of Drink and Drink making machine.

```JavaScript
class Drink
{
  consume() {}
}

class Tea extends Drink
{
  consume() {
    console.log('This is Tea');
  }
}

class Coffee extends Drink
{
  consume()
  {
    console.log(`This is Coffee`);
  }
}
```
Making Drink Factory

```JavaScript
class DrinkFactory
{
  prepare(amount)
}

class TeaFactory extends DrinkFactory
{
  makeTea() 
  {
   console.log(`Tea Created`);
   return new Tea();
  }
}

class CoffeeFactory extends DrinkFactory
{
   makeCoffee() 
  {
   console.log(`Coffee Created`);
   return new Coffee();
  }
}

```
We will use our factory now

```JavaScript
let teaDrinkFactory = new TeaFactory();
let tea = teaDrinkFactory.makeTea()
tea.consume() 

```

## Builder
It construct complex objects from simple objects.

According to Wikipedia 
> The builder pattern is a design pattern designed to provide a flexible solution to various object creation problems in object-oriented programming. 

#### Example
We will be using ab example of a person class which stores a Person's information.

```JavaScript
class Person {
  constructor() {
    this.streetAddress = this.postcode = this.city = "";

    this.companyName = this.position = "";
    this.annualIncome = 0;
  }
  toString() {
    return (
      `Person lives at ${this.streetAddress}, ${this.city}, ${this.postcode}\n` +
      `and works at ${this.companyName} as a ${this.position} earning ${this.annualIncome}`
    );
  }
}
```
Now we will create Person Builder

```JavaScript

class PersonBuilder {
  constructor(person = new Person()) {
    this.person = person;
  }

  get lives() {
    return new PersonAddressBuilder(this.person);
  }

  get works() {
    return new PersonJobBuilder(this.person);
  }

  build() {
    return this.person;
  }
}
```

Now creating PersonJobBuilder that will takes Person's Job's information

```JavaScript

class PersonJobBuilder extends PersonBuilder {
  constructor(person) {
    super(person);
  }
  at(companyName) {
    this.person.companyName = companyName;
    return this;
  }

  asA(position) {
    this.person.position = position;
    return this;
  }

  earning(annualIncome) {
    this.person.annualIncome = annualIncome;
    return this;
  }
}

```

PersonAddressBuilder will keep Person's Address' Information

```JavaScript
class PersonAddressBuilder extends PersonBuilder {
  constructor(person) {
    super(person);
  }

  at(streetAddress) {
    this.person.streetAddress = streetAddress;
    return this;
  }

  withPostcode(postcode) {
    this.person.postcode = postcode;
    return this;
  }

  in(city) {
    this.person.city = city;
    return this;
  }
}
```
Now we will use our builder,

```JavaScript
let personBuilder = new PersonBuilder();
let person = personBuilder.lives
  .at("ABC Road")
  .in("Multan")
  .withPostcode("66000")
  .works.at("Octalogix")
  .asA("Engineer")
  .earning(10000)
  .build();
console.log(person.toString());
```

## Prototype
It creates new objects from the existing objects.

According to Wikipedia 
> The prototype pattern is a creational design pattern in software development. It is used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects. 

#### Example
We will be using example of car

```JavaScript

class Car {

  constructor(name, model) {
    this.name = name;
    this.model = model;
  }
  
  SetName(name) {
   console.log(`${name}`)
  }

  clone() {
    return new Car(this.name, this.model);
  }
}

```

Thst's how we will use this,

```JavaScript
let car = new Car();
car.SetName('Audi);

let car2 = car.clone()
car2.SetName('BMW')
```

## Singleton
It ensure that there's only for object created for a particular class.

According to Wikipedia 
> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one "single" instance. This is useful when exactly one object is needed to coordinate actions across the system.

#### Example
Creating a Singleton class

```JavaScript
class Singleton {
  constructor()
  {
    const instance = this.constructor.instance;
    if (instance) {
      return instance;
    }

    this.constructor.instance = this;
  }

  say() {
    console.log('Saying...')
  }
}

```
Thst's how we will use this,

```JavaScript
let s1 = new Singleton();
let s2 = new Singleton();
console.log('Are they same? ' + (s1 === s2));
s1.say();
```




























