---
typ: methode
themen: [optimierung]
aliases: [Newton-Verfahren, Newton Method, Quasi-Newton, BFGS, L-BFGS, Hesse-Matrix, Second-order methods, Verfahren zweiter Ordnung]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-lab02-pytorch-optimizer-comparison]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-16
---

# Newton- und Quasi-Newton-Verfahren

#flashcards/statistik

## Kurzdefinition
Verfahren **zweiter Ordnung** nutzen Krümmungsinformation: Newton verwendet eine quadratische Approximation um den aktuellen Punkt und die **Hesse-Matrix** $H$ der zweiten Ableitungen. **Quasi-Newton**-Verfahren (BFGS, L-BFGS) approximieren die inverse Hesse-Matrix aus Gradientendifferenzen (→ [[q-vl00-essentiell|VL0]], F. 91, 93).

## Intuition
Der Gradient sagt nur, wo es bergab geht, nicht, wie weit. Die Krümmung sagt, wie schnell sich der Gradient ändert: In flachen Richtungen kann man lange, zuversichtliche Schritte machen, in stark gekrümmten muss man vorsichtig sein. Newton skaliert jede Richtung passend – für eine quadratische Funktion landet es in **einem** Schritt im Minimum.

## Formal

### Newton
$$\theta_{t+1} = \theta_t - H(\theta_t)^{-1}\nabla f(\theta_t)$$

> [!warning] Fehler auf den Folien
> VL0, F. 91 und VL4, F. 55 schreiben $\theta_{t+1} = -H(\theta_t)^{-1}\nabla f(\theta_t)$ – der Summand $\theta_t$ fehlt. Korrekt ist die Formel oben, wie im Pseudocode VL4, F. 57 ($\theta_{t+1} = \theta_t + \alpha_t p_t$ mit $H_tp_t = -g_t$).

- kann **sehr schnell** konvergieren nahe einem gutartigen Optimum
- **teuer** in hohen Dimensionen: $H$ speichern und invertieren ist kostspielig
- braucht ggf. **Dämpfung oder Line Search**, wenn $H$ indefinit oder der Schritt zu aggressiv ist (→ VL0, F. 91)

**Gedämpftes Newton** (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 57):
```
für t = 0, 1, 2, …:
    gₜ = ∇f(θₜ);  Hₜ = ∇²f(θₜ)
    löse Hₜ pₜ = −gₜ  nach Suchrichtung pₜ
    wähle αₜ per Line Search
    θₜ₊₁ = θₜ + αₜ pₜ
```
Dämpfung oder Trust Regions verbessern die Robustheit weit weg vom Optimum.

### Vor- und Nachteile zweiter Ordnung (→ VL0, F. 92)
| Vorteile | Herausforderungen |
|---|---|
| berücksichtigt Krümmung und Parameterskalierung | Berechnung und Faktorisierung von $H$ können die Laufzeit dominieren |
| lange, zuversichtliche Schritte in Richtungen geringer Krümmung | Dämpfung/Trust Regions fern vom Optimum nötig |
| oft weniger Iterationen als erste Ordnung | stochastische Varianten schwieriger, da Krümmungsschätzungen verrauscht |

### Quasi-Newton (→ VL0, F. 93; VL4, F. 59–60)
- vermeidet die exakte Hesse-Matrix
- baut eine Approximation $B_t\approx H^{-1}$ aus **Gradientendifferenzen** auf
- **BFGS:** weit verbreitet für mittelgroße Probleme
- **L-BFGS:** speichert nur eine begrenzte Historie von Update-Paaren → geeignet für größere Modelle, glatte deterministische Ziele mit vielen Variablen

```
BFGS: B₀ = I
für t = 0, 1, 2, …:
    gₜ = ∇f(θₜ);  pₜ = −Bₜ gₜ;  αₜ per Line Search
    θₜ₊₁ = θₜ + αₜ pₜ
    sₜ = θₜ₊₁ − θₜ;  yₜ = ∇f(θₜ₊₁) − gₜ
    Bₜ₊₁ aus sₜ, yₜ aktualisieren
```
Das BFGS-Update ist so konstruiert, dass es unter Standard-Krümmungsbedingungen **positive Definitheit erhält**. L-BFGS berechnet $p_t$ per „two-loop recursion" aus den letzten $m$ Paaren $(s_t, y_t)$.

