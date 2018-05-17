# Garbage Collection

## Memory Safety
- Bei uns werden Metadaten vom .NET-GC abgeräumt
- GC wird z.B. bei Webserver benötigt, wenn immer neue Objekte alloziert werden
- Explizite Freigabe (mit `delete`) führt zu Dangling Pointers (zu frühe Freigabe) und Memory Leaks (zu späte / keine Freigabe)

## Garbage Collection
- Laufzeitsystem kümmert sich um die Freigabe
- Es gibt mehrere Techniken dazu
- Garbage = Objekte, die _nicht mehr erreichbar sind_ und daher nicht mehr gebraucht werden

### Reference Counting
- Naive Variante von GC
- z.B. von Objective C implementiert, bei C++ shared pointers
- Idee: Eingehende Referenzen von einem Objekt zählen
    - Wenn die Zahl = 0 -> Ist Garbage
    - Bei Zyklischen Abhängigkeiten ist Garbage = Reference Count 0 nicht gegeben
- Updates sind teuer
    - Die Reference Counts sind "shared resources", d.h. sie müssen atomar geschrieben werden
- Zyklische Strukturen werden nie zu Garbage, da sie sich immer gegenseitig (bzw. zyklisch) referenzieren
    - = Memory Leak
    - Viele Strukturen sind zyklisch
    - Wird z.B. bei C++ mit `weak_pointer` gelöst
- Vorteil: Deallokation ist deterministisch bei letzter Dereferenzierung

### Garbage Collector
- Heap wird analysiert und Garbage freigegeben
- Heap ist ein flacher Graph
- Läuft nicht-deterministisch
- Garbage sind Nodes im Graph, die vom "Programm" nicht mehr erreichbar sind
- Laufendes Programm ist "Root-Set"
- Root-Set besteht aus allen Referenzen im aktiven Frame auf dem Call Stack

### Mark & Sweep
- Markiere alle transitiven erreichbaren Objekte
- Sweep: Lösche alle nicht markierte Objekte
- Mit Tiefensuche alle Objekte von einem Root-Objekt aus traversieren
- Problem: GC kann bei tiefer Struktur selbst viel Speicher benötigen
- Sweep: Linearer Scan über alle Blöcke im Heap
    - Nach dem Sweep die Markierung wieder resetten
- Nun gibt es Lücken im Speicher (Fragmentierung)
    - Free list führen mit freien Speicherblöcken
    - Nächstes mal: Objekte verschieben

### Detailaspekte
- Programm darf eigentlich nicht laufen während dem Mark
    - Techniken für Concurrent GC, um keine Änderungen während dem Mark zu verpassen
- Ausführungszeitpunkt
    - Spätestens, wenn der Heap voll sind
    - Oder prophylaktisch früher
- Unser GC läuft sequentiell und exklusiv (nicht async), Programm ist unterbrochen
- Root Set wird momentan vereinfacht auf das Main-Objekt

## Woche 2 - Vertiefung

### Stack Root Set
- Erkenne Referenzen auf dem Call Stack
    - Activation Frame durchscanne
- Lokale Variablen, Parameter (auch `this` pointer)
- GC kann auch während einer Auswertung laufen, d.h. Pointer im Evaluation Stack beachten!

## Finalizer
- Üblicherweise kommen Objekte nur einmal in die Finalizer-Liste, d.h. sie können sich höchstens 1 Mal "resurrecten"

## Weak References
- Nicht im Sinne von z.B. C++ Weak References, sondern für einen "richtigen" GC, d.h. Zyklen sind kein Problem
