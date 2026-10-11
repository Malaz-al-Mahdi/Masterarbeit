# Ergebnisse: Beetle

Erklärungs- und Definitionsfragen aus der elementaren Elektrizitätslehre — eine einzige, eng umgrenzte Fachdomäne.

47 Trainingsfragen, einheitlich dreistufig ordinal. Der UQ-Split enthält 9 Fragen, die im Training **nicht** vorkommen. Die enge Domäne ist der vermutete Grund dafür, dass das Modell hier auch ohne Rubriktexte auf neue Fragen generalisiert — anders als auf SciEntsBank.

**Umfang:** 3.941 Trainingsinstanzen; UA 439, UQ 819

Insgesamt liegen hier **86 Trainingsläufe**.

## Was die Ergebnisse aussagen

Die folgenden Aussagen stützen sich, wo nicht anders vermerkt, auf Mittelwerte über drei Zufallssaaten. Ein Unterschied gilt als **Effekt**, wenn die Differenz der Mittelwerte die größere der beiden Spannweiten übersteigt, als **Hinweis**, wenn sich die Spannweiten nicht überlappen, die Differenz aber kleiner ist. Die Tabellen unten zeigen jedes Urteil unter QWK und unter Macro-F1.

- **Ohne Aufgabeninformation generalisiert das Modell hier trotzdem.** Entzieht man die Rubriktexte, verliert der feste Kopf auf UA 0,054 QWK (Hinweis; unter Macro-F1 ein Effekt), auf UQ nichts (−0,001). Anders als auf SciEntsBank bleibt die Accuracy auf neuen Fragen klar über der Mehrheitsschranke. Vermutete Ursache ist die enge, homogene Domäne.
- **Frage und Musterlösung wirken stärker als die Rubrik.** Mit drei Saaten bringt der Aufgabenkontext +0,120 (UA) und +0,102 (UQ) QWK zusätzlich zur Rubrik und +0,210 / +0,149 ohne sie; Kontext allein übertrifft Rubrik allein um +0,157 / +0,150. Alle diese Unterschiede sind unter beiden Metriken Effekte. Bei vorhandenem Kontext schadet die Rubrik sogar: −0,036 auf UA (Hinweis) und −0,048 auf UQ (Effekt unter QWK, Hinweis unter Macro-F1). Llama-3.2-1B mit Kontext ohne Rubrik erreicht im Mittel 0,797 / 0,606 QWK und liegt auf UQ gleichauf mit Mistral-7B (0,605).
- **Span-Alignment schlägt den festen Kopf nur beim kleinen Backbone.** Mit Llama-3.2-1B liegt es auf UA um 0,088 QWK vorn (Effekt unter beiden Metriken, auch mit acht Saaten: +0,092). Mit Llama-3.2-3B ist kein Unterschied nachweisbar (+0,008 / +0,027); der feste Kopf streut dort stark (Spannweite 0,081 / 0,089).
- **Die Fusionsmechanismen sind nicht zu unterscheiden** — auf keinem der drei Backbones. Unter QWK ist kein Unterschied zwischen CONDIFF, CONCAT, DIFF und GATE als Effekt nachweisbar. Einzige Ausnahme unter Macro-F1: GATE liegt auf Llama-3.2-3B auf UA vor DIFF, um nur 0,009. Bei Mistral-7B liegen die drei Varianten auf UA im Mittel nur 0,008 QWK auseinander; der einzige Hinweis (CONDIFF vor GATE auf UQ) entfällt unter Macro-F1.
- **Der Antwort-Span als Bezugsgröße bringt höchstens wenig.** t-concat liegt auf UA 0,026 QWK vor CONCAT (Hinweis; unter Macro-F1 knapp ein Effekt), auf UQ gleichauf.
- **Vollständige Bidirektionalität schadet dem größeren Backbone deutlich.** Llama-3.2-3B verliert 0,073 QWK auf UA und 0,186 auf UQ. Der Gewinn von Llama-3.2-1B auf UA (+0,036 mit drei Saaten) hält der Erweiterung der kausalen Referenz auf acht Saaten nicht stand (+0,033, nicht nachweisbar); auf UQ verliert es unter Macro-F1 (−0,041, auch mit acht Saaten ein Effekt).
- **Die höhere Lernrate bringt keinen gesicherten Gewinn.** Ein Einzellauf von Llama-3.2-1B bei 2·10⁻⁴ (Saat 114514) erreichte 0,779 / 0,605 QWK und schien den Abstand zu Mistral-7B vollständig zu schließen. Mit drei Saaten liegt das Mittel bei 0,766 / 0,551, also nur +0,036 / +0,042 über 5·10⁻⁵ — wegen der stark wachsenden Streuung (Spannweite 0,071 / 0,097) nicht nachweisbar, unter keiner der beiden Metriken. Gegenüber Mistral-7B ist der Abstand bei 5·10⁻⁵ ein Effekt (0,050 / 0,095), bei 2·10⁻⁴ nicht mehr nachweisbar (0,013 / 0,053). Für Llama-3.2-3B liegt bei 2·10⁻⁴ nur ein Einzellauf vor: UA +0,027 über dem Bereich, UQ innerhalb.
- **Drei Saaten können die Streuung unterschätzen.** Über acht Saaten beträgt die Spannweite von CONDIFF mit Llama-3.2-1B auf UA 0,079 statt 0,023 bei den drei ursprünglichen; der Median aller 56 Dreiergruppen (0,039 / 0,034) bestätigt dagegen die Rauschgrenze von 0,036 (Kapitel 5.2).
- **RoBERTa bricht auf UQ ein, bei jeder Lernrate.** Auf UA erreicht es bis zu 0,768, auf UQ nur 0,288 bis 0,362, gegenüber 0,509 für Llama-3.2-1B.
- **Training ist reproduzierbar.** Die beiden `_seed114514_v2`-Läufe stimmen exakt mit ihren Originalen überein.

### Drei-Saat-Gruppen

Mittelwert über drei Zufallssaaten, Spannweite in Klammern; die Zeile „acht Saaten“ fasst alle acht CONDIFF-Läufe von Llama-3.2-1B zusammen. „lr 2·10⁻⁴“ bezeichnet die Läufe bei der Referenzlernrate der Implementierung.

