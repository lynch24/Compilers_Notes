# Language Pattern Implementations - Symbol Table

To build a symbol table, we need to formalise what we do when we write software

![Untitled](Language%20Pattern%20Implementations%20-%20Symbol%20Table%20d4479ce197b345f79349991b5866c64b/Untitled.png)

Implicitly we define three symbols in our head: Class T, Function f and Variable x

![Untitled](Language%20Pattern%20Implementations%20-%20Symbol%20Table%20d4479ce197b345f79349991b5866c64b/Untitled%201.png)

For those definitions we need to do something like this

Each of these symbols has at least the following three properties:

Potential solution: Extend Symbol class; VariableSymbol, FunctionSymbol, ConstSymbol

- Name: symbols are usually ids like x, f and T
- Category: what kind of thing the symbol is; class, function etc
- Type: type of the symbol, int, void etc

A symbol table implements each category with a separate class, holding name and type as properties

![Untitled](Language%20Pattern%20Implementations%20-%20Symbol%20Table%20d4479ce197b345f79349991b5866c64b/Untitled%202.png)

![Untitled](Language%20Pattern%20Implementations%20-%20Symbol%20Table%20d4479ce197b345f79349991b5866c64b/Untitled%203.png)

### Grouping Symbols into Scope

A scope is a code region with a well-defined boundary that groups symbol definitions ( in a dictionary with that code region)

Scope boundaries often coincide with begin and end tokens like curly braces or keywords

We call this lexical scoping because the scope is lexically delimited

To represent scope, we can use an interface so that we tag entities like functions and classes as scopes

### Monolithic Scopes

Single global scope

Today, only simple langs like config files use one scope

Within a scope, a symbol can represent a single entity

That means redefining a property either overwrites it or results in an error

Tracking symbols for a monolithic scope just means maintaining a set of Symbol objects

As we encounter definitions, we create symbols and add them to the set

Later we look them up by name 

### Multiple & Nested Scopes

Lets us reuse the same IDs in different code regions

To avoid ambiguity, programming languages use context to figure out which symbol we’re referring to

To track nested scopes, we push and pop scopes onto a scope stack

As we encounter a new scope, we push it on the scope stack

The top of the stack is the current scope

### Resolving Symbols

If we’ve only got one scope, resolving a symbol is easy

Either we find that symbol in the scope’s Symbol table or we don’t

If resolve() doesn’t find the symbol in the current scope, it asks the enclosing scope if it can find the symbol

resolve() recursively walks toward the root of the scope tree until it finds the symbol or runs out of scopes

On begin, we need to push a new scope

On end, we need to pop that scope