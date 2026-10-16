# Index

> Katalog aller Wiki-Seiten. Wird vom Agenten bei jedem Ingest aktualisiert.
> Format: `- [[seite]] — Einzeiler · relevanz · Quellen: n`
> Relevanz: **hoch** = in der Folienauswahl „essentiell", **mittel** = nur in der übrigen VL, **niedrig** = Vertiefung.

## Übersicht
- [[overview]] — Klausur (Format, Termin), Themenkarte, Lernplan, offene Fragen
- [[dashboard]] — Countdown und Fortschritt nach Relevanz und Beherrschung
- [[formelsammlung]] — alle Formeln nach Kapiteln, markiert nach Relevanz
- [[schwachstellen]] — Fehlerbilder aus Quiz und Übungen

## Konzepte
### Kapitel 1–2 · GLM, Bias-Varianz, Regularisierung, Modellwahl
- [[statistisches-lernproblem]] — $Y = f(x) + \varepsilon$, Vorhersage vs. Inferenz · hoch · Quellen: 2
- [[bias-varianz-tradeoff]] — MSE = Bias² + Varianz + σ², Diagnose Under-/Overfitting · hoch · Quellen: 4
- [[regularisierung]] — Definition, Taxonomie (hart, weich, Output, Early Stopping, Design, Dropout, Label Smoothing), Entscheidungsschema, Diagnose · hoch · Quellen: 4
- [[modellauswahl-vorgehen]] — Faustregeln nach N, p/N, Datentyp; 4-Phasen-Ablauf · hoch · Quellen: 2
- [[train-val-test-split]] — Hyperparameter-Auswahlprozess, Test nur einmal, Data Leakage · hoch · Quellen: 4
### Kapitel 3 · Optimierung
- [[optimierung-grundlagen]] — Minima-Typen, Max↔Min, Konvexität und ihre Folgen · hoch · Quellen: 2
- [[initialisierung]] — Xavier/He, Seeds, konvex vs. nichtkonvex · mittel · Quellen: 2
### Kapitel 5 · Probabilistische Modellierung
- [[probabilistische-sicht-map]] — Bayes, Likelihood, Prior; Gauß-Prior = L2, Laplace-Prior = L1; MLE vs. MAP · mittel · Quellen: 4
### Kapitel 6 · XAI und Fairness
- [[erklaerbarkeit-grundlagen]] — Interpretierbarkeit vs. Erklärbarkeit, global/lokal, Taxonomie, Qualitätskriterien · mittel · Quellen: 2
- [[fairness-grundlagen]] — Schadensarten, geschützte Attribute, Proxys · mittel · Quellen: 3

