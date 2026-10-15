---
typ: aufgabe
themen: [resampling]
quellen: ["[[q-vl00-essentiell]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Wie viele Beobachtungen sind Out-of-Bag?

> [!info] Selbst erstellte Übungsaufgabe
> Nicht aus der Vorlesung; die Rechnung stützt den OOB-Begriff aus [[q-vl00-essentiell|VL0]], F. 67, und folgt ESL 7.11.

## Aufgabenstellung
a) Mit welcher Wahrscheinlichkeit ist eine bestimmte Beobachtung $z_j$ **nicht** in einer Bootstrap-Stichprobe der Größe $n$ enthalten?
b) Berechnen Sie den Wert für $n = 5$ und den Grenzwert für $n\to\infty$.
c) Ein Datensatz hat 1 000 Beobachtungen. Wie viele verschiedene Beobachtungen enthält eine Bootstrap-Stichprobe im Mittel?

## Lösungsweg
a) Bei einem Zug wird $z_j$ mit Wahrscheinlichkeit $1/n$ gezogen, also mit $1 - 1/n$ nicht. Die $n$ Züge sind unabhängig (mit Zurücklegen):
$$P(z_j \notin D^b) = \left(1 - \frac1n\right)^n$$
b) $n = 5$: $(4/5)^5 = 1024/3125 \approx 0{,}328$. Grenzwert: $\lim_{n\to\infty}(1 - 1/n)^n = e^{-1}\approx 0{,}368$.
c) Erwartete Anzahl verschiedener Beobachtungen: $n\big(1 - (1-1/n)^n\big) \approx 1000\cdot 0{,}632 = 632$.

## Ergebnis
| $n$ | $P(\text{OOB})$ | Anteil enthalten |
|---|---|---|
| 5 | 0,328 | 0,672 |
| 10 | 0,349 | 0,651 |
| 100 | 0,366 | 0,634 |
| 1000 | 0,368 | 0,632 |
| $\infty$ | $e^{-1}\approx 0{,}368$ | $\approx 0{,}632$ |

> [!warning] Typischer Fehler
> Annehmen, eine Bootstrap-Stichprobe enthalte alle Beobachtungen genau einmal – sie hat zwar $n$ Einträge, aber viele Duplikate.

## Benötigte Konzepte
- [[bootstrap]]
- [[bagging]]

## Quellen
- [[q-vl00-essentiell|VL0]], Folie 67
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 7.11, S. 249
