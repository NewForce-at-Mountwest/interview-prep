# C# Vocabulary/ Interview Terms
We used or talked about everything in this document, at least briefly. Here are some words to put around the things you've already done so that you can do yourself  justice in an interview.
***

### 1. Web apps vs. APIs
- Both Web Apps and APIs communicate with the client (in our case, usually a browser or Postman) through HTTP requests.
- When an API recieves a `GET` request, it (usually) sends back JSON
- When a web app recieves a `GET` request, it sends back an HTML string
- Web Apps are meant for human interaction through views. APIs are meant for system-to-system interactions (information exchange programatically )
- A web app can _consume_ one or many APIs. An API can be consumed by one or many web apps.


### 2. C#, .NET, .NET Core, ASP.NET, ADO.NET, Entity

##### Language vs. Framework
- **C#** is the programming language that runs on the **.NET framework**. 
- [Without .NET, C# is just a bunch of syntax rules.](https://www.quora.com/What-would-C-be-without-the-NET-Framework) 
- .NET gives us a set of class libraries (any time we add `using System.Whatever` we're leveraging .NET)
- The .NET framework can be used with other languages (F# and Visual Basic)
![diagram of .NET](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/DotNet.svg/1024px-DotNet.svg.png)
- From around 2000 to 2014, .NET only ran on windows. In 2014, Microsoft released **.NET Core**, a cross-platform and open-source version of .NET. We've used .NET Core for the entire server-side part of the course.
- **ASP.NET** extends the .NET framework with tooling for building web apps. This includes both APIs and MVC web apps. The only things we built that _weren't_ in ASP.NET were the console apps from way back in the first few weeks.

##### Data Access Tools
- **ADO.NET** is a tool for interfacing with a database that comes built in with .NET. It's very fast and has almost no abstraction (aka black magic). When we used syntax like ` SqlDataReader reader = cmd.ExecuteReader();` and `reader.Read()`, that was ADO.NET. 
    - Developer writes raw SQL
    - Developer handles mapping data that comes back from db to models (for example, if you get a bunch of rows of student data back from the database, you have to loop through the rows of data and build individual instances of a `Student` model and add them to a list)
- **Entity Framework** is an ORM (Object Relational Mapper) that is built on top of ADO.NET. It's heavily abstracted, less efficient, but faster to write and read.
    - Developer writes Linq queries, Entity translates them into SQL
    - Entity handles mapping the data that comes back from the database to your models
- We also (very briefly!) talked about Dapper, which we didn't use. Dapper is a lightweight ORM between ADO.NET and Entity. 


### 3. MVC
[MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) stands for **model-view-controller** and is a common architectural pattern across languages and frameworks. It's not specific to the C#/.NET ecosystem. 


### 4. HTTP Requests
- HTTP stands for **hypertext transfer protocol**. It enables a client to communicate with a server. HTTP gives clients and servers a common language.
- You might have a client written in JavaScript and a server written and C#. HTTP is the universal language that allows them to send requests and responses to each other even though they're built in different languages.
- HTTP verbs include `GET`, `POST`, etc.


### 5. OOP (Object Oriented Programming)
[This is a great, simply written resource for how OOP works in Java](https://stackify.com/oops-concepts-in-java/). Almost all of it will be applicable to C#. Rather than me paraphrasing, just read this!

### 6. RESTful APIs
* REST stands for Representational State Transfer and is an architecture style
* It gets thrown around as a buzzword to mean any API that communicates via HTTP requests (so basically every API we've built and used)
* Actually! Roy Fielding came up with the term REST in his 2000 dissertation
* For an API to actually be RESTful, it has to fulfill all six guiding of these constraints (most APIs don’t) 
* [You can read about those guiding constraints here](https://restfulapi.net/)


### 7. Strictly typed
When we say that C# is a strictly typed language, we mean that the compiler needs to know what _type_ of data everything is. If we declare a variable, we have to tell the compiler whether it's an `int` or a `bool` or a `string`, etc. If we create a list, we have to tell the compiler what data type the list will hold. 



### 8. Inheritance vs. interface
- Inheritance allows one class to use properties and methods from another class. 
    - This is useful when we're building custom types that have common properties or behaviors and it prevents duplicate code. 
    - When a `Student` class inherits from a `Person` class, we call `Student` the _derived class_ and `Person` the _base class_. We can also say that `Student` extends `Person`. 

- Interfaces allow us to write contracts that say a class _must_ have certan methods or properties.  
    - For example, we might build an interface that says any class that implements it must have a `FirstName` property and a `LastName` property. 
    - If we have `Person` implement our interface, we have to include those properties on our class or we'll get build errors. 

A class can implement more than one interface, but it can only inherit from one class. 


### 9. Inheritance in C# vs inheritance in JS
- In C#, we use class-based inheritance.
- In JavaScript, what often gets called inheritance is actually objects linking to other objects (OLOO) through the prototype chain
Here's an example of inheritance in C#:
```c#
class Animal {
    public string Species {get; set;}
    public List<string> FavoriteFoods {get; set;} = new List<string>();
}

class Dog : Animal {
    public string Breed {get; set;}
}

Dog fido = new Dog();

// We can assign data and call methods on an instance of a class
fido.Species = "C.lupus";
fido.Breed = ""Border collie";
fido.FavoriteFoods.Add("peanut butter");

// We CAN'T assign data or call methods on the class definition 
// This won't work:
Animal.species = "something";
Dog.Breed = "something else";

```
In this example, we only have one object in memory: `fido`. `Dog` and `Animal` are blueprints, but they don't have behavior on their own. We couldn't reference `Animal.Species` or `Dog.Breed`. If we wanted to _use_ either of these blueprints, we'd have to create an instance of the class with the `new` keyword. 

Here's an example of the prototype chain in JavaScript:
```js
const animal = {
    kingdom: "animalia",
    FavoriteFoods: []
}

// dog will inherit everything from animal
const dog = Object.create(animal);
dog.species = "c.Lupus";

const fido = Object.create(dog);
```
At this point, we have _three_ distinct objects in memory:  `animal`, `dog`, and `fido`. All three have data and behavior. All of the following code is valid in JavaScript, whereas the equivalent code would not be valid in C#.
```js
console.log(animal.kingdom) // "animalia"
console.log(dog.species) // "c.Lupus"
console.log(fido.kingdom) // also "animalia", because fido is linked to animal via the prototype chain
```
When we access `fido.kingdom`, the JavaScript engine looks for a property named `kingdom` in the following places and in the following order
1. On the `fido` object. If it doesn't find `kingdom` on `fido`, it moves up to:
1. `dog`, its closest prototype. If it can't find it there, it moves up the chain to:
1. `animal`, which has a `kingdom` property! Hooray! 

[This is an article aimed at mid and senior level developers, but it's great if you want a deep dive into classical vs. prototypal inheritance.](https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9)

### 10. Abstract class vs. interface
- An abstract class allows you to create functionality that subclasses can implement or override. You can't create instances of abstract classes; they're just used for inheritance. 
- An interface only allows you to define what functionality should be there, but not how it will work. 
- A class can inherit from only one abstract class, but it can implement multiple interfaces. 







