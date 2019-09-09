# General JavaScript Terms

## Vanilla JavaScript

1. Vanilla JavaScript is just plain old JavaScript without any libraries or frameworks.
   - For example: `document.querySelector("#contact-container")` is the Vanilla JS way of selecting a DOM Element
   - `$("#contact-container")` is how you'd select with jQuery

## ES6

1. ECMAScript (ES) are a set of standards that can be applied to any scripting language. Every year, Ecma International updates these standards. JavaScript _implements_ these standards. [Here's a good article about the relationship between JS and ES.](https://medium.freecodecamp.org/whats-the-difference-between-javascript-and-ecmascript-cba48c73a2b5)
1. ES6 was published in 2015 and brought in a [unusually large list of new features](https://www.makeuseof.com/tag/es6-javascript-programmers-need-know/) in JavaScript, including our friends: 
      - Fat arrow functions 
      - `const` and `let` 
      - String template literals `${}`

## Framework vs. Library

1. Frameworks and libraries are both tools to help you build apps
1. A framework cares _how_ you build the thing and wants you to structure your files in a certain way.
   - Example: "Here's all the materials, tools, and blueprints you need to build a house! But the materials and tools only work if you follow our blueprints."
1. A library gives you tools and materials, but doesn't care how you use them.
   - Example: "Here's all the materials and tools you need to build a house. Have fun!"
1. jQuery is a library. Moment.js is a library. You can use these tools however and wherever you want in the context of a larger application.
1. Angular and Vue are popular JS frameworks. Entity is a framework for C#. Entity cares a lot about how you structure your files, naming conventions, etc. React doesn't give a hoot. 
1. React is technically a library, because React doesn't care how we structure our components. However, if someone asks you what frameworks you've learned, you should say React. Quibbling over whether React is a framework or a library is a bit like quibbling over whether a tomato is a fruit or a vegetable. Technically it's a fruit, but who cares?

## Methods and Properties
### Properties are data that lives inside an object. 
For example, `name` is a property on the `dog` object.
```js
const dog = {
    name: "Hank",
    breed: "Hound Dog"
}

console.log(dog.name) // Hank
// We can also reassign the dog's name
dog.name = "Frank";
console.log(dog.name) // Frank
```
### Methods are functions inside an object. 
Here's a simple method:
```js
const dog = {
    name: "Hank",
    breed: "Hound Dog",
    bark: () => {
        console.log("woof!")
    }
}

dog.bark(); // "woof!"
```
How about a more complicated example? Is `.innerHTML` a property, a method, or neither?
```js
document.querySelector("#container").innerHTML = "<h1>Hello, world</h1>"
```
It's a property! Every DOM object has a built-in `.innerHTML` property, and the code above just reset it. (It's just like when we reset the dog's name from Hank to Frank.)

How about this one? Method, property, or neither?
```js
document.querySelector("#submit-btn").addEventListener("click", () => {
    console.log("you clicked submit")
})
```
It's a method! Every DOM object has a built in method called `addEventListener` that accepts two arguments: the type of event (`"click"`) and the callback function that will run once the click happens.

How about this one? What's `render` in this context?
```js
class LocationList extends Component {
  render() {
    return (
      <article>
        <h1>Locations</h1>
        {this.props.locations.map(location => {
          return <div key={location.id}>
          {location.name}
          <p>{location.address}</p>
          </div>
        })}
      </article>
    );
  }
}
```
It's a method! Classes are types of JavaScript objects, so anything that looks like a function inside a class is a method.


## Return statements
You should use a return statement when you need to access something from inside a function outside the scope of that function. 

When you use a return statement, you always have to catch the returned value in a variable when you execute the function.

```js
const buildGreeting = name => {
    return `Hello, ${name}!`
}

const janeGreeting = buildGreeting("Jane");
const drakeGreeting = buildGreeting("Drake");

console.log(janeGreeting); // Hello, Jane!
console.log(drakeGreeting); // Hello, Drake!
```

The `buildGreeting` function can also be writtne like this: 
```js
const buildGreeting = name => `Hello, ${name}!`
```
If we don't use curly braces with our fat arrow function, we don't need to use a return statement- it returns automatically.


__Caveat about returns in general:__
Returning a value that depends on asynchronous operations (i.e. database calls) is iffy, because the function might try to return a value that hasn't come back yet. Instead, you should probably return the fetch call itself.

## Anonymous functions
1. Anonymous functions are just functions that don't need to be named. 
1. We've usually seen anonymous functions as _callback functions_ for something else. 
    - A callback function runs after something else is finished, like a database call or a click event.
Here's an example of an anonymous function:
```js
document.querySelector("#submit-btn").addEventListener("click", () => {
    // We're inside an anonymous function! It doesn't have a name, it's just gonna run once the click has happened
    console.log("you clicked submit")
})
```
Here's another one:
```js
const getOneAnimal= id => fetch(`${remoteURL}/animals/${id}`).then(animal => animal.json()).then((singleAnimal) => {
    // We're inside an anonymous function here! It doesn't have a name! 
    console.log(singleAnimal)
}),
```


## Function definitions vs. function executions vs. function references 
In both cases above, `.then()` and `.addEventListener()` are expecting function definitions or references to a function. We gave both examples function definitions. (We defined anonymous functions directly in the `.then()` and `.addEventListener()` methods.)

We could have written the second example this way:
```js
// This is no longer an anonymous function! It has a name now
const logAnimal = (singleAnimal) => {
    console.log(singleAnimal)
}

const getOneAnimal= id => fetch(`${remoteURL}/animals/${id}`).then(animal => animal.json()).then(logAnimal)

```
Here we're passing a *reference* to a function into the `.then()` method. Exact same results, different syntax. The JavaScript engine will see the variable name `logAnimal`, go find the value of that variable (i.e. a function!) and plug it in wherever it sees the `logAnimal` variable name.

It's important to distinguish between a function definition and a function execution.
```js
const buildGreeting = name => `Hello, ${name}!`

// This will console.log the execution of the function:
console.log(buildGreeting("Jim")) // Hello, Jim

// This will console.log the definition of the function:
console.log(buildGreeting) // you'll see the function itself in the console
```
You can't pass a function execution into a method (like `.then()`) that's expecting a definition or a reference. This won't work:

```js
const logAnimal = (singleAnimal) => {
    console.log(singleAnimal)
}

const getOneAnimal= id => fetch(`${remoteURL}/animals/${id}`).then(animal => animal.json()).then(logAnimal()) // since you're passing in an execution to logAnimal(), it won't wait for the data to come back from json-server

```




# React Terms

## React Native

1. React Native is a library built on top of React.js.
1. Instead of building web apps, it lets you build native mobile apps for iOS and Android

## React.js

This is just the normal React that we've been using.

## create-react-app

1. `create-react-app` is a command line tool built by Facbeook. It scafflods a React application for you so you don't have to go through a complicated set up process.
1. Most importantly, `create-react-app` sets up Webpack (a task runner, like Grunt) and Babel (a compiler that translates ES6 to older versions of JavaScript so that it's compatible with old browsers) for you, and it hides all of the configuration files so you can't accidentally break them. It's pretty sweet.

## Redux

Redux is a library for managing state in large, complex web apps. It's often used with React, but doesn't have to be. It's major overkill for the kind of apps y'all are building, but [here's a general overview if you want it](https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835)

## DOM and virtual DOM

1. The DOM stands for the Document Object Model, and is the programming interface for HTML documents. (i.e. it represents your page in a format that JavaScript can interact with).
1. DOM manipulation is at the heart of modern web apps, but it's slow and inefficient.
   - For example, if we're displaying a list of 10 items in a list and we update one of them with vanilla JS, we'd need to make a `GET` request from the database and then reprint all ten items again, even though only one has changed
1. React builds a virtual DOM (i.e. a lightweight copy of the actual DOM).
1. Every time you render a component in React, it updates the entire virtual DOM. Then it compares the updated virtual DOM to the actual (un-updated) DOM and figures out what's different. This process is called "diffing".
1. Once React has figured out the differences between the virtual DOM and the real DOM, it updates the real DOM _only where changes have been made_.
   - In the example above, React would be smart enough to only re-render the list item that you updated and leave the rest alone.

## Lifecycle and componentDidMount

Lifecycle methods in React run automatically in the background. Most of them are used only in [special circumstances](https://blog.bitsrc.io/react-16-lifecycle-methods-how-and-when-to-use-them-f4ad31fb2282). The only one we'll use routinely is `componentDidMount`, which runs when the component is appended to the DOM (i.e, when it "mounts").

## Props vs. params vs. parameters

1. In react, `props` always refer to data passed down from a parent component.
1. `params` is usually short for parameter. In the context of React applications, we've usually seen something like this:

```js
this.props.match.params.animalId;
```

In this context, `this.props.match.params` contains all of the route parameters. (Route parameters are anything with a colon in front of it when you define the route- basically any variable in the context of a route.) In the example below, `animalId` is the route param:

```js
path = "/animals/:animalId(d+)";
```

## Constructor props // super(props)

Sometimes you'll see React components written like this:

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

That's fine! It's just another way of writing it.

1. The constructor method runs as soon as an instance of this class is created, i.e. as soon as the component is mounted.
1. The constructor method calls the super method, which gives this component access to all of the properties and methods on React's base Component class. It's pretty complicated and probably not worth your time to figure out exactly what it does behind the scenes, but if you're dying to know, [here ya go](https://overreacted.io/why-do-we-write-super-props/).
1. JavaScript has since updated! Hooray for ESwhatever! Now we can add properties directly on classes instead of having to initialize them in the constructor method. Now we can just write:

```js
class Clock extends React.Component {
  state = { date: new Date() };

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

## Function vs Class component

A functional component does not keep track of its own state or have access to React's life cycle methods. It can only accept props from a parent and return a JSX string. Functional components are great for components that just display data and don't have any of their own logic. For example:

```js
// This is a fat arrow function without curly braces, so it automatically returns the JSX string. If you needed curly braces, you'd need to add a return statement
const AnimalCard => props => 
    <section>
        <h4>{props.animal.name}</h4>
        <img src={props.animal.url} alt="picture of animal" />
    </section>


export default AnimalCard;
```

A class based component is what we're more used to seeing in this course. It inherits from React's base component class and has access to all of its life cycle methods. It can also keep track of its own state and use `this.setState`. 

# Misc. Terms

## Feature

1. A feature of an app is something the user can do. When we talk about features, we're talking about things from the user's perspective, not the developer's.
1. Your tickets (or user stories) should represent features of the app- one ticket per feature.



