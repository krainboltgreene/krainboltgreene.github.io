---
title: "Darklang is a Starry Night"
layout: article
---

Darklang genius in so many ways that it makes me actually frusted. Frankly I am jealous of Ellen Chisa & Paul Biggar's art.

Okay, so maybe you've never heard of Darklang. That's fine, I learned about it some time ago when I was either asked to edit a blog post about it or someone called out for people to give a review. I can't remember, so I'll go with the one that makes me feel beter.

I remember distinctly reading the outline for Paul's plan: Remove the line between development, staging, and production. All code exists and is ready to be used, it simply has to be called. I was hesitant on the whole idea. In fact, I pretty much figured I'd never see it again. What moron would try to not only keep code in production, but also try to make a business out of owning things engineers love to own?

It turns out Paul previously founded Circle CI and Ellen Chisa did product at Kickstarter and Microsoft, both prepared to deal with (what I believe) are the worst customers in existance.

Fast forward to the first part of this global pandemic and I'm talking to a good friend Lex. He links me to this darklang, which had completely forgotten about. It's live, has users, and is actually running people's code. It's more than just a blog post now it's a thing that breaths. I erronously thought it was a programming language, which is why I even bothered.

Long story short, Darklang is many pieces of a environment you're already familiar with: You start off with a literal blank canvas. The side menu shows a few things you can put on the canvas: A general function, a web function, a triggered function, a scheduled function, a database module, and some orther things. Last it has a item called "404s", but I'll get back to that.

If you click any of these items in the menu it immediately makes a blank instance of said item on the canvas. The different kinds of functions deal with different roles. General functions take input, do a thing, and return an output. Web functions are the same, except the input is always an incoming web request. Triggered functions are for asyncronous acts. Scheduled jobs are the same, but on a timer. The database is the only disappointing part, it's just a document store.

So far I've just described a programming editor, just in a form that hasn't been talked about in a couple of decades. In fact nothing that Darklang does is in any way new. It's just refined. For example, that "404s" tab? Turns out any request that hits your canvas that isn't caught by a web funtion goes there. It generates a nice 404, but with one click you can turn any 404 into a web function.

This is such a powerful feature on it's own, but that's not it: Every one of these functions has a trace history, or a list of all the times it was called, the types and data of the inputs, the return values. Recently they released a new feature where you could see what parts of your code ran for a particular trace. Are you giddy yet? How about if I told you that you can take a particular trace, make a fork of the function specifically for that data?

I urge you to leave this blog and go get beta access. If you're still here, or coming back, lets talk more about the platform itself. One thing I haven't mentioned is that Darklang has it's own...language. It's complicated, but it's like a GUI syntax. I suppose it's based on ocaml, but that's hardly the point.

As a toy project I've been working on a sort of clone project of Darklang. Not a competing project, but where I'm chosing to make all the choices I thought darklang should have made. First, I wanted to pick an already designed language called Elixir. I felt it was ideal for the goal. My toy project has functions just like darklang.

(As an implementation detail functions are records in a database with properties that match elixir functions: Name, arguments, guards, typespecs, docs, a owning module, etc.)

I give functions a sha and a linked history, so when you define the function `greet(subject is Person)` and then use it in many places, but later you want to allow the caller to also greet dogs `greet(subject is Dog)` you wont break any code. The old call sites are referencing the old method's sha. You switch over to the new function when you're ready to refactor that call site.

However, what I quickly realized is that I now have to handle functions my project doesn't own: What if the user wants to encrypt a payload? Base64 decode an input? What about functions that are generated via macros? Darklang doesn't have these problems because it's something new.

These are the types of little geniuses that I've been enjoying about darklang. That said they have a rough road ahead of them. In fact when I talked about it with another engineer, the first thing they said was "Uh, no I don't want anyone else to manage my stack." I casually reminded them that they had recently moved from on-prem to hosted K3S precicely because they did want someone to manage their stack.

I'm excited for darklang's future and further excited to see more little morsels of programming art from that team.
