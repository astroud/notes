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

