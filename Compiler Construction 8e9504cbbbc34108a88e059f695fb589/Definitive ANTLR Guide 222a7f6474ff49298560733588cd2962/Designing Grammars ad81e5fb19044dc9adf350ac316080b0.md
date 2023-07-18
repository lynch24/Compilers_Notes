# Designing Grammars

### Drawing the line between parser and lexer

- Match and discard anything in the lexer that the parser doesn’t need to see at all e.g whitespace and comments
- Otherwise the parser would have to constantly check to see whether there are comments or whitespace in between tokens
- Match common tokens like identifiers, keywords, strings in the lexer. The parser has more overhead than the lexer