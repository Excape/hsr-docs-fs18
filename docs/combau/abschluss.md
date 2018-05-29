# Abschluss

## Offene Themen

### Statische Code-Analyse
- Beispiel: Lesen uninitialisierter Variablen
    - `c` ist sicher nicht initialisiert, `b` abhängig vom `if`
    - Evtl. ist aber die `if`-Bedingung immer gegeben. Problem ist nicht absolut entscheidbar
- Konservativ behandeln: Programmpfade analysieren, die möglich sind
- Nächstes Beispiel: `c` ist sicher ` = 3`
- Wieder Pfade anschauen. Wenn eine Variable auf allen Pfaden den gleichen Wert hat, ist dieser Konstant -> optimierbar
- Mit `while` ist `c` nicht mehr konstant
- Pfad schliesst sich wieder, es muss durchiteriert werden, bis keine Änderungen mehr passieren

### Single Static Assignment
- Bei Wiederverwendung von Variablen
- Optimierung: Jede Variable wird genau einmal zugewiesen (Variablen "indexieren")
- Problematisch bei Verzweigungen
- Die \(\phi\)-Funktion entscheidet, welcher Pfad genommen wird
- Wenn einmal SSA durchgeführt ist, kann man einfach "Dead Code" erkennen, wenn Variablen nie gelesen werden

### Common Subexpression Elimination
- Wiederholt ausgewertete Teilausdrücke identifizieren
- Diese in eine temp-Variable / oder besser Register vorberechnen
- Basiert ebenfalls auf SSA

### Partial Redundancy Elimination
- Doppelter / Redundanter Code erkennen
- In allen Codepfaden den Code duplizieren, in allen Fällen wird dann der Ausdruck nur einmal ausgewertet

### Peephole Optimization
- Ein "Fenster" von z.B. 3 Instruktion über den IL-Code laufen lassen
- Optimierungen innerhalb dieses "Gucklochs"
- Konstante Expressions können so direkt ausgewertet werten
- Laufzeit ist voraussehbar anhand der Grösse des Peepholes