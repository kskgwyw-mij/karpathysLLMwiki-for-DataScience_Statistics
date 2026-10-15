---
typ: aufgabe
themen: [xai, klassifikation]
quellen: ["[[q-vl05-xai-transparente-globale-modelle]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Odds Ratio und Wahrscheinlichkeitseffekte

> [!info] Selbst erstellte Übungsaufgabe
> Zur Aussage aus [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 12: „Dasselbe $\beta_j$ erzeugt je nach Ausgangswahrscheinlichkeit unterschiedliche Wahrscheinlichkeitsänderungen."

## Aufgabenstellung
In einer logistischen Regression ist $\beta_j = 0{,}7$.
a) Interpretieren Sie $e^{\beta_j}$.
b) Wie ändert sich die Wahrscheinlichkeit, wenn $x_j$ um 1 steigt und die Ausgangswahrscheinlichkeit 0,1, 0,5 bzw. 0,9 ist?

## Lösungsweg
a) $e^{0{,}7}\approx 2{,}01$: Die **Odds** $p/(1-p)$ verdoppeln sich ungefähr pro Einheit $x_j$ (bei sonst gleichen Merkmalen).

b) Odds berechnen, mit 2,01 multiplizieren, zurückrechnen $p = \text{Odds}/(1 + \text{Odds})$:

| $p_\text{alt}$ | Odds alt | Odds neu | $p_\text{neu}$ | $\Delta p$ |
|---|---|---|---|---|
| 0,1 | 0,111 | 0,224 | 0,183 | +0,083 |
| 0,5 | 1 | 2,014 | 0,668 | +0,168 |
| 0,9 | 9 | 18,12 | 0,948 | +0,048 |

## Ergebnis
Gleicher Koeffizient, Wahrscheinlichkeitseffekt zwischen 4,8 und 16,8 Prozentpunkten – am größten bei $p = 0{,}5$. Deshalb empfiehlt die VL, **marginale Wahrscheinlichkeitsänderungen bei repräsentativen Fällen** zu berichten.

> [!warning] Typischer Fehler
> „Die Wahrscheinlichkeit verdoppelt sich" – nein, die **Odds** verdoppeln sich.

## Benötigte Konzepte
- [[logistische-regression]]

## Quellen
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folie 12
