---
typ: methode
themen: [regularisierung, regression]
aliases: [L2-Regularisierung, Ridge, Tikhonov-Regularisierung, Weight Decay, Quadratische Regularisierung]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-vl05-xai-transparente-globale-modelle]]", "[[q-nb04-prior-map]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Ridge-Regression / L2-Regularisierung

#flashcards/statistik

## Kurzdefinition
Bei der (variationellen) **L2-Regularisierung** wird eine große quadratische euklidische Norm der Parameter bestraft: $\mathcal L(\theta) = \frac1N\sum_i\|f_\theta(x_i)-y_i\|^2 + \lambda\|\theta\|^2$ (→ [[q-vl00-essentiell|VL0]], F. 45). Andere Namen: **Tikhonov-Regularisierung, Ridge Regression, Weight Decay, quadratische Regularisierung** (→ [[q-vl03-regularisierung-cv|VL3]], F. 12).

## Intuition
Große Koeffizienten bedeuten, dass kleine Änderungen in $x$ große Änderungen in der Vorhersage auslösen – ein Zeichen von Überanpassung. L2 zieht alle Koeffizienten gleichmäßig Richtung 0, macht sie aber (anders als L1) nicht exakt 0. **Effekt: Stetigkeit** (→ VL3, F. 12); das Modell reagiert ruhiger auf die Daten und verbessert die Stabilität (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 13).

## Formal
**Hart** (Nebenbedingung, → VL3, F. 12):
$$\min_\theta \frac1N\sum_{i=1}^N\|f_\theta(x_i)-y_i\|^2 \quad\text{mit}\quad \theta\in B_R = \{\theta : \|\theta\| \le R\}$$
Geometrisch: zulässig ist nur das Innere einer **Kugel** (in 2D ein Kreis) um 0 (→ VL3, F. 13).

**Weich** (Strafterm, → VL0, F. 45):
$$\mathcal L(\theta) = \frac1N\sum_{i=1}^N\|f_\theta(x_i)-y_i\|^2 + \lambda\,\|\theta\|^2$$
- $\lambda = 0$: keine Regularisierung
- $\lambda\to\infty$: „nur Regularisierung", $\theta = 0$

Verallgemeinerung: jede andere Norm, auch mit Transformation $\lambda\|T(\theta)\|^2$ (→ VL3, F. 16).

**Umsetzung** (→ VL3, F. 14): Minimierung mit Strafterm (am verbreitetsten), Projected Gradient Descent (für die harte Variante), proximale Methoden → [[restringierte-optimierung]].

> [!ergaenzung] Ergänzung (nicht aus der VL) – geschlossene Lösung für das lineare Modell
> Für $f_\theta(x) = x^T\theta$ und die Strafe ohne $\frac1N$, also $\|y - X\theta\|^2 + \lambda\|\theta\|^2$:
> $$\nabla = -2X^Ty + 2X^TX\theta + 2\lambda\theta = 0 \;\Rightarrow\; \hat\theta_\text{Ridge} = (X^TX + \lambda I)^{-1}X^Ty$$
> Mit $\frac1N$ vor der Datensumme wird daraus $(X^TX + N\lambda I)^{-1}X^Ty$. $X^TX + \lambda I$ ist für $\lambda > 0$ **immer invertierbar** – Ridge löst also auch das Problem kollinearer Merkmale. In [[q-nb04-prior-map|NB4]] steht dieselbe Form als $(X^TX + \lambda\sigma^2 I)\theta = X^Ty$ (MAP mit Gauß-Prior).

## Annahmen & Voraussetzungen
- Merkmale **standardisieren**, sonst bestraft L2 Koeffizienten je nach Einheit unterschiedlich stark (→ VL0, F. 54: „Standardisieren").

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Den Achsenabschnitt bestraft man üblicherweise nicht (ESL 3.4.1), sonst hängt das Ergebnis von der Lage der Daten ab.

## Beispiel
Daten aus [[lineare-regression-normalgleichungen]], Strafe auf beide Koeffizienten:

| $\lambda$ | $a_0$ | $a_1$ | $\lVert a\rVert_2$ |
|---|---|---|---|
| 0 | 0,250 | −0,250 | 0,354 |
| 1 | 0,201 | −0,254 | 0,324 |
| 10 | 0,090 | −0,241 | 0,257 |

MAP in [[q-nb04-prior-map|NB4]]: Ridge verschiebt den durch einen Ausreißer verzerrten Schätzer von $(0{,}616;\,1{,}335)$ auf $(0{,}504;\,1{,}111)$, nahe an die wahren $(0{,}4;\,1{,}0)$; Test-MSE sinkt von 0,169 auf 0,024.

## Vor- und Nachteile
| + | − |
|---|---|
| konvex, differenzierbar, geschlossene Lösung im linearen Fall | keine Sparsity, also keine Feature-Selektion |
| stabilisiert bei Kollinearität | $\lambda$ muss gewählt werden (CV) |
| äquivalent zu Early Stopping bei GD im linearen Modell ([[early-stopping]]) | abhängig von der Skalierung der Merkmale |

## Typische Prüfungsfragen & Fallstricke
- Vier Namen der L2-Regularisierung nennen.
- Geometrie skizzieren (Kreis) und mit L1 (Raute) vergleichen → [[l0-l1-l2-vergleich]].
- Grenzfälle $\lambda = 0$ und $\lambda\to\infty$.
- **Fallstrick:** Ridge verkleinert die **Norm** des Koeffizientenvektors, nicht zwingend jeden einzelnen Koeffizienten (siehe $a_1$ bei $\lambda = 1$ oben).
- Fallstrick: L2 setzt Koeffizienten nicht exakt auf 0.
- Fallstrick: In NB3 ist der „L2-Term" `torch.norm(param)`, also $\|\theta\|$ statt $\|\theta\|^2$ ([[q-nb03-regularization-simple|NB3]]).

## Verwandt
- [[lasso-regression]] – L1, erzeugt Sparsity
- [[l0-l1-l2-vergleich]] – Geometrie
- [[probabilistische-sicht-map]] – L2 = Gauß-Prior
- [[early-stopping]] – äquivalent im linearen Fall
- [[adaptive-optimierer]] – AdamW statt L2 in Adam

## Karteikarten
Wie lautet die weiche L2-Regularisierung?::$\mathcal L(\theta) = \frac1N\sum\|f_\theta(x_i)-y_i\|^2 + \lambda\|\theta\|^2$.
Vier Namen der L2-Regularisierung?::Tikhonov-Regularisierung, Ridge Regression, Weight Decay, quadratische Regularisierung.
Was passiert bei L2 für $\lambda\to\infty$?::Nur noch Regularisierung, $\theta = 0$.
Welchen Effekt hat L2 laut VL?::Stetigkeit (und Stabilität der Koeffizienten).
Wie sieht die zulässige Menge der harten L2-Regularisierung in 2D aus?::Ein Kreis (Kugel) $\{\theta:\|\theta\|\le R\}$ um den Ursprung.
Geschlossene Ridge-Lösung im linearen Modell?::$\hat\theta = (X^TX+\lambda I)^{-1}X^Ty$ (Strafe ohne $1/N$).
Warum ist $X^TX+\lambda I$ für $\lambda>0$ immer invertierbar?::$X^TX$ ist positiv semidefinit; $+\lambda I$ macht alle Eigenwerte $\ge\lambda>0$.
Setzt L2 Koeffizienten exakt auf null?::Nein, nur L1 (bzw. L0) erzeugt exakte Nullen.

## Quellen
- [[q-vl00-essentiell|VL0]], Folie 45
- [[q-vl03-regularisierung-cv|VL3]], Folien 12–16
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folie 13
- [[q-nb04-prior-map|NB4]]
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 3.4.1, S. 61
