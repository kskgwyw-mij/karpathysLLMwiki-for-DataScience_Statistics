---
typ: konzept
themen: [glm, grundlagen]
aliases: [Statistical Learning Problem, Lernproblem, Vorhersage vs. Inferenz]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl01-glm-basisfunktionen-splines]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Statistisches Lernproblem

#flashcards/statistik

## Kurzdefinition
Man beobachtet eine Zielgröße $Y$ und Merkmale $x = (x_1, \dots, x_p)$ und nimmt einen Zusammenhang $Y = f(x) + \varepsilon$ an. Statistisches Lernen heißt, die unbekannte Funktion $f$ aus Daten zu **schätzen** (→ [[q-vl00-essentiell|VL0]], F. 4–5).

## Intuition
$f$ ist der **systematische** Anteil – die Information, die $x$ über $Y$ trägt. $\varepsilon$ ist alles, was $x$ nicht erklärt: Messrauschen, nicht erhobene Einflüsse. Selbst das wahre $f$ kann $Y$ nicht perfekt vorhersagen; dieser Rest ist der **irreduzible Fehler** in [[bias-varianz-tradeoff]].

## Formal
$$Y = f(x) + \varepsilon, \qquad \mathbb E[\varepsilon] = 0, \quad \varepsilon \text{ unabhängig von } x$$

Für eine Stichprobe $(x_1, y_1), \dots, (x_K, y_K)$ soll $y_j \approx f(x_j)$ für alle Beobachtungen gelten (→ [[q-vl01-glm-basisfunktionen-splines|VL1]], F. 18).

> [!note] Notation
> In der Vorlesung bezeichnet ein **Index unten** die Beobachtung ($x_j$ = $j$-te Stichprobe), ein **Index oben** die Komponente ($x_j^i$ = $i$-tes Merkmal der $j$-ten Beobachtung) – keine Potenz (→ VL1, F. 18–19).

## Zwei Gründe, $f$ zu schätzen
| Ziel | Frage | Anforderung |
|---|---|---|
| **Vorhersage** (prediction) | Welches $y$ erwarte ich für ein neues $x$? | $\hat f$ darf eine Blackbox sein, solange sie gut generalisiert |
| **Inferenz** (inference) | Wie verändert sich $y$, wenn sich $x$ ändert? | $\hat f$ muss interpretierbar sein |

(→ VL0, F. 5)

Typische Modelle für $f(x) \approx y$: lineare Regression, logistische Regression (Klassen), neuronale Netze – allgemein überwachtes Lernen (→ VL0, F. 6).

## Annahmen & Voraussetzungen
- $\varepsilon$ hat Mittelwert 0 und ist unabhängig von $x$ – genau diese Annahme lässt in der Bias-Varianz-Herleitung den Mischterm verschwinden ([[bias-varianz-herleitung]]).

## Beispiel
> [!beispiel] Rechenbeispiel (nicht aus der VL) – reduzierbarer und irreduzibler Fehler
> Wahres Modell $f(x) = 2 + 3x$, Rauschen mit $\mathrm{Var}(\varepsilon) = 1$. Ein geschätztes Modell sei $\hat f(x) = 1 + 3{,}5x$. Für $x = 1$:
> - wahrer Erwartungswert $f(1) = 5$, Vorhersage $\hat f(1) = 4{,}5$
> - erwarteter quadratischer Fehler $\mathbb E\big[(Y - \hat f(1))^2\big] = \underbrace{(5 - 4{,}5)^2}_{0{,}25\ \text{reduzierbar}} + \underbrace{\mathrm{Var}(\varepsilon)}_{1\ \text{irreduzibel}} = 1{,}25$
>
> Selbst mit dem wahren $f$ bliebe der Fehler 1 – mehr Daten oder ein besseres Modell können nur die 0,25 verkleinern. Die Zerlegung folgt aus $\mathbb E[\varepsilon] = 0$ und der Unabhängigkeit; sie ist der Spezialfall von [[bias-varianz-tradeoff]] mit festem $\hat f$.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Wohnungspreise
> $Y$ = Kaufpreis, $x$ = (Fläche, Lage, Baujahr, Balkon ja/nein).
> - **Vorhersage:** Ein Portal will für eine neue Anzeige einen fairen Preis vorschlagen. Ob das Modell eine Blackbox ist, ist egal – Hauptsache, es trifft neue Wohnungen gut.
> - **Inferenz:** Eine Maklerin fragt: „Um wie viel steigt der Preis durch einen Balkon, bei sonst gleicher Wohnung?" Dafür braucht man ein interpretierbares $\hat f$, z. B. ein lineares Modell mit Koeffizient für „Balkon".
> - **$\varepsilon$:** Verhandlungsgeschick, persönlicher Geschmack, Zeitdruck beim Verkauf – Einflüsse, die in $x$ nicht stehen. Sie begrenzen jede Vorhersage nach unten.

## Typische Prüfungsfragen & Fallstricke
- „Erklären Sie den Unterschied zwischen Vorhersage und Inferenz." → Tabelle oben; Inferenz verlangt Interpretierbarkeit (Bezug zu [[transparente-modelle]]).
- Fallstrick: $\varepsilon$ ist **nicht** der Trainingsfehler eines Modells, sondern eine Eigenschaft der Daten.

## Verwandt
- [[lineare-regression]] – einfachste Wahl für $f$
- [[basisfunktionen-regression]] – nichtlineares $f$, aber linear in den Koeffizienten
- [[bias-varianz-tradeoff]] – zerlegt den Fehler von $\hat f$
- [[modellauswahl-vorgehen]] – welche Modellklasse für $f$?

## Karteikarten
Wie lautet das allgemeine Modell des statistischen Lernens?::$Y = f(x) + \varepsilon$ mit unbekanntem $f$ und Fehler $\varepsilon$, $\mathbb E[\varepsilon]=0$, unabhängig von $x$.
Welche zwei Gründe gibt es, $f$ zu schätzen?::Vorhersage (prediction) und Inferenz (wie ändert sich $y$ mit $x$).
Was bedeutet $x_j^i$ in der Vorlesungsnotation?::Die $i$-te Komponente (Merkmal) der $j$-ten Beobachtung – keine Potenz.
Was repräsentiert $f$ im Modell $Y=f(x)+\varepsilon$?::Die systematische Information, die $x$ über $Y$ liefert.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 4–6
- [[q-vl01-glm-basisfunktionen-splines|VL1]], Folien 15–18
