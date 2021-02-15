Terms to understand:

- syntax extension: (syntactic extension)
- JSX:
- transpiler:
- Babel:

## In JSX

To include JavaScript: `{ 'this is treated as JavaScript code' }`


To comment (HTML comments donot work): `{/* */}`


With nested JSX, you must return a single element:

Valid: `<div><p></p><p></p></div>`
Invalid: `<p></p><p></p>`

React uses a rendering API called ReactDOM that renders JSX directly to the HTML DOM.

`ReactDOM.render(componentToRender, targetNode)`

Successesive renders to a targetNode overwrite the targetNode's existing HTML.


Because `class` is a reserved word in JavaScript, `className` is used. React follows the camelCase convention for all HTML attributes and event references (examples: `onClick`, instead of `onclick` and `onchange` becomes `onChange`).

Single or double quotes can be used:
- `<div className='myDiv'>`
- `<div className="myDiv">`


### Self-Closing JSX Tags

"Any JSX element can be written with a self-closing tag, and every element must be closed."

`<br>` and `<hr>` must be written as `<br />` and `<hr />`

Since any JSX element can be written as a self-closing tag, `<div></div>` can be written `<div />` which can be useful but has the downside that there's no way to include anything inside the div.


### Everything in React is a Component: Stateless Functional Components

React components can be created in two ways:

1. Via a JavaScript function
2. The ES6 class syntax

**JavaScript function**

A JavaScript function creates a *stateless functional component* that can receive and render data but does not manage or track changes to the data.

React requires that the function begins with a capital letter and it needs to return either JSX or `null`.


**ES6 class syntax**

