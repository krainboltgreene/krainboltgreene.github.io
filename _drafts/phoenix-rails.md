---
title: "Phoenix Vs Rails"
---

I keep seeing these elixir talks where someone tells the audience "rails is old baggage, don't bring it to phoenix" and their main points are:

  1. rails/active record bad
  2. what if stateful?
  3. elixir is concurrent!

So I want to dissect these for a moment.

re: 1

Rails isn't unique in most of it's design. It's years (multiple decades) of choices that fit the web development's path of least resistance. Often the argument is that "rails is mvc", which is just...not true. It never has been true.

re: 2

I have never seen anyone argue successfully that adding state to something that is easily stateless has been trivial or worth the effort. We use databases not because they are fun, but because they are fucking good at state.

Re: 3

Sweet, that would have mattered a lot more before:

  1. background jobs got a lot easier
  2.
