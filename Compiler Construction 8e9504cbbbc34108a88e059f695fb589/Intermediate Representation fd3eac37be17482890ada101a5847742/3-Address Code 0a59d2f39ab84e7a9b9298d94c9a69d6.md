# 3-Address Code

In 3-address code, there is at most one operator on the right hand side of an instruction

Hence, in 3-address code, the valid instructions for expressions are:

x = y op z

z = op y

x = y

Complex instructions in the source language can be translated in a sequence of 3-address code instructions

w = x + y - z :

t1 = y - z

w = x + t1

### 3-Address Code Instructions

IR using 3-address code are built on the concepts of addresses and instructions

An address can be one of the following:

Name: A name is a pointer to the symbol table entry that corresponds to a source programme name (variable)

Constant: There may be a need to type convert a constant

Temporary: These compiler-generated temporaries are generated as needed. They may be removed by different optimisation passes. The remaining temps will be allocated to registers if possible

### Quadruples

Data structure for storing 3-address code instructions

Each quad has 4 elements

op, arg1, arg2, result

In the case of instructions like x = y or x = op y, arg2 is not used

In the case of instructions like param xi, args and result are not used

In the case of jump instructions, the target label is put into result

Consider:

a = b * -c + b * -c

t1 = minus c

t2 = b * t1

t3 = minus c

t4 = b * t3

t5 =  t2 + t4

a = t5

![Untitled](3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6/Untitled.png)

Table of quads for the instructions

### Triples

With quads, the result field is used to specify the temp that holds the result of the instruction

Triples are an optimisation (in terms of space) where we use the instruction’s position to specify the temp that will hold the result

![Untitled](3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6/Untitled%201.png)

The bracketed numbers are pointers to the temporaries associated with each entry in the triple structure

In the case of copy instructions, arg1 holds the receiving location and arg2 holds the source location

In the case of jump instructions, arg1 holds the condition and arg2 holds the target label

Ternary operations, like x[i] = y, require 2 entries in the triple structure

![Untitled](3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6/Untitled%202.png)

### Indirect Triples

In the subsequent optimisation stage, triples cause complications as reordering triples will result in a triple referring to the wrong temporary

The solution is to use another table to index into the triple structure

![Untitled](3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6/Untitled%203.png)

Now we can reorder the entries without messing up the references to temporaries

### Static Single Assignment(SSA)

An IR is in SSA form if each variable is assigned exactly once and every variable is assigned a value before it is used

If a variable is used more than once then each use is assigned a separate variable, generally using subscripts to differentiate between them

![Untitled](3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6/Untitled%204.png)

Consider the following fragment

![Untitled](3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6/Untitled%205.png)

Which SSA version of y should we use in w = x - y and z = x + y?

![Untitled](3-Address%20Code%200a59d2f39ab84e7a9b9298d94c9a69d6/Untitled%206.png)

The phi function selects the appropriate value of y depending on the control flow that occurred leading up to y3

### Type Expressions for Arrays

Arrays are best stored as a block of consecutive locations

The array name will be the base address of this block

There are 2 layouts for multidimensional arrays

Row-major: A[2][3] represents 2 arrays of size 3

Column-major: A[2][3] represents 3 arrays of size 2

Before accessing an element of an array, we need to know the size of each element and the size of each row

We get this from the array declaration and store it in the symbol table

To access A[i] where each element is stored in w bytes and the index starts at zero, we access the location A + (i * w)

To access a 2d array A[i1][i2] where each element is stored in w bytes, each row contains n2 elements and the index starts at 0

A + (i1 * n2 + i2) * w

To access a k-dimensional array A[i1]…[ik] where each element is stored in w bytes, row i contains ni elements and the index starts at 0:

A + (((i1 * n2 + i2) * n3 + i3) …. ) * w

### Translating Expressions

Build 3-address code instructions for statement S and expression E

S.code, E.code and E.addr represent the code for S, the code for E and the location that holds the value of E

The function gen(x ‘=’ y ‘+’ z) will generate the 3 address code x = y + z where + is any binary operator and concatenates it with the previously generated code(incremental translation)

The function get(id, lexeme) retrieves the address currently associated with the lexeme if id in symbol table