[FCC lessson on ES6 class syntax](https://www.freecodecamp.org/learn/front-end-libraries/react/create-a-react-component)

So much is being glossed over here...

```
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    // Change code below this line



    // Change code above this line
  }
};
```


### Create a Component with Composition

Multiple React components can be combined thus:

return (
 <App>
  <Navbar />
  <Dashboard />
  <Footer />
 </App>
)

In this example, Navbar, Dashboard, & Footer and *children* of *App*.

When React sees a custom HTML tag, referencing another component `< />`, the custom tag is replaced with the component's markup.


### Pass Props to a Stateless Functional Component

props = prop[ertie]s



### Default Props

Default props can be specified for components. The default will be used unless you specify one. If you pass `null` as the value for a prop, React will use `null` instead of the default(s).

`UserCharacteristics.defaultProps = { hobbies: 'home brewing' }`

or multiple defaults:

```
itemName.defaultProps = {
  prop-x: value,
  prop-y: value
}
```

to override:

```
class Profile extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return <UserCharacteristics hobbies={'mma'} />
  }
};
```

In the example above, you could pass the number 100 instead of a string of text and it would work. To protect against this, React provides type-checking features (`propTypes`). Then, when a component receives props of the wrong type, it will throw a useful warning.

*Best Practice: Set `propTypes` when you know a prop's type ahead of time.*

`propTypes` is defined similarly as `defaultProps`

[more notes needed from FCC](https://www.freecodecamp.org/learn/front-end-libraries/react/use-proptypes-to-define-the-props-you-expect)


```
To set a propTypes, the syntax to be followed is

itemName.propTypes = {
  props: PropTypes.dataType.isRequired
};
```


> A common pattern is to try to minimize statefulness and to create stateless functional components wherever possible. This helps contain your state management to a specific area of your application. In turn, this improves development and maintenance of your app by making it easier to follow how changes to state affect its behavior.



### React: Create a Stateful Component

*Initializing state in the constructor*

**important** State — the potentially-changing data your application (and components) need to use/know about. One of React's big advantages is it's system for updating components as state changes (aka, the user interface updates).

> One of the most important topics in React is state. State consists of any data your application needs to know about, that can change over time. You want your apps to respond to state changes and present an updated UI when necessary. React offers a nice solution for the state management of modern web applications.

>You create state in a React component by declaring a state property on the component class in its constructor. This initializes the component with state when it is created. The state property must be set to a JavaScript object. Declaring it looks like this:

```
this.state = {
  // describe your state here
}
```

> You have access to the state object throughout the life of your component. You can update it, render it in your UI, and pass it as props to child components. The state object can be as complex or as simple as you need it to be. Note that you must create a class component by extending React.Component in order to create state like this.


thought to myself: "Perhaps I can think of state as global variables that components have access to, but only when state is created in the component?"


### React: Render State in the User Interface

> React uses what is called a virtual DOM, to keep track of changes behind the scenes. When state data updates, it triggers a re-render of the components using that data - including child components that received the data as a prop. React updates the actual DOM, but only where necessary. This means you don't have to worry about changing the DOM. You simply declare what the UI should look like.

> Note that if you make a component stateful, no other components are aware of its `state`. Its `state` is completely encapsulated, or local to that component, unless you pass `state` data to a child component as `props`. *This notion of encapsulated `state` is very important because it allows you to write certain logic, then have that logic contained and isolated in one place in your code.*


### React: Render State in the User Interface Another Way

You can also render state by assigning state (or props) to new variables, performing calculations, or other manipulations. This is done in the `render()` function before the `return`.


### React: Set State with this.setState

*Setting state with `setState`*

You can call `setState` within your component class: `this.setState()` and passing in an object with key-value pairs.

The keys are the state properties and their values are the updated state data.

You do not update state directly, instead use `this.setState()` and React will do it for you. To improve performance, state changes may be batched together.

>What this means is that state updates through the setState method can be asynchronous. There is an alternative syntax for the setState method which provides a way around this problem. This is rarely needed but it's good to keep it in mind! Please consult the React documentation for further details.


### React: Bind `this` to a Class Method

Q: Why doesn't React do this by default? There must be a reason you wouldn't want `this` bound to the class method.

[I do not understand this lesson and need to heavily revisit it](https://www.freecodecamp.org/learn/front-end-libraries/react/bind-this-to-a-class-method)

"Another way of thinking about it is that `this` refers to the object on the left side of the dot when calling a method." —Tania Rascia

[Tania Rascia -  This, Bind, Call, and Apply](https://www.taniarascia.com/this-bind-call-apply-javascript/)

[MDN - this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)


### React: Use State to Toggle an Element

State updates may be asynchronous because React may batch multiple `setState()` calls to improve performance.

Because of this, you should never rely on the previous value of `this.state` or `this.props` which may have changed somewhere else.

Instead, you should pass setState a function that allows you to access state and props. This will guarantee you are working with the "most current values" of state and props.

Examples:

```
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

```
this.setState(state => ({
  counter: state.counter + 1
}));
```

*Note: The object literal must be wrapped in paraentheses or JavaScript will think it's a block of code.*


First, remember to bind `this` to the method in the constructor:

```
this.toggleVisibility = this.toggleVisibility.bind(this);
```

To toggle the visibility state:

```
toggleVisibility() {
    this.setState(state => ({
      visibility: !state.visibility
    }));
  }
```


### React: Create a Controlled Input

controlled component — a React component that controls the internal state of an element (examples: `input`, `textarea`, and other form elements including the HTML `form` element)

Some HTML elements (`input`, `textarea`, and others) handle their own state in the DOM *as the user types.*

This mutable state (mutable because typing mutates the existing text) can be moved into a React's component state. *"The user's input becomes part of the application state, so React controls the value of that input field. Typically, if you have React components with input fields the user can type into, it will be a controlled input form."* (note to self: by "controlled input form, I think FCC is saying React will be controlling the form and storing it's state in the React component.")


This challenge was relatively straight forward. But I got hung up on updating the state as the user types.

I was passing the `state` to `setState`, resulting in these errors:

```
react-dom.production.min.js:110 TypeError: Cannot read property 'value' of null
    at ControlledInput.<anonymous> (<anonymous>:53:31)
    at Gf (VM4345 react-dom.production.min.js:71)
    at kb (VM4345 react-dom.production.min.js:71)
    at wh (VM4345 react-dom.production.min.js:100)
    at kg (VM4345 react-dom.production.min.js:120)
    at lg (VM4345 react-dom.production.min.js:120)
    at fc (VM4345 react-dom.production.min.js:127)
    at vb (VM4345 react-dom.production.min.js:126)
    at flushInteractiveUpdates (VM4345 react-dom.production.min.js:189)
    at te (VM4345 react-dom.production.min.js:25)
ag @ react-dom.production.min.js:110
c.callback @ react-dom.production.min.js:115
If @ react-dom.production.min.js:73
Jf @ react-dom.production.min.js:74
ic @ react-dom.production.min.js:135
fc @ react-dom.production.min.js:127
vb @ react-dom.production.min.js:126
flushInteractiveUpdates @ react-dom.production.min.js:189
te @ react-dom.production.min.js:25
Nb @ react-dom.production.min.js:41
interactiveUpdates @ react-dom.production.min.js:189
Ye @ react-dom.production.min.js:41
react-dom.production.min.js:126 Uncaught TypeError: Cannot read property 'value' of null
    at ControlledInput.<anonymous> (<anonymous>:53:31)
    at Gf (VM4345 react-dom.production.min.js:71)
    at kb (VM4345 react-dom.production.min.js:71)
    at wh (VM4345 react-dom.production.min.js:100)
    at kg (VM4345 react-dom.production.min.js:120)
    at lg (VM4345 react-dom.production.min.js:120)
    at fc (VM4345 react-dom.production.min.js:127)
    at vb (VM4345 react-dom.production.min.js:126)
    at flushInteractiveUpdates (VM4345 react-dom.production.min.js:189)
    at te (VM4345 react-dom.production.min.js:25)
```

However, the errors crashed the `render()`, resulting in an empty screen. Frustratingly, the code still passed the tests.

Instead, I needed to simply `setState` based on the current value of `event.target.value`.

This works because the `input` field's state is only being manipulated by the user's typing and `handleChange`, so there's no chance of another component affecting state. And `event.target.value` always contains the new, complete value of the input.

```
  handleChange(event) {
    console.log(event.target.value);

    this.setState({
      input: event.target.value
    });
  }
```


### React: Pass State as Props to Child Components

A common pattern is to restrict all `state` to a stateful component. This stateful component then renders child components and provides them the relevant pieces of that `state` in the form of `props`.

For an example, a website might be broken down into an `App` component that contains the state and then renders `Navbar`, `Content`, and `Footer` child components, passing in the relevant pieces of state, but only what each child component needs.

There are two important paradigms at play here:

1. *unidirectional data flow* — the state descends from the stateful (parent) component to the child components, only providing–as props–the pieces of state that each child component needs.

2. By limiting complex stateful apps into a few (possibly one) stateful component, the child components are only tasked with receiving state as props and then rendering the UI.

This is similar to the `Model View Controller (MVC)` approach. But in this case, only state logic and UI logic are being separated. This separation greatly simplifies the building and management of complex applications.

My difficulty or pain point in the challenge: I discovered you cannot simply pass state to a child component like this:

`<Navbar {this.state.name}/>`


Instead, you must specify the `prop` "name" (in this case, the prop was actually "name") and then you set the prop equal to the state you want Navbar to have access to:

`<Navbar name={this.state.name}/>`


### React: Pass a Callback as Props

>You can pass state as props to child components, but you're not limited to passing data. You can also *pass handler functions* or *any method that's defined on a React component to a child component*. This is how you allow child components to interact with their parent components. You pass methods to a child just like a regular prop. It's assigned a name and you have access to that method name under this.props in the child component.

### React: Use the Lifecycle Method componentWillMount

> React components have several special methods that provide opportunities to perform actions at specific points in the lifecycle of a component. These are called *lifecycle methods*, or *lifecycle hooks*, and allow you to catch components at certain points in time. This can be before they are rendered, before they update, before they receive props, before they unmount, and so on. Here is a list of some of the main lifecycle methods:
> `componentWillMount()`
> `componentDidMount()`
> `shouldComponentUpdate()`
> `componentWillUnmount()`
> `componentDidUpdate()`

> Note: The componentWillMount Lifecycle method will be deprecated in a future version of 16.X and removed in version 17.

"The `componentWillMount()` method is called before the `render()` method when a component is being mounted to the DOM."

[Diagram of React Lifecycle Methods](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)


### React: Use the Lifecycle Method componentDidMount

> The best practice with React is to place API calls or any calls to your server in the lifecycle method `componentDidMount()`. This method is called after a component is mounted to the DOM. Any calls to `setState()` here will trigger a re-rendering of your component. When you call an API in this method, and set your state with the data that the API returns, it will automatically trigger an update once you receive the data.


### React: Add Event Listeners

> The `componentDidMount()` method is also the best place to attach any event listeners you need to add for specific functionality. React provides a synthetic event system which wraps the native event system present in browsers. This means that the synthetic event system behaves exactly the same regardless of the user's browser - even if the native events may behave differently between different browsers.

> You've already been using some of these synthetic event handlers such as `onClick()`. React's synthetic event system is great to use for most interactions you'll manage on DOM elements. However, if you want to attach an event handler to the document or window objects, you have to do this directly.

Challenge pain point:

`document.addEventListener('keydown', handleKeyPress())`
or `document.addEventListener('keydown', handleKeyPress`

results in error:
```
ReferenceError: handleKeyPress is not defined
    at MyComponent.componentDidMount (<anonymous>:50:16)
    at ic (react-dom.production.min.js:134)
    at fc (react-dom.production.min.js:127)
    at vb (react-dom.production.min.js:126)
    at ub (react-dom.production.min.js:126)
    at vd (react-dom.production.min.js:124)
    at ra (react-dom.production.min.js:123)
    at Fd (react-dom.production.min.js:138)
    at jc (react-dom.production.min.js:138)
    at Ta.render (react-dom.production.min.js:193)
```

The clue to solving this issue is "handleKeyPress is not defined". You need to specify where to find the method like this:

`document.addEventListener('keydown', this.handleKeyPress);`


### React: Optimize Re-Renders with shouldComponentUpdate

[React docs: shouldComponentUpdate()](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)









































