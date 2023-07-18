# Lexical Analysis

A key issue for scanners is speed. Scanners can be automatically generated using regex and finite automata

Doesn’t parse for ambiguity i.e CHAR STAR vs. STAR

Regex

Symbol

Alternation

Concatenation

Epsilon

Repetition

Kleene closure binds tighter than concat, and concat binds tighter than alternation

Disambiguation rules

Longest Match

Rule priority: In the case where the longest initial substring is matched by multiple regex, the first regex that matches determines the token type