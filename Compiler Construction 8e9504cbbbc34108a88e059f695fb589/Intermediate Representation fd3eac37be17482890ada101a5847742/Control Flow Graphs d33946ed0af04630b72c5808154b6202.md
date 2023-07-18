# Control Flow Graphs

A data structure that represents the flow of control in an IR, such as 3-address code

The first step in creating a control flow graph is to partition the IR into basic blocks that represent consecutive IR statements

These will be the nodes of the control flow graph

The conditional and unconditional jumps generate the edges in the control flow graph

Control flow graphs are very useful in optimising the IR and the generation of target code