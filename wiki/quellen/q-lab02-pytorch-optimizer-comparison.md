---
typ: quelle
themen: [optimierung, neuronale-netze, notebook, lab]
aliases: [Lab 2, Lab02, Optimierer-Vergleich MNIST]
datei: "raw/uebungen/lab02-pytorch-optimizer-comparison.ipynb"
original: "Lab-Kurs (Moodle-Export 14.10.2026): 2_Notebooks/2_pytorch_optimizer_comparison.json"
relevanz: mittel
aktualisiert: 2026-10-16
---

# Lab 2 · Optimierer im Vergleich (MNIST, PyTorch)

Kurs-Notebook aus dem Lab, Fortsetzung von [[q-lab01-pytorch-introduction|Lab 1]] (42 Zellen). Belege nennen den Abschnittstitel im Notebook.

## Zusammenfassung
Dasselbe Netz (784 → 128 → 64 → 10, ReLU, Dropout 0,2) wird mit identischem Seed, identischen Daten (5 000 / 1 000 / 10 000 MNIST-Bilder) und identischem Trainingsloop über 8 Epochen trainiert. Nur der Optimierer wechselt: SGD, SGD mit Momentum, RMSprop, Adam, AdamW, LION (eigene Implementierung) und L-BFGS. Danach werden mit Adam vier Lernraten-Schedules (konstant, step, exponential, cosine) und drei Lernraten verglichen. Early Stopping gibt es hier nicht; ausgewertet wird das Modell nach der letzten Epoche.

