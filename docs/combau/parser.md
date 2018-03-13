# Parser

## Quiz letzte Woche
- Lexer liefert nur den regulären Anteil
    - Keine rekursive Syntax-Elemente, bzw. Elemente, die _nur rekursiv ausdruckbar sind

## Top-Down Parser
- Input: Terminalsymbole, Output: Syntaxbaum
- Kann nur kontextfreie Sprache
    - z.B. nicht, ob Variable deklariert ist, oder Parameter auf Argumente passen, type checking
    - Diese sind kontextabhängig
- Der Parser muss eine Ableitung der Syntaxregeln finden, um einen gegebenen Input herzuleiten
- Quasi eine Ableitung rückwärts, von Token zu einer Regel
- Diese Woche: Syntax-Check, nächste Woche Syntaxbaum

## Syntax-Baum
- Konkreter Syntax-Baum (Parse Tree) befolgt die Syntax genau
- Abstract Syntax-Tree (AST) kann unwichtige Details auslassen
    - z.B. Klammern weg lassen
- AST kann nicht automatisch erzeugt werden, "nach Gusto" des Entwicklers
- Selber implementierte Parser können direkt AST liefern

## Top-Down vs. Bottom-Up
- Top-Down: Zuerst "linke Seite" der Regel anwenden, bis Eingabetext rauskommt
    - Immer von links her auflösen
- Bottom-Up: Mit Eingabetext beginnen, und Regeln darauf anwenden, bis man zum "Startsymbol" kommt
    - Umgekehrt zum Top-Down Parsing

## Recursive Decent
- Wir nutzen den Stack der Methodenaufrufe als "Push-Down Automat"
- Pro Nicht-terminalsymbol eine Methode
- Wenn ein Nicht-Terminalsymbol in Syntax (rechte Seite) vorkommt, wird die entsprechende Methode aufgerufen
- Methoden können sich gegenseitig rekursiv aufrufen
- Gibt keine Entscheidungsprobleme bei Top-Down (zielorientiert)
    - Anderer Ansatz, wenn das nicht geht: Produktion ausprobieren und bei Fehler "backtracken" (exponentielle Laufzeit)

## Tools und Implementation
- One Token lookahead reicht aus
- Momentan geben die Methoden `void` zurück, wo später ein Syntaxbaum erstellt wird
    - Nur Syntax-Check
- Für nicht-terminal-Symbole braucht es z.T. ein weiteres Lookahead
    - Mögliche Terminalsymbole erfassen, die in einem nicht-terminal-symbol als erstes vorkommen können: `FIRST` Set
    - FIRST-Sets können Vereinigungen von "untergeordneten" NT-Symbolen haben
    - Wenn zwei NT-Symbole nicht-disjunkte FIRST-Mengen haben, reicht ein One-Token-Lookahead nicht mehr aus, da nicht entschieden werden kann
        - Braucht weiteres Lookahead, dann irgendwann drei, etc.
        - Stattdessen Syntax "umformen" für Parser
        - Für beide Regeln eine eigene Regeln erfassen und das Gemeinsame zusammen ziehen

### Linksrekursion
- z.B. `Sequence = Sequence [ Statement ]`
- Gibt bei Implementation bei Recursive Decent endlose Rekursion
- Bottom-Up Parser können damit umgehen
- Umschreiben nach `Sequence = { Statement }`

## Syntaxbaum
- Statement-Block ist ein Spezialfall von Statement, in anderen Sprachen ist ein Statement-Block auch ein Statement
- Achten auf Assoziativität bei Expressions
    - 

## Fehlerbehandlung
- Nur häufige Fehler korrigieren
- Basiert immer auf Hypothese, es können leicht Folgefehler entstehen
- Andere Fehler wie inkompatible Typen, falsche Argumentliste, etc. werden im Semantic Checker behandelt

## Attributierte Grammatiken
- EBNF-Grammatiken mit kontext-sensitiven Attributen ergänzen
- Semantische Checks oder Erzeugen des AST
- `Write()` ist eine Aktion und gibt lediglich etwas auf die Konsole aus
- Ausgabe Folie 22: `1 2 - 3 4 - +
    - Postfix-Notation wie bei HP Taschenrechner
- Die Aktionen werden immer zum Schluss "der Parse-Methode" ausgeführt
- Der Parser kann auch direkt statische Ausdrücke evaluieren
- Direkt Syntaxbaum erzeugen mit `new BinaryExpression(...)` etc. direkt im Attribut
- S-attributiert (synthetisiert): Attribute lesen nur Parameter von Teilregeln (rechte Seite)
- L-attributiert (Ererbt): Attribut kann auch "linke" Seite lesen
- Es kann alles in Grammatik beschrieben werden, in der Praxis ist dies aber unübersichtlich und kompliziert

## Bottom-Up Parser (LR-Parser)
- Ansatz: Tokenstream nehmen und "zusammenfalten" zu Syntaxbaum
- Nach jedem gelesenen Symbol prüfen, ob die Folge einer Regel entspricht
    - Wenn ja, "Reduce"
    - Wenn nein, das nächste Zeichen auslesen (shift)
- Am Schluss muss das Startsymbol übrig bleiben, sonst Syntaxfehler
- Es kann zu Entscheidungsschwierigkeiten kommen: "Shift-reduce" / "Reduce-Reduce" Probleme
    - Grammatik anpassen oder "von Hand"
- LR ist "mächtiger" als LL-Parser
    - z.B. Linksrekursion