# Assignment 1 - A lexical and syntax analyser

Implement a lexical and syntax analyser for CAL

## CAL Overview

Not case sensitive, a nonterminal X is represented as <X>

A terminal is represented without angle brackets

A bold typeface is used to represent terminal symbols in the language and reserved words, whereas a non-bold typeface is used for symbols that are used to group terminals and nonterminals

Source code should be kept in files with a .cal extension

## Syntax

Reserved words:

variable, constant, return, integer, boolean, void, main, if, else, true, false, while, begin, end, is, skip

Tokens:

, ; : := ( ) + - ~ | & = ! = < ≤ > ≥

Integers are a string of one or more digits 0-9 and may start with - 

Numbers may not start with a leading zero

Identifiers cannot be a reserved word

A string of letters, digits or underscores beginning with a letter

Comments can appear between any two tokens, /* */ nested, // delimited by end of line

Need to add comments both nested and single line