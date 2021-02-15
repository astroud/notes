# Types & Grammar

## [Appendix A: Mixed Environment JavaScript](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/types%20%26%20grammar/apA.md#appendix-a-mixed-environment-javascript)


### [Global DOM Variables](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/types%20%26%20grammar/apA.md#global-dom-variables)

>"You're probably aware that declaring a variable in the global scope (with or without `var`) creates not only a global variable, but also its mirror: a property of the same name on the `global` object (`window` in the browser)."

>"But what may be less common knowledge is that (because of legacy browser behavior) creating DOM elements with `id` attributes creates global variables of those same names. For example:"

>`<div id="foo"></div>`

### [Native Prototypes](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/types%20%26%20grammar/apA.md#native-prototypes)

"_best practice_wisdom is: **never extend native prototypes**."

[Monkey patching](https://en.wikipedia.org/wiki/Monkey_patch) native prototypes can cause problems for you, the programmers that come after you, and the programmers who's code is running alongside yours.

> "First, don't extend the natives unless you're absolutely sure your code is the only code that will ever run in that environment. If you can't say that 100%, then extending the natives is dangerous. You must weigh the risks."

```js
if (!Array.prototype.push) {
	// Netscape 4 doesn't have Array.push
	Array.prototype.push = function(item) {
		this[this.length] = item;
	};
}
```

Even if your code is the only code that is running, the code may conflict with a future ES standard.

**🤔 thought:** Seems like the best way to protect against this would be to search through all of the code for a project/environment to search for code extending the native prototypes. There must be a reason why he didn't list this as an option.


Shims/Polyfills are extensions intended to bring modern APIs (although generally not syntax) to older environments. It's not 100% safe and has downsides, but is an acceptable decision when "done with caution, defensive code, and lots of obvious documentation about the risks."

>"**Tip:** ES5-Shim ([https://github.com/es-shims/es5-shim](https://github.com/es-shims/es5-shim)) is a comprehensive collection of shims/polyfills for bringing a project up to ES5 baseline, and similarly, ES6-Shim ([https://github.com/es-shims/es6-shim](https://github.com/es-shims/es6-shim)) provides shims for new APIs added as of ES6. While APIs can be shimmed/polyfilled, new syntax generally cannot. To bridge the syntactic divide, you'll want to also use an ES6-to-ES5 transpiler like Traceur ([https://github.com/google/traceur-compiler/wiki/Getting-Started](https://github.com/google/traceur-compiler/wiki/Getting-Started))."

### [`<script>`s](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/types%20%26%20grammar/apA.md#scripts)

The multiple `<script src=..></script>` and `<script> .. </script>` elements largely act like "independent JS programs in most, but not all, respects."

>"The one thing they _share_ is the single `global` object (`window` in the browser), which means multiple files can append their code to that shared namespace and they can all interact."

>"So, if one `script` element defines a global function `foo()`, when a second `script` later runs, it can access and call `foo()` just as if it had defined the function itself."

>"But global variable scope _hoisting_ (see the _Scope & Closures_ title of this series) does not occur across these boundaries…"


### Remember

"There are four categories: 'keywords', 'future reserved words', the `null` literal, and the `true` / `false`boolean literals."

"The JavaScript spec does not place arbitrary limits on things such as the number of arguments to a function or the length of a string literal, but these limits exist nonetheless, because of implementation details in different engines."
