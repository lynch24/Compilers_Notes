# Intro to LR Parsing: Simple LR

Most prevalent kind of LR parsing today is LR(k) parsing

k = 0 and k = 1 are of practical interest

## Why use LR Parsers?

LR parsers can be constructed to recognise virtually all programming language constructs for which CFGs can be written

The LR parsing method is the most general nonbacktracking shift-reduce parsing method known, yet it can be implemented as efficiently as other, more primitive shift-reduce methods

An LR parser can detect a syntax error as soon as it is possible to do so on a left right scan of input

The class of grammars that can be parsed with LR methods is a proper superset of the class of grammars that can be parsed with predictive or LL methods

For a grammar to be LR(k), we must be able to recognise the occurrence of the right side of a production in a right-sentential form, with k input symbols of lookahead

This requirement is far less stringent than that for a LL(k) grammar where we must be able to recognise the use of a production seeing only the first k symbols of what its right side derives

The principal drawback of using LR parsing methods is that it is too much work to construct by hand for a typical programming language grammar

## Items and the LR(0) Automaton

An LR parser makes shift-reduce decisions by maintaining states to keep track of where we are in parse

States represent sets of items

An LR(0) item of a grammar G is a production of G with a dot at some position of the body

A → XYZ yields 4 items

![Untitled](Intro%20to%20LR%20Parsing%20Simple%20LR%20f012288125b8478cacda8e35450fa1c1/Untitled.png)

The production A → empty generates only one item, A → .

An item indicates how much of a production we have seen at a given point in the parsing process

A → X.YZ indicates that we have just seen on the input a string derivable from X and that we hope to see a string derivable from YZ

A → XYZ. indicates that we have seen the body XYZ and that it may be time to reduce to A

The canonical LR(0) collection provides the basis for constructing a DFA that is used to make parsing decisions. Such an automaton is called an LR(0) automaton

Each state of the LR(0) automaton represents a set of items in the canonical LR(0) collection

### Constructing the Canonical LR(0) Collection

To construct the canonical LR(0) collection for a grammar, we define an augmented grammar and two functions; CLOSURE and GOTO

If G is a grammar with start symbol S, then G’ is the augmented grammar with start symbol S’ → S

The purpose of this new start symbol is to indicate to the parser when it should stop parsing and announce acceptance; when and only the parser is about to reduce by S’ → S

 

### Closure of Item Sets

If I is the set of items for a grammar G, then CLOSURE(I) is the set of items constructed from I using two rules:

1. Add every item in I to CLOSURE(I)
2. If A → a . BC is in CLOSURE(I) and B → Y is a production, then add item B → .Y to CLOSURE(I), if it is not already there. Apply this rule until no more new items can be added

A → a . BC indicates that at some point, we expect to see a substring derivable from BC as input

The substring derivable from BC will have a prefix derivable from B, we therefore add items for all the B productions, that is, if B → Y, then we include B → .Y in CLOSURE(I)

Example: Consider

![Untitled](Intro%20to%20LR%20Parsing%20Simple%20LR%20f012288125b8478cacda8e35450fa1c1/Untitled%201.png)

If I is the set of one item {[E’ → .E]}, then CLOSURE(I) contains the set I0

![Untitled](Intro%20to%20LR%20Parsing%20Simple%20LR%20f012288125b8478cacda8e35450fa1c1/Untitled%202.png)

We divide all the sets of items of interest into two classes:

Kernel items: the initial item, S’ → S, and all items whose dots are not at the left end

Non-kernel items: All items with their dots at the left end, except for S’ → S

### The Function GOTO

GOTO(I,X) where I is a set of items and X is a grammar symbol. GOTO( I, X) is defined to be the closure of the set of all items [A → aX.B] such that [A → A . XB ] is in I

The GOTO function is used to define the transitions between states in the LR(0) automaton

GOTO(I, X) specifies the transition from the state for I under input X

If I is the set of items { [E’ → E.], [E → E. + T] }, then GOTO(I, +) contains the items

E → E + . T

T → .T * F

T → .F

F → .(E)

F → .id

## Use of the LR(0) Automaton

How can LR(0) automata help with shift-reduce decisions?

Suppose that a string Y of grammar symbols takes the LR(0) automaton from the start state 0 to some state j

Shift on the next symbol a if state j has a transition on j, else reduce

The items in state j will tell us which production to use

The LR parsing algorithm uses its stack to keep track of states as well as grammar symbols

## The LR Parsing Algorithm

Where a shift reduce parser would shift a symbol, an LR parser shifts a state

![Untitled](Intro%20to%20LR%20Parsing%20Simple%20LR%20f012288125b8478cacda8e35450fa1c1/Untitled%203.png)

The stack holds a sequence of states, in SLR, the stack holds states from the LR(0) automaton

### Constructing the SLR Parsing Table

The SLR methods begins with LR(0) items and LR(0) automata

We augment G to produce G’ with a new start symbol S’

From G’, we construct C, the canonical collection of items for G’ together with the GOTO function

The ACTION and GOTO entries in the parsing table are then constructed, using the FOLLOW(A) for each nonterminal A of the grammar

1. Construct C = { I0, I1, …., In}, the collection of sets of LR(0) items for G’
2. State i is constructed from Ii. The parsing actions for state Ii are determined as follows:
    1. If [A → a . aB] is in Ii and GOTO(Ii, a) = Ij, then set ACTION[i, a] to “shift j”. Here a must be 
    2. If [A → a. ] is in Ii, then set ACTION[i, a] to “reduce A → a” for all a in FOLLOW(A)
    3. If [S’ → S.] is in Ii, then set ACTION[i, $] to “Accept”

If any conflicting actions arise from the above rules, we say the grammar is not SLR(1)

1. The goto transitions for state i are constructed for all nonterminals A using the rule: If GOTO(Ii, A) = Ij, then GOTO[i, A] = j
2. All entries not defined by 2 and 3 are made “error”
3. The initial state of the parser is the one constructed from the set of items containing [S’ → . S]

## Viable Prefixes

The LR(0) automaton for a grammar characterises the strings of grammar symbols that can appear on the stack of a shift-reduce parser for the grammar

The stack contents must be a prefix of a right-sentential form

Not all prefixes of right-sentential forms can appear on the stack, since the parser must not shift past the handle

The prefixes that can appear on the stack are called viable prefixes

SLR parsing is based on the fact that LR(0) automata recognise viable prefixes