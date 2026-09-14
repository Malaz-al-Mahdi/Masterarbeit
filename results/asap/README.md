# Ergebnisse: ASAP-SAS

Zehn unabhängige Aufgaben aus Naturwissenschaften, Biologie und Englisch, bearbeitet von Schülerinnen und Schülern der 8. und 10. Klasse.

Der einzige Benchmark mit **je Aufgabe wechselnder Stufenzahl** — vier Aufgaben sind vierstufig, sechs dreistufig. Genau das ist der Grund, warum ein Modell mit festem Klassifikationskopf hier gar nicht trainiert werden kann. Zugleich der einzige Benchmark, der **nur den UA-Split** bereitstellt.

**Umfang:** 17.043 Trainingsinstanzen, 5.224 im UA-Test

Insgesamt liegen hier **30 Trainingsläufe**.

## Was die Ergebnisse aussagen

Auf ASAP-SAS liegt jede Konfiguration nur mit **einer** Zufallssaat vor. Einzelne Unterschiede sind deshalb an der Rauschgrenze von 0,037 QWK zu messen, die auf Beetle und SciEntsBank aus Wiederholungen mit drei Saaten bestimmt wurde.

- **Die Fusionsmechanismen sind nicht zu unterscheiden.** Je Backbone liegen CONDIFF, CONCAT und GATE innerhalb von höchstens 0,011 QWK (Llama-3.2-1B 0,818 bis 0,829, Llama-3.2-3B 0,838 bis 0,842, Mistral-7B 0,855 bis 0,860).
- **Die Attention-Maske wirkt hier nicht.** Keine Maskenstufe weicht um mehr als 0,015 QWK von der kausalen Referenz ab, und es gibt keinen monotonen Zusammenhang mit der Zahl bidirektionaler Schichten. Auch die vollständig bidirektionale Bedingung ändert das Ergebnis nur um +0,006 (1B) und +0,003 (3B).
- **Die Bezugsgröße der Fusion ist ohne Wirkung.** t-concat liegt 0,014 (1B) und 0,005 (3B) unter CONCAT.
- **Die Aufgabe entscheidet stärker als die Architektur.** Je Einzelaufgabe reicht das QWK von 0,625 bis 0,834 (Llama-3.2-1B) und von 0,653 bis 0,898 (Mistral-7B). Diese Spanne liegt um ein Vielfaches über allen Architekturunterschieden auf diesem Benchmark (Kapitel 5.10).
- **Encoder sind auf dem UA-Split konkurrenzfähig.** RoBERTa-base erreicht 0,821 und damit das Niveau von Llama-3.2-1B (0,818); BERT-base liegt bei 0,788.

## Warum hier weniger Läufe liegen

**Der feste Klassifikationskopf ist hier nicht trainierbar.** Er bildet auf eine feste Zahl von Ausgaben ab, ASAP-SAS wechselt die Stufenzahl aber je Aufgabe zwischen drei und vier. Damit entfallen alle `seqclass`-Varianten.

**ASAP-SAS stellt nur den UA-Split bereit.** Die Wiederholungen mit mehreren Saaten dienten der Absicherung von Aussagen über Generalisierung auf neue Fragen und Domänen; sie wurden daher auf Beetle und SciEntsBank konzentriert.

## Die Läufe im Einzelnen

Jede Zeile ist ein Trainingslauf. Die Werte sind QWK auf dem jeweiligen Testsplit; Macro-F1 und Accuracy stehen in der `*_metrics.json` des Laufs. Die Spalte **Was der Lauf zeigt** nennt, welche Frage der Lauf beantwortet, und vergleicht ihn mit seiner Referenz bei gleicher Saat. Solche Einzelvergleiche sind gegen die Rauschgrenze von 0,037 QWK zu lesen; belastbar sind erst die Drei-Saat-Vergleiche oben.

### Llama-3.2-1B

