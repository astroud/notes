# [Converting HTML+CSS projects to React](https://www.frontendmentor.io/profile/astroud)

Created [template-react-refactor](https://github.com/astroud/template-react-refactor) repo for rapidly refactoring older HTML+CSS projects to React + Styled Components.

References:

[Styled Components](https://styled-components.com)
- For css reset and background: [createGlobalStyle](https://styled-components.com/docs/api#createglobalstyle)
- [Adapting based on props](https://styled-components.com/docs/basics#adapting-based-on-props)
  - Remember to pass the props to the *styled component, not just the parent component*


React
- [Using the Public Folder](https://create-react-app.dev/docs/using-the-public-folder/)
- [Adding Images, Fonts, and Files](https://create-react-app.dev/docs/adding-images-fonts-and-files/)
- [How to destructure a React component props object](https://linguinecode.com/post/destructure-react-component-prop-object)


PropTypes
- [How to specify the shape of an object with PropTypes](https://dev.to/cesareferrari/how-to-specify-the-shape-of-an-object-with-proptypes-3c56)
  
 
z-index
- When positioning text over an image with `position: relative`. Setting a width and a background will extend over the image, but the text will break at the image's border. You need to use `top`, `bottom`, `left`, and `right` to [shift the text's position over the image](https://developer.mozilla.org/en-US/docs/Web/CSS/position#types_of_positioning).


Animation
- [React Animations Tutorial using React Transition Group (video)](https://www.youtube.com/watch?v=BZRyIOrWfHU)
- [Animation Add-Ons](https://reactjs.org/docs/animation.html)
- [React: How to Design Smooth Page Transitions and Animations](https://dev.to/admantium/react-how-to-design-smooth-page-transitions-and-animations-1fii)
- [React Transition Group](http://reactcommunity.org/react-transition-group/)
- [react-motion](https://github.com/chenglou/react-motion)
- [React Spring](https://aleclarson.github.io/react-spring/v9/)


## [[frontendmentor-coding-bootcamp-testimonials-slider](https://github.com/astroud/frontendmentor-coding-bootcamp-testimonials-slider)](https://github.com/astroud/frontendmentor-coding-bootcamp-testimonials-slider)

Console warning when transitioning between slides:

```
Warning: findDOMNode is deprecated in StrictMode. findDOMNode was passed an instance of Transition which is inside StrictMode. Instead, add a ref directly to the element you want to reference. Learn more about using refs safely here: https://reactjs.org/link/strict-mode-find-node
```

[Forwarding refs to DOM components](https://reactjs.org/link/strict-mode-find-node)

[React Transition Group](http://reactcommunity.org/react-transition-group/) [is causing the warning](https://www.kindacode.com/article/react-warning-finddomnode-is-deprecated-in-strictmode/).
