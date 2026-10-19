# Ergebnisse: ASAP-SAS

Zehn unabhängige Aufgaben aus Naturwissenschaften, Biologie und Englisch, bearbeitet von Schülerinnen und Schülern der 8. und 10. Klasse.

Der einzige Benchmark mit **je Aufgabe wechselnder Stufenzahl** — vier Aufgaben sind vierstufig, sechs dreistufig. Genau das ist der Grund, warum ein Modell mit festem Klassifikationskopf hier gar nicht trainiert werden kann. Zugleich der einzige Benchmark, der **nur den UA-Split** bereitstellt.

**Umfang:** 17.043 Trainingsinstanzen, 5.224 im UA-Test

Insgesamt liegen hier **57 Trainingsläufe**, davon 9 Transferläufe, die auf ASAP-SAS trainiert und auf Beetle und SciEntsBank getestet werden, und 6 Transferläufe in umgekehrter Richtung (Training auf Beetle bzw. SciEntsBank, Test auf ASAP-SAS).

## Was die Ergebnisse aussagen

Auf ASAP-SAS liegen die meisten Konfigurationen nur mit **einer** Zufallssaat vor; nur die kausale CONDIFF-Bedingung aller drei Backbones, die vollständig bidirektionale der beiden Llama-Größen sowie die Transferläufe wurden mit drei Saaten wiederholt. Einzelne Unterschiede sind deshalb an der Rauschgrenze von 0,037 QWK zu messen, die auf Beetle und SciEntsBank aus Wiederholungen mit drei Saaten bestimmt wurde.

- **Die Fusionsmechanismen sind nicht zu unterscheiden.** Je Backbone liegen CONDIFF, CONCAT und GATE innerhalb von höchstens 0,011 QWK (Llama-3.2-1B 0,818 bis 0,829, Llama-3.2-3B 0,838 bis 0,842, Mistral-7B 0,855 bis 0,860).
- **Die Attention-Maske wirkt hier nicht.** Keine Maskenstufe weicht um mehr als 0,015 QWK von der kausalen Referenz ab, und es gibt keinen monotonen Zusammenhang mit der Zahl bidirektionaler Schichten. Auch die vollständig bidirektionale Bedingung ist ohne Wirkung: unter drei Saaten +0,003 (1B) und +0,001 (3B) QWK, im Einzellauf mit Mistral-7B −0,007.
- **Die Bezugsgröße der Fusion ist ohne Wirkung.** t-concat liegt 0,014 (1B), 0,005 (3B) und 0,003 (7B) unter CONCAT.
- **Die Aufgabe entscheidet stärker als die Architektur.** Je Einzelaufgabe reicht das QWK von 0,625 bis 0,834 (Llama-3.2-1B) und von 0,653 bis 0,898 (Mistral-7B). Diese Spanne liegt um ein Vielfaches über allen Architekturunterschieden auf diesem Benchmark (Kapitel 5.5.1).
- **Mistral-7B liegt knapp vor den Llama-Größen.** Mit drei Saaten erreicht CONDIFF 0,856 QWK gegenüber 0,827 (1B) und 0,835 (3B). Nach der Auswertungsregel ist das jeweils ein Effekt, unter beiden Metriken, aber die Differenzen liegen unter der Rauschgrenze und bestehen die Regel nur wegen der engen Streuung (Spannweiten 0,004 bis 0,016). Da die Llama-Modelle bei einem Viertel ihrer vorgesehenen Lernrate liefen, ist das kein gesicherter Größeneffekt (Kapitel 5.4.4).
- **Encoder sind auf dem UA-Split konkurrenzfähig.** RoBERTa-base erreicht 0,821 und damit nahezu das Niveau von Llama-3.2-1B (0,827 im Mittel über drei Saaten); BERT-base liegt bei 0,788.
- **Die Übertragung auf andere Benchmarks scheitert.** Auf ASAP-SAS trainiert, erreichen alle drei Backbones auf Beetle und SciEntsBank nur Zufallsniveau (QWK im Mittel −0,017 bis 0,022, unter beiden Metriken ein Effekt gegenüber den In-Domain-Läufen). Alle Modelle ordnen nahezu jede Antwort der untersten Stufe zu. Auch in umgekehrter Richtung scheitert die Übertragung: Llama-3.2-1B, nur auf Beetle bzw. SciEntsBank trainiert, erreicht auf ASAP-SAS im Mittel −0,020 bzw. 0,157 QWK (In-Domain 0,827), mit einer Accuracy auf Höhe oder unter der Mehrheitsschranke. Mit den Daten vereinbar ist die Hypothese, dass die Modelle auf ASAP-SAS die Antwortlänge als Hinweis auf die Stufe gelernt haben; die Antworten der Zielbenchmarks sind durchweg kurz (Kapitel 5.3.4).

