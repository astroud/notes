# Notes taken during creation of my [Frontend Mentor](http://frontendmentor.io) [Pomodoro React App](https://github.com/astroud/pomodoro-react-app)

- [Pomodoro Timer](https://pomodoro.astroud.vercel.app/)
- [github](https://github.com/astroud/pomodoro-react-app)
- [Frontend Mentor Submission](https://www.frontendmentor.io/solutions/pomodoro-react-app-2gFE6LaFn)

<hr />

Why does create-react-app create app.css and index.css?
- [official answer](https://github.com/facebook/create-react-app/issues/2720#issuecomment-312743064)
- [some detail in this comment too](https://stackoverflow.com/questions/44484907/index-css-vs-app-css-in-default-app-created-by-create-react-app-whats-the)

[This guide was helpful in understanding the file structure of a  React app](https://www.pluralsight.com/guides/file-structure-react-applications-created-create-react-app)

For commit messages, referenced:
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#summary)
- [Karma - Git Commit Msg](http://karma-runner.github.io/6.0/dev/git-commit-msg.html)

Docs: [create-react-app - Adding a Stylesheet](https://create-react-app.dev/docs/adding-a-stylesheet)

Including [a basic "smoke test"](https://create-react-app.dev/docs/running-tests#testing-components) to confirm a component rendered without throwing is a good starting point.

Smoke test:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
});
```


## Default Props and Functional Components
For functional components, the [preferred way to handle defaulting props is with object deconstruction and default assignment](https://github.com/yannickcr/eslint-plugin-react/issues/2396#issuecomment-539184761).

Where we once had:

```js
const TestComponent.defaultProps = {
  size: 'medium',
  type: 'primary',
}
```


We now have:

```js
const TestComponent = ({
  size = 'medium',
  type = 'primary',
}) => (
  // ...
)
```

If you're dealing with a lot of props, you can also assign default values below like this:

```js
const TestComponent = (props) => (
	const { name = 'Barry' } = props
  // ...
)

```


## Errors Encountered:

```
index.js:1 Warning: Invalid DOM property `for`. Did you mean `htmlFor`
```

> Since `for` is a reserved word in JavaScript, React elements use `htmlFor` instead. [docs](https://reactjs.org/docs/dom-elements.html#htmlfor)

```
Warning: You provided a `value` prop to a form field without an `onChange` handler. This will render a read-only field. If the field should be mutable use `defaultValue`. Otherwise, set either `onChange` or `readOnly`.
```

Trying to figure out how many button components to have—reading [Thinking in React](https://reactjs.org/docs/thinking-in-react.html#step-4-identify-where-your-state-should-live)


> from discord re: button behavior: "Not that significant... that sounds more like behaviour you'd be putting in a higher up component encapsulating the entire screen/menu, with an appropriate callback passed to the buttons to capture the clicks"

For the Settings font and color selections:
[Styling `input[type="radio"]` `label`s (not the buttons themselves)](https://www.markheath.net/post/customize-radio-button-css)

Do not use `display: none` to hide buttons. Will make them "unfocusable and unable to be navigated via the keyboard":

```css
.radio-toolbar input\[type="radio"\] { opacity: 0; position: fixed; width: 0; }
```


## Testing resources
[Testing React components | flaviocopes.com](https://flaviocopes.com/react-testing-components/)

How do I test for the existance of a className? [Stackoverflow](https://stackoverflow.com/questions/53389956/how-to-test-a-classname-with-jest-and-react-testing-library)

My test:

```js
it('renders a button to show settings', () => {
  render(<Controls type="settings" />)
  const settingsButton = screen.getByRole('button')
  expect(settingsButton.className).toEqual('pomodoro-app__preferences')
```

Remember that `screen.getByRole('button')` is returning a DOM node, so I can interact with it the same way that I would in a browser. That's why I can check to see if the className equals 'pomodoro-app__preferences'.

However, that approach is not robust because classList ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)) will return "the class or space-separated classes of the current element."

So you should instead check that the classList *contains* the desired class:

```js
expect(settingsButton.classList.contains('pomodoro-app__preferences')).toBe(true)
```


## [Identifying the minimal (but complete) Representation of UI State](https://reactjs.org/docs/thinking-in-react.html#step-3-identify-the-minimal-but-complete-representation-of-ui-state)

1. Think of all of the pieces of data in the application.
2. Ask three questions about each piece of data:
	1.  Is it passed in from a parent via props? If so, it probably isn’t state.
	2.  Does it remain unchanged over time? If so, it probably isn’t state.
	3.  Can you compute it based on any other state or props in your component? If so, it isn’t state.

Types of state:
* isTimerRunning
* ☑️ timer mode
* One of these two state approaches (see note below):
	* time left (Works if I use a function to set state)
	* stop time (Set when starting timer. Equal to current time + timer length.) 
* ☑️ pomodoro length
* ☑️ short break length
* ☑️ long break length
* ☑️ font preference 
* ☑️ accent color
* ☑️ settings pane visibility

**note:** Remember that React may batch updates to state to improve performance, so [you cannot rely on the current state value for updating state](https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous). Additional articles: [Making setInterval Declarative with React Hooks](https://overreacted.io/making-setinterval-declarative-with-react-hooks/) and [React hooks and “setInterval”](https://blog.davidvassallo.me/2020/04/09/react-hooks-and-setinterval/)




## [Identify Where Your State Should Live](https://reactjs.org/docs/thinking-in-react.html#step-4-identify-where-your-state-should-live)

After identifying the minimal set of app state, identify which component mutates, or *owns*, this state.

> Remember: React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. **This is often the most challenging part for newcomers to understand,** so follow these steps to figure it out:
> 
> **For each piece of state in your application:**
> -   Identify every component that renders something based on that state.
> -   Find a common owner component (a single component above all the components that need the state in the hierarchy).
> -   Either the common owner or another component higher up in the hierarchy should own the state.
> -   If you can’t find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common owner component.


Code formatting note:
Format the *ternary* operator's conditional expression, the truthy expression, and the falsey expression across three lines of code:

```js
result = isPlatinumMember
	? "I'm with truthy"
	: "I'm with falsey"
```


## [Controlled Components](https://reactjs.org/docs/forms.html)

[Controlled Radio Buttons](http://react.tips/radio-buttons-in-react-16/)

```
index.js:1 Warning: You provided a `checked` prop to a form field without an `onChange` handler. This will render a read-only field. If the field should be mutable use `defaultChecked`. Otherwise, set either `onChange` or `readOnly`.
```




[CSS Variables for React Devs](https://www.joshwcomeau.com/css/css-variables-for-react-devs/)


To update my color and font preferences, this works given all my styles live in `App.css` ([stackoverflow](https://stackoverflow.com/questions/45226712/how-to-change-css-root-variable-in-react)). It's not the best approach, but works:

```js
document.documentElement.style.setProperty("--font-current", fonts[event.target.font.value])
    document.documentElement.style.setProperty("--accent-color", colors[event.target.color.value])
```


css property `letter-spacing` [causes an issue](https://iamsteve.me/blog/entry/remove-letter-spacing-from-last-letter) with centered or right aligned text. The last line of is followed by white space equal to the letter-spacing. The solution is a negative `margin-right` equal to the letter spacing.


Implemented circular progress bar with [React Circular Progressbar](https://www.npmjs.com/package/react-circular-progressbar)


Ternary Error (when *attempting* to update event.target.innerText):
```js
event.target.innerText === 'START' ? 'PAUSE' : 'START'
```

> `Expected an assignment or function call and instead saw an expression  no-unused-expressions`

I was testing the `innerText` but forgot to assign the result ('START' or 'PAUSE') to `innerText`
- [stackoverflow](https://stackoverflow.com/questions/61287650/expected-an-assignment-or-function-call-and-instead-saw-an-expression-eslintno-u)
- [eslint docs](https://eslint.org/docs/rules/no-unused-expressions)




Implementing sound effects:
[Beep Boop! Announcing “use-sound” — A React Hook for Sound Effects](https://www.joshwcomeau.com/react/announcing-use-sound-react-hook/)

- dependency [howler.js](https://howlerjs.com)


error:
```
./src/App.jsModule not found: You attempted to import ../public/sounds/timesUp.mp3 which falls outside of the project src/ directory. Relative imports outside of src/ are not supported.
```

[Using the Public Folder](https://create-react-app.dev/docs/using-the-public-folder/) help me figure out a way to leave the audio files in /public so I moved /sounds to /src


## Writing Documentation
references:
- [Documentation Levels](https://www.swyx.io/documentation-levels/)
- [Mark's summary of the great Divio blogpost](https://github.com/reduxjs/redux/issues/3609)
- [The documentation system | Divio](https://documentation.divio.com)
- [art-of-readme](https://github.com/noffle/art-of-readme)
- [common-readme](https://github.com/noffle/common-readme)
- [standard-readme](https://github.com/richardlitt/standard-readme)
	- [standard-readme spec](https://github.com/RichardLitt/standard-readme/blob/master/spec.md)

## [useSound Hook](https://www.joshwcomeau.com/react/announcing-use-sound-react-hook/) & [Howler.js](https://github.com/goldfire/howler.js)

