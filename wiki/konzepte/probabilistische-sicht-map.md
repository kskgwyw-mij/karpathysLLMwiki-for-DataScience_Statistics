---
typ: konzept
themen: [map, regularisierung, wahrscheinlichkeit]
aliases: [Probabilistische Sicht, MAP, Maximum a posteriori, MLE, Maximum Likelihood, Bayes, Prior, Posterior, Likelihood]
quellen: ["[[q-vl03-regularisierung-cv]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-nb04-prior-map]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Probabilistische Sicht: Likelihood, Prior, MAP

#flashcards/statistik

> [!info] Relevanz
> Nicht in der Auswahl „essentiell", aber in VL3 und VL4 ausführlich und Kern von Kapitel 5 („Probabilistic and Graphical Modeling"). Erklärt, **warum** quadratischer Fehler und L1/L2 natürlich sind.

## Kurzdefinition
Statt einen Loss zu minimieren, maximiert man die **Posterior-Wahrscheinlichkeit** $p(\theta\mid D)$ der Modellparameter gegeben die Daten (MAP). Nach Bayes ist sie proportional zu **Likelihood × Prior**; der negative Logarithmus liefert genau „Datenfehler + Regularisierungsterm" (→ [[q-vl03-regularisierung-cv|VL3]], F. 34–37).

