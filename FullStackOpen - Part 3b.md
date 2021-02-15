# [Deploying app to internet](https://fullstackopen.com/en/part3/deploying_app_to_internet)

## [Same origin policy and CORS](https://fullstackopen.com/en/part3/deploying_app_to_internet#same-origin-policy-and-cors)

Cross-Origin Resource Sharing is a mechanism for accessing restricted resources (fonts, AJAX/API requests). By default, cross-origin images, stylesheets, scripts, iframes, and videos can be freely embedded, but restricted resources are blocked for security and privacy purposes. "Two URLs have the _same origin_ if the [protocol](https://developer.mozilla.org/en-US/docs/Glossary/Protocol), [port](https://developer.mozilla.org/en-US/docs/Glossary/Port) (if specified), and [host](https://developer.mozilla.org/en-US/docs/Glossary/Host) are the same for both." [MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

On the backend, an express service can allow requests from other _origins_ by using Node's [cors](https://github.com/expressjs/cors) middleware.

When using the middleware, remember it's a function, so the *()* must be included or the server will hang. This: `app.use(cors())` not this: `app.use(cors)`.

Additional resources:
* [MDN: origin](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
* [MDN: same origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
* [MDN: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
* [Cross-Origin Resource Sharing @ Wikipedia](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)



## [Application to the Internet](https://fullstackopen.com/en/part3/deploying_app_to_internet#application-to-the-internet)

[Heroku's documentation for Node.js apps](https://devcenter.heroku.com/articles/getting-started-with-nodejs)

Remember to set the port to default to `process.env.PORT` for production or 3001 for development when there is no [environment variable](https://en.wikipedia.org/wiki/Environment_variable) set.

```js
const PORT = process.env.PORT || 3001
```

## [Frontend production build](https://fullstackopen.com/en/part3/deploying_app_to_internet#frontend-production-build) & [Serving static files from the backend](https://fullstackopen.com/en/part3/deploying_app_to_internet#serving-static-files-from-the-backend)

One  way (of many) to deploy the front end:

First create a [production build](https://github.com/facebookincubator/create-react-app#npm-run-build-or-yarn-build) of the application (if created with *create-react-app*):

```bash
npm run build
```

Next copy the */build* directory into the root directory of the backend.

Express uses built-in middleware called [static](http://expressjs.com/en/starter/static-files.html) to serve static html, JavaScript, etc.

```js
app.use(express.static('build'))
```

Now express knows to check the build directory for GET requests, so make sure you put `app.use(express.static('build'))` before your routes.

If your code includes server names in urls, you should update them to use relative urls because both the frontend and the backend are now at the same address.


## [Streamlining deploying of the frontend](https://fullstackopen.com/en/part3/deploying_app_to_internet#streamlining-deploying-of-the-frontend)

This deployment approach involves a lot of steps that can be simplified with scripts. Here's the example from the instructor's setup.

```json
{
  "scripts": {
    //...
    "build:ui": "rm -rf build && cd ../../osa2/materiaali/notes-new && npm run build --prod && cp -r build ../../../osa3/notes-backend/",
    "deploy": "git push heroku main",
    "deploy:full": "npm run build:ui && git add . && git commit -m uibuild && npm run deploy",    
    "logs:prod": "heroku logs --tail"
  }
}
```

>The script _npm run build:ui_ builds the frontend and copies the production version under the backend repository.  _npm run deploy_ releases the current backend to heroku. 
>
>_npm run deploy:full_ combines these two and contains the necessary _git_ commands to update the backend repository. 
>
>There is also a script _npm run logs:prod_ to show the heroku logs.
>
>Note that the directory paths in the script _build:ui_ depend on the location of repositories in the file system.

## [Proxy](https://fullstackopen.com/en/part3/deploying_app_to_internet#proxy)

Changing the *baseUrl* to a relative url will break the dev environment. If the project was created with create-react-app, the fix is as simple as including a proxy in *package.json*:

```json
{
  "dependencies": {
    // ...
  },
  "scripts": {
    // ...
  },
  "proxy": "http://localhost:3001"}
```

> After a restart, the React development environment will work as a [proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/). If the React code does an HTTP request to a server address at _[http://localhost:3000](http://localhost:3000/)_ not managed by the React application itself (i.e. when requests are not about fetching the CSS or JavaScript of the application), the request will be redirected to the server at _[http://localhost:3001](http://localhost:3001/)_.

> Current code of the backend can be found on [Github](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part3-3), in the branch _part3-3_. The changes in frontend code are in _part3-1_ branch of the [frontend repository](https://github.com/fullstack-hy2020/part2-notes/tree/part3-1).

## [Exercises 3.9.-3.11](https://fullstackopen.com/en/part3/deploying_app_to_internet#exercises-3-9-3-11)