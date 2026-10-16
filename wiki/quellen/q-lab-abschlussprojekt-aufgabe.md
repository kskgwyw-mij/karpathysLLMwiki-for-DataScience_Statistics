---
typ: quelle
themen: [regularisierung, optimierung, lab, projekt]
aliases: [Kaggle-Aufgabe, Abschlussprojekt, Mini-Projekt PyTorch]
datei: "raw/uebungen/lab-abschlussprojekt-aufgabe.pdf"
original: "Lab-Kurs (Moodle-Export 14.10.2026): 3_Abschlussprojekt/1_kaggle_aufgabe.pdf"
relevanz: niedrig
aktualisiert: 2026-10-16
---

# Lab · Abschlussprojekt „PyTorch-Projekt mit Kaggle-Datensatz" (Aufgabe, 3 Seiten)

## Zusammenfassung
Mini-Projekt im Lab: Man wählt einen kleinen Kaggle-Datensatz und löst das Problem mit PyTorch. Dabei vergleicht man **mindestens zwei Regularisierungsmethoden** und **mindestens zwei Optimierer** aus der Vorlesung. Es geht nicht um die beste Kaggle-Platzierung, sondern um einen nachvollziehbaren experimentellen Workflow, saubere Umsetzung und sinnvolle Interpretation (S. 1). Abgabe: Dokumentation als PDF (3–5 Seiten) und ein lauffähiges Notebook (S. 2). Die eigene Abgabe steht in [[q-abschlussprojekt-abgabe]].

## Gliederung
| Seiten | Inhalt | Wiki-Seiten |
|---|---|---|
| 1 | Ziel, geeignete Problemtypen, Mindestanforderungen 1–3 (Datensatz, Modell, Regularisierung) | [[regularisierung]], [[data-augmentation]] |
| 2 | Mindestanforderungen 4–6 (Optimierer, Vergleich, beste Variante), Abgabe, Inhalt der Dokumentation, Hinweise | [[optimierer-uebersicht]], [[train-val-test-split]] |
| 3 | Bewertungskriterien (100 Punkte) | – |

## Mindestanforderungen (S. 1–2)
1. **Datensatz** von Kaggle; Zielvariable, Eingabedaten und Problemtyp beschreiben. Geeignet: Klassifikation tabellarischer Daten, Bildklassifikation, einfache Textklassifikation, Regression; nicht zu groß.
2. **PyTorch-Modell** mit nachvollziehbarem Trainingsloop bzw. Trainingsfunktion.
3. **Regularisierung:** mindestens zwei Methoden, z. B. L1, L2/Weight Decay, Dropout, Early Stopping, Data Augmentation, Label Smoothing, Output-Regularisierung.
4. **Optimierer:** mindestens zwei, z. B. SGD, SGD mit Momentum, Adam, AdamW, RMSprop.
5. **Vergleich** mit passenden Metriken (z. B. Accuracy, F1, ROC-AUC, MSE, MAE), übersichtlich als Tabelle oder Diagramm.
6. **Beste Variante** auswählen, begründen und im finalen Notebook vollständig trainieren und evaluieren.

Beispieldatensätze laut Hinweisen: Titanic, House Prices, Digit Recognizer, Fashion MNIST, Heart Disease, Student Performance.

## Bewertungskriterien (S. 3)
| Kriterium | Punkte |
|---|---|
| Datensatzwahl und Problemstellung | 5 |
| Datenbeschreibung und Vorverarbeitung | 10 |
| PyTorch-Modell und Trainingsloop | 15 |
| Mindestens zwei Regularisierungsmethoden | 20 |
| Mindestens zwei Optimierer | 20 |
| Vergleich der Experimente | 10 |
| Interpretation und Reflexion | 10 |
| Lauffähigkeit und Struktur des Notebooks | 5 |
| Kreativität bei der Methodenwahl über die Vorlesung hinaus | 5 |

## Prüfungsrelevante Aussagen
- Die Aufgabe zählt **Data Augmentation** und **Early Stopping** ausdrücklich zu den Regularisierungsmethoden (S. 1) → [[data-augmentation]], [[early-stopping]].
- Sie zählt **RMSprop** und **AdamW** zu den in der Vorlesung besprochenen Optimierern (S. 2) → [[adaptive-optimierer]].
- Relevanz `niedrig`: Das Projekt ist bereits abgegeben; für die Klausur zählen die Methoden, nicht die Projektanforderungen.

## Offene Fragen
- Zählt das Projekt zu den 25 % „Mitarbeit, Aufgaben", die die Note nur verbessern können ([[overview]]), oder ist es eine eigene Lehrveranstaltung?