## Methoden
### Kapitel 1 · General Linear Models
- [[lineare-regression]] — Designmatrix, Herleitung der Normalgleichung $\hat a = (X^TX)^{-1}X^Ty$ · hoch · Quellen: 4
- [[basisfunktionen-regression]] — Polynom, RBF, Sigmoid, Fourier, Bins; Basismatrix B · hoch · Quellen: 3
- [[splines-und-b-splines]] — Spline-Definition, Cox-de-Boor-Rekursion, Eigenschaften · mittel · Quellen: 2
### Kapitel 2 · Regularisierung
- [[ridge-regression]] — L2/Tikhonov/Weight Decay, Geometrie (Kreis), geschlossene Lösung · hoch · Quellen: 5
- [[lasso-regression]] — L0, L1 als Konvexifizierung, Raute, Elastic Net, Soft-Thresholding · hoch · Quellen: 6
- [[output-regularisierung]] — Output bestrafen: TV/Laplace, Shape-/Topologie-/gelernte Priors · hoch · Quellen: 2
- [[early-stopping]] — Abbruch nach Val-Performance; äquivalent zu L2 beim linearen Modell · hoch · Quellen: 5
- [[dropout]] — zufälliges Weglassen im Training, Ensemble-Deutung, Raten · hoch · Quellen: 3
- [[label-smoothing]] — weiche Labels gegen Überkonfidenz · hoch · Quellen: 3
### Kapitel 3 · Optimierung
- [[gradientenabstieg]] — Update-Regel, Lernrate, Schrittweitenstrategien, Stoppkriterien · hoch · Quellen: 2
- [[momentum-und-nesterov]] — Geschwindigkeitsvektor, Look-ahead-Gradient · hoch · Quellen: 2
- [[sgd]] — Minibatch-Gradient, Batchgröße, Shuffling, Decay · hoch · Quellen: 2
- [[adaptive-optimierer]] — AdaGrad, AdaDelta, Adam (Bias-Korrektur, Defaults), LION · hoch · Quellen: 3
- [[newton-und-quasi-newton]] — Newton (Folienfehler!), gedämpft, BFGS, L-BFGS · hoch · Quellen: 2
- [[restringierte-optimierung]] — KKT, projizierter und proximaler Gradient, Algorithmenfamilien · mittel · Quellen: 3
### Kapitel 4 · Resampling
- [[kreuzvalidierung]] — Hold-Out, K-Fold, Stratified, LOOCV · hoch · Quellen: 4
- [[bootstrap]] — Ziehen mit Zurücklegen, Standardfehler, Konfidenzintervalle, OOB · hoch · Quellen: 3
- [[bagging]] — Bootstrap Aggregating, Varianzreduktion · hoch · Quellen: 3
### Kapitel 6 · XAI und Fairness
- [[transparente-modelle]] — Transparenzspektrum, Koeffizienten lesen, Scorecards · mittel · Quellen: 1
- [[logistische-regression]] — Log-Odds, Odds Ratio, Kalibrierung · mittel · Quellen: 4
- [[entscheidungsbaeume]] — Pfad als Erklärung, hohe Varianz · mittel · Quellen: 3
- [[permutation-feature-importance]] — Leistungsabhängigkeit, korrelierte Merkmale · mittel · Quellen: 2
- [[pdp-ice-ale]] — Effektkurven, Extrapolation, Vergleichstabelle · mittel · Quellen: 2
- [[lime]] — lokales gewichtetes Surrogat, Zielfunktion, Stellschrauben · mittel · Quellen: 2
- [[shap]] — Shapley-Formel, interventional vs. conditional, TreeSHAP, Plots · mittel · Quellen: 2
- [[kontrafaktische-erklaerungen]] — minimale Änderung, Recourse · mittel · Quellen: 2
- [[grad-cam]] — Heatmaps für CNNs · niedrig · Quellen: 1
- [[fairness-metriken]] — DP, Equal Opportunity, Equalized Odds, Predictive Parity, Kalibrierung, Kollision · mittel · Quellen: 1
- [[fairness-mitigation]] — Dokumentation, Subgruppen, Schwellen, Pre-/In-/Post-processing · mittel · Quellen: 1

