---
typ: methode
themen: [regularisierung, regression, sparsity]
aliases: [Lasso, L1-Regularisierung, Basis Pursuit, L0-Regularisierung, Sparsity, Elastic Net, Konvexifizierung]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-vl05-xai-transparente-globale-modelle]]", "[[q-nb02-mnist-regularization]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Lasso / L1-Regularisierung (und L0, Elastic Net)

#flashcards/statistik

## Kurzdefinition
Die **L1-Regularisierung** bestraft die Summe der Beträge der Parameter: $\mathcal L(\theta) = \frac1N\sum\|f_\theta(x_i)-y_i\|^2 + \lambda\|\theta\|_1$. Sie ist die **Konvexifizierung** der L0-Regularisierung und erzeugt **sparse** Lösungen, setzt also manche Gewichte exakt auf 0. Andere Namen: **Lasso, Basis Pursuit, L1-Regularisierung** (→ [[q-vl00-essentiell|VL0]], F. 47).

## Intuition
Eigentlich will man **Sparsity**: möglichst wenige Parameter ungleich 0 (wenige relevante Merkmale). Das misst die „L0-Norm", die Anzahl der Nicht-Null-Einträge. Sie ist aber nicht effizient optimierbar. Die L1-Norm ist die konvexe Ersatzgröße, die – theoretisch begründet – die Sparsity-Eigenschaften oft erhält. Geometrisch hat der L1-Ball **Ecken auf den Achsen**; dort, wo Höhenlinien des Datenfehlers den Ball zuerst berühren, sind häufig Koordinaten null.

## Formal

### L0 – das eigentliche Ziel (→ [[q-vl03-regularisierung-cv|VL3]], F. 17–19)
**Hart:** $\theta\in B_R^0 = \{\theta : \|\theta\|_0 \le R\} = \{\theta : \#\{\theta_i\ne 0\}\le s\}$ – höchstens $s$ Parameter ungleich 0. Optimierung nicht effizient möglich.

**Weich:** $\frac1N\sum\|f_\theta(x_i)-y_i\|^2 + \lambda\|\theta\|_0$
- $\|\cdot\|_0$ ist **keine echte Norm**
- $\|\cdot\|_0$ ist **nicht differenzierbar**
- exakte Optimierung im Allgemeinen nur durch Ausprobieren möglich (→ VL0, F. 46)

Geometrisch (Sparsity-Level 1): zulässig sind nur die **Koordinatenachsen** (→ VL3, F. 19).

### L1 – Konvexifizierung (→ VL0, F. 47; VL3, F. 20–22)
$$\mathcal L(\theta) = \frac1N\sum_{i=1}^N\|f_\theta(x_i)-y_i\|^2 + \lambda\,\|\theta\|_1, \qquad \|\theta\|_1 = \sum_j\lvert\theta_j\rvert$$
Hart: $\theta\in B_R^1 = \{\theta : \|\theta\|_1\le R\}$ – in 2D eine **Raute** (Diamant) mit Ecken auf den Achsen (→ VL3, F. 22).

Anderer Ansatz für Sparsity: **greedy Algorithmen** (→ VL0, F. 47).

### Elastic Net (→ VL3, F. 23)
$$\mathcal L(\theta) = \frac1N\sum\|f_\theta(x_i)-y_i\|^2 + \lambda_1\|\theta\|_1 + \lambda_2\|\theta\|_2^2$$
Nützlich, wenn **Gruppen korrelierter Variablen** stabilisiert werden sollen (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 13).

### Optimierung: proximaler Gradient mit Soft-Thresholding
$\|\theta\|_1$ ist in 0 nicht differenzierbar → **proximale Gradientenverfahren**; für L1 ist der proximale Operator das **Soft-Thresholding** (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 74, 76).

> [!info] Ergänzung (nicht aus der VL) – Formel Soft-Thresholding
> $$\mathrm{prox}_{\eta\lambda\|\cdot\|_1}(v)_j = \operatorname{sign}(v_j)\,\max\big(\lvert v_j\rvert - \eta\lambda,\ 0\big)$$
> Beispiel mit $\eta\lambda = 1$: $v = (3;\ 0{,}5;\ -2) \mapsto (2;\ 0;\ -1)$. Kleine Einträge werden **exakt** 0, große um $\eta\lambda$ Richtung 0 verschoben.

## Annahmen & Voraussetzungen
- Merkmale standardisieren (sonst hängt die Auswahl von Einheiten ab).
- Bei stark korrelierten Merkmalen wählt Lasso oft willkürlich eines aus der Gruppe → Elastic Net.

## Beispiel
[[q-nb02-mnist-regularization|NB2]] (MNIST, $\lambda_1 = 10^{-6}$): Anteil der fc1-Gewichte mit $\lvert w\rvert < 10^{-3}$ steigt von 2,3 % (Baseline) auf **15,6 %**; L2 bleibt bei 2,2 %. Die L1-Norm sinkt von 1341 auf 914.

## Vor- und Nachteile
| + | − |
|---|---|
| Sparsity → implizite Feature-Selektion, interpretierbar | nicht differenzierbar in 0 → Subgradienten/proximale Verfahren |
| konvex (anders als L0) | instabil bei korrelierten Merkmalen |
| Laplace-Prior-Deutung ([[probabilistische-sicht-map]]) | Koeffizienten werden verzerrt (geschrumpft) |

## Typische Prüfungsfragen & Fallstricke
- „Warum L1 statt L0?" → L0 keine Norm, nicht differenzierbar, nur durch Ausprobieren (kombinatorisch) lösbar; L1 ist konvex.
- „Warum erzeugt L1 Nullen und L2 nicht?" → Ecken des L1-Balls auf den Achsen bzw. Soft-Thresholding; L2-Kugel ist rund ([[l0-l1-l2-vergleich]]).
- Drei Namen: Lasso, Basis Pursuit, L1-Regularisierung.
- Fallstrick: „L0-Norm" ist keine Norm (Homogenität verletzt: $\|2\theta\|_0 = \|\theta\|_0$).
- Penalty-Stärke per **Cross-Validation** wählen, nicht nach dem besten Ergebnis (→ VL5, F. 13).

## Verwandt
- [[ridge-regression]] – L2 als Gegenstück
- [[l0-l1-l2-vergleich]] – Geometrie im Vergleich
- [[restringierte-optimierung]] – proximaler Gradient, Soft-Thresholding
- [[transparente-modelle]] – sparse lineare Modelle als Erklärung
- [[probabilistische-sicht-map]] – L1 = Laplace-Prior

## Karteikarten
Wie lautet die L1-regularisierte Zielfunktion?::$\frac1N\sum\|f_\theta(x_i)-y_i\|^2 + \lambda\|\theta\|_1$.
Drei Namen der L1-Regularisierung?::Lasso, Basis Pursuit, L1-Regularisierung.
Was misst $\|\theta\|_0$?::Die Anzahl der Einträge ungleich 0.
Drei Probleme der L0-Regularisierung?::Keine echte Norm, nicht differenzierbar, exakte Optimierung nur durch Ausprobieren.
Was bedeutet „Konvexifizierung" bei L1?::L0 wird durch die konvexe L1-Norm ersetzt, in der Hoffnung, dass Sparsity erhalten bleibt.
Wie sieht der L1-Ball in 2D aus?::Eine Raute (Diamant) mit Ecken auf den Koordinatenachsen.
Was ist Elastic Net?::Kombination aus L1- und L2-Strafterm; stabilisiert Gruppen korrelierter Variablen.
Welcher proximale Operator gehört zu L1?::Soft-Thresholding: $\mathrm{sign}(v)\max(\lvert v\rvert-\eta\lambda, 0)$.
Welche Alternative zu L1 nennt die VL für Sparsity?::Greedy Algorithmen.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 46–47
- [[q-vl03-regularisierung-cv|VL3]], Folien 17–23
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 74–76
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folie 13
- [[q-nb02-mnist-regularization|NB2]]
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 3.4.2, S. 68
