# Peephole Optimisation

Limits its optimisations to a very small window i.e a few lines of code, that it slides across the code

### Redundant Instruction Elimination

Depending on the IR generation algo, it can sometimes generate redundant lines such as

a = r0

ro = a

If the 2nd instruction doesn’t have a label, it can be removed

### Unreachable Code

if debug = 1 goto L1

goto L2

becomes

if debug ≠  1 goto L2

### Algebraic Simplification & Reduction in Strength

IR instructions like x = x + 0 and x = x * 1 can be removed

IR instructions like x**2 can be replaced with x * x which are implemented more efficiently

Fixed-point multiplication and division is more efficiently implemented by Floating-point multiplication by an appropriate constant

### Machine Idioms

Many machines have an auto-increment and auto-decrement instruction

Hence x = x + 1 can be replaced with x++