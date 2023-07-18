# Top-Down Parsing, LL(1), LR(1) - Transforming Grammars for Parsing

A top-down parser starts with the root of the parse tree, labelled with the goal symbol of the grammar, and repeats the following steps until the fringe of the parse tree matches the input string

1. At a node labelled A, select a production A → alpha and construct the appropriate child for each symbol of alpha
2. When a terminal is added to the fringe that doesn’t match the input string, backtrack
3. Find the next node to be expanded(must be a nonterminal, Vn)

### Left Recursion & Eliminating Left Recursion

Topdown parsers cannot handle left recursion in a grammar

Formally,  a grammar is left recursive if

A is a nonterm such that A →+ Aalpha for some string alpha

To remove left recursion, we can transform the grammar

A ::= Aalpha

> beta
> 

where alpha and beta do not start with A

We can rewrite this as

A ::= betaA’

A’ ::= alphaA’

> empty
> 

Where A’ is a new nonterminal

### Lookahead

By looking ahead into the input string, we can use the information on the upcoming tokens to select the right production rule

We need arbitrary lookahead to parse a CFG but there is a large number of subclasses of CFGs, including most programming language constructs, that can be parsed with limited lookahead

LL(1): Left to right scan, left-most derivation, 1-token lookahead

LR(1): Left to right scan, right-most derivation, 1-token lookahead

### Predictive Parsing

For any two productions A → alpha | beta, we would like a distinct way of choosing the correct production to expand

For some right hand side alpha element of grammar G, FIRST(alpha) is the set of tokens that appear first in some string derived from alpha

That is, for some w element of Vt*, w is an element of FIRST(alpha) iff. alpha → w gamma

Where gamma represents any number of other tokens

Whenever two productions A → alpha and A → beta both appear in the grammar, we would like

FIRST(alpha) intersection FIRST(beta) = null

This would allow the parser to make the correct choice with a lookahead of 1 symbol

### Left Factoring(Transforming a grammar to allow for predictive parsing)

What if a grammar doesn’t have this key property?

Sometimes, we can transform a grammar to have this property

- For each nonterm A, find the longest prefix alpha common to two or more of its alternatives
- If alpha is not empty, replace all of the A productions
- A → alpha beta1 | alpha beta2|…|alpha betan,
- with
- A → alphaA’
- A’ → beta1|beta2|…|betan
- Where A’ is a new nonterm
- Repeat until no two alternatives exist for a single nonterm have a common prefix

.eg

<expr> ::= <term> + <expr> | <term> - <expr> | <term>

Here <term> is common

<expr> ::= <term> <expr’>

<expr’> ::= + <expr> | - <expr> | empty

Now selection requires only a single token lookahead, but it is still right associative

### Eliminating Indirect Left Recursion *** Needs further reading

Left recursion can be hidden by indirect references

i.e A0 → A1alpha1 → A2alpha2alpha1 → A0alpha2

Which is left recursive

If the grammar has no cycles i.e there is no derivation of the form B →* B, for any nonterminal B, and there are no empty productions, then we can apply the following algorithm to eliminate left recursion

- Arrange all the nonterms into some arbitrary order, say A1, A2, A3 etc.
- For each nonterminal Ai in turn, do:
    - For each Ai such that 1 ≤ j < i and there is a production rule of the form

## FIRST

For a string of grammar symbols alpha, we define FIRST(alpha) as:

The set of terminal symbols that begin strings derived from alpha: { a element Vt | a →* alpha beta}

If alpha derives empty, then empty is an element of FIRST(alpha)

![Untitled](Top-Down%20Parsing,%20LL(1),%20LR(1)%20-%20Transforming%20Gram%207370fc09bb524c8f97f4fd5efde6b3b2/Untitled.png)

FIRST(XYZ) = {a, c, d}

Symbols that can derive the empty string, are called nullable and we must keep track of what can follow a nullable symbol

To compute FIRST(X) for all grammar symbol X, apply the following rules until no more terminals or empty can be added to any FIRST set:

1. If X is a terminal, then FIRST(X) = {X}
2. If X is a nonterminal and X → Y1, Y2 … Yk is a production rule for some k ≥ 1, then add a to FIRST(X) if for some i, a is an element of FIRST(Yi) and Y1 to Yi-1 are nullable. If all productions are nullable, add empty to FIRST(X). Basically, add the first part of the first nonterminal of the production that isn’t empty, if there are none, just add empty
3. If X → empty is a production rule, then add empty to FIRST(X)

## FOLLOW

FOLLOW(X) is the set of terminals that can immediately follow X

t is an element of FOLLOW(X) if there is any derivation containing Xt

This would include any derivation XYZt where Y and Z are nullable

To compute FOLLOW(A) for all nonterminals A, apply the following rules

1. Place $ in FOLLOW(S) where S is the start symbol and $ in the end of input marker
2. If there is a production rule A → alphaBbeta, then everything in FIRST(beta), except empty, is in FOLLOW(B)
3. If there is a production rule A → alphaB or a production rule A → alphaBbeta where FIRST(beta) contains empty, then everything in FOLLOW(A) is in FOLLOW(B) - (as beta is empty, B is practically the end of the production, so anything that follows B effectively follows the whole rule for A)

## LOOKAHEAD

For a production rule A → alpha, we define LOOKAHEAD(A → alpha) as the set of terminals which can appear next in the input when recognising production rule A → alpha

Thus, a production rule’s LOOKAHEAD set specifies the tokens which should appear next in the input before the production rule is applied

To build LOOKAHEAD(A→ alpha):

1. Put FIRST(alpha) - (empty) in LOOKAHEAD(A→alpha)
2. If empty is an element of FIRST(alpha), then put FOLLOW(A) in LOOKAHEAD(A→alpha)

A grammar G is LL(1) iff for each set of productions A → alpha1|alpha2|…|alphan:

LOOKAHEAD(A→alpha1), LOOKAHEAD(A→alpha2) etc. are all pairwise disjoint

## LL(1) Grammars

No left recursive grammar is LL(1)

No ambiguous grammar is LL(1)

Some languages have no LL(1) grammar

An empty-free grammar where each alternative expansion for A begins with a distinct terminal is a simple LL(1) grammar