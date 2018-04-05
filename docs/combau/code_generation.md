# Code Generierung

## Optimization
- Zwischendarstellung (AST) transformieren für effizienteren Code
- z.B konstante Expressions, leere Statement-Blöcke, etc.
- Wird bei uns noch ausgelassen
- Kann auch später, z.B. im JIT geschehen

## Intermediate Language
- Ziel bei uns: Erzeugung von Code für eine VM
- Zwischendarstellung ist die Schnittstelle zum "Backend", in diesem Fall eine Runtime
    - Mehrere Frontends: Multi-Language
    - Mehrere Backends: Multi-Platform

### Unsere Zielmaschine
- Subset von .NET IL
- Virtueller Stack-Prozessor
    - Keine Register
- "goto"-machine
    - Keine loops, nur "flacher" Code mit Branches
- Metadaten: Was sind unsere Klassen, Variablen, Typen, etc.?
    - U.a. für Linking, Querreferenzen von anderen Libraries / Binaries
    - Referenzen für GC 

## Virtual Stack Machine
- Stack in der Prozedur
- Operanden lesen (pop) und Ergebnis schreiben (push)
    - Erster Pop ist right-hand-side der Operation
- Jede Instruktion hat gewisse Anzahl Pops und Pushes
- Stack ist vor und nach der Methode "leer"
    - Kann auch alles in einem Stack ausgeführt werden
- Lokale Variablen / Parameter werden durchnummeriert
- Beispiel Auswertung: `x = x * (7 + 11)`
- Im Unterschied zu Java / .NET haben wir konkrete Boolean-Werte
    - Sonst wären es 0/1 integers
    - `and` und `or`-Operators müssen mit Branches nachgebaut werden (äquivalent zu if-else)
- Beispiel Boolean: `b = !a`

## Kontrollfluss
- Branch to Label Instruktionen
- Conditional Instruktionen (z.B. `brfalse`), um zu anderen Branches zu springen

## IL-Generierung
- Builder Pattern
- Serialisierung in XML (bei uns, für Komfort)
- Zuerst Metadaten mit Symboltabelle erstellen
- AST Traversieren mit Visitor und Instruktionen erzeugen
- Labels werden zuerst erstellt (`CreateLabel()`) und erst später an den IL-Code "attached" (`SetLabel()`)
- Labels werden am Schluss übersetzt in relative Sprungstellen (in Anzahl Instruktionen)

### Code-Templates
- Mit Pattern-Matching Muster im AST erkennen
- z.B. wenn `+` kommt, generieren wir `add`
- Post-Order-Traversierung und jeweils Pattern erkennen
- Kleine Optimierungen möglich
- Pattern-Erkennung schwieriger, z.B. um `if` und `if-else` zu unterscheiden
- Binäre Operatoren mit Booleans als `if-else` behandeln