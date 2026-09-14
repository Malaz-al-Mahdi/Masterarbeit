# Ergebnisse: SciEntsBank

Breites naturwissenschaftliches Spektrum über 15 Themenblöcke, einheitlich dreistufig ordinal.

Der einzige Benchmark mit **allen drei** Generalisierungsbedingungen. Die 46 Fragen des UD-Splits stammen aus drei Themenblöcken (WA, HB, EV), die in keiner einzigen Trainingsfrage auftauchen — echter Domänentransfer. Mit 4.562 Instanzen ist UD zugleich der größte Testsplit der ganzen Arbeit.

**Umfang:** 4.969 Trainingsinstanzen; UA 540, UQ 733, UD 4.562

Insgesamt liegen hier **73 Trainingsläufe**.

## Was die Ergebnisse aussagen

Die folgenden Aussagen stützen sich, wo nicht anders vermerkt, auf Mittelwerte über drei Zufallssaaten. Ein Unterschied gilt als **Effekt**, wenn die Differenz der Mittelwerte die größere der beiden Spannweiten übersteigt, als **Hinweis**, wenn sich die Spannweiten nicht überlappen, die Differenz aber kleiner ist. Die Tabellen unten zeigen jedes Urteil unter QWK und unter Macro-F1.

- **Aufgabenbezogene Information ist der bestimmende Faktor.** Ohne Rubrik und ohne Kontext fällt der feste Kopf um 0,235 / 0,520 / 0,361 QWK (1B) und 0,255 / 0,528 / 0,440 (3B). Auf UQ und UD liegt die Leistung dann auf oder unter dem Zufallsniveau. Dieser Befund gilt unter beiden Metriken.
- **Frage und Musterlösung ersetzen die Rubrik.** Kontext allein übertrifft Rubrik allein um 0,100 / 0,060 / 0,077 QWK. Unter Macro-F1 gilt das nur auf UA; auf UQ und UD macht Kontext das Modell vor allem seltener grob falsch: Weite Fehler (incorrect ↔ correct) sinken von rund 11 % auf rund 7 % der Instanzen.
- **Die Rubrik bringt bei vorhandenem Kontext unter QWK nichts** (−0,012 / +0,015 / +0,006). Unter Macro-F1 trägt sie auf UD etwas bei (+0,029, Effekt).
- **Die Fusionsmechanismen sind nicht zu unterscheiden.** Unter keiner der beiden Metriken ist ein Unterschied als Effekt nachweisbar; auf Llama-3.2-3B liegt GATE nominell auf allen drei Splits vorn.
- **Span-Alignment gegen festen Kopf: kleiner Vorteil nur bei 1B.** Auf UA (+0,021) und UD (+0,026) besteht der Vorteil die Auswertungsregel, liegt aber unter der Rauschgrenze; bei 3B ist kein Unterschied nachweisbar.
- **Die Bezugsgröße ist ohne Wirkung.** t-concat und CONCAT liegen auf allen drei Splits innerhalb ihrer Spannweiten. Mit nur der Saat 114514 hätte t-concat einen deutlichen Vorteil nahegelegt.
- **Vollständige Bidirektionalität nützt dem größeren Backbone auf UQ.** Llama-3.2-3B gewinnt 0,060 QWK auf UQ (Effekt unter beiden Metriken); bei Llama-3.2-1B ist kein Unterschied nachweisbar.
- **Die höhere Lernrate hilft hier nicht.** Llama-3.2-1B bei 2·10⁻⁴ liegt auf UA und UD unter allen Läufen bei 5·10⁻⁵ (0,679 / 0,496 QWK).
- **Encoder sind auf diesem Benchmark fragil.** BERT liegt bei jeder geprüften Lernrate auf Zufallsniveau; RoBERTa funktioniert nur bei 1·10⁻⁵ und bleibt auch dann hinter Llama-3.2-1B.
- **Die Bibliotheksversion ändert Ergebnisse messbar.** Bei gleicher Saat weichen `_tf4576` und `_tf5161` um +0,016 / −0,014 / −0,001 QWK ab.

### Drei-Saat-Gruppen

Mittelwert über drei Zufallssaaten, Spannweite in Klammern.

