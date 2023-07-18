# Decoupling Grammars from Application Specific Code

Learn how to use parse-tree listeners and visitors to build language applications

A listener is an object that responds to rule entry and exit events triggered by a parse-tree walker as it discovers and finishes nodes

To support situations where an application must control how a tree is walked, parse trees also support visitors

It’s often most convenient and good programming practice to pass around arguments and return values, rather than using fields or other global variables

### Traversing Parse Trees with Visitors

ex: Building calculator

To build a visitor based calc, the easiest approach is to have the events methods associated with the rule expr return subexpression values

eg. visitAdd() would return the result of adding two subexpressions, visitInt() would return the value of an integer