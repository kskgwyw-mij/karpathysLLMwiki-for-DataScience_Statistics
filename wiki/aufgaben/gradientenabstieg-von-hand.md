---
typ: aufgabe
themen: [optimierung]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Gradientenabstieg, Momentum und Newton von Hand

> [!info] Selbst erstellte Übungsaufgabe
> Die Update-Regeln stammen aus [[q-vl00-essentiell|VL0]], F. 78–91; die Zahlenbeispiele sind ergänzt und mit Code nachgerechnet (`notebooks/optimierer-vergleich.ipynb`).

## Teil A · Lernrate
**Aufgabe.** $f(\theta) = \theta^2$, $\theta_0 = 2$. Führen Sie vier GD-Schritte mit $\eta \in \{0{,}1;\ 0{,}5;\ 0{,}9;\ 1{,}1\}$ durch. Für welche $\eta$ konvergiert das Verfahren?

**Lösung.** $\nabla f = 2\theta$ ⇒ $\theta_{t+1} = \theta_t - 2\eta\theta_t = (1 - 2\eta)\,\theta_t$.

| $\eta$ | Faktor $1-2\eta$ | $\theta_1$ | $\theta_2$ | $\theta_3$ | $\theta_4$ |
|---|---|---|---|---|---|
| 0,1 | 0,8 | 1,6 | 1,28 | 1,024 | 0,819 |
| 0,5 | 0 | 0 | 0 | 0 | 0 |
| 0,9 | −0,8 | −1,6 | 1,28 | −1,024 | 0,819 |
| 1,1 | −1,2 | −2,4 | 2,88 | −3,456 | 4,147 |
Konvergenz genau für $\lvert 1 - 2\eta\rvert < 1 \Leftrightarrow 0 < \eta < 1$. Bei $\eta = 0{,}9$ oszilliert das Verfahren, bei $\eta > 1$ divergiert es.

## Teil B · Momentum
**Aufgabe.** Gleiche Funktion, $\theta_0 = 2$, $\eta = 0{,}1$, $\beta = 0{,}9$, $v_0 = 0$. Vier Schritte mit $v_{t+1} = \beta v_t + \nabla f(\theta_t)$, $\theta_{t+1} = \theta_t - \eta v_{t+1}$.

**Lösung.**

| $t$ | $\theta_t$ | $g_t = 2\theta_t$ | $v_{t+1} = 0{,}9\,v_t + g_t$ | $\theta_{t+1} = \theta_t - 0{,}1\,v_{t+1}$ |
|---|---|---|---|---|
| 0 | 2 | 4 | 4 | 1,6 |
| 1 | 1,6 | 3,2 | 6,8 | 0,92 |
| 2 | 0,92 | 1,84 | 7,96 | 0,124 |
| 3 | 0,124 | 0,248 | 7,412 | −0,617 |
Der angesammelte Schwung trägt über das Minimum hinaus. In 1D bringt Momentum hier nichts – sein Nutzen zeigt sich in **schlecht konditionierten** Problemen (Teil C).

## Teil C · Konditionierung und Newton
**Aufgabe.** $f(\theta) = (\theta_1 - 1)^2 + 10(\theta_2 + 2)^2$, Start $\theta_0 = (0, 0)$.
1. Bestimmen Sie Gradient und Hesse-Matrix.
2. Welche Lernraten sind für GD zulässig? Rechnen Sie mit $\eta = 0{,}05$.
3. Führen Sie einen Newton-Schritt durch.

**Lösung.**
1. $\nabla f = \big(2(\theta_1 - 1);\ 20(\theta_2 + 2)\big)$, also $\nabla f(0,0) = (-2;\ 40)$. $H = \mathrm{diag}(2, 20)$.
2. Koordinatenweise $\theta_{1,t+1} - 1 = (1 - 2\eta)(\theta_{1,t} - 1)$ und $\theta_{2,t+1} + 2 = (1 - 20\eta)(\theta_{2,t} + 2)$. Beide Faktoren betragsmäßig < 1 ⇒ $0 < \eta < 0{,}1$ (begrenzt durch die **steile** Richtung). Mit $\eta = 0{,}05$: $\theta_2$ ist nach einem Schritt exakt $-2$ (Faktor 0), $\theta_1$ nähert sich mit Faktor 0,9 – nach 10 Schritten erst $1 - 0{,}9^{10}\approx 0{,}651$. Die flache Richtung bremst.
3. $\theta_1 = \theta_0 - H^{-1}\nabla f(\theta_0) = (0,0) - \big(\tfrac{-2}{2};\ \tfrac{40}{20}\big) = (1;\ -2)$ → exaktes Minimum in **einem** Schritt.

> [!warning] Typischer Fehler
> - Beim Newton-Schritt $\theta_t$ vergessen (Folienfehler, siehe [[newton-und-quasi-newton]]).
> - Die zulässige Lernrate an der flachen statt an der steilen Richtung festmachen.

## Benötigte Konzepte
- [[gradientenabstieg]]
- [[momentum-und-nesterov]]
- [[newton-und-quasi-newton]]
- [[optimierung-grundlagen]]

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 78–83, 91
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 32–37, 55–57