| Gruppe | Backbone | UA QWK | UA F1 | UQ QWK | UQ F1 | UD QWK | UD F1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CONDIFF, vollständig bidirektional | Llama-3.2-1B | 0,692 (0,049) | 0,705 (0,028) | 0,553 (0,087) | 0,629 (0,046) | 0,588 (0,046) | 0,661 (0,043) |
| CONCAT, kausal | Llama-3.2-1B | 0,698 (0,034) | 0,696 (0,043) | 0,569 (0,043) | 0,616 (0,019) | 0,577 (0,025) | 0,641 (0,002) |
| CONDIFF, kausal | Llama-3.2-1B | 0,702 (0,007) | 0,692 (0,007) | 0,544 (0,058) | 0,607 (0,018) | 0,560 (0,026) | 0,630 (0,015) |
| GATE, kausal | Llama-3.2-1B | 0,718 (0,039) | 0,707 (0,027) | 0,564 (0,032) | 0,595 (0,032) | 0,559 (0,047) | 0,622 (0,026) |
| fester Kopf, Kontext und Rubrik | Llama-3.2-1B | 0,769 (0,024) | 0,730 (0,037) | 0,630 (0,026) | 0,659 (0,004) | 0,617 (0,016) | 0,646 (0,006) |
| fester Kopf, nur Kontext | Llama-3.2-1B | 0,782 (0,041) | 0,750 (0,042) | 0,615 (0,022) | 0,640 (0,023) | 0,611 (0,038) | 0,617 (0,023) |
| fester Kopf, Rubrik | Llama-3.2-1B | 0,682 (0,007) | 0,674 (0,009) | 0,555 (0,051) | 0,643 (0,051) | 0,534 (0,016) | 0,603 (0,009) |
| fester Kopf, weder Rubrik noch Kontext | Llama-3.2-1B | 0,447 (0,055) | 0,559 (0,022) | 0,035 (0,023) | 0,357 (0,015) | 0,172 (0,038) | 0,354 (0,033) |
| CONDIFF, vollständig bidirektional | Llama-3.2-3B | 0,767 (0,024) | 0,723 (0,024) | 0,636 (0,026) | 0,696 (0,010) | 0,625 (0,036) | 0,667 (0,022) |
| CONCAT, kausal | Llama-3.2-3B | 0,720 (0,042) | 0,713 (0,021) | 0,582 (0,049) | 0,651 (0,015) | 0,626 (0,035) | 0,662 (0,022) |
| CONDIFF, kausal | Llama-3.2-3B | 0,734 (0,047) | 0,712 (0,009) | 0,576 (0,009) | 0,645 (0,021) | 0,617 (0,028) | 0,656 (0,014) |
| DIFF, kausal | Llama-3.2-3B | 0,722 (0,014) | 0,704 (0,010) | 0,579 (0,048) | 0,643 (0,039) | 0,623 (0,018) | 0,660 (0,012) |
| GATE, kausal | Llama-3.2-3B | 0,737 (0,030) | 0,716 (0,016) | 0,624 (0,060) | 0,664 (0,026) | 0,630 (0,011) | 0,660 (0,007) |
| fester Kopf, Rubrik | Llama-3.2-3B | 0,746 (0,028) | 0,725 (0,021) | 0,580 (0,076) | 0,633 (0,066) | 0,641 (0,026) | 0,646 (0,011) |
| fester Kopf, weder Rubrik noch Kontext | Llama-3.2-3B | 0,491 (0,025) | 0,573 (0,027) | 0,052 (0,063) | 0,368 (0,027) | 0,201 (0,029) | 0,385 (0,035) |
| t-concat, kausal | Llama-3.2-3B | 0,734 (0,066) | 0,721 (0,039) | 0,584 (0,059) | 0,649 (0,029) | 0,615 (0,019) | 0,657 (0,013) |

### Alle Vergleiche mit Urteil

Δ ist die Differenz der Mittelwerte, erste minus zweite Bedingung. „—“ bedeutet nicht nachweisbar.

