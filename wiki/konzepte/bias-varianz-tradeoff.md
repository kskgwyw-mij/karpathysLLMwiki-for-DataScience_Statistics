---
typ: konzept
themen: [bias-varianz, generalisierung]
aliases: [Bias-Variance Trade-Off, Bias-Varianz-Zerlegung, Fehlerzerlegung, Overfitting, Underfitting]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl02-bias-varianz-modellwahl]]", "[[q-vl03-regularisierung-cv]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Bias-Varianz-Trade-off

#flashcards/statistik

## Kurzdefinition
Der erwartete quadratische Fehler eines geschätzten Modells $\hat f$ zerfällt in **Bias²** (falsche Modellannahme), **Varianz** (Überanpassung an die Trainingsdaten) und **irreduziblen Fehler** (Rauschen in den Daten). Gegen den irreduziblen Fehler kann man nichts tun (→ [[q-vl00-essentiell|VL0]], F. 14).

## Intuition
Zwei Ziele stehen gegeneinander (→ VL0, F. 13):
- **Datenfit:** Das Modell soll auf den vorhandenen Trainingsdaten sehr gut funktionieren.
- **Generalisierung:** Die Vorhersagen sollen auch auf ungesehenen Testdaten gut sein.

Beides gleichzeitig perfekt zu erfüllen ist im Allgemeinen nicht möglich. Man denkt sich viele verschiedene Trainingsdatensätze aus derselben Verteilung und trainiert auf jedem ein Modell:
- **Bias** fragt, wie weit das *durchschnittliche* dieser Modelle vom wahren $f$ entfernt ist. Ein zu einfaches Modell (Gerade für eine Parabel) liegt systematisch daneben.
- **Varianz** fragt, wie stark die Modelle *untereinander* schwanken, also wie sensibel $\hat f$ auf die konkreten Trainingsdaten reagiert. Ein zu flexibles Modell folgt jedem Rauschpunkt.

## Formal
Mit $y = f(x) + \varepsilon$, $\mathbb E[\varepsilon] = 0$, $\mathbb E[\varepsilon^2] = \sigma^2$ (→ [[q-vl02-bias-varianz-modellwahl|VL2]], F. 5–7):
$$\mathrm{MSE}(x) = \mathbb E\big[(y - \hat f(x))^2\big] = \underbrace{\big(f(x) - \mathbb E[\hat f(x)]\big)^2}_{\text{Bias}^2} + \underbrace{\mathbb E\Big[\big(\mathbb E[\hat f(x)] - \hat f(x)\big)^2\Big]}_{\text{Varianz}} + \underbrace{\sigma^2}_{\text{irreduzibel}}$$

Der Erwartungswert läuft über verschiedene Trainingsdatensätze (und das Rauschen). Die Herleitung (in der VL „nur für Interessierte") steht in [[bias-varianz-herleitung]].

| Term | Ursache (→ VL0, F. 14) | Symptom | Gegenmittel |
|---|---|---|---|
| Bias² | falsche Modellannahme | Trainings- **und** Testfehler hoch (Underfitting) | flexibleres Modell, bessere Features, weniger Regularisierung |
| Varianz | Überanpassung an Trainingsdaten | Trainingsfehler niedrig, Testfehler hoch (Overfitting) | Regularisierung, mehr Daten, einfacheres Modell, Ensembles |
| $\sigma^2$ | Rauschen in den Daten | Untergrenze für jeden Fehler | keines (nur bessere Messung/Labels) |

## Methoden zur Balance (→ VL0, F. 16)
- [[kreuzvalidierung]] – Komplexität anhand ungesehener Daten wählen
- [[regularisierung]] – Komplexität per Strafterm/Nebenbedingung begrenzen
- Feature-Selektion – Anzahl der Eingaben reduzieren (L1 macht das implizit → [[lasso-regression]])
- Ensemble-Methoden – z. B. [[bagging]] reduziert Varianz durch Mitteln

## Annahmen & Voraussetzungen
- Quadratischer Fehler; Rauschen mit Mittelwert 0, unabhängig von $x$; $f$ deterministisch.

## Beispiel
Polynom-Regression auf verrauschten Daten ([[basisfunktionen-uebung-folie-30]]): Grad 1 → hoher Bias; Grad 4 bei 5 Punkten → Interpolation, Trainingsfehler 0, maximale Varianz. Die Regularisierungsdiagnose der VL übersetzt das direkt (→ VL0, F. 57):
- **Unter-Regularisierung** (zu kleines $\lambda$): hohe Varianz, große Lücke zwischen Train und Val.
- **Über-Regularisierung** (zu großes $\lambda$): hoher Bias, Train und Val beide schlecht.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – Zerlegung mit Zahlen
> Wahrer Wert $f(x_0) = 10$, Rauschen $\sigma^2 = 1$. Auf drei Trainingsdatensätzen liefern zwei Modelle bei $x_0$:
>
> | Modell | $\hat f(x_0)$ auf Datensatz 1, 2, 3 | $\mathbb E[\hat f]$ | Bias² | Varianz | MSE |
> |---|---|---|---|---|---|
> | einfach | 8; 9; 10 | 9 | $(10-9)^2 = 1$ | $\frac{1 + 0 + 1}{3} \approx 0{,}67$ | $1 + 0{,}67 + 1 \approx 2{,}67$ |
> | flexibel | 6; 10; 14 | 10 | 0 | $\frac{16 + 0 + 16}{3} \approx 10{,}67$ | $0 + 10{,}67 + 1 \approx 11{,}67$ |
>
> Das flexible Modell ist im Mittel **unverzerrt**, verliert aber wegen der Schwankung deutlich. Ein bisschen Bias gegen viel weniger Varianz zu tauschen ist genau die Idee von [[regularisierung]].

> [!beispiel] Anwendungsfall (nicht aus der VL) – Nachfrageprognose im Onlinehandel
> Ein Händler prognostiziert die Tagesnachfrage pro Artikel.
> - **Modell A** „Wert der Vorjahreswoche": robust, aber verpasst Trends und Aktionen → Trainings- und Testfehler beide hoch → **Bias**. Gegenmittel: mehr Features (Preis, Aktion, Feiertag), flexibleres Modell.
> - **Modell B** großes Netz auf drei Monaten Daten: Trainingsfehler fast 0, auf der nächsten Woche schlecht → **Varianz**. Gegenmittel: Regularisierung, mehr Historie, einfacheres Modell oder Ensembles ([[bagging]]).
> - Unvorhersehbares (Wetterumschwung, virale Posts) bleibt als $\sigma^2$ – kein Modell beseitigt es.

## Typische Prüfungsfragen & Fallstricke
- Formel der Zerlegung aufschreiben und **jeden Term in Worten** erklären.
- „Ein Modell hat Trainingsfehler 2 % und Testfehler 25 % – Bias- oder Varianzproblem? Was tun?" → Varianz; regularisieren, mehr Daten, einfacheres Modell.
- „Beide Fehler 25 %?" → Bias; flexibleres Modell oder weniger Regularisierung.
- Fallstrick: Mehr Daten senken die **Varianz**, nicht den Bias und nicht $\sigma^2$.
- Fallstrick: Der irreduzible Fehler hängt nicht vom Modell ab.
- Fallstrick: Es heißt **Bias²**, nicht Bias.

## Verwandt
- [[bias-varianz-herleitung]] – Schritt-für-Schritt-Herleitung
- [[regularisierung]] – wichtigstes Werkzeug gegen Varianz
- [[modellauswahl-vorgehen]] – „Fehleranalyse: Bias vs. Varianz" als Schritt nach der Shortlist
- [[bagging]] – Varianzreduktion durch Mitteln
- [[entscheidungsbaeume]] – Beispiel für Schätzer mit hoher Varianz

## Karteikarten
Wie lautet die Bias-Varianz-Zerlegung des MSE?::$\mathrm{MSE}(x) = (f(x) - \mathbb E[\hat f(x)])^2 + \mathbb E[(\mathbb E[\hat f(x)] - \hat f(x))^2] + \sigma^2$.
Welche Ursache hat der Bias-Term laut VL?::Eine falsche Modellannahme.
Welche Ursache hat der Varianz-Term laut VL?::Überanpassung an die Trainingsdaten – Sensitivität bezüglich der Trainingsdaten.
Was ist der irreduzible Fehler?::Das Rauschen $\sigma^2 = \mathbb E[\varepsilon^2]$ in den Daten; dagegen kann man nichts tun.
Nenne vier Methoden zur Balance von Bias und Varianz.::Cross-Validation, Regularisierung, Feature-Selektion, Ensemble-Methoden.
Train-Fehler niedrig, Val-Fehler hoch – Diagnose?::Hohe Varianz / Overfitting / Unter-Regularisierung → $\lambda$ erhöhen, Kapazität senken, Early Stopping aggressiver.
Train- und Val-Fehler beide hoch – Diagnose?::Hoher Bias / Underfitting / Über-Regularisierung → $\lambda$ senken, Kapazität erhöhen.
Welchen Term reduzieren mehr Trainingsdaten?::Die Varianz.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 12–16, 57
- [[q-vl02-bias-varianz-modellwahl|VL2]], Folien 3–8
- [[q-vl03-regularisierung-cv|VL3]], Folie 2
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitte 2.9 (S. 37) und 7.2–7.3 (S. 219–223)
