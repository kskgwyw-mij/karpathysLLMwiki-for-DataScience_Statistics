---
typ: quelle
themen: [regularisierung, notebook]
aliases: [NB2]
datei: "raw/uebungen/nb02-mnist-regularization.ipynb"
original: "4_Jupyter Notebooks/2_mnist_regularization_pytorch.xxx"
relevanz: mittel
aktualisiert: 2026-10-15
---

# NB2 · Regularisierungsmethoden im Vergleich (MNIST, PyTorch)

Original mit Endung `.xxx`, inhaltlich ein Jupyter-Notebook.

## Zusammenfassung
Ein MLP (784 → 64 → 10, ReLU) wird auf 5 000 MNIST-Trainingsbildern (1 000 Validierung, 10 000 Test) mit Adam ($\eta = 10^{-3}$, 8 Epochen, Early Stopping mit Patience 3) trainiert. Verglichen werden acht Konfigurationen: Baseline, L2 über `weight_decay`, L1 als Zusatzterm, Max-Norm pro Neuron, Nichtnegativität, Gewichts-Clipping (diese drei als Projektion nach jedem Optimizer-Schritt), Dropout 0,5, Label Smoothing 0,1. Anschließend werden Gewichte als 28×28-Bilder, Heatmaps und Histogramme visualisiert.

## Ergebnisse (Zelle 5)
| Konfiguration | beste Val-Loss | Test-Accuracy |
|---|---|---|
| baseline | 0,3049 | 0,9237 |
| l2 ($10^{-4}$) | 0,3225 | 0,9197 |
| l1 ($10^{-6}$) | 0,3236 | 0,9190 |
| maxnorm (3,0) | 0,3093 | 0,9212 |
| nonneg | 0,4162 | 0,8977 |
| clipW (0,5) | 0,3261 | 0,9190 |
| dropout (0,5) | 0,3343 | 0,9208 |
| label_smooth (0,1) | 0,7700 | **0,9336** |

## Gewichtsstatistik fc1 (Zelle 8)
| Konfiguration | $\lVert W\rVert_1$ | $\lVert W\rVert_2$ | Anteil $\lvert w\rvert < 10^{-3}$ |
|---|---|---|---|
| baseline | 1341 | 7,53 | 2,3 % |
| l2 | 1314 | 7,38 | 2,2 % |
| **l1** | **914** | **6,06** | **15,6 %** |
| nonneg | 801 | 6,96 | **47,7 %** |
| dropout | 1265 | 7,11 | 2,4 % |
| label_smooth | 1222 | 6,86 | 2,5 % |

## Prüfungsrelevante Aussagen
- **L1 erzeugt Sparsity:** siebenmal so viele (fast) Nullgewichte wie die Baseline; L2 schrumpft die Norm nur leicht und erzeugt keine Nullen. → [[lasso-regression]], [[l0-l1-l2-vergleich]]
- Nichtnegativität setzt knapp die Hälfte der Gewichte auf 0 (alle, die negativ werden wollten).
- **Harte Regularisierung = Projektion** auf die zulässige Menge nach jedem Schritt (Max-Norm, Nichtnegativität, Clipping) → vgl. projizierter Gradient in [[restringierte-optimierung]].
- Label Smoothing hat die **höchste** Val-Loss, aber die **beste** Test-Accuracy: Die Loss-Werte sind nicht vergleichbar, weil die Loss-Funktion selbst (geglättete Zielverteilung) eine andere ist. → [[label-smoothing]]
- Nichtnegativität schränkt die Modellklasse am stärksten ein → schlechteste Accuracy (zu viel Bias).

## Offene Fragen
- Keine.
