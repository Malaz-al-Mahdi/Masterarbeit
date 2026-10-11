# Ergebnisse: Stance Detection (SemEval-2016 Task 6)

Erkennung von Standpunkten in Tweets: Jeder Tweet bezieht Stellung zu einem *Target* und ist mit einer von drei Haltungen annotiert — **Against**, **None** oder **Favor**. Der Datensatz stammt aus SemEval-2016 Task 6 (Mohammad et al., 2016) und dient in der Arbeit der **dritten Forschungsfrage (RQ3)**: Lässt sich das Span-Alignment auf eine Klassifikationsaufgabe außerhalb von ASAG übertragen?

**Umfang:** 2.914 Trainings-Tweets zu 5 Targets (davon automatisch 2.622 Training und 292 Validierung); UA 1.249 Tweets zu denselben 5 Targets, UQ 707 Tweets zum ungesehenen Target *Donald Trump*.

Insgesamt liegen hier **9 Trainingsläufe**, alle mit Llama-3.2-1B.

## Wie die Aufgabe auf RUSPAN abgebildet wird

| Stance Detection | ASAG-Entsprechung |
| --- | --- |
| Tweet | Schülerantwort (`answer`) |
| Target | Frage (`question_id`) |
| Against / None / Favor | Stufen 0 / 1 / 2 (`level`) |
| Beschreibung jeder Haltung | Rubriktexte (`rubrics`) |
| Test auf den 5 Trainings-Targets | UA-Split |
| Test auf dem Target *Donald Trump* | UQ-Split |

Die Klassenbeschreibungen folgen einer festen Vorlage, in die nur der Name des Targets eingesetzt wird; sie sind nicht sprachmodellgeneriert:

- **Against:** *The author of the tweet opposes {target}; the tweet argues against it or supports an opposing position.*
- **None:** *The tweet expresses neither support for nor opposition to {target}, or no stance toward it can be inferred.*
- **Favor:** *The author of the tweet supports {target}; the tweet argues for it or defends it.*

Die drei Haltungen bilden keine ordinale Skala. Leitmetrik ist deshalb **Macro-F1**; ergänzend wird **F_avg** berichtet, der Mittelwert der F1-Werte von *Against* und *Favor* (die übliche Hauptmetrik der Stance Detection). QWK steht der Vollständigkeit halber in den `*_metrics.json`, ist hier aber nicht inhaltlich zu deuten.

## Was die Ergebnisse aussagen

- **Auf bekannten Targets schlägt das Span-Alignment den festen Kopf.** Um 0,026 Macro-F1 (0,749 gegenüber 0,724), unter Macro-F1, F_avg und QWK jeweils als Effekt. Das ist eindeutiger als in ASAG, wo der Vorteil nur mit Llama-3.2-1B auf Beetle bestand.
- **Auf dem ungesehenen Target entscheiden die Klassenbeschreibungen, nicht die Architektur.** Ohne Beschreibungen fällt der feste Kopf auf 0,249 Macro-F1 und ordnet 72 bis 81 % der Tweets der Klasse *None* zu (Anteil in den Referenzdaten: 37 %). Mit Beschreibungen liegt er 0,199 Macro-F1 höher (Effekt); Span-Alignment und fester Kopf liegen dann gleichauf.
- **Die Übertragung auf das neue Target bleibt schwach.** Mit Beschreibungen sagen die Modelle überwiegend *Against* vorher (60 bis 81 % der Tweets); die Accuracy liegt mit 0,495 bis 0,505 nur wenig über der Mehrheitsschranke von 0,423.
- **Vorsicht beim Vergleich mit der Rangliste von 2016:** Die Tweets sind öffentlich und könnten im Vortraining von Llama-3.2 enthalten gewesen sein. Die Vergleiche zwischen den Bedingungen sind davon nicht betroffen, da alle dasselbe Backbone nutzen.

### Drei-Saat-Gruppen

Mittelwert über drei Zufallssaaten, Spannweite in Klammern.

| Gruppe | Backbone | UA Macro-F1 | UA F_avg | UQ Macro-F1 | UQ F_avg |
| --- | --- | --- | --- | --- | --- |
| Span-Alignment (CONDIFF) | Llama-3.2-1B | 0,749 (0,017) | 0,782 (0,007) | 0,431 (0,071) | 0,382 (0,027) |
| fester Kopf, mit Beschreibungen | Llama-3.2-1B | 0,724 (0,007) | 0,754 (0,028) | 0,448 (0,044) | 0,408 (0,064) |
| fester Kopf, ohne Beschreibungen | Llama-3.2-1B | 0,707 (0,030) | 0,734 (0,031) | 0,249 (0,042) | 0,125 (0,058) |

