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

