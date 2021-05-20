# [Frontend Mentor - REST Countries API](https://www.frontendmentor.io/challenges/rest-countries-api-with-color-theme-switcher-5cacc469fec04111f7b848ca)

[Styled Components](https://styled-components.com)
For css syntax highlighting: [vscode-styled-components](https://marketplace.visualstudio.com/items?itemName=jpoissonnier.vscode-styled-components) by [Julien Poissonnier](https://marketplace.visualstudio.com/publishers/jpoissonnier)


Articles Used:
- [The state of CSS, CSS in JS & how styled-components is solving the problems we’ve had for decades](https://medium.com/@vviikk/the-state-of-css-css-in-js-how-styled-components-is-solving-the-problems-weve-had-for-decades-d8abbc8bc148)
- [Styled Components: The Essentials Explained in 3 Steps](https://www.freecodecamp.org/news/styled-components-essentials-in-three-steps/)
- [Implementing Dark Mode In React Apps Using styled-components](https://www.smashingmagazine.com/2020/04/dark-mode-react-apps-styled-components/)
- [The styled-components Happy Path](https://www.joshwcomeau.com/css/styled-components/)
- [CSS Variables for React Devs](https://www.joshwcomeau.com/css/css-variables-for-react-devs/)
- [How to use SVGs in React](https://blog.logrocket.com/how-to-use-svgs-in-react/)
- [Axiomatic CSS and Lobotomized Owls](https://alistapart.com/article/axiomatic-css-and-lobotomized-owls/)
- [Watch Out for Undefined State](https://daveceddia.com/watch-out-for-undefined-state/)


<hr>

Eslint errors:
In useDarkMode.js, I export like this:

```javascript
export default useDarkMode()
```

And get this error in *App.js*:

> useDarkMode not found in './components/useDarkMode'

However, ignoring the rule as exporting like this works:

```javascript
// eslint-disable-next-line import/prefer-default-export
export const useDarkMode = () => {
```

<hr>

Eslint/airbnb also does not like calling `setMode()` using a ternary
```javascript
// eslint-disable-next-line no-unused-expressions
theme === 'light' ? setMode('dark') : setMode('light')
```

<hr>

> error  Form label must have associated control  jsx-a11y/label-has-for

```javascript
<input type="checkbox" id="theme" onChange={themeToggler} checked={theme === 'light'} />
    <label htmlFor="theme">label</label>
```

[Solution](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/issues/302):  The `<input>` must be *inside* the `<label>`

<hr>

To add *onClick* to my `<CountryCard>`s, the *onClick* must go in the CountryCard.jsx not where `<CountryCard>` is used in the `<FilterableCountryList>`.

<hr>

[Understanding How To Render Arrays in React](https://www.digitalocean.com/community/conceptual_articles/understanding-how-to-render-arrays-in-react)

[React - Cannot read property 'map' of undefined](https://www.debuggr.io/react-map-of-undefined/)

TypeError: countryArray.map is not a function

```console
is my countryArray no longer an array? 
{border: Array(3)}
border: (3) ["alligator", "snake", "lizard"]
__proto__: Object
```

The problem was: `const CountryLink = ( countryArray ) => {`

I'd forgotten to destructure `countryArray` from props.

Fix: `const CountryLink = ({ countryArray }) => {`

<hr>

To get object-fit to work, the `<img>`'s height and width needs to be defined. It's not sufficient to define the parent `<div>`'s dimensions.

```
export const FlagWrapper = styled.div`
  width: 100%;
  height: 10rem;
  max-height: 10rem;
  /* overflow: hidden; */

  img {
    width: 100%;
    height: 10rem;
    object-fit: cover;
  }  
`
```

<hr>

[To add commas to population numbers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString)

<hr>
## Implementing the dropdown filtering

[ReactJS: Dropdown menus](https://blog.campvanilla.com/reactjs-dropdown-menus-b6e06ae3a8fe)

[8 awesome features of styled-components - LogRocket Blog](https://blog.logrocket.com/8-awesome-features-of-styled-components/)

[HTML Inputs and Labels: A Love Story](https://css-tricks.com/html-inputs-and-labels-a-love-story/)

✪ [ReactJS + styled components + pseudoClasses - Stack Overflow](https://stackoverflow.com/questions/44646621/reactjs-styled-components-pseudoclasses)

[Building a Dropdown Menu Component With React Hooks](https://letsbuildui.dev/articles/building-a-dropdown-menu-component-with-react-hooks)

<hr>
Leave `useEffect`'s second parameter empty if you want it to only run once (like onload when pinging the countries api)

<hr>
## Fixing Darkmode Flash
[The Quest for the Perfect Dark Mode](https://www.joshwcomeau.com/react/dark-mode/)

<hr>
## Adding a Loader

[How to Build a Skeleton Loading Placeholder](https://letsbuildui.dev/articles/how-to-build-a-skeleton-loading-placeholder)

<hr>

## Darkmode Toggle Sfx
[Announcing “use-sound”, a React Hook for Sound Effects — A React Hook for Sound Effects](https://www.joshwcomeau.com/react/announcing-use-sound-react-hook/)

## a11y improvements
[Why you should use focus styles - LogRocket Blog](https://blog.logrocket.com/why-you-should-use-focus-styles-193d58672c5c/)

useful? https://allegra9.medium.com/add-focus-visible-to-your-react-application-7680994779e5