| Gruppe | Backbone | UA QWK | UA F1 | UQ QWK | UQ F1 |
| --- | --- | --- | --- | --- | --- |
| CONDIFF, vollständig bidirektional | Llama-3.2-1B | 0,765 (0,017) | 0,754 (0,009) | 0,493 (0,058) | 0,562 (0,018) |
| CONCAT, kausal | Llama-3.2-1B | 0,725 (0,016) | 0,734 (0,007) | 0,503 (0,021) | 0,599 (0,018) |
| CONDIFF, kausal | Llama-3.2-1B | 0,729 (0,023) | 0,739 (0,006) | 0,509 (0,073) | 0,603 (0,028) |
| GATE, kausal | Llama-3.2-1B | 0,696 (0,051) | 0,732 (0,048) | 0,471 (0,118) | 0,562 (0,046) |
| fester Kopf, Rubrik | Llama-3.2-1B | 0,641 (0,055) | 0,681 (0,020) | 0,456 (0,068) | 0,559 (0,041) |
| fester Kopf, weder Rubrik noch Kontext | Llama-3.2-1B | 0,587 (0,011) | 0,641 (0,034) | 0,457 (0,037) | 0,561 (0,024) |
| CONDIFF, kausal, acht Saaten | Llama-3.2-1B | 0,732 (0,079) | 0,742 (0,050) | 0,525 (0,073) | 0,601 (0,034) |
| fester Kopf, Kontext und Rubrik | Llama-3.2-1B | 0,761 (0,037) | 0,751 (0,060) | 0,557 (0,033) | 0,636 (0,020) |
| fester Kopf, nur Kontext | Llama-3.2-1B | 0,797 (0,025) | 0,779 (0,033) | 0,606 (0,009) | 0,656 (0,004) |
| CONDIFF, kausal, lr 2·10⁻⁴ | Llama-3.2-1B | 0,766 (0,071) | 0,752 (0,040) | 0,551 (0,097) | 0,603 (0,028) |
| CONDIFF, vollständig bidirektional | Llama-3.2-3B | 0,657 (0,061) | 0,697 (0,060) | 0,358 (0,094) | 0,526 (0,056) |
| CONCAT, kausal | Llama-3.2-3B | 0,735 (0,042) | 0,737 (0,013) | 0,546 (0,046) | 0,602 (0,016) |
| CONDIFF, kausal | Llama-3.2-3B | 0,730 (0,041) | 0,725 (0,025) | 0,544 (0,044) | 0,596 (0,027) |
| DIFF, kausal | Llama-3.2-3B | 0,740 (0,010) | 0,734 (0,008) | 0,572 (0,048) | 0,605 (0,016) |
| GATE, kausal | Llama-3.2-3B | 0,757 (0,022) | 0,743 (0,006) | 0,550 (0,044) | 0,608 (0,021) |
| t-concat, kausal | Llama-3.2-3B | 0,761 (0,006) | 0,750 (0,004) | 0,552 (0,019) | 0,602 (0,014) |
| fester Kopf, Rubrik | Llama-3.2-3B | 0,722 (0,081) | 0,720 (0,032) | 0,517 (0,089) | 0,578 (0,055) |
| CONCAT, kausal | Mistral-7B | 0,772 (0,037) | 0,757 (0,041) | 0,604 (0,058) | 0,632 (0,024) |
| CONDIFF, kausal | Mistral-7B | 0,779 (0,046) | 0,766 (0,009) | 0,605 (0,007) | 0,628 (0,008) |
| GATE, kausal | Mistral-7B | 0,781 (0,020) | 0,758 (0,032) | 0,567 (0,044) | 0,615 (0,026) |

### Alle Vergleiche mit Urteil

Δ ist die Differenz der Mittelwerte, erste minus zweite Bedingung. „—“ bedeutet nicht nachweisbar.

