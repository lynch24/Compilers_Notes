# Chapter 2 - Parsing, Visitors

By default, ANTLR generated parsers build a data structure called a parse tree or syntax tree that records how the parser recognised the structure of the input sentence and its component phrases

The interior nodes of the parse tree are phrase names that group and identify their children

The root node is the most abstract phrase name

The leaves of a parse tree are always the input tokens

### Implementing Parsers

ANTLR generates recursive descent parsers from grammar rules

RD parsers are really just a collection of recursive methods, one per rule

The parsing begins at the root of the tree and proceeds towards the leaves

The rule that we invoke first, the start symbol, becomes the root of the parse tree

### Building Languages Applications using Parse Trees

Lexers process characters and pass tokens to the parser, which in turn checks syntax and creates a parse tree

The corresponding ANTLR classes are:

CharStream, Lexer, Token, Parser & ParseTree

The pipe connecting the lexer and parser is called a TokenStream

![Untitled](Chapter%202%20-%20Parsing,%20Visitors%207d900d3204824cc6ad3d0aab05163c5b/Untitled.png)

The diagram shows that leaf(token) nodes in the parse tree are containers that point at the tokens in the token stream

The tokens record start and stop character indexes into the CharStream, rather than making copies of substrings

The figure also shows parse tree subclasses RuleNode and TerminalNode that correspond to subtree roots and leaf nodes

ANTLR generates a RuleNode subclass for each rule

![Untitled](Chapter%202%20-%20Parsing,%20Visitors%207d900d3204824cc6ad3d0aab05163c5b/Untitled%201.png)

These are called context objects because they record everything we know about the recognition of a phrase by a rule

AssignContext provides methods ID() and expr() to access the identifier node and expression subtree

### Parse-Tree Visitors

For when we want to control the tree walk process, explicitly calling methods to visit children