## Intuition
- **Likelihood** $p(D\mid\theta)$: Wie wahrscheinlich sind die beobachteten Daten, wenn das Modell mit Parametern $\theta$ stimmt?
- **Prior** $p(\theta)$: Was glauben wir **vor** den Daten über $\theta$ (z. B. „eher klein")?
- **Posterior** $p(\theta\mid D)$: Wie wahrscheinlich ist ein Modell, **nachdem** wir die Daten gesehen haben?

Wenige, verrauschte Daten → die Likelihood ist breit oder durch Ausreißer verzerrt → der Prior zieht die Schätzung zu plausiblen Werten zurück.

## Formal
Datensatz $D = \{(x_i, y_i)\}$. Satz von Bayes (→ VL3, F. 35):
$$p(\theta\mid D) \;\propto\; \underbrace{p(D\mid\theta)}_{\text{Likelihood}}\cdot\underbrace{p(\theta)}_{\text{Model Prior}}$$
Ein Output-Prior ist hier nicht berücksichtigt. Maximieren ist äquivalent zum Minimieren des negativen Logarithmus:
$$-\log p(\theta\mid D) = -\log p(D\mid\theta) - \log p(\theta) + \text{const}$$

**Likelihood** (Standardwahl, unabhängige Beobachtungen, Gauß-Rauschen, Varianzen weggelassen, → VL3, F. 36):
$$p(D\mid\theta) = \prod_{i=1}^N \exp\big(-\|f_\theta(x_i) - y_i\|^2\big) \;\Rightarrow\; -\log p(D\mid\theta) = \sum_{i=1}^N\|f_\theta(x_i) - y_i\|^2$$
→ Kleinste Quadrate = **Maximum Likelihood** unter Gauß-Rauschen.

**Prior** (→ VL3, F. 37):
| Prior | $-\log p(\theta)$ | ergibt |
|---|---|---|
| $p(\theta) = \exp(-\|\theta\|^2)$ (Gauß) | $\|\theta\|^2$ | L2-Regularisierung ([[ridge-regression]]) |
| $p(\theta) = \exp(-\|\theta\|_1)$ (Laplace) | $\|\theta\|_1$ | L1-Regularisierung ([[lasso-regression]]) |

Gleichheiten gelten nur bis auf Konstanten; Varianzen und Normierungen sind weggelassen. Mit Varianzen ([[q-nb04-prior-map|NB4]]):
$$\text{MAP minimiert}\quad \frac{\mathrm{SSE}(\theta)}{2\sigma_\text{noise}^2} + \frac{\lambda}{2}\|\theta\|^2 \;\Rightarrow\; \big(X^TX + \lambda\sigma_\text{noise}^2 I\big)\theta = X^Ty$$
Der Regularisierungsparameter entspricht also dem Verhältnis von Rauschvarianz zu Prior-Varianz.

**Maximieren ↔ Minimieren** (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 28): Maximum Likelihood maximiert die Log-Likelihood $\ell(\theta)$; äquivalent minimiert man die negative Log-Likelihood $-\ell(\theta)$. Der Optimierer ist identisch, nur Vorzeichen von Ziel und Gradient ändern sich.

## Annahmen & Voraussetzungen
- Unabhängige Beobachtungen (Produkt der Einzel-Likelihoods).
- Gauß-Rauschen für den quadratischen Fehler.

## Beispiel ([[q-nb04-prior-map|NB4]])
Wahre Parameter $(0{,}4;\,1{,}0)$, sechs Punkte mit einem Ausreißer bei hohem $x$, $\sigma_\text{noise} = 0{,}8$, $\lambda = 2$:
| | Schätzung | Abstand zur Wahrheit | Test-MSE |
|---|---|---|---|
| MLE | $(0{,}616;\,1{,}335)$ | 0,398 | 0,169 |
| MAP | $(0{,}504;\,1{,}111)$ | 0,152 | 0,024 |

Der Prior-Mittelwert $(0,0)$ ist **nicht** die Wahrheit – er hilft trotzdem, weil er der Überanpassung entgegenwirkt. Bei sehr großem $\lambda$ wird zu stark Richtung $(0,0)$ geschrumpft (Underfitting).

## Typische Prüfungsfragen & Fallstricke
- „Zeigen Sie, dass ein Gauß-Prior zur L2-Regularisierung führt." → $-\log$ von Likelihood × Prior.
- „Warum minimiert man den negativen Log-Posterior statt den Posterior zu maximieren?" → Logarithmus ist monoton, Produkte werden Summen, Software minimiert.
- Fallstrick: Posterior $\ne$ Likelihood; MLE ignoriert den Prior.
- Fallstrick: „Der Prior muss richtig sein, damit er hilft" – falsch, siehe NB4.

## Verwandt
- [[regularisierung]] – Prior = Vorwissen
- [[ridge-regression]], [[lasso-regression]] – die zwei Standard-Priors
- [[lineare-regression]] – MLE unter Gauß-Rauschen
- [[optimierung-grundlagen]] – Max ↔ Min
- [[bootstrap]] – alternative Unsicherheitsquantifizierung ohne Prior

## Karteikarten
Wie lautet der Satz von Bayes für Modellparameter?::$p(\theta\mid D)\propto p(D\mid\theta)\,p(\theta)$ – Posterior ∝ Likelihood × Prior.
Was ist die Likelihood?::Die Wahrscheinlichkeit, dass die Daten durch ein bestimmtes Modell $\theta$ entstanden sind: $p(D\mid\theta)$.
Was ist der Posterior?::Die Wahrscheinlichkeit eines Modells, gegeben die Daten: $p(\theta\mid D)$.
Welcher Prior führt zu L2-, welcher zu L1-Regularisierung?::$e^{-\|\theta\|^2}$ (Gauß) → L2; $e^{-\|\theta\|_1}$ (Laplace) → L1.
Welche Likelihood führt zum quadratischen Fehler?::$\prod_i\exp(-\|f_\theta(x_i)-y_i\|^2)$, also Gauß-Rauschen.
Was ist der Unterschied zwischen MLE und MAP?::MLE maximiert nur die Likelihood; MAP maximiert Likelihood × Prior (= regularisierte Schätzung).
Wie wird aus einer Maximierung eine Minimierung?::$\max g \Leftrightarrow \min -g$; z. B. negative Log-Likelihood minimieren.
Warum hilft ein Prior mit Mittelwert 0, obwohl die wahren Parameter nicht 0 sind?::Er schrumpft eine durch wenige, verrauschte Daten überangepasste Schätzung – weniger Varianz.

## Quellen
- [[q-vl03-regularisierung-cv|VL3]], Folien 5, 34–37
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 1–4, 28
- [[q-nb04-prior-map|NB4]]
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitte 8.2–8.3, S. 261–267