| Vergleich | Kapitel | Split | Δ QWK | Urteil QWK | Δ Macro-F1 | Urteil Macro-F1 |
| --- | --- | --- | --- | --- | --- | --- |
| Rubrik gegen weder Rubrik noch Kontext (1B) | 5.3 | UA | +0,235 | **Effekt** | +0,116 | **Effekt** |
| Rubrik gegen weder Rubrik noch Kontext (1B) | 5.3 | UQ | +0,520 | **Effekt** | +0,286 | **Effekt** |
| Rubrik gegen weder Rubrik noch Kontext (1B) | 5.3 | UD | +0,361 | **Effekt** | +0,249 | **Effekt** |
| Rubrik gegen weder Rubrik noch Kontext (3B) | 5.3 | UA | +0,255 | **Effekt** | +0,152 | **Effekt** |
| Rubrik gegen weder Rubrik noch Kontext (3B) | 5.3 | UQ | +0,528 | **Effekt** | +0,265 | **Effekt** |
| Rubrik gegen weder Rubrik noch Kontext (3B) | 5.3 | UD | +0,440 | **Effekt** | +0,260 | **Effekt** |
| Kontext und Rubrik gegen nur Kontext (1B) | 5.3 | UA | −0,012 | — | −0,020 | — |
| Kontext und Rubrik gegen nur Kontext (1B) | 5.3 | UQ | +0,015 | — | +0,019 | Hinweis |
| Kontext und Rubrik gegen nur Kontext (1B) | 5.3 | UD | +0,006 | — | +0,029 | **Effekt** |
| nur Kontext gegen nur Rubrik (1B) | 5.3 | UA | +0,100 | **Effekt** | +0,076 | **Effekt** |
| nur Kontext gegen nur Rubrik (1B) | 5.3 | UQ | +0,060 | **Effekt** | −0,003 | — |
| nur Kontext gegen nur Rubrik (1B) | 5.3 | UD | +0,077 | **Effekt** | +0,014 | Hinweis |
| Kontext und Rubrik gegen nur Rubrik (1B) | 5.3 | UA | +0,088 | **Effekt** | +0,055 | **Effekt** |
| Kontext und Rubrik gegen nur Rubrik (1B) | 5.3 | UQ | +0,075 | **Effekt** | +0,017 | — |
| Kontext und Rubrik gegen nur Rubrik (1B) | 5.3 | UD | +0,083 | **Effekt** | +0,043 | **Effekt** |
| CONDIFF gegen CONCAT (1B) | 5.4 | UA | +0,004 | — | −0,004 | — |
| CONDIFF gegen CONCAT (1B) | 5.4 | UQ | −0,025 | — | −0,009 | — |
| CONDIFF gegen CONCAT (1B) | 5.4 | UD | −0,017 | — | −0,011 | Hinweis |
| CONDIFF gegen GATE (1B) | 5.4 | UA | −0,016 | — | −0,015 | — |
| CONDIFF gegen GATE (1B) | 5.4 | UQ | −0,020 | — | +0,012 | — |
| CONDIFF gegen GATE (1B) | 5.4 | UD | ±0,000 | — | +0,008 | — |
| CONCAT gegen GATE (1B) | 5.4 | UA | −0,020 | — | −0,012 | — |
| CONCAT gegen GATE (1B) | 5.4 | UQ | +0,005 | — | +0,021 | — |
| CONCAT gegen GATE (1B) | 5.4 | UD | +0,017 | — | +0,019 | Hinweis |
| CONDIFF gegen CONCAT (3B) | 5.4 | UA | +0,014 | — | −0,001 | — |
| CONDIFF gegen CONCAT (3B) | 5.4 | UQ | −0,006 | — | −0,007 | — |
| CONDIFF gegen CONCAT (3B) | 5.4 | UD | −0,008 | — | −0,006 | — |
| CONDIFF gegen DIFF (3B) | 5.4 | UA | +0,012 | — | +0,008 | — |
| CONDIFF gegen DIFF (3B) | 5.4 | UQ | −0,003 | — | +0,002 | — |
| CONDIFF gegen DIFF (3B) | 5.4 | UD | −0,006 | — | −0,004 | — |
| CONDIFF gegen GATE (3B) | 5.4 | UA | −0,002 | — | −0,004 | — |
| CONDIFF gegen GATE (3B) | 5.4 | UQ | −0,049 | Hinweis | −0,019 | — |
| CONDIFF gegen GATE (3B) | 5.4 | UD | −0,013 | — | −0,004 | — |
| CONCAT gegen DIFF (3B) | 5.4 | UA | −0,002 | — | +0,009 | — |
| CONCAT gegen DIFF (3B) | 5.4 | UQ | +0,003 | — | +0,008 | — |
| CONCAT gegen DIFF (3B) | 5.4 | UD | +0,002 | — | +0,002 | — |
| CONCAT gegen GATE (3B) | 5.4 | UA | −0,016 | — | −0,003 | — |
| CONCAT gegen GATE (3B) | 5.4 | UQ | −0,042 | — | −0,013 | — |
| CONCAT gegen GATE (3B) | 5.4 | UD | −0,004 | — | +0,003 | — |
| DIFF gegen GATE (3B) | 5.4 | UA | −0,014 | — | −0,012 | Hinweis |
| DIFF gegen GATE (3B) | 5.4 | UQ | −0,046 | — | −0,021 | — |
| DIFF gegen GATE (3B) | 5.4 | UD | −0,007 | — | +0,001 | — |
| t-concat gegen CONCAT (3B) | 5.5 | UA | +0,013 | — | +0,007 | — |
| t-concat gegen CONCAT (3B) | 5.5 | UQ | +0,002 | — | −0,003 | — |
| t-concat gegen CONCAT (3B) | 5.5 | UD | −0,011 | — | −0,005 | — |
| Span-Alignment gegen festen Kopf (1B) | 5.6 | UA | +0,021 | **Effekt** | +0,018 | **Effekt** |
| Span-Alignment gegen festen Kopf (1B) | 5.6 | UQ | −0,011 | — | −0,036 | — |
| Span-Alignment gegen festen Kopf (1B) | 5.6 | UD | +0,026 | **Effekt** | +0,026 | **Effekt** |
| Span-Alignment gegen festen Kopf (3B) | 5.6 | UA | −0,011 | — | −0,013 | — |
| Span-Alignment gegen festen Kopf (3B) | 5.6 | UQ | −0,004 | — | +0,011 | — |
| Span-Alignment gegen festen Kopf (3B) | 5.6 | UD | −0,024 | — | +0,010 | — |
| vollständig bidirektional gegen kausal (1B) | 5.7 | UA | −0,010 | — | +0,013 | — |
| vollständig bidirektional gegen kausal (1B) | 5.7 | UQ | +0,009 | — | +0,022 | — |
| vollständig bidirektional gegen kausal (1B) | 5.7 | UD | +0,028 | — | +0,031 | — |
| vollständig bidirektional gegen kausal (3B) | 5.7 | UA | +0,032 | Hinweis | +0,011 | — |
| vollständig bidirektional gegen kausal (3B) | 5.7 | UQ | +0,060 | **Effekt** | +0,052 | **Effekt** |
| vollständig bidirektional gegen kausal (3B) | 5.7 | UD | +0,008 | — | +0,011 | — |

## Die Läufe im Einzelnen

Jede Zeile ist ein Trainingslauf. Die Werte sind QWK auf dem jeweiligen Testsplit; Macro-F1 und Accuracy stehen in der `*_metrics.json` des Laufs. Die Spalte **Was der Lauf zeigt** nennt, welche Frage der Lauf beantwortet, und vergleicht ihn mit seiner Referenz bei gleicher Saat. Solche Einzelvergleiche sind gegen die Rauschgrenze von 0,037 QWK zu lesen; belastbar sind erst die Drei-Saat-Vergleiche oben.

### Llama-3.2-1B

