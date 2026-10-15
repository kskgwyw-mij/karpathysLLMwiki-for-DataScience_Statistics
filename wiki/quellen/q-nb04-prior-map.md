---
typ: quelle
themen: [map, regularisierung, notebook]
aliases: [NB4]
datei: "raw/uebungen/nb04-prior-map.ipynb"
original: "4_Jupyter Notebooks/4_prior_MAP.json"
relevanz: mittel
aktualisiert: 2026-10-15
---

# NB4 · Prior-Verteilung und Regularisierung (MLE vs. MAP)

## Zusammenfassung
Lineares Modell $y = \theta_0 + \theta_1 x + \varepsilon$ mit wahren Parametern $\theta_\text{true} = (0{,}4;\,1{,}0)$, sechs Trainingspunkten und **einem Ausreißer mit hohem Hebel** bei $x = 1{,}6$. Gaußsche Likelihood mit $\sigma_\text{noise} = 0{,}8$, gaußscher Prior mit Mittelwert $(0,0)$ und $\lambda = 2$. Heatmaps von Likelihood, Prior und Posterior; Vergleich der Geraden und des Testfehlers.

## Ergebnisse
| | $\theta_0$ | $\theta_1$ | Abstand zu $\theta_\text{true}$ | Test-MSE |
|---|---|---|---|---|
| MLE (= kleinste Quadrate) | 0,616 | 1,335 | 0,398 | 0,1688 |
| MAP (= Ridge) | 0,504 | 1,111 | 0,152 | 0,0242 |

MAP-Schätzer: $\big(X^TX + \lambda\sigma_\text{noise}^2 I\big)\theta = X^Ty$ (Zelle 8).

## Prüfungsrelevante Aussagen
- Der Prior hilft, **obwohl** sein Mittelwert $(0,0)$ nicht die wahren Parameter sind – er wirkt der Überanpassung an wenige, verrauschte Daten entgegen.
- $\lambda = 0$ → MLE; kleines bis mittleres $\lambda$ → nützliches Schrumpfen; sehr großes $\lambda$ → Underfitting Richtung $(0,0)$ (Zelle 19).
- Siehe [[probabilistische-sicht-map]] und [[ridge-regression]].