| Vergleich | Kapitel | Split | Δ QWK | Urteil QWK | Δ Macro-F1 | Urteil Macro-F1 |
| --- | --- | --- | --- | --- | --- | --- |
| Span-Alignment gegen festen Kopf (1B) | 5.3.1 | UA | +0,088 | **Effekt** | +0,058 | **Effekt** |
| Span-Alignment gegen festen Kopf (1B) | 5.3.1 | UQ | +0,054 | — | +0,044 | **Effekt** |
| Span-Alignment gegen festen Kopf (1B, acht Saaten) | 5.3.1 | UA | +0,092 | **Effekt** | +0,060 | **Effekt** |
| Span-Alignment gegen festen Kopf (1B, acht Saaten) | 5.3.1 | UQ | +0,069 | — | +0,042 | **Effekt** |
| Span-Alignment gegen festen Kopf (3B) | 5.3.1 | UA | +0,008 | — | +0,005 | — |
| Span-Alignment gegen festen Kopf (3B) | 5.3.1 | UQ | +0,027 | — | +0,018 | — |
| Rubrik gegen weder Rubrik noch Kontext (1B) | 5.3.2 | UA | +0,054 | Hinweis | +0,040 | **Effekt** |
| Rubrik gegen weder Rubrik noch Kontext (1B) | 5.3.2 | UQ | −0,001 | — | −0,002 | — |
| Kontext mit gegen ohne Rubrik (1B) | 5.3.2 | UA | −0,036 | Hinweis | −0,027 | — |
| Kontext mit gegen ohne Rubrik (1B) | 5.3.2 | UQ | −0,048 | **Effekt** | −0,019 | Hinweis |
| nur Kontext gegen nur Rubrik (1B) | 5.3.2 | UA | +0,157 | **Effekt** | +0,097 | **Effekt** |
| nur Kontext gegen nur Rubrik (1B) | 5.3.2 | UQ | +0,150 | **Effekt** | +0,097 | **Effekt** |
| Kontext und Rubrik gegen nur Rubrik (1B) | 5.3.2 | UA | +0,120 | **Effekt** | +0,070 | **Effekt** |
| Kontext und Rubrik gegen nur Rubrik (1B) | 5.3.2 | UQ | +0,102 | **Effekt** | +0,077 | **Effekt** |
| nur Kontext gegen weder Rubrik noch Kontext (1B) | 5.3.2 | UA | +0,210 | **Effekt** | +0,138 | **Effekt** |
| nur Kontext gegen weder Rubrik noch Kontext (1B) | 5.3.2 | UQ | +0,149 | **Effekt** | +0,095 | **Effekt** |
| CONDIFF gegen CONCAT (1B) | 5.4.1 | UA | +0,005 | — | +0,006 | Hinweis |
| CONDIFF gegen CONCAT (1B) | 5.4.1 | UQ | +0,006 | — | +0,005 | — |
| CONDIFF gegen GATE (1B) | 5.4.1 | UA | +0,033 | — | +0,008 | — |
| CONDIFF gegen GATE (1B) | 5.4.1 | UQ | +0,038 | — | +0,041 | Hinweis |
| CONCAT gegen GATE (1B) | 5.4.1 | UA | +0,028 | — | +0,002 | — |
| CONCAT gegen GATE (1B) | 5.4.1 | UQ | +0,032 | — | +0,036 | Hinweis |
| CONDIFF gegen CONCAT (3B) | 5.4.1 | UA | −0,005 | — | −0,012 | — |
| CONDIFF gegen CONCAT (3B) | 5.4.1 | UQ | −0,002 | — | −0,006 | — |
| CONDIFF gegen DIFF (3B) | 5.4.1 | UA | −0,010 | — | −0,009 | — |
| CONDIFF gegen DIFF (3B) | 5.4.1 | UQ | −0,028 | — | −0,009 | — |
| CONDIFF gegen GATE (3B) | 5.4.1 | UA | −0,027 | Hinweis | −0,018 | Hinweis |
| CONDIFF gegen GATE (3B) | 5.4.1 | UQ | −0,006 | — | −0,012 | — |
| CONCAT gegen DIFF (3B) | 5.4.1 | UA | −0,005 | — | +0,003 | — |
| CONCAT gegen DIFF (3B) | 5.4.1 | UQ | −0,026 | — | −0,003 | — |
| CONCAT gegen GATE (3B) | 5.4.1 | UA | −0,022 | — | −0,006 | — |
| CONCAT gegen GATE (3B) | 5.4.1 | UQ | −0,004 | — | −0,006 | — |
| DIFF gegen GATE (3B) | 5.4.1 | UA | −0,017 | Hinweis | −0,009 | **Effekt** |
| DIFF gegen GATE (3B) | 5.4.1 | UQ | +0,022 | — | −0,003 | — |
| CONDIFF gegen CONCAT (7B) | 5.4.1 | UA | +0,007 | — | +0,009 | — |
| CONDIFF gegen CONCAT (7B) | 5.4.1 | UQ | ±0,000 | — | −0,004 | — |
| CONDIFF gegen GATE (7B) | 5.4.1 | UA | −0,002 | — | +0,008 | — |
| CONDIFF gegen GATE (7B) | 5.4.1 | UQ | +0,038 | Hinweis | +0,013 | — |
| CONCAT gegen GATE (7B) | 5.4.1 | UA | −0,008 | — | −0,001 | — |
| CONCAT gegen GATE (7B) | 5.4.1 | UQ | +0,037 | — | +0,017 | — |
| t-concat gegen CONCAT (3B) | 5.4.2 | UA | +0,026 | Hinweis | +0,014 | **Effekt** |
| t-concat gegen CONCAT (3B) | 5.4.2 | UQ | +0,006 | — | +0,001 | — |
| vollständig bidirektional gegen kausal (1B) | 5.4.3 | UA | +0,036 | **Effekt** | +0,015 | **Effekt** |
| vollständig bidirektional gegen kausal (1B) | 5.4.3 | UQ | −0,016 | — | −0,041 | **Effekt** |
| vollständig bidirektional gegen kausal (3B) | 5.4.3 | UA | −0,073 | **Effekt** | −0,028 | — |
| vollständig bidirektional gegen kausal (3B) | 5.4.3 | UQ | −0,186 | **Effekt** | −0,070 | **Effekt** |
| vollständig bidirektional gegen kausal (1B, acht Saaten) | 5.4.3 | UA | +0,033 | — | +0,012 | — |
| vollständig bidirektional gegen kausal (1B, acht Saaten) | 5.4.3 | UQ | −0,032 | — | −0,038 | **Effekt** |
| lr 2·10⁻⁴ gegen 5·10⁻⁵, CONDIFF (1B) | 5.4.4 | UA | +0,036 | — | +0,013 | — |
| lr 2·10⁻⁴ gegen 5·10⁻⁵, CONDIFF (1B) | 5.4.4 | UQ | +0,042 | — | ±0,000 | — |

## Die Läufe im Einzelnen

Jede Zeile ist ein Trainingslauf. Die Werte sind QWK auf dem jeweiligen Testsplit; Macro-F1 und Accuracy stehen in der `*_metrics.json` des Laufs. Die Spalte **Was der Lauf zeigt** nennt, welche Frage der Lauf beantwortet, und vergleicht ihn mit seiner Referenz bei gleicher Saat. Solche Einzelvergleiche sind gegen die Rauschgrenze von 0,036 QWK zu lesen; belastbar sind erst die Drei-Saat-Vergleiche oben.

### Llama-3.2-1B

