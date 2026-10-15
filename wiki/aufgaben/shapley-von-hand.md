---
typ: aufgabe
themen: [xai, shap]
quellen: ["[[q-vl06-xai-lokal-fairness]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Shapley-Werte von Hand

> [!beispiel] Selbst erstellte Übungsaufgabe
> Formel aus [[q-vl06-xai-lokal-fairness|VL6]], F. 7; Zahlen ergänzt und mit Code nachgerechnet (`notebooks/xai-fairness-handrechnungen.ipynb`).

## Aufgabenstellung
Ein Modell mit drei Merkmalen liefert für einen Fall die folgenden Vorhersagen, je nachdem, welche Merkmale „bekannt" sind (die übrigen werden durch die Baseline ersetzt):

| $S$ | $\varnothing$ | $\{1\}$ | $\{2\}$ | $\{3\}$ | $\{1,2\}$ | $\{1,3\}$ | $\{2,3\}$ | $\{1,2,3\}$ |
|---|---|---|---|---|---|---|---|---|
| $f(S)$ | 10 | 14 | 12 | 10 | 18 | 15 | 13 | 20 |

Berechnen Sie $\phi_1, \phi_2, \phi_3$ und prüfen Sie die Effizienzeigenschaft.

## Lösungsweg
**Gewichte** für $p = 3$: $w(k) = \frac{k!\,(3-k-1)!}{3!}$ ⇒ $w(0) = \frac{1\cdot 2}{6} = \frac13$, $w(1) = \frac{1\cdot 1}{6} = \frac16$, $w(2) = \frac{2\cdot 1}{6} = \frac13$.

**Merkmal 1** – Teilmengen $S\subseteq\{2,3\}$:

| $S$ | $w$ | $f(S\cup\{1\}) - f(S)$ |
|---|---|---|
| $\varnothing$ | 1/3 | 14 − 10 = 4 |
| $\{2\}$ | 1/6 | 18 − 12 = 6 |
| $\{3\}$ | 1/6 | 15 − 10 = 5 |
| $\{2,3\}$ | 1/3 | 20 − 13 = 7 |

$\phi_1 = \frac43 + 1 + \frac56 + \frac73 = 5{,}5$

**Merkmal 2** – Teilmengen $S\subseteq\{1,3\}$: Beiträge $12-10 = 2$ (1/3), $18-14 = 4$ (1/6), $13-10 = 3$ (1/6), $20-15 = 5$ (1/3)
$\phi_2 = \frac23 + \frac46 + \frac36 + \frac53 = 3{,}5$

**Merkmal 3** – Teilmengen $S\subseteq\{1,2\}$: Beiträge $10-10 = 0$ (1/3), $15-14 = 1$ (1/6), $13-12 = 1$ (1/6), $20-18 = 2$ (1/3)
$\phi_3 = 0 + \frac16 + \frac16 + \frac23 = 1{,}0$

## Ergebnis
$$\phi_1 = 5{,}5,\quad \phi_2 = 3{,}5,\quad \phi_3 = 1{,}0, \qquad \sum\phi_i = 10 = f(\{1,2,3\}) - f(\varnothing) = 20 - 10 \;\checkmark$$
Lokale Zerlegung: $f(x) = 20 = \underbrace{10}_{\text{Baseline}} + 5{,}5 + 3{,}5 + 1{,}0$.

**Interpretation:** Merkmal 3 bringt allein nichts ($f(\{3\}) = f(\varnothing)$), wirkt aber in Kombination mit den anderen – Shapley-Werte verteilen auch Interaktionsbeiträge fair.

> [!warning] Typischer Fehler
> - Gewichte vertauschen: $w(0)$ und $w(p-1)$ sind gleich groß und am größten.
> - Nur den Beitrag „zur leeren Menge" oder „zu allen anderen" nehmen (das ist die Lehr-Approximation aus NB7, nicht SHAP).

## Variante: lineares Modell
Für $f(x) = \beta_0 + \sum_j\beta_jx_j$ mit unabhängigen Merkmalen und Baseline $\mathbb E[x]$ gilt $\phi_j = \beta_j\,(x_j - \mathbb E[x_j])$. Beispiel: $\beta = (2;\,-1)$, $x = (3;\,1)$, $\mathbb E[x] = (1;\,2)$ ⇒ $\phi = (4;\ 1)$.

## Benötigte Konzepte
- [[shap]]

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folie 7