| Lauf | Konfiguration | lr | Saat | UA | UQ | UD | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `scientsbank_bidir02_llama1b` ¹ | CONDIFF, 3 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,697 | 0,551 | 0,555 | Attention-Maske: 3 von 16 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b`: UA −0,009 · UQ −0,031 · UD −0,017 → im Rauschen. |
| `scientsbank_bidir04_llama1b` ¹ | CONDIFF, 6 Schichten bidirektional (ρ = 0,4) | 5e-5 ¹ | 114514 | 0,683 | 0,571 | 0,561 | Attention-Maske: 6 von 16 Schichten bidirektional, ρ = 0,4, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b`: UA −0,023 · UQ −0,011 · UD −0,011 → im Rauschen. |
| `scientsbank_bidir06_llama1b` ¹ | CONDIFF, 9 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,681 | 0,533 | 0,541 | Attention-Maske: 9 von 16 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b`: UA −0,024 · UQ −0,049 · UD −0,031 → über der Rauschgrenze auf UQ (Einzelvergleich). |
| `scientsbank_bidir08_llama1b` ¹ | CONDIFF, 12 Schichten bidirektional (ρ = 0,8) | 5e-5 ¹ | 114514 | 0,704 | 0,567 | 0,589 | Attention-Maske: 12 von 16 Schichten bidirektional, ρ = 0,8, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b`: UA −0,002 · UQ −0,015 · UD +0,017 → im Rauschen. |
| `scientsbank_bidir10_llama1b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,691 | 0,547 | 0,558 | Attention-Maske: 1 von 16 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b`: UA −0,014 · UQ −0,035 · UD −0,014 → im Rauschen. |
| `scientsbank_bidirfull_llama1b` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,675 | 0,498 | 0,560 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b`: UA −0,031 · UQ −0,084 · UD −0,012 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,692 (Δ 0,049) · UQ 0,553 (Δ 0,087) · UD 0,588 (Δ 0,046). **Schwächste der drei Saaten; der hier sichtbare Verlust bestätigt sich unter drei Saaten nicht.** |
| `scientsbank_bidirfull_llama1b_seed1` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 1 | 0,723 | 0,576 | 0,606 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b_seed1`: UA +0,021 · UQ +0,048 · UD +0,060 → über der Rauschgrenze auf UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,692 (Δ 0,049) · UQ 0,553 (Δ 0,087) · UD 0,588 (Δ 0,046). |
| `scientsbank_bidirfull_llama1b_seed2` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 2 | 0,679 | 0,585 | 0,597 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama1b_seed2`: UA −0,020 · UQ +0,062 · UD +0,036 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,692 (Δ 0,049) · UQ 0,553 (Δ 0,087) · UD 0,588 (Δ 0,046). |
| `scientsbank_concat_llama1b` ¹ | CONCAT, kausal | 5e-5 ¹ | 114514 | 0,689 | 0,565 | 0,563 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama1b`: UA −0,016 · UQ −0,017 · UD −0,010 → im Rauschen. Drei-Saat-Mittel: UA 0,698 (Δ 0,034) · UQ 0,569 (Δ 0,043) · UD 0,577 (Δ 0,025). |
| `scientsbank_concat_llama1b_seed1` | CONCAT, kausal | 5e-5 | 1 | 0,685 | 0,550 | 0,588 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama1b_seed1`: UA −0,017 · UQ +0,023 · UD +0,041 → über der Rauschgrenze auf UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,698 (Δ 0,034) · UQ 0,569 (Δ 0,043) · UD 0,577 (Δ 0,025). |
| `scientsbank_concat_llama1b_seed2` | CONCAT, kausal | 5e-5 | 2 | 0,719 | 0,593 | 0,580 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama1b_seed2`: UA +0,020 · UQ +0,070 · UD +0,019 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,698 (Δ 0,034) · UQ 0,569 (Δ 0,043) · UD 0,577 (Δ 0,025). |
| `scientsbank_condiff_llama1b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,705 | 0,582 | 0,572 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. Drei-Saat-Mittel: UA 0,702 (Δ 0,007) · UQ 0,544 (Δ 0,058) · UD 0,560 (Δ 0,026). |
| `scientsbank_condiff_llama1b_lr2e4` | CONDIFF, kausal | 2e-4 | 114514 | 0,679 | 0,537 | 0,496 | Lernrate 2·10⁻⁴ (Referenzwert der Implementierung für dieses Backbone) statt 5·10⁻⁵. Gegenüber `scientsbank_condiff_llama1b`: UA −0,027 · UQ −0,045 · UD −0,076 → über der Rauschgrenze auf UQ, UD (Einzelvergleich). Grundlage der Aussage, dass der Größenvergleich mit der Lernrate konfundiert ist (Kapitel 5.8). |
| `scientsbank_condiff_llama1b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,703 | 0,527 | 0,546 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,702 (Δ 0,007) · UQ 0,544 (Δ 0,058) · UD 0,560 (Δ 0,026). |
| `scientsbank_condiff_llama1b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,698 | 0,524 | 0,561 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,702 (Δ 0,007) · UQ 0,544 (Δ 0,058) · UD 0,560 (Δ 0,026). |
| `scientsbank_gate_llama1b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,737 | 0,544 | 0,584 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama1b`: UA +0,032 · UQ −0,038 · UD +0,012 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,718 (Δ 0,039) · UQ 0,564 (Δ 0,032) · UD 0,559 (Δ 0,047). |
| `scientsbank_gate_llama1b_seed1` | GATE, kausal | 5e-5 | 1 | 0,718 | 0,572 | 0,537 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama1b_seed1`: UA +0,016 · UQ +0,044 · UD −0,010 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,718 (Δ 0,039) · UQ 0,564 (Δ 0,032) · UD 0,559 (Δ 0,047). |
| `scientsbank_gate_llama1b_seed2` | GATE, kausal | 5e-5 | 2 | 0,698 | 0,577 | 0,557 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama1b_seed2`: UA ±0,000 · UQ +0,053 · UD −0,003 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,718 (Δ 0,039) · UQ 0,564 (Δ 0,032) · UD 0,559 (Δ 0,047). |
| `scientsbank_seqclass_ctx_llama1b` | fester Kopf, kausal, mit Kontext | 5e-5 | 114514 | 0,778 | 0,615 | 0,624 | Frage und Musterlösung zusätzlich zu den Rubriktexten; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b`: UA +0,099 · UQ +0,080 · UD +0,081 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,769 (Δ 0,024) · UQ 0,630 (Δ 0,026) · UD 0,617 (Δ 0,016). |
| `scientsbank_seqclass_ctx_llama1b_seed1` | fester Kopf, kausal, mit Kontext | 5e-5 | 1 | 0,754 | 0,641 | 0,619 | Frage und Musterlösung zusätzlich zu den Rubriktexten; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b_seed1`: UA +0,074 · UQ +0,096 · UD +0,092 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,769 (Δ 0,024) · UQ 0,630 (Δ 0,026) · UD 0,617 (Δ 0,016). |
| `scientsbank_seqclass_ctx_llama1b_seed2` | fester Kopf, kausal, mit Kontext | 5e-5 | 2 | 0,776 | 0,634 | 0,608 | Frage und Musterlösung zusätzlich zu den Rubriktexten; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b_seed2`: UA +0,090 · UQ +0,048 · UD +0,077 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,769 (Δ 0,024) · UQ 0,630 (Δ 0,026) · UD 0,617 (Δ 0,016). |
| `scientsbank_seqclass_ctx_norub_llama1b` | fester Kopf, kausal, ohne Rubrik, mit Kontext | 5e-5 | 114514 | 0,789 | 0,615 | 0,596 | Frage und Musterlösung statt der Rubriktexte; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b`: UA +0,111 · UQ +0,080 · UD +0,053 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,782 (Δ 0,041) · UQ 0,615 (Δ 0,022) · UD 0,611 (Δ 0,038). |
| `scientsbank_seqclass_ctx_norub_llama1b_seed1` | fester Kopf, kausal, ohne Rubrik, mit Kontext | 5e-5 | 1 | 0,799 | 0,626 | 0,633 | Frage und Musterlösung statt der Rubriktexte; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b_seed1`: UA +0,118 · UQ +0,081 · UD +0,107 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,782 (Δ 0,041) · UQ 0,615 (Δ 0,022) · UD 0,611 (Δ 0,038). |
| `scientsbank_seqclass_ctx_norub_llama1b_seed2` | fester Kopf, kausal, ohne Rubrik, mit Kontext | 5e-5 | 2 | 0,757 | 0,604 | 0,604 | Frage und Musterlösung statt der Rubriktexte; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b_seed2`: UA +0,072 · UQ +0,018 · UD +0,072 → über der Rauschgrenze auf UA, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,782 (Δ 0,041) · UQ 0,615 (Δ 0,022) · UD 0,611 (Δ 0,038). |
| `scientsbank_seqclass_llama1b` | fester Kopf, kausal | 5e-5 | 114514 | 0,679 | 0,535 | 0,543 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `scientsbank_condiff_llama1b`: UA −0,027 · UQ −0,047 · UD −0,029 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,682 (Δ 0,007) · UQ 0,555 (Δ 0,051) · UD 0,534 (Δ 0,016). |
| `scientsbank_seqclass_llama1b_seed1` | fester Kopf, kausal | 5e-5 | 1 | 0,680 | 0,545 | 0,526 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `scientsbank_condiff_llama1b_seed1`: UA −0,022 · UQ +0,017 · UD −0,020 → im Rauschen. Drei-Saat-Mittel: UA 0,682 (Δ 0,007) · UQ 0,555 (Δ 0,051) · UD 0,534 (Δ 0,016). |
| `scientsbank_seqclass_llama1b_seed2` | fester Kopf, kausal | 5e-5 | 2 | 0,685 | 0,586 | 0,532 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `scientsbank_condiff_llama1b_seed2`: UA −0,013 · UQ +0,063 · UD −0,029 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,682 (Δ 0,007) · UQ 0,555 (Δ 0,051) · UD 0,534 (Δ 0,016). |
| `scientsbank_seqclass_norub_llama1b` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 114514 | 0,450 | 0,027 | 0,188 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b`: UA −0,228 · UQ −0,508 · UD −0,355 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,447 (Δ 0,055) · UQ 0,035 (Δ 0,023) · UD 0,172 (Δ 0,038). |
| `scientsbank_seqclass_norub_llama1b_seed1` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 1 | 0,472 | 0,050 | 0,150 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b_seed1`: UA −0,208 · UQ −0,495 · UD −0,376 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,447 (Δ 0,055) · UQ 0,035 (Δ 0,023) · UD 0,172 (Δ 0,038). |
| `scientsbank_seqclass_norub_llama1b_seed2` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 2 | 0,418 | 0,027 | 0,179 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama1b_seed2`: UA −0,268 · UQ −0,559 · UD −0,353 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,447 (Δ 0,055) · UQ 0,035 (Δ 0,023) · UD 0,172 (Δ 0,038). |
| `scientsbank_tconcat_llama1b` | t-concat, kausal | 5e-5 | 114514 | 0,723 | 0,537 | 0,575 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `scientsbank_concat_llama1b`: UA +0,034 · UQ −0,028 · UD +0,012 → im Rauschen. |