| Lauf | Konfiguration | lr | Saat | UA | UQ | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- |
| `beetle_bidir02_llama1b` ¹ | CONDIFF, 3 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,711 | 0,526 | Attention-Maske: 3 von 16 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b`: UA +0,015 · UQ −0,006 → im Rauschen. |
| `beetle_bidir04_llama1b` ¹ | CONDIFF, 6 Schichten bidirektional (ρ = 0,4) | 5e-5 ¹ | 114514 | 0,691 | 0,472 | Attention-Maske: 6 von 16 Schichten bidirektional, ρ = 0,4, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b`: UA −0,005 · UQ −0,060 → über der Rauschgrenze auf UQ (Einzelvergleich). |
| `beetle_bidir06_llama1b` ¹ | CONDIFF, 9 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,724 | 0,503 | Attention-Maske: 9 von 16 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b`: UA +0,027 · UQ −0,029 → im Rauschen. |
| `beetle_bidir08_llama1b` ¹ | CONDIFF, 12 Schichten bidirektional (ρ = 0,8) | 5e-5 ¹ | 114514 | 0,750 | 0,413 | Attention-Maske: 12 von 16 Schichten bidirektional, ρ = 0,8, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b`: UA +0,053 · UQ −0,119 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). |
| `beetle_bidir10_llama1b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,707 | 0,544 | Attention-Maske: 1 von 16 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b`: UA +0,011 · UQ +0,012 → im Rauschen. |
| `beetle_bidirfull_llama1b` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,754 | 0,462 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b`: UA +0,058 · UQ −0,070 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,765 (Δ 0,017) · UQ 0,493 (Δ 0,058). |
| `beetle_bidirfull_llama1b_seed1` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 1 | 0,770 | 0,520 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b_seed1`: UA +0,029 · UQ +0,010 → im Rauschen. Drei-Saat-Mittel: UA 0,765 (Δ 0,017) · UQ 0,493 (Δ 0,058). |
| `beetle_bidirfull_llama1b_seed2` | CONDIFF, 16 Schichten bidirektional (ρ = L) | 5e-5 | 2 | 0,771 | 0,497 | Attention-Maske: alle 16 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama1b_seed2`: UA +0,043 · UQ −0,048 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,765 (Δ 0,017) · UQ 0,493 (Δ 0,058). |
| `beetle_concat_llama1b` ¹ | CONCAT, kausal | 5e-5 ¹ | 114514 | 0,731 | 0,514 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama1b`: UA +0,034 · UQ −0,018 → im Rauschen. Drei-Saat-Mittel: UA 0,725 (Δ 0,016) · UQ 0,503 (Δ 0,021). |
| `beetle_concat_llama1b_seed1` | CONCAT, kausal | 5e-5 | 1 | 0,715 | 0,494 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama1b_seed1`: UA −0,027 · UQ −0,017 → im Rauschen. Drei-Saat-Mittel: UA 0,725 (Δ 0,016) · UQ 0,503 (Δ 0,021). |
| `beetle_concat_llama1b_seed2` | CONCAT, kausal | 5e-5 | 2 | 0,729 | 0,501 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama1b_seed2`: UA ±0,000 · UQ −0,044 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,725 (Δ 0,016) · UQ 0,503 (Δ 0,021). |
| `beetle_condiff_llama1b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,696 | 0,532 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. |
| `beetle_condiff_llama1b_lr2e4` | CONDIFF, kausal | 2e-4 | 114514 | 0,779 | 0,605 | Lernrate 2·10⁻⁴ (Referenzwert der Implementierung für dieses Backbone) statt 5·10⁻⁵. Gegenüber `beetle_condiff_llama1b`: UA +0,083 · UQ +0,073 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Grundlage der Aussage, dass der Größenvergleich mit der Lernrate konfundiert ist (Kapitel 5.4.4). Drei-Saat-Mittel: UA 0,766 (Δ 0,071) · UQ 0,551 (Δ 0,097). Mit drei Saaten ist der Vorsprung gegenüber 5·10⁻⁵ nicht mehr nachweisbar; der Einzellauf war der beste der drei. |
| `beetle_condiff_llama1b_lr2e4_seed1` | CONDIFF, kausal | 2e-4 | 1 | 0,794 | 0,541 | Lernrate 2·10⁻⁴ (Referenzwert der Implementierung für dieses Backbone) statt 5·10⁻⁵, Wiederholung mit Saat 1. Gegenüber `beetle_condiff_llama1b_seed1`: UA +0,053 · UQ +0,030 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,766 (Δ 0,071) · UQ 0,551 (Δ 0,097). |
| `beetle_condiff_llama1b_lr2e4_seed2` | CONDIFF, kausal | 2e-4 | 2 | 0,724 | 0,508 | Lernrate 2·10⁻⁴ (Referenzwert der Implementierung für dieses Backbone) statt 5·10⁻⁵, Wiederholung mit Saat 2. Gegenüber `beetle_condiff_llama1b_seed2`: UA −0,005 · UQ −0,037 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,766 (Δ 0,071) · UQ 0,551 (Δ 0,097). |
| `beetle_condiff_llama1b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,741 | 0,511 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,729 (Δ 0,023) · UQ 0,509 (Δ 0,073). |
| `beetle_condiff_llama1b_seed114514_verified` | CONDIFF, kausal | 5e-5 | 114514 | 0,718 | 0,472 | Wiederholung von `beetle_condiff_llama1b` mit derselben Saat, aber neuerer Softwareumgebung: UA +0,022 · UQ −0,060. Die Abweichung zeigt den Einfluss der Bibliotheksversion, nicht Zufall. Bildet mit `_seed1` und `_seed2` die Drei-Saat-Gruppe. Drei-Saat-Mittel: UA 0,729 (Δ 0,023) · UQ 0,509 (Δ 0,073). |
| `beetle_condiff_llama1b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,728 | 0,545 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,729 (Δ 0,023) · UQ 0,509 (Δ 0,073). |
| `beetle_condiff_llama1b_seed3` | CONDIFF, kausal | 5e-5 | 3 | 0,731 | 0,534 | Zusatzsaat zur Prüfung der Rauschgrenze (Kapitel 5.2). Gegenüber dem Mittel der drei ursprünglichen Saaten: UA +0,002 · UQ +0,024. Acht-Saat-Mittel: UA 0,732 (Δ 0,079) · UQ 0,525 (Δ 0,073). |
| `beetle_condiff_llama1b_seed4` | CONDIFF, kausal | 5e-5 | 4 | 0,702 | 0,545 | Zusatzsaat zur Prüfung der Rauschgrenze (Kapitel 5.2). Gegenüber dem Mittel der drei ursprünglichen Saaten: UA −0,027 · UQ +0,035. Acht-Saat-Mittel: UA 0,732 (Δ 0,079) · UQ 0,525 (Δ 0,073). |
| `beetle_condiff_llama1b_seed5` | CONDIFF, kausal | 5e-5 | 5 | 0,710 | 0,520 | Zusatzsaat zur Prüfung der Rauschgrenze (Kapitel 5.2). Gegenüber dem Mittel der drei ursprünglichen Saaten: UA −0,019 · UQ +0,011. Acht-Saat-Mittel: UA 0,732 (Δ 0,079) · UQ 0,525 (Δ 0,073). |
| `beetle_condiff_llama1b_seed6` | CONDIFF, kausal | 5e-5 | 6 | 0,781 | 0,535 | Zusatzsaat zur Prüfung der Rauschgrenze (Kapitel 5.2). Gegenüber dem Mittel der drei ursprünglichen Saaten: UA +0,052 · UQ +0,026. Acht-Saat-Mittel: UA 0,732 (Δ 0,079) · UQ 0,525 (Δ 0,073). |
| `beetle_condiff_llama1b_seed7` | CONDIFF, kausal | 5e-5 | 7 | 0,747 | 0,536 | Zusatzsaat zur Prüfung der Rauschgrenze (Kapitel 5.2). Gegenüber dem Mittel der drei ursprünglichen Saaten: UA +0,018 · UQ +0,027. Acht-Saat-Mittel: UA 0,732 (Δ 0,079) · UQ 0,525 (Δ 0,073). |
| `beetle_gate_llama1b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,695 | 0,497 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama1b`: UA −0,001 · UQ −0,035 → im Rauschen. Drei-Saat-Mittel: UA 0,696 (Δ 0,051) · UQ 0,471 (Δ 0,118). |
| `beetle_gate_llama1b_seed1` | GATE, kausal | 5e-5 | 1 | 0,671 | 0,399 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama1b_seed1`: UA −0,070 · UQ −0,111 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,696 (Δ 0,051) · UQ 0,471 (Δ 0,118). |
| `beetle_gate_llama1b_seed2` | GATE, kausal | 5e-5 | 2 | 0,722 | 0,517 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama1b_seed2`: UA −0,006 · UQ −0,028 → im Rauschen. Drei-Saat-Mittel: UA 0,696 (Δ 0,051) · UQ 0,471 (Δ 0,118). |
| `beetle_seqclass_ctx_llama1b` | fester Kopf, kausal, mit Kontext | 5e-5 | 114514 | 0,764 | 0,554 | Frage und Musterlösung zusätzlich zu den Rubriktexten; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b`: UA +0,095 · UQ +0,111 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,761 (Δ 0,037) · UQ 0,557 (Δ 0,033). |
| `beetle_seqclass_ctx_llama1b_seed1` | fester Kopf, kausal, mit Kontext | 5e-5 | 1 | 0,741 | 0,542 | Frage und Musterlösung zusätzlich zu den Rubriktexten; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b_seed1`: UA +0,101 · UQ +0,047 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,761 (Δ 0,037) · UQ 0,557 (Δ 0,033). |
| `beetle_seqclass_ctx_llama1b_seed2` | fester Kopf, kausal, mit Kontext | 5e-5 | 2 | 0,779 | 0,575 | Frage und Musterlösung zusätzlich zu den Rubriktexten; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b_seed2`: UA +0,165 · UQ +0,147 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,761 (Δ 0,037) · UQ 0,557 (Δ 0,033). |
| `beetle_seqclass_ctx_norub_llama1b` | fester Kopf, kausal, ohne Rubrik, mit Kontext | 5e-5 | 114514 | 0,799 | 0,602 | Frage und Musterlösung statt der Rubriktexte; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b`: UA +0,131 · UQ +0,158 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,797 (Δ 0,025) · UQ 0,606 (Δ 0,009). |
| `beetle_seqclass_ctx_norub_llama1b_seed1` | fester Kopf, kausal, ohne Rubrik, mit Kontext | 5e-5 | 1 | 0,784 | 0,611 | Frage und Musterlösung statt der Rubriktexte; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b_seed1`: UA +0,144 · UQ +0,115 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,797 (Δ 0,025) · UQ 0,606 (Δ 0,009). |
| `beetle_seqclass_ctx_norub_llama1b_seed2` | fester Kopf, kausal, ohne Rubrik, mit Kontext | 5e-5 | 2 | 0,809 | 0,604 | Frage und Musterlösung statt der Rubriktexte; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b_seed2`: UA +0,195 · UQ +0,176 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,797 (Δ 0,025) · UQ 0,606 (Δ 0,009). |
| `beetle_seqclass_llama1b` | fester Kopf, kausal | 5e-5 | 114514 | 0,669 | 0,443 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `beetle_condiff_llama1b`: UA −0,028 · UQ −0,089 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,641 (Δ 0,055) · UQ 0,456 (Δ 0,068). |
| `beetle_seqclass_llama1b_seed1` | fester Kopf, kausal | 5e-5 | 1 | 0,640 | 0,496 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `beetle_condiff_llama1b_seed1`: UA −0,101 · UQ −0,015 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,641 (Δ 0,055) · UQ 0,456 (Δ 0,068). |
| `beetle_seqclass_llama1b_seed114514_v2` | fester Kopf, kausal | 5e-5 | 114514 | 0,669 | 0,443 | Reproduktionsprüfung: Wiederholung von `beetle_seqclass_llama1b` mit identischer Saat und Umgebung. Alle Kennzahlen stimmen exakt überein, das Training ist deterministisch. |
| `beetle_seqclass_llama1b_seed2` | fester Kopf, kausal | 5e-5 | 2 | 0,613 | 0,428 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `beetle_condiff_llama1b_seed2`: UA −0,115 · UQ −0,117 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,641 (Δ 0,055) · UQ 0,456 (Δ 0,068). |
| `beetle_seqclass_norub_llama1b` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 114514 | 0,588 | 0,445 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b`: UA −0,080 · UQ +0,001 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,587 (Δ 0,011) · UQ 0,457 (Δ 0,037). |
| `beetle_seqclass_norub_llama1b_seed1` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 1 | 0,592 | 0,481 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b_seed1`: UA −0,049 · UQ −0,015 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,587 (Δ 0,011) · UQ 0,457 (Δ 0,037). |
| `beetle_seqclass_norub_llama1b_seed114514_v2` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 114514 | 0,588 | 0,445 | Reproduktionsprüfung: Wiederholung von `beetle_seqclass_norub_llama1b` mit identischer Saat und Umgebung. Alle Kennzahlen stimmen exakt überein, das Training ist deterministisch. |
| `beetle_seqclass_norub_llama1b_seed2` | fester Kopf, kausal, ohne Rubrik | 5e-5 | 2 | 0,581 | 0,445 | Ablation: Rubriktexte entfernt, keine andere Aufgabeninformation, das Modell sieht nur die Antwort; fester Klassifikationskopf. Gegenüber Rubrik allein `beetle_seqclass_llama1b_seed2`: UA −0,032 · UQ +0,017 → im Rauschen. Drei-Saat-Mittel: UA 0,587 (Δ 0,011) · UQ 0,457 (Δ 0,037). |
| `beetle_tconcat_llama1b` | t-concat, kausal | 5e-5 | 114514 | 0,686 | 0,502 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `beetle_concat_llama1b`: UA −0,045 · UQ −0,012 → über der Rauschgrenze auf UA (Einzelvergleich). |

### Llama-3.2-3B

| Lauf | Konfiguration | lr | Saat | UA | UQ | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- |
| `beetle_bidir02_llama3b` ¹ | CONDIFF, 5 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,753 | 0,571 | Attention-Maske: 5 von 28 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b`: UA +0,005 · UQ −0,002 → im Rauschen. |
| `beetle_bidir04_llama3b` ¹ | CONDIFF, 11 Schichten bidirektional (ρ = 0,4) | 5e-5 ¹ | 114514 | 0,721 | 0,586 | Attention-Maske: 11 von 28 Schichten bidirektional, ρ = 0,4, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b`: UA −0,027 · UQ +0,013 → im Rauschen. |
| `beetle_bidir06_llama3b` ¹ | CONDIFF, 16 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,765 | 0,544 | Attention-Maske: 16 von 28 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b`: UA +0,017 · UQ −0,029 → im Rauschen. |
| `beetle_bidir08_llama3b` ¹ | CONDIFF, 22 Schichten bidirektional (ρ = 0,8) | 5e-5 ¹ | 114514 | 0,759 | 0,542 | Attention-Maske: 22 von 28 Schichten bidirektional, ρ = 0,8, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b`: UA +0,010 · UQ −0,031 → im Rauschen. |
| `beetle_bidir10_llama3b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,758 | 0,590 | Attention-Maske: 1 von 28 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b`: UA +0,009 · UQ +0,017 → im Rauschen. |
| `beetle_bidirfull_llama3b` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 114514 | 0,636 | 0,419 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b`: UA −0,112 · UQ −0,153 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,657 (Δ 0,061) · UQ 0,358 (Δ 0,094). |
| `beetle_bidirfull_llama3b_seed1` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 1 | 0,698 | 0,325 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b_seed1`: UA −0,038 · UQ −0,203 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,657 (Δ 0,061) · UQ 0,358 (Δ 0,094). |
| `beetle_bidirfull_llama3b_seed2` | CONDIFF, 28 Schichten bidirektional (ρ = L) | 5e-5 | 2 | 0,637 | 0,329 | Attention-Maske: alle 28 Schichten bidirektional, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_llama3b_seed2`: UA −0,070 · UQ −0,202 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,657 (Δ 0,061) · UQ 0,358 (Δ 0,094). |
| `beetle_concat_llama3b` ¹ | CONCAT, kausal | 5e-5 ¹ | 114514 | 0,745 | 0,571 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b`: UA −0,003 · UQ −0,002 → im Rauschen. Drei-Saat-Mittel: UA 0,735 (Δ 0,042) · UQ 0,546 (Δ 0,046). |
| `beetle_concat_llama3b_seed1` | CONCAT, kausal | 5e-5 | 1 | 0,709 | 0,525 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b_seed1`: UA −0,026 · UQ −0,004 → im Rauschen. Drei-Saat-Mittel: UA 0,735 (Δ 0,042) · UQ 0,546 (Δ 0,046). |
| `beetle_concat_llama3b_seed2` | CONCAT, kausal | 5e-5 | 2 | 0,751 | 0,544 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b_seed2`: UA +0,044 · UQ +0,013 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,735 (Δ 0,042) · UQ 0,546 (Δ 0,046). |
| `beetle_condiff_llama3b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,748 | 0,573 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. Drei-Saat-Mittel: UA 0,730 (Δ 0,041) · UQ 0,544 (Δ 0,044). |
| `beetle_condiff_llama3b_lr2e4` | CONDIFF, kausal | 2e-4 | 114514 | 0,775 | 0,564 | Lernrate 2·10⁻⁴ (Referenzwert der Implementierung für dieses Backbone) statt 5·10⁻⁵. Gegenüber `beetle_condiff_llama3b`: UA +0,027 · UQ −0,009 → im Rauschen. Gegenüber dem Drei-Saat-Bereich bei 5·10⁻⁵: UA darüber (+0,027), UQ innerhalb (Kapitel 5.4.4). |
| `beetle_condiff_llama3b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,735 | 0,528 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,730 (Δ 0,041) · UQ 0,544 (Δ 0,044). |
| `beetle_condiff_llama3b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,707 | 0,531 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,730 (Δ 0,041) · UQ 0,544 (Δ 0,044). |
| `beetle_diff_llama3b` | DIFF, kausal | 5e-5 | 114514 | 0,734 | 0,552 | Fusionsmechanismus DIFF (Differenzterm z − r allein) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b`: UA −0,014 · UQ −0,021 → im Rauschen. Drei-Saat-Mittel: UA 0,740 (Δ 0,010) · UQ 0,572 (Δ 0,048). |
| `beetle_diff_llama3b_seed1` | DIFF, kausal | 5e-5 | 1 | 0,744 | 0,599 | Fusionsmechanismus DIFF (Differenzterm z − r allein) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b_seed1`: UA +0,009 · UQ +0,071 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,740 (Δ 0,010) · UQ 0,572 (Δ 0,048). |
| `beetle_diff_llama3b_seed2` | DIFF, kausal | 5e-5 | 2 | 0,742 | 0,565 | Fusionsmechanismus DIFF (Differenzterm z − r allein) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b_seed2`: UA +0,036 · UQ +0,035 → im Rauschen. Drei-Saat-Mittel: UA 0,740 (Δ 0,010) · UQ 0,572 (Δ 0,048). |
| `beetle_gate_llama3b` ¹ | GATE, kausal | 5e-5 ¹ | 114514 | 0,771 | 0,570 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b`: UA +0,023 · UQ −0,003 → im Rauschen. Drei-Saat-Mittel: UA 0,757 (Δ 0,022) · UQ 0,550 (Δ 0,044). |
| `beetle_gate_llama3b_seed1` | GATE, kausal | 5e-5 | 1 | 0,749 | 0,556 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b_seed1`: UA +0,013 · UQ +0,027 → im Rauschen. Drei-Saat-Mittel: UA 0,757 (Δ 0,022) · UQ 0,550 (Δ 0,044). |
| `beetle_gate_llama3b_seed2` | GATE, kausal | 5e-5 | 2 | 0,751 | 0,526 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_llama3b_seed2`: UA +0,044 · UQ −0,005 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,757 (Δ 0,022) · UQ 0,550 (Δ 0,044). |
| `beetle_seqclass_llama3b` | fester Kopf, kausal | 5e-5 | 114514 | 0,730 | 0,534 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `beetle_condiff_llama3b`: UA −0,018 · UQ −0,039 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,722 (Δ 0,081) · UQ 0,517 (Δ 0,089). |
| `beetle_seqclass_llama3b_seed1` | fester Kopf, kausal | 5e-5 | 1 | 0,678 | 0,464 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `beetle_condiff_llama3b_seed1`: UA −0,058 · UQ −0,064 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,722 (Δ 0,081) · UQ 0,517 (Δ 0,089). |
| `beetle_seqclass_llama3b_seed2` | fester Kopf, kausal | 5e-5 | 2 | 0,758 | 0,553 | Baseline mit festem Klassifikationskopf: Rubriktexte in der Eingabe, aber kein Span-Alignment. Gegenüber Span-Alignment `beetle_condiff_llama3b_seed2`: UA +0,051 · UQ +0,022 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,722 (Δ 0,081) · UQ 0,517 (Δ 0,089). |
| `beetle_tconcat_llama3b` | t-concat, kausal | 5e-5 | 114514 | 0,760 | 0,544 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `beetle_concat_llama3b`: UA +0,015 · UQ −0,026 → im Rauschen. Drei-Saat-Mittel: UA 0,761 (Δ 0,006) · UQ 0,552 (Δ 0,019). |
| `beetle_tconcat_llama3b_seed1` | t-concat, kausal | 5e-5 | 1 | 0,764 | 0,549 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `beetle_concat_llama3b_seed1`: UA +0,055 · UQ +0,024 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,761 (Δ 0,006) · UQ 0,552 (Δ 0,019). |
| `beetle_tconcat_llama3b_seed2` | t-concat, kausal | 5e-5 | 2 | 0,758 | 0,563 | Bezugsgröße der Fusion: Antwort-Span statt Sequenzrepräsentation, sonst wie CONCAT. Gegenüber `beetle_concat_llama3b_seed2`: UA +0,007 · UQ +0,020 → im Rauschen. Drei-Saat-Mittel: UA 0,761 (Δ 0,006) · UQ 0,552 (Δ 0,019). |

