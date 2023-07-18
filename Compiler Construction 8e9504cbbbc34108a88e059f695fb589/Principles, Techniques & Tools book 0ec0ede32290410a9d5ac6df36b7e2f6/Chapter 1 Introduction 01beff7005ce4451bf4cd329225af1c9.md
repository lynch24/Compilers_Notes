# Chapter 1: Introduction

## 1.1 Language Processors

A compiler is a program that can read a program in one language(the source language) and translate it into an equivalent program into another language(target language)

An important role of the compiler is to report any errors in the source program that it detects during the translation process

An interpreter is another common kind of language processor. Instead of directly producing a target program as a translation, an interpreter appears to directly execute the operations specified in the source program on inputs supplied by the user

The machine language target program produced by a compiler is usually faster than an interpreter at mapping inputs to outputs. An interpreter, however, can usually give much better error diagnostics than a compiler, because it executes the source program statement by statement

Ex. 1.1 

Java language processors combine compilation and interpretation. A java source program may first be compiled into an intermediate form called bytecodes. The bytecodes are then interpreted by a virtual machine. A benefit of this arrangement is that bytecodes compiled on one machine can be interpreted on another machine, perhaps across a network. In order to achieve faster processing of inputs to outputs, some Java compilers, called just-in-time compilers, translate the bytecodes into machine language immediately before they run the intermediate program to process the input

In addition to a compiler, several other programs may be required to create an executable target program. A source program may be divided into modules stored in separate files. The task of collecting the source program is sometimes entrusted to a separate program, called a preprocessor. The preprocessor may also expand shorthands, called macros, into source language statements.

The modified source program is then fed to a compiler. The compiler may produce an assembly language program as its output, because assembly language is easier to produce as output and is easier to debug. The assembly language is then processed by a program called an assembler that produces relocatable machine code as its output.

Large programs are often compiled in pieces, so the relocatable machine code may have to be linked together with other relocatable object files and library files into the code that actually runs on the machine. 

The linker resolves external memory addresses, where the code in one file may refer to a location in another file. 

The loader then puts together all of the executable object files into memory for execution

### Exercises

- 1.1.1 What is the difference between a compiler and an interpreter?
    
    A compiler reads a source program and translates it into an equivalent program in another language. An interpreter directly executes the operations specified in the source program
    
- 1.1.2 What are the advantages of a compiler over an interpreter and vice versa?
    
    An executable target language program compiled by a compiler is usually much faster than an interpreted program. However an interpreter can give much better error diagnostics than a compiler
    
- 1.1.3 What advantages are there to a language processing system in which the compiler produces assembly language rather than machine language?
    
    Assembly language is easier to produce as output and easier to debug
    

[1.2 Structure of a Compiler](Chapter%201%20Introduction%2001beff7005ce4451bf4cd329225af1c9/1%202%20Structure%20of%20a%20Compiler%20878fd51d4da94ae7a5db792f31c452aa.md)

[1.4 ](Chapter%201%20Introduction%2001beff7005ce4451bf4cd329225af1c9/1%204%204aa5264de83c4bcea7fd8763741f3d56.md)