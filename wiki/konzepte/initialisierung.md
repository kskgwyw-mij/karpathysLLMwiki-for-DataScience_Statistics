---
typ: konzept
themen: [optimierung, neuronale-netze]
aliases: [Initialization, Initialisierung, Xavier, Glorot, He-Initialisierung, Kaiming, Seeds]
quellen: ["[[q-vl04-map-resampling-optimierung]]", "[[q-nb02-mnist-regularization]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Initialisierung

#flashcards/statistik

## Kurzdefinition
Die Wahl des Startpunkts $\theta_0$ einer iterativen Optimierung. Bei **konvexen** Zielen beeinflusst sie nur die Geschwindigkeit, bei **nichtkonvexen** kann sie bestimmen, welches Tal (basin of attraction) erreicht wird (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 65).

## Intuition
In einer Landschaft mit vielen Tälern landet man je nach Startpunkt in einem anderen. Und schlechte Skalierung der Startgewichte lässt Signale durch viele Schichten hindurch explodieren oder verschwinden.

## Warum Initialisierung zählt (→ VL4, F. 65)
- konvex: beeinflusst Geschwindigkeit, nicht das globale Optimum (sofern die Methode konvergiert)
- nichtkonvex: kann das erreichte Einzugsgebiet bestimmen
- schlechte Skalierung → explodierende oder verschwindende Aktivierungen und Gradienten
- **mehrere Random Seeds** sind wichtig für ehrliche empirische Vergleiche

## Strategien (→ VL4, F. 66)
| Kontext | Strategie | Idee |
|---|---|---|
| Lineare Modelle | Nullen oder kleine Zufallswerte | konvexes Ziel → Initialisierung weniger kritisch |
| NN mit tanh | Xavier / Glorot | Aktivierungsvarianz über die Schichten ungefähr stabil halten |
| NN mit ReLU | He / Kaiming | kompensiert, dass ReLU etwa die Hälfte der Aktivierungen auf 0 setzt |
| Transfer Learning | vortrainierte Gewichte | Start von Features, die auf verwandten Daten gelernt wurden |
| Clustering | k-means++ | Startzentren streuen → stabilere Ergebnisse |

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Xavier: $\mathrm{Var}(w) = 2/(n_\text{in} + n_\text{out})$; He: $\mathrm{Var}(w) = 2/n_\text{in}$. In [[q-nb02-mnist-regularization|NB2]] nutzt `ExplicitLinear` `kaiming_uniform_` (He). Ein Netz komplett mit Nullen zu initialisieren ist schlecht: Alle Neuronen einer Schicht bekommen dieselben Gradienten (Symmetrie wird nie gebrochen).

## Beispiel
> [!beispiel] Rechenbeispiel (nicht aus der VL) – Skalen der Startgewichte
> Voraussetzung: standardisierte Inputs ($\mathrm{Var}(x) = 1$). Für $z = \sum_{i=1}^{n_\text{in}} w_ix_i$ gilt näherungsweise $\mathrm{Var}(z) = n_\text{in}\,\mathrm{Var}(w)$.
> - **Naiv** $w \sim \mathcal N(0, 1)$, Schicht 784 → 256: $\mathrm{Var}(z) = 784$, Standardabweichung ≈ 28 – tanh/Sigmoid sind sofort gesättigt, Gradienten ≈ 0.
> - **He** (ReLU): $\mathrm{Var}(w) = 2/784 \approx 0{,}00255$, Std ≈ 0,051; als Gleichverteilung mit Grenze $\sqrt{6/784} \approx 0{,}087$.
> - **Xavier** (tanh), Schicht 256 → 128: $\mathrm{Var}(w) = 2/(256 + 128) \approx 0{,}0052$, Std ≈ 0,072.
>
> Mit diesen Skalen bleibt die Varianz der Aktivierungen über die Schichten ungefähr konstant.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Röntgenbilder mit Transfer Learning
> Ein Krankenhaus hat 2 000 gelabelte Röntgenbilder. Von Null initialisiert überanpasst ein großes CNN sofort. Stattdessen: auf ImageNet vortrainierte Gewichte laden, die frühen Schichten (Kanten, Texturen) einfrieren, die letzte Schicht für die neuen Klassen **neu initialisieren** und nur die oberen Schichten finetunen (VL4, F. 66–67). Für den Vergleich zweier Varianten mehrere Seeds verwenden – Unterschiede von 0,3 Prozentpunkten liegen oft im Seed-Rauschen.

## Checkliste (→ VL4, F. 67)
- Random Seeds setzen und berichten, wenn Reproduzierbarkeit zählt
- mehrere Seeds bei kleinen Datensätzen oder instabilen Modellen
- frühes Training beobachten: explodierender Loss deutet auf zu große Lernrate oder schlechte Skalierung
- Inputs standardisieren – Initialisierungsformeln setzen vernünftige Input-Skalen voraus
- beim Transfer Learning festlegen, welche Schichten eingefroren, neu initialisiert oder finegetuned werden

## Typische Prüfungsfragen & Fallstricke
- „Spielt die Initialisierung bei der linearen Regression eine Rolle?" → nur für die Geschwindigkeit (konvex).
- Xavier vs. He zuordnen (tanh vs. ReLU).
- Fallstrick: Ergebnisse nur mit einem Seed vergleichen.

## Verwandt
- [[optimierung-grundlagen]] – konvex vs. nichtkonvex
- [[gradientenabstieg]], [[sgd]]
- [[train-val-test-split]] – mehrere Seeds mitteln

## Karteikarten
Welche Rolle spielt die Initialisierung bei konvexen Zielen?::Sie beeinflusst nur die Geschwindigkeit, nicht das globale Optimum.
Welche Rolle spielt sie bei nichtkonvexen Zielen?::Sie kann bestimmen, welches Einzugsgebiet bzw. Minimum erreicht wird.
Welche Initialisierung für ReLU-Netze, welche für tanh?::ReLU: He/Kaiming; tanh: Xavier/Glorot.
Warum He-Initialisierung bei ReLU?::Kompensiert, dass ReLU etwa die Hälfte der Aktivierungen auf 0 setzt.
Initialisierungsstrategie für Clustering?::k-means++ – Startzentren streuen.
Was deutet ein explodierender Loss zu Trainingsbeginn an?::Zu große Lernrate oder schlechte Skalierung.

## Quellen
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 64–67
- [[q-nb02-mnist-regularization|NB2]]