### Mistral-7B-v0.1

| Lauf | Konfiguration | lr | Saat | UA | UQ | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- |
| `beetle_bidir02_mistral7b` ¹ | CONDIFF, 6 Schichten bidirektional (ρ = 0,2) | 5e-5 ¹ | 114514 | 0,587 | 0,210 | Attention-Maske: 6 von 32 Schichten bidirektional, ρ = 0,2, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_mistral7b`: UA −0,216 · UQ −0,396 → über der Rauschgrenze auf UA, UQ (Einzelvergleich). **Instabiler Lauf: sagt alle Stufen vorher, liegt aber weit unter allen übrigen Mistral-Bedingungen; in Kapitel 5.5.2 von der Deutung ausgenommen.** Die Wiederholung `beetle_bidir02_mistral7b_v2` liegt im Bereich der übrigen Mistral-Läufe. |
| `beetle_bidir02_mistral7b_v2` | CONDIFF, 6 Schichten bidirektional (ρ = 0,2) | 5e-5 | 114514 | 0,778 | 0,593 | Wiederholung des instabilen Laufs `beetle_bidir02_mistral7b` mit derselben Saat in neuerer Umgebung (transformers 4.57.6). Gegenüber kausaler Referenz `beetle_condiff_mistral7b`: UA −0,024 · UQ −0,013 → im Rauschen. Der ursprüngliche Einbruch war kein Effekt der Maske (Kapitel 5.5.2). |
| `beetle_bidir06_mistral7b` ¹ | CONDIFF, 19 Schichten bidirektional (ρ = 0,6) | 5e-5 ¹ | 114514 | 0,801 | 0,609 | Attention-Maske: 19 von 32 Schichten bidirektional, ρ = 0,6, Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_mistral7b`: UA −0,002 · UQ +0,002 → im Rauschen. |
| `beetle_bidir10_mistral7b` ¹ | CONDIFF, 1 Schicht bidirektional (ρ = 1,0) | 5e-5 ¹ | 114514 | 0,793 | 0,592 | Attention-Maske: 1 von 32 Schichten bidirektional, ρ = 1,0 (ρ = 1,0 bedeutet genau eine Schicht, nicht das volle Modell), Fusion CONDIFF. Gegenüber kausaler Referenz `beetle_condiff_mistral7b`: UA −0,010 · UQ −0,014 → im Rauschen. |
| `beetle_concat_mistral7b` | CONCAT, kausal | 5e-5 | 114514 | 0,756 | 0,610 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_mistral7b`: UA −0,047 · UQ +0,004 → über der Rauschgrenze auf UA (Einzelvergleich). Drei-Saat-Mittel: UA 0,772 (Δ 0,037) · UQ 0,604 (Δ 0,058). |
| `beetle_concat_mistral7b_seed1` | CONCAT, kausal | 5e-5 | 1 | 0,794 | 0,631 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_mistral7b_seed1`: UA +0,016 · UQ +0,030 → im Rauschen. Drei-Saat-Mittel: UA 0,772 (Δ 0,037) · UQ 0,604 (Δ 0,058). |
| `beetle_concat_mistral7b_seed2` | CONCAT, kausal | 5e-5 | 2 | 0,768 | 0,572 | Fusionsmechanismus CONCAT (Konkatenation [z ‖ r] ohne Differenzterm) statt CONDIFF, kausal. Gegenüber `beetle_condiff_mistral7b_seed2`: UA +0,011 · UQ −0,035 → im Rauschen. Drei-Saat-Mittel: UA 0,772 (Δ 0,037) · UQ 0,604 (Δ 0,058). |
| `beetle_condiff_mistral7b` ¹ | CONDIFF, kausal | 5e-5 ¹ | 114514 | 0,803 | 0,606 | Referenzbedingung: Span-Alignment mit CONDIFF und kausaler Maske. Vergleichsbasis für Fusion, Maske und festen Kopf. Drei-Saat-Mittel: UA 0,779 (Δ 0,046) · UQ 0,605 (Δ 0,007). |
| `beetle_condiff_mistral7b_seed1` | CONDIFF, kausal | 5e-5 | 1 | 0,777 | 0,600 | Wiederholung der CONDIFF-Referenz mit Saat 1. Drei-Saat-Mittel: UA 0,779 (Δ 0,046) · UQ 0,605 (Δ 0,007). |
| `beetle_condiff_mistral7b_seed2` | CONDIFF, kausal | 5e-5 | 2 | 0,757 | 0,607 | Wiederholung der CONDIFF-Referenz mit Saat 2. Drei-Saat-Mittel: UA 0,779 (Δ 0,046) · UQ 0,605 (Δ 0,007). |
| `beetle_gate_mistral7b` | GATE, kausal | 5e-5 | 114514 | 0,770 | 0,576 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_mistral7b`: UA −0,033 · UQ −0,031 → im Rauschen. Drei-Saat-Mittel: UA 0,781 (Δ 0,020) · UQ 0,567 (Δ 0,044). |
| `beetle_gate_mistral7b_seed1` | GATE, kausal | 5e-5 | 1 | 0,790 | 0,541 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_mistral7b_seed1`: UA +0,012 · UQ −0,060 → über der Rauschgrenze auf UQ (Einzelvergleich). Drei-Saat-Mittel: UA 0,781 (Δ 0,020) · UQ 0,567 (Δ 0,044). |
| `beetle_gate_mistral7b_seed2` | GATE, kausal | 5e-5 | 2 | 0,782 | 0,585 | Fusionsmechanismus GATE (gelernte Gate-Gewichtung von z und r) statt CONDIFF, kausal. Gegenüber `beetle_condiff_mistral7b_seed2`: UA +0,025 · UQ −0,022 → im Rauschen. Drei-Saat-Mittel: UA 0,781 (Δ 0,020) · UQ 0,567 (Δ 0,044). |

