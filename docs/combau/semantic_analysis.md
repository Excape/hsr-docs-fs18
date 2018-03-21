# Semantische Analyse

## Quiz
- Ist syntaktisch korrekt, weil ein `if` als condition eine Expression erwartet
- Aber semantisch falsch, bool nicht addierbar etc.
    - Dies sind *kontextsensitive* Regeln

## Semantic Checker
- Input: AST oder konkreter Syntaxtree
- Output: AST + Symboltabelle
- Prüfen, dass das Programm gemäss Sprachregeln "Sinn macht"
    - Deklarationen prüfen (eindeutig und nicht mehrfach)
    - Typen (Typregeln sind erfüllt)
    - Method-Calls (Argumente und Parameter sind kompatibel)
    - Und mehr...
- Transformiere Programm, so dass Code Generation einfach wird
    - Syntaxbaum umstellen
    - z.B. Vereinheitlichen von verschiedenen Loops, z.B. aus for- einen while-loop
    - Array Boundary Check
    - Implizite Casts
    - usw.

## Symboltabelle
- Datenstruktur zur Verwaltung von Deklarationen
- Zu jedem Namen ist der Scope angegeben
- Scopes können rekursiv sein, z.B. Methode -> Methode etc.
- Scoping auch wichtig bei z.B. loops

### Shadowing
- Überdecken von Deklarationen in äusserem Scope durch Defintion in innerem Scope
- In Rappi-sharp möglich
- Unterschied zu "Hiding": Beim Hiding verdecken Methoden/Variablen in einer Unterklasse die Deklaration in der Superklasse
- Shadowing in 

### Global Scope
- Es gibt einen global Scope
    - Bei uns können dort nur Klassen vorkommen
- Keywords wie `true`, `int`, `bool` können auch dort definiert werden

### Design der Symboltabelle
- `CompilationUnit` ist der "global" Scope
- Im ersten Schritt werden die Deklarationen aufgelöst, erste danach werden Typen aufgelöst
    - Weil während dem Durchlaufen Typen erst später deklariert werden könnten
- Zu einer lokalen Variable wird dazu gespeichert, in welchen Statements die Variable "lebt"
- Spätestens jetzt muss geprüft werden, dass `int`, `bool` etc nicht als Identifier verwendet werden!
- `null` muss speziell behandelt werden
- Es gibt ein Dictionary, das Symbole zu Knoten-Referenzen mappt.


### Vorgehen
1. Symboltabelle aufbauen
2. Typen auflösen
3. AST durchlaufen und mit Symboltabelle verbinden
    - Alle Vorkommenden Identifier verlinken mit entsprechendem Symbol 
4. (Unsere Aufgabe) Typen in AST bestimmen

## Namensauflösung
- z.B. Typen auflösen
- Zuerst suche im innersten Scope, dann im nächst äusseren Scope, usw. rekursiv
- Suche mit `Find` Method auf Symboltabelle

## Type Checking

!!! important
    Wichtig für Übung: Folie 32, Folie 36 Punkte 2, 4-7, 9, Folien 38-42

- Für jede Expression den Typ bestimmen
- Insbesondere Binary/Binary Expressions: Typen checken
- Auch z.B. bei `if`: Ist condition ein bool?

### Ablauf
- Post-Order-Traversierung
    - Zuerst child-nodes auflösen
- AST nicht verändern, sondern z.B. Symboltabelle `FixType()` aufrufen
- Nach dem Auflösen folgt das Checking
    - z.B. Array length read-only, noch selbst implementieren
- Bei Zuweisungen: Prüfen, ob rechte und linke Seite gleichen Typ haben
    - Ausser bei Polymorphismus und `null`
    - Gleiches Prinzip bei Methoden-Aufrufen

## Statische Analyse
- Checks wie Array-Bounds sind nicht allgemein entscheidbar
    - Kann vom Compiler transformiert werden, z.B. neue Statements einfügen
- Muss bei einer Runtime aber sowieso überprüft werden (Sicherheit), deshalb lohnt es sich hier nicht