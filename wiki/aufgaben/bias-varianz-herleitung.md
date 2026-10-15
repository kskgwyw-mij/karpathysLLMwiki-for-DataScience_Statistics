---
typ: aufgabe
themen: [bias-varianz]
quellen: ["[[q-vl02-bias-varianz-modellwahl]]"]
relevanz: niedrig
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Herleitung der Bias-Varianz-Zerlegung

> [!info] Relevanz
> In der Vorlesung ausdrücklich „nur für Interessierte" (→ [[q-vl02-bias-varianz-modellwahl|VL2]], F. 5–6). Das **Ergebnis** ist dagegen prüfungsrelevant ([[bias-varianz-tradeoff]]).

## Aufgabenstellung
Zeigen Sie für $y = f(x) + \varepsilon$ mit $\mathbb E[\varepsilon] = 0$, $\mathbb E[\varepsilon^2] = \sigma^2$, $\varepsilon$ unabhängig von $\hat f$:
$$\mathbb E\big[(y - \hat f(x))^2\big] = \big(f(x) - \mathbb E[\hat f(x)]\big)^2 + \mathbb E\big[(\mathbb E[\hat f(x)] - \hat f(x))^2\big] + \sigma^2$$

## Lösungsweg
**Schritt 1 – Rauschen abspalten.** $y$ einsetzen und ausmultiplizieren:
$$\mathbb E\big[(f + \varepsilon - \hat f)^2\big] = \mathbb E\big[(f - \hat f)^2\big] + 2\,\mathbb E\big[(f - \hat f)\,\varepsilon\big] + \mathbb E[\varepsilon^2]$$
- Letzter Term: $\mathbb E[\varepsilon^2] = \sigma^2$ → irreduzibler Fehler.
- Mittlerer Term: wegen Unabhängigkeit $\mathbb E[(f-\hat f)\varepsilon] = \mathbb E[f - \hat f]\cdot\mathbb E[\varepsilon] = 0$.

Also $\mathrm{MSE}(x) = \mathbb E[(f - \hat f)^2] + \sigma^2$.

**Schritt 2 – Null addieren.** Im ersten Term $\mathbb E[\hat f]$ einschieben:
$$\mathbb E\big[(f - \mathbb E[\hat f] + \mathbb E[\hat f] - \hat f)^2\big] = \mathbb E\big[(f - \mathbb E[\hat f])^2\big] + \mathbb E\big[(\mathbb E[\hat f] - \hat f)^2\big] + 2\,\mathbb E\big[(f - \mathbb E[\hat f])(\mathbb E[\hat f] - \hat f)\big]$$

**Schritt 3 – Mischterm verschwindet.** $f(x)$ ist deterministisch und $\mathbb E[\hat f]$ eine Zahl, also ist $(f - \mathbb E[\hat f])$ konstant und kann vor den Erwartungswert:
$$2\,(f - \mathbb E[\hat f])\cdot\mathbb E\big[\mathbb E[\hat f] - \hat f\big] = 2\,(f - \mathbb E[\hat f])\cdot(\mathbb E[\hat f] - \mathbb E[\hat f]) = 0$$

**Schritt 4 – Bias-Term.** Ebenfalls konstant: $\mathbb E[(f - \mathbb E[\hat f])^2] = (f - \mathbb E[\hat f])^2$.

## Ergebnis
$$\mathrm{MSE}(x) = \underbrace{\big(f(x) - \mathbb E[\hat f(x)]\big)^2}_{\text{Bias}^2} + \underbrace{\mathbb E\big[(\mathbb E[\hat f(x)] - \hat f(x))^2\big]}_{\text{Varianz}} + \underbrace{\sigma^2}_{\text{irreduzibel}}$$

> [!warning] Typischer Fehler
> - Den Mischterm in Schritt 1 ohne Begründung streichen – man braucht $\mathbb E[\varepsilon] = 0$ **und** die Unabhängigkeit.
> - In Schritt 3 vergessen, dass $f$ deterministisch ist (VL2, F. 6: „Da $f$ keine Zufallsvariable … ist").

## Benötigte Konzepte
- [[bias-varianz-tradeoff]]
- [[statistisches-lernproblem]]

## Quellen
- [[q-vl02-bias-varianz-modellwahl|VL2]], Folien 5–7