| Lauf | Konfiguration | lr | Saat | UA | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- |
| `asap_bidir02_llama1b` ¹ | CONDIFF, 3 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,822 | Attention-Maske: 3 von 16 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b`: UA +0,004 → im Rauschen. |
| `asap_bidir04_llama1b` ¹ | CONDIFF, 6 Schichten bidirektional (ρ = 0,4) | 5e-5 ¹ | 114514 | 0,822 | Attention-Maske: 6 von 16 Schichten bidirektional, ρ = 0,4, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b`: UA +0,004 → im Rauschen. |
| `asap_bidir06_llama1b` ¹ | CONDIFF, 9 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,819 | Attention-Maske: 9 von 16 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b`: UA +0,001 → im Rauschen. |
| `asap_bidir08_llama1b` ¹ | CONDIFF, 12 Schichten bidirektional (ρ = 0,8) | 5e-5 ¹ | 114514 | 0,823 | Attention-Maske: 12 von 16 Schichten bidirektional, ρ = 0,8, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b`: UA +0,005 → im Rauschen. |
| `asap_bidir10_llama1b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,821 | Attention-Maske: 1 von 16 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b`: UA +0,003 → im Rauschen. |
| `asap_concat_llama1b` ¹ | CONCAT, kausal | 5e-5 ¹ | 114514 | 0,828 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `asap_condiff_llama1b`: UA +0,010 → im Rauschen. |
| `asap_condiff_llama1b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,818 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. |
| `asap_gate_llama1b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,829 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `asap_condiff_llama1b`: UA +0,011 → im Rauschen. |
| `asap_sas_bidirfull_llama1b` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,825 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b`: UA +0,006 → im Rauschen. |
| `asap_sas_tconcat_llama1b` | t-concat, kausal | 5e-5 | 114514 | 0,814 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `asap_concat_llama1b`: UA −0,014 → im Rauschen. |

### Llama-3.2-3B

| Lauf | Konfiguration | lr | Saat | UA | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- |
| `asap_bidir02_llama3b` ¹ | CONDIFF, 5 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,836 | Attention-Maske: 5 von 28 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b`: UA −0,001 → im Rauschen. |
| `asap_bidir04_llama3b` ¹ | CONDIFF, 11 Schichten bidirektional (ρ = 0,4) | 5e-5 ¹ | 114514 | 0,834 | Attention-Maske: 11 von 28 Schichten bidirektional, ρ = 0,4, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b`: UA −0,003 → im Rauschen. |
| `asap_bidir06_llama3b` ¹ | CONDIFF, 16 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,834 | Attention-Maske: 16 von 28 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b`: UA −0,004 → im Rauschen. |
| `asap_bidir08_llama3b` ¹ | CONDIFF, 22 Schichten bidirektional (ρ = 0,8) | 5e-5 ¹ | 114514 | 0,839 | Attention-Maske: 22 von 28 Schichten bidirektional, ρ = 0,8, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b`: UA +0,001 → im Rauschen. |
| `asap_bidir10_llama3b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,843 | Attention-Maske: 1 von 28 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b`: UA +0,006 → im Rauschen. |
| `asap_concat_llama3b` ¹ | CONCAT, kausal | 5e-5 ¹ | 114514 | 0,842 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `asap_condiff_llama3b`: UA +0,005 → im Rauschen. |
| `asap_condiff_llama3b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,838 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. |
| `asap_gate_llama3b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,841 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `asap_condiff_llama3b`: UA +0,003 → im Rauschen. |
| `asap_sas_bidirfull_llama3b` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,840 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b`: UA +0,003 → im Rauschen. |
| `asap_sas_tconcat_llama3b` | t-concat, kausal | 5e-5 | 114514 | 0,837 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `asap_concat_llama3b`: UA −0,005 → im Rauschen. |

### Mistral-7B-v0.1

| Lauf | Konfiguration | lr | Saat | UA | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- |
| `asap_bidir02_mistral7b` ¹ | CONDIFF, 6 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,851 | Attention-Maske: 6 von 32 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_mistral7b`: UA −0,009 → im Rauschen. |
| `asap_bidir04_mistral7b` ¹ | CONDIFF, 12 Schichten bidirektional (ρ = 0,4) | 5e-5 ¹ | 114514 | 0,845 | Attention-Maske: 12 von 32 Schichten bidirektional, ρ = 0,4, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_mistral7b`: UA −0,015 → im Rauschen. |
| `asap_bidir06_mistral7b` ¹ | CONDIFF, 19 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,855 | Attention-Maske: 19 von 32 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_mistral7b`: UA −0,005 → im Rauschen. |
| `asap_bidir08_mistral7b` ¹ | CONDIFF, 25 Schichten bidirektional (ρ = 0,8) | 5e-5 ¹ | 114514 | 0,847 | Attention-Maske: 25 von 32 Schichten bidirektional, ρ = 0,8, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_mistral7b`: UA −0,013 → im Rauschen. |
| `asap_bidir10_mistral7b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,849 | Attention-Maske: 1 von 32 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_mistral7b`: UA −0,011 → im Rauschen. |
| `asap_concat_mistral7b` ¹ | CONCAT, kausal | 5e-5 ¹ | 114514 | 0,855 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `asap_condiff_mistral7b`: UA −0,005 → im Rauschen. |
| `asap_condiff_mistral7b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,860 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. |
| `asap_gate_mistral7b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,858 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `asap_condiff_mistral7b`: UA −0,002 → im Rauschen. |

### RoBERTa-base

