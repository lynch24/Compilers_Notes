# 2.2 Syntax Definition

Context-free grammars are used to specify the syntax of a language

A grammar naturally describes the hierarchical structure of most programming language constructs

if (expression) statement else statement

stmt → if (expr) stmt else stmt

This rule is called a production

In a production, lexical elements like the keyword if and the parentheses are called terminals

Variables like expr and stmt represent series of terminals and are called nonterminals

### Definition of Grammars

A context-free grammar has 4 components:

1. A set of terminal symbols, sometimes referred to as tokens. The terminals are the elementary symbols of the language defined by the grammar
2. A set of nonterminals, sometimes called syntactic variables. Each nonterminal represents a set of strings of terminals
3. A set of productions, where each production consists of a nonterminal called the head or left side, an arrow and a sequence of terminals and or nonterminals called the body or right side.
4. A designation of one of the nonterminals as the start symbol

### Derivations

A grammar derives strings by beginning with the start symbol and repeatedly replacing a nonterminal by the body of a production for that nonterminal.

The terminal strings that can be derived from the start symbol form the language defined by the grammar

Parsing is the problem of taking a string of terminals and figuring out how to derive it from the start symbol of the grammar, and if it cannot be derived from the start symbol of the grammar, then reporting syntax errors within the string

### Ambiguity

A grammar can have more than one parse tree generating a given string of terminals. Such a grammar is said to be ambiguous, all we need to do is find a terminal string that is the yield of more than one parse tree

### Exercises

Consider the following context-free grammar:

![Untitled](2%202%20Syntax%20Definition%20bc115e8c11594667be52678a93c7fb14/Untitled.png)

1. Show how the string aa+a* can be generated by this grammar

S → SS+ | SS* | a

S → SS*

S → (SS+)S*

S → aa+a*

b. Construct a parse tree for this string

c. What language does this grammar generate?

Postfix expression with + and *

 

### Exercise 2.2.2

What language is generated by the following grammars?

1. S → 0 S 1 | 0 1
2. S → + S S | - S S | a

Prefix expressions of a with + and -

c.  S → S (S) S | empty

Matched brackets, including empty string

d. S → a S b S | b S a S | empty

String with same amount of a and b with empty string

e. S → a | S + S | S S | S * | (S)

 

### Exercise 2.2.3

Which grammars in 2.2.2 are ambiguous?