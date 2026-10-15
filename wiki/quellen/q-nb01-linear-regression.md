---
typ: quelle
themen: [glm, notebook]
aliases: [NB1]
datei: "raw/uebungen/nb01-linear-regression.ipynb"
original: "4_Jupyter Notebooks/1_linear_regression.json"
relevanz: hoch
aktualisiert: 2026-10-15
---

# NB1 · Lineare Regression mit NumPy (7 Zellen)

## Zusammenfassung
Minimalbeispiel zur Normalgleichung: fünf Datenpunkte $x = (-7,-4,-2,0,2)$, $y = (1,2,2,0,-1)$, Designmatrix mit Einsen-Spalte, Lösung von $X^TXa = X^Ty$ mit `np.linalg.solve`, Plot der Geraden.

## Gliederung
| Zellen | Inhalt | Wiki-Seiten |
|---|---|---|
| 0–3 | Daten, Designmatrix | [[lineare-regression]] |
| 4–5 | $X^TX = \begin{pmatrix}5 & -11\\-11 & 73\end{pmatrix}$, Lösung $a = (0{,}25;\,-0{,}25)$ | [[lineare-regression-normalgleichungen]] |
| 6 | Plot | – |

## Prüfungsrelevante Aussagen
- `np.linalg.solve(XTX, XTy)` statt expliziter Inverse – numerisch stabiler (gleiches Ergebnis).
- Die Zahlen sind klein genug für eine Handrechnung → ideale Klausurübung, siehe [[lineare-regression-normalgleichungen]].
