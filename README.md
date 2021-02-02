<h1 align="center"> :computer: Design Patterns in JavaScript :computer: </h1>
<p align="center">
 20 Design Patterns explaination in JavaScript
</p>
<p align="center">
We will discuss implementation of Design Patterns by using JavaScript ES6 classes.
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

 * [Factory Method](#factory-method)
 * [Abstract Factory](#abstract-factory)
 * [Builder](#builder)
 * [Prototype](#prototype)
 * [Singleton](#singleton)
 
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

## Structural Design Patterns
These patterns concern class and object composition. They use inheritance to compose interfaces.

According to Wikipedia 
> In software engineering, structural design patterns are design patterns that ease the design by identifying a simple way to realize relationships among entities.

 * [Adapter](#adapter)
 * [Bridge](#bridge)
 * [Composite](#composite)
 * [Decorator](#decorator)
 * [Facade](#facade)
 * [Flyweight](#flyweight)
 * [Proxy](#proxy)


## Adapter
This pattern allows classes with incompatible interfaces to work together by wrapping its own interface around existing class

According to Wikipedia 
> In software engineering, the adapter pattern is a software design pattern that allows the interface of an existing class to be used as another interface. It is often used to make existing classes work with others without modifying their source code.

#### Example
We are using an example of calculator. Calculator1 is an old interface and Calculator2 is new interfcae. We will bw building an adapter that will wraps up new interface and will give us results using it's new methods,

```JavaScript

class Calculator1 {
  constructor() {
    this.operations = function(value1, value2, operation) {
      switch (operation) {
        case 'add':
          return value1 + value2;
        case 'sub':
          return value1 - value2;
       
      }
    };
  }
}


class Calculator2 {
  constructor() {
    this.add = function(value1, value2) {
      return value1 + value2;
    };
    this.sub = function(value1, value2) {
      return value1 - value2;
    };
  }
}

```
Creating Adapter class,

```JavaScript
class CalcAdapter {
  constructor() {
    const cal2 = new Calculator2();

    this.operations = function(value1, value2, operation) {
      switch (operation) {
        case 'add':
          return cal2.add(value1, value2);
        case 'sub':
          return cal2.sub(value1, value2);
      }
    };
  }
}
```
Thst's how we will use this,

```JavaScript

const adaptedCalc = new CalcAdapter();
console.log(adaptedCalc.operations(10, 55, 'sub'));

```

## Bridge

It separate the abstraction from the implementation so that the two can vary independently.

According to Wikipedia 
> Bridge is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.

#### Example
We will be creating Renderer classes for rendering multiple shapes,

```JavaScript
class VectorRenderer {
  renderCircle(radius) {
    console.log(`Drawing a circle of radius ${radius}`);
  }
}

class RasterRenderer {
  renderCircle(radius) {
    console.log(`Drawing pixels for circle of radius ${radius}`);
  }
}

class Shape {
  constructor(renderer) {
    this.renderer = renderer;
  }
}

class Circle extends Shape {
  constructor(renderer, radius) {
    super(renderer);
    this.radius = radius;
  }

  draw() {
    this.renderer.renderCircle(this.radius);
  }

  resize(factor) {
    this.radius *= factor;
  }
}
```

That's how we use this,

```JavaScript
let raster = new RasterRenderer();
let vector = new VectorRenderer();
let circle = new Circle(vector, 5);
circle.draw();
circle.resize(2);
circle.draw();
```

## Composite
composes  objects so that they can be manipulated as single object.

According to Wikipedia 
> The composite pattern describes a group of objects that are treated the same way as a single instance of the same type of object.

#### Example
We will be using job example,

```JavaScript
class Employer{
  constructor(name, role){
    this.name=name;
    this.role=role;
    
  }
  print(){
    console.log("name:" +this.name + " relaxTime: " );
  }
}
```
Creating GroupEmployer,

```JavaScript
class EmployerGroup{
  constructor(name, composite=[]){
    console.log(name)
    this.name=name;
    this.composites=composite;
  }
  print(){
    console.log(this.name);
    this.composites.forEach(emp=>{
     emp.print();
    })
  }
}
```
Thst's how we will use this,

```JavaScript
let zee= new Employer("zee","developer")
let shan= new Employer("shan","developer")

let groupDevelopers = new EmployerGroup( "Developers", [zee,shan] );
```

## Decorator

It dynamically adds or overrides the behaviour of an object.

According to Wikipedia 
> The decorator pattern is a design pattern that allows behavior to be added to an individual object, dynamically, without affecting the behavior of other objects from the same class.

#### Exampe
We will be taking the example of color and shapes. If we have to draw a circle we will create methods and will draw circle. If we have to draw red circle. Now the beaviour is added to an object and Decorator pattern will helps me in that.

```JavaScript
class Shape {
  constructor(color) {
    this.color = color;
  }
}

class Circle extends Shape {
  constructor(radius = 0) {
    super();
    this.radius = radius;
  }

  resize(factor) {
    this.radius *= factor;
  }

  toString() {
    return `A circle ${this.radius}`;
  }
}
```
Creating ColoredShape class,

```JavaScript
class ColoredShape extends Shape {
  constructor(shape, color) {
    super();
    this.shape = shape;
    this.color = color;
  }
  toString() {
    return `${this.shape.toString()}` + `has the color ${this.color}`;
  }
}
```

That's how we will use this,

```JavaScript
let circle = new Circle(2);
console.log(circle);

let redCircle = new ColoredShape(circle, "red");
console.log(redCircle.toString());
```

## Facade
It provides a simplified interface to complex code.

According to Wikipedia 
> The facade pattern (also spelled façade) is a software-design pattern commonly used in object-oriented programming. Analogous to a facade in architecture, a facade is an object that serves as a front-facing interface masking more complex underlying or structural code.

#### Example

Let's take an example of a client intracts with computer.

```JavaScript
class CPU {
  freeze() {console.log("Freezed....")}
  jump(position) { console.log("Go....")}
  execute() { console.log("Run....") }
}

class Memory {
  load(position, data) { console.log("Load....") }
}

class HardDrive {
  read(lba, size) { console.log("Read....") }
}
```
Creating Facade

```JavaScript
class ComputerFacade {
  constructor() {
    this.processor = new CPU();
    this.ram = new Memory();
    this.hd = new HardDrive();
  }

  start() {
    this.processor.freeze();
    this.ram.load(this.BOOT_ADDRESS, this.hd.read(this.BOOT_SECTOR, this.SECTOR_SIZE));
    this.processor.jump(this.BOOT_ADDRESS);
    this.processor.execute();
  }
}
```
That's how we will use this,

```JavaScript
let computer = new ComputerFacade();
computer.start();
```

## Flyweight

It reduces the memory cost of creating similar objects.

According to Wikipedia 
> A flyweight is an object that minimizes memory usage by sharing as much data as possible with other similar objects.

#### Example
Let's take an example of user. Let's we have multiple users with the same name. We can save our memory by storing a name and give it's reference to ther users having same names.

```JavaScript
class User
{
  constructor(fullName)
  {
    this.fullName = fullName;
  }
}

class User2
{
  constructor(fullName)
  {
    let getOrAdd = function(s)
    {
      let idx = User2.strings.indexOf(s);
      if (idx !== -1) return idx;
      else
      {
        User2.strings.push(s);
        return User2.strings.length - 1;
      }
    };

    this.names = fullName.split(' ').map(getOrAdd);
  }
}
User2.strings = [];

function getRandomInt(max) {
  return Math.floor(Math.random() * Math.floor(max));
}

let randomString = function()
{
  let result = [];
  for (let x = 0; x < 10; ++x)
    result.push(String.fromCharCode(65 + getRandomInt(26)));
  return result.join('');
};
```

That's how we will use this. <br> Now we will make memory compersion without Flyweight and with Flyweight, by making 10k users.

```JavaScript

let users = [];
let users2 = [];
let firstNames = [];
let lastNames = [];

for (let i = 0; i < 100; ++i)
{
  firstNames.push(randomString());
  lastNames.push(randomString());
}

// making 10k users
for (let first of firstNames)
  for (let last of lastNames) {
    users.push(new User(`${first} ${last}`));
    users2.push(new User2(`${first} ${last}`));
  }

console.log(`10k users take up approx ` +
  `${JSON.stringify(users).length} chars`);

let users2length =
  [users2, User2.strings].map(x => JSON.stringify(x).length)
    .reduce((x,y) => x+y);
console.log(`10k flyweight users take up approx ` +
  `${users2length} chars`);
```

## Proxy
By using Proxy, a class can represent functionality of another class.

According to Wikipedia 
> The proxy pattern is a software design pattern. A proxy, in its most general form, is a class functioning as an interface to something else. 

#### Example

Let's take an example of value proxy

```JavaScript
class Percentage {
  constructor(percent) {
    this.percent = percent;
  }

  toString() {
    return `${this.percent}&`;
  }

  valueOf() {
    return this.percent / 100;
  }
}

```
That's how we can use that,

```JavaScript
let fivePercent = new Percentage(5);
console.log(fivePercent.toString());
console.log(`5% of 50 is ${50 * fivePercent}`);
```

## Behavioral Design Patterns

 Behavioral Design Patterns are specifically concerned with communication between objects.
 
 According to Wikipedia
 > In software engineering, behavioral design patterns are design patterns that identify common communication patterns among objects. By doing so, these patterns increase flexibility in carrying out communication.
 
* [Chain of Responsibility](#chain-of-responsibility)
* [Command](#command)
* [Iterator](#iterator)
* [Mediator](#mediator)
* [Memento](#memento)
* [Observer](#observer)
* [Visitor](#visitor)
* [Strategy](#strategy)
* [State](#state)
* [Template Method](#template-method)

## Chain of Responsibility
It creates chain of objects. Starting from a point, it stops until it finds a certain condition.

According to Wikipedia  
> In object-oriented design, the chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of processing objects.

#### Example
We will be using an example of a game having a creature. The creature will increase it's defense and attack when it reaches to a certain point. It will create a chain and attack and defense will increase and decrease.

```JavaScript
class Creature {
  constructor(name, attack, defense) {
    this.name = name;
    this.attack = attack;
    this.defense = defense;
  }

  toString() {
    return `${this.name} (${this.attack}/${this.defense})`;
  }
}

class CreatureModifier {
  constructor(creature) {
    this.creature = creature;
    this.next = null;
  }

  add(modifier) {
    if (this.next) this.next.add(modifier);
    else this.next = modifier;
  }

  handle() {
    if (this.next) this.next.handle();
  }
}

class NoBonusesModifier extends CreatureModifier {
  constructor(creature) {
    super(creature);
  }

  handle() {
    console.log("No bonuses for you!");
  }
}
```

Increase attack,

```JavaScript
class DoubleAttackModifier extends CreatureModifier {
  constructor(creature) {
    super(creature);
  }

  handle() {
    console.log(`Doubling ${this.creature.name}'s attack`);
    this.creature.attack *= 2;
    super.handle();
  }
}

```
Increase defense

```JavaScript
class IncreaseDefenseModifier extends CreatureModifier {
  constructor(creature) {
    super(creature);
  }

  handle() {
    if (this.creature.attack <= 2) {
      console.log(`Increasing ${this.creature.name}'s defense`);
      this.creature.defense++;
    }
    super.handle();
  }
}
```

That's how we will use this,

```JavaScript
let peekachu = new Creature("Peekachu", 1, 1);
console.log(peekachu.toString());

let root = new CreatureModifier(peekachu);

root.add(new DoubleAttackModifier(peekachu));
root.add(new IncreaseDefenseModifier(peekachu));

root.handle();
console.log(peekachu.toString());
```

## Command
It creates objects which encapsulate actions in object.

According to Wikipedia 
> In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

#### Example
We will be taking a simple example of bank account in which we give a command if we have to deposit or withdraw certain amonto of money.

```JavaScript
class BankAccount {
  constructor(balance = 0) {
    this.balance = balance;
  }
  deposit(amount) {
    this.balance += amount;
    console.log(`Deposited ${amount} Total balance ${this.balance}`);
  }

  withdraw(amount) {
    if (this.balance - amount >= BankAccount.overdraftLimit) {
      this.balance -= amount;
      console.log("Widhdrawn");
    }
  }

  toString() {
    return `Balance ${this.balance}`;
  }
}

BankAccount.overdraftLimit = -500;

let Action = Object.freeze({
  deposit: 1,
  withdraw: 2,
});
```

Creating our commands,

```JavaScript
class BankAccountCommand {
  constructor(account, action, amount) {
    this.account = account;
    this.action = action;
    this.amount = amount;
  }

  call() {
    switch (this.action) {
      case Action.deposit:
        this.account.deposit(this.amount);
        break;
      case Action.withdraw:
        this.account.withdraw(this.amount);
        break;
    }
  }

  undo() {
    switch (this.action) {
      case Action.deposit:
        this.account.withdraw(this.amount);
        break;
      case Action.withdraw:
        this.account.deposit(this.amount);
        break;
    }
  }
}
```
That's how we will use this,

```JavaScript
let bankAccount = new BankAccount(100);
let cmd = new BankAccountCommand(bankAccount, Action.deposit, 50);
cmd.call();
console.log(bankAccount.toString());
cmd.undo();
console.log(bankAccount.toString());
```

## Iterator

Iterator accesses the elements of an object without exposing its underlying representation.

According to Wikipedia 
> In object-oriented programming, the iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements.

#### Example
We will be taking an example of an array in which we print the values of an array and then by using an iterator we print it's value backwords.

```JavaScript
class Stuff
{
  constructor()
  {
    this.a = 11;
    this.b = 22;
  }

 
  [Symbol.iterator]()
  {
    let i = 0;
    let self = this;
    return {
      next: function()
      {
        return {
          done: i > 1,
          value: self[i++ === 0 ? 'a' : 'b']
        };
      }
    }
  }

  get backwards()
  {
    let i = 0;
    let self = this;
    return {
      next: function()
      {
        return {
          done: i > 1,
          value: self[i++ === 0 ? 'b' : 'a']
        };
      },
      // make iterator iterable
      [Symbol.iterator]: function() { return this; }
    }
  }
}
```

That's how we will use this,

```JavaScript
let values = [100, 200, 300];
for (let i in values)
{
  console.log(`Element at pos ${i} is ${values[i]}`);
}

for (let v of values)
{
  console.log(`Value is ${v}`);
}

let stuff = new Stuff();
for (let item of stuff)
  console.log(`${item}`);

for (let item of stuff.backwards)
  console.log(`${item}`);
```

## Mediator

Mediator pattern adds a third party object to control the interaction between two objects. It allows loose coupling between classes by being the only class that has detailed knowledge of their methods.

According to Wikipedia 
> The mediator pattern defines an object that encapsulates how a set of objects interact. This pattern is considered to be a behavioral pattern due to the way it can alter the program's running behavior. In object-oriented programming, programs often consist of many classes. 

#### Example

We will be using an example of a person using a chat room. Here a chatroom act as a mediator between two people communicating.

```JavaScript
class Person {
  constructor(name) {
    this.name = name;
    this.chatLog = [];
  }

  receive(sender, message) {
    let s = `${sender}: '${message}'`;
    console.log(`[${this.name}'s chat session] ${s}`);
    this.chatLog.push(s);
  }

  say(message) {
    this.room.broadcast(this.name, message);
  }

  pm(who, message) {
    this.room.message(this.name, who, message);
  }
}
```

Creating chat room,

```JavaScript
class ChatRoom {
  constructor() {
    this.people = [];
  }

  broadcast(source, message) {
    for (let p of this.people)
      if (p.name !== source) p.receive(source, message);
  }

  join(p) {
    let joinMsg = `${p.name} joins the chat`;
    this.broadcast("room", joinMsg);
    p.room = this;
    this.people.push(p);
  }

  message(source, destination, message) {
    for (let p of this.people)
      if (p.name === destination) p.receive(source, message);
  }
}
```

That's how we will use this,

```JavaScript
let room = new ChatRoom();

let zee = new Person("Zee");
let shan = new Person("Shan");

room.join(zee);
room.join(shan);

zee.say("Hello!!");


let doe = new Person("Doe");
room.join(doe);
doe.say("Hello everyone!");
```
## Memento

Memento restore an object to its previous state.

According to Wikipedia 
> The memento pattern is a software design pattern that provides the ability to restore an object to its previous state. The memento pattern is implemented with three objects: the originator, a caretaker and a memento. 

#### Example

 We will be taking an example of a bank account in which we store our previous state and will have the functionality of undo.
 
 ```JavaScript
 class Memento {
  constructor(balance) {
    this.balance = balance;
  }
}
 ```
 Adding bank account,
 
 ```JavaScript
 class BankAccount {
  constructor(balance = 0) {
    this.balance = balance;
  }

  deposit(amount) {
    this.balance += amount;
    return new Memento(this.balance);
  }

  restore(m) {
    this.balance = m.balance;
  }

  toString() {
    return `Balance: ${this.balance}`;
  }
}
 ```
That's how we will use this,

```JavaScript
let bankAccount = new BankAccount(100);
let m1 = bankAccount.deposit(50);

console.log(bankAccount.toString());

// restore to m1
bankAccount.restore(m1);
console.log(bankAccount.toString());
```

## Observer

It allows a number of observer objects to see an event.

According to Wikipedia 
> The observer pattern is a software design pattern in which an object, named the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

#### Example

We will be taking an example of a person in which if a person falls ill, it will display a notification.

```JavaScript
class Event {
  constructor() {
    this.handlers = new Map();
    this.count = 0;
  }

  subscribe(handler) {
    this.handlers.set(++this.count, handler);
    return this.count;
  }

  unsubscribe(idx) {
    this.handlers.delete(idx);
  }

  fire(sender, args) {
    this.handlers.forEach((v, k) => v(sender, args));
  }
}

class FallsIllArgs {
  constructor(address) {
    this.address = address;
  }
}

class Person {
  constructor(address) {
    this.address = address;
    this.fallsIll = new Event();
  }

  catchCold() {
    this.fallsIll.fire(this, new FallsIllArgs(this.address));
  }
}
```

That's how we will use this,

```JavaScript
let person = new Person("ABC road");
let sub = person.fallsIll.subscribe((s, a) => {
  console.log(`A doctor has been called ` + `to ${a.address}`);
});
person.catchCold();
person.catchCold();

person.fallsIll.unsubscribe(sub);
person.catchCold();
```

## Visitor

It add operations to objects without having to modify them.

According to Wikipedia 
> The visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying the structures.

#### Example
We will be taking an example of NumberExpression in which it gives us the result og given expression.

```JavaScript
class NumberExpression
{
  constructor(value)
  {
    this.value = value;
  }

  print(buffer)
  {
    buffer.push(this.value.toString());
  }
}
```

Creating AdditionExpression,

```JavaScript
class AdditionExpression
{
  constructor(left, right)
  {
    this.left = left;
    this.right = right;
  }

  print(buffer)
  {
    buffer.push('(');
    this.left.print(buffer);
    buffer.push('+');
    this.right.print(buffer);
    buffer.push(')');
  }
}
```
That's how we will use this,

```JavaScript
// 5 + (1+9)
let e = new AdditionExpression(
  new NumberExpression(5),
  new AdditionExpression(
    new NumberExpression(1),
    new NumberExpression(9)
  )
);
let buffer = [];
e.print(buffer);
console.log(buffer.join(''));
```

## Strategy

It allows one of an algorithms to be selected on certain sitution.

According to Wikipedia 
> The strategy pattern is a behavioral software design pattern that enables selecting an algorithm at runtime. Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use. 

#### Example
We will take an example in which we have text processor that will display data based on strategy(HTML or Markdown).

```JavaScript
let OutputFormat = Object.freeze({
  markdown: 0,
  html: 1,
});

class ListStrategy {
  start(buffer) {}
  end(buffer) {}
  addListItem(buffer, item) {}
}

class MarkdownListStrategy extends ListStrategy {
  addListItem(buffer, item) {
    buffer.push(` * ${item}`);
  }
}

class HtmlListStrategy extends ListStrategy {
  start(buffer) {
    buffer.push("<ul>");
  }

  end(buffer) {
    buffer.push("</ul>");
  }

  addListItem(buffer, item) {
    buffer.push(`  <li>${item}</li>`);
  }
}
```

Creating TextProcessor class,

```JavaScript

class TextProcessor {
  constructor(outputFormat) {
    this.buffer = [];
    this.setOutputFormat(outputFormat);
  }

  setOutputFormat(format) {
    switch (format) {
      case OutputFormat.markdown:
        this.listStrategy = new MarkdownListStrategy();
        break;
      case OutputFormat.html:
        this.listStrategy = new HtmlListStrategy();
        break;
    }
  }

  appendList(items) {
    this.listStrategy.start(this.buffer);
    for (let item of items) this.listStrategy.addListItem(this.buffer, item);
    this.listStrategy.end(this.buffer);
  }

  clear() {
    this.buffer = [];
  }

  toString() {
    return this.buffer.join("\n");
  }
}
```
That's how we will use this,

```JavaScript
let tp = new TextProcessor();
tp.setOutputFormat(OutputFormat.markdown);
tp.appendList(["one", "two", "three"]);
console.log(tp.toString());

tp.clear();
tp.setOutputFormat(OutputFormat.html);
tp.appendList(["one", "two", "three"]);
console.log(tp.toString());
```

## State

It alter its behavior of an object when its internal state changes.

According to Wikipedia 
> he state pattern is a behavioral software design pattern that allows an object to alter its behavior when its internal state changes. This pattern is close to the concept of finite-state machines.

#### Example

We will be taking an example of a light switch in which if we turned on or off the switch it state changes.

```JavaScript
class Switch {
  constructor() {
    this.state = new OffState();
  }

  on() {
    this.state.on(this);
  }

  off() {
    this.state.off(this);
  }
}

class State {
  constructor() {
    if (this.constructor === State) throw new Error("abstract!");
  }

  on(sw) {
    console.log("Light is already on.");
  }

  off(sw) {
    console.log("Light is already off.");
  }
}
```

Creating state classes

```JavaScript
class OnState extends State {
  constructor() {
    super();
    console.log("Light turned on.");
  }

  off(sw) {
    console.log("Turning light off...");
    sw.state = new OffState();
  }
}

class OffState extends State {
  constructor() {
    super();
    console.log("Light turned off.");
  }

  on(sw) {
    console.log("Turning light on...");
    sw.state = new OnState();
  }
}
```

That's how we use this,

```JavaScript
let switch = new Switch();
switch.on();
switch.off();
```
## Template Method

It defines the skeleton of an algorithm as an abstract class, that how should it perforned.

According to Wikipedia 
> Template Method is a method in a superclass, usually an abstract superclass, and defines the skeleton of an operation in terms of a number of high-level steps.

#### Example

We will be taking an example of a chess game,

```JavaScript
class Game {
  constructor(numberOfPlayers) {
    this.numberOfPlayers = numberOfPlayers;
    this.currentPlayer = 0;
  }

  run() {
    this.start();
    while (!this.haveWinner) {
      this.takeTurn();
    }
    console.log(`Player ${this.winningPlayer} wins.`);
  }

  start() {}
  get haveWinner() {}
  takeTurn() {}
  get winningPlayer() {}
}
```

Creating our chess class,

```JavaScript
class Chess extends Game {
  constructor() {
    super(2);
    this.maxTurns = 10;
    this.turn = 1;
  }

  start() {
    console.log(
      `Starting a game of chess with ${this.numberOfPlayers} players.`
    );
  }

  get haveWinner() {
    return this.turn === this.maxTurns;
  }

  takeTurn() {
    console.log(`Turn ${this.turn++} taken by player ${this.currentPlayer}.`);
    this.currentPlayer = (this.currentPlayer + 1) % this.numberOfPlayers;
  }

  get winningPlayer() {
    return this.currentPlayer;
  }
}
```
That's how we will use this,

```JavaScript
let chess = new Chess();
chess.run();
```

----------------------------------------------------------------

That was all about <b> JavaScript Design Patterns </b>. I'll try to improve it further over time. It you think it needs some changes, open a pull request with improvmnents.
 <br>
 You would like to follow me on twitter ![@zeeshanhshaheen](https://twitter.com/zeeshanhshaheen) for more updates.
 
 -----------------------------------------------------------------
 
 ## License








