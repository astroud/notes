# [Intro to React Testing [Jest and React Testing Library Tutorial]](https://www.youtube.com/watch?v=ZmVBCpefQe8)

> Chris gives an introduction to React Testing and walks through a Jest and React Testing Library tutorial. Testing is a critical part of the React development process. There is no shortage of different ways to test our apps. This talk will give an intro to general testing principles and tools before diving into the specifics of testing React components using Jest and React Testing Library, the tooling recommended by the React core team.

> Guest presenter, Chris Schmitz, Senior Software Engineer at Handshake, will teach you how to get started with Jest and the React Testing Library.

Segments:
* [0:00](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=0s) - Intro and Agenda
* [2:23](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=143s) - What is a React Test?
* [3:53](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=233s) - Why Write Tests?
* [4:13](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=253s) - Documentation
* [5:17](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=317s) - Consistency 
* [6:10](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=370s) - Comfort and Confidence in Testing 
* [7:29](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=449s) - Productivity 
* [8:16](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=496s) - Types of Tests 
* [10:14](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=614s) - React Component Tests 
* [12:36](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=756s) - A Jest Test 
* [15:10](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=910s) - Walkthrough Begins 
* [18:53](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=1133s) - Rendering Components for Testing 
* [23:54](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=1434s) - Use DOM Testing Library for Querying the DOM 
* [31:14](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=1874s) - Rendering and Testing with React Testing Library 
* [35:11](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=2111s) - Simulating User Interaction 
* [41:45](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=2505s) - Testing Async Code 
* [49:00](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=2940s) - More Resources to Explore


## Types of Tests [8:16](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=496s):
- End-to-End: Firing up the app and simulating user behavior. Think of a robot actually sitting at a computer using the mouse and keyboard to perform the task.
- Integration: Verifying that multiple units work together. Examples, confirming components interact properly. This doesn't involve setting up the whole stack.
- Unit: Verify the functionality of a single function/component.
- Static: Catch typos and errors while writing code (examples: typescript, flow, eslint) 

## React Component Tests:
A Basic Test:

```js
const expected = true
const actual = false

if (actual !== expected) {
	throw new Error(`${actual} is not ${expected}`)
}

```


This is the core of a test. We expect something to happen or be returned and when it is not, an error is thrown.

But when the error is thrown:
`Error: true is not false`
the output is not useful for tracking down the issue.

