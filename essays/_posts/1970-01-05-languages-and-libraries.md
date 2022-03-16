---
title: "Languages & Libraries"
layout: article
---

Before I get into the meat of this essay lets lay down some shared terminology:

  - *import*, to make behavior, identity, or data available in the targeted scope
  - *library*, a collection of one or more behaviors, data, or identities
  - *engine*, the program that reads the source code and executes the instructions based on the language specification

In most programming languages there are three categories of libraries:

  0. "Core library", automatically available, no import required, defined by the standard leaders, each language engine must maintain the implementation
  0. "Standard library" (often called stdlib), shipped with the language engine, must be imported by the user, maintained by the language engine's team
  0. "Third Party", possibly available from a package manager's index, must be imported by the user to have access to, defined by the owner, maintained by owner

As you may have noticed there are a few properties of a library:

  0. If it comes with the language engine
  0. If it's automatically imported into the runtime
  0. Who is defining the interface
  0. Who is implementing/maintaining the definition
  0. If the library is removable after installing the engine
  0. If the library is accessible from an index

Each of these plays an important role in how the language prospers. Some libraries can exist in multiple categories or shift categories. A good example of this is the ruby library "psych". Psych was originally a third party library, an attempt at being a better implementation of the syck library in Ruby's MRI standard library. Eventually psych was elevated to "standard library" status, as a replacement for syck. syck became available as a third party library.

Elixir's an interesting case because all of it's core libraries and standard libraries are available via the primary package manager index hex. It also has an extra set of libraries from the underlying virtual machine Erlang.
