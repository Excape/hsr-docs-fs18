# Virtual Machine

## Virtual Machine
- Hypothetische Maschine, die die IL versteht
- "Virtueller Prozessor" mit IL als Instruktionssatz
- Bietet Mehrsprachigkeit, Multi Platform
- Trennt Compiler von Laufzeit

## Loader
- Lädt IL in Speicher
- Alloziert Speicher
- Address Relocation
    - Adressen müssen "gefixt" werden, weil Speicher nacheinander alloziert wird
- Verifier: Statische Analyse des Codes
- Initiiert Ausführung auf Interpreter / JIT
- IL-Code wird direkt in Speicher geladen
    - Methodenaufrufe werden aufgelöst mit Deskriptoren ("Fixup")

### Metadaten
- Typ-Deskriptoren
    - Ähnlich Symboltabelle
    - Beschreiben die Subtypen von Typen
    - z.B. Field types von Klassen


## Interpreter
- Interpretiert den IL-Code
- Produziert Code für Hardware-Ausführung oder für JIT
- Der Prozess wird unterstützt von Metadaten, Heap und Stacks
- Interpreter-Loop emuliert eine Instruktion nach der anderen
- Instruction Pointer kann vor- oder zurück springen (z.B. bei Branches)


## Call Stack
- Stack von Activation Frames
    - Activation Frame ist der "Datenraum" einer Methode
    - In jedem Activation Frame befindet sich ein Evaluation Stack
- Bei uns gibt es nur einen Call-Stack (single-threaded)
- Statische Methoden bei uns gehen auf .NET Call-Stack

### Method-Call
- Noch überprüfen, ob die Methode auch auf dem richtigen Objekt aufgerufen wird

### Method Return
- Rückgabetyp prüfen
- Prüfen, ob Stack leer ist nach Return
