---
typ: quelle
themen: [xai, notebook]
aliases: [NB6]
datei: "raw/uebungen/nb06-global-xai.ipynb"
original: "6_Explainable AI and Fairness/5_global_explainability.json"
daten: "raw/uebungen/daten/customer-churn-synthetic.csv"
relevanz: mittel
aktualisiert: 2026-10-15
---

# NB6 · Globale Modellinspektion (synthetische Kundenabwanderung)

## Zusammenfassung
900 Kunden, Ziel `churn` (12,3 % positiv), Merkmale `tenure_months`, `monthly_fee`, `support_tickets`, `usage_hours`, `contract_type`, `promo_user`, `late_payments`, `region`. Vergleich einer logistischen Regression (`class_weight="balanced"`) mit Gradient Boosting; wiederholte Permutation Importance (20×, Metrik ROC AUC), PDP+ICE für die zwei wichtigsten numerischen Merkmale mit Histogramm zur Datenabdeckung, selbst implementiertes ALE.

## Ergebnisse
| Modell | Accuracy | ROC AUC |
|---|---|---|
| Logistische Regression (balanced) | 0,680 | **0,783** |
| Gradient Boosting | **0,862** | 0,726 |

Permutation Importance (Gradient Boosting): `tenure_months` 0,062 ± 0,028, `usage_hours` 0,053 ± 0,026, `monthly_fee` 0,053 ± 0,037, `contract_type` 0,035 ± 0,028 …

ALE für `tenure_months`: von +0,10 (kurze Laufzeit) fallend auf −0,05 (lange Laufzeit).

## Prüfungsrelevante Aussagen
- **Accuracy-Falle:** Die Dummy-Baseline „nie churn" erreicht auf dem Testset 87,6 % – das Boosting-Modell liegt mit 86,2 % darunter und erkennt nur 2 von 28 Churnern (nachgerechnet). Die AUC zeigt, dass die logistische Regression besser rangiert. → [[churn-output-interpretation]]
- Die Standardabweichungen der Permutation Importance sind so groß, dass die Plätze 2 und 3 nicht unterscheidbar sind → Intervalle statt Rangliste berichten ([[permutation-feature-importance]]).
- Datenabdeckung prüfen, bevor PDP-Bereiche interpretiert werden ([[pdp-ice-ale]]).
