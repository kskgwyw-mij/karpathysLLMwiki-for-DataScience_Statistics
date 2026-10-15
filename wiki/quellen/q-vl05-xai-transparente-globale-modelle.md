---
typ: quelle
themen: [xai, interpretierbarkeit]
aliases: [VL5]
datei: "raw/vorlesung/vl05-xai-transparente-globale-modelle.pdf"
original: "3_Slides/5_Statistical_Learning_2_XAI_1.pdf"
relevanz: mittel
aktualisiert: 2026-10-15
---

# VL5 · Erklärbare KI I: Grundlagen, transparente Modelle, globale Erklärungen (29 Folien)

Folie = PDF-Seite. Die Fußzeilen tragen eine eigene Nummerierung (z. B. „Fairness 25"); im Wiki wird immer die PDF-Seite zitiert.

## Zusammenfassung
Kapitel 6 Teil 1. Motivation (Erklärbarkeit fragt „warum?", Fairness fragt „wer ist betroffen?"), Begriffe (Interpretierbarkeit vs. Erklärbarkeit, global vs. lokal), Taxonomie der Erklärungsmethoden, Erklärungsqualität und typische Fallen, zwei Aktivitäten. Teil 2: transparente Modelle – Transparenzspektrum, lineare Modelle und logistische Regression als Erklärung, Regularisierung und Interpretierbarkeit, Entscheidungsbäume, Scorecards und Regellisten, Baum-Notebook. Teil 3: globale Post-hoc-Erklärungen – Permutation Feature Importance, korrelierte Merkmale, PDP, ICE, ALE, Vergleich und Übung.

## Gliederung
| Folien | Inhalt | Wiki-Seiten |
|---|---|---|
| 2–6 | Motivation, Leitfragen, Begriffe, Taxonomie, Qualität und Fallen | [[erklaerbarkeit-grundlagen]] |
| 7–8 | Aktivitäten 1 und 2 | [[zwei-erklaerungen-pruefen]] |
| 9–10, 15–16 | Transparenzspektrum, Scorecards, Zusammenfassung | [[transparente-modelle]] |
| 11, 13 | Lineare Modelle als Erklärung, Regularisierung | [[transparente-modelle]] |
| 12 | Logistische Regression | [[logistische-regression]] |
| 14, 17 | Entscheidungsbäume, Baum-Notebook | [[entscheidungsbaeume]], [[q-nb05-tree-exam-grade]] |
| 18–20 | Globale Erklärungen, Bild „Kluger Hans" (nur Grafik) | [[erklaerbarkeit-grundlagen]] |
| 21–22 | Permutation Feature Importance, korrelierte Merkmale | [[permutation-feature-importance]] |
| 23–28 | PDP, ICE, ALE, Vergleich, Kernaussagen | [[pdp-ice-ale]] |
| 29 | Übung globale Modellinspektion | [[q-nb06-global-xai]] |

## Prüfungsrelevante Aussagen
- Feature-Attribution ist **kein kausaler Effekt** (Folie 6, 28).
- $\exp(\beta_j)$ = multiplikatives Odds Ratio pro Einheit $x_j$ (Folie 12).
- L1 → sparse, L2 → gleichmäßiges Schrumpfen + Stabilität, Elastic Net für korrelierte Gruppen (Folie 13).
- Bäume sind Schätzer mit **hoher Varianz** (Folie 14).
- PDP extrapoliert bei abhängigen Merkmalen; ALE ist dann sicherer; ICE zeigt Heterogenität (Folie 27–28).

## Widersprüche / Abweichungen
- Folie 23: PDP-Formel als Summe ohne $\frac{1}{N}$ und mit Index $N$ statt $p$ – gemeint ist der Mittelwert über die Beobachtungen. Siehe [[pdp-ice-ale]].
- Folie 19 ist eine reine Bildfolie (historisches Foto eines Pferdes vor Publikum – sehr wahrscheinlich der „Kluge Hans"; der Folientext nennt ihn nicht). Interpretation im Wiki als Ergänzung gekennzeichnet.

## Offene Fragen
- Kommt Kapitel 6 in der Klausur vor? Es fehlt in [[q-vl00-essentiell|VL0]].
