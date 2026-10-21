# Span-based Classifier for Short Answer Scoring and Beyond

Dieses Repository enthält die Arbeit als PDF (deutsche und englische Fassung) und die Ergebnisse **aller 273 Trainingsläufe**, auf denen Kapitel 5 beruht.

## Inhalt

```
main.pdf                 Die Masterarbeit (deutsche Fassung)
main_en.pdf              Die Masterarbeit (englische Fassung)
results/
├── alle_metriken.csv    QWK, Macro-F1 und Accuracy aller Läufe und Splits in einer Tabelle
├── asap/                 59 Läufe, ASAP-SAS, inkl. 15 Transferläufe (README mit Erklärung jedes Laufs)
├── beetle/               97 Läufe, Beetle (README mit Erklärung jedes Laufs)
├── scientsbank/          99 Läufe, SciEntsBank (README mit Erklärung jedes Laufs)
├── stance/               18 Läufe, Stance Detection, SemEval-2016 Task 6 (RQ3)
└── logs/                 Trainingslogs und Softwareversionen der Nachtrainings
```

## Worum es geht

Die Arbeit untersucht die automatische Bewertung kurzer Schülerantworten (Automatic Short Answer Grading, ASAG) mit Decoder-Sprachmodellen. Grundlage ist RUSPAN: Das Modell liest Antwort und Rubriktexte in einer Sequenz, extrahiert die Rubrikstufen als Spans und bewertet die Antwort durch den Abgleich mit jeder Stufe. Dadurch kann dasselbe Modell Aufgaben mit unterschiedlich vielen Bewertungsstufen verarbeiten.

Kapitel 5 folgt drei Forschungsfragen. **RQ1:** Übertrifft der span-basierte Klassifikator die Vergleichsbedingungen? **RQ2:** Lassen sich Decoder-only-Sprachmodelle an ihn anpassen? **RQ3:** Lässt er sich auf eine andere Sequenzklassifikationsaufgabe übertragen?

| Achse | Ausprägungen | Kapitel |
| --- | --- | --- |
| Scoring | Span-Alignment oder fester Klassifikationskopf | 5.3.1 |
| Verfügbarkeit der Information | Rubriktexte ja/nein × Frage und Musterlösung ja/nein | 5.3.2 |
| Encoder gegen Decoder | BERT-base, RoBERTa-base gegen Llama-3.2-1B | 5.3.3 |
| Transfer zwischen Benchmarks | Training auf ASAP-SAS, Test auf Beetle und SciEntsBank | 5.3.4 |
| Fusionsmechanismus | CONDIFF, CONCAT, DIFF, GATE | 5.4.1 |
| Bezugsgröße der Fusion | Sequenzrepräsentation oder Antwort-Span | 5.4.2 |
| Attention-Maske | kausal, anteilig oder vollständig bidirektional | 5.4.3 |
| Backbone und Lernrate | Llama-3.2-1B, Llama-3.2-3B, Mistral-7B; 5·10⁻⁵ und 2·10⁻⁴ | 5.4.4 |
| Andere Aufgabe (RQ3) | Stance Detection: Span-Alignment, fester Kopf mit und ohne Klassenbeschreibungen | 5.6 |

## Die wichtigsten Ergebnisse

