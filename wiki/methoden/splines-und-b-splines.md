---
typ: methode
themen: [glm, basisfunktionen, splines]
aliases: [Splines, B-Splines, Cox-de-Boor, Kubische Splines, Spline-Regression]
quellen: ["[[q-vl01-glm-basisfunktionen-splines]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Splines und B-Splines

#flashcards/statistik

> [!info] Relevanz
> Splines stehen nur in VL1, nicht in der Auswahl „essentiell". Da die B-Spline-Übung aber eine typische Papieraufgabe ist, lohnt sie sich trotzdem.

## Kurzdefinition
Ein **Spline** vom Grad $d$ ist eine stückweise polynomiale Funktion, deren Stücke an den **Knoten** $t_1 < \dots < t_k$ glatt aneinandergesetzt sind. **B-Splines** sind spezielle Basisfunktionen, aus denen sich jeder Spline als Linearkombination $S(x) = \sum_j a_j B_j(x)$ bauen lässt (→ [[q-vl01-glm-basisfunktionen-splines|VL1]], F. 31–34).

## Intuition
Ein globales Polynom hohen Grades schwingt an den Rändern stark. Splines verwenden stattdessen viele **lokale** Polynome niedrigen Grades. B-Splines sind kleine „Hügel", die jeweils nur über wenigen Knotenintervallen ungleich null sind – ändert man einen Koeffizienten, ändert sich die Kurve nur lokal.

## Formal

### Spline-Definition (→ VL1, F. 32)
$$S(x) = \begin{cases} P_1(x) & x \le t_1 \\ P_2(x) & t_1 < x \le t_2 \\ \ \vdots \\ P_{k+1}(x) & x > t_k\end{cases}$$
mit Polynomen $P_i$ vom Grad $\le d$, z. B. für $d = 3$: $P_i(x) = a_i + b_ix + c_ix^2 + d_ix^3$.

**Glattheit** an den Knoten (→ VL1, F. 31): gleiche Funktionswerte (Glattheit 0. Grades), gleiche erste Ableitungen (1. Grades), gleiche höhere Ableitungen (höheren Grades).

**Kubische Spline-Regression** (→ VL1, F. 33): $y = a_0 + \sum_{j=1}^p a_j B_j(x) + \varepsilon$ mit Spline-Basisfunktionen $B_j$ – also eine [[basisfunktionen-regression]].

### B-Splines rekursiv (→ VL1, F. 36–39)
**Grad 0** – Treppenfunktionen:
$$B_{i,0}(x) = \begin{cases}1 & t_i \le x < t_{i+1}\\ 0 & \text{sonst}\end{cases}$$

**Grad $d$** – Cox-de-Boor-Rekursion:
$$B_{i,d}(x) = \frac{x - t_i}{t_{i+d} - t_i}\,B_{i,d-1}(x) + \frac{t_{i+d+1} - x}{t_{i+d+1} - t_{i+1}}\,B_{i+1,d-1}(x)$$

Die beiden Gewichte sind lineare Rampen (→ VL1, F. 38):
- $\frac{x-t_i}{t_{i+d}-t_i}$ ist 0 bei $x = t_i$ und 1 bei $x = t_{i+d}$ (steigend),
- $\frac{t_{i+d+1}-x}{t_{i+d+1}-t_{i+1}}$ ist 1 bei $x = t_{i+1}$ und 0 bei $x = t_{i+d+1}$ (fallend).

Die Rekursion bildet also gewichtete Mittel zweier B-Splines niedrigeren Grades.

### Eigenschaften (→ VL1, F. 40)
| Eigenschaft | Bedeutung |
|---|---|
| Kompakter Träger | $B_{i,d}$ ist nur auf $[t_i, t_{i+d+1})$, also $d+1$ Knotenintervallen, ungleich 0 → für jedes $x$ tragen nur wenige B-Splines bei |
| Zerlegung der Eins | $\sum_j B_j(x) = 1$ |
| Nichtnegativität | $B_j(x) \ge 0$ |
| Stetigkeit | gewünschte Glattheit automatisch; kubische B-Splines: Funktion, 1. und 2. Ableitung stetig |

> [!ergaenzung] Ergänzung (nicht aus der VL)
> - Mit $k$ Knoten gibt es $k - d - 1$ B-Splines vom Grad $d$.
> - Die **Zerlegung der Eins** gilt nur dort, wo $d+1$ B-Splines überlappen. Bei nur 4 Knoten (Übung) gibt es einen einzigen quadratischen B-Spline, und $B_{1,2}(1{,}5) = 0{,}75$. In der Praxis werden die Randknoten deshalb mehrfach wiederholt; dabei gilt in der Rekursion die Konvention $0/0 = 0$.
> - Bei einfachen Knoten ist ein Spline vom Grad $d$ an den Knoten $(d-1)$-mal stetig differenzierbar.

### Matrixform (→ VL1, F. 42–43)
$$y = Ba + \varepsilon, \quad B \in \mathbb R^{n\times p},\ B_{kj} = B_j(x_k), \qquad \hat a = (B^TB)^{-1}B^Ty$$
($n$ Beobachtungen, $p$ B-Spline-Basisfunktionen).

## Annahmen & Voraussetzungen
- Anzahl und Lage der Knoten sind vorher gewählt (Hyperparameter → Komplexität, [[bias-varianz-tradeoff]]).

## Beispiel
Knoten $t_1 = 0, t_2 = 1, t_3 = 2, t_4 = 3$, Grad 2 (Übung VL1, F. 41):
$$B_{1,2}(x) = \begin{cases} x^2/2 & 0 \le x < 1\\ (-2x^2 + 6x - 3)/2 & 1 \le x < 2 \\ (3-x)^2/2 & 2 \le x < 3 \\ 0 & \text{sonst}\end{cases}$$
Vollständiger Rechenweg über Grad 0 und Grad 1: [[b-splines-grad-2-von-hand]]. Plot: `notebooks/kapitel-1-regression-splines.ipynb`.

## Typische Prüfungsfragen & Fallstricke
- **B-Splines von Hand rekursiv berechnen** – erst alle $B_{i,0}$, dann $B_{i,1}$, dann $B_{i,2}$; immer intervallweise.
- Gewichte der Rekursion erklären (zwei lineare Rampen).
- Eigenschaften aufzählen (Träger, Zerlegung der Eins, Nichtnegativität, Stetigkeit).
- Fallstrick: Indexverschiebung – $B_{i,d}$ nutzt die Knoten $t_i, \dots, t_{i+d+1}$.
- Fallstrick: Intervalle halboffen $[t_i, t_{i+1})$.

## Verwandt
- [[basisfunktionen-regression]] – Splines sind eine spezielle Basis
- [[lineare-regression]] – gleiche Normalgleichung
- [[output-regularisierung]] – Glattheit kann alternativ über einen Strafterm erzwungen werden
- [[bias-varianz-tradeoff]] – mehr Knoten = flexibleres Modell

## Karteikarten
Was ist ein Spline vom Grad $d$?::Eine stückweise polynomiale Funktion (Stücke vom Grad $\le d$), die an den Knoten glatt verbunden ist.
Wie sind B-Splines vom Grad 0 definiert?::$B_{i,0}(x) = 1$ für $t_i \le x < t_{i+1}$, sonst 0.
Wie lautet die Cox-de-Boor-Rekursion?::$B_{i,d} = \frac{x-t_i}{t_{i+d}-t_i}B_{i,d-1} + \frac{t_{i+d+1}-x}{t_{i+d+1}-t_{i+1}}B_{i+1,d-1}$.
Über wie viele Knotenintervalle ist $B_{i,d}$ ungleich null?::Über $d+1$ Intervalle, nämlich $[t_i, t_{i+d+1})$.
Nenne die vier Eigenschaften von B-Splines aus der VL.::Kompakter Träger, Zerlegung der Eins, Nichtnegativität, Stetigkeit an den Knoten.
Welche Ableitungen sind bei kubischen B-Splines stetig?::Funktion, erste und zweite Ableitung.
Wie lautet der quadratische B-Spline zu den Knoten 0,1,2,3 auf $[1,2)$?::$(-2x^2+6x-3)/2$.

## Quellen
- [[q-vl01-glm-basisfunktionen-splines|VL1]], Folien 31–43
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 5.2 (S. 141) und Appendix B-splines (S. 186)
