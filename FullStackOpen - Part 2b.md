# [Forms](https://fullstackopen.com/en/part2/forms)

"You can find the code for our current [note] application in its entirety in the _part2-2_ branch of [this GitHub repository](https://github.com/fullstack-hy2020/part2-notes/tree/part2-2)."

"You can find the code for our current application in its entirety in the _part2-3_ branch of [this GitHub repository](https://github.com/fullstack-hy2020/part2-notes/tree/part2-3)."

"There are many ways to accomplish this; the first method we will take a look at is through the use of so-called [controlled components](https://reactjs.org/docs/forms.html#controlled-components)."

"The new note is added to the list of notes using the [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) array method, introduced in [part 1](https://fullstackopen.com/en/part1/java_script#arrays):"

```js
setNotes(notes.concat(noteObject))
```

"The method does not mutate the original _notes_ array, but rather creates _a new copy of the array with the new item added to the end_. This is important since we must [never mutate state directly](https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly) in React!"

## Exercises
- Got a type error because I forgot to destructure my props
- You need to `event.preventDefault()` on the `<form>`, not the `<button type="submit">`
- The people array is an array of objects, so you need to `setPeople(people.concat({name: newName}))`

For the filter field, the field would lose focus after typing each character. Adding this code to the input field fixed it:

`ref={(input) => {input && input.focus() }}`

[source](https://www.geeksforgeeks.org/how-to-set-focus-on-an-input-field-after-rendering-in-reactjs/)

However, that code was no longer needed when I started properly using State to control the field.