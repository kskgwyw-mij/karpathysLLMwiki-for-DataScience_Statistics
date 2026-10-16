---
typ: methode
themen: [regularisierung, neuronale-netze]
aliases: [Dropout, Dropout-Rate, MC-Dropout]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-nb02-mnist-regularization]]", "[[q-lab01-pytorch-introduction]]", "[[q-abschlussprojekt-abgabe]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-16
---

# Dropout

#flashcards/statistik

## Kurzdefinition
Beim Dropout werden Bestandteile des Modells (z. B. Neuronen) während der Optimierung **zufällig weggenommen**. Bei der Auswertung des trainierten Modells werden **alle** Bestandteile verwendet (→ [[q-vl00-essentiell|VL0]], F. 51).

## Intuition
Kein Neuron kann sich darauf verlassen, dass ein bestimmtes anderes Neuron da ist – die Repräsentation wird redundanter und robuster. Dropout lässt sich als **Ensemble** vieler Teilmodelle sehen, die sich die meisten Parameter teilen (→ VL0, F. 51), ähnlich zu [[bagging]].

## Formal
- Training: jedes Element wird mit Wahrscheinlichkeit $p$ (Dropout-Rate) auf 0 gesetzt.
- Inferenz: alle Elemente aktiv. In PyTorch über den Modus: `model.train()` → `model.eval()` (die Folie schreibt `model.test`).
- Spezialfall: Dropout **auch in der Inferenz**, um eine **Konfidenz** der Vorhersage zu schätzen (→ VL0, F. 51).

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Damit die erwartete Aktivierung gleich bleibt, skalieren Implementierungen im Training die verbleibenden Werte mit $1/(1-p)$ („inverted dropout"). Wiederholte stochastische Vorhersagen mit aktivem Dropout heißen MC-Dropout.

## Praxisempfehlungen (→ [[q-vl03-regularisierung-cv|VL3]], F. 30)
| | |
|---|---|
| Fully-connected | Rate 0,2–0,5 |
| Convolutional | eher kleiner, 0,1–0,3 |
| Kombination | mit L2-Regularisierung |
| Höhere Rate | stärkere Regularisierung |
| Tiefere Schichten | eventuell kleinere Werte |
| Kleine Datensätze | nicht zu hoch |

**Fallstricke:** zu hohe Rate → Lernverlangsamung, hoher Bias, degradierte Repräsentationen; in kleinen Netzen kann L2- oder Design-Regularisierung effektiver sein als starkes Dropout (→ VL3, F. 30).

## Beispiel
[[q-nb02-mnist-regularization|NB2]], Dropout 0,5 im kleinen MLP (64 Hidden): Test-Accuracy 0,921 gegenüber 0,924 der Baseline – bei kleinem Netz und nur 8 Epochen bringt starkes Dropout nichts, passend zum Fallstrick oben.

[[q-lab01-pytorch-introduction|Lab 1]], gleiches Netz, Dropout 0,5 nach der Hidden-Schicht, 20 Epochen: Test-Acc 0,930 gegenüber 0,928 der Baseline. Der Trainings-Loss bleibt mit 0,24 viermal so hoch wie bei der Baseline (0,06), die Val-Loss ist ähnlich (0,27 vs. 0,28). Dropout ist nur im Training aktiv; `model.train()` und `model.eval()` schalten um (Abschnitt „Dropout"; [[trainingsschleife]]).

> [!beispiel] Anwendungsfall (nicht aus der VL) – eigenes Abschlussprojekt
> Im [[q-abschlussprojekt-abgabe|eigenen Fashion-MNIST-Projekt]] (CNN, Dropout $p = 0{,}3$ im FC-Head, im Rahmen der VL-Empfehlung 0,2–0,5 für Fully-connected) ist Dropout das zweitbeste Setup: +0,7 Prozentpunkte Val-Accuracy gegenüber keiner Regularisierung im Mittel über vier Optimierer. Mit RMSprop erreicht es 0,9312, nur 0,03 Prozentpunkte hinter der besten Zelle.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – inverted Dropout mit $p = 0{,}5$
> Aktivierungen $h = (2, 4, 1, 3)$, zufällige Maske $m = (1, 0, 1, 0)$:
> - Training: $h \odot m / (1 - p) = (2, 0, 1, 0)/0{,}5 = (4, 0, 2, 0)$
> - Erwartungswert je Element: $(1-p)\cdot h_i/(1-p) = h_i$ – im Mittel unverändert.
> - Inferenz (`model.eval()`): alle aktiv, $h = (2, 4, 1, 3)$ ohne Skalierung.
>
> Bei 64 Hidden-Neuronen gibt es $2^{64} \approx 1{,}8\cdot 10^{19}$ mögliche Masken – das „Ensemble mit geteilten Parametern" aus der VL.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Konfidenz per MC-Dropout
> Ein Hautbild-Klassifikator läuft mit **aktivem** Dropout fünfmal über dasselbe Bild (Spezialfall aus VL0, F. 51) und liefert für „bösartig" $0{,}55;\ 0{,}80;\ 0{,}40;\ 0{,}75;\ 0{,}60$. Mittel 0,62, Standardabweichung ≈ 0,14 – das Modell ist unsicher, der Fall geht an eine Dermatologin. Ein anderes Bild mit $0{,}97 \pm 0{,}01$ kann automatisch vorsortiert werden.

## Typische Prüfungsfragen & Fallstricke
- „Warum wirkt Dropout regularisierend?" → Ensemble mit geteilten Parametern, keine Co-Adaption.
- „Was ändert sich zwischen Training und Inferenz?" → Training zufälliges Weglassen, Inferenz alles aktiv (außer für Unsicherheitsschätzung).
- Fallstrick: vergessen, das Modell in den Auswertungsmodus zu schalten.

## Verwandt
- [[regularisierung]] – Taxonomie
- [[bagging]] – Ensemble-Idee
- [[label-smoothing]] – weitere Regularisierung für Netze
- [[ridge-regression]] – empfohlene Kombination

## Karteikarten
Was passiert beim Dropout?::Im Training werden Modellbestandteile zufällig weggelassen; in der Auswertung werden alle verwendet.
Als was kann man Dropout interpretieren?::Als Ensemble mehrerer Modelle, die viele Parameter teilen.
Typische Dropout-Rate für Fully-connected bzw. Conv-Layer?::FC 0,2–0,5; Conv 0,1–0,3.
Wozu Dropout in der Inferenz?::Um eine Konfidenz der Vorhersage zu schätzen (Spezialfall).
Risiko einer zu hohen Dropout-Rate?::Lernverlangsamung, hoher Bias, degradierte Repräsentationen.

## Quellen
- [[q-vl00-essentiell|VL0]], Folie 51
- [[q-vl03-regularisierung-cv|VL3]], Folien 29–30
- [[q-nb02-mnist-regularization|NB2]]
- [[q-lab01-pytorch-introduction|Lab 1]], Abschnitte „Dropout", „Comparing the results"
