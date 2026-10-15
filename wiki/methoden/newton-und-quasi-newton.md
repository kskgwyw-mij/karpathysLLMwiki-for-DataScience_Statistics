---
typ: methode
themen: [optimierung]
aliases: [Newton-Verfahren, Newton Method, Quasi-Newton, BFGS, L-BFGS, Hesse-Matrix, Second-order methods, Verfahren zweiter Ordnung]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
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

### Erste vs. zweite Ordnung (→ VL0, F. 94)
| Erste Ordnung | Zweite Ordnung / Quasi-Newton |
|---|---|
| billige Iterationen; skaliert auf riesige Daten und Parameterzahlen | krümmungsbewusster; weniger Iterationen möglich |
| natürlich für Minibatches und GPUs | höhere Kosten pro Iteration, mehr Speicher |
| braucht Lernraten-Tuning und oft viele Iterationen | attraktiv für kleinere glatte Probleme oder „final polishing" |

## Beispiel
$f(\theta) = (\theta_1 - 1)^2 + 10(\theta_2 + 2)^2$, Start $(0,0)$: $\nabla f = (-2;\ 40)$, $H = \mathrm{diag}(2, 20)$, $\theta_1 = (0,0) - H^{-1}\nabla f = (1;\,-2)$ – das exakte Minimum nach **einem** Schritt. GD mit $\eta = 0{,}05$ braucht für $\theta_1$ dagegen viele Schritte (Faktor 0,9 pro Schritt). Rechenweg: [[gradientenabstieg-von-hand]].

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

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 91–94
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 54–61
