---
typ: quelle
themen: [literatur]
aliases: [Lektüre, Begleitliteratur, ISLP, PSDS, HOML]
datei: "– (nicht in raw/)"
original: "–"
relevanz: niedrig
aktualisiert: 2026-10-16
---

# Lektüreliste zur Vorlesung · ISLP, PSDS, HOML

Die Lektüreliste stammt von Jan (16.10.). Die Bücher liegen **nicht** in `raw/`, deshalb gibt es noch keine eigenen Quellenseiten, keine Seitenangaben und keine Belege aus den Büchern. Ein weiteres Lehrbuch, [[q-esl-elements-of-statistical-learning|ESL]], steht in der Literaturliste von [[q-vl01-glm-basisfunktionen-splines|VL1]], Folie 11. Es ist bereits zugeordnet und gehört nicht zu dieser Liste.

## Liste (wie von Jan angegeben)
- James, G.; Witten, D; Hastie, T.; Tibshirani, R. (2023). An Introduction to Statistical Learning: with Applications with Python (ISBN: 978-3031387463)
- Bruce. P.; Bruce, A.; Gedeck, P. (2020): Practical Statistics for Data Scientists: 50+ Essential Concepts Using R and Python (ISBN: 978-1492072942)
- Geron, A. (2022): Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow: Concepts, Tools, and Techniques to Build Intelligent Systems, (ISBN: 978-1098125974)

Kurzbezeichnungen im Wiki: **ISLP**, **PSDS**, **HOML**.

> [!note] Rückfrage zur Bibliographie
> Soweit ich weiß, hat die Python-Ausgabe von ISLP (2023) einen fünften Autor (Jonathan Taylor), und der Untertitel lautet „with Applications **in** Python“. Ich habe die Liste trotzdem unverändert übernommen. Prüf das bitte am Moodle-Original, bevor du das Zitat irgendwo verwendest.

## Rolle für die Klausur
Die Klausur beruht auf den Folien. Laut [[q-vl00-essentiell|Folienauswahl „essentiell“]] ist eine gute Note möglich, wenn diese Folien gut verstanden sind. Die Bücher sind deshalb **Vertiefung** (Relevanz `niedrig`), so wie [[q-esl-elements-of-statistical-learning|ESL]]. Im Wiki gilt die Notation der Vorlesung.

## Zuordnung zu den Wiki-Themen

> [!ergaenzung] Ergänzung (nicht aus der VL) – Kapitelzuordnung aus den Inhaltsverzeichnissen
> **Nicht gegen die Bücher geprüft.** Die Zuordnung habe ich aus meiner Kenntnis der Inhaltsverzeichnisse erstellt, nur auf Kapitelebene, ohne Abschnitts- oder Seitennummern. Beim Ingest der PDFs wird sie verifiziert.
>
> | Buch | Kapitel | Wiki-Seiten |
> |---|---|---|
> | ISLP | 2 Statistical Learning | [[statistisches-lernproblem]], [[bias-varianz-tradeoff]] |
> | ISLP | 3 Linear Regression | [[lineare-regression]] |
> | ISLP | 4 Classification | [[logistische-regression]] |
> | ISLP | 5 Resampling Methods | [[kreuzvalidierung]], [[bootstrap]], [[cv-vs-bootstrap]] |
> | ISLP | 6 Linear Model Selection and Regularization | [[ridge-regression]], [[lasso-regression]], [[l0-l1-l2-vergleich]], [[probabilistische-sicht-map]] |
> | ISLP | 7 Moving Beyond Linearity | [[basisfunktionen-regression]], [[splines-und-b-splines]] |
> | ISLP | 8 Tree-Based Methods | [[entscheidungsbaeume]], [[bagging]] |
> | ISLP | 10 Deep Learning | [[sgd]], [[dropout]], [[regularisierung]] |
> | PSDS | 2 Data and Sampling Distributions | [[bootstrap]] |
> | PSDS | 4 Regression and Prediction | [[lineare-regression]], [[kreuzvalidierung]], [[basisfunktionen-regression]], [[splines-und-b-splines]] |
> | PSDS | 5 Classification | [[logistische-regression]], [[churn-output-interpretation]] |
> | PSDS | 6 Statistical Machine Learning | [[entscheidungsbaeume]], [[bagging]], [[permutation-feature-importance]] |
> | HOML | 1 The Machine Learning Landscape | [[train-val-test-split]] |
> | HOML | 2 End-to-End Machine Learning Project | [[kreuzvalidierung]], [[modellauswahl-vorgehen]] |
> | HOML | 3 Classification | [[churn-output-interpretation]] |
> | HOML | 4 Training Models | [[lineare-regression]], [[gradientenabstieg]], [[sgd]], [[bias-varianz-tradeoff]], [[ridge-regression]], [[lasso-regression]], [[early-stopping]], [[logistische-regression]] |
> | HOML | 6 Decision Trees | [[entscheidungsbaeume]] |
> | HOML | 7 Ensemble Learning and Random Forests | [[bagging]] |
> | HOML | 11 Training Deep Neural Networks | [[initialisierung]], [[momentum-und-nesterov]], [[adaptive-optimierer]], [[optimierer-uebersicht]], [[dropout]], [[regularisierung]] |
>
> **Vermutlich nicht oder kaum abgedeckt** (ebenfalls ungeprüft): [[newton-und-quasi-newton]], [[restringierte-optimierung]], [[output-regularisierung]] sowie die XAI- und Fairness-Methoden ([[lime]], [[shap]], [[pdp-ice-ale]], [[kontrafaktische-erklaerungen]], [[grad-cam]], [[fairness-metriken]], [[fairness-mitigation]]). Für diese Themen bleiben die Folien und Kurs-Notebooks die einzige Quelle.
>
> **Grobe Einordnung:**
> - ISLP ist die zugängliche Variante von ESL und kommt mit wenig Matrixalgebra aus.
> - PSDS legt den Schwerpunkt auf klassische Statistik und Interpretation.
> - HOML ist praxisorientiert und deckt Optimierer und neuronale Netze ab.
>
> ISLP bieten die Autoren nach meinem Kenntnisstand auf der Buch-Website (statlearning.com) als kostenloses PDF an.

## Offene Fragen
- **Herkunft der Liste:** Stammt sie aus der Modulbeschreibung, aus Moodle oder aus VL1, Folie 11? Laut [[q-vl01-glm-basisfunktionen-splines|VL1]] steht auf Folie 11 nur ESL.
- **PDFs:** Hast du die Bücher als PDF? Vorschlag für die Dateinamen in `raw/literatur/`:
  - `islp-introduction-statistical-learning-python.pdf`
  - `psds-practical-statistics-data-scientists.pdf`
  - `homl-hands-on-machine-learning.pdf`
  
  Daraus würden dann eigene Seiten `q-islp-…`, `q-psds-…` und `q-homl-…` mit Seitenangaben. Diese Seite bekäme `status: ersetzt`.
- **Verweise auf den Themenseiten:** Soll jede Themenseite im Abschnitt „Quellen“ einen Buchverweis bekommen? Mein Vorschlag: erst nach dem Ingest, damit die Verweise belegt sind.
