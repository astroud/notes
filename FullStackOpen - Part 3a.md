# [# Node.js and Express](https://fullstackopen.com/en/part3/node_js_and_express)

##  [CommonJS](https://en.wikipedia.org/wiki/CommonJS) vs [ES6 Modules](https://hacks.mozilla.org/2015/08/es6-in-depth-modules/)

Node uses [CommonJS](https://en.wikipedia.org/wiki/CommonJS) modules because Node precedes the creation of ES6:

```js
const http = require('http')
```

This line imports Node's built-in [web server](https://nodejs.org/docs/latest-v8.x/api/http.html) module. It is very simliar to the the browser-side approach with ES6 modules:

```js
import http from 'http'
```

> These days, code that runs in the browser uses ES6 modules. Modules are defined with an [export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) and taken into use with an [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import).

> CommonJS modules function almost exactly like ES6 modules, at least as far as our needs in this course are concerned.


## Express

Remember: A browser plugin like [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc) will format json files when viewed in the browser.

Node's built-in [http](https://nodejs.org/docs/latest-v8.x/api/http.html) web server can be used to build out an entire server, however there are many libraries that are pleasant to use, especially for projects that will grow beyond a few lines of code. The most popular is [express](http://expressjs.com/).

After installing `express`, you'll find express installed in `node_modules` and express' dependencies. You'll also find the transitive dependencies—"any dependency that is induced by the components that the program references directly." [wikipedia](https://en.wikipedia.org/wiki/Transitive_dependency)

[Further reading on NPM's dependency model](https://lexi-lambda.github.io/blog/2016/08/24/understanding-the-npm-dependency-model/)


> The version 4.17.1. of express was installed in [the author's] project. What does the caret in front of the version number in _package.json_ mean?

```json
"express": "^4.17.1"
```

>The versioning model used in npm is called [semantic versioning](https://docs.npmjs.com/getting-started/semantic-versioning).

>The caret in the front of _^4.17.1_ means, that if and when the dependencies of a project are updated, the version of express that is installed will be at least _4.17.1_. However, the installed version of express can also be one that has a larger _patch_ number (the last number), or a larger _minor_ number (the middle number). The major version of the library indicated by the first _major_number must be the same.

To update a project's dependencies:

```bash
npm update
```

When joining a new project, `npm install` will install all up-to-date dependencies of the project defined in _package.json_:


# [Web and express](https://fullstackopen.com/en/part3/node_js_and_express#web-and-express)

```js
const express = require('express')
const app = express()

let notes = [
  ...
]

app.get('/', (request, response) => {
  response.send('<h1>Hello World!</h1>')
})

app.get('/api/notes', (request, response) => {
  response.json(notes)
})

const PORT = 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

> The application did not change a whole lot. Right at the beginning of our code we're importing _express_, which this time is a _function_ that is used to create an express application stored in the _app_ variable.

There are two routes in the code above each with an event handler that takes two parameters:

> The first [request](http://expressjs.com/en/4x/api.html#req) parameter contains all of the information of the HTTP request, and the second [response](http://expressjs.com/en/4x/api.html#res) parameter is used to define how the request is responded to.

Unlike node's built-in `http` server, express has a `response` object with many useful methods.

Passing a string to the `send` method, automatically results in the correct `Content-Type` in the header. *text/html* for the root '/' route.

Similarly, *response*'s `json` method automatically converts the JSON object into a string and sets the content type to *application/json*.


## [nodemon](https://fullstackopen.com/en/part3/node_js_and_express#nodemon)

Install [nodemon](https://github.com/remy/nodemon) as a *development dependency* to automatically restart the server every time a file is updated.

```bash
npm install --save-dev nodemon
```


When you forget to install a package as a developer dependency, move the package from "dependencies" to "devDependencies":

```json
  "devDependencies": {
    "rightSpot": "^2.0.7"
  }
```

You can start `nodemon`:

```bash
node_modules/.bin/nodemon index.js
```

But it's more convenient to add another npm script in `package.json`:

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js",
  "test": "echo \"Error: no test specified\" && exit 1"
},
```

Nodemon will now restart the server every time you update a file, but nodemon does not provide [hot reload](https://gaearon.github.io/react-hot-loader/getstarted/) functionality, so you'll still need to refresh your browser manually.


## Section Links With Minimal notes
A quick overview of [REST](https://fullstackopen.com/en/part3/node_js_and_express#rest)

[Fetching a single resource](https://fullstackopen.com/en/part3/node_js_and_express#fetching-a-single-resource): Remember that params values will be strings, so you'll need to change `request.params.id` into a number to look up objects by their id: `Number(request.params.id)`

### Handling an id that does not exist

```js
  const note = notes.find(note => note.id === id)
  
  if (note) {
    response.json(note)
  } else {
    response.status(404).end()
  }
})
```

> Since no data is attached to the response, we use the [status](http://expressjs.com/en/4x/api.html#res.status) method for setting the status, and the [end](http://expressjs.com/en/4x/api.html#res.end) method for responding to the request without sending any data.

> The if-condition leverages the fact that all JavaScript objects are [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy), meaning that they evaluate to true in a comparison operation. However, _undefined_ is [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) meaning that it will evaluate to false.

### Deletion route

To delete, send a DELETE request to the url of the resource:

```js
app.delete('/api/notes/:id', (request, response) => {
  const id = Number(request.params.id)
  notes = notes.filter(note => note.id !== id)

  response.status(204).end()
})
```

> If deleting the resource is successful, meaning that the note exists and it is removed, we respond to the request with the status code [204 no content](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.5) and return no data with the response.

> There's no consensus on what status code should be returned to a DELETE request if the resource does not exist. Really, the only two options are 204 and 404. For the sake of simplicity our application will respond with 204 in both cases.

> Many tools exist for making the testing of backends easier. One of these is a command line program [curl](https://curl.haxx.se/). However, instead of curl, we will take a look at using [Postman](https://www.getpostman.com/) for testing the application…If you use Visual Studio Code, you can use the VS Code [REST client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) plugin instead of Postman.

The VSC Rest Client is very handy and [simple to use](https://fullstackopen.com/en/part3/node_js_and_express#the-visual-studio-code-rest-client).

> One benefit that the REST client has over Postman is that the requests are handily available at the root of the project repository, and they can be distributed to everyone in the development team. You can also add multiple requests in the same file using `###`separators.
> 
> **Important sidenote**
> 
> Sometimes when you're debugging, you may want to find out what headers have been set in the HTTP request. One way of accomplishing this is through the [get](http://expressjs.com/en/4x/api.html#req.get)method of the _request_ object, that can be used for getting the value of a single header. The _request_ object also has the _headers_ property, that contains all of the headers of a specific request.
>
> Problems can occur with the VS REST client if you accidentally add an empty line between the top row and the row specifying the HTTP headers. In this situation, the REST client interprets this to mean that all headers are left empty, which leads to the backend server not knowing that the data it has received is in the JSON format.

### [Receiving Data](https://fullstackopen.com/en/part3/node_js_and_express#receiving-data)

Useful for debugging:

```js
console.log(request.headers)
```


```js
app.post('/api/notes', (request, response) => {
  const maxId = notes.length > 0
    ? Math.max(...notes.map(n => n.id)) 
    : 0

  const note = request.body
  note.id = maxId + 1

  notes = notes.concat(note)

  response.json(note)
})
```

**note:** The method above used to create a unique id "is not recommended", but will be refactored later in the course.


```js
if (!body.content) {
  return response.status(400).json({ 
    error: 'content missing' 
  })
}
```

When checking for required properties, it's important to `return` the status error, otherwise the code will continue executing to the end (eventually saving the incomplete data).

> You can find the code for our current application in its entirety in the _part3-1_ branch of [this github repository](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part3-1).
> Notice that the master branch of the repository contains the code from a later version of the application. The code for the current state of the application is specifically in branch [part3-1](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part3-1).

## [Exercises 3.1-3.6](https://fullstackopen.com/en/part3/node_js_and_express#exercises-3-1-3-6)

You can access `:id`  in the `request.params`. Also, remember to convert the `request.params.id` string into a number.

```js
app.get('/persons/:id', (req, res) => {
  const id = Number(request.params.id)
})
```


To send an error [status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes):

```js
if (entry.length === 0) {
    return res.status(404).json({ 
      error: 'person not found' 
    })
  }
```



When working with the response's `body`, remember to use [express' json middleware](http://expressjs.com/en/api.html#express.json):

```js
app.use(express.json())
```

Otherwise the *body* property will be *undefined*. 

> The json-parser functions so that it takes the JSON data of a request, transforms it into a JavaScript object and then attaches it to the _body_ property of the _request_ object before the route handler is called.



## [About HTTP request types](https://fullstackopen.com/en/part3/node_js_and_express#about-http-request-types)

[The HTTP standard](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

## [Middleware](https://fullstackopen.com/en/part3/node_js_and_express#middleware)

Middleware are functions express uses to handle request and response objects.

* You often use several middleware at the same time
* They're executed in the order their taken into use by express
* At the end of each middleware the `next()` function must be called to yield control to the next middleware
* If routes depend on work done by middleware, remember to call the middleware first
* You might want to use middleware after the routes, perhaps to handle situations where no route exists