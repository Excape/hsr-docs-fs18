# Just-In-Time Compiler

- Ausführung direkt auf Hardware (Intel 64) statt auf .NET Runtime
- nur kritische Teile, die oft ausgeführt werden
- Profiling: Analyse zur Laufzeit, wie viel Codeausschnitte ausgeführt werde, um "Hot Spots" zu finden
    - Auf ebene Branches analysieren / zählen

## Intel 64 Architektur
- Benutzt Register (statt Stack)
- `8[RCX]`: Lies von Adresse `RCX + 8` (Von Memory in Register laden)
- Signed und Unsigned Integer Addition / Subtraktion sind die gleichen (wegen 2er Komplement)
- Division und Modulo mit `IDIV`
    - Bei jeder Division wird von `RAX` als Dividend gelesen
    - `RDX` muss 0 sein
    - Das Argument ist der Divisor
    - `RAX = RAX / Argument`
    - `RDX = RAX & Argument`

## Code Generation
- Template-based: Codemuster aus IL erkennen (z.B. für jede Instruktion) und daraus Intel64-Code generieren

## Register Allocation
- In den "Templates" muss man wissen, in welchen Register was liegt und in welche was geschrieben werden soll
- Dazu ein Register-Stack führen, der wie Evaluation-Stack verläuft
- Statt beim Eval-Stack die Werte selbst, wird im Register-Stack geschrieben, in welchen Registern die Werte liegen
- Bei Division braucht es das `RDX`-Register, und im `RAX` muss der Dividend stehen
    - `RAX` und `RDX` temporär in freie Register kopieren

## Branches
- Bei conditional jumps immer zuerst `CMP`, dann `JE`, `JNE` usw.