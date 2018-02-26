# Lexer

## Trailing else problem
- Nicht eindeutige Syntax
- Java umgeht das mit separat definiertem "StatementNoShortIf"

## Lexikalische Analyse
- Analyse von Terminalsymbolen zu Tokens
    - Token = "Terminalsymbol"
- Von Charakter-Stream zu Token-Stream
- Fasst mehrere Textzeichen zusammen zu Tokens
- Eliminiert Whitespaces und Code-Kommentare
    - Whitespace ist aber relevant, um Tokens zu trennen
    - Kommentare können z.B. für Refactoring-Tools relevant sein
- Merkt Position von Tokens im Code für Fehlermeldung und Debugging

### Nutzer für Parser
- Parser arbeitet nur mit Tokens
- Lookahead pro Symbol (nur nächstes Symbol)

### Tokens
- Keywords sind reservierte Begriffe
- Interpunktionen wie `;`, `{`, `]` etc.
- Identifiers sind "custom" tokens wie Variablennamen (die keine keywords sein dürfen)
- Reservierte typennamen und Werte (true, false, ...) nicht als fixe Tokens scannen, sondern als Identifier

## Reguläre Sprachen
- Scanner / Lexer versteht nur reguläre Sprachen
- in EBNF ohne Rekursion möglich = regulär
    - = als eine EBNF-Regel formulierbar
- `Ìnteger = Digit [ Integer ]` ist rekursiv, kann aber regulär formuliert werden
    - als `Integer = Digit { Digit }.`
    - Syntax äquivalent
- Scanner kann reguläre Sprachen, Parser kontextfreie Sprachen
    - Lexer ist endlicher Automat
- kontextsensitive Sprache wird begrenzt durch Semantic Checker abgedeckt

## Maximum Munch
- Scanner absorbiert immer möglichst viel
    - a.k.a. Regex "greedy evaluation"
- Hört bei whitespace auf

## Kommentare
- Nicht schachtelbar -> reguläre Ausdrücke

## Tips für Scanner
- Overflow in Integers?
    - Achtung: Min-value ist 1 "grösser" als max value!
    - im Lexer nicht auflösbar, 1 zu gross erlauben
- Negative Zahlen als fix Token `-` und Integer scannen
- String-Scanner:
    - Escaping beachten
    - Newline beachten, String terminiert?
- Ein `/` kann ein Kommentar oder ein Divide-Tag sein
- Zeile / Position mitführen (Position alleine reicht)

## Fehlerbehandlung
- Error-Tokens zurück liefern, damit nicht direkt beim ersten Fehler abgebrochen wird
    - Braucht aber eine Korrektur