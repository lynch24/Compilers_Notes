# Intermediate Representation

1. Intermediate Code - Convert source code into 3-address intermediate code using syntax-directed approach, generate control flow graph
2. Construct a DAG

### Why use an Intermediate Representation?

- Allows us to partition the compilation process and in particular, separate the frontend from the backend
- Simplifies changing the target code architecture without modifying the lexical, syntactic and semantic analysis
- Allows for multiple machine independent optimisation passes to be carried out

### Types of IRs

Goal of an IR to is allow multiple optimsation passes, where each transforms the current IR into an equivalent IR that runs more efficiently

Many different types of IRs:

- Abstract Syntax Trees
- Directed Acyclic Graphs(DAGs)
- Control flow graphs
- 3-address code
    - Single Statement Assignment(SSA)
- Hybrids
    - Combinations of all of the above, this is what is usually implemented

[Directed Acyclic Graphs](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/Directed%20Acyclic%20Graphs%2075f6d0f1775447668f37f2851df932be.md)

[3-Address Code](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6.md)

[Control Flow Graphs](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/Control%20Flow%20Graphs%20d33946ed0af04630b72c5808154b6202.md)

### 3-Address Code

In 3AC there is at most one operator on the right-hand side

Valid instructions for expressions are

x = y op z

x = op y

x = y

Complex expression in the source language can be translated to 3AC

w = x + y - z

→

t1 = x + y

w = t1 - z

3AC is actually a linearised version of an AST or DAG

![Translate bottom-up](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/Untitled.png)

Translate bottom-up

### 3AC Instructions

Intermediate representation using 3AC is built on the concepts of addresses and instructions

An address can be one of the following:

- Name: Pointer to the symbol table entry that corresponds to a source program name
- Constant: May be a need to type convert a constant
- Temporary: Compiler-generated temporaries that are created as needed. They may be removed by different optimisation passes. Remaining temps will be allocated to registers if possible

![Untitled](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/Untitled%201.png)

![Untitled](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/Untitled%202.png)

![Untitled](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/Untitled%203.png)

### Quadruples

Data structure for storing 3AC instructions

Each quad has 4 elements

op, arg1, arg2, result

![Untitled](Intermediate%20Representation%20fd3eac37be17482890ada101a5847742/Untitled%204.png)

### Translating Expressions

Syntax-directed definition approach to build the 3AC instructions for expressions statement S and expression E

The attributes S.code, E.code and E.addr respectively represent code for S, code for E and address that holds the value for E

The function gen(x = y + z) will produce the 3AC code x = y + z where + is any binary operator and concatenates it with the previously generated code (incremental translation)

The function get(id.lexeme) retrieve the address currently associated with the lexeme if id in the symbol table