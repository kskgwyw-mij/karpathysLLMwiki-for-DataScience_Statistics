---
typ: methode
themen: [glm, regression, basisfunktionen]
aliases: [General Linear Models, Allgemeine lineare Modelle, Basisfunktionen, Basis Functions, Basismatrix]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl01-glm-basisfunktionen-splines]]", "[[q-vl02-bias-varianz-modellwahl]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Lineare Regression mit Basisfunktionen (General Linear Models)

#flashcards/statistik

## Kurzdefinition
Nichtlineare Zusammenhänge werden modelliert, indem man die Eingabe durch frei wählbare **Basisfunktionen** $B_j$ transformiert und dann linear kombiniert: $y = a_0 + \sum_{j=1}^p a_j B_j(x) + \varepsilon$. Die optimalen Koeffizienten sind $\hat a = (B^TB)^{-1}B^Ty$ (→ [[q-vl00-essentiell|VL0]], F. 11).

## Intuition
„Linear" bezieht sich auf die **Koeffizienten**, nicht auf $x$. Man wählt vorab einen Werkzeugkasten von Funktionen (Potenzen, Glocken, Sinuswellen …) und lässt die Regression nur noch die Gewichte bestimmen. Dadurch bleibt die geschlossene Lösung der [[lineare-regression]] erhalten.

## Formal
$$y = a_0 \cdot 1 + \sum_{j=1}^p a_j B_j(x) + \varepsilon, \qquad \text{oft } B_0(x) = 1$$

**Basismatrix** – jede Zeile eine Beobachtung, jede Spalte eine Basisfunktion (→ [[q-vl01-glm-basisfunktionen-splines|VL1]], F. 29):
$$B = \begin{pmatrix} B_0(x_1) & B_1(x_1) & \cdots & B_n(x_1) \\ B_0(x_2) & B_1(x_2) & \cdots & B_n(x_2) \\ \vdots & & & \vdots \\ B_0(x_K) & B_1(x_K) & \cdots & B_n(x_K)\end{pmatrix}, \qquad \hat a = (B^TB)^{-1}B^Ty$$

### Gängige Basen (→ VL1, F. 25–28)
| Basis | Funktionen | Wählbar |
|---|---|---|
| Polynom (1D) | $1, x, x^2, x^3, \dots$ | Grad |
| Multivariates Polynom ($x = (x_1, x_2)$) | $1, x_1, x_2, x_1^2, x_1x_2, x_2^2, \dots$ | Grad |
| Gauß-RBF | $\exp\!\big(-(x-c_k)^2/(2\sigma)^2\big)$ mit Zentren $c_k$ | Anzahl, Breite $\sigma$ |
| Sigmoid | $1/\big(1+\exp(-a(x-c_k))\big)$ | Zentren, Steilheit |
| Fourier | $\sin(2\pi kx/T)$, $\cos(2\pi kx/T)$ | Anzahl Frequenzen, Periode $T$ |
| Stückweise konstant | $B_k(x) = 1$, falls $x\in I_k$ (Bins) | Intervalle |
| Splines | stückweise Polynome mit Glattheit an Knoten | Grad, Knoten → [[splines-und-b-splines]] |

> [!note] Notation
> Die Folie schreibt beim Gauß-RBF $(2\sigma)^2$ im Nenner. Üblich ist $2\sigma^2$. Für die Methode ist es egal (nur eine andere Skalierung der Breite), in der Klausur aber die Folien-Schreibweise verwenden oder den Unterschied benennen.

## Annahmen & Voraussetzungen
- Basisfunktionen sind **vor** dem Fit festgelegt (nicht gelernt).
- $B^TB$ invertierbar – bei zu vielen Basisfunktionen relativ zu den Daten nicht mehr gegeben → Regularisierung ([[ridge-regression]]).

## Beispiel
Übung aus VL1, F. 30: $x = (-3, -1, 0, 0{,}5, 4)$, $y = (3, 4, 2, -1, 2)$.
- Basis $\{1, x\}$: $\hat a \approx (2{,}025;\,-0{,}248)$, SSE ≈ 12,39
- Basis $\{1, x, x^2\}$: $\hat a \approx (1{,}421;\,-0{,}399;\,0{,}118)$, SSE ≈ 10,20

Rechenweg: [[basisfunktionen-uebung-folie-30]]. Notebook: `notebooks/kapitel-1-regression-splines.ipynb`.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – stückweise konstante Basis
> Daten $x = (0{,}5;\ 1{,}5;\ 2{,}5;\ 3{,}5)$, $y = (1, 3, 6, 8)$, zwei Bins $I_1 = [0, 2)$, $I_2 = [2, 4)$, Basis $B_1 = \mathbb 1_{I_1}$, $B_2 = \mathbb 1_{I_2}$ (ohne Einsen-Spalte):
> $$B = \begin{pmatrix}1&0\\1&0\\0&1\\0&1\end{pmatrix},\quad B^TB = \begin{pmatrix}2&0\\0&2\end{pmatrix},\quad B^Ty = \begin{pmatrix}4\\14\end{pmatrix} \;\Rightarrow\; \hat a = \begin{pmatrix}2\\7\end{pmatrix}$$
> Die Koeffizienten sind die **Mittelwerte je Bin**. Fallstrick: Nimmt man zusätzlich die Einsen-Spalte $B_0 = 1$ auf, ist $B_0 = B_1 + B_2$ – die Spalten sind linear abhängig, $B^TB$ ist **singulär** und die Normalgleichung hat keine eindeutige Lösung.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Saisonalität mit Fourier-Basis
> Monatlicher Absatz einer Eisdiele mit Trend und Jahreszyklus ($T = 12$ Monate):
> $$y_m = a_0 + a_1 m + a_2\sin\!\big(\tfrac{2\pi m}{12}\big) + a_3\cos\!\big(\tfrac{2\pi m}{12}\big) + \varepsilon_m$$
> Die Zeile der Basismatrix für März ($m = 3$) ist $(1,\ 3,\ 1,\ 0)$, für Juni ($m = 6$) $(1,\ 6,\ 0,\ -1)$. Obwohl die Kurve wellenförmig ist, bleibt es eine lineare Regression in $a$ – gelöst mit derselben Normalgleichung. Wird der Sommergipfel zu spitz abgebildet, ergänzt man die nächste Frequenz ($k = 2$) – jede zusätzliche Basisfunktion erhöht aber die Varianz ([[bias-varianz-tradeoff]]).

## Typische Prüfungsfragen & Fallstricke
- **Basismatrix aufstellen** für gegebene Daten und Basen (Zeile = Datenpunkt, Spalte = Funktion).
- „Warum ist ein Polynom-Modell ein *lineares* Modell?" → linear in $a$.
- Mehr Basisfunktionen ⇒ kleinerer Trainingsfehler, aber höhere Varianz → [[bias-varianz-tradeoff]]. Ein Polynom vom Grad $K-1$ interpoliert $K$ Punkte exakt (Trainingsfehler 0).
- Bei der Folie 21 von VL1 heißt die Matrix $X$, ab Folie 29 $B$ – inhaltlich dasselbe.

## Verwandt
- [[lineare-regression]] – Spezialfall $B_j(x) = x^j$ (Komponente)
- [[splines-und-b-splines]] – wichtigste glatte Basis
- [[bias-varianz-tradeoff]] – Anzahl Basisfunktionen steuert die Komplexität
- [[regularisierung]] – wenn $p$ groß ist

## Karteikarten
Wie lautet das Modell der linearen Regression mit Basisfunktionen?::$y = a_0 + \sum_j a_j B_j(x) + \varepsilon$.
Wie berechnet man die Koeffizienten bei Basisfunktionen?::$\hat a = (B^TB)^{-1}B^Ty$ mit Basismatrix $B_{kj} = B_j(x_k)$.
Warum heißt ein Polynommodell trotzdem „lineares Modell"?::Es ist linear in den Koeffizienten $a$; die Nichtlinearität steckt in den fest gewählten Basisfunktionen.
Nenne fünf Typen von Basisfunktionen.::Polynom, Gauß-RBF, Sigmoid, Fourier, stückweise konstant (Bins), Splines.
Was wird beim Gauß-RBF gewählt?::Anzahl und Lage der Zentren $c_k$ und die Breite $\sigma$.
Was passiert, wenn man sehr viele Basisfunktionen wählt?::Trainingsfehler sinkt, Varianz steigt – Overfitting; $B^TB$ kann singulär werden.

## Quellen
- [[q-vl00-essentiell|VL0]], Folie 11
- [[q-vl01-glm-basisfunktionen-splines|VL1]], Folien 24–30
- [[q-vl02-bias-varianz-modellwahl|VL2]], Folie 1
