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

