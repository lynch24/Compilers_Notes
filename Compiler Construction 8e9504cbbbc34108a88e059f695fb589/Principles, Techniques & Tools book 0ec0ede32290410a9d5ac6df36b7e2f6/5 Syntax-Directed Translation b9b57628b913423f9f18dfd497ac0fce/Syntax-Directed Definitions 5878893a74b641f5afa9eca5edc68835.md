# Syntax-Directed Definitions

A syntax-directed definition(SDD) is a context-free grammar together with attributes and rules

If X is a symbol and a is an attribute of X, then we write X.a to denote the value of a at a particular parse tree node labeled X

### Inherited & Synthesised Attributes

An attribute is said to be synthesised if its parse tree node value is determined by the attribute value at its child nodes and at itself. The production must have the nonterminal  as its head

An attribute is said to be inherited if its parse tree node value is determined by the attribute value at the parent or sibling nodes. For a nonterminal B, the production must have B in its body. An inherited attribute at node N is defined only in terms of attribute values at N’s parent, N itself and N’s siblings

Example 5.1 - S-attributed SDD

SDD of a simple desk calculator

![Untitled](Syntax-Directed%20Definitions%205878893a74b641f5afa9eca5edc68835/Untitled.png)

Evaluates expressions terminated by an endmarker n

In the SDD, each of the terminals has a single synthesised attribute val

We also suppose that the terminal digit has a single synthesised attribute lexval

An SDD that has only synthesised attributes is called S-attributed

In an S-attributed SDD, each rule computes an attribute for the nonterminal at the head of a production from attributes taken from the body of the production

An S-attributed SDD can be implemented naturally in conjunction with an LR parser