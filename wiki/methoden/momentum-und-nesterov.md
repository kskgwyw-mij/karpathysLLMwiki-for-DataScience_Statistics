---
typ: methode
themen: [optimierung]
aliases: [Momentum, Nesterov, Nesterov Accelerated Gradient, NAG, Heavy Ball]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Momentum und Nesterov-Beschleunigung

#flashcards/statistik

## Kurzdefinition
**Momentum** führt einen Geschwindigkeitsvektor $v$ ein, der die letzten Gradienten mittelt: $v_{t+1} = \beta v_t + \nabla f(\theta_t)$, $\theta_{t+1} = \theta_t - \eta v_{t+1}$. **Nesterov** wertet den Gradienten an einer vorausgeschätzten Position aus (→ [[q-vl00-essentiell|VL0]], F. 81–84).

## Intuition
Eine Kugel, die bergab rollt, behält Schwung. In einem schmalen, langen Tal zeigt der Gradient abwechselnd nach links und rechts (Zickzack) und nur schwach entlang des Tals. Mit Momentum **heben sich die oszillierenden Komponenten auf**, die **konsistenten Komponenten addieren sich** (→ VL0, F. 81). Nesterov schaut „voraus", wohin der Schwung trägt, und korrigiert die Richtung früher.

## Formal

### Momentum (→ VL0, F. 82–83)
$$v_{t+1} = \beta v_t + \nabla f(\theta_t), \qquad \theta_{t+1} = \theta_t - \eta_t v_{t+1}$$
- $\beta$ steuert das **Gedächtnis**; übliche Werte um **0,9**
- reduziert Zickzack in Richtungen mit wechselndem Gradienten
- beschleunigt in Richtungen mit konsistentem Gradienten
- sehr nützlich bei **schlecht konditionierten** Zielen

```
v₀ = 0
für t = 0, 1, 2, …:
    gₜ = ∇f(θₜ)
    vₜ₊₁ = β vₜ + gₜ
    θₜ₊₁ = θₜ − η vₜ₊₁
```
Manche Bibliotheken schreiben $v = \beta v - \eta g$, $\theta = \theta + v$ – algebraisch äquivalent (→ VL0, F. 83).

### Nesterov (→ VL0, F. 84; [[q-vl04-map-resampling-optimierung|VL4]], F. 39)
$$v_{t+1} = \beta v_t + \nabla f(\theta_t - \eta\beta v_t), \qquad \theta_{t+1} = \theta_t - \eta v_{t+1}$$
```
v₀ = 0
für t = 0, 1, 2, …:
    yₜ = θₜ − η β vₜ        # Look-ahead-Punkt
    gₜ = ∇f(yₜ)
    vₜ₊₁ = β vₜ + gₜ
    θₜ₊₁ = θₜ − η vₜ₊₁
```
- Gradient an einer **vorhergesagten zukünftigen Position**
- kann die Richtung früher korrigieren als klassisches Momentum
- für glatte konvexe Probleme können beschleunigte Methoden die Worst-Case-Raten verbessern
- im Deep Learning ist der praktische Unterschied problemabhängig

> [!note] Notation
> VL0, F. 81 schreibt das Momentum-Update ohne Index an $\eta$; F. 82–83 mit $\eta_t$ bzw. $\eta$. Inhaltlich identisch.

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Bei konstantem Gradienten $g$ wächst $v$ auf $g/(1-\beta)$; mit $\beta = 0{,}9$ ist die effektive Schrittweite also bis zu **10-mal** so groß wie ohne Momentum.

## Beispiel
$f(\theta) = \theta^2$, $\theta_0 = 2$, $\eta = 0{,}1$, $\beta = 0{,}9$:

| $t$ | $g_t$ | $v_{t+1}$ | $\theta_{t+1}$ |
|---|---|---|---|
| 0 | 4 | 4 | 1,6 |
| 1 | 3,2 | 6,8 | 0,92 |
| 2 | 1,84 | 7,96 | 0,124 |
| 3 | 0,248 | 7,412 | −0,617 |
Der Schwung trägt über das Minimum hinaus (Überschießen) – auf einem einfachen 1D-Problem ist Momentum nicht schneller, sein Vorteil liegt in schlecht konditionierten, mehrdimensionalen Tälern. Rechenweg: [[gradientenabstieg-von-hand]].

## Typische Prüfungsfragen & Fallstricke
- Momentum-Update aufschreiben und die Rolle von $\beta$ erklären.
- „Warum hilft Momentum in schmalen Tälern?" → oszillierende Komponenten heben sich auf, konsistente verstärken sich.
- Unterschied Momentum vs. Nesterov: Ort der Gradientenauswertung.
- Fallstrick: $v$ mit 0 initialisieren, nicht mit dem ersten Gradienten (beides kommt vor, VL nutzt $v_0 = 0$).

## Verwandt
- [[gradientenabstieg]] – Grundverfahren
- [[adaptive-optimierer]] – Adam kombiniert Momentum mit adaptiver Skalierung
- [[sgd]] – Momentum wird meist mit Minibatches kombiniert
- [[optimierer-uebersicht]]

## Karteikarten
Wie lautet das Momentum-Update?::$v_{t+1}=\beta v_t+\nabla f(\theta_t)$, $\theta_{t+1}=\theta_t-\eta v_{t+1}$.
Typischer Wert für $\beta$ beim Momentum?::Etwa 0,9.
Warum reduziert Momentum Zickzack?::Wechselnde Gradientenkomponenten heben sich in $v$ auf, konsistente addieren sich.
Wie lautet das Nesterov-Update?::$v_{t+1}=\beta v_t+\nabla f(\theta_t-\eta\beta v_t)$, $\theta_{t+1}=\theta_t-\eta v_{t+1}$.
Was ist der Kernunterschied von Nesterov zu Momentum?::Der Gradient wird am Look-ahead-Punkt $\theta_t-\eta\beta v_t$ statt bei $\theta_t$ ausgewertet.
Wofür ist Momentum besonders nützlich?::Für schlecht konditionierte Zielfunktionen (schmale Täler).

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 81–84
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 35–39