**L-BFGS in PyTorch** (→ [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitte „The optimizers we compare", „Training function"): `torch.optim.LBFGS` nähert Krümmungsinformation an und funktioniert eher bei kleineren, glatten Problemen. Weil es pro Update Loss und Gradienten **mehrfach** auswerten kann, braucht `optimizer.step(closure)` eine **Closure**, die Gradienten zurücksetzt, den Loss neu berechnet und `backward()` aufruft ([[trainingsschleife]]). SGD und Adam brauchen das nicht, sie verwenden genau einen Gradienten pro Schritt.

### Erste vs. zweite Ordnung (→ VL0, F. 94)
| Erste Ordnung | Zweite Ordnung / Quasi-Newton |
|---|---|
| billige Iterationen; skaliert auf riesige Daten und Parameterzahlen | krümmungsbewusster; weniger Iterationen möglich |
| natürlich für Minibatches und GPUs | höhere Kosten pro Iteration, mehr Speicher |
| braucht Lernraten-Tuning und oft viele Iterationen | attraktiv für kleinere glatte Probleme oder „final polishing" |

## Beispiel
**L-BFGS auf Minibatches** (→ [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitte „Optimizer experiments", „Test accuracy table"): Auf MNIST mit Minibatches von 128 Bildern und Lernrate 0,8 ist L-BFGS mit Abstand am schlechtesten (Test-Acc 0,708 gegenüber 0,935 für SGD mit Momentum). Der Trainings-Loss springt in Epoche 3 auf 3 968, die Val-Accuracy schwankt zwischen 0,40 und 0,90. Das Notebook nimmt L-BFGS „vor allem zum Vergleich und zur Diskussion" auf: Es ist eher für kleinere deterministische Probleme gedacht als für Deep Learning mit Minibatches. Das bestätigt die VL-Aussage, dass stochastische Varianten zweiter Ordnung schwierig sind, weil die Krümmungsschätzungen verrauscht sind.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – quadratische Funktion in einem Schritt
> $f(\theta) = (\theta_1 - 1)^2 + 10(\theta_2 + 2)^2$, Start $(0,0)$: $\nabla f = (-2;\ 40)$, $H = \mathrm{diag}(2, 20)$, $\theta_1 = (0,0) - H^{-1}\nabla f = (1;\,-2)$ – das exakte Minimum nach **einem** Schritt. GD mit $\eta = 0{,}05$ braucht für $\theta_1$ dagegen viele Schritte (Faktor 0,9 pro Schritt). Rechenweg: [[gradientenabstieg-von-hand]].

> [!beispiel] Rechenbeispiel (nicht aus der VL) – quadratische Konvergenz
> $f(\theta) = e^\theta - 2\theta$, Minimum bei $\ln 2 \approx 0{,}693147$. Mit $f'(\theta) = e^\theta - 2$, $f''(\theta) = e^\theta$ lautet der Newton-Schritt $\theta_{t+1} = \theta_t - (e^{\theta_t} - 2)/e^{\theta_t}$:
>
> | $t$ | 0 | 1 | 2 | 3 | 4 |
> |---|---|---|---|---|---|
> | $\theta_t$ | 0 | 1 | 0,735759 | 0,694042 | 0,693148 |
> | Fehler | 0,69 | 0,31 | 0,043 | 0,0009 | $4\cdot10^{-7}$ |
>
> Nahe am Optimum **verdoppelt** sich die Zahl der korrekten Stellen pro Schritt ungefähr. Bei einer nicht quadratischen Funktion braucht Newton also mehrere Schritte, aber sehr wenige.

> [!beispiel] Anwendungsfall (nicht aus der VL) – logistische Regression in Statistik-Software
> Für logistische Regression mit einigen Dutzend Merkmalen ist die Hesse-Matrix klein. R (`glm`) fittet sie mit IRLS, einer Newton-Variante, und ist meist nach 4–8 Iterationen fertig. scikit-learn nutzt standardmäßig L-BFGS (Quasi-Newton). Für ein neuronales Netz mit Millionen Parametern wäre $H$ nicht einmal speicherbar – dort bleibt man bei Verfahren erster Ordnung (VL0, F. 94).

## Typische Prüfungsfragen & Fallstricke
- Newton-Update aufschreiben (mit $\theta_t$!).
- „Warum nutzt man im Deep Learning kaum Newton?" → $H$ hat $d^2$ Einträge, Invertieren $O(d^3)$, Krümmung im Minibatch verrauscht.
- Unterschied BFGS vs. L-BFGS.
- Fallstrick: Bei indefinitem $H$ zeigt der Newton-Schritt nicht unbedingt bergab → Dämpfung/Line Search.

## Verwandt
- [[gradientenabstieg]] – erste Ordnung
- [[adaptive-optimierer]] – diagonale Vorkonditionierung als „billiger" Ersatz
- [[optimierung-grundlagen]] – Konvexität, Hesse-Matrix
- [[optimierer-uebersicht]]

## Karteikarten
Wie lautet das Newton-Update?::$\theta_{t+1}=\theta_t-H(\theta_t)^{-1}\nabla f(\theta_t)$.
Welcher Fehler steckt in der Newton-Formel auf den Folien?::Es fehlt der Summand $\theta_t$; die Folie schreibt $\theta_{t+1}=-H^{-1}\nabla f$.
Wie viele Newton-Schritte braucht eine quadratische Funktion?::Einen (bei positiv definiter Hesse-Matrix).
Hauptnachteil von Newton in hohen Dimensionen?::Speichern und Invertieren der Hesse-Matrix ist teuer.
Wie approximieren Quasi-Newton-Verfahren die Hesse-Matrix?::Die inverse Hesse-Matrix aus Gradientendifferenzen $y_t$ und Schritten $s_t$.
Unterschied BFGS und L-BFGS?::L-BFGS speichert nur eine begrenzte Historie der letzten $m$ Update-Paare und eignet sich für größere Modelle.
Wann sind Verfahren zweiter Ordnung attraktiv?::Für kleinere glatte Probleme oder zum „final polishing".
Warum sind stochastische Varianten zweiter Ordnung schwierig?::Krümmungsschätzungen aus Minibatches sind verrauscht.
Warum braucht L-BFGS in PyTorch eine Closure?::Es kann pro Update Loss und Gradient mehrfach auswerten; die Closure berechnet beides neu.
Wie schnitt L-BFGS in Lab 2 auf MNIST-Minibatches ab?::Am schlechtesten (Test-Acc 0,71), mit Loss-Sprüngen; L-BFGS passt eher zu kleinen, glatten, deterministischen Problemen.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 91–94
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 54–61
- [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitte „The optimizers we compare", „Training function", „Test accuracy table"
