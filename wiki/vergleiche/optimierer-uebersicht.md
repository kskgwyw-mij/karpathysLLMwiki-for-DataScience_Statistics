---
typ: vergleich
themen: [optimierung]
aliases: [Optimierer-Vergleich, Optimization takeaways, Algorithmenübersicht]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-lab02-pytorch-optimizer-comparison]]", "[[q-abschlussprojekt-abgabe]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-16
---

# Übersicht: Optimierungsverfahren

#flashcards/statistik

## Familien (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 80)
| Familie | Algorithmen | Hauptidee |
|---|---|---|
| Erste Ordnung | Gradientenabstieg, Momentum, Nesterov, proximaler Gradient | Gradienten nutzen |
| Stochastisch/adaptiv | SGD, AdaGrad, AdaDelta, Adam, LION | Minibatches und/oder koordinatenweise Statistiken |
| Zweite Ordnung | Newton, BFGS, L-BFGS | Krümmung oder Krümmungsapproximation |
| Restringiert | projizierter Gradient, FISTA, Penalty, Barrier, Augmented Lagrangian, ADMM, SQP | Zulässigkeit über Projektion, Strafterme, Barrieren oder Teilprobleme |

## Update-Regeln auf einen Blick
| Verfahren | Update | Seite |
|---|---|---|
| GD | $\theta_{t+1} = \theta_t - \eta\nabla f(\theta_t)$ | [[gradientenabstieg]] |
| Momentum | $v_{t+1} = \beta v_t + \nabla f(\theta_t)$; $\theta_{t+1} = \theta_t - \eta v_{t+1}$ | [[momentum-und-nesterov]] |
| Nesterov | $v_{t+1} = \beta v_t + \nabla f(\theta_t - \eta\beta v_t)$; $\theta_{t+1} = \theta_t - \eta v_{t+1}$ | [[momentum-und-nesterov]] |
| SGD | $\theta_{t+1} = \theta_t - \eta\nabla\ell_B(\theta_t)$ | [[sgd]] |
| AdaGrad | $G_t = G_{t-1} + g_t^2$; $\theta_t = \theta_{t-1} - \eta g_t/\sqrt{G_t + \varepsilon}$ | [[adaptive-optimierer]] |
| Adam | $\hat m_t, \hat v_t$ bias-korrigiert; $\theta_t = \theta_{t-1} - \alpha\hat m_t/\sqrt{\hat v_t + \varepsilon}$ | [[adaptive-optimierer]] |
| LION | $\theta_t = \theta_{t-1} - \eta\,\mathrm{sign}(u_t) - \eta\lambda\theta_{t-1}$ | [[adaptive-optimierer]] |
| Newton | $\theta_{t+1} = \theta_t - H^{-1}\nabla f(\theta_t)$ | [[newton-und-quasi-newton]] |
| Projizierter GD | $\theta_{t+1} = \Pi_C(\theta_t - \eta\nabla f(\theta_t))$ | [[restringierte-optimierung]] |
| Proximaler GD | $\theta_{t+1} = \mathrm{prox}_{\eta h}(\theta_t - \eta\nabla f(\theta_t))$ | [[restringierte-optimierung]] |

> [!ergaenzung] Ergänzung (nicht aus der VL) – RMSprop und AdamW aus dem Lab
> Beide kommen in [[q-lab02-pytorch-optimizer-comparison|Lab 2]] nur als Beschreibung vor, die Formeln sind ergänzt:
>
> | Verfahren | Update | Seite |
> |---|---|---|
> | RMSprop | $v_t = \alpha v_{t-1} + (1-\alpha)g_t^2$; $\theta_t = \theta_{t-1} - \eta\,g_t/(\sqrt{v_t} + \varepsilon)$ | [[adaptive-optimierer]] |
> | AdamW | wie Adam, zusätzlich $-\alpha\lambda\theta_{t-1}$ außerhalb der adaptiven Skalierung | [[adaptive-optimierer]] |

## Empirischer Vergleich im Lab (→ [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitt „Test accuracy table")
MNIST, gleiches Netz (784 → 128 → 64 → 10), gleicher Seed, 8 Epochen, je eine feste Lernrate:

| Rang | Optimierer | Lernrate | Test-Acc |
|---|---|---|---|
| 1 | SGD mit Momentum | 0,05 | 0,935 |
| 2 | AdamW | $10^{-3}$ | 0,923 |
| 3 | Adam | $10^{-3}$ | 0,923 |
| 4 | RMSprop | $10^{-3}$ | 0,915 |
| 5 | SGD | 0,1 | 0,908 |
| 6 | LION | $10^{-4}$ | 0,894 |
| 7 | L-BFGS | 0,8 | 0,708 |

Fazit des Notebooks: Das Ranking hängt von Lernrate, Epochen, Batchgröße, Architektur, Seed, Regularisierung und Datenmenge ab; Vergleiche sind nur mit Blick auf die Hyperparameter aussagekräftig. Im [[q-abschlussprojekt-abgabe|eigenen Projekt]] lag SGD mit Momentum dagegen hinten.

## Erste vs. zweite Ordnung (→ [[q-vl00-essentiell|VL0]], F. 94)
| Erste Ordnung | Zweite Ordnung / Quasi-Newton |
|---|---|
| billige Iterationen; skaliert auf riesige Daten und Parameterzahlen | krümmungsbewusster; weniger Iterationen möglich |
| natürlich für Minibatches und GPUs | höhere Kosten pro Iteration, mehr Speicher |
| braucht Lernraten-Tuning und oft viele Iterationen | attraktiv für kleinere glatte Probleme oder „final polishing" |

## Takeaways (→ VL4, F. 81)
- Ziel, Variablen, Nebenbedingungen und Validierungsprotokoll klar angeben.
- Konvexität gibt starke Garantien; nichtkonvexe Probleme brauchen Diagnostik und mehrere Läufe.
- GD ist einfach; Momentum und Beschleunigung helfen bei schwieriger Krümmung.
- SGD und adaptive Optimierer tauschen deterministischen Fortschritt gegen skalierbare, verrauschte Updates.
- Zweite Ordnung kann sehr effizient sein, wenn Krümmungsinformation bezahlbar ist.
- Restringierte Probleme brauchen Algorithmen, die Zulässigkeit respektieren oder bestrafen.
- **Initialisierung, Lernraten-Schedules und Stoppkriterien sind Teil des Modelldesigns** ([[initialisierung]]).

## Karteikarten
Welche vier Familien von Optimierungsverfahren unterscheidet die VL?::Erste Ordnung, stochastisch/adaptiv, zweite Ordnung, restringiert.
Zwei Vorteile von Verfahren erster Ordnung?::Billige Iterationen, skalieren auf riesige Daten; natürlich für Minibatches und GPUs.
Was ist laut Takeaways ebenfalls Teil des Modelldesigns?::Initialisierung, Lernraten-Schedules und Stoppkriterien.

## Verwandt
- [[optimierung-grundlagen]]
- [[gradientenabstieg-von-hand]]
- [[adam-adagrad-erster-schritt]]
- [[trainingsschleife]] – wo der Optimierer im Trainingsloop sitzt (`optimizer.step()`)