### Alle Vergleiche mit Urteil

Δ ist die Differenz der Mittelwerte, erste minus zweite Bedingung. „—“ bedeutet nicht nachweisbar.

| Vergleich | Kapitel | Split | Δ Macro-F1 | Urteil | Δ F_avg | Urteil | Δ QWK | Urteil |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Span-Alignment gegen festen Kopf | 5.6 | UA | +0,026 | **Effekt** | +0,028 | **Effekt** | +0,052 | **Effekt** |
| Span-Alignment gegen festen Kopf | 5.6 | UQ | −0,017 | — | −0,026 | — | −0,009 | — |
| fester Kopf: mit gegen ohne Beschreibungen | 5.6 | UA | +0,016 | — | +0,020 | — | +0,016 | — |
| fester Kopf: mit gegen ohne Beschreibungen | 5.6 | UQ | +0,199 | **Effekt** | +0,282 | **Effekt** | +0,091 | — |

## Die Läufe im Einzelnen

Die Werte sind Macro-F1 und F_avg auf dem jeweiligen Testsplit; QWK und Accuracy stehen in der `*_metrics.json` des Laufs.

### Llama-3.2-1B

| Lauf | Konfiguration | lr | Saat | UA Macro-F1 | UA F_avg | UQ Macro-F1 | UQ F_avg |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `stance_condiff_llama1b` | Span-Alignment, CONDIFF, kausal | 5e-5 | 114514 | 0,739 | 0,778 | 0,463 | 0,399 |
| `stance_condiff_llama1b_seed1` | Span-Alignment, CONDIFF, kausal | 5e-5 | 1 | 0,752 | 0,785 | 0,391 | 0,372 |
| `stance_condiff_llama1b_seed2` | Span-Alignment, CONDIFF, kausal | 5e-5 | 2 | 0,756 | 0,783 | 0,438 | 0,375 |
| `stance_seqclass_llama1b` | fester Kopf, mit Beschreibungen | 5e-5 | 114514 | 0,722 | 0,737 | 0,472 | 0,440 |
| `stance_seqclass_llama1b_seed1` | fester Kopf, mit Beschreibungen | 5e-5 | 1 | 0,728 | 0,765 | 0,444 | 0,407 |
| `stance_seqclass_llama1b_seed2` | fester Kopf, mit Beschreibungen | 5e-5 | 2 | 0,721 | 0,760 | 0,428 | 0,376 |
| `stance_seqclass_norub_llama1b` | fester Kopf, ohne Beschreibungen | 5e-5 | 114514 | 0,723 | 0,750 | 0,276 | 0,161 |
| `stance_seqclass_norub_llama1b_seed1` | fester Kopf, ohne Beschreibungen | 5e-5 | 1 | 0,693 | 0,719 | 0,233 | 0,104 |
| `stance_seqclass_norub_llama1b_seed2` | fester Kopf, ohne Beschreibungen | 5e-5 | 2 | 0,705 | 0,735 | 0,238 | 0,111 |

Alle Läufe liegen in derselben Umgebung (torch 2.11.0, transformers 4.57.6, peft 0.20.0; `results/logs/server_stance/`) und sind bis auf Benchmark und Saat identisch mit den entsprechenden Beetle-Läufen mit Llama-3.2-1B.

## Was in einem Lauf-Ordner steht

| Datei | Inhalt |
| --- | --- |
| `predictions/test_<split>_metrics.json` | QWK, Macro-F1 und Accuracy des Laufs |
| `predictions/test_<split>_per_question_metrics.json` | Dieselben Kennzahlen je Target |
| `predictions/test_<split>_predictions.csv` | Jeder Test-Tweet mit Referenzlabel und Vorhersage (0 = Against, 1 = None, 2 = Favor) |
| `config.json` | Architektur: Fusionsmechanismus (`p-condiff` bzw. `p-only` für den festen Kopf), Bibliotheksversion |
| `training_args.json` | Hyperparameter: Lernrate, Zufallssaat, Epochen, `drop_all_rubrics` (True = ohne Beschreibungen) |
| `adapter_config.json` | LoRA-Konfiguration (Rang, Alpha, Zielmodule) |

## Quelle

Mohammad, S., Kiritchenko, S., Sobhani, P., Zhu, X. & Cherry, C. (2016). *SemEval-2016 Task 6: Detecting Stance in Tweets.* Proceedings of the 10th International Workshop on Semantic Evaluation (SemEval-2016), 31–41. https://doi.org/10.18653/v1/S16-1003
