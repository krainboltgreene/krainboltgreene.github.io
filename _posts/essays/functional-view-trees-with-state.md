---
title: "Functional View Trees with State"
---

So when it comes to doing functional programming for the web there's really only one thing that's been bothering me lately: Rendering views with state. I'll explain with the following example:

``` javascript
import {main} from "snabdom-helpers"
import {section} from "snabdom-helpers"
import {footer} from "snabdom-helpers"
import {header} from "snabdom-helpers"

function navigation ({name}) {
  return header({
    content: `Hello ${name}.`
  })
}

function frontPage () {
  return section({
    content: "Welcome to the front page"
  })
}

function information () {
  return footer({
    content: "Check me out on mastodon.social"
  })
}

function view (state) {
  return main({
    content: [
      navigation(state),
      frontPage(),
      information(),
    ]
  })
}
```

The implementation looks like this:

``` javascript
view({
  name: "Kurtis Rainbolt-Greene"
})
```

The above function when applied to a virtual DOM tree, like snabdom, should result in:


``` html
<main>
  <header>
    Hello Kurtis Rainbolt-Greene.
  </header>
  <setion>
    Welcome to the front page
  </section>
  <footer>
    Check me out on mastodon.social
  </footer>
</main>
```

Pretty easy right? The component functions that need state get state and view always gets state. Lets complicate this a little further by now making the footer have a component that needs state:

``` javascript
import {p} from "snabdom-helpers"

function authorSignature ({author: {name, email}}) {
  return p({
    content: `My name is ${name} and my email is ${email}`
  })
}
```

And now we use it:

``` javascript
function information () {
  return footer({
    content: [
      "Check me out on mastodon.social",
      authorSignature()
    ]
  })
}
```

But we have a problem. Footer didn't originally receive state and now a submodule needs state.

``` javascript
function information (state) {
  return footer({
    content: [
      "Check me out on mastodon.social",
      authorSignature(state)
    ]
  })
}
```

This is not a hard change, but we're not done:

``` javascript
function view (state) {
  return main({
    content: [
      navigation(state),
      frontPage(),
      information(state),
    ]
  })
}
```

Again not terrible, but maybe you can start to see where this is going. Lets imagine for a moment a more complex application where we have a widget that shows part of state and we place that widget component deep into one page. Now we need to pass state from `view -> ... -> widget` where `...` could be anywhere between 1 and 5 components!

Further, what happens when we move `information()` into `navigation()`? Notice that `navigation()` destructs state in the argument, but `information()` currently needs all of state.

Due to these circumstances you start writing things like:

``` javascript
function view (state) {
  return main({
    content: [
      navigation(state),
      frontPage(state),
      information(state),
    ]
  })
}
```

Passing state to every component and receiving state from every component on the off chance that you might need state lower down in the chain. React developers are probably thinking "What are you talking about? I've never had to do this!" and that's correct if you're using something like Redux via `react-redux` you get a very special function called `connect()`. This will give your component the ability to receive state as props.

I still don't know this function really works, but I have to imagine it's some sort of global accessibility, where connected components get the special access and non-connected components never care. I don't think this can be replicated with a purely functional dom tree like above.