| Lauf | Konfiguration | lr | Saat | UA | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- |
| `asap_condiff_roberta` ¹ | CONDIFF, kausal | ? ¹ | 114514 | 0,821 | Encoder-Baseline (Hauptlauf, erste Trainingswelle, Lernrate nicht dokumentiert). Gegenüber Llama-3.2-1B `asap_condiff_llama1b`: UA +0,003 → im Rauschen. |

### BERT-base-uncased

| Lauf | Konfiguration | lr | Saat | UA | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- |
| `asap_condiff_bert` ¹ | CONDIFF, kausal | ? ¹ | 114514 | 0,788 | Encoder-Baseline (Hauptlauf, erste Trainingswelle, Lernrate nicht dokumentiert). Gegenüber Llama-3.2-1B `asap_condiff_llama1b`: UA −0,030 → im Rauschen. |

¹ Erste Trainingswelle ohne `config.json` und `training_args.json`; Konfiguration aus dem Ordnernamen abgeleitet. Bei den Decoder-Läufen war die Lernrate durchgängig 5·10⁻⁵; bei den Encoder-Hauptläufen ist sie nicht dokumentiert (?).

## Was in einem Lauf-Ordner steht

| Datei | Inhalt |
| --- | --- |
| `predictions/test_<split>_metrics.json` | Die drei Kennzahlen des Laufs: QWK, Macro-F1, Accuracy |
| `predictions/test_<split>_per_question_metrics.json` | Dieselben Kennzahlen, aufgeschlüsselt nach Einzelfrage |
| `predictions/test_<split>_predictions.csv` | Jede Testinstanz mit Referenzlabel und Vorhersage |
| `config.json` | Architektur: Fusionsmechanismus, Zahl bidirektionaler Schichten, Bibliotheksversion |
| `training_args.json` | Hyperparameter: Lernrate, Zufallssaat, Epochen, Ablationsschalter |
| `adapter_config.json` | LoRA-Konfiguration (Rang, Alpha, Zielmodule) |
| `non_peft_params.bin` | Fusions- und Scoring-Parameter, die außerhalb von LoRA voll trainiert werden |

Die trainierten LoRA-Gewichte (`adapter_model.safetensors`, je rund 180 MB) sind nicht Teil des Repositorys, da sie die Dateigrößengrenze von GitHub überschreiten.

Läufe der ersten Trainingswelle haben **keine** `config.json` und `training_args.json`. Ihre Konfiguration ergibt sich aus dem Ordnernamen; sie sind in den Tabellen mit ¹ markiert.

## Wie die Ordnernamen zu lesen sind

```
<benchmark>_<variante>_<backbone>[_seed<N>][_<zusatz>]
```

| Namensbestandteil | Bedeutung |
| --- | --- |
| `condiff`, `concat`, `gate` | Fusionsmechanismus bei kausaler Attention |
| `tconcat` | wie `concat`, aber am **Antwort-Span** verankert statt an der Sequenzrepräsentation |
| `diff` | Fusion allein über den Differenzterm z − r_k (ohne Konkatenation) |
| `bidir02` … `bidir08` | anteilige Aufhebung der kausalen Maske (ρ = 0,2 … 0,8) |
| `bidir10` | **Achtung:** ρ = 1,0 wird als *absolute* Schichtzahl gelesen und bedeutet **eine** bidirektionale Schicht, nicht das volle Modell |
| `bidirfull` | vollständig bidirektional, alle Schichten |
| `seqclass` | fester Klassifikationskopf statt Span-Alignment (die Baseline) |
| `seqclass_norub` | dasselbe, aber **ohne Rubriktexte** in der Eingabe |
| `seqclass_ctx` | fester Kopf **mit** Frage und Musterlösung in der Eingabe |
| `seqclass_ctx_norub` | mit Frage und Musterlösung, aber ohne Rubriktexte |
| `_seed1`, `_seed2` | Wiederholung mit anderer Zufallssaat; ohne Zusatz gilt die Referenzsaat 114514 |
| `_lr2e4` | abweichende Lernrate 2·10⁻⁴ statt der sonst durchgängigen 5·10⁻⁵ |
| `_batch16_lr1e5` usw. | Encoder-Lernratenreihe: Batchgröße und Lernrate stehen im Namen |
| `_tf4576`, `_tf5161` | Bibliotheksversion (transformers 4.57.6 bzw. 5.16.1) — nur dort vergeben, wo zwei Läufe sonst denselben Namen trügen |
| `_v2` | Wiederholung desselben Laufs zur Prüfung der Reproduzierbarkeit |

## Bekannte Einschränkungen

- Sämtliche Läufe beruhen auf einer einzigen Zufallssaat.
- Die Encoder-Läufe `asap_condiff_bert` und `asap_condiff_roberta` stammen aus der ersten Trainingswelle; Lernrate, Batchgröße und Epochenzahl sind nicht dokumentiert.
