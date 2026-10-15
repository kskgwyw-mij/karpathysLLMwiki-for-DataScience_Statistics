---
typ: vergleich
themen: [regularisierung, sparsity]
aliases: [L1 vs L2, Ridge vs Lasso, Normen-Vergleich]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-vl05-xai-transparente-globale-modelle]]", "[[q-nb02-mnist-regularization]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Vergleich: L0 vs. L1 vs. L2

#flashcards/statistik

## Gegenüberstellung
| | **L0** | **L1** (Lasso) | **L2** (Ridge) |
|---|---|---|---|
| Strafterm | $\lambda\lVert\theta\rVert_0 = \lambda\cdot\#\{\theta_j\ne 0\}$ | $\lambda\sum_j\lvert\theta_j\rvert$ | $\lambda\sum_j\theta_j^2$ |
| Echte Norm? | nein | ja | ja (quadriert) |
| Konvex? | nein | ja | ja (streng) |
| Differenzierbar? | nein | nicht in 0 | ja |
| Zulässige Menge 2D (hart) | Koordinatenachsen (Sparsity-Level 1) | Raute mit Ecken auf den Achsen | Kreis |
| Wirkung | exakte Sparsity | Sparsity + Schrumpfen | gleichmäßiges Schrumpfen, Stetigkeit/Stabilität |
| Optimierung | nur durch Ausprobieren / greedy | Subgradient, proximal (Soft-Thresholding) | Gradient, geschlossene Lösung im linearen Fall |
| Prior (MAP) | – | Laplace $e^{-\lVert\theta\rVert_1}$ | Gauß $e^{-\lVert\theta\rVert^2}$ |
| Namen | Sparsity-Regularisierung | Lasso, Basis Pursuit | Tikhonov, Ridge, Weight Decay |
| Richtwert $\lambda$ | – | $10^{-6}\ldots10^{-3}$ | $10^{-6}\ldots10^{-2}$ |

Quellen: [[q-vl00-essentiell|VL0]] F. 45–47; [[q-vl03-regularisierung-cv|VL3]] F. 12–23, 37, 41; [[q-vl05-xai-transparente-globale-modelle|VL5]] F. 13.

## Warum erzeugt L1 Nullen, L2 aber nicht?
**Geometrisch** (VL3, F. 13, 22): Man vergrößert die Höhenlinien (Ellipsen) des Datenfehlers um das unregularisierte Optimum, bis sie die zulässige Menge berühren. Die L1-Raute hat **Ecken auf den Achsen** – die Berührung findet häufig an einer Ecke statt, dort ist eine Koordinate 0. Die L2-Kugel ist rund; ein Berührpunkt genau auf einer Achse ist Zufall.

> [!info] Ergänzung (nicht aus der VL) – analytisch in 1D
> Für $\min_\theta \tfrac12(\theta - v)^2 + \lambda\,\mathcal R(\theta)$:
> - L1: $\hat\theta = \mathrm{sign}(v)\max(\lvert v\rvert - \lambda, 0)$ → für $\lvert v\rvert \le \lambda$ **exakt 0**.
> - L2 ($\mathcal R = \theta^2$): $\hat\theta = v/(1 + 2\lambda)$ → nie exakt 0 (außer $v = 0$).
>
> Der L1-Gradient hat in der Nähe von 0 konstant Betrag $\lambda$ und zieht kleine Werte bis auf 0; der L2-Gradient $2\lambda\theta$ wird mit $\theta$ immer kleiner.

## Empirisch ([[q-nb02-mnist-regularization|NB2]])
| | Anteil fc1-Gewichte $\lvert w\rvert < 10^{-3}$ | Test-Accuracy |
|---|---|---|
| Baseline | 2,3 % | 0,924 |
| L2 ($10^{-4}$) | 2,2 % | 0,920 |
| L1 ($10^{-6}$) | 15,6 % | 0,919 |

## Wann was?
- Viele irrelevante Merkmale, Interpretierbarkeit gewünscht → **L1**
- Viele korrelierte, alle etwas relevante Merkmale, Stabilität → **L2**
- Korrelierte Gruppen **und** Sparsity → **Elastic Net** (L1 + L2)

## Karteikarten
Welche der drei „Normen" L0, L1, L2 ist nicht konvex?::L0.
Wie sehen die zulässigen Mengen von L0 (Level 1), L1 und L2 in 2D aus?::L0: Koordinatenachsen; L1: Raute; L2: Kreis.
Warum führt L1 geometrisch zu Sparsity?::Der L1-Ball hat Ecken auf den Achsen; die Höhenlinien des Datenfehlers berühren ihn oft dort, wo Koordinaten 0 sind.
Wann Elastic Net statt Lasso?::Wenn Gruppen korrelierter Merkmale stabilisiert und trotzdem Sparsity erreicht werden soll.
Lösung von $\min \frac12(\theta-v)^2 + \lambda\lvert\theta\rvert$?::$\mathrm{sign}(v)\max(\lvert v\rvert-\lambda, 0)$ (Soft-Thresholding).

## Verwandt
- [[ridge-regression]]
- [[lasso-regression]]
- [[probabilistische-sicht-map]]
- [[restringierte-optimierung]]