[Jest](http://jestjs.io) is a test runner, an assertion library, and also some utilities for mocking.

A Jest Test:

```js
const expected = true
const actual = false

test("it works", () => {
	expect(actual).toBe(expected)
})
```

test runner portion: `test("it works", () => {`
assertion library portion: `expect(actual).toBe(expected)`


Jest's output for failed tests is more useful than the basic output above. When written well, the test output also can be used as documentation.


## [15:10](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=910s) - Walkthrough Begins

* [CodeSandboxes 0-starting-file](https://codesandbox.io/s/0-starting-file-zm2zt)
* [1-rendering-components-for-testing](https://codesandbox.io/s/1-rendering-components-for-testing-y69g5) 
* [2-use-dom-testing-library-for-querying-the-dom](https://codesandbox.io/s/2-use-dom-testing-library-for-querying-the-dom-bzh71)
* [3-rendering-and-testing-with-react-testing-library](https://codesandbox.io/s/3-rendering-and-testing-with-react-testing-library-0kvor)
* [4-simulating-user-interaction ](https://codesandbox.io/s/4-simulating-user-interaction-jg46k)
* [5-testing-async-code](https://codesandbox.io/s/5-testing-async-code-4wtsi)

Looking at example one:

>"Jest ships with `jsdom` which simulates a DOM environment as if you were in the browser. This means that every DOM API that we call can be observed in the same way it would be observed in a browser!" [Jest docs](https://jestjs.io/docs/en/tutorial-jquery)

This is really useful for giving us DOM APIs so we can write code like:
`const root = document.createElement("div")`

Buy that can be janky and lead to errors. With `@testing-library/react` we get [queries that allow us to find elements on the page](https://testing-library.com/docs/queries/about) (`getByLabelText`, `getByTitle`, `queryByTitle`, `getAllByTitle`, etc).

These queries are also easier to read than `document.querySelector…`.

Example given with a typo has been introduced into a label's `htmlFor` attribute. When searching via the dom:

`expect(root.querySelector("label").textContent.toBe("What needs to be done?"))`

the test passes. But when `getByLabelText` is used, a more useful error message is shown highlighting the issue:
> `Found a label with the text of: What needs to be done?, however no form control was found associated to that label. Make sure you're using the "for" attribute or "aria-labelledby" attribute correctly.`

Re: Style
You can eliminate some of the repetition: `expect().not.toBeNull()` because these "tests will throw an error if they fail anyway", so you can remove the assertion and simplify:

```js
expect(getByText("TODOS")).not.toBeNull();
expect(getByLabelText("What needs to be done?")).not.toBeNull();
expect(getByText("Add #1")).not.toBeNull();
```

to this:
```js
getByText("TODOS");
getByLabelText("What needs to be done?");
getByText("Add #1");
```


You can further refactor this:

```js
import React from "react";
import ReactDOM from "react-dom";
import { within } from "@testing-library/dom";

import { App } from "./App";


test("it works", () => {
	const root = document.createElement("div");
	ReactDOM.render(<App />, root);

	const { getByText, getByLabelText } = within(root);

	getByText("TODOS");
	getByLabelText("What needs to be done?");
	getByText("Add #1");
});
```

to this:
```js
import React from "react";
import { App } from "./App";
// no longer needed => import ReactDOM from "react-dom";

// In the place of the commented out code comes
// in testing-library/react
import { render } from "@testing-library/react";

// import ReactDOM from "react-dom";
// import { within } from "@testing-library/dom";
// const render = (component) => {
//   const root = document.createElement("div");
//   ReactDOM.render(component, root);

//   return within(root);
// };

test("it works", () => {
  const { getByText, getByLabelText } = render(<App />);

  getByText("TODOS");
  getByLabelText("What needs to be done?");
  getByText("Add #1");
});

```

The react testing library does a lot of stuff, but the render method is one that you'll be using most of the time.


## Simulating User Interaction [35:11](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=2111s)

`fireEvent` can be used to simulate user interaction:

```js
import { render, fireEvent } from "@testing-library/react";
```


### Example 4 [codesandbox.io]](https://codesandbox.io/s/4-simulating-user-interaction-jg46k)

```js
import React from "react";
import { render, fireEvent } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

import { App } from "./App";

test("it works", () => {
  const { getByText, getByLabelText } = render(<App />);

  getByText("TODOS");
  getByLabelText("What needs to be done?");
  getByText("Add #1");
});

// fireEvent
test("allows users to add items to their list", () => {
  const { getByText, getByLabelText } = render(<App />);

  const input = getByLabelText("What needs to be done?");
  const button = getByText("Add #1");

  // Simulate user events
  fireEvent.change(input, { target: { value: "Learn spanish" } });
  fireEvent.click(button);

  // Make assertion
  getByText("Learn spanish");
  getByText("Add #2");
});

// userEvent
test("user-events allows users to add...", () => {
  const { getByText, getByLabelText } = render(<App />);

  const input = getByLabelText("What needs to be done?");
  const button = getByText("Add #1");

  userEvent.type(input, "Learn spanish");
  userEvent.click(button);

  getByText("Learn spanish");
  getByText("Add #2");
});
```


With `userEvent from "@testing-library/user-event"` you can simulate individual keystrokes instead of submitting all of the text at one time into an input elements.

```js
userEvent.type(input, "Learn spanish")
```

> [`user-event`](https://github.com/testing-library/user-event) is a companion library for Testing Library that provides more advanced simulation of browser interactions than the built-in[`fireEvent`](https://testing-library.com/docs/dom-testing-library/api-events#fireevent) method. [Testing Library docs](https://testing-library.com/docs/ecosystem-user-event/)

## Testing async code [41:45](https://www.youtube.com/watch?v=ZmVBCpefQe8&t=2505s)
* [codesandbox.io](https://codesandbox.io/s/5-testing-async-code-4wtsi)

>"Benefit of abstracting away [for example] an API library into it's own module, so that you can do things in your tests here like mock out the entire module really easily. You could also think about setting up default resolve values...fixture code to make the set up a little bit easier."

Any time you mock something, you need to protect against false positives by making an assertion that the mock is actually hit. You can also assert that it was hit with the correct value:

```js
expect(mockCreateItem).toBeCalledTimes(1)
expect(mockCreateItem).toBeCalledWith(
	"/items",
	expect.objectContaining({ text: todoText })
)
```

It's not always relevant, but this is giving you a chance to assert that the data was sent outside of the component in the correct format.

`expect.objectContaining` is a Jest helper that allows you to mock out *part* of an object.

```js
	expect.objectContaining({ text: todoText })
```

### More Things to Explore
* Clearer assertions with [jest-dom](https://github.com/testing-library/jest-dom)
* Accessibility testing with [jest-axe](https://github.com/nickcolley/jest-axe)
* Testing hooks with [react-hooks-testing-library](https://react-hooks-testing-library.com)
* The code sandboxes from this talk
	* [CodeSandboxes 0-starting-file](https://codesandbox.io/s/0-starting-file-zm2zt)
	* [1-rendering-components-for-testing](https://codesandbox.io/s/1-rendering-components-for-testing-y69g5) 
	* [2-use-dom-testing-library-for-querying-the-dom](https://codesandbox.io/s/2-use-dom-testing-library-for-querying-the-dom-bzh71)
	* [3-rendering-and-testing-with-react-testing-library](https://codesandbox.io/s/3-rendering-and-testing-with-react-testing-library-0kvor)
	* [4-simulating-user-interaction ](https://codesandbox.io/s/4-simulating-user-interaction-jg46k)
	* [5-testing-async-code](https://codesandbox.io/s/5-testing-async-code-4wtsi)