### Drei-Saat-Gruppen

Mittelwert über drei Zufallssaaten, Spannweite in Klammern.

| Gruppe | Backbone | UA QWK | UA F1 |
| --- | --- | --- | --- |
| CONDIFF, kausal | Llama-3.2-1B | 0,827 (0,016) | 0,744 (0,030) |
| CONDIFF, vollständig bidirektional | Llama-3.2-1B | 0,830 (0,010) | 0,754 (0,013) |
| CONDIFF, kausal | Llama-3.2-3B | 0,835 (0,004) | 0,762 (0,010) |
| CONDIFF, vollständig bidirektional | Llama-3.2-3B | 0,836 (0,008) | 0,767 (0,013) |
| CONDIFF, kausal | Mistral-7B | 0,856 (0,009) | 0,794 (0,023) |

### Alle Vergleiche mit Urteil

Δ ist die Differenz der Mittelwerte, erste minus zweite Bedingung. „—“ bedeutet nicht nachweisbar. Die Transferläufe werden gegen die auf dem jeweiligen Zielbenchmark trainierten CONDIFF-Läufe desselben Backbones verglichen.

| Vergleich | Kapitel | Split | Δ QWK | Urteil QWK | Δ Macro-F1 | Urteil Macro-F1 |
| --- | --- | --- | --- | --- | --- | --- |
| Transfer gegen In-Domain (1B) | 5.3.4 | Beetle UA | −0,731 | **Effekt** | −0,561 | **Effekt** |
| Transfer gegen In-Domain (1B) | 5.3.4 | Beetle UQ | −0,510 | **Effekt** | −0,423 | **Effekt** |
| Transfer gegen In-Domain (1B) | 5.3.4 | SciEntsBank UA | −0,694 | **Effekt** | −0,509 | **Effekt** |
| Transfer gegen In-Domain (1B) | 5.3.4 | SciEntsBank UQ | −0,540 | **Effekt** | −0,430 | **Effekt** |
| Transfer gegen In-Domain (1B) | 5.3.4 | SciEntsBank UD | −0,557 | **Effekt** | −0,448 | **Effekt** |
| Transfer gegen In-Domain (3B) | 5.3.4 | Beetle UA | −0,734 | **Effekt** | −0,524 | **Effekt** |
| Transfer gegen In-Domain (3B) | 5.3.4 | Beetle UQ | −0,561 | **Effekt** | −0,375 | **Effekt** |
| Transfer gegen In-Domain (3B) | 5.3.4 | SciEntsBank UA | −0,712 | **Effekt** | −0,491 | **Effekt** |
| Transfer gegen In-Domain (3B) | 5.3.4 | SciEntsBank UQ | −0,567 | **Effekt** | −0,454 | **Effekt** |
| Transfer gegen In-Domain (3B) | 5.3.4 | SciEntsBank UD | −0,601 | **Effekt** | −0,442 | **Effekt** |
| Transfer gegen In-Domain (7B) | 5.3.4 | Beetle UA | −0,778 | **Effekt** | −0,596 | **Effekt** |
| Transfer gegen In-Domain (7B) | 5.3.4 | Beetle UQ | −0,605 | **Effekt** | −0,448 | **Effekt** |
| Transfer gegen In-Domain (7B) | 5.3.4 | SciEntsBank UA | −0,738 | **Effekt** | −0,534 | **Effekt** |
| Transfer gegen In-Domain (7B) | 5.3.4 | SciEntsBank UQ | −0,635 | **Effekt** | −0,487 | **Effekt** |
| Transfer gegen In-Domain (7B) | 5.3.4 | SciEntsBank UD | −0,630 | **Effekt** | −0,476 | **Effekt** |
| Transfer Beetle → ASAP-SAS gegen In-Domain (1B) | 5.3.4 | UA | −0,846 | **Effekt** | −0,555 | **Effekt** |
| Transfer SciEntsBank → ASAP-SAS gegen In-Domain (1B) | 5.3.4 | UA | −0,670 | **Effekt** | −0,464 | **Effekt** |
| vollständig bidirektional gegen kausal (1B) | 5.4.3 | UA | +0,003 | — | +0,010 | — |
| vollständig bidirektional gegen kausal (3B) | 5.4.3 | UA | +0,001 | — | +0,005 | — |

