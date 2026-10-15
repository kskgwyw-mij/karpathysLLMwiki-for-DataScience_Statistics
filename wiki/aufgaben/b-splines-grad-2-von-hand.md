---
typ: aufgabe
themen: [splines]
quellen: ["[[q-vl01-glm-basisfunktionen-splines]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · B-Splines vom Grad 2 von Hand

## Aufgabenstellung
Gegeben sind die Knoten $t_1 = 0,\ t_2 = 1,\ t_3 = 2,\ t_4 = 3$. Berechnen Sie die B-Spline-Basisfunktionen vom Grad $d = 2$ mithilfe der Definition der B-Splines vom Grad 0 und der Rekursionsformel. Skizzieren Sie die Funktionen. (→ [[q-vl01-glm-basisfunktionen-splines|VL1]], F. 41)

## Lösungsweg

**Schritt 1 – Grad 0** ($k - d - 1 = 4 - 0 - 1 = 3$ Funktionen):
$$B_{1,0} = \mathbb 1_{[0,1)}, \quad B_{2,0} = \mathbb 1_{[1,2)}, \quad B_{3,0} = \mathbb 1_{[2,3)}$$

**Schritt 2 – Grad 1** (2 Funktionen). Alle Knotenabstände sind 1, die Nenner also 1.
$$B_{1,1}(x) = \frac{x-0}{1-0}B_{1,0} + \frac{2-x}{2-1}B_{2,0} = \begin{cases} x & [0,1)\\ 2-x & [1,2)\end{cases}$$
$$B_{2,1}(x) = \frac{x-1}{1}B_{2,0} + \frac{3-x}{1}B_{3,0} = \begin{cases} x-1 & [1,2)\\ 3-x & [2,3)\end{cases}$$
Das sind zwei „Hütchen" mit Spitze 1 bei $x = 1$ bzw. $x = 2$.

**Schritt 3 – Grad 2** (1 Funktion). Nenner: $t_3 - t_1 = 2$ und $t_4 - t_2 = 2$.
$$B_{1,2}(x) = \frac{x - 0}{2}B_{1,1}(x) + \frac{3 - x}{2}B_{2,1}(x)$$

Intervallweise einsetzen:
- $[0,1)$: nur $B_{1,1} = x$ aktiv → $\frac{x}{2}\cdot x = \frac{x^2}{2}$
- $[1,2)$: $\frac{x}{2}(2-x) + \frac{3-x}{2}(x-1) = \frac{2x - x^2 + 4x - 3 - x^2}{2} = \frac{-2x^2 + 6x - 3}{2}$
- $[2,3)$: nur $B_{2,1} = 3-x$ aktiv → $\frac{3-x}{2}(3-x) = \frac{(3-x)^2}{2}$

## Ergebnis
$$B_{1,2}(x) = \begin{cases} x^2/2 & 0 \le x < 1\\ (-2x^2 + 6x - 3)/2 & 1 \le x < 2 \\ (3-x)^2/2 & 2 \le x < 3 \\ 0 & \text{sonst}\end{cases}$$

**Kontrollwerte:** $B_{1,2}(0{,}5) = 0{,}125$, $B_{1,2}(1) = 0{,}5$, $B_{1,2}(1{,}5) = 0{,}75$ (Maximum), $B_{1,2}(2) = 0{,}5$, $B_{1,2}(2{,}5) = 0{,}125$.

**Glattheitsprüfung bei $x = 1$:** linkes Stück $x^2/2 \to 0{,}5$, Ableitung $x \to 1$; rechtes Stück $\to (-2+6-3)/2 = 0{,}5$, Ableitung $(-4x+6)/2 \to 1$. Wert und erste Ableitung stimmen überein ✓ (quadratischer Spline ist $C^1$).

> [!warning] Typischer Fehler
> - Nenner aus den falschen Knoten: für $B_{i,d}$ sind es $t_{i+d}-t_i$ und $t_{i+d+1}-t_{i+1}$, bei Grad 2 hier also jeweils 2, nicht 1.
> - Nicht intervallweise rechnen und dadurch Terme vergessen, die auf dem Intervall null sind.
> - Erwarten, dass sich die B-Splines zu 1 summieren: Mit nur 4 Knoten gibt es einen einzigen quadratischen B-Spline (Maximum 0,75). Die Zerlegung der Eins gilt nur, wo $d+1$ Funktionen überlappen (Ergänzung, siehe [[splines-und-b-splines]]).

## Benötigte Konzepte
- [[splines-und-b-splines]]

## Quellen
- [[q-vl01-glm-basisfunktionen-splines|VL1]], Folien 36–41
- Nachgerechnet in `notebooks/kapitel-1-regression-splines.ipynb`
