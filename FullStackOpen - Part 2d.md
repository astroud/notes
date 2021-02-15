# [Altering data in server](https://fullstackopen.com/en/part2/altering_data_in_server)


## REST

- resources - individual dataobjects
- Every resource has a unique address, it's URL. For example: 
	- `/notes/` would contain all of the notes
	- `/notes/3` would be the third note
- HTTP GET requests are used to retrieve resources and 
- HTTP POST requests can be used to create them. For example:
	- A new note could be created with a HTTP POST request to the notes URL. The new note would be in the `body` of the request and the `Content-Type` request header would have the value `application/json` (because the *json-server* tool we're using requires the JSON format)


## Sending Data to the Server

Remember, to restart json-server:

````js
json-server --port 3001 --watch db.json
````

### Revisiting the notes application

When updating `addNote` to post to the server, we eliminate the *id* property "since it's better to let the server generate ids for our resources"

HTTYP requests can be inspected in the browser's *Network* tab. You can 
- check the POST request headers are what you intended or expected
- you can check the values
- When using *axios* and JS objects, axios will automatically set the appropriate `application/json` value for the `Content-Type` header

After updating `addNote` to post to the server and update our application's state, the code looks like this:

After posting to the server, the response is used to update state with `setNotes(notes.concat(response.data))`

Remember that state should be updated with a new object which is why the note is concatenated (it creates a copy of the list with the new note appended).

```js
addNote = event => {
  event.preventDefault()
  const noteObject = {
    content: newNote,
    date: new Date(),
    important: Math.random() > 0.5,
  }

  axios
    .post('http://localhost:3001/notes', noteObject)
    .then(response => {
      setNotes(notes.concat(response.data))      setNewNote('')    })
}
```

>"Once the data returned by the server starts to have an effect on the behavior of our web applications, we are immediately faced with a whole new set of challenges arising from, for instance, the asynchronicity of communication. This necessitates new debugging strategies, console logging and other means of debugging become increasingly more important, and we must also develop a sufficient understanding of the principles of both the JavaScript runtime and React components. Guessing won't be enough."

We can confirm the data made it to the server by inspecting the state of the backend server e.g. through the browser: http://localhost:3001/notes

There are more advanced methods and tools like [postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) but this is sufficient for now.

note: We're currently setting the note's creation time in the browser (which can be incorrect). It's better to have the server timestamp new notes.

## Changing the importance of notes

**[How to create a unique event handler for each item or element:](https://fullstackopen.com/en/part2/altering_data_in_server#changing-the-importance-of-notes)**

> "Notice how every note receives its own _unique_ event handler function, since the _id_ of every note is unique."

> "Individual notes stored in the json-server backend can be modified in two different ways by making HTTP requests to the note's unique URL. We can either _replace_ the entire note with an `HTTP PUT` request, or only change some of the note's properties with an `HTTP PATCH` request."

**Important, this code appears to work but is *dangerous*:**

```js
const note = notes.find(n => n.id === id)
note.important = !note.important

axios.put(url, note).then(response => {
  // ...
```

In the above code, `note` is a *reference* to a note in the `notes` array. It is not an independent  copy and cause issues if the note's attributes are changed—*never mutate state directly in React*.

This is the proper way to update a note:

```js
const toggleImportanceOf = id => {
  const url = `http://localhost:3001/notes/${id}`
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, important: !note.important }

  axios.put(url, changedNote).then(response => {
    setNotes(notes.map(note => note.id !== id ? note : response.data))
  })
}
```

> **Note:** "that the new object _changedNote_ is only a so-called [shallow copy](https://en.wikipedia.org/wiki/Object_copying#Shallow_copy), meaning that the values of the new object are the same as the values of the old object. If the values of the old object were objects themselves, then the copied values in new object would reference the same objects that were in the old object."


## [Extracting communication with the backend into a separate module](https://fullstackopen.com/en/part2/altering_data_in_server#extracting-communication-with-the-backend-into-a-separate-module)

> "The **single-responsibility principle** (**SRP**) is a computer-programming principle that states that every [class](https://en.wikipedia.org/wiki/Class_(computer_programming) "Class (computer programming)") in a [computer program](https://en.wikipedia.org/wiki/Computer_program "Computer program") should have responsibility over a single part of that program's [functionality](https://en.wikipedia.org/wiki/Software_feature "Software feature"), which it should [encapsulate](https://en.wikipedia.org/wiki/Encapsulation_(object-oriented_programming) "Encapsulation (object-oriented programming)"). All of that module, class or function's [services](https://en.wikipedia.org/wiki/Service_(systems_architecture) "Service (systems architecture)") should be narrowly aligned with that responsibility." [wikipedia](https://en.wikipedia.org/wiki/Single-responsibility_principle)

This section on refactoring unpacks a lot. To take notes on it, I would need to copy [the entire section](https://fullstackopen.com/en/part2/altering_data_in_server#extracting-communication-with-the-backend-into-a-separate-module).

> "This is all quite complicated and attempting to explain it may just make it harder to understand. The internet is full of material discussing the topic, such as [this](https://javascript.info/promise-chaining) one.
>"The "Async and performance" book from the [You do not know JS](https://github.com/getify/You-Dont-Know-JS/tree/1st-ed) book series explains the topic [well](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch3.md), but the explanation is many pages long."
>"Promises are central to modern JavaScript development and it is highly recommended to invest a reasonable amount of time into understanding them."

## [Cleaner syntax for defining object literals](https://fullstackopen.com/en/part2/altering_data_in_server#cleaner-syntax-for-defining-object-literals)

The `services/notes.js` module exports three methods in an object.

```js
{ 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

Because the keys and the values are the same, the object can be rewritten to:

```js
{ 
  getAll, 
  create, 
  update 
}
```

[This more concise notation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Property_definitions) was introduced in ES6.

**note:** ESLint is warning: 
>Assign object to a variable before exporting as module defaulteslint[import/no-anonymous-default-export](https://github.com/benmosher/eslint-plugin-import/blob/v2.22.1/docs/rules/no-anonymous-default-export.md)

As written, it works and allows me to name the object on import, but the linter is complaining and React is warning me in console, so I assigned the object to a variable that matches the name used to import the module.

```js
const noteService = { getAll, create, update }
export default noteService
```


## [Promises and errors](https://fullstackopen.com/en/part2/altering_data_in_server#promises-and-errors)


Remember, a *promise* will have one of three states:
- **pending** — task not completed yet
- **fulfilled** aka resolved — task completed, containing the final value
- **rejected** — task failed due to an error

Right now, the app does not handle errors at all.

>"The rejection of a promise is [handled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) by providing the _then_ method with a second callback function, which is called in the situation where the promise is rejected."

>"The more common way of adding a handler for rejected promises is to use the [catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method. "

>"In practice, the error handler for rejected promises is defined like this:"

```js
axios
  .get('http://example.com/probably_will_fail')
  .then(response => {
    console.log('success!')
  })
  .catch(error => {
    console.log('fail')
  })
```

>"If the request fails, the event handler registered with the _catch_ method gets called."

>"The _catch_ method is often utilized by placing it deeper within the promise chain."


The code for the current state of our application can be found in the _part2-6_ branch on [github](https://github.com/fullstack-hy2020/part2-notes/tree/part2-6).


### Additional notes on Promises

[MDN Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)

"The more common way of adding a handler for rejected promises is to use the [catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method."

[promise chaining](https://javascript.info/promise-chaining)


## [Adding styles to React app](https://fullstackopen.com/en/part2/adding_styles_to_react_app)

There are multiple ways to include css styles in your React app:

You can take the traditional approach (separate html, css, & javascript),  importing your css rules by importing it `import './index.css'` at the top of *index.js* file.

**Note:** In React/JSX, you need to use the camelCase [className](https://reactjs.org/docs/dom-elements.html#classname) attribute instead of the `class` attribute.

Example:
```js
    <li className='note'>
```

Or you can take the more common (in React) approach of [inline styles](https://react-cn.github.io/react/tips/inline-styles.html).

Of the multiple approaches to css styles, [inline styles](https://react-cn.github.io/react/tips/inline-styles.html) and using classes (`className`) are popular.

Inline styles:
- Define css declarations [inside of javascript objects](https://react-cn.github.io/react/tips/inline-styles.html)
- They are slower than using an external css file combined with `className`s
- "[pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) can't be used straightforwardly."
- React docs recommend using `className`, reserving inline styles for styles dynamically-computed at render time.

"Inline styles come with certain limitations. For instance, so-called [pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)can't be used straightforwardly."

>"Inline styles and some of the other ways of adding styles to React components go completely against the grain of old conventions. Traditionally, it has been considered the best practice to entirely separate CSS from the content (HTML) and functionality (JavaScript). According to this older school of thought, the goal was to write CSS, HTML, and JavaScript into their separate files."
>
>"The philosophy of React is, in fact, the polar opposite of this. Since the separation of CSS, HTML, and JavaScript into separate files did not seem to scale well in larger applications, React bases the division of the application along the lines of its logical functional entities."
>
>"The structural units that make up the application's functional entities are React components. A React component defines the HTML for structuring the content, the JavaScript functions for determining functionality, and also the component's styling; all in one place. This is to create individual components that are as independent and reusable as possible."



## Exercises

Ran across this error:

```
react-dom.development.js:13231 Uncaught Error: Objects are not valid as a React child (found: object with keys {message}). If you meant to render a collection of children, use an array instead.
```

In the component, I'd forgotten to destructure message out of the props.

```js
const Notification = (message) => {
```

Needed to be:
```js
const Notification = ({ message }) => {
```


I wanted my notifications to default to the success styling with an optional error class applied to set error styling. First I tried using a ternary operator, wrapped inside curly brackets, inside the `className="notification {error ? 'notification--error': ''}"`. 

The linter showed the error props as unused, so I shouldn't have been surprised when this approach didn't work:

```js
const Notification = ({ message, error }) => {
  return(
    <div className="notification {error ? 'notification--error': ''}">{message}</div>
  )
}
```


The solution is wrapping the entire `className={``}` statement in curly brackets and a template string with back ticks (`)

```js
const Notification = ({ message, error }) => {
  return(
    <div className={`notification ${error ? 'notification--error': ''}`}>{message}</div>
  )
}
```



When there is no notification message to display, I wanted an empty return:

```js
  if(message.length === 0) {
    return()
  }
```

I got an `unexpected token` error because the `return()`  was empty. That's because something must be returned ([conditional rendering](https://reactjs.org/docs/conditional-rendering.html)).

`return(<></>)` works but `return(null)` is better.


[Extracting communication with the backend into a separate module](https://fullstackopen.com/en/part2/altering_data_in_server#extracting-communication-with-the-backend-into-a-separate-module)