> [!warning] Lücken im Notebook (Übungsaufgaben)
> Die Funktionen `create_optimizer` und `create_scheduler` sind leere Zellen, und in der LION-Klasse fehlen die beiden Update-Zeilen. Die Ausgaben stammen aus einer vollständigen Fassung. Hyperparameter, die nur in diesen Lücken stehen (Momentum-Wert, Schedule-Parameter, L-BFGS-Einstellungen), sind deshalb nicht direkt belegt. Der Momentum-Wert 0,9 ergibt sich aus Übungsfrage 2 („Change the momentum value from 0.9 …").

## Gliederung
| Abschnitte | Inhalt | Wiki-Seiten |
|---|---|---|
| Optimization in neural networks | Optimierer bestimmt, *wie* aus dem Gradienten ein Update wird | [[trainingsschleife]], [[gradientenabstieg]] |
| The optimizers we compare | Kurzbeschreibung von SGD, Momentum, RMSprop, Adam, AdamW, L-BFGS, LION | [[sgd]], [[momentum-und-nesterov]], [[adaptive-optimierer]], [[newton-und-quasi-newton]] |
| A small LION optimizer implementation | Update-Richtung = Vorzeichen einer momentumartigen Schätzung, entkoppelter Weight Decay | [[adaptive-optimierer]] |
| Learning-rate schedules | konstant, step, exponential, cosine (`torch.optim.lr_scheduler`) | [[gradientenabstieg]], [[sgd]] |
| Training function | L-BFGS braucht eine **Closure** | [[trainingsschleife]], [[newton-und-quasi-newton]] |
| Optimizer experiments, test accuracy table | Ergebnisse unten | [[optimierer-uebersicht]] |
| Comparing learning-rate schedules, learning rate sensitivity | Schedules und Lernraten für Adam | [[gradientenabstieg]] |
| Summary, Try | Takeaways und zehn Übungsfragen | – |

## Ergebnisse Optimierer (Abschnitt „Test accuracy table")
| Optimierer | Lernrate | Weight Decay | beste Val-Acc | Val-Acc Epoche 8 | Test-Acc |
|---|---|---|---|---|---|
| SGD mit Momentum | 0,05 | 0 | 0,940 | 0,936 | **0,9353** |
| AdamW | $10^{-3}$ | $10^{-4}$ | 0,919 | 0,908 | 0,9228 |
| Adam | $10^{-3}$ | 0 | 0,917 | 0,909 | 0,9226 |
| RMSprop | $10^{-3}$ | 0 | 0,926 | 0,916 | 0,9149 |
| SGD | 0,1 | 0 | 0,912 | 0,898 | 0,9082 |
| LION | $10^{-4}$ | $10^{-4}$ | 0,896 | 0,896 | 0,8939 |
| L-BFGS | 0,8 | 0 | 0,895 | 0,687 | 0,7078 |

Auffällige Verläufe im Trainingslog (Abschnitt „Optimizer experiments"):
- **L-BFGS** ist auf Minibatches instabil: Der Trainings-Loss springt in Epoche 3 auf 3 968, die Val-Accuracy schwankt zwischen 0,40 und 0,90.
- **SGD** ohne Momentum startet langsam (Val-Acc 0,38 nach Epoche 1, erst ab Epoche 4 über 0,88).
- **RMSprop** schwankt: Die Val-Acc fällt von Epoche 2 auf 3 von 0,870 auf 0,747.
- **LION** mit $\eta = 10^{-4}$ lernt gleichmäßig, ist nach 8 Epochen aber noch nicht am Ende (Val-Loss sinkt bis zuletzt).
- **Adam und AdamW** verlaufen bei Weight Decay $10^{-4}$ praktisch identisch.

## Ergebnisse Schedules und Lernraten (Abschnitte „Comparing learning-rate schedules", „Learning rate sensitivity")
| Adam, $\eta_0 = 10^{-3}$, 8 Epochen | konstant | step decay | exponential decay | cosine annealing |
|---|---|---|---|---|
| Lernrate in Epoche 8 | 0,001 | 0,00025 | 0,000321 | 0,000048 |
| Test-Acc | **0,9226** | 0,9212 | 0,9197 | 0,9187 |

| Adam, 5 Epochen | $\eta = 10^{-4}$ | $\eta = 10^{-3}$ | $\eta = 10^{-2}$ |
|---|---|---|---|
| Test-Acc | 0,8460 | 0,9200 | **0,9306** |

> [!ergaenzung] Ergänzung (nicht aus der VL) – Schedule-Parameter aus den ausgegebenen Lernraten zurückgerechnet
> Die Lernraten pro Epoche stehen im Trainingslog. Daraus folgen (mit Python nachgerechnet, alle acht Werte stimmen exakt):
> - **step:** Halbierung alle 3 Epochen, $\eta_t = 10^{-3}\cdot 0{,}5^{\lfloor t/3\rfloor}$ → 0,001; 0,0005 (ab Epoche 4); 0,00025 (ab Epoche 7)
> - **exponential:** $\eta_t = 10^{-3}\cdot 0{,}85^t$ → 0,00085; 0,000722; …; 0,000321
> - **cosine:** $\eta_t = \eta_\min + \tfrac12(\eta_0 - \eta_\min)\big(1 + \cos(\pi t/8)\big)$ mit $\eta_\min = 10^{-5}$ → 0,000962; …; 0,000505 (Epoche 5); …; 0,000048
>
> Dabei ist $t = 0, 1, \dots, 7$ der Epochenindex (Epoche $t+1$).

## Prüfungsrelevante Aussagen
- Optimierer-Vergleiche sind nur aussagekräftig, wenn **Lernrate und andere Hyperparameter** berücksichtigt werden; ein fairer Vergleich braucht Tuning (Abschnitte „Optimizer experiments", „Summary").
- „Bester Optimierer" hängt ab von Lernrate, Epochenzahl, Batchgröße, Architektur, Seed, Regularisierung und Datensatzgröße; man soll nicht folgern, dass ein Optimierer immer der beste ist (Abschnitt „Which optimizer worked best?").
- **Momentum** sammelt Gradienten mit konsistenter Richtung und reduziert Zickzack → [[momentum-und-nesterov]]. Hier ist SGD mit Momentum der beste Optimierer.
- **RMSprop** passt die Lernrate pro Parameter über ein gleitendes Mittel der quadrierten Gradienten an → [[adaptive-optimierer]].
- **Adam** = momentumartiges Verhalten + adaptive Lernraten, starker Default. **AdamW** = Adam mit **entkoppeltem** Weight Decay, bevorzugt, wenn Weight Decay zur Regularisierung verwendet wird.
- **L-BFGS** ist Quasi-Newton, nähert Krümmungsinformation an und funktioniert eher bei kleineren, glatten Problemen. In PyTorch braucht `step` eine **Closure**, weil das Verfahren Loss und Gradient pro Update mehrfach auswerten kann (Abschnitte „The optimizers we compare", „Training function") → [[newton-und-quasi-newton]].
- **LION** verwendet das Vorzeichen einer momentumartigen Schätzung; die Schrittgröße steuert vor allem die Lernrate. Es kann speichereffizient sein (Abschnitt „Summary").
- **Lernraten-Schedules:** große Lernraten am Anfang für schnellen Fortschritt, kleinere später für Stabilität; manche Schedules senken die Lernrate erst, wenn der Fortschritt stockt (Abschnitt „Learning-rate schedules").
- **Diagnose aus Loss-Kurven:** Train-Loss fällt und Val-Loss steigt → mögliches Overfitting; Loss springt stark → Lernrate eventuell zu hoch; Loss ändert sich kaum → Lernrate zu klein oder Underfitting (Abschnitt „Comparing training and validation loss").
- Hyperparameter mit dem Validierungsset wählen, das Testset nur für die finale Bewertung (Abschnitt „Test accuracy table") → [[train-val-test-split]].

## Widersprüche / Abweichungen zu anderen Quellen
- In diesem Lauf schlägt SGD mit Momentum alle adaptiven Verfahren, in [[q-abschlussprojekt-abgabe|der eigenen Projektabgabe]] ist es umgekehrt (ungetuntes SGD mit Momentum am schwächsten). Beide sind Einzelläufe mit je einer festen Lernrate; das Notebook selbst warnt vor Verallgemeinerung. Kein Widerspruch zur VL, die nur sagt, dass Adam in manchen Settings schlechter generalisieren kann als SGD (→ [[adaptive-optimierer]]).
- Keiner der drei Schedules schlägt hier die konstante Lernrate; die Unterschiede liegen unter 0,4 Prozentpunkten. Die VL nennt Schedules dagegen als oft hilfreich (→ [[q-vl03-regularisierung-cv|VL3]], F. 41).

> [!ergaenzung] Ergänzung (nicht aus der VL) – warum die Schedules hier nichts bringen
> Bei nur 8 Epochen senken alle drei Schedules die Lernrate, bevor das Modell konvergiert ist: Die Val-Loss fällt bei konstanter Lernrate bis Epoche 7 noch. Ihr Vorteil, das Zittern nahe dem Optimum zu dämpfen, kommt erst bei längerem Training zum Tragen. Mit einem Seed und Abständen unter 0,4 Prozentpunkten ist das Ergebnis ohnehin kaum belastbar.

## Offene Fragen
- Keine.
