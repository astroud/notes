# [Rendering a collection, modules](https://fullstackopen.com/en/part2/rendering_a_collection_modules)

console.log, console.log, console.log. Experienced devs use console.log 10-100 times more.

"When something does not work, don't just guess what's wrong. Instead, log or use some other way of debugging."

VSCode supports code snippets ([instructions here](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_creating-your-own-snippets)).

There are many sources of existing snippets, including via plugins. [example](https://marketplace.visualstudio.com/items?itemName=xabikos.ReactSnippets).

VSC has a built in log snippet: `log + Tab`

## JavaScript Arrays

[Entertaining video series on functional programming](https://www.youtube.com/playlist?list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84)

note: He demos `.reject` on an array (opposite of `.filter`), but that is not part of the JS standard and was included via a library.

Functions are just values, like any other value in JavaScript

Higher order function - A function that takes another function as it's argument.

## Key-attribute
When working with lists, React uses keys to help it identify which items have changed/added/removed. React will use the array index as a key of last resort, but that can lead to bugs. Keys should always be "to the elements inside the array to give the elements a stable identity." [Lists & Keys](https://reactjs.org/docs/lists-and-keys.html)


## Refactoring
Whe the lesson example is refactored, moving the Note portion into it's own Component. The key attribute is now defined for the Note component, not the li tags.

```js
const Note = ({ note }) => { 
	return (<li>{note.content}</li>)
}

const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note =>
					<Note key={note.id} note={note} />        
				)}
			</ul>
    </div>
  )
}
```


> "A whole React application can be written in a single file…Common practice is to declare each component in their own file as an _ES6-module_."
> "In smaller applications, components are usually placed in a directory called _components_, which is in turn placed within the _src_ directory. The convention is to name the file after the component."

For each React component (in it's own file), you need to import React and [export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) the new, declared module, the variable _Note_.

```js
import React from 'react'

const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  )
}

export default Note
```


Then the file using the component (in this case *index.js*) needs to import the component:

```js
import Note from './components/Note'
```

> "Note that when importing our own components, their location must be given _in relation to the importing file_"

Including the filename extension (.js) is completely optional and can be safely ommitted.

## When the application breaks

- console.log is your friend
- Compact components declared as a single statement or without a return need to be changed to the full form, so you can console.log the props.
- "often the root of the problem is that the props are expected to be of a different type, or called with a different name than they actually are, and destructuring fails as a result. The problem often begins to solve itself when destructuring is removed and we see what the `props` actually contains."



## [Exercise] 2.1: Course information step6

After using the official Part 1 solution (package.json and src/index.js), I got the following error:

```
TypeError \[ERR\_INVALID\_ARG\_TYPE\]: The "path" argument must be of type string. Received undefined  
    at validateString (internal/validators.js:120:11)  
    at Object.join (path.js:1039:7)  
    at noopServiceWorkerMiddleware (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/react-dev-utils/noopServiceWorkerMiddleware.js:14:26)  
    at Layer.handle \[as handle\_request\] (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/layer.js:95:5)  
    at trim\_prefix (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:317:13)  
    at /Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:284:7  
    at Function.process\_params (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:335:12)  
    at next (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:275:10)  
    at launchEditorMiddleware (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/react-dev-utils/errorOverlayMiddleware.js:20:7)  
    at Layer.handle \[as handle\_request\] (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/layer.js:95:5)  
    at trim\_prefix (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:317:13)  
    at /Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:284:7  
    at Function.process\_params (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:335:12)  
    at next (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/index.js:275:10)  
    at handleWebpackInternalMiddleware (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/react-dev-utils/evalSourceMapMiddleware.js:42:7)  
    at Layer.handle \[as handle\_request\] (/Users/astroud/repos/fullstackopen/part2/courseinfo/node\_modules/express/lib/router/layer.js:95:5)
```

The error appears to be caused by the version react-scripts specified in their package.json. To fix, I upgraded my version of react-scripts [as described here](https://stackoverflow.com/a/60242323/13395366).

1.  Replaced (in *package.json*) `"react-scripts": "^3.3.0"` with `"react-scripts": "4.0.1"` (latest version)
2. Ran `npm install`


## Exercises

One of the exercises involved moving from a single course object to an array of course objects.

The solution involves passing the *courses* to the component and then where the JSX is returned, simply map over the array of courses, returning the relevant components.

```js
const Course = ({courses}) => {
  return(
    courses.map( course => {
      return(
        <div key={course.id}>
          <Header title={course.name} />
          <Content parts={course.parts} />
          <Total parts={course.parts} />
        </div>
      )
    }) 
  )
}
```












