---
typ: quelle
themen: [regularisierung, neuronale-netze, notebook, lab]
aliases: [Lab 1, Lab01, PyTorch-Einführung]
datei: "raw/uebungen/lab01-pytorch-introduction.ipynb"
original: "Lab-Kurs (Moodle-Export 14.10.2026): 2_Notebooks/1_pytorch_introduction.json"
relevanz: mittel
aktualisiert: 2026-10-16
---

# Lab 1 · PyTorch-Einführung mit MNIST (Autograd, Trainingsloop, Regularisierung)

Kurs-Notebook aus dem Lab zur Vorlesung, Original mit Endung `.json`, inhaltlich ein Jupyter-Notebook (59 Zellen). Belege nennen den Abschnittstitel im Notebook.

## Zusammenfassung
Einführung in PyTorch ohne Vorkenntnisse. Erst werden Tensoren, automatische Differenzierung (Autograd) und ein Lernproblem mit einem einzigen Gewicht gezeigt, dann ein kleines MLP auf MNIST trainiert (784 → 64 → 10, ReLU, Adam mit $\eta = 10^{-3}$, Batch 128). Trainiert wird auf 5 000 Bildern, validiert auf 1 000, getestet auf 10 000, mit Seed 33. Verglichen werden Baseline, L2 (`weight_decay`), L1 (manueller Zusatzterm), Dropout und Label Smoothing, jeweils 20 Epochen mit Early Stopping (Patience 10, beste Val-Loss-Epoche wird geladen). Dazu kommt ein selbst definiertes `nn.Module` mit Gate und Skip-Verbindung. Zum Schluss werden Gewichte als Bilder, Histogramme und Kennzahlen inspiziert.

Inhaltlich ist das Notebook eine Variante von [[q-nb02-mnist-regularization|NB2]]: gleiches Netz, gleiche Datenmengen, aber andere $\lambda$-Werte, mehr Epochen und eine andere Patience. Gegenüberstellung: [[regularisierung-mnist-lab01-vs-nb02]].

## Gliederung
| Abschnitte | Inhalt | Wiki-Seiten |
|---|---|---|
| Tensors, Automatic differentiation | Tensor als Datenstruktur, Rechengraph, Kettenregel, `.backward()`, `.grad` | [[trainingsschleife]] |
| A tiny automatic differentiation example | $y = x^2 + 2x + 1$, $dy/dx$ bei $x = 3$ ist 8 | [[trainingsschleife]] |
| Automatic differentiation for a tiny learning problem | ein Gewicht lernt $w\cdot 3 = 6$, Lernrate 0,05, zehn Schritte | [[trainingsschleife]], [[gradientenabstieg]] |
| Why gradients are reset to zero | Gradienten werden **akkumuliert** → `zero_grad()` | [[trainingsschleife]] |
| Loading MNIST, DataLoader | Normalisierung, Subset, Minibatches mit Shuffling | [[sgd]] |
| A simple neural network, forward pass, loss and optimizer | Logits, `CrossEntropyLoss`, Adam | [[trainingsschleife]], [[adaptive-optimierer]] |
| The training loop, evaluation function | `train()`/`eval()`, `no_grad`, Early Stopping mit Wiederherstellung | [[trainingsschleife]], [[early-stopping]] |
| Regularization: L2, L1, Dropout, Label Smoothing | Umsetzung in PyTorch, je ein Lauf | [[ridge-regression]], [[lasso-regression]], [[dropout]], [[label-smoothing]] |
| A more complex custom model with `nn.Module` | zwei Zweige (ReLU, tanh), gelerntes Gate, Skip-Verbindung, Bottleneck | [[trainingsschleife]] |
| Comparing the results | Val-Accuracy-Kurven, Test-Accuracy-Tabelle | [[regularisierung-mnist-lab01-vs-nb02]] |
| Inspecting model weights, weight distributions, weight statistics | Gewichte als 28×28-Bilder, Histogramme, Anteil fast-null | [[lasso-regression]], [[l0-l1-l2-vergleich]] |
| Key takeaways, Exercises | Trainingsmuster in fünf Zeilen, Übungsvorschläge | [[trainingsschleife]] |

