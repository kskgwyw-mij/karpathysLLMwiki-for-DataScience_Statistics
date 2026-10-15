---
typ: quelle
themen: [regularisierung, map, resampling]
aliases: [VL3]
datei: "raw/vorlesung/vl03-regularisierung-cv.pdf"
original: "3_Slides/3_Statistisches_Lernen_2_bbm_v3.pdf"
relevanz: hoch
aktualisiert: 2026-10-15
---

# VL3 · Regularisierung und Cross-Validation (48 Folien)

Folie = PDF-Seite.

## Zusammenfassung
Ausführlichste Quelle zur Regularisierung: Taxonomie (hart, weich, Output, Early Stopping, Design, Dropout, Label Smoothing), harte und weiche L2-Regularisierung samt Geometrie und Namen (Tikhonov, Ridge, Weight Decay), Implementierungswege (Strafterm, projizierter Gradient, proximale Verfahren), L0/Sparsity, Konvexifizierung zu L1/Lasso, Elastic Net, Output-Regularisierung mit Beispielen, Early Stopping, Regularisierung durch Design, Dropout und Label Smoothing jeweils mit Praxisempfehlungen und Fallstricken. Dann die probabilistische Sicht (Bayes, Likelihood, Prior → L2/L1), ein Entscheidungsschema, Hyperparameter-Richtwerte und -Auswahlprozess, Diagnose, praktische Shortcuts und der Einstieg in die Cross-Validation.

## Gliederung
| Folien | Inhalt | Wiki-Seiten |
|---|---|---|
| 2–10 | Fehlerzerlegung, Definition, Taxonomie | [[regularisierung]] |
| 12–16 | L2 hart/weich, Geometrie, Implementierung, Transformation $T(\theta)$ | [[ridge-regression]], [[l0-l1-l2-vergleich]] |
| 17–22 | L0 hart/weich, Geometrie, Konvexifizierung, L1 hart, Geometrie | [[lasso-regression]], [[l0-l1-l2-vergleich]] |
| 23 | Kombinationen, Elastic Net | [[lasso-regression]] |
| 24–25 | Output-Regularisierung, Beispiele | [[output-regularisierung]] |
| 26–28 | Early Stopping, Regularisierung durch Design | [[early-stopping]], [[regularisierung]] |
| 29–32 | Dropout, Label Smoothing inkl. Praxis/Fallstricke | [[dropout]], [[label-smoothing]] |
| 33 | Notebook-Hinweis | [[q-nb02-mnist-regularization]], [[q-nb03-regularization-simple]] |
| 34–37 | Probabilistische Sicht | [[probabilistische-sicht-map]] |
| 38–44 | Entscheidungsschema, Hyperparameter, Diagnose, Shortcuts | [[regularisierung]], [[train-val-test-split]] |
| 45–48 | Cross-Validation | [[kreuzvalidierung]] |

## Prüfungsrelevante Aussagen
- Effekt von L2: **Stetigkeit** (Folie 12). L0 ist nicht effizient optimierbar (Folie 17).
- Geometrie: L2-Kugel (Kreis), L1-Ball (Raute/Diamant), L0 mit Sparsity-Level 1 (Achsen) – Folien 13, 19, 22.
- Probabilistisch: $-\log p(\theta\mid D) = -\log p(D\mid\theta) - \log p(\theta)$; Gauß-Likelihood → quadratischer Fehler; Prior $e^{-\|\theta\|^2}$ → L2, $e^{-\|\theta\|_1}$ → L1 (Folien 35–37).
- Hyperparameter-Richtwerte: L2 $10^{-6}\ldots10^{-2}$, L1 $10^{-6}\ldots10^{-3}$, Max-Norm 1…3, Dropout 0,1…0,5, Patience 5–10 (Folie 41).

## Widersprüche / Abweichungen
- Folie 12 und 21: In der Mengendefinition steht $\vartheta$ statt $\theta$ (Tippfehler).
- Folie 31 enthält wie VL0 Folie 52 versehentlich Dropout-Text auf der Label-Smoothing-Folie.

## Offene Fragen
- Keine.
