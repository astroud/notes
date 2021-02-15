# [Chapter 1: Asynchrony: Now & Later](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#chapter-1-asynchrony-now--later)

"One of the most important and yet often misunderstood parts of programming in a language like JavaScript is how to express and manipulate program behavior spread out over a period of time."

It's about what happens when part of your program runs now, and another part of your program runs later -- there's a gap between now and later where your program isn't actively executing.

"In all these various ways, your program has to manage state across the gap in time."

"In fact, the relationship between the _now_ and _later_ parts of your program is at the heart of asynchronous programming."

"Asynchronous programming has been around since the beginning of JS, for sure. But most JS developers have never really carefully considered exactly how and why it crops up in their programs, or explored various _other_ ways to handle it. The _good enough_ approach has always been the humble callback function. Many to this day will insist that callbacks are more than sufficient."

"But as JS continues to grow in both scope and complexity, to meet the ever-widening demands of a first-class programming language that runs in browsers and servers and every conceivable device in between, the pains by which we manage asynchrony are becoming increasingly crippling, and they cry out for approaches that are both more capable and more reason-able."

"While this all may seem rather abstract right now, I assure you we'll tackle it more completely and concretely as we go on through this book. We'll explore a variety of emerging techniques for async JavaScript programming over the next several chapters."

"But before we can get there, we're going to have to understand much more deeply what asynchrony is and how it operates in JS."

## [A Program in Chunks](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#a-program-in-chunks)



> "**Warning:** You may have heard that it's possible to make synchronous Ajax requests. While that's technically true, you should never, ever do it, under any circumstances, because it locks the browser UI (buttons, menus, scrolling, etc.) and prevents any user interaction whatsoever. This is a terrible idea, and should always be avoided."
>
> "Before you protest in disagreement, no, your desire to avoid the mess of callbacks is _not_justification for blocking, synchronous Ajax."

"Any time you wrap a portion of code into a `function` and specify that it should be executed in response to some event (timer, mouse click, Ajax response, etc.), you are creating a _later_ chunk of your code, and thus introducing asynchrony to your program."

## [Async Console](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#async-console)

> "**Note:** If you run into this rare scenario, the best option is to use breakpoints in your JS debugger instead of relying on `console` output. The next best option would be to force a "snapshot" of the object in question by serializing it to a `string`, like with `JSON.stringify(..)`."

## [Event Loop](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#event-loop)



## [Parallel Threading](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#parallel-threading)
## [Run-to-Completion](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#run-to-completion)
## [Concurrency](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#concurrency)
## [Cooperation](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#cooperation)
## [Jobs](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#jobs)
## [Statement Ordering](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#statement-ordering)
## [Review](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch1.md#review)