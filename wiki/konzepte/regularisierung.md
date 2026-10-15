---
typ: konzept
themen: [regularisierung]
aliases: [Regularization, Regularisierungsmethoden, Harte Regularisierung, Weiche Regularisierung, Variationelle Regularisierung, Regularisierung durch Design]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-nb02-mnist-regularization]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Regularisierung (Überblick)

#flashcards/statistik

## Kurzdefinition
**Definition:** Methoden, um die Komplexität des Modells zu steuern. **Ziel:** die Generalisierbarkeit des Modells verbessern (→ [[q-vl00-essentiell|VL0]], F. 36). Regularisierung senkt vor allem die **Varianz** und erkauft das mit etwas Bias ([[bias-varianz-tradeoff]]).

## Intuition
Ohne Regularisierung minimiert man nur den Datenfehler – das Modell darf beliebig kompliziert werden. Regularisierung bringt **Vorwissen** ein (→ VL0, F. 38): „Koeffizienten sind eher klein", „nur wenige Merkmale zählen", „das Ausgabebild ist glatt". Dieses Vorwissen bestimmt die passende Methode und kann sogar neue Methoden begründen. Oft werden Algorithmen-Namen statt Methodennamen verwendet (Ridge statt „weiche L2-Regularisierung") (→ VL0, F. 36).

## Formal
**Ausgangsproblem** (→ VL0, F. 37): Modell $f_\theta$ mit Parametern $\theta\in\mathbb R^d$,
$$\mathcal L(\theta; X, Y) = \frac{1}{N}\sum_{i=1}^N \mathcal D\big(f_\theta(x_i), y_i\big), \qquad \text{häufig } \mathcal D = \|f_\theta(x_i) - y_i\|^2$$

### Taxonomie (→ VL0, F. 36–43)
| Methode | Idee | Formel / Umsetzung | Seite |
|---|---|---|---|
| **Harte** Regularisierung | Modellklasse strikt einschränken (Gleichung/Ungleichung, beliebige Aussage) | $\min \frac1N\sum\|f_\theta(x_i)-y_i\|^2$ mit $\theta \in C \subset \mathbb R^d$ | [[ridge-regression]], [[restringierte-optimierung]] |
| **Weiche** (variationelle) Regularisierung | Bedingung als Strafterm (Funktional $\mathcal R:\mathbb R^d\to[0,\infty)$) | $\frac1N\sum\|f_\theta(x_i)-y_i\|^2 + \lambda\,\mathcal R(\theta)$, $\lambda\ge 0$ | [[ridge-regression]], [[lasso-regression]] |
| **Output**-Regularisierung | Ausgabe statt Parameter bestrafen | $\ldots + \lambda\,\mathcal R(f_\theta(x_i))$ | [[output-regularisierung]] |
| **Early Stopping** | Optimierung frühzeitig abbrechen | Val-Daten als Abbruchkriterium | [[early-stopping]] |
| Regularisierung **durch Design** | Modellklasse so wählen, dass sie nicht überanpassen kann | Deep Image Prior, Equivariant Networks, Input Convex Networks; Vorwissen steckt in der Modellklasse | – |
| **Dropout** | Bestandteile zufällig weglassen (nur im Training) | Ensemble mit geteilten Parametern | [[dropout]] |
| **Label Smoothing** | harte One-Hot-Labels durch weiche ersetzen | $y = (1-\varepsilon)y + \varepsilon(1-y)$ | [[label-smoothing]] |

$\lambda$ heißt **Regularisierungsparameter** und gibt an, wie stark die Bestrafung einfließt (→ VL0, F. 40). Mehrere Funktionale lassen sich kombinieren: $\ldots + \lambda_1\mathcal R_1(\theta) + \lambda_2\mathcal R_2(\theta)$, z. B. Elastic Net = L1 + L2 (→ [[q-vl03-regularisierung-cv|VL3]], F. 23).

### Wahrscheinlichkeitstheoretische Deutung
Weiche Regularisierung = Maximum a posteriori mit Prior $p(\theta)$: Gauß-Prior → L2, Laplace-Prior → L1. Siehe [[probabilistische-sicht-map]].

## Entscheidungsschema (→ VL0, F. 53–54)
| Szenario | Empfehlung | Hinweise |
|---|---|---|
| Wenig Daten – viele Features | L2/L1, Early Stopping, einfache Architektur | standardisieren, Cross-Validation für $\lambda$ |
| Strukturierte Outputs | TV/Laplace | Output-Regularisierung ist oft hilfreich |
| Starkes Vorwissen | Regularisierung durch Design | Kapazität begrenzen, induktiven Bias nutzen |
| Optimierung instabil | Gradient-Clipping, Max-Norm, Non-Negativity bei Bedarf | kleinere Learning Rate |

