---
title: "Looking Glass"
draft: false
---

One of the newer projects I've been working on lately is what I call **Looking Glass**. The Looking Glass project is a script written in the Ruby programming language. The script takes content and keys and produces HTML output. The keys are defined blocks that contain regular expressions and style properties paired with values. Many people will instantly recognize the similarity between a Glass list and Cascading Style Sheets. Here's an example of [two key blocks][0].

The similarities should be rather stark. One of the large differences is that **Looking Glass** has less properties and values, focusing mainly on typography and similar. Key blocks that start with a "!" will automatically look for other matching values. It's this kind of behavior which will allowing certain stylistic freedom and ease of developing. Changing your stylesheet is spot on simple.

This project got it's start after I heard about a similar python scrip that was being used with Ruby markdown/textile generators. I figured a Ruby version wouldn't be a bad idea. The project was folded into another idea I had for simple stylesheet like syntax highlighting instead of the bulky mess that is XML.

The project also has the secondary benefit of being a great learning tool for me. Writing my own parser has been rather enjoyable and helped me realize which areas I need to improve on.

[0]: http://pastie.org/879590