### RoBERTa-base

| Lauf | Konfiguration | lr | Saat | UA | UQ | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- |
| `beetle_condiff_roberta` ¹ | CONDIFF, kausal | ? ¹ | 114514 | 0,694 | 0,289 | Encoder-Baseline (Hauptlauf, erste Trainingswelle, Lernrate nicht dokumentiert). Gegenüber Llama-3.2-1B `beetle_condiff_llama1b`: UA −0,002 · UQ −0,243 → über der Rauschgrenze auf UQ (Einzelvergleich). |
| `beetle_condiff_roberta_batch16_lr1e5` | CONDIFF, kausal | 1e-5 | 114514 | 0,665 | 0,362 | Lernratenvariation des Encoders: Lernrate 1·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA −0,030 · UQ +0,073. Gegenüber Llama-3.2-1B: UA −0,032 · UQ −0,170. |
| `beetle_condiff_roberta_batch16_lr2e5` | CONDIFF, kausal | 2e-5 | 114514 | 0,768 | 0,337 | Lernratenvariation des Encoders: Lernrate 2·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA +0,074 · UQ +0,049. Gegenüber Llama-3.2-1B: UA +0,072 · UQ −0,195. |
| `beetle_condiff_roberta_batch16_lr5e5` | CONDIFF, kausal | 5e-5 | 114514 | 0,688 | 0,288 | Lernratenvariation des Encoders: Lernrate 5·10⁻⁵, Batchgröße 16. Gegenüber dem Encoder-Hauptlauf: UA −0,007 · UQ −0,001. Gegenüber Llama-3.2-1B: UA −0,008 · UQ −0,244. |

