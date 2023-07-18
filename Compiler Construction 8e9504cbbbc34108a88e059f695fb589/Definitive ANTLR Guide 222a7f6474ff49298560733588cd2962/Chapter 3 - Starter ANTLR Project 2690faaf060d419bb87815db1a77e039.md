# Chapter 3 - Starter ANTLR Project

### Building a language application

To move beyond recognition, an application has to extract data from the parse tree

The easiest way to do that is to have ANTLR’s builtin parse tree walker trigger a bunch of callbacks as it performs a depth-first walk

To write a program that reacts to the input, all we have to do is implement a few methods in a subclass of the BaseListener