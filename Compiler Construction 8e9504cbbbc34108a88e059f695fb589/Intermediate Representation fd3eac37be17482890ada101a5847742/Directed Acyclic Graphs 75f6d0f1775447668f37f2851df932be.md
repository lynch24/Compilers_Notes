# Directed Acyclic Graphs

A variant of abstract syntax trees where nodes are not duplicated and any given node may have more than one parent

Very efficient at representing expressions and therefore generate efficient code for the expression

Consider:

a + a * (b - c) + (b - c) * d

![Untitled](Directed%20Acyclic%20Graphs%2075f6d0f1775447668f37f2851df932be/Untitled.png)

AST for this expression

The DAG can be built from the AST using a postorder traversal, constructing each node and linking to an existing node if the constructed already exists in the DAG

Otherwise add the constructed node to the DAG

![Untitled](Directed%20Acyclic%20Graphs%2075f6d0f1775447668f37f2851df932be/Untitled%201.png)

DAG for this expression