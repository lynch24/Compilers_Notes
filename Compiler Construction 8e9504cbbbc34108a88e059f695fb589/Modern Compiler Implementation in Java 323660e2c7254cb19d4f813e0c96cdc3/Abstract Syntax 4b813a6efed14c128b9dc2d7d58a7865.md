# Abstract Syntax

### Visitors

A visitor implements an interpretation; it is an object that contains a visit method for each syntax tree class

Each syntax-tree class should contain an accept method

An accept method serves as a hook for all interpretations

It is called by a visitor and it has just one task: it passes control back to an appropriate method of the visitor