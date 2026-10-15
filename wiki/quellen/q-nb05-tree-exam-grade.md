---
typ: quelle
themen: [xai, baeume, fairness, notebook]
aliases: [NB5]
datei: "raw/uebungen/nb05-tree-exam-grade.ipynb"
original: "6_Explainable AI and Fairness/2_notebook_tree_exam_grade_SL2.json"
daten: "raw/uebungen/daten/exam-grades-synthetic.csv"
relevanz: mittel
aktualisiert: 2026-10-15
---

# NB5 · Entscheidungsbaum als Erklärung (synthetische Klausurnoten)

## Zusammenfassung
Synthetischer Datensatz mit 6 000 Zeilen und 19 Spalten (u. a. `mock_exam_score`, `quiz_avg`, `assignment_avg`, `attendance_rate`, `work_hours_week`, `commute_minutes`, `exam_anxiety_self_report`, `cohort` BB/VZ). Ziel: `exam_grade` (1–5). `exam_points` und `passed` werden als **Leakage** ausgeschlossen. Bäume der Tiefe 2, 3, 4 (`min_samples_leaf=12`, `class_weight="balanced"`), 75/25-Split stratifiziert.

## Ergebnisse
| Tiefe | Accuracy | Balanced Accuracy | Blätter |
|---|---|---|---|
| 2 | 0,461 | 0,495 | 4 |
| 3 | 0,561 | 0,514 | 8 |
| 4 | 0,516 | 0,554 | 16 |

Wurzelsplit immer `mock_exam_score ≤ 64,5`; zweitwichtigstes Merkmal `cohort_VZ` (Importance 0,19–0,25). Tiefe 2 sagt Note 3 nie vorher (Recall 0).

## Prüfungsrelevante Aussagen
- Baumpfad = Konjunktion von Wenn-dann-Bedingungen → [[entscheidungsbaeume]].
- **Accuracy und Balanced Accuracy wählen unterschiedliche Tiefen** (3 vs. 4) – die Metrikwahl ist Teil der Modellentscheidung.
- Der Split nach `cohort` ist ein mögliches **Proxy-/Fairnessrisiko**; Feature Importance ist **nicht kausal** (Zelle 18) → [[fairness-grundlagen]].
- Reflexionsfragen (Zelle 20) eignen sich als offene Prüfungsfragen.
