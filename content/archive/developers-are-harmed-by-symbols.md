---
title: "An ORM born from my frustration with the status quo"
draft: false
---

> Note: If you're reading this and have English as a second language I want to apologize up front if I come off as saying something that sounds like "English is the best for programming". There is nothing to my knowledge that makes English good for programming, but instead I make this commentary because I only know English. If I only knew Spanish or French I would probably have that in place of English.

Most programmers I know can probably understand this piece of code:

``` ruby
2 + 3
```

I could ask them the meaning of the expression and they would probably say something like "You're adding two and three, to get a sum." This is obvious to them (and me) because of the heavy amount of modern education we have received. I specify modern because the `+` sign, or the plus sign, hasn't existed very long as the abstract concept of numerical addition:

> ... the symbols that have become entwined with our conception of math—like the + and = signs—are in fact rather modern inventions. Until around the 16th century, most math was written in metered verse. For thousands of years, even the simplest of math equations was a word problem (not unlike those at the root of much frustration over new Common Core educational standards). ... Mazur writes that "mathematical symbols begin as deliberate designs created by mathematicians." The plus sign, originally a shorthand for the Latin word for "and," et, came about in the late 15th century.
>
> [Shaunacy Ferro, The Surprisingly Short History Of The Plus Sign][HISTORYOFPLUS]

The use of shorthand to mean something more verbose isn't a concept alien to software developers. In the oldest book I have on the subject, [Starting Forth by Leo Brodie][STARTINGFORTH], describes the Forth language's `DUP` word which stood shorthand for "duplicate". This lives on in things like Ruby's `def` which stands for "define" or more technically "define method".

Incidentally however in the same book Mr. Brodie's code is mostly devoid of shorthand:

``` forth
( Large letter F )
: star 42 emit ;
: stars   0 do  star  loop ;
: margin  cr 30 spaces ;
: blip margin star ;
: bar  margin 5 stars ;
: f    bar blip bar blip blip cr ;
```

It seems in wanting to teach the language Mr. Brodie chose to not pick shorthand for his usage. I posit that he wanted to avoid confusing developer tendencies to be as clear as possible to the student. Though the developer's shorthand and the reason for the `+` are evident they exist as a shorthand for another concept. An abstraction for the reader to come to understand. This poses a problem of sorts, because while you and I know what `+` means in the first code example why not try to figure out this Ruby example:

``` ruby
nga_mga ++ nga_mga2
```

I've purposefully used non-English words to accentuate the point. There is no `++` method in Ruby's core or standard library. If you knew Elixir you could probably guess this was appending an list to another list, but you don't know the developer's intent. Instead consider this:

``` forth
materials2 materials join
```

Even though this is Forth you are probably able to see exactly the intent, because the word join has a specific enough meaning. While the developer could have defined a word called join that means something other than bringing together it would be foolish to do so and we would see it immediately. Our reliance as developers on shorthand and symbols creates a barrier to entry in that we either are required divine intent or know the sum of the code.

To experienced developers, like my peers, this may seem like a really poor argument. "Of course ++ means join, what else could it mean?" they'll ask as if stupified that I would suggest it hard to understand. Yet when I have students one of the most common things that comes up is how weird it is that `=` doesn't mean equality. This can be especially annoying for kids as they are constantly hammered with the idea that it *does* mean equality in daily school life.

I bring up Mr. Brodie's book for another reason: His examples are almost without any shorthand. This is actually very common as far as I've read among Forth and Scheme books. While

blah blah blah

  - talk about confusion
  - clarity in clear writing code
  - possible language barriers


[HISTORYOFPLUS]: http://www.fastcodesign.com/3032233/the-surprisingly-short-history-of-the-plus-sign#1
[STARTINGFORTH]: http://www.forth.com/starting-forth/
