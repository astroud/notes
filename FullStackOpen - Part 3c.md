# [Saving data to MongoDB](https://fullstackopen.com/en/part3/saving_data_to_mongo_db)

[7 Database Paradigms | Fireship](https://www.youtube.com/watch?v=W2Z7fbCLSTw) (9.5m video)

## [Debugging Node applications](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#debugging-node-applications)

Debugging options:

* [Visual Studio Code](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#debugging-node-applications)'s [Debugging documentation](https://code.visualstudio.com/docs/editor/debugging).
* [Chrome dev tools](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#debugging-node-applications)
	* Debugging is also possible with the Chrome developer console by starting your application with the command: `node --inspect index.js`

When Debugging—Question Everything:
* When something "doesn't work", you need to first figure out where the problem actually occurs
* Often the problem is not where you'd expect it to be
* Be systematic. Question everything and eliminate possibilities one by one.
* "Logging to the console, Postman, debuggers, and experience will help."
* When bugs occur—"the worst of all possible strategies is to continue" coding


## [MongoDB](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#mongo-db)

Setting up a database with [MongoDB Atlas](https://www.mongodb.com/cloud/atlas):

1. Create a Project
2. Create a cluster and select the free tier
	1. Wait for cluster to deploy
3. Use the _database access_ tab for creating user credentials for the database (credentials are for the app)
	1. grant the user with permissions to read and write to the databases.
4. Define the IP addresses that are allowed access to the database
	1. for simplicity's sake (aka less secure), set to access from anywhere
5. Under Clusters, click *Connect* to connect to our database
	1. choose _Connect your application_
	2. The view displays the _MongoDB URI_, which is the address of the database that we will supply to the MongoDB client library we will add to our application.
	3. Use the URI MongoDB provides. Example: `mongodb+srv://<username>:<password>@cluster0.wrp9g.mongodb.net/<dbname>?retryWrites=true&w=majority`
	4. Replace **<username>** and **<password>** with the credentials you created for your app. Replace **<dbname>** with the name of the database that connections will use by default. Ensure any option params are [URL encoded](https://dochub.mongodb.org/core/atlas-url-encoding).


>We could use the database directly from our JavaScript code with the [official MongoDb Node.js driver](https://mongodb.github.io/node-mongodb-native/) library, but it is quite cumbersome to use. We will instead use the [Mongoose](http://mongoosejs.com/index.html)library that offers a higher level API.
>
>Mongoose could be described as an _object document mapper_ (ODM), and saving JavaScript objects as Mongo documents is straightforward with this library.

## [Schema](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#schema)

A *mongoose* *model* is created with a *Schema*. You can then create JavaScript objects from that model.
	
>Models are so-called _constructor functions_ that create new JavaScript objects based on the provided parameters. Since the objects are created with the model's constructor function, they have all the properties of the model, which include methods for saving the object to the database.

## [Creating and saving objects](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#creating-and-saving-objects)

## [Fetching objects from the database ](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#fetching-objects-from-the-database)

## [Exercise 3.12](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#exercise-3-12)

> You can get the command-line parameters from the [process.argv](https://nodejs.org/docs/latest-v8.x/api/process.html#process_process_argv) variable.  


## [Backend connected to a database](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#backend-connected-to-a-database)

Note: If you forgot to start the server with `npm run dev` to use nodemon to watch for changes, you'll need to restart the server after [modifying](https://stackoverflow.com/questions/7034848/mongodb-output-id-instead-of-id) the [`toJSON` method](https://mongoosejs.com/docs/api.html#document_Document-toJSON).

```js
noteSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id.toString()
    delete returnedObject._id
    delete returnedObject.__v
  }
})
```

> Even though the *_id* property of Mongoose objects looks like a string, it is in fact an object. The _toJSON_ method we defined transforms it into a string just to be safe. If we didn't make this change, it would cause more harm for us in the future once we start writing tests.
  

## [Database configuration into its own module](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#database-configuration-into-its-own-module)

> There are many ways to define the value of an environment variable. One way would be to define it when the application is started:
>
> ```bash
> MONGODB_URI=address_here npm run dev
> ```
>
> A more sophisticated way is to use the [dotenv](https://github.com/motdotla/dotenv#readme) library. You can install the library with the command:
>
> ```bash
> npm install dotenv
> ```
>
> To use the library, we create a _.env_ file at the root of the project. The environment variables are defined inside of the file, and it can look like this:
>
> ```bash
> MONGODB_URI='mongodb+srv://fullstack:sekred@cluster0-ostce.mongodb.net/note-app?retryWrites=true'
> PORT=3001
> ```
>
> We also added the hardcoded port of the server into the _PORT_ environment variable.
>
> 🧨 **The _.env_ file should be gitignored right away, since we do not want to publish any confidential information publicly online!**
>
> The environment variables defined in the _.env_ file can be taken into use with the expression _require('dotenv').config()_ and you can reference them in your code just like you would reference normal environment variables, with the familiar _process.env.MONGODB\_URI_ syntax.
  > It's important that _dotenv_ gets imported before the _note_ model is imported. This ensures that the environment variables from the _.env_ file are available globally before the code from the other modules is imported.
  
😖 Pain Point. After moving my database config into it's own file (*models/note.js*), route *api/notes* began returning an empty array. When I remembered I copied the mongoDB URI from my records, that's when I realized I forgot to add the database name to the URI. After adding the name and restarting, I was good to go.
  
## [Using database in route handlers](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#using-database-in-route-handlers)

😖 Pain Point. Remember that "the note objects are created with the _Note_ constructor function", so the `const note` variable must be created  as a `new Note`, otherwise it won't have the methods and database access mongoose provides.
  
## [Verifying frontend and backend integration](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#verifying-frontend-and-backend-integration)

Remember: REST API requests do not need updating. They're handled by the frontend which now uses' the mongoDB for dev and production (same database too at this stage).
  
> You can find the code for our current application in its entirety in the _part3-4_ branch of [this Github repository](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part3-4).
  
## [Exercises 3.13.-3.14](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#exercises-3-13-3-14)

[`Model.countDocuments()`](https://mongoosejs.com/docs/api.html#model_Model.countDocuments) Counts the exact number of documents matching `filter` in a database collection.
  
If you want to count all documents in a large collection, use the [`estimatedDocumentCount()` function](https://mongoosejs.com/docs/api.html#model_Model.estimatedDocumentCount) instead. If you call `countDocuments({})`, MongoDB will always execute a full collection scan and **not** use any indexes.
  
[The **HTTP `DELETE` request method**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE)
  
[`Model.deleteOne`](https://mongoosejs.com/docs/api.html#model_Model.deleteOne)
  
## [Error handling](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#error-handling)
  
> When dealing with Promises, it's almost always a good idea to add error and exception handling, because otherwise you will find yourself dealing with strange bugs.
>
> It's never a bad idea to print the object that caused the exception to the console in the error handler:
  
```js
.catch(error => {
  console.log(error)
  response.status(400).send({ error: 'malformatted id' })
})
```
  

## [Moving error handling into middleware](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#moving-error-handling-into-middleware)

Sometimes it's acceptable to handle errors in one's routing code, but often moving error handling code into middleware makes more sense. Especially if you plan to use an external error tracking system like [Sentry](https://sentry.io/welcome/).
  
> Express [error handlers](https://expressjs.com/en/guide/error-handling.html) are middleware that are defined with a function that accepts _four parameters_. Our error handler looks like this:
  
```js
const errorHandler = (error, request, response, next) => {
  console.error(error.message)

  if (error.name === 'CastError') {
    return response.status(400).send({ error: 'malformatted id' })
  } 

  next(error)
}

app.use(errorHandler)
```
  
## [The order of middleware loading](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#the-order-of-middleware-loading)
  
The order of middleware loading is very important. Generally, you'd handle errors last and second to last would be your unsupported routes handler.

  
## [Other operations](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#other-operations)
  
[findByIdAndRemove](https://mongoosejs.com/docs/api.html#model_Model.findByIdAndRemove) is the easiest way to delete one document in the database.

Documents (notes, phone entries, etc) can be updated with the [findByIdAndUpdate](https://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate) method and a HTTP `PUT` request.
  
> The **HTTP `PUT` request method** creates a new resource or replaces a representation of the target resource with the request payload. [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT)

  
To toggle the importance of a note or update the note's content:
  
```js
app.put('/api/notes/:id', (request, response, next) => {
  const body = request.body

  const note = {
    content: body.content,
    important: body.important,
  }

  Note.findByIdAndUpdate(request.params.id, note, { new: true })
    .then(updatedNote => {
      response.json(updatedNote)
    })
    .catch(error => next(error))
})
```
  
**Note: **The _findByIdAndUpdate_ method receives a regular JavaScript object as its parameter—not a new note object created with the _Note_ constructor function.
 
**Note: **The [_findByIdAndUpdate_](https://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate) method by default returns the original document ***before updating.*** If you want the document's new state returned (after updating), you must pass  the optional `{ new: true }`.

> You can find the code for our current application in its entirety in the _part3-5_ branch of [this github repository](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part3-5).
  
  
**miscellenous note:** [res.end()](http://expressjs.com/en/5x/api.html#res.end)
> Ends the response process. This method actually comes from Node core, specifically the [response.end() method of http.ServerResponse](https://nodejs.org/api/http.html#http_response_end_data_encoding_callback).
>
>Use to quickly end the response without any data. If you need to respond with data, instead use methods such as [res.send()](http://expressjs.com/en/5x/api.html#res.send)and [res.json()](http://expressjs.com/en/5x/api.html#res.json).
  
> ```javascript
> res.end()
> res.status(404).end()
> ```
  
## [Exercises 3.15.-3.18](https://fullstackopen.com/en/part3/saving_data_to_mongo_db#exercises-3-15-3-18)
  
