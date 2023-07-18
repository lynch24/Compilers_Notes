# Implementing Cymbol

Non-object oriented lang that looks like C without structs

To build an interpreter, compiler or translator for a lang like Cymbol, we need to verify that programs used symbols properly

- Variable references have corresponding definitions that are visible to them (in scope)
- Function references have corresponding definitions (functions can appear in any order)
- Variables are not used as functions
- Functions are not used as variables

To verify that everything is ok within a program according to the previous conditions, we should print out a list of functions and their local variables plus the list of global symbols

We should emit an error when we find a problem

The key to implementing this kind of problem is an appropriate data structure called a symbol table

Our application will store symbols in the symbol table and then check identifier references for correctness by looking them up in the symbol table

### Crash Course in Symbol Tables

The language being implemented dictates a symbol table’s structure and complexity

If a language allows the same identifier to mean different things in different contexts, the symbol table groups symbols into scopes

A scope is just a set of symbols such as a list of parameters for a function or the list of variables and functions in a global scope

Defining a symbol means adding it to scope

Resolving a symbol means figuring out which definition the symbol refers to; resolving a symbol means finding the closest matching definition

The closest scope is the closest enclosing scope

```python
listeners/vars2.cymbol
❶ int x; int y;
❷ void a() ❸{
int x;
x = 1; // x resolves to current scope, not x in global scope
y = 2; // y is not found in current scope, but resolves in global 
{ int y = x; }❹
❺ void b(int z)
6 {}
```

The global scope, 1, contains the variables x and y as well as the functions a and b

Functions live in the global scope, but they also constitute new scopes that hold the functions’ parameters, if any

Also nested within a function scope is the function’s local code block(3 and 6), which constitutes another new scope

Local variables are held in local scopes(3, 4 and 6) nested within function scopes

Because x is defined twice, we can’t just stuff all of our ids into a single set without collision

That’s where scopes come in

We keep a set of scopes and allow only a single definition for each identifier in a scope

We also keep a pointer to the parent scope so we can find symbol definitions in outer scopes

We distinguish program entities by these three parameters:

- Name: symbols are usually ids like x, f, and T but they can be operators too
- Category: What kind of the thing the symbol is- class, method, variable etc.
- Type: to validate operation x+y, we need to know the types of x and y

A symbol table implements each symbol category with a separate class, holding the name and type as properties

We can factor these out into a common Symbol superclass

```python
public class Symbol {
public String name; // All symbols at least have a name
public Type type; // Symbols have types
}
```

### Grouping symbols into scopes

To represent a scope, we’ll use an interface so that we can tag entities like functions and classes as scopes

A function is a kind of Symbol that also plays the role of a scope

Scopes have pointers to their enclosing scopes and can have names

The AST for the code regions point to their scopes