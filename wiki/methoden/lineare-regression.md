---
typ: methode
themen: [glm, regression]
aliases: [Linear Regression, Kleinste Quadrate, Least Squares, Normalgleichung, OLS]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl01-glm-basisfunktionen-splines]]", "[[q-nb01-linear-regression]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Lineare Regression

#flashcards/statistik

## Kurzdefinition
Modell, bei dem die Ausgabe **linear** von den Eingaben abhängt: $y_j = a_0 + a_1 x_j^1 + \dots + a_n x_j^n + \varepsilon_j$. Die Koeffizienten werden durch Minimieren der Fehlerquadratsumme bestimmt: $\hat a = (X^TX)^{-1}X^Ty$ (→ [[q-vl00-essentiell|VL0]], F. 7–10).

## Intuition
Man sucht die Gerade (Ebene, Hyperebene), deren senkrechte Abstände zu den Datenpunkten im Quadrat möglichst klein sind. Die Lösung ist geschlossen berechenbar – kein iteratives Verfahren nötig.

## Formal

**Matrixform.** Mit $x_j^0 = 1$ (Einsen-Spalte für den Achsenabschnitt) gilt $y_j = (x_j)^T a + \varepsilon_j$, für alle $K$ Beobachtungen zusammen
$$y = Xa + \varepsilon, \qquad X = \begin{pmatrix} 1 & x_1^1 & \cdots & x_1^n \\ 1 & x_2^1 & \cdots & x_2^n \\ \vdots & & & \vdots \\ 1 & x_K^1 & \cdots & x_K^n\end{pmatrix} \in \mathbb R^{K\times(n+1)}$$
$X$ heißt **Designmatrix** (Regressormatrix). Das sind $K$ Gleichungen mit $n+1$ Unbekannten (→ [[q-vl01-glm-basisfunktionen-splines|VL1]], F. 20–21).

**Zielfunktion.**
$$L(a) = \|y - Xa\|^2$$

**Herleitung der Normalgleichung** (→ VL1, F. 22–23):
$$\frac{\partial L}{\partial a}(a) = -2X^Ty + 2X^TXa \overset{!}{=} 0 \quad\Longrightarrow\quad X^TXa = X^Ty \quad\Longrightarrow\quad \hat a = (X^TX)^{-1}X^Ty$$

> [!tip] Merke
> Normalgleichung: $X^TX\,a = X^Ty$. Im Notebook wird sie mit `np.linalg.solve(XTX, XTy)` gelöst, nicht über die explizite Inverse (→ [[q-nb01-linear-regression|NB1]]).

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Herleitung der Ableitung: $L(a) = y^Ty - 2a^TX^Ty + a^TX^TXa$. Mit $\nabla_a(a^Tb) = b$ und $\nabla_a(a^TMa) = 2Ma$ für symmetrisches $M$ folgt die Formel oben. $X^TX$ ist invertierbar, wenn die Spalten von $X$ linear unabhängig sind (insbesondere braucht man $K \ge n+1$). Da $L$ konvex ist, ist der stationäre Punkt das globale Minimum ([[optimierung-grundlagen]]).

**Für $\{1, x\}$ (einfache Regression)** sind die Einträge Summen:
$$X^TX = \begin{pmatrix} K & \sum x_j \\ \sum x_j & \sum x_j^2\end{pmatrix}, \qquad X^Ty = \begin{pmatrix}\sum y_j \\ \sum x_j y_j\end{pmatrix}$$

## Annahmen & Voraussetzungen
- Modell linear in den **Koeffizienten** $a$ (nicht zwingend in $x$ → [[basisfunktionen-regression]]).
- $X^TX$ invertierbar (keine perfekte Kollinearität).
- Fehler mit Mittelwert 0; die probabilistische Deutung (Gauß-Rauschen ⇒ quadratischer Fehler = Maximum Likelihood) steht in [[probabilistische-sicht-map]].

## Beispiel
Daten aus [[q-nb01-linear-regression|NB1]]: $x = (-7,-4,-2,0,2)$, $y = (1,2,2,0,-1)$.
$$X^TX = \begin{pmatrix}5 & -11\\-11 & 73\end{pmatrix}, \quad X^Ty = \begin{pmatrix}4\\-21\end{pmatrix}, \quad \hat a = \begin{pmatrix}0{,}25\\-0{,}25\end{pmatrix}$$
Vollständiger Rechenweg: [[lineare-regression-normalgleichungen]]. Notebook: `notebooks/kapitel-1-regression-splines.ipynb`.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – Residuen prüfen
> Mit $\hat a = (0{,}25;\,-0{,}25)$ aus den NB1-Daten ist $\hat y = 0{,}25 - 0{,}25\,x$:
>
> | $x$ | −7 | −4 | −2 | 0 | 2 |
> |---|---|---|---|---|---|
> | $y$ | 1 | 2 | 2 | 0 | −1 |
> | $\hat y$ | 2 | 1,25 | 0,75 | 0,25 | −0,25 |
> | $r = y - \hat y$ | −1 | 0,75 | 1,25 | −0,25 | −0,75 |
>
> - $\sum r_j = 0$ und $\sum x_j r_j = 7 - 3 - 2{,}5 + 0 - 1{,}5 = 0$ – das ist genau die Normalgleichung $X^T(y - X\hat a) = 0$: Die Residuen stehen senkrecht auf jeder Spalte von $X$. **Schnelltest in der Klausur:** Ist eine der beiden Summen ≠ 0, ist $\hat a$ falsch.
> - Fehlerquadratsumme $L(\hat a) = 1 + 0{,}5625 + 1{,}5625 + 0{,}0625 + 0{,}5625 = 3{,}75$.
> - Vorhersage für $x = 4$: $\hat y = 0{,}25 - 1 = -0{,}75$.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Heizenergie und Außentemperatur
> Ein Gebäude-Monitoring schätzt aus Tagesdaten $\hat y = 60 - 2{,}5\,T$ (Heizenergie in kWh, Außentemperatur $T$ in °C).
> - $a_1 = -2{,}5$: Jedes Grad wärmer spart im Mittel 2,5 kWh pro Tag. $a_0 = 60$: erwarteter Verbrauch bei 0 °C.
> - Vorhersage bei 10 °C: $60 - 25 = 35$ kWh.
> - Bei 30 °C ergäbe sich −15 kWh – unsinnig. Das Modell gilt nur im Bereich der Trainingsdaten (Heizperiode); **Extrapolation** ist eine typische Fehlerquelle. Ein nichtlinearer Verlauf (ab ca. 18 °C kein Heizen mehr) ließe sich mit [[basisfunktionen-regression]] abbilden.

## Typische Prüfungsfragen & Fallstricke
- **Normalgleichung herleiten** (Ableitung, Nullsetzen, Auflösen) – klassische Papieraufgabe.
- **2×2-System von Hand lösen**: Inverse $\begin{pmatrix}a&b\\c&d\end{pmatrix}^{-1} = \frac{1}{ad-bc}\begin{pmatrix}d&-b\\-c&a\end{pmatrix}$.
- Einsen-Spalte vergessen → Modell ohne Achsenabschnitt.
- Verwechslung $X^TX$ ($(n{+}1)\times(n{+}1)$) mit $XX^T$ ($K\times K$).
- Hochgestellte Indizes sind Komponenten, keine Potenzen.

## Verwandt
- [[statistisches-lernproblem]] – lineares $f$ als Spezialfall
- [[basisfunktionen-regression]] – gleiche Lösung mit Basismatrix $B$ statt $X$
- [[ridge-regression]] – addiert $\lambda I$ zu $X^TX$
- [[gradientenabstieg]] – iterative Alternative bei sehr großen Problemen
- [[transparente-modelle]] – Koeffizienten als Erklärung

## Karteikarten
Wie lautet die Normalgleichung der linearen Regression?::$X^TX\,a = X^Ty$, also $\hat a = (X^TX)^{-1}X^Ty$.
Wie lautet die Zielfunktion der linearen Regression?::$L(a) = \|y - Xa\|^2$.
Wie lautet der Gradient von $L(a)=\|y-Xa\|^2$?::$\nabla L(a) = -2X^Ty + 2X^TXa$.
Wozu dient die Einsen-Spalte in der Designmatrix?::Sie liefert den Achsenabschnitt $a_0$ ($x^0 = 1$).
Welche Dimension hat die Designmatrix bei $K$ Beobachtungen und $n$ Merkmalen?::$K \times (n+1)$.
Wann ist $X^TX$ invertierbar?::Wenn die Spalten von $X$ linear unabhängig sind (keine perfekte Kollinearität, $K \ge n+1$).
Warum `np.linalg.solve` statt `inv`?::Löst das lineare System direkt, numerisch stabiler und günstiger als die explizite Inverse.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 7–10
- [[q-vl01-glm-basisfunktionen-splines|VL1]], Folien 19–23
- [[q-nb01-linear-regression|NB1]]
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 3.2, S. 44
