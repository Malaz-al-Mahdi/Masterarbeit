# Span-based Classifier for Short Answer Scoring and Beyond

Dieses Repository enthält die Arbeit als PDF und die Ergebnisse **aller 166 Trainingsläufe**, auf denen Kapitel 5 beruht.

## Inhalt

```
main.pdf                 Die Masterarbeit
results/
├── alle_metriken.csv    QWK, Macro-F1 und Accuracy aller Läufe und Splits in einer Tabelle
├── asap/                 30 Läufe, ASAP-SAS (README mit Erklärung jedes Laufs)
├── beetle/               63 Läufe, Beetle (README mit Erklärung jedes Laufs)
└── scientsbank/          73 Läufe, SciEntsBank (README mit Erklärung jedes Laufs)
```

## Worum es geht

Die Arbeit untersucht die automatische Bewertung kurzer Schülerantworten (Automatic Short Answer Grading, ASAG) mit Decoder-Sprachmodellen. Grundlage ist RUSPAN: Das Modell liest Antwort und Rubriktexte in einer Sequenz, extrahiert die Rubrikstufen als Spans und bewertet die Antwort durch den Abgleich mit jeder Stufe. Dadurch kann dasselbe Modell Aufgaben mit unterschiedlich vielen Bewertungsstufen verarbeiten.

Untersucht wird, welche Bestandteile dieses Aufbaus die Leistung tatsächlich bestimmen:

| Achse | Ausprägungen | Kapitel |
| --- | --- | --- |
| Verfügbarkeit der Information | Rubriktexte ja/nein × Frage und Musterlösung ja/nein | 5.3 |
| Fusionsmechanismus | CONDIFF, CONCAT, DIFF, GATE | 5.4 |
| Bezugsgröße der Fusion | Sequenzrepräsentation oder Antwort-Span | 5.5 |
| Scoring | Span-Alignment oder fester Klassifikationskopf | 5.6 |
| Attention-Maske | kausal, anteilig oder vollständig bidirektional | 5.7 |
| Backbone | Llama-3.2-1B, Llama-3.2-3B, Mistral-7B, BERT-base, RoBERTa-base | 5.8, 5.9 |

## Die wichtigsten Ergebnisse

1. **Entscheidend ist, welche Information das Modell erhält, nicht wie sie verarbeitet wird.** Ohne jede Aufgabeninformation verliert das Modell auf SciEntsBank bis zu 0,53 QWK und fällt auf neuen Fragen auf Zufallsniveau. Frage und Musterlösung ersetzen die Rubriktexte dabei weitgehend; unter QWK sind sie ihnen sogar überlegen, weil sie grobe Fehlbewertungen seltener machen.
2. **Die Ausgestaltung der Fusion ist ohne nachweisbare Wirkung.** Unter QWK ist zwischen CONDIFF, CONCAT, DIFF und GATE und zwischen den beiden Bezugsgrößen der Fusion kein Unterschied als Effekt nachweisbar.
3. **Die Attention-Maske wirkt, aber mit wechselndem Vorzeichen.** Vollständige Bidirektionalität kostet Llama-3.2-3B auf Beetle 0,186 QWK auf neuen Fragen und bringt ihm auf SciEntsBank 0,060; Llama-3.2-1B gewinnt auf Beetle 0,036.
4. **Über die Modellgröße ist keine Aussage möglich.** Die kleinen Backbones liefen bei einem Viertel ihrer vorgesehenen Lernrate. Bei der passenden Rate holt Llama-3.2-1B auf Beetle den Abstand zu Mistral-7B auf neuen Fragen fast vollständig auf.
5. **Der Evaluationssplit entscheidet über die Schlussfolgerung.** Auf bekannten Fragen sind Encoder konkurrenzfähig, auf neuen Fragen liegt Llama-3.2-1B um 0,221 QWK vor RoBERTa.
6. **Die Aufgabe wirkt stärker als jede Architekturvariante.** Auf ASAP-SAS reicht das QWK je Einzelaufgabe von 0,625 bis 0,898.

## Wie die Ergebnisse zu lesen sind

### Testsplits

| Split | Was neu ist | Was er misst |
| --- | --- | --- |
| **UA** (Unseen Answers) | nur die Schülerantwort | Reproduziert das Modell die Bewertungskonvention bekannter Fragen? |
| **UQ** (Unseen Questions) | die Frage, gleiche Domäne | Hat es das Bewertungsprinzip von der Einzelfrage gelöst? |
| **UD** (Unseen Domains) | Frage und Fachgebiet | Überträgt sich das Gelernte auf neue Inhalte? |

ASAP-SAS stellt nur UA bereit, Beetle UA und UQ, SciEntsBank alle drei.

### Kennzahlen

- **QWK** (Quadratic Weighted Kappa) ist die Leitmetrik. Sie bestraft eine Fehlbewertung umso stärker, je weiter sie danebenliegt.
- **Macro-F1** gewichtet alle Stufen gleich und unterscheidet nicht zwischen nahen und weiten Fehlern. Kapitel 5.12 prüft jeden Drei-Saat-Vergleich zusätzlich unter dieser Metrik.
- **Accuracy** ist der Anteil exakt richtiger Vorhersagen.

### Rauschgrenze

Viele Konfigurationen wurden mit drei Zufallssaaten wiederholt (114514, 1 und 2). Über 72 solcher Zellen schwankt das Ergebnis allein durch die Saat um:

| Kennwert | Spannweite über drei Saaten |
| --- | --- |
| Median | 0,037 QWK |
| Mittelwert | 0,038 QWK |
| Minimum | 0,006 QWK |
| Maximum | 0,118 QWK |

Ein Unterschied zwischen zwei Konfigurationen gilt als **Effekt**, wenn die Differenz der Mittelwerte die größere der beiden Spannweiten übersteigt. Überlappen die Spannweiten nicht, ist die Differenz aber kleiner, ist das ein **Hinweis**. Sonst ist der Unterschied nicht nachweisbar. Einzelläufe ohne Wiederholung sind gegen den Median von 0,037 QWK zu lesen.

Gleiche Saat und gleiche Softwareumgebung liefern byteweise identische Ergebnisse. Ein Wechsel der Bibliotheksversion bei gleicher Saat verschiebt das Ergebnis um bis zu 0,060 QWK.

## Übersicht der Läufe

| Benchmark | Llama-3.2-1B | Llama-3.2-3B | Mistral-7B | Encoder | Summe |
| --- | --- | --- | --- | --- | --- |
| [ASAP-SAS](results/asap/README.md) | 10 | 10 | 8 | 2 | **30** |
| [Beetle](results/beetle/README.md) | 30 | 24 | 4 | 5 | **63** |
| [SciEntsBank](results/scientsbank/README.md) | 31 | 30 | 4 | 8 | **73** |
| | | | | | **166** |

| Variante | ASAP | Beetle | SciEntsBank |
| --- | --- | --- | --- |
| `condiff` (inkl. Encoder und Lernratenvarianten) | 5 | 14 | 16 |
| `concat` | 3 | 6 | 6 |
| `diff` | 0 | 3 | 3 |
| `gate` | 3 | 6 | 6 |
| `tconcat` | 2 | 4 | 4 |
| `bidir*` (alle Maskenstufen) | 17 | 19 | 19 |
| `seqclass` (alle vier Varianten) | **0** | **11** | **19** |
| **Summe** | **30** | **63** | **73** |

Die README jedes Benchmarks erklärt jeden einzelnen Lauf: welche Frage er beantwortet, wie er gegenüber seiner Referenz abschneidet und was die Drei-Saat-Vergleiche daraus ergeben.
