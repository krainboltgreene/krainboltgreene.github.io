---
title: "Loving Io is Painful"
draft: false
---

Recently (as in within the last two months) I've switched almost completely from doing things in Ruby to doing things in Io. Well, at least for hobby projects. Io simply isn't at the point where it can be used for the kinds of things I do. There's no equivalent Rails or even Sinatra. While I'm working on one there still isn't a lib manager that doesn't have "testing" or "documentation" in the todo list.

Programming in Io is dreadfully fun. It makes a lot of tasks trivial and seem laughable in other languages that I've tried. I've grown fond of Io very quickly and it's helped me become aware of what I want out of a language. There's a lot that Io does right over Ruby and other languages. The focus of this post however is what Io does wrong.


**We're Still Writing** When it comes right down too it programmers are just writers who work in a shittier subset of natural languages. A lot of my ability in programming stems from my ability to write. I've always been a good editor and polisher and when needed I could pump out a 1500 essay on whatever you like. I didn't like to ever and avoided it when I could which I think translated well to programming. I also think that's why I enjoy the Red Green Refactor style of programming and BDD. I think because of that I had an excellent time learning how to use Ruby.


**Less (syntax) Is More (work)** Ruby's syntax and culture are expressive as a first class, for instance:

``` ruby
class Person
  attr_accessor :name, :age, :likes, :family

  def has_father?
    family.has_key?(:father)
  end
end

a_person = Person.new

a_person.name = "Kurtis Rainbolt-Greene"
a_person.age = 25
a_person.likes = ["Magic The Gathering", "Hacking", "Techno"]
a_person.family = { father: "Michael Greene", mother: "Beverly Rainbolt" }
a_person.has_father?
```


While some parts of this code would have to be explained a lot of it is easily understandable. What this ultimately translates to is that anyone of any Ruby experience could pick this up and run with it. There's simply not that much to understand and that's not just because it uses simplistic constructs. See how the method `has_father?` contains a `?`? Any ruby developer will tell you that method returns a `true` or `false` value. Here's a similar piece of code in Io:

``` io
Person := clone do(
  name ::= nil
  age ::= nil
  likes ::= nil
  family ::= nil

  hasFather := method(
    family hasKey("father")
  )
)

a_person := Person clone

a_person setName("Kurtis Rainbolt-Greene")
a_person setAge(25)
a_person setLikes(list("Magic The Gathering", "Hacking", "Techno"))
a_person setFamily(Map with("father", "Michael Greene", "mother", "Beverly Rainbolt"))
a_person hasFather
```


**The Thorns** I'll divide the list up into two parts. The first is syntax related and entirely fixable if Io takes the step to do so. The second is shortcut related (things that are common and can be defined in a shorter manner), but doesn't need to be done by Io.

Here's the list of syntax problems:

  * I can't create a method with a `?` or `!` which is strange considering how flexible Io is, oh but wait it works just fine if you're defining a method like `=?` or similar.
  * There are `list()` and a `map()` methods on Object, but *the latter doesn't create a* `Map`!
    It's like Ruby's `Enumerable#map()`.
  * Neither `List` nor `Map` have any literals defined and every Io core developer will come up each with a different way to write one.
    None of them seem to understand the problem behind that.
  * Symbols, or Atoms, are a important part of programming and Io has them, but they are literally indistinguishable from Strings (Which are sometimes called Sequences?).
  * By defining a slot with the setter assigner `::=` you create 2 methods, a setter and the actual slot.
    Io has decided to surprise you by mutating the name of your slot.

Here's the list of shortcuts I find missing in Io:

  * There's no way to define a list of setters with nil value, which happens a lot.
  * Another shortcut missing is a way of defining methods that would visibly differentiate them from assignments.
  * I shouldn't have to specify `clone do()` everywhere.

Noticeably one list is longer than the other and that's because I think adding these shortcuts would actually change Io for the worse. One of Io's biggest plusses in my book is that the syntax is unilaterally the same. Defining a assignment or method is the same as defining a "class" or "module", which also happen to be the exact same thing. These things are also the same as calling a method on an object, because under the covers `:=` and `::=` are just methods!

So what would it look like with my suggestions? Turns out it works much better:

``` io
Person := clone(
  setters("name", "age", "likes", "family")

  hasFather? := method(
    family hasKey?("father")
  )
)

a_person := Person new
a_person name("Kurtis Rainbolt-Greene")
a_person age(23)
a_person likes([ "Magic The Gathering", "Hacking", "Techno" ])
a_person family({ father := "Michael Greene", mother := "Beverly Rainbolt" })
a_person hasFather?
```

And the Io version again:  

``` io
Person := clone do(
  name ::= nil
  age ::= nil
  likes ::= nil
  family ::= nil

  hasFather := method(
    family hasKey("father")
  )
)

a_person := Person clone

a_person setName("Kurtis Rainbolt-Greene")
a_person setAge(25)
a_person setLikes(list("Magic The Gathering", "Hacking", "Techno"))
a_person setFamily(Map with("father", "Michael Greene", "mother", "Beverly Rainbolt"))
a_person hasFather
```

And now the Ruby version, for posterity:

``` ruby
class Person
  attr_accessor :name, :age, :likes, :family

  def has_father?
    family.has_key?(:father)
  end
end

a_person = Person.new

a_person.name = "Kurtis Rainbolt-Greene"
a_person.age = 25
a_person.likes = ["Magic The Gathering", "Hacking", "Techno"]
a_person.family = { father: "Michael Greene", mother: "Beverly Rainbolt" }
a_person.has_father?
```


Now lets be clear about these examples. These are simplistic examples of what I consider to be a problem. I will even admit that complex things in Io tend to be less work than complex things in Ruby. I admit that with a pretty heavy caveat: Complex things are less work in spite of these things rather than because of these syntax "issues".


**Conclusion** I'm not quite sure how to end this piece. As someone who is building the *4th Io package manager* I find myself annoyed that these things aren't in the language. I have to deal with it constantly while writing Io and it shocks me that others wouldn't find this a problem. I also realize that the people who couldn't care less about the my issue are people who have already accepted Io. They actively work on Io as a main hobby.

Shouldn't the Io community, as small as it already is, be more concerned as to why it's the only interpreted language without `List` or `Map` literals? It's been suggested that I poll Io devs on if they want it or not. I submit that it would be the most useless poll ever as we already know if they want it or not. They're actively using Io. Instead a better poll would be if Ruby, Python, or Javascript developers would be OK with removing `[]` or `{}` from their programming vocabulary.

Here's a good example: Name a single repository of Ruby, Python, Javascript, or Lua code that doesn't use these literals. How's that for a metric of need?
