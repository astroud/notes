If Next.js starts complaining about a `.DS_Store` file in your posts or _posts directory, the only fix I could find was creating a `.DS_Store.md` file with post metadata. Once Next.js successfully refreshed rendered the page, I was able to successfully rm the `.DS_Store.md` file without Next.js complaining.


```
./components/navigation.js
SyntaxError: /Users/astroud/Developer/aaronstroud.com/components/navigation.js: Namespace tags are not supported by default. React's JSX doesn't support namespace tags. You can set \`throwIfNamespace: false\` to bypass this warning.
```



Create CamelCase properties [solution](https://stackoverflow.com/questions/59820954/syntaxerror-unknown-namespace-tags-are-not-supported-by-default):
```
xmlns:xlink TO  xmlnsXlink
xlink:href TO xlinkHref
```



```
**Unhandled Runtime Error**

TypeError: react\_\_WEBPACK\_IMPORTED\_MODULE\_1\_\_\_default(...) is not a function

```

Pointed to:

```
const [expanded, setExpanded] = useState(false)
```

Solution was changing this:

```js
import useState from 'react'
```

to this:

```js
import { useState } from 'react'
```

Not sure why this fixed it.



During initial deploy to netlify (formerly a jekyll site), I got this error:

```
10:40:55 AM: ./styles/index.css
10:40:55 AM: TypeError: Object.entries(...).flatMap is not a function
10:40:55 AM:     at Array.forEach (<anonymous>)
10:40:55 AM: > Build error occurred
10:40:55 AM: Error: > Build failed because of webpack errors
10:40:55 AM:     at nextBuildSpan.traceAsyncFn (/opt/build/repo/node_modules/next/dist/build/index.js:17:924)
```

Issue was an old version of Node being used.
Solution: [Updating Node](https://docs.netlify.com/configure-builds/manage-dependencies/#node-js-and-javascript)

