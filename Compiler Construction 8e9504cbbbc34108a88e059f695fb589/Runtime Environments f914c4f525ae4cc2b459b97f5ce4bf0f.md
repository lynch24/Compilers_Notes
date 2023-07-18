# Runtime Environments

Runtime Environment

10(a) [5 Marks]What is a Runtime Environment ? What does the use of a Runtime Environment enable?

10(b) [5 Marks]Using a piece of code as an example, describe how the stack frame in procedure A is modified when another procedure, procedure B, with arguments, is invoked from within procedure A

When procedure A calls procedure B, we name A the caller and B the callee

A runtime environment, also known as an activation record, is a convention that defined the environment in which a procedure is executed

It can be considered as a contract between caller and callee

It covers how the data is passed from the caller to the callee and how data is returned from callee to caller

It also describes how local data is stored and managed in a procedure

Runtime environments allow

- Separate compilation of procedures; build large programs with reasonable compilation times
- Use of libraries; use sys libraries, combine procedures written in different languages

There is no standard runtime environment

Each compiler may have its own, but then you can only link procedures compiled with the same compiler

An OS vendor can define a runtime environment for its sys libraries

Then a compiler vendor either has to use the same env, or write libraries that convert between different runtime envs

The key challenges in designing a runtime env are

- to handle nested procedures
- be as efficient as possible

Runtime envs are arch specific

We will use a MIPS runtime env to highlight the major aspects of a runtime env

### MIPS Register

![Untitled](Runtime%20Environments%20f914c4f525ae4cc2b459b97f5ce4bf0f/Untitled.png)

## Stack Frames

In most modern languages, a procedure can define local variables that are created when the procedure is entered and destroyed when exited

If a procedure is nested, particularly if recursive, there can be several instances of variables with the same name

Each instance is local to the instance of the procedure that declares it

As these instances of procedures’ local variables go out of scope in LIFO manner, a stack is a natural data structure to handle the runtime environments

Each time a procedure is called a stack frame is created by the caller

The stack frame contains:

- Space to store the arguments passed to the procedure
- Space to save the saved registers ($s0-$s7)
- Space to store the procedures return address
- Space for local data

### Arguments

The stack frame contains space to store the arguments passed to the procedure by the current procedure

The first 4 words (arg0, arg1, arg2, arg3) are not used by the caller as these arguments are passed in registers $a0 to $a3

Space is reserved for them in the stack frame in case the callee needs to spill them

While early machines passed the arguments on the stack, modern machines use registers as most procedures have 4 or less arguments

What if procedure f calls another procedure g?

It will have to save its arguments(f’s) onto the stack frame

This loses time saved by putting arguments into registers except for leaf procedures(procedures that do not call other procedures)

### Callee Saved Registers

Space for the callee to save the values of the saved registers whose value may change during the execution of the procedure

On exit from the procedure, the callee restores the original values of $s0 - $s7

### Return Address

Used by the callee to store the value of the return address register, $ra

It is copied back to $ra just before the callee returns

### Pad & Local Data

Used for local variables and any temporary registers that may need to be preserved across calls

The Pad is inserted before the data to ensure the local data starts on a double word boundary i.e stack frame size is a multiple of 8

Local Data needs to be padded to ensure its size is also a multiple of 8

## Frame Pointer