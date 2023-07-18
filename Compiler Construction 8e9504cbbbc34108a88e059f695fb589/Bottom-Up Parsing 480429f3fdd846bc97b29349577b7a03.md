# Bottom-Up Parsing

LL(k) parsers must predict which production rule to use having seen the first k tokens of the right hand side

LL(k) stands for left to right parse, leftmost derivation using k token lookahead

LR(k) parsing stands for left to right parse, rightmost derivation using k token lookahead

Many programming languages require an LR(1) grammar and parser

## LR Parse Engine

An LR parser has a stack and an input

The first k tokens of the input are the lookahead

The LR engine shifts tokens from the input onto the stack and when the tokens on top of the stack match the right hand side of a production rule, the left hand side is pushed onto the stack after popping the tokens off

if stack contains ABC and there is a rule X → ABC

Pop C, B, A and push X onto stack

The stack is initially empty and when the parser shifts the EOF marker $ onto the stack and the input has a valid derivation, the parser enters the accepting state

Otherwise the parser ends in an error state

## Valid LR Parse Actions

- shift(n): advance input by one token and push state n onto the stack
- reduce(k): Pop off the number of symbols on the righthand side of rule k. Let X be the lefthand side symbol of rule k. Lookup X in the state table to get “goto n”, push state n onto the stack
- accept: stop parsing and report success
- error: stop parsing and report failure

## LR(0) Parsing

The key to LR parsing is generating an LR parse table

LR(0) makes its decisions without lookahead, solely by looking at the stack

Initially we will have an empty stack and the input will have a complete sentence S followed by the eof marker $

We denote this as

S’ → .S$

where the dot indicates the current parser position

A production rule combined with a parser position is called an LR(0) item

In this state, an input that can be parsed not only begins with S but also with any possible righthandside of an S production

S’ → .S$

S → .x

S → .(L)

This is state 1; a set of LR(0) items