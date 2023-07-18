# Intro

We associate information with a language construct by attaching attributes to the grammar symbols representing the construct

A syntax-directed definition specifies the values of attributes by associating semantic rules with the grammar productions

Infix-to-postfix translator

![Untitled](Intro%202fde06fea5ff44e2b3bb8004b90505a7/Untitled.png)

This production has two nonterminals, E and T. The subscript in E1 distinguishes it from E in the head

The most general approach to SDT is to construct a parse tree or a syntax tree, and then to compute the values of attributes at the nodes of the tree by visiting the nodes of the tree

Translation can be done during parsing without building a tree