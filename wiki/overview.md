---
typ: uebersicht
themen: [klausur]
aktualisiert: 2026-10-15
---

# Klausurüberblick · Statistical Learning 2

| | |
|---|---|
| **Termin** | Fr, 13.11.2026, 11:00 Uhr – aktueller Termin (Jan, 15.10.); der 03.07. aus [[q-vl01-glm-basisfunktionen-splines\|VL1]], F. 12, ist überholt |
| **Modul** | Statistical Learning 2 / Statistisches Lernen 2, M.Eng. Intelligent Analytics & Data Science (berufsbegleitend), FH Kufstein Tirol; Dozent Johannes Schwab, PhD |
| **Format** | **Papier-und-Stift-Prüfung ohne Hilfsmittel** (→ [[q-vl01-glm-basisfunktionen-splines\|VL1]], F. 10) |
| **Dauer** | für den alten Termin 03.07. laut Terminplan der VL 2 Stunden (12:00–14:00) – für den 13.11. unbestätigt |
| **Gewichtung** | Prüfung **75 %** der Gesamtnote; **> 60 %** in der Prüfung nötig zum Bestehen; die übrigen 25 % (Mitarbeit, Aufgaben) können **nur verbessern** |
| **Wichtigste Quelle** | [[q-vl00-essentiell\|Folienauswahl „essentiell"]] – „eine gute Note ist möglich, wenn diese Folien gut verstanden sind"; einzelne Teilaufgaben können zusätzliches VL-Wissen brauchen |

Fortschritt und Countdown: [[dashboard]] · Alle Formeln: [[formelsammlung]] · Katalog: [[index]]

> [!tip] Was „ohne Hilfsmittel" für das Lernen heißt
> Formeln auswendig ([[formelsammlung]]), kleine Rechnungen sicher von Hand (2×2-Normalgleichung, B-Spline-Rekursion, GD-/Momentum-Schritte, Newton-Schritt, Shapley-Werte, Fairness-Raten), Output-Interpretation und begründete Modellvorschläge in Worten.

## Themenkarte
| Kapitel (VL-Plan) | Kernthemen | Relevanz | Aufgaben | Notebook |
|---|---|---|---|---|
| **1 General Linear Models** | [[statistisches-lernproblem]], [[lineare-regression]], [[basisfunktionen-regression]], [[splines-und-b-splines]] | hoch (Splines: mittel) | [[lineare-regression-normalgleichungen]], [[basisfunktionen-uebung-folie-30]], [[b-splines-grad-2-von-hand]] | `kapitel-1-regression-splines` |
| **2 Bias-Varianz & Regularisierung** | [[bias-varianz-tradeoff]], [[regularisierung]], [[ridge-regression]], [[lasso-regression]], [[l0-l1-l2-vergleich]], [[output-regularisierung]], [[early-stopping]], [[dropout]], [[label-smoothing]], [[train-val-test-split]] | hoch | [[bias-varianz-herleitung]] (niedrig) | – |
| **2b Modellauswahl** | [[modellauswahl-vorgehen]] | hoch | [[modellauswahl-fallbeispiele]], [[churn-output-interpretation]] | – |
| **3 Optimierung** | [[optimierung-grundlagen]], [[gradientenabstieg]], [[momentum-und-nesterov]], [[sgd]], [[adaptive-optimierer]], [[newton-und-quasi-newton]], [[optimierer-uebersicht]] | hoch | [[gradientenabstieg-von-hand]], [[adam-adagrad-erster-schritt]] | `optimierer-vergleich` |
| **3b Optimierung (Vertiefung)** | [[initialisierung]], [[restringierte-optimierung]] | mittel | – | – |
| **4 Resampling & Modellbewertung** | [[kreuzvalidierung]], [[bootstrap]], [[bagging]], [[cv-vs-bootstrap]] | hoch | [[bootstrap-oob-anteil]] | `xai-fairness-handrechnungen` |
| **5 Probabilistische Modellierung** | [[probabilistische-sicht-map]] | mittel | – | – |
| **6 XAI** | [[erklaerbarkeit-grundlagen]], [[transparente-modelle]], [[logistische-regression]], [[entscheidungsbaeume]], [[permutation-feature-importance]], [[pdp-ice-ale]], [[lime]], [[shap]], [[lime-vs-shap]], [[kontrafaktische-erklaerungen]], [[grad-cam]] | mittel | [[logistische-regression-odds-ratio]], [[shapley-von-hand]], [[zwei-erklaerungen-pruefen]] | `xai-fairness-handrechnungen` |
| **6 Fairness** | [[fairness-grundlagen]], [[fairness-metriken]], [[fairness-mitigation]] | mittel | [[fairness-metriken-rechnen]] | `xai-fairness-handrechnungen` |

## Lernplan (Vorschlag, ab 15.10.)
| Zeitraum | Fokus | Wiki-Operationen |
|---|---|---|
| 15.–20.10. | Kapitel 1 + Bias-Varianz: Normalgleichung herleiten, Aufgaben von Hand rechnen | query, `quiz glm` |
| 21.–26.10. | Kapitel 2 Regularisierung + Modellauswahl + Train/Val/Test | `quiz regularisierung`, Fallbeispiele schriftlich |
| 27.10.–01.11. | Kapitel 3 Optimierung + Kapitel 4 Resampling | `quiz optimierung`, `quiz resampling` |
| 02.–06.11. | Kapitel 5 + 6 (Umfang je nach Antwort zur Prüfungsrelevanz) | `quiz xai`, `quiz fairness` |
| 07.–10.11. | Gesamtwiederholung **ohne Hilfsmittel**: Formelsammlung aus dem Kopf, alle Aufgaben mit Zeitlimit | lint, quiz gemischt |
| 11.–12.11. | Nur noch Karteikarten, [[schwachstellen]], Formelsammlung | quiz |
| 13.11. | Klausur 11:00 | – |

## Offene Fragen an Jan
- **Prüfungsrelevanz von Kapitel 5–6:** Probabilistische Sicht, XAI und Fairness fehlen in der Auswahl „essentiell". Kommen sie in der Klausur vor? Je nach Antwort stufe ich sie auf `hoch` hoch oder auf `niedrig` herunter.
- **Dauer am 13.11.:** Die VL nannte für den alten Termin 03.07. zwei Stunden. Gilt das auch am 13.11.?
- **Altklausuren/Probeklausur:** Gibt es welche? Dann kommen sie nach `raw/altklausuren/`, und die Quizfragen orientieren sich an ihrem Stil.
