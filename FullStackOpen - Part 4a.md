# [Structure of backend application, introduction to testing](https://fullstackopen.com/en/part4/structure_of_backend_application_introduction_to_testing)

> [**express.Router**](https://expressjs.com/en/guide/routing.html)
>
> Use the `express.Router` class to create modular, mountable route handlers. A `Router` instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.
> 
> The following example creates a router as a module, loads a middleware function in it, defines some routes, and mounts the router module on a path in the main app.


When you load a router module in your app, you define the root of the route `app.use('/api/notes'` and the notesRouter handles the "related", relative parts of the part. Example: `app.delete('/api/notes/:id'`

> The _app.js_ file that creates the actual application, takes the router into use as shown below:

```js
const notesRouter = require('./controllers/notes')
app.use('/api/notes', notesRouter)
```

> The router we defined earlier is used _if_ the URL of the request starts with _/api/notes_. For this reason, the notesRouter object must only define the relative parts of the routes, i.e. the empty path _/_ or just the parameter _/:id_.


After refactoring, the express app's MVC directory structure looks like this:

```bash
├── index.js
├── app.js
├── build
│   └── ...
├── controllers
│   └── notes.js
├── models
│   └── note.js
├── package-lock.json
├── package.json
├── utils
│   ├── config.js
│   ├── logger.js
│   └── middleware.js  
```


You can find the code for our current application in its entirety in the _part4-1_ branch of [this Github repository](https://github.com/fullstack-hy/part3-notes-backend/tree/part4-1).


> **Best practice:** Commit your code every time it is in a stable state. This makes it easy to rollback to a situation where the application still works.


## Unit Tests

Install Jest as a dev dependency:

```bash
npm install --save-dev jest
```

You can update the test npm script to use a *verbose* style:

```bash
{
  //...
    "lint": "eslint .",
    "test": "jest --verbose"
  },
  //...
}
```

Jest requires you to define it's environment, either adding the following to the end of _package.json_:

```js
{
 //...
 "jest": {
   "testEnvironment": "node"
 }
}
```

Or you can specify the test environment in *jest.config.js*:

```js
module.exports = {
  testEnvironment: 'node',
};
```


If ESLint complains about  the _test_ and _expect_ commands, update *.eslintrc* with `'jest': true,` like so:

```js
module.exports = {
  'env': {
    'commonjs': true,
    'es2021': true,
    'node': true,
    'jest': true,
    },
    // ...
```


You can group tests (and testing output) with `describe` like this:

```js
describe('GROUPING_NAME', () => {
  test('palindrome of a', () => {
    const result = palindrome('a')

    expect(result).toBe('a')
  })
  //...
})
```


## Exercise 4.6
[Getting key with the highest value from object](https://stackoverflow.com/questions/27376295/getting-key-with-the-highest-value-from-object)

### ESLint error
[How to handle eslint no-param-reassign rule in Array.prototype.reduce() functions](https://stackoverflow.com/questions/41625399/how-to-handle-eslint-no-param-reassign-rule-in-array-prototype-reduce-function)
[Discussion about the rule](https://github.com/airbnb/javascript/issues/719)

