---
typ: konzept
themen: [optimierung]
aliases: [Optimization, Lernen als Optimierung, Konvexität, Convexity, Minima, Globales Minimum, Lokales Minimum]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Optimierung: Grundlagen und Konvexität

#flashcards/statistik

## Kurzdefinition
Training heißt meist, eine **empirische Zielfunktion** zu minimieren: Loss auf den Daten plus Regularisierungsterm. Entscheidungsvariablen sind die Parameter $\theta$, manchmal auch latente Variablen oder Hyperparameter. Die Wahl des Algorithmus hängt ab von Glattheit, Dimensionalität, Stochastizität, Nebenbedingungen und Krümmung (→ [[q-vl00-essentiell|VL0]], F. 70).

## Intuition
Man stellt sich die Zielfunktion als Landschaft über dem Parameterraum vor und sucht den tiefsten Punkt. Ob das leicht ist, hängt von der Form der Landschaft ab: Eine Schüssel (konvex) hat genau ein Tal; ein Gebirge (nicht konvex, z. B. neuronale Netze) viele.

## Formal
**Grundproblem** (→ VL0, F. 71): $\min f(\theta)$ über $\theta\in\mathbb R^d$.

| Begriff | Definition |
|---|---|
| Globales Minimum | $f(\theta^*) \le f(\theta)$ für alle zulässigen $\theta$ |
| Lokales Minimum | $f(\theta^*) \le f(\theta)$ in einer Umgebung von $\theta^*$ |
| Striktes Minimum | Ungleichung strikt für alle anderen nahen Punkte |
| Flaches Minimum | viele nahe Punkte haben fast denselben Zielwert |

**Typische Fälle** (→ VL0, F. 72–73):

| Fall | Beschreibung | Beispiel |
|---|---|---|
| Kein Minimum | Infimum existiert, wird aber nicht angenommen, oder $f$ ist nach unten unbeschränkt | $f(x) = e^x$: Infimum 0, kein $x$ erreicht es |
| Ein Minimum | genau ein Optimierer | streng konvexe quadratische Schüssel |
| Mehrere Minima | mehrere getrennte Optimierer | Symmetrien in neuronalen Netzen |
| Flacher Bereich | Kontinuum von (Fast-)Minimierern | redundante Parameter, überparametrisierte Modelle |

**Max → Min** (→ VL0, F. 74): $\max g(\theta) \Leftrightarrow \min f(\theta) = -g(\theta)$, $\nabla f = -\nabla g$. Beispiel: Maximum Likelihood ↔ negative Log-Likelihood minimieren ([[probabilistische-sicht-map]]).

### Konvexität (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 29)
$$f(\lambda\alpha + (1-\lambda)\beta) \le \lambda f(\alpha) + (1-\lambda)f(\beta) \qquad \text{für } 0\le\lambda\le 1$$
Geometrisch: Jede Verbindungsstrecke zweier Punkte des Graphen liegt **über** dem Graphen – keine versteckten Täler unter der Sehne.
- **Beispiele:** quadratischer Loss der linearen Regression, Zielfunktion der logistischen Regression mit konvexer Regularisierung
- **Gegenbeispiele:** die meisten Trainingsziele tiefer neuronaler Netze

**Was Konvexität bedeutet** (→ VL0, F. 76):

| Gute Nachrichten | Wichtige Grenzen |
|---|---|
| Jedes lokale Minimum ist global | Konvex heißt nicht leicht, wenn Dimension oder Datenmenge riesig sind |
| Stationärer Punkt $\nabla f(\theta) = 0$ ist hinreichend für globale Optimalität (differenzierbar) | Nicht glatte konvexe Losses brauchen Subgradienten oder proximale Methoden |
| Starke Konvexität → eindeutiger Minimierer, oft schnellere Konvergenz | Flache Richtungen → nicht eindeutige Minimierer |
| Nichtkonvexe Methoden funktionieren empirisch oft sehr gut | Deep-Learning-Ziele sind meist **nicht** konvex |

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Für zweimal differenzierbares $f$: konvex ⇔ Hesse-Matrix überall positiv semidefinit. Beispiel $f(\theta) = \|y - X\theta\|^2$: $\nabla^2 f = 2X^TX \succeq 0$ → konvex; mit Ridge-Term $2(X^TX + \lambda I)\succ 0$ → streng konvex, eindeutiges Minimum.

## Annahmen & Voraussetzungen
- Für die Gradientenverfahren: $f$ (zumindest stückweise) differenzierbar.

## Typische Prüfungsfragen & Fallstricke
- Konvexität definieren (Formel + Geometrie) und zwei Beispiele/Gegenbeispiele nennen.
- „Warum genügt bei der linearen Regression $\nabla L = 0$?" → konvex, stationärer Punkt ist global.
- „Hat $f(x) = e^x$ ein Minimum?" → Nein, nur Infimum 0.
- Fallstrick: Konvex ⇒ eindeutig – nur bei **strenger** Konvexität.

## Verwandt
- [[gradientenabstieg]] – Verfahren erster Ordnung
- [[newton-und-quasi-newton]] – Verfahren zweiter Ordnung
- [[restringierte-optimierung]] – mit Nebenbedingungen
- [[initialisierung]] – entscheidend bei nichtkonvexen Zielen
- [[optimierer-uebersicht]] – alle Verfahren im Vergleich

## Karteikarten
Was bedeutet „Lernen als Optimierung"?::Training minimiert eine empirische Zielfunktion, typischerweise Loss auf den Daten plus Regularisierungsterm.
Wie lautet die Definition von Konvexität?::$f(\lambda\alpha+(1-\lambda)\beta)\le\lambda f(\alpha)+(1-\lambda)f(\beta)$ für $0\le\lambda\le1$.
Geometrische Bedeutung von Konvexität?::Jede Sehne zwischen zwei Punkten liegt über dem Graphen; keine versteckten Täler.
Wichtigste Folge der Konvexität für die Minimierung?::Jedes lokale Minimum ist global; $\nabla f = 0$ ist hinreichend.
Was garantiert starke Konvexität zusätzlich?::Einen eindeutigen Minimierer und oft schnellere Konvergenz.
Beispiel für eine Funktion ohne Minimum?::$f(x)=e^x$ – Infimum 0 wird nicht angenommen.
Woher kommen mehrere Minima bei neuronalen Netzen?::Aus Symmetrien (z. B. vertauschbare Neuronen).
Wie wandelt man eine Maximierung in eine Minimierung um?::$\max g \Leftrightarrow \min -g$; Gradient wechselt das Vorzeichen.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 69–76
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 23–30
