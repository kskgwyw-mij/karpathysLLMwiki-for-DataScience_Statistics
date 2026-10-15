---
typ: aufgabe
themen: [glm, regression]
quellen: ["[[q-nb01-linear-regression]]", "[[q-vl01-glm-basisfunktionen-splines]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Lineare Regression über die Normalgleichung (von Hand)

## Aufgabenstellung
Gegeben sind die Datenpunkte (aus [[q-nb01-linear-regression|NB1]]):

| $x$ | −7 | −4 | −2 | 0 | 2 |
|---|---|---|---|---|---|
| $y$ | 1 | 2 | 2 | 0 | −1 |

Bestimmen Sie die Koeffizienten der Regressionsgeraden $y = a_0 + a_1 x$ ohne Hilfsmittel.

## Lösungsweg
1. **Designmatrix** mit Einsen-Spalte:
$$X = \begin{pmatrix}1&-7\\1&-4\\1&-2\\1&0\\1&2\end{pmatrix}$$
2. **Summen bilden:** $K = 5$, $\sum x = -11$, $\sum x^2 = 49+16+4+0+4 = 73$, $\sum y = 4$, $\sum xy = -7 - 8 - 4 + 0 - 2 = -21$.
3. **Normalgleichung** $X^TXa = X^Ty$:
$$\begin{pmatrix}5 & -11\\-11 & 73\end{pmatrix}\begin{pmatrix}a_0\\a_1\end{pmatrix} = \begin{pmatrix}4\\-21\end{pmatrix}$$
4. **Determinante:** $5\cdot 73 - (-11)^2 = 365 - 121 = 244$.
5. **Inverse anwenden:**
$$\begin{pmatrix}a_0\\a_1\end{pmatrix} = \frac{1}{244}\begin{pmatrix}73 & 11\\ 11 & 5\end{pmatrix}\begin{pmatrix}4\\-21\end{pmatrix} = \frac{1}{244}\begin{pmatrix}292 - 231\\ 44 - 105\end{pmatrix} = \frac{1}{244}\begin{pmatrix}61\\-61\end{pmatrix}$$

## Ergebnis
$$\hat a_0 = 0{,}25, \qquad \hat a_1 = -0{,}25, \qquad \hat y = 0{,}25 - 0{,}25\,x$$
Deckt sich mit dem Notebook-Output `array([ 0.25, -0.25])`.

**Kontrolle:** Die Regressionsgerade geht durch den Schwerpunkt $(\bar x, \bar y) = (-2{,}2;\ 0{,}8)$: $0{,}25 - 0{,}25\cdot(-2{,}2) = 0{,}8$ ✓.

> [!warning] Typischer Fehler
> - Vorzeichen in der 2×2-Inversen: Diagonale tauschen, Nebendiagonale negieren.
> - $\sum x^2$ mit $(\sum x)^2$ verwechseln.

## Variante mit Ridge
Mit Strafe $\lambda\|a\|^2$ (ohne $1/N$) wird $X^TX$ zu $X^TX + \lambda I$:

| $\lambda$ | $a_0$ | $a_1$ | $\lVert a\rVert_2$ |
|---|---|---|---|
| 0 | 0,250 | −0,250 | 0,354 |
| 1 | 0,201 | −0,254 | 0,324 |
| 10 | 0,090 | −0,241 | 0,257 |
| 100 | 0,026 | −0,120 | 0,123 |
Auffällig: Bei $\lambda = 1$ wird $\lvert a_1\rvert$ sogar **größer** – Ridge verkleinert die Norm des Vektors, nicht jeden einzelnen Koeffizienten (siehe [[ridge-regression]]).

## Benötigte Konzepte
- [[lineare-regression]]
- [[ridge-regression]]

## Quellen
- [[q-nb01-linear-regression|NB1]]
- [[q-vl01-glm-basisfunktionen-splines|VL1]], Folien 20–23