## Warum hier weniger Läufe liegen

**Der feste Klassifikationskopf ist hier nicht trainierbar.** Er bildet auf eine feste Zahl von Ausgaben ab, ASAP-SAS wechselt die Stufenzahl aber je Aufgabe zwischen drei und vier. Damit entfallen alle `seqclass`-Varianten.

**ASAP-SAS stellt nur den UA-Split bereit.** Die Wiederholungen mit mehreren Saaten dienten der Absicherung von Aussagen über Generalisierung auf neue Fragen und Domänen; sie wurden daher auf Beetle und SciEntsBank konzentriert. Ausnahmen sind die kausale CONDIFF-Bedingung aller drei Backbones, die vollständig bidirektionale Bedingung der Llama-Backbones und die Transferläufe, die auch hier mit drei Saaten vorliegen.

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
| `asap_condiff_llama1b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,818 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. Drei-Saat-Mittel: UA 0,827 (Δ 0,016). |
| `asap_condiff_llama1b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,834 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,827 (Δ 0,016). |
| `asap_condiff_llama1b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,827 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,827 (Δ 0,016). |
| `asap_gate_llama1b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,829 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `asap_condiff_llama1b`: UA +0,011 → im Rauschen. |
| `asap_sas_bidirfull_llama1b` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,825 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b`: UA +0,006 → im Rauschen. Drei-Saat-Mittel: UA 0,830 (Δ 0,010). |
| `asap_sas_bidirfull_llama1b_seed1` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 1 | 0,830 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b_seed1`: UA −0,004 → im Rauschen. Drei-Saat-Mittel: UA 0,830 (Δ 0,010). |
| `asap_sas_bidirfull_llama1b_seed2` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 2 | 0,835 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama1b_seed2`: UA +0,007 → im Rauschen. Drei-Saat-Mittel: UA 0,830 (Δ 0,010). |
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
| `asap_condiff_llama3b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,838 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. Drei-Saat-Mittel: UA 0,835 (Δ 0,004). |
| `asap_condiff_llama3b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,833 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,835 (Δ 0,004). |
| `asap_condiff_llama3b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,835 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,835 (Δ 0,004). |
| `asap_gate_llama3b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,841 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `asap_condiff_llama3b`: UA +0,003 → im Rauschen. |
| `asap_sas_bidirfull_llama3b` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,840 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b`: UA +0,003 → im Rauschen. Drei-Saat-Mittel: UA 0,836 (Δ 0,008). |
| `asap_sas_bidirfull_llama3b_seed1` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 1 | 0,836 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b_seed1`: UA +0,002 → im Rauschen. Drei-Saat-Mittel: UA 0,836 (Δ 0,008). |
| `asap_sas_bidirfull_llama3b_seed2` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 2 | 0,832 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_llama3b_seed2`: UA −0,003 → im Rauschen. Drei-Saat-Mittel: UA 0,836 (Δ 0,008). |
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
| `asap_condiff_mistral7b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,860 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. Drei-Saat-Mittel: UA 0,856 (Δ 0,009). |
| `asap_condiff_mistral7b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,851 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,856 (Δ 0,009). |
| `asap_condiff_mistral7b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,857 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,856 (Δ 0,009). |
| `asap_gate_mistral7b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,858 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `asap_condiff_mistral7b`: UA −0,002 → im Rauschen. |
| `asap_sas_bidirfull_mistral7b` | CONDIFF, 32 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,853 | Attention-Maske: alle 32 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `asap_condiff_mistral7b`: UA −0,007 → im Rauschen. Die Referenz stammt aus der ersten Trainingswelle, also aus einer anderen Umgebung. |
| `asap_sas_tconcat_mistral7b` | t-concat, kausal | 5e-5 | 114514 | 0,852 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `asap_concat_mistral7b`: UA −0,003 → im Rauschen. |