## Aufgaben
- [[lineare-regression-normalgleichungen]] — 2×2-System von Hand, Ridge-Variante · hoch · Quellen: 2
- [[basisfunktionen-uebung-folie-30]] — Basismatrix, $\{1,x\}$ von Hand, $\{1,x,x^2\}$ · hoch · Quellen: 1
- [[b-splines-grad-2-von-hand]] — Cox-de Boor Grad 0 → 1 → 2 · mittel · Quellen: 1
- [[bias-varianz-herleitung]] — Herleitung Schritt für Schritt („nur für Interessierte") · niedrig · Quellen: 1
- [[modellauswahl-fallbeispiele]] — Qualitätskontrolle und Fake-Shop gegen die 4 Phasen · hoch · Quellen: 3
- [[gradientenabstieg-von-hand]] — Lernrate, Momentum, Konditionierung, Newton-Schritt · hoch · Quellen: 2
- [[adam-adagrad-erster-schritt]] — diagonale Vorkonditionierung, Bias-Korrektur · mittel · Quellen: 1
- [[bootstrap-oob-anteil]] — $(1-1/n)^n\to e^{-1}$ · mittel · Quellen: 2
- [[logistische-regression-odds-ratio]] — gleiches β, verschiedene Δp · mittel · Quellen: 1
- [[shapley-von-hand]] — drei Merkmale, Gewichte, Effizienz · mittel · Quellen: 1
- [[fairness-metriken-rechnen]] — Konfusionsmatrizen je Gruppe · mittel · Quellen: 1
- [[churn-output-interpretation]] — Accuracy-Falle, AUC vs. Accuracy · mittel · Quellen: 3
- [[zwei-erklaerungen-pruefen]] — Aussagen klassifizieren, umformulieren (Aktivität 2) · mittel · Quellen: 3

## Vergleiche
- [[l0-l1-l2-vergleich]] — Normen, Geometrie, warum L1 Nullen erzeugt · hoch · Quellen: 4
- [[optimierer-uebersicht]] — alle Update-Regeln, erste vs. zweite Ordnung, Takeaways · hoch · Quellen: 2
- [[cv-vs-bootstrap]] — Ziehen, Zweck, Output · hoch · Quellen: 2
- [[lime-vs-shap]] — LIME, SHAP, Kontrafaktuale gegenübergestellt · mittel · Quellen: 2

## Quellen
| Wiki-Seite | Datei in `raw/` | Original im Moodle-Export |
|---|---|---|
| [[q-vl00-essentiell]] — **Folienauswahl „essentiell", Hauptquelle** · hoch | `vorlesung/vl00-essentiell.pdf` | `7_Folienauswahl/1_Statistisches_Lernen_2_bbm_essentiell.pdf` |
| [[q-vl01-glm-basisfunktionen-splines]] — Organisation, GLM, Basisfunktionen, Splines · hoch | `vorlesung/vl01-glm-basisfunktionen-splines.pdf` | `3_Slides/1_Statistisches_Lernen_2_L1.pdf` |
| [[q-vl02-bias-varianz-modellwahl]] — Bias-Varianz, Modellwahl, Workshop · hoch | `vorlesung/vl02-bias-varianz-modellwahl.pdf` | `3_Slides/2_Statistisches_Lernen_2_v2.pdf` |
| [[q-vl03-regularisierung-cv]] — Regularisierung, probabilistische Sicht, CV · hoch | `vorlesung/vl03-regularisierung-cv.pdf` | `3_Slides/3_Statistisches_Lernen_2_bbm_v3.pdf` |
| [[q-vl04-map-resampling-optimierung]] — MAP, Bootstrap, Optimierung · hoch | `vorlesung/vl04-map-resampling-optimierung.pdf` | `3_Slides/4_Statistisches_Lernen_2_v4.pdf` |
| [[q-vl05-xai-transparente-globale-modelle]] — XAI I · mittel | `vorlesung/vl05-xai-transparente-globale-modelle.pdf` | `3_Slides/5_Statistical_Learning_2_XAI_1.pdf` |
| [[q-vl06-xai-lokal-fairness]] — XAI II, Fairness · mittel | `vorlesung/vl06-xai-lokal-fairness.pdf` | `3_Slides/6_slides_6.pdf` |
| [[q-esl-elements-of-statistical-learning]] — Lehrbuch, Kapitelzuordnung · niedrig | `literatur/esl-elements-of-statistical-learning.pdf` | `3_Slides/7_elements.pdf` |
| [[q-literaturliste]] — Lektüreliste ISLP, PSDS, HOML, Kapitelzuordnung (ungeprüft) · niedrig | – (nicht in `raw/`) | – |
| [[q-nb01-linear-regression]] — Normalgleichung in NumPy · hoch | `uebungen/nb01-linear-regression.ipynb` | `4_Jupyter Notebooks/1_linear_regression.json` |
| [[q-nb02-mnist-regularization]] — 8 Regularisierungen auf MNIST · mittel | `uebungen/nb02-mnist-regularization.ipynb` | `4_Jupyter Notebooks/2_mnist_regularization_pytorch.xxx` |
| [[q-nb03-regularization-simple]] — vereinfachte Version (mit Bugs) · niedrig | `uebungen/nb03-regularization-simple.ipynb` | `4_Jupyter Notebooks/3_regularization_pytorch_simple.json` |
| [[q-nb04-prior-map]] — MLE vs. MAP mit Ausreißer · mittel | `uebungen/nb04-prior-map.ipynb` | `4_Jupyter Notebooks/4_prior_MAP.json` |
| [[q-nb05-tree-exam-grade]] — Baum als Erklärung · mittel | `uebungen/nb05-tree-exam-grade.ipynb` (+ `daten/exam-grades-synthetic.csv`) | `6_Explainable AI and Fairness/2_…json`, `3_…csv` |
| [[q-nb06-global-xai]] — globale Inspektion Churn · mittel | `uebungen/nb06-global-xai.ipynb` (+ `daten/customer-churn-synthetic.csv`) | `6_Explainable AI and Fairness/5_…json`, `6_…csv` |
| [[q-nb07-local-xai]] — LIME, SHAP, Kontrafaktuale · mittel | `uebungen/nb07-local-xai.ipynb` | `6_Explainable AI and Fairness/4_local_XAI_nb.json` |
| [[q-aktivitaet-zwei-erklaerungen]] — Aktivität 2 · mittel | `uebungen/aktivitaet-zwei-erklaerungen.pdf` | `6_Explainable AI and Fairness/1_zwei_erklaerungen.pdf` |
| [[q-modellauswahl-abgaben]] — eigene Workshop-Abgaben · mittel | `uebungen/modellauswahl/` | `5_Modellauswahl/…` |

## Notebooks (`notebooks/`)
- `kapitel-1-regression-splines.ipynb` — Normalgleichung, Ridge-Pfad, Basisfunktionen, B-Splines inkl. Zerlegung der Eins
- `optimierer-vergleich.ipynb` — GD, Momentum, Nesterov, AdaGrad, Adam, Newton auf schlecht konditionierter Quadratik
- `xai-fairness-handrechnungen.ipynb` — exakte Shapley-Werte, Fairness-Metriken, Odds Ratio, Bootstrap-OOB
