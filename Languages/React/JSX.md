```js
const heading=React.createElement({
	'div',{id='heading'},"This is heading"
})

const jsxHeading=<h1 className='jsxHeading'>This is jsxHeading</h1>
```

Now many of use think heading variable is a HTML as React.createElement creates a HTML right?
But no it is a Object that when rendered converts into HTML

Writing React in React.createElement fashion is mehhh...
So react devs used a notation known as JSX and variable named jsxHeading in above example is a valid jsx
But again jsxHeading is not a pure JavaScript as js only understands ES6 i.e., ECMAScript so how dose this code works?
when code is executed it first transpiled by bundler (babel) and then it is converted into ES6 and then rendered


#### Element v/s Component

React Element:
- Plain JavaScript object that describes what you want to see on the screen. It is an immutable, lightweight description of a DOM node or another component.
- Elements are typically created using JSX, which is syntactic sugar for `React.createElement()`.
- An element specifies the type of the UI element (e.g., `'div'`, `'p'`, or a custom component name) and its properties (props) and children.
- Elements are the "what" of your UI – they are the instructions React uses to build the actual DOM.

React Component:
- Component is a reusable, self-contained piece of code that defines both the behavior and appearance of a part of your UI.
- Components can be either functional components (JavaScript functions that return JSX) or class components (JavaScript classes that extend `React.Component` and have a `render()` method).
- Components take in data (props) as input and return React elements as output, describing the UI they represent.
- Components are the "how" of your UI – they encapsulate logic, manage state, and define how elements are rendered and interact.

## Q: What is the difference between `JS expression and JS statement`?

A: A `JS expression` returns a value that we use in the application. for example:

```js
1 + 2 // expresses
"foo".toUpperCase() // expresses 'FOO'
console.log(2) // logs '2'
isTrue ? true : false // returns us a true or false value based on isTrue value
```

A `JS statement`, does not return a value. for example:

```js
let x; // variable declaration
if () { } // if condition
```

If we want to use JS expression in JSX, we have to wrap in {/* expression slot */} and if we want to use JS statement in JSX, we have to wrap in {(/* statement slot */)};

