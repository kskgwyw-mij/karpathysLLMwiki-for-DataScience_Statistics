---
typ: aufgabe
themen: [glm, basisfunktionen]
quellen: ["[[q-vl01-glm-basisfunktionen-splines]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Regression mit Basisfunktionen (Übung VL1, Folie 30)

## Aufgabenstellung
Gegeben sind die Datenpunkte:

| $x$ | −3 | −1 | 0 | 0,5 | 4 |
|---|---|---|---|---|---|
| $y$ | 3 | 4 | 2 | −1 | 2 |

Wählen Sie Basisfunktionen, berechnen Sie $B$, $B^TB$ und $B^Ty$, lösen Sie nach $a$ und visualisieren Sie die Regressionsfunktion. (Original als Notebook-Aufgabe; hier zusätzlich als Papierversion mit der Basis $\{1, x\}$.)

## Lösungsweg (Basis $\{1, x\}$, von Hand)
1. $K = 5$, $\sum x = 0{,}5$, $\sum x^2 = 9 + 1 + 0 + 0{,}25 + 16 = 26{,}25$, $\sum y = 10$, $\sum xy = -9 - 4 + 0 - 0{,}5 + 8 = -5{,}5$.
2. $$B^TB = \begin{pmatrix}5 & 0{,}5\\ 0{,}5 & 26{,}25\end{pmatrix}, \qquad B^Ty = \begin{pmatrix}10\\-5{,}5\end{pmatrix}$$
3. Determinante: $5\cdot 26{,}25 - 0{,}25 = 131$.
4. $a_0 = \frac{26{,}25\cdot 10 - 0{,}5\cdot(-5{,}5)}{131} = \frac{265{,}25}{131} \approx 2{,}025$, $\quad a_1 = \frac{5\cdot(-5{,}5) - 0{,}5\cdot 10}{131} = \frac{-32{,}5}{131} \approx -0{,}248$.

## Lösungsweg (Basis $\{1, x, x^2\}$, mit Rechner)
$$B = \begin{pmatrix}1 & -3 & 9\\ 1 & -1 & 1\\ 1 & 0 & 0\\ 1 & 0{,}5 & 0{,}25 \\ 1 & 4 & 16\end{pmatrix},\quad B^TB = \begin{pmatrix}5 & 0{,}5 & 26{,}25\\ 0{,}5 & 26{,}25 & 36{,}125\\ 26{,}25 & 36{,}125 & 338{,}06\end{pmatrix},\quad B^Ty = \begin{pmatrix}10\\-5{,}5\\62{,}75\end{pmatrix}$$

## Ergebnis
| Basis | $\hat a$ | SSE (Training) |
|---|---|---|
| $\{1, x\}$ | $(2{,}025;\ -0{,}248)$ | 12,39 |
| $\{1, x, x^2\}$ | $(1{,}421;\ -0{,}399;\ 0{,}118)$ | 10,20 |
| Polynom Grad 4 | interpoliert alle 5 Punkte | 0 |

Mehr Basisfunktionen senken den Trainingsfehler immer – bis zur Interpolation. Ob das besser generalisiert, sagt erst ein Test- oder CV-Fehler ([[bias-varianz-tradeoff]], [[kreuzvalidierung]]).

> [!warning] Typischer Fehler
> - In $B^TB$ der Eintrag $(1,3)$ ist $\sum x^2$, der Eintrag $(3,3)$ ist $\sum x^4$.
> - Den sinkenden Trainingsfehler als Beleg für ein besseres Modell lesen.

## Benötigte Konzepte
- [[basisfunktionen-regression]]
- [[lineare-regression]]

## Quellen
- [[q-vl01-glm-basisfunktionen-splines|VL1]], Folien 29–30
- Nachgerechnet in `notebooks/kapitel-1-regression-splines.ipynb`
