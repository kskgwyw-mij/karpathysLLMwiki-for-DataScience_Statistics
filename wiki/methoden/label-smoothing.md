---
typ: methode
themen: [regularisierung, klassifikation]
aliases: [Label Smoothing, weiche Labels]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-nb02-mnist-regularization]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Label Smoothing

#flashcards/statistik

## Kurzdefinition
Regularisierung für Klassifikationsmodelle, die harte One-Hot-Labels durch **„weiche" Zielverteilungen** ersetzt und dadurch die **Überkonfidenz** des Modells reduziert (→ [[q-vl00-essentiell|VL0]], F. 52).

## Intuition
Mit harten Labels (1 für die richtige Klasse, 0 sonst) lohnt es sich für das Modell, die Logits immer weiter zu vergrößern – es wird beliebig sicher. Weiche Ziele wie 0,9 statt 1 begrenzen das. Das hilft besonders, wenn manche Labels falsch sind.

## Formal
Für $\varepsilon\in[0,1]$ (→ VL0, F. 52):
$$y_c = (1-\varepsilon)\cdot y + \varepsilon\cdot(1-y)$$
Für ein binäres Label wird aus $y = 1$ der Wert $1-\varepsilon$ und aus $y = 0$ der Wert $\varepsilon$. Es existieren mehrere Varianten.

> [!note] Notation
> Gebräuchlich für $K$ Klassen (z. B. PyTorch `CrossEntropyLoss(label_smoothing=ε)`): $y_c = (1-\varepsilon)\,y_c + \varepsilon/K$. Für $K = 2$ ergibt das $1 - \varepsilon/2$ bzw. $\varepsilon/2$ – also dieselbe Idee mit anderer Skalierung von $\varepsilon$. In der Klausur die Folienformel verwenden.

## Wirkung und Praxis (→ [[q-vl03-regularisierung-cv|VL3]], F. 32)
| Wirkung | Praxis | Fallstricke |
|---|---|---|
| weniger überkonfidente Vorhersagen | typisches $\varepsilon$ klein | zu großes $\varepsilon$ → untertrainierte, zu glatte Entscheidungen |
| stabilisiert Training bei Label-Noise | **nur auf Trainingslabels**, nicht auf Val/Test | bei stark unbalancierten Klassen nicht gut |
| leicht geringere Spitzen-Accuracy | nicht mit Maßnahmen gegen Klassenimbalance verwechseln | |

## Beispiel
[[q-nb02-mnist-regularization|NB2]] mit $\varepsilon = 0{,}1$: **höchste** Val-Loss (0,770 gegenüber 0,305 Baseline), aber **beste** Test-Accuracy (0,934). Der Loss-Wert ist nicht vergleichbar, weil die Zielverteilung eine andere ist – selbst ein perfektes Modell erreicht mit weichen Zielen keine Cross-Entropy nahe 0.

> [!warning] Hinweis zu den Folien
> Auf VL0 F. 52 und VL3 F. 31 steht im unteren Teil der Text der Dropout-Folie („Das Weglassen der Bestandteile …"). Er gehört zu [[dropout]].

## Typische Prüfungsfragen & Fallstricke
- „Welches Problem löst Label Smoothing?" → Überkonfidenz, Label-Noise.
- „Darf man Label Smoothing auf das Test-Set anwenden?" → Nein, nur auf Trainingslabels.
- Fallstrick: Losses zwischen Konfigurationen mit und ohne Label Smoothing vergleichen.

## Verwandt
- [[regularisierung]] – Taxonomie
- [[output-regularisierung]] – verwandt: Eigenschaften der Zielgröße steuern
- [[dropout]] – andere Regularisierung für Netze
- [[fairness-metriken]] – Kalibrierung von Scores

## Karteikarten
Welches Problem adressiert Label Smoothing?::Überkonfidenz des Modells; stabilisiert zudem bei Label-Noise.
Wie lautet die Label-Smoothing-Formel der VL?::$y_c = (1-\varepsilon)y + \varepsilon(1-y)$ für $\varepsilon\in[0,1]$.
Wird Label Smoothing auf Val/Test angewendet?::Nein, nur auf die Trainingslabels.
Was passiert bei zu großem $\varepsilon$?::Untertrainierte, zu glatte Entscheidungen.
Warum ist die Val-Loss mit Label Smoothing nicht mit der Baseline vergleichbar?::Die Zielverteilung und damit die Loss-Funktion sind anders; die minimal erreichbare Loss liegt höher.

## Quellen
- [[q-vl00-essentiell|VL0]], Folie 52
- [[q-vl03-regularisierung-cv|VL3]], Folien 31–32
- [[q-nb02-mnist-regularization|NB2]]