1. **Entscheidend ist, welche Information das Modell erhält, nicht wie sie verarbeitet wird.** Ohne jede Aufgabeninformation verliert das Modell auf SciEntsBank bis zu 0,60 QWK und fällt auf neuen Fragen auf Zufallsniveau. Frage und Musterlösung ersetzen die Rubriktexte dabei weitgehend; unter QWK sind sie ihnen sogar überlegen, weil sie grobe Fehlbewertungen seltener machen.
2. **Gegenüber dem festen Kopf hat das Span-Alignment nur beim kleinen Backbone einen Vorteil.** Mit Llama-3.2-1B liegt es auf Beetle um 0,088 QWK vorn (auch mit acht Saaten), mit Llama-3.2-3B und Mistral-7B ist auf keinem Benchmark ein Vorteil nachweisbar.
3. **Strukturelle Übertragbarkeit ist keine inhaltliche.** Auf ASAP-SAS trainiert, lässt sich das Modell ohne Anpassung auf Beetle und SciEntsBank anwenden, erreicht dort aber auf allen drei Backbones nur Zufallsniveau und ordnet fast jede Antwort der untersten Stufe zu. Auch in umgekehrter Richtung (Training auf Beetle oder SciEntsBank, Test auf ASAP-SAS) scheitert die Übertragung.
4. **Die Ausgestaltung der Fusion ist ohne nachweisbare Wirkung** — auf allen drei Backbones. Zwischen CONDIFF, CONCAT, DIFF und GATE und zwischen den beiden Bezugsgrößen ist kein Unterschied als Effekt nachweisbar.
5. **Die Attention-Maske wirkt, aber mit wechselndem Vorzeichen.** Vollständige Bidirektionalität kostet Llama-3.2-3B auf Beetle 0,186 QWK auf neuen Fragen und bringt ihm auf SciEntsBank 0,060; auf ASAP-SAS ist sie bei allen drei Backbones ohne Wirkung. Mistral-7B verliert auf Beetle-UQ ebenfalls (0,089 QWK), gewinnt auf SciEntsBank aber nicht.
6. **Ein Größeneffekt zeigt sich nur eingeschränkt.** Bei gleicher Lernrate (5·10⁻⁵) liegt Mistral-7B vor beiden Llama-Größen. Werden die Llama-Modelle zur Gegenprobe mit 2·10⁻⁴ trainiert (drei Saaten auf Beetle und SciEntsBank), ist der Vorsprung auf Beetle nicht mehr nachweisbar, auf SciEntsBank bleibt er, am deutlichsten beim Transfer auf neue Domänen (UD). Die höhere Lernrate selbst ist nicht durchgängig günstiger.
7. **Der Evaluationssplit entscheidet über die Schlussfolgerung.** Auf bekannten Fragen sind Encoder konkurrenzfähig, auf neuen Fragen liegt Llama-3.2-1B um 0,221 QWK vor RoBERTa.
8. **Die Aufgabe wirkt stärker als jede Architekturvariante.** Auf ASAP-SAS reicht das QWK je Einzelaufgabe von 0,625 bis 0,898.
9. **Das Verfahren lässt sich auf Stance Detection übertragen (RQ3).** Auf bekannten Targets schlägt das Span-Alignment den festen Kopf mit beiden Llama-Größen (um 0,026 bzw. 0,018 Macro-F1). Auf einem ungesehenen Target wirken die Klassenbeschreibungen stärker als die Architektur: Ohne sie fällt der feste Kopf um 0,199 (1B) bzw. 0,044 Macro-F1 (3B) zurück; die Übertragung bleibt insgesamt schwach.

## Wie die Ergebnisse zu lesen sind

### Testsplits

| Split | Was neu ist | Was er misst |
| --- | --- | --- |
| **UA** (Unseen Answers) | nur die Schülerantwort | Reproduziert das Modell die Bewertungskonvention bekannter Fragen? |
| **UQ** (Unseen Questions) | die Frage, gleiche Domäne | Hat es das Bewertungsprinzip von der Einzelfrage gelöst? |
| **UD** (Unseen Domains) | Frage und Fachgebiet | Überträgt sich das Gelernte auf neue Inhalte? |

ASAP-SAS stellt nur UA bereit, Beetle UA und UQ, SciEntsBank alle drei. Bei Stance Detection entspricht UA den fünf Trainings-Targets und UQ dem ungesehenen Target *Donald Trump*. Die Transferläufe werden auf den Splits von Beetle und SciEntsBank ausgewertet; in `alle_metriken.csv` steht ihr Split deshalb als `beetle_ua`, `scientsbank_uq` usw.

### Kennzahlen

