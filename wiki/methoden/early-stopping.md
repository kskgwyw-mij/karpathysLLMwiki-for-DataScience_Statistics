---
typ: methode
themen: [regularisierung, optimierung]
aliases: [Early Stopping, Frühzeitiges Stoppen, Patience]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-nb02-mnist-regularization]]", "[[q-nb03-regularization-simple]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Early Stopping

#flashcards/statistik

## Kurzdefinition
Regularisierung, bei der die Optimierung der **Standard-Lossfunktion frühzeitig abgebrochen** wird, sobald die Performance auf dem Validierungsdatensatz nachlässt. Man findet also bewusst **nicht** den eigentlichen Minimierer $\theta^*$ (→ [[q-vl00-essentiell|VL0]], F. 42, 50).

## Intuition
Iterative Verfahren lernen erst die groben Strukturen und passen sich später zunehmend an das Rauschen an. Der Trainingsfehler sinkt immer weiter, der Validierungsfehler sinkt zuerst und steigt dann wieder. Am Wendepunkt hat das Modell eine kleine Varianz, und man hat **Evidenz**, dass es gut generalisiert; weiteres Training würde zu Overfitting führen (→ VL0, F. 50). „Sehr weit verbreitet und oft irgendwie unerkannt" (→ VL0, F. 42).

## Formal
Nur möglich, wenn die Parameter mit einem **iterativen Verfahren** optimiert werden (→ VL0, F. 49). Typische Umsetzung mit Patience (aus [[q-nb02-mnist-regularization|NB2]]):
```
best_val = ∞; wait = 0
für jede Epoche:
    trainieren; val_loss berechnen
    falls val_loss < best_val − 1e-4:  best_val, best_state merken; wait = 0
    sonst: wait += 1; falls wait ≥ patience: stoppen
am Ende best_state laden
```

> [!tip] Merke
> Für ein **simples lineares Modell** ist Early Stopping des Gradientenabstiegs **äquivalent zur L2-Regularisierung** (→ VL0, F. 50).

> [!ergaenzung] Ergänzung (nicht aus der VL) – Intuition der Äquivalenz
> Startet GD bei $\theta_0 = 0$, wachsen die Komponenten in Richtungen großer Krümmung (große Eigenwerte von $X^TX$) schnell, in flachen Richtungen langsam. Nach $t$ Schritten sind die flachen Richtungen noch nahe 0 – genau die Richtungen, die Ridge am stärksten schrumpft. Grob entspricht $1/(\eta t)$ einem $\lambda$.

## Annahmen & Voraussetzungen
- Separater Validierungsdatensatz ([[train-val-test-split]]).
- Iteratives Training (GD, SGD, Adam …).

## Beispiel
Richtwert **Patience 5–10** (→ [[q-vl03-regularisierung-cv|VL3]], F. 41). In NB2 Patience 3 bei nur 8 Epochen. In [[q-nb03-regularization-simple|NB3]] ist Early Stopping durch einen Bug (`'use_early_stopping'==True`) nie aktiv.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – Patience Schritt für Schritt
> Patience 3, Mindestverbesserung $10^{-4}$, Val-Loss je Epoche:
>
> | Epoche | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
> |---|---|---|---|---|---|---|---|---|---|
> | Val-Loss | 0,90 | 0,70 | 0,62 | 0,60 | 0,61 | **0,59** | 0,63 | 0,64 | 0,66 |
> | `wait` | 0 | 0 | 0 | 0 | 1 | 0 | 1 | 2 | 3 → Stopp |
>
> Der kleine Anstieg in Epoche 5 löst noch keinen Abbruch aus – genau dafür gibt es Patience. Nach Epoche 9 wird gestoppt und `best_state` aus **Epoche 6** geladen, nicht der letzte Stand.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Fine-Tuning eines Textklassifikators
> Ein vortrainiertes Sprachmodell wird auf 3 000 Kundenmails feingetunt. Nach jeder Epoche wird der Val-Loss geprüft; ab Epoche 3 steigt er, obwohl der Trainings-Loss weiter fällt. Man nimmt den Checkpoint von Epoche 3 und spart zusätzlich Rechenzeit. Weil das Val-Set jetzt zum Stoppen benutzt wurde, ist sein Wert optimistisch – die ehrliche Zahl kommt vom separaten Test-Set ([[train-val-test-split]]).

## Typische Prüfungsfragen & Fallstricke
- „Warum ist Early Stopping eine Regularisierung?" → Begrenzt die effektive Komplexität, Varianz bleibt klein; im linearen Fall äquivalent zu L2.
- Validierungs-Loss überwachen, **nicht nur** Trainings-Loss (→ VL0, F. 87).
- Fallstrick: das Modell der **letzten** statt der **besten** Epoche verwenden.
- Fallstrick: mit dem Test-Set stoppen → Test-Fehler wird optimistisch.

## Verwandt
- [[regularisierung]] – Taxonomie
- [[ridge-regression]] – Äquivalenz im linearen Fall
- [[sgd]] – Early Stopping als Praxiswahl
- [[train-val-test-split]] – Val-Set als Entscheidungsgrundlage

## Karteikarten
Was ist Early Stopping?::Die Optimierung der Standard-Loss wird abgebrochen, wenn die Validierungs-Performance nachlässt; man findet nicht den eigentlichen Minimierer.
Wann ist Early Stopping überhaupt möglich?::Wenn die Parameter iterativ optimiert werden.
Zu welcher Regularisierung ist Early Stopping beim linearen Modell äquivalent?::Zur L2-Regularisierung.
Welche Daten entscheiden über den Abbruch?::Die Validierungsdaten.
Richtwert für die Patience?::5–10 Epochen.
Welches Modell nimmt man nach Early Stopping?::Das der besten Validierungsepoche (best_state), nicht das der letzten.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 42, 49–50, 87
- [[q-vl03-regularisierung-cv|VL3]], Folien 9, 26–27, 41
- [[q-nb02-mnist-regularization|NB2]], [[q-nb03-regularization-simple|NB3]]
