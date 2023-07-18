# More Powerful LR Parsers

Extend LR parsing techniques to use one symbol of lookahead

1. Canonical LR, which makes full use of the lookahead symbols. Uses a large set of items, called the LR(1) items
2. The lookahead LR or LALR method, based on LR(0) and has many fewer states than typical parsers based on the LR(1) items. By carefully introducing lookaheads into the LR(0) items, we can handle many more grammars with the LALR method than SLR and build parsing tables that are no bigger than SLR. LALR is the method of choice in most cases

## Canonical LR(1) Items

It is possible to carry more info in states that allow us to rule out invalid reductions

The extra information is incorporated into the state by redefining items to include a terminal symbol as a second component

[A → a . B, a] where A → aB is a production and a is a terminal or the endmarker $

This is an LR(1) item

The lookahead has no effect on an item such as [A → a . B, a] where B is not empty

but an item of the form [A → a. , a] calls for a reduction by A → a only if the next input symbol is a

The set of such items a will always be a subset of FOLLOW(A)