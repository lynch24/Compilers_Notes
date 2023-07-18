# 1.2 Structure of a Compiler

Analysis(frontend) and Synthesis(backend)

![Structure of a compiler, broken into phases](1%202%20Structure%20of%20a%20Compiler%20878fd51d4da94ae7a5db792f31c452aa/Untitled.png)

Structure of a compiler, broken into phases

## Lexical Analysis

The first phase of a compiler is called lexical analysis or scanning. The lexical analyser reads the stream of characters making up the source program and groups the characters into meaningful sequences called lexemes. For each lexeme, the analyser produces as output a token of the form

{token-name, attribute-value}

that it passes onto the subsequent phase, syntax analysis.

Token name is an abstract symbol that is used during syntax analysis, and the second component attribute value points to an entry in the symbol table for this token

Information from the symbol table entry is needed for semantic analysis and code generation

position = initial + rate * 60

1. Position is a lexeme that would be mapped into a token {id, 1}, where id is abstract symbol standing for identifier and 1 points to the symbol table entry for position. The symbol table entry for an identifier holds information about the identifier, such as name and type
2. The assignment symbol = is a lexeme that is mapped to the token {=}. Since this token needs no attribute value, we have omitted the second component. We could have used any abstract symbol such as assign for the token name, but for convenience we have chosen to use the lexeme itself as the name of the abstract symbol
3. initial is a lexeme mapped to the token {id, 2}
- + is a lexeme mapped to token {+}
- rate is {id, 3}
- * is a lexeme mapped to {*}
- 60 is a lexeme mapped to {60}

![Translation of an assignment statement](1%202%20Structure%20of%20a%20Compiler%20878fd51d4da94ae7a5db792f31c452aa/Untitled%201.png)

Translation of an assignment statement

## Syntax Analysis

The parser uses the first components of the tokens produced by the lexical analyser to create a tree-like intermediate representation that depicts the grammatical structure of the token stream. A typical representation is a syntax tree in which each interior node represents an operation and the children of the node represent the arguments of the operation

The subsequent phases of the compiler use the grammatical structure to help analyse the source program and generate the target program. In chapter 4 we shall use context-free grammars to specify the grammatical structure of programming languages and discuss algos for constructing efficient syntax analysers automatically from certain classes of grammars.

## Semantic Analysis

The semantic analyser uses the syntax tree and the information in the symbol table to check the source program for semantic consistency with the language definition. It also gathers type information and saves it either in the syntax tree or the symbol table, for subsequent use during the intermediate code-generation.

An important part of semantic analysis is type checking, where the compiler checks that each operation has matching operands.

The language specification may permit some type conversions called coercions.

## Intermediate Code Generation

In the process of translating a source program into target code, a compiler may construct one or more intermediate representations, which can have a variety of forms. Syntax trees are a form of intermediate representation

After syntax and semantic analysis of the source program, many compilers generate an explicit low level or machine like intermediate representation, which we can think of as a program for an abstract machine. This intermediate form should have two important properties; it should be easy to produce and easy to translate to the target machine

## Compiler-Construction Tools

Parser generators: automatically produce syntax analysers from a grammatical description of a programming language

Scanner generators: lexical analysers from regex description of the tokens of a lang

Syntax-directed translation engines

Code-generator generators

Data flow analysis engines - code optimisation