### Transferläufe (Training ASAP-SAS, Test Beetle und SciEntsBank)

Die Werte sind QWK auf den Testsplits der Zielbenchmarks; die Dateien in `predictions/` tragen deren Präfix (`beetle_test_ua_metrics.json` usw.).

| Lauf | Backbone | Saat | Beetle UA | Beetle UQ | SciEntsBank UA | SciEntsBank UQ | SciEntsBank UD | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `transfer_asap_condiff_llama1b` | Llama-3.2-1B | 114514 | −0,000 | −0,001 | 0,001 | 0,001 | −0,001 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 99,1 bis 99,9 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_llama1b_seed1` | Llama-3.2-1B | 1 | −0,003 | 0,000 | 0,013 | 0,013 | 0,008 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 97,2 bis 100,0 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_llama1b_seed2` | Llama-3.2-1B | 2 | 0,000 | 0,000 | 0,009 | −0,000 | 0,001 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 98,9 bis 100,0 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_llama3b` | Llama-3.2-3B | 114514 | −0,007 | 0,011 | 0,017 | 0,007 | 0,009 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 89,5 bis 95,5 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_llama3b_seed1` | Llama-3.2-3B | 1 | −0,008 | −0,013 | 0,018 | 0,006 | 0,017 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 95,2 bis 97,1 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_llama3b_seed2` | Llama-3.2-3B | 2 | 0,003 | −0,048 | 0,032 | 0,011 | 0,024 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 80,2 bis 94,1 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_mistral7b` | Mistral-7B | 114514 | 0,002 | 0,000 | 0,015 | 0,012 | 0,017 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 97,2 bis 100,0 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_mistral7b_seed1` | Mistral-7B | 1 | −0,000 | −0,001 | 0,016 | 0,002 | 0,008 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 98,0 bis 99,9 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |
| `transfer_asap_condiff_mistral7b_seed2` | Mistral-7B | 2 | 0,000 | 0,001 | 0,021 | −0,001 | 0,009 | Training nur auf ASAP-SAS, Test ohne weiteres Training mit den Rubriken des Zielbenchmarks. 97,0 bis 100,0 % der Antworten werden der untersten Stufe zugeordnet; QWK auf Zufallsniveau (Kapitel 5.3.4). |

### Transferläufe in umgekehrter Richtung (Training Beetle bzw. SciEntsBank, Test ASAP-SAS)

Die Werte sind QWK auf dem UA-Split von ASAP-SAS; die Dateien in `predictions/` tragen das Präfix `asap_sas_`.

| Lauf | Training | Saat | ASAP-SAS UA | Was der Lauf zeigt |
| --- | --- | --- | --- | --- |
| `transfer_beetle_condiff_llama1b` | Beetle | 114514 | 0,060 | Training nur auf Beetle, Test ohne weiteres Training auf ASAP-SAS mit dessen Rubriken. 89.4 % der Antworten werden der untersten Stufe zugeordnet; Accuracy 0,399 gegenüber einer Mehrheitsschranke von 0,393 (Kapitel 5.3.4). |
| `transfer_beetle_condiff_llama1b_seed1` | Beetle | 1 | −0,166 | Training nur auf Beetle, Test ohne weiteres Training auf ASAP-SAS mit dessen Rubriken. 52.0 % der Antworten werden der untersten Stufe zugeordnet; Accuracy 0,275 gegenüber einer Mehrheitsschranke von 0,393 (Kapitel 5.3.4). |
| `transfer_beetle_condiff_llama1b_seed2` | Beetle | 2 | 0,046 | Training nur auf Beetle, Test ohne weiteres Training auf ASAP-SAS mit dessen Rubriken. 89.8 % der Antworten werden der untersten Stufe zugeordnet; Accuracy 0,388 gegenüber einer Mehrheitsschranke von 0,393 (Kapitel 5.3.4). |
| `transfer_scientsbank_condiff_llama1b` | SciEntsBank | 114514 | 0,043 | Training nur auf SciEntsBank, Test ohne weiteres Training auf ASAP-SAS mit dessen Rubriken. 29.9 % der Antworten werden der untersten Stufe zugeordnet; Accuracy 0,320 gegenüber einer Mehrheitsschranke von 0,393 (Kapitel 5.3.4). |
| `transfer_scientsbank_condiff_llama1b_seed1` | SciEntsBank | 1 | 0,270 | Training nur auf SciEntsBank, Test ohne weiteres Training auf ASAP-SAS mit dessen Rubriken. 18.5 % der Antworten werden der untersten Stufe zugeordnet; Accuracy 0,382 gegenüber einer Mehrheitsschranke von 0,393 (Kapitel 5.3.4). |
| `transfer_scientsbank_condiff_llama1b_seed2` | SciEntsBank | 2 | 0,157 | Training nur auf SciEntsBank, Test ohne weiteres Training auf ASAP-SAS mit dessen Rubriken. 28.2 % der Antworten werden der untersten Stufe zugeordnet; Accuracy 0,371 gegenüber einer Mehrheitsschranke von 0,393 (Kapitel 5.3.4). |

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
| `_seed3` … `_seed7` | Zusatzsaaten für CONDIFF mit Llama-3.2-1B auf Beetle, zur Prüfung der Rauschgrenze |
| `_lr2e4` | abweichende Lernrate 2·10⁻⁴ statt der sonst durchgängigen 5·10⁻⁵ |
| `_batch16_lr1e5` usw. | Encoder-Lernratenreihe: Batchgröße und Lernrate stehen im Namen |
| `_tf4576`, `_tf5161` | Bibliotheksversion (transformers 4.57.6 bzw. 5.16.1) — nur dort vergeben, wo zwei Läufe sonst denselben Namen trügen |
| `_v2` | Wiederholung desselben Laufs mit derselben Saat: zur Prüfung der Reproduzierbarkeit oder, bei Mistral-7B, eines instabilen Laufs |
| `transfer_asap_condiff_*` | Training ausschließlich auf ASAP-SAS, Test auf Beetle und SciEntsBank (liegt unter `asap/`) |
| `transfer_beetle_condiff_*`, `transfer_scientsbank_condiff_*` | umgekehrte Richtung: Training auf Beetle bzw. SciEntsBank, Test auf ASAP-SAS |

## Bekannte Einschränkungen

- Bis auf die kausale und die vollständig bidirektionale CONDIFF-Bedingung der Llama-Backbones und die Transferläufe mit Llama beruhen sämtliche Läufe auf einer einzigen Zufallssaat.
- **`asap_sas_bidirfull_mistral7b`** stammt aus einer neueren Umgebung (transformers 4.57.6) als die kausale Referenz `asap_condiff_mistral7b` aus der ersten Trainingswelle.
- **Die Transferläufe** prüfen nur eine Richtung (ASAP-SAS → Beetle, SciEntsBank); die Längenerklärung für ihr Scheitern ist nicht kausal geprüft.
- Die Encoder-Läufe `asap_condiff_bert` und `asap_condiff_roberta` stammen aus der ersten Trainingswelle; Lernrate, Batchgröße und Epochenzahl sind nicht dokumentiert.
