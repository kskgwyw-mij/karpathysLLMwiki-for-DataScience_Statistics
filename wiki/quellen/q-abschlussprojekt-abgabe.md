---
typ: quelle
themen: [regularisierung, optimierung, lab, projekt]
aliases: [Projektabgabe, Fashion-MNIST-Projekt, eigene Kaggle-Abgabe]
datei: "raw/uebungen/abschlussprojekt/"
original: "Lab-Kurs (Moodle-Export 14.10.2026): 3_Abschlussprojekt/2_Projektabgabe/Einreichungen/<eigene Abgabe>/ (PDF + ipynb)"
relevanz: mittel
aktualisiert: 2026-10-16
---

# Eigene Abgabe zum Abschlussprojekt · Fashion-MNIST: Regularisierung × Optimierer

Eigene Abgabe (Juli 2026) zur [[q-lab-abschlussprojekt-aufgabe|Kaggle-Aufgabe]]: Dokumentation (`abgabe-fashion-mnist.pdf`, 4 Seiten) und Notebook (`abgabe-fashion-mnist.ipynb`, 32 Zellen). Kein Kursmaterial; auf Themenseiten stehen die Ergebnisse deshalb in grünen Anwendungsfall-Callouts.

## Zusammenfassung
Fashion-MNIST (70 000 Graustufenbilder 28×28, 10 balancierte Kleidungsklassen; geladen über `torchvision`, identisch mit dem Kaggle-Datensatz) als 10-Klassen-Bildklassifikation. Hauptmodell ist ein kleines CNN (2 Conv-Blöcke, FC-Head, ≈ 422 000 Parameter), Vergleichsmodell ein MLP (≈ 536 000 Parameter). Verglichen wird eine volle **5×4-Matrix** aus Regularisierung und Optimierer. Die Regularisierungs-Setups sind keine, Dropout, Weight Decay, Data Augmentation und Label Smoothing; die Optimierer SGD mit Momentum, Adam, AdamW und RMSprop. **Early Stopping** läuft in jeder Zelle mit (Patience 3 auf der Val-Loss, beste Epoche wird geladen). Ausgewählt wird nach Val-Macro-F1; die beste Variante wird auf allen 60 000 Trainingsbildern neu trainiert und dann **einmal** auf dem Testset ausgewertet.

## Gliederung (Dokumentation)
| Seite | Inhalt | Wiki-Seiten |
|---|---|---|
| 1 | Datensatz, Metriken (Accuracy, Macro-F1), Vorverarbeitung, Splits, Architektur, Trainingsloop | [[train-val-test-split]], [[trainingsschleife]] |
| 2 | Experimentmatrix, Fairness-Details AdamW, Heatmap Val-Accuracy | [[adaptive-optimierer]], [[regularisierung]] |
| 3 | Lernkurven, CNN vs. MLP, beste Variante, Test-Evaluation | [[label-smoothing]], [[early-stopping]] |
| 4 | Fehleranalyse (Confusion Matrix), Fazit und Verbesserungen | – |

## Setup
| Regularisierung | Umsetzung | Optimierer | Hyperparameter |
|---|---|---|---|
| none | Referenz | SGD mit Momentum | $\eta = 0{,}05$, $\beta = 0{,}9$ |
| dropout | $p = 0{,}3$ im FC-Head | Adam | $\eta = 10^{-3}$ |
| weight_decay | L2, $\lambda = 10^{-4}$ | AdamW | $\eta = 10^{-3}$, entkoppeltes Weight Decay |
| augmentation | horizontales Spiegeln + RandomCrop (Padding 2), nur Training | RMSprop | $\eta = 10^{-3}$, $\alpha = 0{,}99$ |
| label_smoothing | $\varepsilon = 0{,}1$ | | |

Gemeinsam: Batch 128, höchstens 12 Epochen, Seed 42, 6 000 Validierungsbilder aus dem Training abgezweigt, Standardisierung mit Mittelwert 0,286 und Standardabweichung 0,353. AdamW wurde außerhalb des Setups `weight_decay` explizit auf Weight Decay 0 gesetzt, weil der PyTorch-Default 0,01 sonst eine versteckte Regularisierung wäre. Auf BatchNorm wurde verzichtet, weil es selbst regularisiert.

