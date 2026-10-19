# Vergleich von CLP(FD) und CP-SAT

Dieses Repository enthält die Implementierungen, Benchmarks und Auswertungen zu einer Seminararbeit über **Constraint Logic Programming over Finite Domains (CLP(FD)) in SWI-Prolog** und den **CP-SAT-Solver von Google OR-Tools**.

Ziel der Arbeit ist es, die beiden Constraint-Systeme hinsichtlich ihrer internen Modellverarbeitung, Such- und Propagationsverfahren sowie ihres Monotonieverhaltens gegenüberzustellen. Ergänzend wird ihre Laufzeit anhand zweier kombinatorischer Probleme untersucht.

## Untersuchte Systeme

- **SWI-Prolog CLP(FD):** Constraint-Programmierung über ganzzahligen Domänen mit Propagation, Labeling und Backtracking.
- **Google OR-Tools CP-SAT:** Hybrider Solver, der unter anderem Constraint-Propagation, SAT-basierte Konfliktanalyse und parallele Suchstrategien kombiniert.

Der theoretische Vergleich behandelt insbesondere Unterschiede in der internen Repräsentation der Modelle, der Durchführung der Suche und der Behandlung von Monotonie.

## Benchmarks

Für die experimentelle Gegenüberstellung werden zwei Probleme verwendet:

| Problem | Untersuchungsgegenstand |
| --- | --- |
| **N-Queens** | Skalierung der Laufzeit bei steigender Problemgröße (`N = 25` bis `50`) |
| **7-11** | Verarbeitung eines kleinen Modells mit kombinierten Summen- und Produktbedingungen |

CLP(FD) wird mit der **First-Fail-Strategie** ausgeführt. Für CP-SAT werden drei Varianten betrachtet:

1. Eine explizit nachgebildete First-Fail-Strategie mit einem Worker.
2. Die standardmäßige Suchstrategie mit einem Worker.
3. Die standardmäßige Suchstrategie mit mehreren Workern.

Die CP-SAT-Variante mit vorgegebener Strategie bildet die Variablen- und Wertauswahl von First-Fail näherungsweise ab. Aufgrund der weiteren Suchmechanismen von CP-SAT ist sie nicht mit dem Suchverfahren von CLP(FD) gleichzusetzen.

Jede Benchmarkkonfiguration wird **100-mal** gemessen, mit **fünf vorangehenden Warm-up-Durchläufen**. Dokumentiert werden die Mittelwerte und Standardabweichungen der gemessenen CPU-Zeiten. Bei der Interpretation der Multithreading-Ergebnisse ist zu beachten, dass **CPU-Zeit nicht mit verstrichener Laufzeit (Wall-Clock-Zeit) gleichzusetzen ist**.

### Verwendete Umgebung

Die veröffentlichten Messungen wurden unter folgenden Bedingungen durchgeführt:

| Komponente | Konfiguration |
| --- | --- |
| Prozessor | AMD Ryzen 7 5700X (8 Kerne / 16 Threads) |
| Arbeitsspeicher | 32 GB DDR4-3666 |
| Betriebssystem | Windows 11 Pro (23H2, x64) |
| Python | 3.11 |
| OR-Tools | 9.12.4544 |
| SWI-Prolog | 9.2.9 |

Ein fester Zufalls-Seed wurde für CP-SAT verwendet. Messergebnisse können dennoch je nach Hardware, Betriebssystem, Solver-Version und Laufzeitumgebung abweichen.

## Ergebnisse

Beim **N-Queens-Problem** erzielte CP-SAT in den untersuchten Konfigurationen überwiegend kürzere und gleichmäßigere Laufzeiten als CLP(FD). Insbesondere die Variante mit mehreren Workern zeigte eine vergleichsweise stabile Laufzeitentwicklung bei steigender Problemgröße.

Beim **7-11-Problem** fiel das Ergebnis differenzierter aus: CP-SAT war mit der nachgebildeten First-Fail-Strategie am schnellsten, während CLP(FD) gegenüber den beiden CP-SAT-Varianten ohne explizite Suchstrategie besser abschnitt.

Die Ergebnisse zeigen damit vor allem die Bedeutung von **Problemstruktur und Solver-Konfiguration**. Aus den beiden Benchmarks lässt sich keine allgemeine Überlegenheit eines Solvers für beliebige Constraint-Probleme ableiten.

## Repository-Struktur

```text
.
├── 711/             # Messdaten und Auswertung zum 7-11-Problem
├── NQueens/         # Messdaten und Auswertung zum N-Queens-Problem
├── Diagramme/       # Erzeugte Diagramme
├── Quellcode/       # Implementierungen der untersuchten Probleme
└── main.pdf         # Die Seminararbeit als PDF```

Die Benchmarkverzeichnisse enthalten die jeweiligen Messdaten sowie ein Skript `data.py` zur Auswertung und Diagrammerzeugung. Über `performance.py` lassen sich die Benchmarks konfigurieren und ausführen; dabei werden die CLP(FD)- und CP-SAT-Implementierungen aufgerufen und ihre Ausgaben in Textdateien gespeichert.

**Hinweis:** Für eine erneute Ausführung müssen Python mit Google OR-Tools sowie SWI-Prolog verfügbar sein. Die konkreten Aufrufparameter und gegebenenfalls verwendeten Dateipfade sind in den Skripten zu prüfen. Die veröffentlichten Messergebnisse beziehen sich auf die oben genannte Umgebung.

## Einordnung

Die Arbeit ist eine **vergleichende Seminararbeit**, keine umfassende Solver-Benchmark-Suite. Untersucht werden zwei exemplarische Probleme und eine begrenzte Anzahl von Suchkonfigurationen. Für weitergehende Aussagen wären zusätzliche Instanzen, weitere Problemklassen (z. B. Scheduling und Routing) sowie ergänzende Messgrößen erforderlich.
