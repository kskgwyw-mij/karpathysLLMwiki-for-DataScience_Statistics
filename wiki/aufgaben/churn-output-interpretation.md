---
typ: aufgabe
themen: [xai, metriken, modellwahl]
quellen: ["[[q-nb06-global-xai]]", "[[q-nb07-local-xai]]", "[[q-vl00-essentiell]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Output-Interpretation: Kundenabwanderung (Accuracy-Falle)

> [!info] Selbst formulierte Fragen zu echten Notebook-Outputs
> Zahlen aus [[q-nb06-global-xai|NB6]]; Fragen im Stil einer Output-Interpretationsaufgabe ergänzt.

## Gegeben
900 Kunden, Testanteil 25 % (stratifiziert), **12,3 % Churn**.

| Modell | Accuracy | ROC AUC |
|---|---|---|
| Logistische Regression, `class_weight="balanced"` | 0,680 | 0,783 |
| Gradient Boosting | 0,862 | 0,726 |

Permutation Importance (Gradient Boosting, ROC AUC, 20 Wiederholungen): `tenure_months` 0,062 ± 0,028; `usage_hours` 0,053 ± 0,026; `monthly_fee` 0,053 ± 0,037; `contract_type` 0,035 ± 0,028.

## Fragen
a) Welche Accuracy erreicht eine Dummy-Baseline?
b) Warum widersprechen sich Accuracy und AUC?
c) Welches Modell würden Sie für eine Kampagne „gefährdete Kunden anrufen" bevorzugen, und welche Metrik?
d) Ist `tenure_months` sicher das wichtigste Merkmal?
e) Formulieren Sie einen Satz zur Wirkung der Vertragslaufzeit ohne kausale Überbehauptung.

## Lösung
a) „Nie Churn" vorhersagen: auf dem Testset 197 von 225 richtig = **87,6 %** Accuracy – mehr als das Boosting-Modell (86,2 %). Ohne Dummy-Baseline ([[modellauswahl-vorgehen]], Phase 3) würde man 86 % für gut halten.

b) Accuracy hängt am Schwellenwert 0,5 und an der Klassenverteilung. Nachgerechnet auf dem Testset (28 Churner):

| Modell | vorhergesagt positiv | TP / FN / FP | Recall | Precision |
|---|---|---|---|---|
| Logistische Regression (balanced) | 39 % | 22 / 6 / 66 | 0,79 | 0,25 |
| Gradient Boosting | 3 % | 2 / 26 / 5 | 0,07 | 0,29 |
Gradient Boosting sagt fast nur „kein Churn" vorher → Accuracy nahe dem Mehrheitsanteil, aber nur 2 von 28 Churnern erkannt. Die logistische Regression gewichtet die seltene Klasse hoch, sagt viel mehr Churn vorher → viele False Positives → niedrigere Accuracy, aber hoher Recall. Die **AUC** ist schwellenunabhängig und misst die Rangfolge: Die logistische Regression trennt Churner und Nicht-Churner besser.

c) Für eine Kampagne zählt, gefährdete Kunden zu finden → Rangfolge und Recall der Churn-Klasse, also **AUC bzw. Recall/Precision bei gewähltem Schwellenwert**, nicht Accuracy. Auf diesen Zahlen spricht mehr für die **logistische Regression** (höhere AUC, zudem transparent). Danach Schwellenwert über Kampagnenkosten wählen.

d) Nein. Die Intervalle (Mittel ± Std) der ersten vier Merkmale überlappen; eine strikte Rangfolge ist nicht gestützt ([[permutation-feature-importance]]).

e) „Im trainierten Modell geht eine kurze Vertragslaufzeit mit einer höheren vorhergesagten Abwanderungswahrscheinlichkeit einher; ob eine längere Bindung Abwanderung tatsächlich verhindert, lässt sich daraus nicht ableiten."

> [!warning] Typischer Fehler
> Accuracy bei unbalancierten Klassen als Hauptmetrik verwenden.

## Benötigte Konzepte
- [[modellauswahl-vorgehen]]
- [[logistische-regression]]
- [[permutation-feature-importance]]
- [[pdp-ice-ale]]

## Quellen
- [[q-nb06-global-xai|NB6]]
- [[q-nb07-local-xai|NB7]]
- [[q-vl00-essentiell|VL0]], Folie 32
