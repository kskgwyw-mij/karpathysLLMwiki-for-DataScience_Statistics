---
typ: methode
themen: [resampling, ensemble]
aliases: [Bagging, Bootstrap Aggregating, Ensemble, Random Forest]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Bagging (Bootstrap Aggregating)

#flashcards/statistik

## Kurzdefinition
Ensemble-Methode: Auf mehreren **Bootstrap-Stichproben** des Trainingsdatensatzes wird jeweils ein Modell trainiert; die Vorhersagen werden **aggregiert** (Mehrheitsentscheidung bei Klassifikation, Mittelwert bei Regression). Ziel: **Varianz reduzieren** (→ [[q-vl00-essentiell|VL0]], F. 66, 68).

## Intuition
Die Grafik der VL (VL0, F. 68): ursprüngliche Trainingsdaten → Bootstrap-Stichproben 1, 2, 3 → Modelle 1, 2, 3 → Aggregation (Majority Voting / Averaging) → finale Vorhersage, „more accurate & robust", mit **reduziertem Overfitting** und **verbesserter Stabilität**. Einzelne flexible Modelle (z. B. tiefe Bäume) schwanken stark mit den Daten; ihre Fehler sind teilweise zufällig und mitteln sich heraus.

## Formal
$$\hat f_\text{bag}(x) = \frac1B\sum_{b=1}^B \hat f^{*b}(x) \quad\text{(Regression)}, \qquad \hat G_\text{bag}(x) = \text{Mehrheit}\{\hat G^{*b}(x)\} \quad\text{(Klassifikation)}$$
mit $\hat f^{*b}$ trainiert auf der $b$-ten Bootstrap-Stichprobe.

> [!ergaenzung] Ergänzung (nicht aus der VL) – warum die Varianz sinkt
> Für $B$ identisch verteilte Vorhersagen mit Varianz $\sigma^2$ und paarweiser Korrelation $\rho$ gilt $\mathrm{Var}\big(\frac1B\sum\hat f^{*b}\big) = \rho\sigma^2 + \frac{1-\rho}{B}\sigma^2$ (ESL 15.2). Mehr Modelle senken den zweiten Term; der erste bleibt. **Random Forests** senken zusätzlich $\rho$, indem jeder Split nur eine zufällige Teilmenge der Merkmale betrachten darf. Der Bias bleibt beim Bagging etwa gleich.

## Annahmen & Voraussetzungen
- Lohnt sich bei **Schätzern mit hoher Varianz** (tiefe Bäume, [[entscheidungsbaeume]]); bei stabilen Modellen wie linearer Regression bringt Bagging wenig.

## Beispiel
Random Forest als guter Default bei mittlerer Datenmenge (→ VL0, F. 23). Dropout lässt sich ebenfalls als Ensemble mit geteilten Parametern deuten ([[dropout]]). Out-of-Bag-Beobachtungen jedes Modells liefern eine eingebaute Fehlerschätzung ([[bootstrap]]).

## Typische Prüfungsfragen & Fallstricke
- „Wie funktioniert Bagging und welchen Fehlerterm reduziert es?" → Bootstrap + Aggregation; Varianz.
- Fallstrick: Bagging mit Boosting verwechseln – Boosting trainiert **sequentiell** auf den Fehlern der Vorgänger (siehe Kurzpräsentation in [[q-modellauswahl-abgaben]]) und reduziert vor allem den Bias.
- Fallstrick: Bagging senkt nicht den Bias.

## Verwandt
- [[bootstrap]] – Grundlage
- [[bias-varianz-tradeoff]] – Ensembles als Methode zur Balance
- [[entscheidungsbaeume]] – typische Basismodelle
- [[dropout]] – implizites Ensemble

## Karteikarten
Wofür steht Bagging?::Bootstrap Aggregating.
Wie funktioniert Bagging?::Modelle auf mehreren Bootstrap-Stichproben trainieren und Vorhersagen aggregieren (Mehrheit bzw. Mittelwert).
Welchen Fehlerterm reduziert Bagging?::Die Varianz.
Bei welchen Modellen lohnt Bagging besonders?::Bei Schätzern mit hoher Varianz, z. B. tiefen Entscheidungsbäumen.
Unterschied Bagging vs. Boosting?::Bagging: parallel auf Bootstrap-Stichproben, mitteln, senkt Varianz. Boosting: sequentiell auf Fehlern der Vorgänger, senkt vor allem Bias.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 66, 68
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 20, 22
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 8.7 (S. 282), Kapitel 15 (S. 587)