## Ergebnisse (Abschnitt „Comparing the results")
Jeder Lauf hat bis zu 20 Epochen; ausgewertet wird das Modell der Epoche mit der besten Val-Loss.

| Konfiguration | Stärke | Test-Accuracy |
|---|---|---|
| baseline | – | 0,9282 |
| l2 (`weight_decay`) | $10^{-2}$ | 0,9305 |
| l1 (Zusatzterm) | $10^{-4}$ | 0,9347 |
| dropout | $p = 0{,}5$ | 0,9297 |
| label smoothing | $\varepsilon = 0{,}1$ | **0,9439** |
| complex model (`nn.Module`) | Dropout 0,2 im Modell | 0,9399 |

Die Val-Loss in Epoche 20 liegt mit Label Smoothing bei 0,70, bei allen anderen Läufen zwischen 0,25 und 0,29. Das ist derselbe Befund wie in NB2: Die Loss ist nicht vergleichbar, weil die Zielverteilung eine andere ist (→ [[label-smoothing]]). Early Stopping hat nur beim komplexen Modell ausgelöst (beste Val-Loss in Epoche 10, Abbruch nach Epoche 20).

## Gewichtsstatistik erste Schicht (Abschnitt „Comparing weight statistics")
| Konfiguration | mittleres $\lvert w\rvert$ | max $\lvert w\rvert$ | Anteil $\lvert w\rvert < 10^{-3}$ |
|---|---|---|---|
| baseline | 0,0329 | 0,328 | 2,0 % |
| l2 | **0,0135** | **0,127** | 8,6 % |
| l1 | 0,0176 | 0,252 | **37,0 %** |
| dropout | 0,0291 | 0,252 | 2,3 % |
| label smoothing | 0,0291 | 0,271 | 2,3 % |

## Prüfungsrelevante Aussagen
- **Trainingsmuster:** Vorhersage → Loss → `zero_grad()` → `loss.backward()` → `optimizer.step()`. `backward()` berechnet die Gradienten per Kettenregel, `step()` aktualisiert die Parameter (Abschnitt „Key takeaways") → [[trainingsschleife]].
- **Gradienten akkumulieren** standardmäßig; deshalb vor jedem Backward-Schritt zurücksetzen (Abschnitt „Why gradients are reset to zero").
- **Dropout** ist nur im Training aktiv, in der Auswertung nicht; umgeschaltet wird mit `model.train()` bzw. `model.eval()` (Abschnitt „Dropout") → [[dropout]].
- **L2** wird über den Optimizer-Parameter `weight_decay` eingebaut, **L1** muss man von Hand vor `backward()` zum Loss addieren (Abschnitte „L2 regularization", „L1 regularization").
- **L1 erzeugt Sparsity:** 37 % fast-null-Gewichte gegenüber 2 % in der Baseline. **L2 macht Gewichte klein, aber nicht null:** das mittlere $\lvert w\rvert$ sinkt um 59 %, der Anteil fast-null steigt nur auf 8,6 % → [[l0-l1-l2-vergleich]].
- **Label Smoothing** macht das Modell weniger überkonfident; es wird direkt in `CrossEntropyLoss(label_smoothing=0.1)` gesetzt.
- Die Logits sind noch keine Wahrscheinlichkeiten; `CrossEntropyLoss` verarbeitet sie direkt (Abschnitt „A simple neural network").

## Widersprüche / Abweichungen zu anderen Quellen
- Gegenüber [[q-nb02-mnist-regularization|NB2]] kehrt sich das Ergebnis für L1, L2 und Dropout um: Hier verbessern sie die Baseline leicht, in NB2 verschlechtern sie sie leicht. Das ist kein Widerspruch, sondern eine Folge von 100-mal größerem $\lambda$, 20 statt 8 Epochen und Patience 10 statt 3. Beide Notebooks zeigen jeweils nur einen Lauf mit einem Seed. Details: [[regularisierung-mnist-lab01-vs-nb02]].
- Wie in NB2 wird L2 über `torch.optim.Adam(weight_decay=…)` umgesetzt, also die an die adaptive Skalierung gekoppelte Variante, nicht AdamW (→ [[adaptive-optimierer]]).

## Offene Fragen
- Keine.
