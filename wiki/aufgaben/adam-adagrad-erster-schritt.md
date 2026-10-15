---
typ: aufgabe
themen: [optimierung]
quellen: ["[[q-vl04-map-resampling-optimierung]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Erste Schritte von AdaGrad und Adam

> [!beispiel] Selbst erstellte Übungsaufgabe
> Formeln aus [[q-vl04-map-resampling-optimierung|VL4]], F. 46–51; Zahlen ergänzt und nachgerechnet.

## Teil A · AdaGrad skaliert pro Koordinate
**Aufgabe.** Zwei Parameter, konstanter Gradient $g = (4;\ 0{,}5)$ in jedem Schritt, $\eta = 0{,}1$, $\varepsilon\approx 0$, $G_0 = 0$. Berechnen Sie die Schritte $\eta\, g_t/\sqrt{G_t}$ für $t = 1, 2$.

**Lösung.**

| $t$ | $G_t$ | Schritt Koordinate 1 | Schritt Koordinate 2 |
|---|---|---|---|
| 1 | $(16;\ 0{,}25)$ | $0{,}1\cdot 4/4 = 0{,}1$ | $0{,}1\cdot 0{,}5/0{,}5 = 0{,}1$ |
| 2 | $(32;\ 0{,}5)$ | $0{,}1\cdot 4/\sqrt{32}\approx 0{,}0707$ | $0{,}1\cdot 0{,}5/\sqrt{0{,}5}\approx 0{,}0707$ |

**Interpretation:** Obwohl die Gradienten sich um den Faktor 8 unterscheiden, macht AdaGrad in beiden Koordinaten **gleich große** Schritte – das ist die diagonale Vorkonditionierung. Bei konstantem Gradienten schrumpfen die Schritte wie $1/\sqrt t$ → die Lernrate fällt monoton (Hauptschwäche, motiviert AdaDelta).

## Teil B · Adam im ersten Schritt
**Aufgabe.** Adam mit Defaults $\alpha = 0{,}001$, $\beta_1 = 0{,}9$, $\beta_2 = 0{,}999$, $\varepsilon = 10^{-8}$, $m_0 = v_0 = 0$. Berechnen Sie den ersten Schritt für $g_1 \in \{0{,}001;\ 5;\ -100\}$. Was wäre ohne Bias-Korrektur?

**Lösung.**
$$m_1 = 0{,}1\,g_1,\quad v_1 = 0{,}001\,g_1^2,\quad \hat m_1 = \frac{m_1}{1 - 0{,}9} = g_1,\quad \hat v_1 = \frac{v_1}{1 - 0{,}999} = g_1^2$$
$$\Delta\theta_1 = -\alpha\,\frac{g_1}{\sqrt{g_1^2 + \varepsilon}} \approx -\alpha\,\mathrm{sign}(g_1)$$

| $g_1$ | $\Delta\theta_1$ |
|---|---|
| 0,001 | −0,000995 |
| 5 | −0,001 |
| −100 | +0,001 |
Der erste Schritt hat Betrag ≈ $\alpha$, **egal wie groß der Gradient ist**.

Ohne Bias-Korrektur: $m_1/\sqrt{v_1} = 0{,}1\,g_1/(\sqrt{0{,}001}\,\lvert g_1\rvert)\approx 3{,}16\,\mathrm{sign}(g_1)$ → der erste Schritt wäre gut dreimal so groß wie beabsichtigt. Die Korrektur gleicht die Nullinitialisierung von $m$ und $v$ aus.

> [!warning] Typischer Fehler
> $\beta_1^t$ bzw. $\beta_2^t$ als $\beta\cdot t$ lesen – es ist die **Potenz** $\beta^t$.

## Benötigte Konzepte
- [[adaptive-optimierer]]
- [[momentum-und-nesterov]]

## Quellen
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 46–51
