# [Getting data from server](https://fullstackopen.com/en/part2/getting_data_from_server)

## [The browser as a runtime environment](https://fullstackopen.com/en/part2/getting_data_from_server#the-browser-as-a-runtime-environment)

During development, [JSON Server](https://github.com/typicode/json-server) can act as a server (for our JSON file) or to simulate APIs.

After installing globally, start with the following command. Json-server will watch for changes in the db.json created to store notes and we specify port 3001 because React and json-server both default to server 3000.

````js
json-server --port 3001 --watch db.json
````

If you do not have the permissions to install globally, you can run with npx:

```js
npx json-server --port 3001 --watch db.json
```

A browser plugin like [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc) will format json files when viewed in the browser.


> **Do not use XHR code sample below, instead use [fetch](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) (code copied from Part 0)**:
> "The use of XHR is no longer recommended, and browsers already widely support the [fetch](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) method, which is based on so-called [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), instead of the event-driven model used by XHR."

> "On the other hand, JavaScript engines, or runtime environments, follow the [asynchronous model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop). In principle, this requires all [IO-operations](https://en.wikipedia.org/wiki/Input/output) (with some exceptions) to be executed as non-blocking. This means that the code execution continues immediately after calling an IO function, without waiting for it to return."
> "Currently, JavaScript engines are _single-threaded_, which means that they cannot execute code in parallel. As a result, it is a requirement in practice to use a non-blocking model for executing IO operations. Otherwise, the browser would "freeze" during, for instance, the fetching of data from a server."
> "There is a host of additional material on the subject to be found on the internet. One particularly clear presentation of the topic is the keynote by Philip Roberts called [What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
> "In today's browsers, it is possible to run parallelized code with the help of so-called [web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers). The event loop of an individual browser window is, however, still only handled by a [single thread](https://medium.com/techtrument/multithreading-javascript-46156179cf9a)."


## npm

[Fetch](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) is a great tool for pulling data servers. It is standardized and supported by all modern browsers (excluding IE).

But Full Stack Open's instructors have chosen to use the [axios](https://github.com/axios/axios) library instead for communication between the browser and server. It functions like fetch, but they feel it "is somewhat more pleasant to use." It also gives us practice adding npm packages (external libraries) to React projects.

>"Nowadays, practically all JavaScript projects are defined using the node package manager, aka [npm](https://docs.npmjs.com/getting-started/what-is-npm). The projects created using create-react-app also follow the npm format. A clear indicator that a project uses npm is the _package.json_ file located at the root of the project."

**"_npm_ commands should always be run in the project root directory**, which is where the _package.json_ file can be found."

```
npm install axios
```

When the above npm command is run, it automatically updates the package.json's dependencies with *axios*.

Json-server can be recorded as a development dependency like so:

```js
npm install json-server --save-dev
```

And we can update the package.json's scripts with a shortcut to start the json-server:

```json
{
  // ... 
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "server": "json-server -p3001 --watch db.json"
}
```

The server can now be run: `npm run server`


## Axios and promises

"A Promise is an object representing the eventual completion or failure of an asynchronous operation." [MDN Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)

A *promise* will have one of three states:
**pending** — task not completed yet
**fulfilled** aka resolved — task completed, containing the final value
**rejected** — task failed due to an error

In order to use the result of the operation (the promise), an event handler is created with `.then` like so:

```js
const promise = axios.get('http://localhost:3001/notes')

promise.then(response => {
  console.log(response)
})
```

Typically, promises are not stored in variables, but instead are *chained* together like:

```js
axios
  .get('http://localhost:3001/notes')
  .then(response => {
    const notes = response.data
    console.log(notes)
  })
```


## Effect-hooks

need note on only running it once with an empty array


## The development runtime environment
## [Exercises 2.11.-2.14.](https://fullstackopen.com/en/part2/getting_data_from_server#exercises-2-11-2-14)



Adding the *axios* and *json-server* packages results in a new `package-lock.json`. This file should be committed because it describes the exact tree of modules used to create the app. This is essential for teammates, deployments, continuous integration.


### Countries app
I'm still not completely sure why I encountered this error:

```
Warning: Cannot update a component from inside the function body of a different component.
```

[documentation](https://reactjs.org/blog/2020/02/26/react-v16.13.0.html#new-warnings)


[Weatherstack.com](weatherstack.com) offers a free tier to their weather API.

Updating country with weather:

> I asked on Discord: For displaying weather in exercise 2.14, should I track weather in state so React will re-render when the weather data arrives? Or should I delay rendering until the data arrives? I tried moving the return into Axios' .then() chain but React couldn't find the return statement:
> `Error: DisplayCountry(...): Nothing was returned from render. This usually means a return statement is missing. Or, to render nothing, return null.`

> Workmad3 Responded
> @Aaron Stroud you can't delay the render like that with react... you need to rerender after the data has been fetched, and the easiest way to do that is to trigger the fetch in a `useEffect` hook and store the result in a state hook

[docs: useEffect](https://reactjs.org/docs/hooks-reference.html#useeffect)

After moving weather into state and using useEffect, everything worked. But react and ESLint now warn:

```
React Hook useEffect has missing dependencies: React Hook useEffect has missing dependencies: 'API_KEY', 'country.capital', 'country.name', 'setWeather', and 'weather'. Either include them or remove the dependency array. If 'setWeather' changes too often, find the parent component that defines it and wrap that definition in useCallback  react-hooks/exhaustive-deps
```


But when I add the following dependencies to my dependency array, useEffect goes into an infinite loop. In my situation, the API_KEY should never change and the country object would only change when displaying a new country, so I'm not sure the warning is relevant.

As a general rule, it makes sense though. If you don't list the functions/variables/state/props that useEffect relies on, it won't know when it needs to update/reinvoke itself.

This is the relevant code from DisplayCountry.js:

```js
const DisplayCountry = ({ country, weather, setWeather }) => {
  const API_KEY = process.env.REACT_APP_API_KEY_WEATHER

  useEffect( () => {
    axios
      .get(`http://api.weatherstack.com/current?access_key=${API_KEY}&query=${country.name},${country.capital}`)
      .then(response => {
        setWeather(response.data)
        console.log(weather);
      })
      // .then(console.log(weather))
  }, [])
```

I also needed to handle the initial render when weather was `undefined`. [This article](https://dmitripavlutin.com/7-tips-to-handle-undefined-in-javascript/) was helpful in understanding the situation and multiple ways to check if an object has a specific property:

>-   `obj.prop !== undefined`: compare against `undefined` directly
>-   `typeof obj.prop !== 'undefined'`: verify the property value type
>-   `obj.hasOwnProperty('prop')`: verify whether the object has an own property
>-   `'prop' in obj`: verify whether the object has an own or inherited property

I went with: `if('current' in weather)`
