---
typ: methode
themen: [xai, klassifikation, glm]
aliases: [Logistic Regression, Logit, Log-Odds, Odds Ratio, Sigmoid]
quellen: ["[[q-vl05-xai-transparente-globale-modelle]]", "[[q-vl00-essentiell]]", "[[q-nb06-global-xai]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Logistische Regression

#flashcards/statistik

## Kurzdefinition
Lineares Modell für binäre Klassifikation auf der **Log-Odds-Skala**: $\mathrm{logit}(p) = \log\frac{p}{1-p} = \beta_0 + \beta^Tx$, also $p = \mathrm{sigmoid}(\beta_0 + \beta^Tx)$. $\exp(\beta_j)$ ist das multiplikative **Odds Ratio** für eine Erhöhung von $x_j$ um eine Einheit (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 12).

## Intuition
Wahrscheinlichkeiten liegen zwischen 0 und 1, eine Linearkombination nicht. Die Logit-Funktion bildet $(0,1)$ auf $\mathbb R$ ab – auf dieser Skala darf das Modell linear sein. Jeder Koeffizient verschiebt die Log-Odds additiv bzw. multipliziert die Odds.

## Formal
$$p(x) = \frac{1}{1 + e^{-(\beta_0 + \beta^Tx)}}, \qquad \frac{p}{1-p} = e^{\beta_0}\prod_j e^{\beta_jx_j}$$
- $\beta_j$ wirkt **additiv auf die Log-Odds**; $e^{\beta_j}$ ist das **Odds Ratio** pro Einheit $x_j$.
- Dasselbe $\beta_j$ erzeugt je nach Ausgangswahrscheinlichkeit **unterschiedliche Wahrscheinlichkeitsänderungen**.
- Für die Kommunikation **marginale Wahrscheinlichkeitsänderungen bei repräsentativen Fällen** berichten, nicht nur Odds Ratios.
- **Kalibrierung prüfen:** Ein gut rangierendes Modell kann trotzdem schlecht kalibrierte Wahrscheinlichkeiten ausgeben.

(→ VL5, F. 12)

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Gefittet wird über die negative Log-Likelihood (Cross-Entropy) $-\sum_i\big[y_i\log p_i + (1-y_i)\log(1-p_i)\big]$, die **konvex** ist – keine geschlossene Lösung, aber Gradienten-/Newton-Verfahren finden das globale Optimum ([[optimierung-grundlagen]]). Die VL nennt die logistische Regression als Beispiel für eine konvexe Zielfunktion (→ [[q-vl00-essentiell|VL0]], F. 75).

## Annahmen & Voraussetzungen
- linear in den Log-Odds; Interaktionen nur, wenn explizit modelliert.

## Beispiel
In [[q-nb06-global-xai|NB6]] hat die logistische Regression (balanced) die **höhere AUC** (0,783) als Gradient Boosting (0,726) – eine transparente Baseline kann gewinnen.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – gleiches $\beta_j$, verschiedene Effekte
> $\beta_j = 0{,}7$ ⇒ Odds Ratio $e^{0{,}7}\approx 2{,}01$: Die Odds verdoppeln sich pro Einheit.
>
> | Ausgangs-$p$ | neues $p$ | Änderung |
> |---|---|---|
> | 0,10 | 0,183 | +0,083 |
> | 0,50 | 0,668 | +0,168 |
> | 0,90 | 0,948 | +0,048 |
>
> Derselbe Koeffizient – drei verschiedene Wahrscheinlichkeitseffekte (Aussage aus VL5, F. 12). Rechenweg: [[logistische-regression-odds-ratio]].

> [!beispiel] Anwendungsfall (nicht aus der VL) – Spamfilter
> Modell: $\mathrm{logit}(p) = -3 + 0{,}8\,x_1 + 1{,}5\,x_2$ mit $x_1$ = Anzahl verdächtiger Wörter, $x_2 = 1$ bei unbekanntem Absender.
> - Mail mit $x_1 = 2$, $x_2 = 1$: $\eta = -3 + 1{,}6 + 1{,}5 = 0{,}1$ ⇒ $p = \frac{1}{1 + e^{-0{,}1}} \approx 0{,}525$ → bei Schwelle 0,5 Spam.
> - Gleiche Mail von bekanntem Absender ($x_2 = 0$): $\eta = -1{,}4$ ⇒ $p \approx 0{,}198$.
> - Odds: $e^{0{,}1} \approx 1{,}105$ statt $e^{-1{,}4} \approx 0{,}247$, Verhältnis $e^{1{,}5} \approx 4{,}48$ = Odds Ratio von $x_2$.
> - **Kommunikation** wie in VL5, F. 12 empfohlen: nicht „unbekannter Absender ver-4,5-facht die Odds", sondern „bei dieser typischen Mail steigt die Spam-Wahrscheinlichkeit von 20 % auf 52 %".

## Typische Prüfungsfragen & Fallstricke
- $e^{\beta_j}$ interpretieren (Odds Ratio, nicht Wahrscheinlichkeitsverhältnis).
- „Warum ist der Effekt auf $p$ nicht konstant?" → Sigmoid ist nichtlinear; am stärksten bei $p = 0{,}5$.
- Fallstrick: Odds mit Wahrscheinlichkeit verwechseln ($p = 0{,}2$ ⇒ Odds $= 0{,}25$).
- Fallstrick: gute AUC ≠ gute Kalibrierung.

## Verwandt
- [[transparente-modelle]] – lineare Modelle als Erklärung
- [[lineare-regression]] – Gegenstück für stetige Ziele
- [[lasso-regression]] – L1-regularisierte logistische Regression
- [[fairness-metriken]] – Kalibrierung
- [[modellauswahl-vorgehen]] – ML-Baseline für Klassifikation

## Karteikarten
Modellgleichung der logistischen Regression?::$\mathrm{logit}(p)=\log\frac{p}{1-p}=\beta_0+\beta^Tx$, $p=\mathrm{sigmoid}(\beta_0+\beta^Tx)$.
Was ist $\exp(\beta_j)$?::Das multiplikative Odds Ratio für eine Erhöhung von $x_j$ um eine Einheit.
Warum soll man marginale Wahrscheinlichkeitsänderungen berichten?::Weil derselbe Koeffizient je nach Ausgangswahrscheinlichkeit unterschiedliche Änderungen von $p$ bewirkt.
Warum Kalibrierung prüfen?::Ein gut rangierendes Modell kann schlecht kalibrierte Wahrscheinlichkeiten ausgeben.
Welche Odds gehören zu $p=0{,}2$?::$0{,}2/0{,}8 = 0{,}25$.
Ist die Zielfunktion der logistischen Regression konvex?::Ja (negative Log-Likelihood, mit konvexer Regularisierung weiterhin konvex).

## Quellen
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folie 12
- [[q-vl00-essentiell|VL0]], Folie 75
- [[q-nb06-global-xai|NB6]]
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 4.4, S. 119