### BERT-base-uncased

| Lauf | Konfiguration | lr | Saat | UA | UQ | Was der Lauf zeigt |
| --- | --- | --- | --- | --- | --- | --- |
| `beetle_condiff_bert` ¹ | CONDIFF, kausal | ? ¹ | 114514 | 0,478 | --- | Encoder-Baseline (Hauptlauf, erste Trainingswelle, Lernrate nicht dokumentiert). Gegenüber Llama-3.2-1B `beetle_condiff_llama1b`: UA −0,219 → über der Rauschgrenze auf UA (Einzelvergleich). |

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

## Bekannte Einschränkungen

- **`beetle_bidir02_mistral7b`** erreicht nur 0,587 (UA) und 0,210 (UQ). Der Lauf ist funktionsfähig, aber instabil trainiert und von der Deutung ausgenommen. Die Wiederholung `_v2` erreicht 0,778 / 0,593; ob Umgebung oder Zufall den Einbruch verursacht hat, ist nicht trennbar, ein Maskeneffekt ist es nicht.
- **Mistral-7B ist bei der Maske unvollständig:** Es fehlen `bidir04`, `bidir08` und `bidirfull`. BERT liegt nur für UA vor.
- **Gemischte Softwareumgebungen:** In mehreren Drei-Saat-Gruppen stammt der Lauf mit Saat 114514 aus einer anderen Umgebung als die Saaten 1 und 2. Für `beetle_condiff_llama1b` wird deshalb `_seed114514_verified` verwendet. Details in Kapitel 5.2 der Arbeit.
