---
typ: quelle
themen: [xai, notebook]
aliases: [NB7]
datei: "raw/uebungen/nb07-local-xai.ipynb"
original: "6_Explainable AI and Fairness/4_local_XAI_nb.json"
daten: "raw/uebungen/daten/customer-churn-synthetic.csv"
relevanz: mittel
aktualisiert: 2026-10-15
---

# NB7 · Lokale Erklärungen für Grenzfälle und Fehlklassifikationen

## Zusammenfassung
Gleicher Churn-Datensatz wie [[q-nb06-global-xai|NB6]], Gradient Boosting (Accuracy 0,862, AUC 0,726). Drei Fälle werden gezielt ausgewählt: **typisch korrekt** (p = 0,006), **Grenzfall** (p = 0,504), **Fehlklassifikation** (p = 0,015, tatsächlich churn). Methoden: (1) Lehr-Approximation „ein Merkmal durch Hintergrund-Mittelwert/Modus ersetzen", (2) Stabilitätsprüfung mit fünf Hintergrund-Stichproben, (3) LIME-artiges Surrogat (1 200 Nachbarn, gewichtete Ridge-Regression, Kernbreite 3, lokales R²), (4) SHAP mit `TreeExplainer` inkl. Waterfall-Plot, (5) Kontrafaktuale per Zufallssuche über veränderbare Merkmale.

## Prüfungsrelevante Aussagen
- **Fallauswahl** für lokale Erklärungen: typisch, Grenzfall, Fehler.
- Grenzfall: Ersetzen von `tenure_months` (20 → Mittel 36,3) verändert p um 0,40 – lokale Effekte sind nahe der Entscheidungsgrenze groß.
- Top-3-Merkmale schwanken über Hintergrund-Seeds → Stabilität berichten.
- SHAP-Werte liegen bei Bäumen oft auf **Log-Odds-/Rohskala**, nicht auf der Wahrscheinlichkeitsskala.
- Gute Kontrafaktuale: sparsam, plausibel, handlungsfähig, robust; kein Garant für reale Interventionen.
- Siehe [[lime]], [[shap]], [[kontrafaktische-erklaerungen]].