### Llama-3.2-3B

| Lauf | Konfiguration | lr | Saat | UA | UQ | UD | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `scientsbank_bidir02_llama3b` ¹ | CONDIFF, 5 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,749 | 0,621 | 0,614 | Attention-Maske: 5 von 28 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b`: UA −0,001 · UQ +0,045 · UD +0,009 → über der Rauschgrenze auf UQ (Einzelvergleich). |
| `scientsbank_bidir04_llama3b` ¹ | CONDIFF, 11 Schichten bidirektional (ρ = 0,4) | 5e-5 ¹ | 114514 | 0,738 | 0,584 | 0,611 | Attention-Maske: 11 von 28 Schichten bidirektional, ρ = 0,4, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b`: UA −0,012 · UQ +0,008 · UD +0,006 → im Rauschen. |
| `scientsbank_bidir06_llama3b` ¹ | CONDIFF, 16 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,753 | 0,597 | 0,671 | Attention-Maske: 16 von 28 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b`: UA +0,004 · UQ +0,021 · UD +0,066 → über der Rauschgrenze auf UD (Einzelvergleich). |
| `scientsbank_bidir08_llama3b` ¹ | CONDIFF, 22 Schichten bidirektional (ρ = 0,8) | 5e-5 ¹ | 114514 | 0,749 | 0,620 | 0,637 | Attention-Maske: 22 von 28 Schichten bidirektional, ρ = 0,8, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b`: UA −0,001 · UQ +0,044 · UD +0,031 → über der Rauschgrenze auf UQ (Einzelvergleich). |
| `scientsbank_bidir10_llama3b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,754 | 0,624 | 0,638 | Attention-Maske: 1 von 28 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b`: UA +0,005 · UQ +0,048 · UD +0,033 → über der Rauschgrenze auf UQ (Einzelvergleich). |
| `scientsbank_bidirfull_llama3b` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,780 | 0,623 | 0,613 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b`: UA +0,030 · UQ +0,047 · UD +0,008 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,767 (Δ 0,024) · UQ 0,636 (Δ 0,026) · UD 0,625 (Δ 0,036). |
| `scientsbank_bidirfull_llama3b_seed1` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 1 | 0,765 | 0,635 | 0,613 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b_seed1`: UA +0,015 · UQ +0,055 · UD ±0,000 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,767 (Δ 0,024) · UQ 0,636 (Δ 0,026) · UD 0,625 (Δ 0,036). |
| `scientsbank_bidirfull_llama3b_seed2` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 2 | 0,755 | 0,649 | 0,649 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_llama3b_seed2`: UA +0,052 · UQ +0,078 · UD +0,016 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,767 (Δ 0,024) · UQ 0,636 (Δ 0,026) · UD 0,625 (Δ 0,036). |
| `scientsbank_concat_llama3b` ¹ | CONCAT, kausal | 5e-5 ¹ | 114514 | 0,717 | 0,598 | 0,610 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b`: UA −0,033 · UQ +0,022 · UD +0,004 → im Rauschen. Drei-Saat-Mittel: UA 0,720 (Δ 0,042) · UQ 0,582 (Δ 0,049) · UD 0,626 (Δ 0,035). |
| `scientsbank_concat_llama3b_seed1` | CONCAT, kausal | 5e-5 | 1 | 0,701 | 0,597 | 0,644 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b_seed1`: UA −0,049 · UQ +0,018 · UD +0,031 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,720 (Δ 0,042) · UQ 0,582 (Δ 0,049) · UD 0,626 (Δ 0,035). |
| `scientsbank_concat_llama3b_seed2` | CONCAT, kausal | 5e-5 | 2 | 0,743 | 0,550 | 0,624 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b_seed2`: UA +0,040 · UQ −0,021 · UD −0,010 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,720 (Δ 0,042) · UQ 0,582 (Δ 0,049) · UD 0,626 (Δ 0,035). |
| `scientsbank_condiff_llama3b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,750 | 0,576 | 0,605 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. Drei-Saat-Mittel: UA 0,734 (Δ 0,047) · UQ 0,576 (Δ 0,009) · UD 0,617 (Δ 0,028). |
| `scientsbank_condiff_llama3b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,750 | 0,580 | 0,614 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,734 (Δ 0,047) · UQ 0,576 (Δ 0,009) · UD 0,617 (Δ 0,028). |
| `scientsbank_condiff_llama3b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,703 | 0,571 | 0,633 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,734 (Δ 0,047) · UQ 0,576 (Δ 0,009) · UD 0,617 (Δ 0,028). |
| `scientsbank_diff_llama3b` | DIFF, kausal | 5e-5 | 114514 | 0,716 | 0,559 | 0,634 | Fusionsmechanismus DIFF (Differenzterm z − r allein) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b`: UA −0,033 · UQ −0,017 · UD +0,029 → im Rauschen. Drei-Saat-Mittel: UA 0,722 (Δ 0,014) · UQ 0,579 (Δ 0,048) · UD 0,623 (Δ 0,018). |
| `scientsbank_diff_llama3b_seed1` | DIFF, kausal | 5e-5 | 1 | 0,720 | 0,607 | 0,620 | Fusionsmechanismus DIFF (Differenzterm z − r allein) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b_seed1`: UA −0,030 · UQ +0,028 · UD +0,007 → im Rauschen. Drei-Saat-Mittel: UA 0,722 (Δ 0,014) · UQ 0,579 (Δ 0,048) · UD 0,623 (Δ 0,018). |
| `scientsbank_diff_llama3b_seed2` | DIFF, kausal | 5e-5 | 2 | 0,731 | 0,569 | 0,616 | Fusionsmechanismus DIFF (Differenzterm z − r allein) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b_seed2`: UA +0,027 · UQ −0,002 · UD −0,017 → im Rauschen. Drei-Saat-Mittel: UA 0,722 (Δ 0,014) · UQ 0,579 (Δ 0,048) · UD 0,623 (Δ 0,018). |
| `scientsbank_gate_llama3b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,750 | 0,655 | 0,635 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b`: UA +0,001 · UQ +0,079 · UD +0,029 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,737 (Δ 0,030) · UQ 0,624 (Δ 0,060) · UD 0,630 (Δ 0,011). |
| `scientsbank_gate_llama3b_seed1` | GATE, kausal | 5e-5 | 1 | 0,740 | 0,624 | 0,632 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b_seed1`: UA −0,010 · UQ +0,045 · UD +0,018 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,737 (Δ 0,030) · UQ 0,624 (Δ 0,060) · UD 0,630 (Δ 0,011). |
| `scientsbank_gate_llama3b_seed2` | GATE, kausal | 5e-5 | 2 | 0,720 | 0,594 | 0,624 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `scientsbank_condiff_llama3b_seed2`: UA +0,017 · UQ +0,023 · UD −0,009 → im Rauschen. Drei-Saat-Mittel: UA 0,737 (Δ 0,030) · UQ 0,624 (Δ 0,060) · UD 0,630 (Δ 0,011). |
| `scientsbank_seqclass_llama3b_seed1` | fester Kopf, kausal | 5e-5 | 1 | 0,735 | 0,619 | 0,625 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `scientsbank_condiff_llama3b_seed1`: UA −0,015 · UQ +0,039 · UD +0,012 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,746 (Δ 0,028) · UQ 0,580 (Δ 0,076) · UD 0,641 (Δ 0,026). |
| `scientsbank_seqclass_llama3b_seed2` | fester Kopf, kausal | 5e-5 | 2 | 0,739 | 0,578 | 0,651 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `scientsbank_condiff_llama3b_seed2`: UA +0,035 · UQ +0,007 · UD +0,018 → im Rauschen. Drei-Saat-Mittel: UA 0,746 (Δ 0,028) · UQ 0,580 (Δ 0,076) · UD 0,641 (Δ 0,026). |
| `scientsbank_seqclass_llama3b_tf4576` | fester Kopf, kausal | 5e-5 | 114514 | 0,747 | 0,557 | 0,648 | Fester Klassifikationskopf mit Rubriktexten unter transformers 4.57.6; Gegenstück zu `_tf5161`. Gegenüber Span-Alignment `scientsbank_condiff_llama3b`: UA −0,003 · UQ −0,019 · UD +0,043 → über der Rauschgrenze auf UD (Einzelvergleich). |
| `scientsbank_seqclass_llama3b_tf5161` | fester Kopf, kausal | 5e-5 | 114514 | 0,763 | 0,543 | 0,648 | Gleiche Konfiguration und Saat wie `scientsbank_seqclass_llama3b_tf4576`, aber transformers 5.16.1: UA +0,016 · UQ −0,014 · UD −0,001. Misst den Einfluss der Bibliotheksversion (Kapitel 5.2). Bildet mit `_seed1` und `_seed2` die Drei-Saat-Gruppe. Drei-Saat-Mittel: UA 0,746 (Δ 0,028) · UQ 0,580 (Δ 0,076) · UD 0,641 (Δ 0,026). |
| `scientsbank_seqclass_norub_llama3b` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 114514 | 0,499 | 0,089 | 0,219 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama3b_tf5161`: UA −0,264 · UQ −0,454 · UD −0,429 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,491 (Δ 0,025) · UQ 0,052 (Δ 0,063) · UD 0,201 (Δ 0,029). |
| `scientsbank_seqclass_norub_llama3b_seed1` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 1 | 0,500 | 0,039 | 0,190 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama3b_seed1`: UA −0,236 · UQ −0,580 · UD −0,435 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,491 (Δ 0,025) · UQ 0,052 (Δ 0,063) · UD 0,201 (Δ 0,029). |
| `scientsbank_seqclass_norub_llama3b_seed2` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 2 | 0,474 | 0,026 | 0,194 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `scientsbank_seqclass_llama3b_seed2`: UA −0,265 · UQ −0,552 · UD −0,458 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). Drei-Saat-Mittel: UA 0,491 (Δ 0,025) · UQ 0,052 (Δ 0,063) · UD 0,201 (Δ 0,029). |
| `scientsbank_tconcat_llama3b` | t-concat, kausal | 5e-5 | 114514 | 0,762 | 0,601 | 0,608 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `scientsbank_concat_llama3b`: UA +0,046 · UQ +0,003 · UD −0,002 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,734 (Δ 0,066) · UQ 0,584 (Δ 0,059) · UD 0,615 (Δ 0,019). |
| `scientsbank_tconcat_llama3b_seed1` | t-concat, kausal | 5e-5 | 1 | 0,744 | 0,604 | 0,627 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `scientsbank_concat_llama3b_seed1`: UA +0,043 · UQ +0,007 · UD −0,018 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,734 (Δ 0,066) · UQ 0,584 (Δ 0,059) · UD 0,615 (Δ 0,019). |
| `scientsbank_tconcat_llama3b_seed2` | t-concat, kausal | 5e-5 | 2 | 0,696 | 0,545 | 0,610 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `scientsbank_concat_llama3b_seed2`: UA −0,048 · UQ −0,005 · UD −0,014 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,734 (Δ 0,066) · UQ 0,584 (Δ 0,059) · UD 0,615 (Δ 0,019). |

### Mistral-7B-v0.1

| Lauf | Konfiguration | lr | Saat | UA | UQ | UD | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `scientsbank_bidir02_mistral7b` ¹ | CONDIFF, 6 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,766 | 0,627 | 0,652 | Attention-Maske: 6 von 32 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_mistral7b`: UA +0,023 · UQ +0,005 · UD +0,017 → im Rauschen. |
| `scientsbank_bidir06_mistral7b` ¹ | CONDIFF, 19 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,650 | 0,378 | 0,393 | Attention-Maske: 19 von 32 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_mistral7b`: UA −0,093 · UQ −0,243 · UD −0,241 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). **Instabiler Lauf wie `beetle_bidir02_mistral7b`; von der Deutung ausgenommen.** |
| `scientsbank_bidir10_mistral7b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,754 | 0,618 | 0,650 | Attention-Maske: 1 von 32 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `scientsbank_condiff_mistral7b`: UA +0,011 · UQ −0,003 · UD +0,016 → im Rauschen. |
| `scientsbank_condiff_mistral7b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,743 | 0,621 | 0,635 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. |

### RoBERTa-base

| Lauf | Konfiguration | lr | Saat | UA | UQ | UD | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `scientsbank_condiff_roberta` ¹ | CONDIFF, kausal | ? ¹ | 114514 | 0,633 | 0,451 | 0,472 | Encoder-Baseline (Hauptlauf, erste Trainingswelle, Lernrate nicht dokumentiert). Gegenüber Llama-3.2-1B `scientsbank_condiff_llama1b`: UA −0,072 · UQ −0,131 · UD −0,100 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). |
| `scientsbank_condiff_roberta_batch16_lr1e5` | CONDIFF, kausal | 1e-5 | 114514 | 0,634 | 0,486 | 0,427 | Lernratenvariation des Encoders: Lernrate 1·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA ±0,000 · UQ +0,035 · UD −0,046. Gegenüber Llama-3.2-1B: UA −0,072 · UQ −0,096 · UD −0,145. |
| `scientsbank_condiff_roberta_batch16_lr2e5` | CONDIFF, kausal | 2e-5 | 114514 | −0,054 | 0,028 | −0,010 | Lernratenvariation des Encoders: Lernrate 2·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA −0,687 · UQ −0,423 · UD −0,482. Gegenüber Llama-3.2-1B: UA −0,760 · UQ −0,554 · UD −0,582. **Gescheitert: Accuracy unter der Mehrheitsschranke von 0,431.** |
| `scientsbank_condiff_roberta_batch16_lr5e5` | CONDIFF, kausal | 5e-5 | 114514 | 0,049 | 0,078 | −0,018 | Lernratenvariation des Encoders: Lernrate 5·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA −0,584 · UQ −0,374 · UD −0,490. Gegenüber Llama-3.2-1B: UA −0,656 · UQ −0,504 · UD −0,590. **Gescheitert: Accuracy unter der Mehrheitsschranke von 0,431.** |

### BERT-base-uncased

| Lauf | Konfiguration | lr | Saat | UA | UQ | UD | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `scientsbank_condiff_bert` ¹ | CONDIFF, kausal | ? ¹ | 114514 | 0,052 | 0,061 | −0,004 | Encoder-Baseline (Hauptlauf, erste Trainingswelle, Lernrate nicht dokumentiert). Gegenüber Llama-3.2-1B `scientsbank_condiff_llama1b`: UA −0,654 · UQ −0,520 · UD −0,576 → über der Rauschgrenze auf UA, UQ, UD (Einzelvergleich). **Ausfall: Accuracy genau auf der Mehrheitsschranke (0,431), Ergebnis auf Zufallsniveau.** |
| `scientsbank_condiff_bert_batch16_lr1e5` | CONDIFF, kausal | 1e-5 | 114514 | 0,006 | −0,023 | −0,049 | Lernratenvariation des Encoders: Lernrate 1·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA −0,046 · UQ −0,084 · UD −0,045. Gegenüber Llama-3.2-1B: UA −0,699 · UQ −0,605 · UD −0,621. **Gescheitert: Accuracy unter der Mehrheitsschranke von 0,431.** |
| `scientsbank_condiff_bert_batch16_lr2e5` | CONDIFF, kausal | 2e-5 | 114514 | 0,009 | 0,076 | −0,021 | Lernratenvariation des Encoders: Lernrate 2·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA −0,043 · UQ +0,014 · UD −0,017. Gegenüber Llama-3.2-1B: UA −0,696 · UQ −0,506 · UD −0,593. **Gescheitert: Accuracy unter der Mehrheitsschranke von 0,431.** |
| `scientsbank_condiff_bert_batch32_lr3e5` | CONDIFF, kausal | 3e-5 | 114514 | −0,026 | 0,020 | −0,050 | Lernratenvariation des Encoders: Lernrate 3·10⁻⁵, Batchgröße 32. Gegenüber dem Encoder-Hauptlauf: UA −0,078 · UQ −0,042 · UD −0,046. Gegenüber Llama-3.2-1B: UA −0,731 · UQ −0,562 · UD −0,622. **Gescheitert: Accuracy unter der Mehrheitsschranke von 0,431.** |

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

- **BERT scheitert auf diesem Benchmark vollständig**, bei allen geprüften Lernraten. Die Ursache ist ungeklärt.
- **RoBERTa scheitert bei 2·10⁻⁵ und 5·10⁻⁵** (Accuracy unter der Mehrheitsschranke von 0,431).
- **`scientsbank_bidir06_mistral7b`** fällt deutlich ab; vermutlich eine Trainingsinstabilität.
- **Mistral-7B ist unvollständig:** Es fehlen `concat`, `gate`, `bidir04` und `bidir08`.
- **Gemischte Softwareumgebungen:** In mehreren Drei-Saat-Gruppen stammt der Lauf mit Saat 114514 aus einer anderen Umgebung als die Saaten 1 und 2. Für den festen Kopf auf Llama-3.2-3B wird deshalb `_tf5161` verwendet. Details in Kapitel 5.2 der Arbeit.
