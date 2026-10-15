---
typ: methode
themen: [xai, lokal]
aliases: [SHAP, Shapley-Werte, Shapley Values, TreeSHAP, Interventional SHAP, Conditional SHAP, Waterfall Plot, Summary Plot]
quellen: ["[[q-vl06-xai-lokal-fairness]]", "[[q-nb07-local-xai]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# SHAP (Shapley-Werte)

#flashcards/statistik

## Kurzdefinition
SHAP behandelt Merkmale als **Spieler**, die zur Vorhersage beitragen. Der Shapley-Wert $\phi_i$ ist der **durchschnittliche marginale Beitrag** eines Merkmals über alle möglichen Merkmalskoalitionen. Lokal gilt die additive Zerlegung $f(x) = \text{baseline} + \phi_1 + \dots + \phi_p$ (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 7).

## Intuition
Wie verteilt man einen gemeinsamen Gewinn fair auf Teammitglieder? Man schaut für jede mögliche Reihenfolge bzw. Teilgruppe, wie viel ein Mitglied zusätzlich bringt, und mittelt. Bei Modellen ist der „Gewinn" die Differenz zwischen Vorhersage und Baseline.

## Formal
$N = \{1, \dots, p\}$ Menge der Merkmale; Gewicht für eine Teilmenge der Größe $k$ (→ VL6, F. 7):
$$w(k) = \frac{k!\,(p - k - 1)!}{p!}, \qquad \phi_i = \sum_{S\subseteq N\setminus\{i\}} w(\lvert S\rvert)\,\big(f(S\cup\{i\}) - f(S)\big)$$
$f(S)$ ist die Vorhersage, wenn nur die Merkmale in $S$ „bekannt" sind. Wie fehlende Merkmale simuliert werden und welche **Baseline** gilt, beeinflusst die Interpretation stark.

> [!info] Ergänzung (nicht aus der VL)
> - Für $p = 3$: $w(0) = \tfrac13$, $w(1) = \tfrac16$, $w(2) = \tfrac13$. Die Gewichte entsprechen dem Anteil aller $p!$ Reihenfolgen, in denen genau $S$ vor $i$ kommt.
> - **Effizienz:** $\sum_i\phi_i = f(N) - f(\varnothing)$ – genau die additive Zerlegung oben.
> - Linearer Fall mit unabhängigen Merkmalen und interventionaler Baseline: $\phi_j = \beta_j\,(x_j - \mathbb E[x_j])$.
> - Rechenbeispiel: [[shapley-von-hand]].

### Abhängigkeitsannahmen und TreeSHAP (→ VL6, F. 8)
| Variante | Idee | Folge |
|---|---|---|
| **Interventional SHAP** | fehlende Merkmale aus einer Hintergrundverteilung samplen | bricht Merkmalsabhängigkeit |
| **Conditional SHAP** | versucht Abhängigkeiten zu respektieren | kann korrelierten Merkmalen anders Kredit zuweisen |
| **TreeSHAP** | nutzt die Baumstruktur | exakte Shapley-Werte für Baum-Ensembles effizient (unter bestimmten Annahmen) |
- SHAP-Werte können auf **Wahrscheinlichkeits-, Logit- oder Rohscore-Skala** liegen – **immer die Skala beschriften**.
- Bei korrelierten Merkmalen die exakte Aufteilung des Beitrags zwischen Proxys nicht überinterpretieren.

### Diagramme (→ VL6, F. 9)
- **Waterfall-/Force-Plot:** additiver Pfad von der Baseline zur Vorhersage eines Falls.
- **Summary-Plot:** Merkmale nach **mittlerem absoluten SHAP-Wert** gerankt, mit Verteilung der lokalen Beiträge.
- **Mean |SHAP|** ist ein Modellattributions-Ranking, **kein kausales** Wichtigkeitsranking.

## Annahmen & Voraussetzungen
- Wahl von Baseline/Hintergrunddaten und Behandlung fehlender Merkmale explizit machen.
- Exakte Berechnung ist exponentiell in $p$ ($2^{p-1}$ Teilmengen pro Merkmal) → Approximationen oder TreeSHAP.

## Beispiel ([[q-nb07-local-xai|NB7]])
`shap.TreeExplainer` auf dem Gradient-Boosting-Churn-Modell; SHAP-Werte auf **Log-Odds-/Rohskala**. Fehlt das Paket, nutzt das Notebook als Lehr-Approximation „ein Merkmal durch Hintergrund-Mittelwert ersetzen" – das ist **nicht** SHAP, weil es nur die Koalition „alle anderen bekannt" betrachtet.

## Typische Prüfungsfragen & Fallstricke
- **Shapley-Werte für ein kleines Spiel von Hand berechnen** ([[shapley-von-hand]]).
- Gewichtsformel $w(k)$ erklären.
- Interventional vs. conditional.
- Fallstrick: Skala nicht angeben (Log-Odds vs. Wahrscheinlichkeit).
- Fallstrick: mean |SHAP| als kausale Wichtigkeit lesen.

## Verwandt
- [[lime]] – lokales Surrogat
- [[lime-vs-shap]] – Gegenüberstellung
- [[permutation-feature-importance]] – andere globale Wichtigkeit
- [[logistische-regression]] – Log-Odds-Skala
- [[shapley-von-hand]] – Rechenübung

## Karteikarten
Was ist ein Shapley-Wert?::Der durchschnittliche marginale Beitrag eines Merkmals über alle möglichen Merkmalskoalitionen.
Shapley-Formel?::$\phi_i=\sum_{S\subseteq N\setminus\{i\}}w(\lvert S\rvert)(f(S\cup\{i\})-f(S))$ mit $w(k)=\frac{k!(p-k-1)!}{p!}$.
Lokale SHAP-Dekomposition?::$f(x)=\text{baseline}+\phi_1+\dots+\phi_p$.
Interventional vs. Conditional SHAP?::Interventional samplet fehlende Merkmale aus einer Hintergrundverteilung (bricht Abhängigkeit); conditional versucht Abhängigkeiten zu respektieren.
Wofür ist TreeSHAP gut?::Effiziente exakte Shapley-Werte für Baum-Ensembles (unter bestimmten Annahmen).
Was zeigt ein Waterfall-Plot?::Den additiven Pfad von der Baseline zur Vorhersage eines einzelnen Falls.
Was ist mean |SHAP| – und was nicht?::Ein Modellattributions-Ranking; kein kausales Wichtigkeitsranking.
Warum die Skala von SHAP-Werten beschriften?::Sie können auf Wahrscheinlichkeits-, Logit- oder Rohscore-Skala liegen.

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 7–9, 13
- [[q-nb07-local-xai|NB7]]
