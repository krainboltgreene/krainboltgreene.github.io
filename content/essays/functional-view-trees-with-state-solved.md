---
title: "Functional View Trees with State (Solved)"
draft: false
---

[In a previous blog post I lamented about having difficulty figuring out how to handle view trees where some nodes needed complete access to state](https://kurtis.rainbolt-greene.online/functional-view-trees-with-state.html). In a spurt of brain power I figured out a rather eloquent solution that doesn't involve global stores! So again, consider this last example:

``` javascript
import {main} from "snabdom-helpers"
import {section} from "snabdom-helpers"
import {footer} from "snabdom-helpers"
import {header} from "snabdom-helpers"
import {p} from "snabdom-helpers"

function authorSignature ({author: {name, email}}) {
  return p({
    content: `My name is ${name} and my email is ${email}`
  })
}

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
    content: [
      "Check me out on mastodon.social",
      authorSignature()
    ]
  })
}

function view () {
  return main({
    content: [
      navigation(),
      frontPage(),
      information(),
    ]
  })
}
```

The implementation looks like this:

``` javascript
view({
  name: "Kurtis Rainbolt-Greene",
  author: {
    name: "Kurtis Rainbolt-Greene",
    email: "kurtis@rainbolt-greene.online"
  }
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
    My name is Kurtis Rainbolt-Greene and my email is kurtis@rainbolt-greene.online
  </footer>
</main>
```

Now as is this won't work. Two nodes (marked with `*`) need state, but their parents don't need state:

```
- view
  - navigation*
  - frontPage
  - information
    - authorSignature*
```

So my current solution is two fold, first a `stateful()` function that is literally just `applicator()`:

``` javascript
function stateful (component) {
  return statefulComponent (state) {
    return component(state)
  }
}
```

This will become important later, but here's how it's used:

``` javascript
function navigation () {
  return view(function withState ({name}) {
    return header({
      content: `Hello ${name}.`
    })
  })
}
```

We've simply wrapped the presenter function in something that knows it will eventually get state. When we render the tree at this point we will get this data structure (virtual dom nodes incoming):

``` javascript
{
  sel: "main",
  children: [
    f(),
    {
      sel: "section",
      text: "Welcome to the front page"
    },
    {
      sel: "footer",
      children: [
        "Check me out on mastodon.social",
        f()
      ]
    }
  ]
}
```

You may have noticed that two parts of this tree, the nodes that need state, are functions. These are the very same `statefulComponent()` functions we defined above. Now on to the second piece to this puzzle: Recursion!

``` javascript
function withState (state) {
  return function withStateState (component) {
    if (component.text) {
      return component
    }

    const rendered = isType("Function")(component) ? component(state) : component

    if (rendered.text) {
      return rendered
    }

    if (rendered.children) {
      return {
        ...rendered,
        children: mapValues(withStateState)(rendered.children),
      }
    }

    return rendered
  }
}
```

Our entire tree will now be implemented as:

``` javascript
withState(
  {
    name: "Kurtis Rainbolt-Greene",
    author: {
      name: "Kurtis Rainbolt-Greene",
      email: "kurtis@rainbolt-greene.online"
    }
  }
)(
  view()
)
```

For this to make sense there are two rules about virtual doms you need to know:

  1. If a dom node has `text` property, it can't have a `children` property (and the reverse).
  2. If a dom node has neither `text` nor `children`, it's a `void element` (`<meta>`, `<input>`, etc)

What we're essentially doing is traversing the tree until it's fully realized:

``` javascript
{
  sel: "main",
  children: [
    {
      sel: "header",
      text: "Hello, Kurtis Rainbolt-Greene"
    },
    {
      sel: "section",
      text: "Welcome to the front page"
    },
    {
      sel: "footer",
      children: [
        "Check me out on mastodon.social",
        "My name is Kurtis Rainbolt-Greene and my email is kurtis@rainbolt-greene.online"
      ]
    }
  ]
}
```
