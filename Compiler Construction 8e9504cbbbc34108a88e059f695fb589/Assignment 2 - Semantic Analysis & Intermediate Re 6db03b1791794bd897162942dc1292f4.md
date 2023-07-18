# Assignment 2 - Semantic Analysis & Intermediate Representation

05/12/2022 

/dat

Add semantic checks and intermediate representation to the lexical and syntax analyser you have implemented

The generated intermediate code should be 3-address code and stored in a file with a “.ir” extension

You will need to extend your submission from Assignment 1 to:

- Generate an abstract syntax tree
- Add a symbol table that can handle scope
- Perform a set of semantic checks
    - Is every identifier within scope before it is used?
    - Is no identifier declared more than once in the same scope?
    - etc.
- Generate an intermediate representation using 3-address code

[Generating ASTs](Assignment%202%20-%20Semantic%20Analysis%20&%20Intermediate%20Re%206db03b1791794bd897162942dc1292f4/Generating%20ASTs%205aa047597097481bbe33e631e1ea1c90.md)

[Semantic Analysis](Assignment%202%20-%20Semantic%20Analysis%20&%20Intermediate%20Re%206db03b1791794bd897162942dc1292f4/Semantic%20Analysis%20be2188655d0c4134826802db8212a977.md)

[](Assignment%202%20-%20Semantic%20Analysis%20&%20Intermediate%20Re%206db03b1791794bd897162942dc1292f4/Untitled%20654c6d1460f347d2947a6f6fbec86160.md)

- [ ]  Build for Cymbol, then analyse github iterations previous, and start to implement for CAL grammar