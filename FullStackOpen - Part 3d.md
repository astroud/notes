# [Validation and ESLint](https://fullstackopen.com/en/part3/validation_and_es_lint)

[Mongoose supports a variety of built-in and custom validatior rules which you define in the schema](https://mongoosejs.com/docs/validation.html)


[Promise chaining](https://javascript.info/promise-chaining) can be used to improve readability.

For example:

```js
app.post('/api/notes', (request, response, next) => {
  // ...

  note.save()
    .then(savedNote => {
      response.json(savedNote.toJSON())
    })
    .catch(error => next(error)) 
})
```

can be split up and rewritten as:

```js
app.post('/api/notes', (request, response, next) => {
  // ...

  note
    .save()
    .then(savedNote => {
      return savedNote.toJSON()    
    })    
    .then(savedAndFormattedNote => {      
      response.json(savedAndFormattedNote)    
    })
    .catch(error => next(error)) 
})
```

To code can be made even cleaner using arrow functions:

```js
app.post('/api/notes', (request, response, next) => {
  // ...

  note
    .save()
    .then(savedNote => savedNote.toJSON())    
    .then(savedAndFormattedNote => {
      response.json(savedAndFormattedNote)
    }) 
    .catch(error => next(error)) 
})
```


## [Deploying the database backend to production](https://fullstackopen.com/en/part3/validation_and_es_lint#deploying-the-database-backend-to-production)

Use the `heroku config:set` command to define the database URL for production. example:

```bash
$ heroku config:set MONGODB_URI=mongodb+srv://fullstack:secretpasswordhere@cluster0-ostce.mongodb.net/note-app?retryWrites=true
```


## [3.19: Phonebook database, step7](https://fullstackopen.com/en/part3/validation_and_es_lint#exercises-3-19-3-21)

>  Add validation to your phonebook application, that will make sure that a newly added person has a unique name. Our current frontend won't allow users to try and create duplicates, but we can attempt to create them directly with Postman or the VS Code REST client.
>
> Mongoose does not offer a built-in validator for this purpose. Install the [mongoose-unique-validator](https://github.com/blakehaswell/mongoose-unique-validator#readme) package with npm and use it instead.
>
> If an HTTP POST request tries to add a name that is already in the phonebook, the server must respond with an appropriate status code and error message.



## [3.20*: Phonebook database, step8](https://fullstackopen.com/en/part3/validation_and_es_lint#exercises-3-19-3-21)

[On update operations, mongoose validators are off by default](https://mongoosejs.com/docs/validation.html)

When using `mongoose-unique-validator` plugin, you also need to [set the context option to `query`](https://github.com/blakehaswell/mongoose-unique-validator#find--updates) if you want to turn validators on for update operations.


## [3.21 Deploying the database backend to production](https://fullstackopen.com/en/part3/validation_and_es_lint#exercises-3-19-3-21)

Needed to revisit [3d: Deploying the database backend to production](https://fullstackopen.com/en/part3/validation_and_es_lint#deploying-the-database-backend-to-production) to fix Config Vars issue on Heroku. Frontend couldn't connect to database.

```bash
2021-02-25T21:03:56.613889+00:00 app[web.1]: error connecting to MongoDB: The `uri` parameter to `openUri()` must be a string, got "undefined". Make sure the first parameter to `mongoose.connect()` or `mongoose.createConnection()` is a string.
```


It worked after deleting MONGODB_URI in heroku app and resubmitting with the value of MONGODB_URI in apostrophes. Example:

```bash
$ heroku config:set MONGODB_URI='mongodb+srv://fullstack:secretpasswordhere@cluster0-ostce.mongodb.net/note-app?retryWrites=true'
```


## [Lint](https://fullstackopen.com/en/part3/validation_and_es_lint#lint)

"Install ESlint as a development dependency to the backend project with the command:"

```bash
npm install eslint --save-dev
```

"After this we can initialize a default ESlint configuration with the command:"

```bash
node_modules/.bin/eslint --init
```

> It is recommended to create a separate _npm script_ for linting:

```json
{
  // ...
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    // ...
    "lint": "eslint ."
  },
  // ...
}
```

> Now the _npm run lint_ command will check every file in the project.

> When you make changes to the _.eslintrc.js_ file, run the linter from the command line. This will verify that the configuration file is correctly formatted:

```bash
npm run lint
```


Make sure you create an `.eslintrc` file and include the `build` directory plus any additional files or folders you do not want linted.


Lint error: '# [ESLint - 'process' is not defined](https://stackoverflow.com/questions/50894000/eslint-process-is-not-defined)'
https://stackoverflow.com/questions/50894000/eslint-process-is-not-defined

Solution was updated .eslintrc "browser" env to "node":

```json
'env': {
  'browser': true,
```

You can find the code for our current application in its entirety in the _part3-7_ branch of [this github repository](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part3-7).


