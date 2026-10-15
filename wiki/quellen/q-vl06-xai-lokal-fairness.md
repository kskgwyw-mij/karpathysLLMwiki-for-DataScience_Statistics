---
typ: quelle
themen: [xai, fairness]
aliases: [VL6]
datei: "raw/vorlesung/vl06-xai-lokal-fairness.pdf"
original: "3_Slides/6_slides_6.pdf"
relevanz: mittel
aktualisiert: 2026-10-15
---

# VL6 · Erklärbare KI II: lokale Erklärungen, Fairness (34 Folien)

Folie = PDF-Seite.

## Zusammenfassung
Kapitel 6 Teil 2. Teil 4 lokale Erklärungen: LIME (Algorithmus, Zielfunktion, Stellschrauben, Diagnostik), SHAP (Shapley-Formel mit Gewichten, interventional vs. conditional, TreeSHAP, Diagramme), kontrafaktische Erklärungen, algorithmischer Recourse, Grad-CAM, Übung. Teil 5 Fairness-Grundlagen: Schadensarten, geschützte Attribute und Proxys, Demographic Parity, Equal Opportunity, Equalized Odds, Predictive Parity, Kalibrierung, Unvereinbarkeit der Metriken, Metrikwahl nach Schaden. Teil 6 Fairness in der Praxis: Dokumentation, Subgruppen und Unsicherheit, Schwellenwerte, Mitigation (pre-, in-, post-processing), Trade-off-Kurven, Validierung nach Mitigation.

## Gliederung
| Folien | Inhalt | Wiki-Seiten |
|---|---|---|
| 1–2 | Leitfragen, lokale Erklärungen | [[erklaerbarkeit-grundlagen]] |
| 3–6 | LIME | [[lime]] |
| 7–9 | SHAP | [[shap]] |
| 10–11 | Kontrafaktuale, Recourse | [[kontrafaktische-erklaerungen]] |
| 12 | Grad-CAM | [[grad-cam]] |
| 13–14 | Kernaussagen, Übung | [[lime-vs-shap]], [[q-nb07-local-xai]] |
| 15–18 | Fairness: Warum, Schadensarten, Proxys | [[fairness-grundlagen]] |
| 19–24 | Gruppenmetriken, Kollision, Metrikwahl | [[fairness-metriken]] |
| 25–34 | Dokumentation, Subgruppen, Schwellenwerte, Mitigation, Validierung | [[fairness-mitigation]] |

## Prüfungsrelevante Aussagen
- LIME: $F(x_0) = \arg\min_{g\in G} \mathcal L(f, g, \pi_{x_0}) + \Omega(g)$ (Folie 4).
- Shapley: $\phi_i = \sum_{S\subseteq N\setminus\{i\}} w(|S|)\,\big(f(S\cup\{i\}) - f(S)\big)$, $w(k) = \frac{k!\,(p-k-1)!}{p!}$ (Folie 7).
- Demographic Parity $P(\hat Y = 1\mid A=a)$, Equal Opportunity $P(\hat Y=1\mid Y=1, A=a)$, Equalized Odds zusätzlich $Y=0$, Predictive Parity $P(Y=1\mid \hat Y=1, A=a)$ (Folien 19–21).
- Kalibrierung, Equalized Odds und Predictive Parity sind im Allgemeinen **nicht gleichzeitig** erfüllbar (Folie 22).

## Widersprüche / Abweichungen
- Keine gefunden.

## Offene Fragen
- Prüfungsrelevanz von Kapitel 6 (siehe [[q-vl05-xai-transparente-globale-modelle|VL5]]).