Kein allgemeines Schema, nur Vorschläge.

## Hyperparameter-Richtwerte (→ VL3, F. 41)
| Hyperparameter | Bereich |
|---|---|
| L2 $\lambda$ | $10^{-6}\ldots10^{-2}$ |
| L1 $\lambda$ | $10^{-6}\ldots10^{-3}$ |
| Max-Norm | 1 … 3 (pro Layer) |
| Dropout-Rate | 0,1 … 0,5 |
| Patience (Early Stopping) | 5–10 |
| Lernraten-Schedule | oft hilfreich |

**Standard-Baseline** (→ VL3, F. 44): kleines L2 ($10^{-4}$), Early Stopping, kein Dropout; danach Output-Regularisierung, falls die Outputs strukturiert sind. Auswahlprozess für $\lambda$: [[train-val-test-split]].

## Diagnose Unter- vs. Über-Regularisierung (→ VL0, F. 57)
| | Unter-Regularisierung ($\lambda$ zu klein) | Über-Regularisierung ($\lambda$ zu groß) |
|---|---|---|
| Symptom | hohe Varianz, große Lücke Train vs. Val, instabile Grenzen | hoher Bias, Train **und** Val schlecht, verschwommene/zu glatte Outputs |
| Maßnahme | $\lambda$ erhöhen, Kapazität senken, Early Stopping aggressiver | $\lambda$ reduzieren, Kapazität leicht erhöhen, Datenaugmentation prüfen |
| TV (Output) | zu wenig TV: Zacken/Rauschen, „flatternde" Grenzen | zu viel TV: überglättete, verkürzte Strukturen, keine feinen Details |

## Beispiel
[[q-nb02-mnist-regularization|NB2]] vergleicht acht Varianten auf MNIST. Alle Test-Accuracies liegen zwischen 0,898 (Nichtnegativität, zu starke Einschränkung) und 0,934 (Label Smoothing). L1 erzeugt siebenmal so viele Nullgewichte wie die Baseline.

## Typische Prüfungsfragen & Fallstricke
- „Definieren Sie Regularisierung und nennen Sie fünf Methoden." → Definition + Taxonomie.
- **Hart vs. weich** unterscheiden: Nebenbedingung $\theta\in C$ vs. Strafterm $\lambda\mathcal R(\theta)$.
- Diagnose aus Lernkurven ablesen (Tabelle oben).
- Fallstrick: $\lambda = 0$ → keine Regularisierung, $\lambda\to\infty$ → „nur Regularisierung", bei L2 $\theta = 0$.
- Fallstrick: Regularisierung reduziert Varianz, nicht den irreduziblen Fehler.

## Verwandt
- [[bias-varianz-tradeoff]] – warum Regularisierung hilft
- [[l0-l1-l2-vergleich]] – Geometrie und Eigenschaften der Normen
- [[probabilistische-sicht-map]] – Regularisierung als Prior
- [[train-val-test-split]] – wie man $\lambda$ wählt
- [[restringierte-optimierung]] – wie man harte Regularisierung umsetzt (Projektion, proximal)

## Karteikarten
Wie definiert die VL Regularisierung?::Methoden, um die Komplexität des Modells zu steuern, mit dem Ziel, die Generalisierbarkeit zu verbessern.
Unterschied harte vs. weiche Regularisierung?::Hart: Nebenbedingung $\theta\in C$ (Modellklasse einschränken). Weich: Strafterm $\lambda\mathcal R(\theta)$ zum Loss addieren.
Was ist der Regularisierungsparameter $\lambda$?::Zahl $\lambda\ge 0$, die angibt, wie stark die Bestrafung in die Optimierung einfließt.
Was ist Output-Regularisierung?::Nicht die Parameter, sondern der Output $f_\theta(x)$ wird bestraft: $\ldots + \lambda\mathcal R(f_\theta(x_i))$.
Was ist Regularisierung durch Design?::Wahl einer Modellklasse, die nicht überanpassen kann (z. B. Deep Image Prior, Equivariant Networks); Vorwissen steckt in der Modellklasse.
Empfehlung bei wenig Daten und vielen Features?::L2/L1, Early Stopping, einfache Architektur; standardisieren, $\lambda$ per CV.
Symptome von Unter-Regularisierung?::Hohe Varianz, große Lücke Train vs. Val, instabile Grenzen.
Symptome von Über-Regularisierung?::Hoher Bias, Train und Val schlecht, zu glatte Outputs.
Richtwert für L2-$\lambda$ und Dropout-Rate?::L2: $10^{-6}$ bis $10^{-2}$; Dropout: 0,1 bis 0,5.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 35–57
- [[q-vl03-regularisierung-cv|VL3]], Folien 2–44
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 5–11
- [[q-nb02-mnist-regularization|NB2]]
