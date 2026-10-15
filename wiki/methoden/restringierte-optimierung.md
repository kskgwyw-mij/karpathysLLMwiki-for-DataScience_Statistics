---
typ: methode
themen: [optimierung, regularisierung]
aliases: [Constrained Optimization, Restringierte Optimierung, Nebenbedingungen, KKT, Lagrange, Projected Gradient, Projizierter Gradient, Proximal Gradient, Proximaler Operator, ISTA, FISTA, ADMM]
quellen: ["[[q-vl04-map-resampling-optimierung]]", "[[q-vl03-regularisierung-cv]]", "[[q-nb02-mnist-regularization]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Restringierte Optimierung: KKT, projizierter und proximaler Gradient

#flashcards/statistik

## Kurzdefinition
Optimierung mit zulässiger Menge: $\min f(\theta)$ unter $g_i(\theta)\le 0$ und $h_j(\theta) = 0$. Wichtigste Algorithmen der VL: **projizierter Gradient** (Schritt, dann zurückprojizieren) und **proximaler Gradient** (Schritt auf dem glatten Teil, dann proximaler Operator auf dem nicht glatten Teil, z. B. Soft-Thresholding für L1) (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 69–76).

## Intuition
Nebenbedingungen kodieren Physik, Budgets, Fairness, Monotonie, Sparsity oder Parametergrenzen. Der unrestringierte Minimierer ist oft unzulässig; das restringierte Optimum liegt häufig **auf dem Rand** der zulässigen Menge (→ VL4, F. 69). Harte Regularisierung ([[regularisierung]]) ist genau so ein Problem.

## Formal

### Problem und KKT (→ VL4, F. 69–70)
$$\min_\theta f(\theta) \quad\text{u. d. N.}\quad g_i(\theta)\le 0,\ h_j(\theta) = 0, \qquad \mathcal L(\theta,\lambda,v) = f(\theta) + \sum_i\lambda_i g_i(\theta) + \sum_j v_j h_j(\theta),\ \lambda_i\ge 0$$
KKT-Intuition: Am Optimum balancieren sich Ziel und **aktive** Nebenbedingungen.
1. **Stationarität:** Gradient der Lagrange-Funktion verschwindet
2. **Primale Zulässigkeit:** $\theta$ erfüllt die ursprünglichen Nebenbedingungen
3. **Duale Zulässigkeit:** $\lambda_i\ge 0$
4. **Komplementärer Schlupf:** inaktive Nebenbedingungen haben $\lambda_i = 0$

### Projizierter Gradient (→ VL4, F. 71–73)
$$y_t = \theta_t - \eta_t\nabla f(\theta_t), \qquad \theta_{t+1} = \Pi_C(y_t)$$
$\Pi_C$ liefert den **nächstgelegenen zulässigen Punkt** (unter einer gewählten Norm). Einfach und robust, wenn die Projektion billig ist: Boxen, Kugeln, Simplexe, Nichtnegativität. Für $C = \mathbb R^d$ tut die Projektion nichts → gewöhnlicher GD.

> [!ergaenzung] Ergänzung (nicht aus der VL) – häufige Projektionen
> Box $[l,u]$: komponentenweise `clip`. Nichtnegativität: $\max(\theta, 0)$. L2-Kugel mit Radius $R$: $\theta\cdot\min(1, R/\|\theta\|)$. Genau das tun Clipping, Non-Negativity und Max-Norm in [[q-nb02-mnist-regularization|NB2]] nach jedem Optimizer-Schritt.

### Proximaler Operator und proximaler Gradient (→ VL4, F. 74–76)
$$\mathrm{prox}_{\eta h}(v) = \arg\min_\theta\ h(\theta) + \frac{1}{2\eta}\|\theta - v\|^2$$
Wähle den Punkt, der eine kleine Strafe $h$ mit Nähe zu $v$ ausbalanciert. Für $\min \mathcal L(\theta) = f(\theta) + h(\theta)$ mit glattem $f$ und nicht glattem $h$:
$$\theta_{t+1} = \mathrm{prox}_{\eta h}\big(\theta_t - \eta\nabla f(\theta_t)\big)$$

| $h$ | proximaler Operator |
|---|---|
| $\lambda\lVert\theta\rVert_1$ | **Soft-Thresholding** (Lasso) |
| Indikatorfunktion einer Menge $C$ | **Projektion** auf $C$ → projizierter Gradient ist Spezialfall |
| $\lambda\lVert\theta\rVert_2^2$ | Schrumpfen Richtung 0 |

Key idea: Ein proximaler Schritt ist ein regularisierter Korrekturschritt – den schwierigen Teil $h$ optimieren und dabei nahe am Gradientenvorschlag bleiben.

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Soft-Thresholding: $[\mathrm{prox}_{\eta\lambda\|\cdot\|_1}(v)]_j = \mathrm{sign}(v_j)\max(\lvert v_j\rvert - \eta\lambda, 0)$. L2-Prox: $v/(1 + 2\eta\lambda)$. Proximaler Gradient mit L1 heißt ISTA, die beschleunigte Variante FISTA.

### Beispiele für Nebenbedingungen (→ VL4, F. 77)
Nichtnegativität (Koeffizienten, Matrixfaktoren ≥ 0); Simplex (Wahrscheinlichkeiten ≥ 0, Summe 1); Normschranken (Kapazität, Robustheit, Lipschitz-Verhalten); Monotonie (Vorhersage steigt mit ausgewählten Merkmalen); Fairness-/Ressourcenbedingungen.

### Algorithmenfamilien (→ VL4, F. 78)
| Familie | Kernidee | Passt für |
|---|---|---|
| Projizierter Gradient | Gradientenschritt, dann Projektion | einfache Mengen: Boxen, Kugeln, Simplex |
| Proximale Methoden | proximaler Operator für nicht glatte Strafen | L1, Sparsity, zusammengesetzte Ziele |
| Penalty-Methoden | großer Strafterm für Verletzungen | leicht implementierbar; Nebenbedingungen nur näherungsweise |
| Barrier / Interior Point | mit Barriereterm im Inneren bleiben | glatte konvexe restringierte Probleme |
| Augmented Lagrangian | Multiplikatoren + Strafterme | Gleichungsnebenbedingungen, schwierige Zulässigkeit |
| SQP | Folge quadratischer Approximationen | nichtlinear restringiert mit genauen Ableitungen |
| ADMM | Aufteilung in einfachere Teilprobleme mit Konsens-Updates | groß, verteilt, strukturiert |

## Beispiel
> [!beispiel] Rechenbeispiel (nicht aus der VL) – projizierter Gradient und KKT
> $f(\theta) = (\theta_1 - 3)^2 + (\theta_2 + 1)^2$ auf der Box $C = [0, 2]\times[0, 2]$, Start $(1, 1)$, $\eta = 0{,}25$.
> - Schritt 1: $\nabla f = (-4;\ 4)$, $y = (2;\ 0)$ liegt in $C$ ⇒ $\theta_1 = (2, 0)$.
> - Schritt 2: $\nabla f = (-2;\ 2)$, $y = (2{,}5;\ -0{,}5)$ ⇒ $\Pi_C(y) = (2;\ 0)$ (clip) – Fixpunkt.
>
> Das unrestringierte Minimum $(3, -1)$ ist unzulässig; das Optimum $(2, 0)$ liegt **auf dem Rand** (in der Ecke). **KKT-Check:** aktiv sind $g_1 = \theta_1 - 2 \le 0$ und $g_2 = -\theta_2 \le 0$. Stationarität $\nabla f + \lambda_1(1, 0) + \lambda_2(0, -1) = 0$ mit $\nabla f = (-2, 2)$ ergibt $\lambda_1 = 2 \ge 0$, $\lambda_2 = 2 \ge 0$ ✓; die inaktiven Schranken $\theta_1 \ge 0$, $\theta_2 \le 2$ haben $\lambda = 0$ (komplementärer Schlupf).

> [!beispiel] Anwendungsfall (nicht aus der VL) – Portfolio und Monotonie
> - **Portfolio:** Gewichte $w_i \ge 0$, $\sum w_i = 1$ (Simplex aus VL4, F. 77), minimiere Risiko $w^T\Sigma w$ bei Mindestrendite. Nach jedem Gradientenschritt auf den Simplex projizieren.
> - **Kreditscoring mit Monotonie:** Ein höheres Einkommen darf das vorhergesagte Ausfallrisiko nie erhöhen – aus Fairness- und Erklärbarkeitsgründen. Gradient-Boosting-Bibliotheken bieten dafür Monotonie-Nebenbedingungen pro Merkmal.

## Typische Prüfungsfragen & Fallstricke
- Projizierten Gradienten erklären und eine Projektion angeben (Box, Kugel).
- „Was ist der proximale Operator für L1?" → Soft-Thresholding.
- Vier KKT-Bedingungen nennen.
- „Warum ist der projizierte Gradient ein Spezialfall des proximalen?" → $h$ = Indikatorfunktion von $C$.
- Fallstrick: komplementären Schlupf falsch herum – **inaktive** Nebenbedingungen haben $\lambda_i = 0$.

## Verwandt
- [[regularisierung]] – harte Regularisierung als Nebenbedingung
- [[lasso-regression]] – Soft-Thresholding
- [[ridge-regression]] – L2-Kugel als zulässige Menge
- [[gradientenabstieg]] – Basisschritt
- [[fairness-mitigation]] – Fairness als Nebenbedingung (In-processing)

## Karteikarten
Allgemeine Form eines restringierten Optimierungsproblems?::$\min f(\theta)$ u. d. N. $g_i(\theta)\le0$, $h_j(\theta)=0$.
Wo liegt das restringierte Optimum oft?::Auf dem Rand der zulässigen Menge.
Vier KKT-Bedingungen?::Stationarität, primale Zulässigkeit, duale Zulässigkeit ($\lambda_i\ge0$), komplementärer Schlupf.
Was besagt komplementärer Schlupf?::Inaktive Nebenbedingungen haben Multiplikator $\lambda_i=0$.
Iteration des projizierten Gradienten?::$y_t=\theta_t-\eta\nabla f(\theta_t)$, $\theta_{t+1}=\Pi_C(y_t)$.
Definition des proximalen Operators?::$\mathrm{prox}_{\eta h}(v)=\arg\min_\theta h(\theta)+\frac{1}{2\eta}\|\theta-v\|^2$.
Proximaler Operator von $\lambda\|\theta\|_1$?::Soft-Thresholding.
Proximaler Operator der Indikatorfunktion einer Menge $C$?::Die Projektion auf $C$.
Wofür eignet sich ADMM?::Für große, verteilte oder strukturierte Probleme, die in einfachere Teilprobleme zerlegt werden.

## Quellen
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 68–78
- [[q-vl03-regularisierung-cv|VL3]], Folie 14
- [[q-nb02-mnist-regularization|NB2]]
