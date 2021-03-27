# [Tutorial: Intro to React](https://reactjs.org/tutorial/tutorial.html#what-are-we-building)

Complements the  [step-by-step guide](https://reactjs.org/docs/hello-world.html) in the documentation.
My notes for the documentation:  [[React - Reactjs Official step-by-step guide]]

<hr>

> In this tutorial, we’ll show how to build an [interactive tic-tac-toe game](https://codepen.io/gaearon/pen/gWWZgR?editors=0010) with React.

Encountered [error](https://github.com/facebook/create-react-app/issues/10601) due to npm bug caching older versions of create-react-app:

```bash
❯ npx create-react-app react-tutorial

You are running `create-react-app` 4.0.2, which is behind the latest release (4.0.3).

We no longer support global installation of Create React App.

Please remove any global installs with one of the following commands:
- npm uninstall -g create-react-app
- yarn global remove create-react-app
```

Solutions:

Installing the latest version of npm fixes the issue:

```bash
npm install npm@latest -g
```


But you can also force npx to download the latest version of create-react-app:

```bash
npx create-react-app@latest myapp
```


<hr>

## Overview
### What is React?

> React is a declarative, efficient, and flexible JavaScript library for building user interfaces. It lets you compose complex UIs from small and isolated pieces of code called “components”.

> We use components to tell React what we want to see on the screen. When our data changes, React will efficiently update and re-render our components.

> A component takes in parameters, called `props` (short for “properties”), and returns a hierarchy of views to display via the `render` method.

> The `render` method returns a _description_ of what you want to see on the screen. React takes the description and displays the result. In particular, `render` returns a **React element**, which is a lightweight description of what to render. Most React developers use a special syntax called “JSX” which makes these structures easier to write.

> The `<div />` syntax is transformed at build time to `React.createElement('div')`. The example above is equivalent to:

```javascript
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

> `createElement()` is described in more detail in the [API reference](https://reactjs.org/docs/react-api.html#createelement)

> You can put _any_ JavaScript expressions within braces inside JSX. Each React element is a JavaScript object that you can store in a variable or pass around in your program.

> Passing props is how information flows in React apps, from parents to children.

### [Making an Interactive Component](https://reactjs.org/tutorial/tutorial.html#making-an-interactive-component)

> As a next step, we want the Square component to “remember” that it got clicked, and fill it with an “X” mark. To “remember” things, components use **state**.

> React components can have state by setting `this.state` in their constructors. `this.state`should be considered as private to a React component that it’s defined in.

> First, add a constructor to the class to initialize the state:

```javascript
class Square extends React.Component {
  constructor(props) {    super(props);    this.state = {      value: null,    };  }
  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

> In [JavaScript classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), you need to always call `super` when defining the constructor of a subclass. All React component classes that have a `constructor` should start with a `super(props)` call.

> **[Constructor (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#constructor "Permalink to Constructor")**
> 
> The [constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) method is a special method for creating and initializing an object created with a `class`. There can only be one special method with the name "constructor" in a class. A [`SyntaxError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError) will be thrown if the class contains more than one occurrence of a `constructor` method.
> 
> A constructor can use the `super` keyword to call the constructor of the super class.


## [Completing the Game](https://reactjs.org/tutorial/tutorial.html#completing-the-game)

### Lifting State Up

> To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead. The parent component can pass the state back down to the children by using props; this keeps the child components in sync with each other and with the parent component.

Refactoring the state into the shared board parent component:

> Lifting state into a parent component is common when React components are refactored...let add a constructor to the Board and set the Board’s initial state to contain an array of 9 nulls corresponding to the 9 squares:

> We [also] need to change what happens when a Square is clicked. The Board component now maintains which squares are filled. We need to create a way for the Square to update the Board’s state. Since state is considered to be private to a component that defines it, we cannot update the Board’s state directly from Square.
>
> Instead, we’ll pass down a function from the Board to the Square, and we’ll have Square call that function when a square is clicked.

> Note
>
>The DOM `<button>` element’s `onClick` attribute has a special meaning to React because it is a built-in component…In React, it’s conventional to use `on[Event]` names for props which represent events and `handle[Event]` for the methods which handle the events.

> Keeping the state of all squares in the Board component will allow it to determine the winner in the future.

> Since the Square components no longer maintain state, the Square components receive values from the Board component and inform the Board component when they’re clicked. In React terms, the Square components are now **controlled components**. The Board has full control over them.



### [Why Immutability Is Important](https://reactjs.org/tutorial/tutorial.html#why-immutability-is-important)

There are two main approaches to changing data:

1. Mutate the data by directly changing it's values
2. Replace the data with a new copy, containing the updated values

There are several benefits to an immutable approach (updating data with a copy):
- "Immutability makes complex features much easier to implement…an ability to undo and redo certain actions is a common requirement in applications. Avoiding direct data mutation lets us keep previous versions of the game’s history intact, and reuse them later."
- Detecting changes in mutable objects is more difficult. 
- "Determining when to re-render in React. The main benefit of immutability is that it helps you build _pure components_ in React. Immutable data can easily determine if changes have been made, which helps to determine when a component requires re-rendering. You can learn more about `shouldComponentUpdate()` and how you can build _pure components_ by reading [Optimizing Performance](https://reactjs.org/docs/optimizing-performance.html#examples)."

### [Function Components](https://reactjs.org/tutorial/tutorial.html#function-components)

> In React, **function components** are a simpler way to write components that only contain a `render` method and don’t have their own state. Instead of defining a class which extends `React.Component`, we can write a function that takes `props` as input and returns what should be rendered. Function components are less tedious to write than classes, and many components can be expressed this way.


## [Adding Time Travel](https://reactjs.org/tutorial/tutorial.html#adding-time-travel)

### [Storing a history of moves](https://reactjs.org/tutorial/tutorial.html#storing-a-history-of-moves)

### [Picking a Key](https://reactjs.org/tutorial/tutorial.html#picking-a-key)

> Warning: Each child in an array or iterator should have a unique “key” prop. Check the render method of “Game”.

> In the tic-tac-toe game’s history, each past move has a unique ID associated with it: it’s the sequential number of the move. The moves are never re-ordered, deleted, or inserted in the middle, so it’s safe to use the move index as a key.
> 
> In the Game component’s `render` method, we can add the key as `<li key={move}>` and React’s warning about keys should disappear


After adding eslint and airbnb's lint rules, I enountered:

`'onClick' is missing in props validation`

I tried adding

```javascript
Board.PropTypes = {

squares: PropTypes.array.isRequired,

onClick: PropTypes.func.isRequired,

}
```

And the error persisted because it should have been `Board.propTypes` (note the lowercase 'p').
