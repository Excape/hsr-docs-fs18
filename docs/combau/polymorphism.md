# Typ-Polymorphismus

## Review letzte Woche
- Type-Tag wird benötigt für Dynamic Dispatch (vtables), Garbage Collector, Interpreter (type safety), type tests/casts

## Single Inheritance

- Zur Einfachheit nur Einfachvererbung

### Code Reuse
- Subklassen erben transitiv die Felder und Methoden der Basisklasse
    - Memory Layout: Fields der Basisklasse zuerst, damit der Field index bei Polymorphismus gleich bleibt
- Hiding: Variablen in Subklasse überdecken Variablen in Basisklasse
    - Wird statisch aufgelöst

### Typ-Polymorphismus
- Subtyp ist auf Basistyp zuweisungskompatibel
- Typ wird dynamisch bestimmt anhand des Type-Tags
- Vererbung kann beliebig tief verschachtelt sein
    - Ineffizient, die ganze Vererbungskette durch zu loopen für einen Check
- Es braucht eine fixe Struktur, in der die Vererbungshierarchie abgebildet wird, um den Check in konstanter Zeit zu machen

#### Effiziente Codierung Vererbung
- Jeder Typ-Deskriptor hat eine Tabelle der Basistypen in verschiedenen Stufen
- Die letzte Stufe zeigt auf sich selbst
- Die Vererbungshierarchien sind fest zu Compile-Time, daher weiss man direkt, in welcher Stufe ein Basistyp sein muss
- Unnötige Type-Casts können vom Compiler direkt aufgelöst werden
- Type-Tests bei `null` evaluiert zu `false`
- Type-Cast von `null` ist erfolgreich


## Virtuelle Methoden / Dynamic Dispatch
- Beim Typ-Deskriptor wird eine Virtual Table hinterlegt
- Jeder Eintrag zeigt auf die konkrete Methode, entweder von sich selbst oder von einer Basisklasse
- Kann vom Compiler aufgelöst werden (bzw. bei uns vom Loader)
- Methoden der Basisklasse zuerst in vtable, dann stimmt die Reihenfolge bei Dynamic Dispatch direkt
