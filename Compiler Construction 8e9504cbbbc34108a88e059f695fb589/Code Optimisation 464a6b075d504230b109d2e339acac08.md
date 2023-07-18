# Code Optimisation

1. Liveness - Describe how Data flow analysis is used to calculate liveness of variables

Once an IR is generated, it can then be optimised and significantly improve its execution time

1. Peephole optimisation: Very localised optimisations
2. Basic Block Optimisation: Limits its optimisation to within each basic block
3. Global Optimisation: Uses the control flow graph to optimise across basic blocks

[Peephole Optimisation](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Peephole%20Optimisation%201bf3e004cf4b48169b5635e32e38ce8c.md)

[Basic Blocks](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Basic%20Blocks%20c0246b58101c4eab81298b78c611eaec.md)

- DAG Representation
    
    ![Untitled](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Untitled.png)
    
    ![Untitled](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Untitled%201.png)
    
    If variable b is not live on exit, we could optimise the block to
    
    a = b + c
    
    d = a - d
    
    c = d + c
    
    If the variable b and d are live on exit from this block, then we would need four instructions, but the fourth would simply be a copy
    
    a = b + c
    
    d = a - d
    
    c = d + c
    
    b = d
    
    Consider
    
    a = b + c
    
    b = b - d
    
    c = c + d
    
    e = b + c
    
    Note that a = b + c and e = b + c are not the same, as b has been redefined between the instructions
    

### Dead Code Elimination

- When is a variable live?
    
    If its value may be used in subsequent instructions
    
- When is a variable dead?
    
     if its value is not used by any subsequent instruction. Dead variables no longer require storage, freeing up registers and memory
    
- Describe a Directed Acyclical Graph
    - There is a node representing the initial value of each variable in the basic block
    - Each instruction s in the basic block has a node N associated with it. Its children are the nodes that correspond to the last definition of its operands used by s
    - Each node N is labelled by the operator at s and the list of variables and temporaries for which it is the last definition within the basic block
    - A node is an output node if its variables are live on exit i.e they will be used by another block in the control flow graph
- How do you eliminate dead code?
    
    Remove any root node that has no live variable attached to it. Repeat this until all roots have at least one live variable
    
    ![Untitled](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Untitled%202.png)
    

### Algebraic Identities

- What is constant folding?
    
    Because constant expressions occur frequently, we can replace an expression like 2 * 3.14 with 6.28 at compile time
    
- Examples of computationally cheaper expressions for code optimisation
    
    x**2 = x * x
    
    2 * x = x + x
    
    x / 2 = x * 0.5
    
     
    

### Handling Array References

- Describe the correct way to handle array references in a DAG
    1. For assignments from an array, x = a[i], create a note with operator = [] with 2 children representing the initial value of the array a0 and the index i. Label the node with variable x
    2. For assignments to an array, a[i] = x, create a node with operator [] with 3 children, a0, i and x. There is no label for this node. Any existing node whose value depends on a0 is killed. A killed node cannot receive any more labels
    
- Give the DAG for x = a[i] ,a[j] = y, z = a[i]
    
    ![Untitled](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Untitled%203.png)
    
- Give the DAG for b = a + 12 , x = b[i] , b[j] = y
    
    ![Untitled](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Untitled%204.png)
    

### Building Basic Blocks from DAGs

- What are the rules for building a basic block from a DAG?
    1. Remove dead code
    2. We cannot generate an instruction for a node until we have generate the instructions for the node’s children
    3. If there are live variables attached to the node, store the node values in one of them and copy the value to the rest of them. If there are no variables, create a temporary
    4. Writes to an array must follow previous reads to or writes from, the same array
    5. Reads from an array element must follow any previous writes to the array
    
    - 

Create toggle for DAG question

### Code Motion

- What is code motion?
    
    Identifying loop invariant code (code that evaluates to the same value in each loop) and moving it outside the loop
    
    ![Untitled](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Untitled%205.png)
    
- What is an induction variable?
    
    A variable that increases by a constant (positive or negative) during each iteration of the loop. 
    
    If there are other variables that are dependent on an induction, they may also be induction variables
    
- What is strength reduction?
    
    With induction, where a computationally expensive operation is replaced by a computationally cheaper option
    

### Data Flow Analysis

- Describe data flow analysis
    
    Data flow analysis gathers information about the flow of data along all execution paths
    
    A program’s executions can be viewed as a series of transformations of the program’s state
    
    The state of a program consists of the values of all the variables, including those associated with stack frames, the stack and instruction pointer
    
    Each IR instruction s transforms the program’s state
    

