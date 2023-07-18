# Register Allocation

1. Register Allocation & Graph Colouring - consider liveness at each point in block

While generating the IR, we have assumed that we can have an infinite number of temporaries

During code optimisation we take many measures to reduce the number of temps

The remaining variables + temps are then assigned to registers and memory

Accessing registers is faster than accessing memory

If temps ti and tj are not live at the same time, then we could use the same register for both of them

### Colouring by Simplification

Colouring by simplification is a 4 phase linear-time approximation

1. Build an inference graph where each node represents a variable or temporary and an edge exists between two nodes if they are both live at the same time
2. Simplify removes a node with < K neighbours (where K is the number of registers) and places it on a stack
3. Spill nodes with ≥ K neighbours. We’ll use optimistic colouring and assume that potential spill nodes do not interfere with other graph nodes. If later we are wrong they become actual spill nodes and are stored in memory
4. Select a colour (register) for each popped node so that no neighbouring node has the same colour. If we can’t select a colour it is an actual spill node. Rewrite the code to use memory for that variable/temp and rerun the algo.

### Build

To build the inference graph, make a node for each variable/temp that appears in the OUT section (each group of variables that appear together in an OUT are all live at the same time)

Connect the nodes that are live at the same time

![Untitled](Register%20Allocation%206eb1cf78ea554ff7854173d39476e523/Untitled.png)

![Untitled](Register%20Allocation%206eb1cf78ea554ff7854173d39476e523/Untitled%201.png)

Inference graph for this block

Move instruction means assignment e.g d = c

### Simplify

During the simplify phase, we select any node in the interference graph with < K edges

We remove this node and its edges from the graph, and push it onto a stack

Removing a node and its edges may result in a node that previously had ≥ K edges now having < K edges

Repeat until there are no more nodes with < K edges

In the example, assuming K = 4, we can initially remove t1, t2, t3, g, h, k

![The resulting graph & stack of the simplify phase](Register%20Allocation%206eb1cf78ea554ff7854173d39476e523/Untitled%202.png)

The resulting graph & stack of the simplify phase

### Spill

After the simplify phase, we are left only with significant nodes, with ≥ K edges

These may be a problem and need to be spilled i.e stored in memory, if all the neighbouring nodes use different colours

We will adopt an optimistic approach and assume that any potential spill node can be coloured and push it onto the stack

### Select

In the select phase, a node x is popped from the stack and the graph is rebuilt after the node is assigned a colour (register)

The colour is assigned arbitrarily but the only limitation is you cannot use a colour of a neighbouring node while rebuilding the graph

Sometimes, when processing a potential spill node, it may not be possible to assign it a colour

In this case a potential spill node becomes an actual spill node and the code must be rewritten to retrieve the variable from memory before using it and storing it back to memory immediately after use

Then the whole build-simplify-spill-select process is repeated

![Resulting stack and rebuilt graph of the spill-select phases, each node coloured by the number of its respective register](Register%20Allocation%206eb1cf78ea554ff7854173d39476e523/Untitled%203.png)

Resulting stack and rebuilt graph of the spill-select phases, each node coloured by the number of its respective register

### Coalescence

If two nodes are move related but do not interfere with each other (i.e no edge between them), then they can be coalesced into one node whose edges are the union of the edges of the two nodes

This eliminates the move instruction

Any two non-interfering nodes can be coalesced, but the resulting node has more edges and may become an actual spill node

Briggs Strategy:

Nodes x and y can be coalesced if the resulting node xy will have less than K neighbours of significant degree( ≥ K edges)

### Coalescence Based Colouring

1. Build an interference graph as before but this time mark each node as non-move-related or move-related if the node was the src or dst of a move instruction
2. Simplify non-move-related nodes with  < K edges
3. Coalesce move-related nodes. The resulting nodes are non-move related and are available for further simplify phases
4. If neither coalesce or simplify can be applied, select a low-degree move related node and freeze it. This causes the node to be considered as a non-move-related node. Essentially we have to give up hope of coalescing it. The node is now available for further simplify and coalesce phases
5. If there are no low degree nodes, we select a significant degree node as a potential spill node and push it onto the stack
6. As before, pop nodes from the stack, assign colour or make an actual spill node