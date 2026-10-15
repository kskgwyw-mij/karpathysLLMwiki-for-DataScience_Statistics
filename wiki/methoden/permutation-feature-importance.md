---
typ: methode
themen: [xai, global]
aliases: [Permutation Feature Importance, Permutationswichtigkeit, PFI, Gruppierte Permutation, Bedingte Permutation]
quellen: ["[[q-vl05-xai-transparente-globale-modelle]]", "[[q-nb06-global-xai]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Permutation Feature Importance

#flashcards/statistik

## Kurzdefinition
Globale, modellagnostische Wichtigkeit: Man berechnet einen Score $S$ auf Validierungsdaten, **permutiert** die Werte eines Merkmals $x_j$ zufällig und berechnet $S_\text{perm}$ neu. Die Verschlechterung misst, wie stark sich das Modell **für seine Vorhersageleistung** auf $x_j$ verlassen hat (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 21).

## Intuition
Mischt man eine Spalte, bleibt ihre Verteilung gleich, aber der Zusammenhang zum Ziel ist zerstört. Bricht die Leistung ein, hat das Modell das Merkmal gebraucht; bleibt sie gleich, nicht.

## Formal (→ VL5, F. 21)
1. Baseline-Score $S$ auf Validierungsdaten (AUC, Accuracy, RMSE, Log-Loss …).
2. Werte von $x_j$ zufällig permutieren, $S_\text{perm}$ berechnen.
3. Importance $= S - S_\text{perm}$ (Scores, höher ist besser) bzw. $\text{loss}_\text{perm} - \text{loss}$ (Losses).
4. **Permutation wiederholen**, um die Variabilität zu schätzen; **Intervalle zeigen, nicht nur ein Ranking**.

**Interpretation:** Das Modell hat sich auf diesem Datensatz und für diese Metrik auf das Merkmal verlassen – **Leistungsabhängigkeit, nicht Kausalität** (→ VL5, F. 28).

### Korrelierte Merkmale (→ VL5, F. 22)
- Kodieren zwei Merkmale ähnliche Information, ändert das Permutieren eines Merkmals die Leistung kaum, weil das andere das Signal weiter trägt → beide erscheinen unwichtig.
- Marginale Permutation kann **unrealistische Kombinationen** erzeugen (z. B. Alter und Berufserfahrung passen nicht mehr zusammen).
- **Gruppierte Permutation:** sinnvollen Merkmalsblock gemeinsam permutieren (z. B. alle One-Hot-Spalten eines Bildungsniveaus).
- **Bedingte Permutation:** aus der bedingten Verteilung von $x_j$ gegeben die anderen Merkmale ziehen → weniger unrealistische Störungen.
- Immer berichten, ob die Wichtigkeit **marginal, gruppiert oder bedingt** ist.

## Annahmen & Voraussetzungen
- Auf **Validierungs-/Testdaten** berechnen, nicht auf Trainingsdaten (sonst misst man auch Overfitting).

## Beispiel ([[q-nb06-global-xai|NB6]])
Gradient Boosting, Churn, 20 Wiederholungen, Metrik ROC AUC:

| Merkmal | Mittel | Std |
|---|---|---|
| `tenure_months` | 0,062 | 0,028 |
| `usage_hours` | 0,053 | 0,026 |
| `monthly_fee` | 0,053 | 0,037 |
| `contract_type` | 0,035 | 0,028 |
| `support_tickets` | 0,021 | 0,011 |
| `region` | 0,000 | 0,012 |
Die Plätze 1–4 überlappen innerhalb einer Standardabweichung – eine strikte Rangfolge wäre überinterpretiert. `promo_user` hat sogar leicht negative Importance (Rauschen).

## Typische Prüfungsfragen & Fallstricke
- Algorithmus in vier Schritten beschreiben.
- „Zwei stark korrelierte Merkmale haben beide geringe Importance – was ist los?" → das jeweils andere trägt das Signal; gruppiert permutieren.
- Fallstrick: Importance als kausalen Effekt formulieren.
- Fallstrick: nur ein Ranking ohne Unsicherheit berichten.

## Verwandt
- [[pdp-ice-ale]] – Richtung und Form des Effekts statt nur Stärke
- [[shap]] – mean |SHAP| als alternative globale Attribution
- [[entscheidungsbaeume]] – Impurity-Importance als modellspezifische Alternative
- [[erklaerbarkeit-grundlagen]]

## Karteikarten
Wie berechnet man Permutation Feature Importance?::Score $S$ auf Val-Daten; Merkmal $x_j$ permutieren; $S_\text{perm}$ berechnen; Importance $=S-S_\text{perm}$; wiederholen.
Was misst Permutation Importance – und was nicht?::Die Leistungsabhängigkeit des Modells von einem Merkmal; keine Kausalität.
Problem bei korrelierten Merkmalen?::Das andere Merkmal trägt das Signal weiter, Importance wird unterschätzt; zudem unrealistische Kombinationen.
Was ist gruppierte Permutation?::Einen sinnvollen Merkmalsblock gemeinsam permutieren, z. B. alle One-Hot-Spalten einer Variable.
Was ist bedingte Permutation?::Werte aus der bedingten Verteilung von $x_j$ gegeben die anderen Merkmale ziehen.
Warum die Permutation wiederholen?::Um die Variabilität zu schätzen und Intervalle statt nur eines Rankings zu berichten.

## Quellen
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folien 21–22, 28
- [[q-nb06-global-xai|NB6]]