Depending the analysis being performed, data-flow analysis gathers particular info for each instruction in the program by analysing all the execution paths

- Describe an execution path from p1 to pn
    
    An execution path from p1 to pn is a sequence of points p1, p2, …, pn where
    
    - In a block, pi is the point immediately preceding a statement and pi+1 is the point immediately following the statement; or
    - If pi is the end of the block, then pi+1 is the beginning of the successor block
    

Consider the following program

![Untitled](Code%20Optimisation%20464a6b075d504230b109d2e339acac08/Untitled%206.png)

If we limit ourselves to what value a variable may have at a particular point, there may be a finite solution

At the entry point to B3, the variable a has a value from {1, 234}

This is the reaching definitions problem and is useful for constant propagation if there is only one reaching definition

### Data Flow Analysis Schema

At every point in the program, we associate a data-flow value that is an abstraction of all possible program states at that point in the program

The set of all possible data-flow values is called the domain

The data-flow values before and after an instruction s is denoted by IN[s] and OUT[s] respectively

The result of a particular data-flow analysis is the solution to a set of constraints on the IN and OUT sets of s

The constraints on IN[s] and OUT[s] are either from:

- An abstraction of the semantics of s given the particular data-flow analysis; or
- The flow of control

### Transfer Functions

the relationship between data-flow values before and after an instruction s

For the same instruction, the transfer function may differ for different data-flow analyses

For some data-flow analyses, e.g reaching definitions, we are interested in how certain data flows from a known initial state at the start of the program towards the end of the program

In these cases, we will use a forward transfer function where

OUT[s] = fs(IN[s])

- When do we use a backwards transfer function?
    
    When we are interested in how certain data flows from a known final state back to the start of the program e.g live variables
    
    IN[s] = fs(OUT[s])
    

### Control-Flow Constraints

### Reaching Definitions

By knowing what value each variable x may have when the flow of control reaches p in a program, we can determine many things about x

If x can only have the same unique value no matter the flow of control, we can use constant propagation

A definition d reaches a point p if there is an execution path from the point immediately following d to p such that d is not killed along the path

A definition d of variable x is killed if there is another definition of x along the execution path

The set of definitions for the program entry point, p1, must be empty i.e OUT[ENTRY] = null

Hence a forward data-flow analysis is appropriate

Given a definition d where u = ….

fd(x) = gen(d) UNION (x - kill(d))

where gen(d) = {d}, the set of definitions generated by this instruction

kill(d) = the set of all other definitions of u in the program

We can extend this to basic blocks where

fB(x) = gen(B) UNION (x - kill(B))

where gen(B) is the set of downward exposed definitions in B and kill(B) is the union of kill sets of the block’s individual instructions

If a definition appears in both gen(B) and kill(B), the gen set takes precedence so the kill set is applied before the gen set

The control flow equations for reaching definitions uses set union as its meet operator

IN[B] = UNION p elem predecessor(B) OUT[P]

### Live Variables

backwards data-flow analysis

At the end of a program all variables must be dead

Therefore we work back from this boundary condition

Live variable analysis is important given the limited number of registers in CPUs

Since reads, writes and operations are faster when using registers, we would like to store the variables and temporaries in registers if at all possible

Typically we will have many more vars and temps than registers

However not every variable or temp is live at every point in the program

Once a var or temp is dead, we can use its register for another var or temp

The block transfer function for live variable analysis requires

def(B)

### Available Expressions Analysis

Identifying global common subexpressions

An expression x + y is available at point p if:

- Every path from the entry point to p evaluates x + y; and
- After the last evaluation prior to p there is no subsequent assignment to x or y

We know that at the start of the program there are no available expressions

Therefore this is a forward data-flow analysis and the boundary condition is OUT[ENTRY] = null

if S is the set of available expressions, then x = y + z:

- Adds y + z to S, e_gen
- Removes from S any expression involving x, e_kill

Let e_gen be the expressions generated by block B and e_kill be the set of expressions killed by block B

Then the data flow equations are

OUT[ENTRY] = null

OUT[B] = e_gen U (IN[B] - e_kill)

IN[B] =  INTERSECTION OUT[P]

Initialise OUT[B1] with U, the set of all expression in the program

The meet operator is intersection as we are interested in the available expression no matter the execution path, we have to consider all execution paths