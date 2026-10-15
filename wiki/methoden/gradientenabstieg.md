---
typ: methode
themen: [optimierung]
aliases: [Gradient Descent, GD, Gradientenverfahren, Lernrate, Learning Rate, Schrittweite, Line Search]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Gradientenabstieg

#flashcards/statistik

## Kurzdefinition
Iteratives Verfahren erster Ordnung: in Richtung des **negativen Gradienten** gehen, $\theta_{t+1} = \theta_t - \eta_t\nabla f(\theta_t)$, mit Lernrate bzw. Schrittweite $\eta_t$ (→ [[q-vl00-essentiell|VL0]], F. 78).

## Intuition
Der Gradient zeigt in die Richtung des steilsten Anstiegs; man geht ein Stück in die Gegenrichtung. Die Schrittweite entscheidet: zu groß → man springt über das Tal hinaus oder divergiert; zu klein → man kommt kaum voran.

## Formal
$$\theta_{t+1} = \theta_t - \eta_t\,\nabla f(\theta_t)$$
- großes $\eta$: schnell, kann aber **überschießen oder divergieren**
- kleines $\eta$: stabil, aber **langsam**
- Konvergenz hängt stark von **Krümmung und Konditionierung** ab (→ VL0, F. 78)

**Pseudocode** (→ VL0, F. 80):
```
Input: differenzierbares f, Start θ₀, Schrittweiten ηₜ
für t = 0, 1, 2, … bis Stoppkriterium:
    gₜ = ∇f(θₜ)
    θₜ₊₁ = θₜ − ηₜ gₜ
return θₜ
```
**Stoppkriterien:** kleine Gradientennorm, kleine Änderung des Zielwerts, Validierungsmetrik, Iterationsbudget.

### Schrittweitenstrategien (→ VL0, F. 79)
| Fest oder nach Plan | Line Search |
|---|---|
| konstante Lernrate für einfache Probleme | $\eta$ durch Testen von Kandidatenschritten wählen |
| Decay-Schedules: step, exponential, cosine | Armijo-/Wolfe-Bedingungen balancieren Abstieg und Krümmung |
| Warm-up stabilisiert frühe Iterationen | oft in deterministischer Optimierung; in großen Modellen mit verrauschten Minibatches seltener |

> [!beispiel] Ergänzung (nicht aus der VL) – Schrittweite am Beispiel
> Für $f(\theta) = \theta^2$ ist $\nabla f = 2\theta$, also $\theta_{t+1} = (1 - 2\eta)\theta_t$. Konvergenz genau dann, wenn $\lvert 1 - 2\eta\rvert < 1$, also $0 < \eta < 1$. Allgemein für eine quadratische Funktion mit größtem Hesse-Eigenwert $L$: $0 < \eta < 2/L$. Schlechte Konditionierung (sehr unterschiedliche Eigenwerte) erzwingt ein kleines $\eta$, mit dem flache Richtungen nur langsam vorankommen → Zickzack, siehe [[momentum-und-nesterov]]. Rechenbeispiel: [[gradientenabstieg-von-hand]].

## Annahmen & Voraussetzungen
- $f$ differenzierbar; bei konvexem $f$ und passender Schrittweite Konvergenz zum globalen Minimum ([[optimierung-grundlagen]]).

## Beispiel
$f(\theta) = \theta^2$, $\theta_0 = 2$:

| $\eta$ | $\theta_1, \theta_2, \theta_3, \theta_4$ | Verhalten |
|---|---|---|
| 0,1 | 1,6; 1,28; 1,024; 0,819 | langsam, monoton |
| 0,5 | 0; 0; 0; 0 | ein Schritt (hier optimal) |
| 0,9 | −1,6; 1,28; −1,024; 0,819 | oszilliert, konvergiert |
| 1,1 | −2,4; 2,88; −3,456; 4,147 | **divergiert** |

## Typische Prüfungsfragen & Fallstricke
- Update-Regel aufschreiben und ein paar Schritte von Hand rechnen.
- Effekt großer vs. kleiner Lernrate erklären.
- Vier Stoppkriterien nennen.
- Fallstrick: Vorzeichen – **minus** Gradient beim Minimieren.
- Fallstrick: Gradient der Zielfunktion falsch ableiten (Kettenregel).

## Verwandt
- [[momentum-und-nesterov]] – gegen Zickzack in schmalen Tälern
- [[sgd]] – Minibatch-Gradienten statt vollem Gradienten
- [[adaptive-optimierer]] – Lernrate pro Koordinate
- [[newton-und-quasi-newton]] – Krümmung nutzen
- [[early-stopping]] – Abbruch als Regularisierung
- [[gradientenabstieg-von-hand]] – Rechenübung

## Karteikarten
Wie lautet die Update-Regel des Gradientenabstiegs?::$\theta_{t+1} = \theta_t - \eta_t\nabla f(\theta_t)$.
Was passiert bei zu großer Lernrate?::Überschießen, Oszillation oder Divergenz.
Was passiert bei zu kleiner Lernrate?::Stabil, aber sehr langsame Konvergenz.
Nenne vier Stoppkriterien für GD.::Kleine Gradientennorm, kleine Änderung des Zielwerts, Validierungsmetrik, Iterationsbudget.
Drei Decay-Schedules für die Lernrate?::Step Decay, Exponential Decay, Cosine Decay.
Was ist Line Search?::Schrittweite durch Testen von Kandidaten wählen, z. B. mit Armijo-/Wolfe-Bedingungen.
Für welche $\eta$ konvergiert GD auf $f(\theta)=\theta^2$?::Für $0<\eta<1$, da $\theta_{t+1}=(1-2\eta)\theta_t$.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 77–80
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 31–34
