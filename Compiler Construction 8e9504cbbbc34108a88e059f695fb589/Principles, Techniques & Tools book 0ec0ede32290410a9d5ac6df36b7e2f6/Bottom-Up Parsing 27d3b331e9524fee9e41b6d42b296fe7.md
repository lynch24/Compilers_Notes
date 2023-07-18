# Bottom-Up Parsing

Shift-reduce parsing

Largest class of grammars for shift-reduce: LR grammars

## Reductions

We can think of bottom-up parsing as the process of reducing a string w to the start symbol of a grammar

At each reduction step, a specific substring matching the body of a production is replaced by a nonterminal at the head of that production

Rightmost derivation

## Handle Pruning

Bottom-up parsing during a left to right scan of the input constructs a rightmost derivation in reverse

A “handle” is a substring that matches the body of a production and whose reduction represents one step along the reverse of a rightmost derivation

A rightmost derivation in reverse can be obtained by handle pruning

We start with a string of terminals w to be parsed. If w is a sentence of the grammar at hand, then w = Yn, where Yn is the nth right-sentential form of some as yet unknown rightmost derivation

![Untitled](Bottom-Up%20Parsing%2027d3b331e9524fee9e41b6d42b296fe7/Untitled.png)

To reconstruct this derivation in reverse order, we locate the handle Bn, in Yn and replace Bn at the head of the relevant production An → Bn to obtain the previous right-sentential form Yn-1

We repeat this until we reach the start symbol S

## Shift-Reduce Parsing

Shift-Reduce Parsing is a form of bottom-up parsing where a stack holds grammar symbols and an input buffer holds the rest of the string to be parsed

The handle always appears at the top of the stack just before it is identified as the handle

We use $ to mark the bottom of the stack and the right end of the input

For bottom-up, we show the top of the stack on the right, instead of on the left

Initially, the stack is empty, and the string w is on the input

![Untitled](Bottom-Up%20Parsing%2027d3b331e9524fee9e41b6d42b296fe7/Untitled%201.png)

During a left to right scan of the input, the parser shifts 0 or more input symbols onto the stack until it is ready to reduce a string beta of grammar symbols on top of the stack

It then reduces beta to the head of the appropriate production

The parser repeats this cycle until it has detected an error or until the stack contains the start symbol and the input is empty

![Untitled](Bottom-Up%20Parsing%2027d3b331e9524fee9e41b6d42b296fe7/Untitled%202.png)

w = id * id

### The Four Operations

1. Shift: shift the next input symbol onto the stack
2. Reduce: The right end of the string to be reduced must be at the top of the stack. Locate the left end of the string within the stack and decide with what nonterminal to replace the string
3. Accept: Announce successful completion of parsing
4. Error: Discover a syntax error and call an error recovery routine

## Conflicts During Shift-Reduce Parsing

There are CFGs for which Shift-Reduce parsing cannot be used

Every shift reduce parser for such a grammar reaches a point in which the parser, knowing the entire stack contents and the next input symbol, cannot decide whether to shift or to reduce(a shift/reduce conflict) or decide which of several reductions to make (reduce/reduce conflict)

Ex: An ambiguous grammar can’t ever be an LR grammar

Consider 

![Untitled](Bottom-Up%20Parsing%2027d3b331e9524fee9e41b6d42b296fe7/Untitled%203.png)

![Untitled](Bottom-Up%20Parsing%2027d3b331e9524fee9e41b6d42b296fe7/Untitled%204.png)

We cannot tell whether if expr then stmt is the handle, no matter what appears below it on the stack - shift/reduce conflict