## Ergebnisse: Val-Accuracy der 5×4-Matrix (S. 2)
| | SGD + Momentum | Adam | AdamW | RMSprop | Zeilenmittel |
|---|---|---|---|---|---|
| none | 0,9137 | 0,9247 | 0,9247 | 0,9223 | 0,9213 |
| dropout | 0,9243 | 0,9290 | 0,9290 | **0,9312** | 0,9284 |
| weight_decay | 0,9210 | 0,9207 | 0,9247 | 0,9252 | 0,9229 |
| augmentation | 0,9207 | 0,9223 | 0,9223 | 0,9135 | 0,9197 |
| label_smoothing | 0,9273 | **0,9315** | **0,9315** | 0,9298 | **0,9300** |
| **Spaltenmittel** | 0,9214 | 0,9256 | **0,9264** | 0,9244 | |

**Beste Variante:** CNN mit Label Smoothing und AdamW (Val-F1 0,9322); nach Neutraining auf 60 000 Bildern mit 11 Epochen:

| Test-Accuracy | Test-Macro-F1 | Test-Loss (CE) |
|---|---|---|
| 92,90 % | 0,9287 | 0,2872 |

## Kernaussagen der Abgabe
- **Regularisierung:** Label Smoothing (+0,9 Prozentpunkte gegenüber none) und Dropout (+0,7) helfen am meisten, Weight Decay etwas. Data Augmentation liegt im Mittel **unter** der Referenz: Bei höchstens 12 Epochen konvergieren die augmentierten Läufe langsamer und sind am Ende noch nicht auskonvergiert. Regularisierungsstärke und Trainingsbudget hängen also zusammen.
- **Optimierer:** Die adaptiven Verfahren liegen ohne Lernraten-Tuning vorn (AdamW > Adam > RMSprop > SGD mit Momentum).
- **Adam vs. AdamW:** Ohne Weight Decay liefern beide exakt identische Ergebnisse (Sanity-Check). Mit Weight Decay schlägt AdamW (0,9247) Adam (0,9207).
- **Early Stopping:** Unregularisierte Läufe stoppen am frühesten (none × SGD mit Momentum: beste Epoche 5), regularisierte nutzen fast das volle Budget. Die Val-Loss von none steigt ab etwa Epoche 9 wieder.
- **Label Smoothing** liegt konstruktionsbedingt auf höherem Loss-Niveau, weil Smoothing das Minimum der Cross-Entropy anhebt.
- **CNN vs. MLP:** Das MLP erreicht 89,07 % bzw. 90,45 % gegenüber 92,47 % bzw. 93,15 % beim CNN (AdamW, ohne Regularisierung bzw. mit Label Smoothing). Der räumliche Induktionsbias ist rund 3 Prozentpunkte wert; Regularisierung kann fehlende Architektureignung nicht ausgleichen.
- **Fehleranalyse:** Von 710 Testfehlern entfallen die meisten auf Oberteile. Shirt hat mit 76,1 % den schwächsten Recall; Shirt ↔ T-Shirt/Top macht 166 Fehler aus. Ein Teil des Restfehlers ist irreduzibel, weil die Bilder auch für Menschen mehrdeutig sind.
- **Einschränkung:** Die Abstände zwischen den besten Zellen (< 0,2 Prozentpunkte) liegen vermutlich im Bereich des Seed-Rauschens; es gibt nur einen Seed pro Zelle.

> [!ergaenzung] Ergänzung (nicht aus der VL) – Kontrolle der Matrix
> - Zeilen- und Spaltenmittel aus der Heatmap nachgerechnet, sie stimmen mit der Dokumentation überein.
> - Mit AdamW bringt Weight Decay gegenüber none **nichts** (beide 0,9247); mit Adam **schadet** es (0,9207 statt 0,9247). „AdamW schlägt Adam bei Weight Decay" stimmt also, aber vor allem, weil die gekoppelte L2-Variante in Adam schlechter abschneidet, nicht weil AdamW durch Weight Decay gewinnt.
> - Lab 2 zeigt beim Optimierer-Ranking das Gegenteil (SGD mit Momentum vorn, [[q-lab02-pytorch-optimizer-comparison|Lab 2]]). Beide Ergebnisse stammen aus Einzelläufen mit je einer festen Lernrate pro Optimierer und verallgemeinern nicht.

## Bezug zur Vorlesung
- Vorgehen nach dem Auswahlprozess aus [[train-val-test-split]]: Modellwahl nur auf Val, Neutraining auf allen Trainingsdaten, Test einmal. Der Abstand Val–Test von etwa 0,3 Prozentpunkten spricht gegen eine Überanpassung an das Val-Set.
- Der Fallstrick „Losses mit und ohne Label Smoothing nicht vergleichen" aus [[label-smoothing]] zeigt sich in den Lernkurven.

## Offene Fragen
- Keine.
