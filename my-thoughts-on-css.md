I know so very little about the world, the universe, and the people around me. With that said I can be confidant in saying that **I know that Cascading Style Sheet syntax is terrible.**

If you're reading this blog and don't know what Cascading Style Sheets (CSS) are then I here's a little introduction:

* **Developed by**: World Wide Web Consortium  
* **Initially Released**: 17 December 1996, or *13 years ago*.  
* CSS is the "language" used to style HTML pages. HTML Rendering engines (Gecko, Webkit, Trident, Presto...) render the HTML and then style the output based on the CSS defined in the HTML.

**What's Wrong** Beyond being 13 years old CSS has a whole lot of problems, both major and paper cut, that lead to an unsatisfying experience. It could be a whole lot easier and that alone is something to look into.

* CSS can be defined 3 different ways. Inline in the *style* element of a tag, inline in a *style* tag in the *head* tag, and via a file referenced in the *src* element of a *link* tag inside the *head* tag. **If this doesn't sound confusing right off the bat then you've probably been working with CSS a lot.**
* Inline CSS inside the *style* element is a glut of characters that becomes, without significantly intelligent syntax highlighting, absolutely unreadable to anyone without a trained eye.
* CSS has a mass of characters that aren't needed considering the abilities of modern parsers and syntax highlighters. These characters include: **;  {  } :**
* The property names are mismatched and unorganized and with the combination variants result in a list of properties that alone surpass 200 lines. **While the combination properties were intended to shorten style sheets they have inadvertently bloated CSS rendering parsers.**
* CSS syntax is simply far too shortsighted for the uses that we see today. For instance, each property value listing is delimited with a space when there are artifacts that clearly require a space.

# The Result
In the end what you've got is a styling syntax that is a clutter mess. Sadly, the styling syntax is everywhere and there isn't much you can do about it. Even the best wrapper for CSS, SASS, is limited to Ruby (for now).

I propose that a new CSS be written for HTML Rendering Engines. Something that is far easier to understand, write, and modify.

I'd like to call this project **Looking Glass**.