- **QWK** (Quadratic Weighted Kappa) ist die Leitmetrik. Sie bestraft eine Fehlbewertung umso stärker, je weiter sie danebenliegt.
- **Macro-F1** gewichtet alle Stufen gleich und unterscheidet nicht zwischen nahen und weiten Fehlern. Kapitel 5.5.3 prüft jeden Drei-Saat-Vergleich zusätzlich unter dieser Metrik.
- **Accuracy** ist der Anteil exakt richtiger Vorhersagen.

### Rauschgrenze

Viele Konfigurationen wurden mit drei Zufallssaaten wiederholt (114514, 1 und 2). Über 122 solcher ASAG-Zellen aus 52 Gruppen schwankt das Ergebnis allein durch die Saat um:

| Kennwert | Spannweite über drei Saaten |
| --- | --- |
| Median | 0,037 QWK |
| Mittelwert | 0,043 QWK |
| Minimum | 0,004 QWK |
| Maximum | 0,206 QWK |

Das Maximum geht auf einen einzelnen schwachen Lauf zurück (`scientsbank_bidirfull_mistral7b_seed1`, Kapitel 5.5.2); ohne diese Gruppe liegt es bei 0,182 QWK.

Ein Unterschied zwischen zwei Konfigurationen gilt als **Effekt**, wenn die Differenz der Mittelwerte die größere der beiden Spannweiten übersteigt. Überlappen die Spannweiten nicht, ist die Differenz aber kleiner, ist das ein **Hinweis**. Sonst ist der Unterschied nicht nachweisbar. Einzelläufe ohne Wiederholung sind gegen den Median von 0,037 QWK zu lesen.

Zur Prüfung dieser Grenze wurde CONDIFF mit Llama-3.2-1B auf Beetle mit fünf weiteren Saaten (3 bis 7) trainiert. Der Median der Spannweite über alle 56 möglichen Dreiergruppen (0,039 auf UA, 0,034 auf UQ) bestätigt die Rauschgrenze; eine einzelne Dreiergruppe kann die Streuung aber deutlich unterschätzen (0,023 statt 0,079 über alle acht Läufe).

Gleiche Saat und gleiche Softwareumgebung liefern byteweise identische Ergebnisse. Ein Wechsel der Bibliotheksversion bei gleicher Saat verschiebt das Ergebnis um bis zu 0,060 QWK.

## Übersicht der Läufe

| Benchmark | Llama-3.2-1B | Llama-3.2-3B | Mistral-7B | Encoder | Summe |
| --- | --- | --- | --- | --- | --- |
| [ASAP-SAS](results/asap/README.md) | 23 | 17 | 17 | 2 | **59** |
| [Beetle](results/beetle/README.md) | 41 | 29 | 22 | 5 | **97** |
| [SciEntsBank](results/scientsbank/README.md) | 33 | 33 | 25 | 8 | **99** |
| [Stance Detection](results/stance/README.md) | 9 | 9 | 0 | 0 | **18** |
| | | | | | **273** |

| Variante | ASAP | Beetle | SciEntsBank | Stance |
| --- | --- | --- | --- | --- |
| `condiff` (inkl. Encoder, Lernratenvarianten und Zusatzsaaten) | 11 | 26 | 23 | 6 |
| `concat` | 3 | 9 | 9 | 0 |
| `diff` | 0 | 3 | 3 | 0 |
| `gate` | 3 | 9 | 9 | 0 |
| `tconcat` | 3 | 5 | 5 | 0 |
| `bidir*` (alle Maskenstufen inkl. Wiederholungen `_v2`) | 24 | 25 | 25 | 0 |
| `seqclass` (alle Varianten) | **0** | **20** | **25** | 12 |
| `transfer` (beide Richtungen, liegen unter `asap/`) | 15 | 0 | 0 | 0 |
| **Summe** | **59** | **97** | **99** | **18** |

Die README jedes Benchmarks erklärt jeden einzelnen Lauf: welche Frage er beantwortet, wie er gegenüber seiner Referenz abschneidet und was die Drei-Saat-Vergleiche daraus ergeben.
