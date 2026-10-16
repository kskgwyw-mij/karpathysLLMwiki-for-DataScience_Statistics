---
typ: vergleich
themen: [regularisierung, neuronale-netze, notebook, lab]
aliases: [Lab 1 vs. NB2, Regularisierung MNIST Vergleich]
quellen: ["[[q-lab01-pytorch-introduction]]", "[[q-nb02-mnist-regularization]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-16
---

# Regularisierung auf MNIST: Lab 1 vs. NB2

#flashcards/statistik

Zwei Kurs-Notebooks trainieren **dasselbe** kleine MLP (784 → 64 → 10, ReLU, Adam mit $\eta = 10^{-3}$, Batch 128) auf **denselben** Datenmengen (5 000 Training, 1 000 Validierung, 10 000 Test) mit denselben Regularisierungsmethoden. Unterschiedlich sind nur die Stärken und das Trainingsbudget, und damit teils auch das Ergebnis.

## Setup
| | [[q-nb02-mnist-regularization\|NB2]] | [[q-lab01-pytorch-introduction\|Lab 1]] |
|---|---|---|
| Epochen (max.) | 8 | 20 |
| Early Stopping | Patience 3 | Patience 10 |
| L2 (`weight_decay`) | $10^{-4}$ | $10^{-2}$ |
| L1 (Zusatzterm) | $10^{-6}$ | $10^{-4}$ |
| Dropout | 0,5 | 0,5 |
| Label Smoothing | 0,1 | 0,1 |
| weitere Varianten | Max-Norm, Nichtnegativität, Clipping | eigenes `nn.Module` |

## Test-Accuracy (Differenz zur jeweiligen Baseline in Prozentpunkten)
| Konfiguration | NB2 | Δ | Lab 1 | Δ |
|---|---|---|---|---|
| baseline | 0,9237 | – | 0,9282 | – |
| L2 | 0,9197 | −0,40 | 0,9305 | +0,23 |
| L1 | 0,9190 | −0,47 | 0,9347 | +0,65 |
| Dropout | 0,9208 | −0,29 | 0,9297 | +0,15 |
| Label Smoothing | **0,9336** | **+0,99** | **0,9439** | **+1,57** |

## Anteil fast-null-Gewichte $\lvert w\rvert < 10^{-3}$ in der ersten Schicht
| Konfiguration | NB2 | Lab 1 |
|---|---|---|
| baseline | 2,3 % | 2,0 % |
| L2 | 2,2 % | 8,6 % |
| **L1** | **15,6 %** | **37,0 %** |
| Dropout | 2,4 % | 2,3 % |
| Label Smoothing | 2,5 % | 2,3 % |

## Was sich wiederholt und was nicht
- **In beiden Notebooks:** Label Smoothing hat die beste Test-Accuracy, obwohl seine Val-Loss am höchsten ist (NB2 0,770, Lab 1 0,70). Die Loss ist wegen der anderen Zielverteilung nicht vergleichbar → [[label-smoothing]].
- **In beiden Notebooks:** L1 erzeugt deutlich mehr Nullgewichte als L2 und die Baseline → [[l0-l1-l2-vergleich]], [[lasso-regression]].
- **Nur in Lab 1:** L1, L2 und Dropout verbessern die Baseline. In NB2 liegen sie leicht darunter.
- **Nur in Lab 1:** L2 verkleinert die Gewichte sichtbar (mittleres $\lvert w\rvert$ von 0,0329 auf 0,0135, −59 %). In NB2 sinkt $\lVert W\rVert_2$ nur von 7,53 auf 7,38.

> [!ergaenzung] Ergänzung (nicht aus der VL) – Deutung
> - **Stärke:** NB2 nutzt $\lambda$-Werte am unteren Rand der VL-Richtwerte (L2 $10^{-6}$–$10^{-2}$, L1 $10^{-6}$–$10^{-3}$, → [[regularisierung]]), Lab 1 Werte am oberen Rand. Bei so kleinen Strafen wie in NB2 ist kaum ein Effekt zu erwarten; die kleinen Verschlechterungen dort liegen im Bereich des Zufalls.
> - **Budget:** In Lab 1 sinkt der Trainings-Loss der Baseline nach 20 Epochen auf 0,06, die Val-Loss liegt bei 0,28. Diese Lücke zwischen Training und Validierung kann Regularisierung schließen ([[bias-varianz-tradeoff]]). NB2 stoppt nach höchstens 8 Epochen; vermutlich ist die Lücke dort kleiner, sodass es weniger zu regularisieren gibt.
> - **Vorsicht:** Beide Notebooks zeigen einen Lauf mit einem Seed. Unterschiede von wenigen Zehntel Prozentpunkten auf 10 000 Testbildern entsprechen wenigen Dutzend Bildern. Die VL empfiehlt, bei kleinem N über 3–5 Seeds zu mitteln ([[train-val-test-split]]).
>
> **Merksatz:** Ob eine Regularisierung „hilft", hängt an $\lambda$ und am Trainingsbudget. Deshalb sucht man $\lambda$ auf einem logarithmischen Gitter mit dem Validierungsset, statt einen Wert zu raten.

## Karteikarten
Warum verbessern L1/L2 in Lab 1 die Baseline, in NB2 aber nicht?::Lab 1 nutzt 100-mal größere $\lambda$ und 20 statt 8 Epochen: Die Strafe ist stark genug, und die Baseline passt sich bei längerem Training stärker an die Trainingsdaten an. Beides sind Einzelläufe mit einem Seed.
Welcher Befund wiederholt sich in NB2 und Lab 1?::Label Smoothing hat die höchste Val-Loss, aber die beste Test-Accuracy; L1 erzeugt viel mehr Nullgewichte als L2.
Wie viele fast-null-Gewichte erzeugt L1 in Lab 1 gegenüber der Baseline?::37 % gegenüber 2 % (erste Schicht, $\lvert w\rvert<10^{-3}$).

## Verwandt
- [[regularisierung]] – Taxonomie und Richtwerte für $\lambda$
- [[label-smoothing]], [[dropout]], [[ridge-regression]], [[lasso-regression]] – die verglichenen Methoden
- [[train-val-test-split]] – wie man $\lambda$ richtig wählt

## Quellen
- [[q-nb02-mnist-regularization|NB2]], Zellen 5 und 8
- [[q-lab01-pytorch-introduction|Lab 1]], Abschnitte „Comparing the results", „Comparing weight statistics"
