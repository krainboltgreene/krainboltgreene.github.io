---
title: "Iplo: A Written Language"
draft: false
---

On a lark I decided to design my own alphabet. I had no prior experience, but it I figured it would be fun to theorycraft. I had these goals:

  - Simple for the machines
  - Learn tricks from machine languages
  - Parsable even with small print
  - Contexts like sarcasm

Here's the template for each character:

```
 ◌   ◌   ◌
 □   □   □
 □   □   □
 □   □   □
 ◌   □   ◌
```

Here's an example character:

```
 ●   ◌   ◌
 ■   ■   □
 ■   □   □
 ■   □   ■
 ◌   ■   ◌
```

This script's characters are Base 2 (Either empty or full, aka false or true, aka 1 or 0) with 10 digits (the squares). You may also notice there are 5 dots. Each dot represents a mutation of the character. In this example the single dot denotes the character is in a first word of a sentence. Alternatively a character in the last word of a sentence looks like this:

```
 ◌   ◌   ●
 ■   ■   □
 ■   □   □
 ■   □   ■
 ◌   ■   ◌
```

The first dot represents a first word, the last dot represents a last word. The middle dot represents the difference between a word or a name.

The lower two dots represent three states of context. If the first dot is full, or true, then the context is "uncertainty". The second dot represents "excitement". Finally, if both dots are true then the context is "sarcasm".

The dot mutation removes the need for certain ambiguous unique characters like the `?`, `!`, and the full stop `.`.

In addition to letters the script also has 10 special characters for "variables". Variables are single characters that can be assigned other characters.

As you might understand this can help reduce a lot of writing and reading. Also, the variable characters can be given context dots that apply to the internals.

As you may have noticed the script has 1024 permutations, excluding mutations. Here's a list of the sections:

  - `0` = Space, null character. No squares or dots are true.
  - `1..999` = Characters & Verbs
  - `1000..1009` = Variables
  - `1010..1024` = 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 100, 1000, 10000, 100000
