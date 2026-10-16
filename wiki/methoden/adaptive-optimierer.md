---
typ: methode
themen: [optimierung]
aliases: [AdaGrad, AdaDelta, RMSprop, Adam, AdamW, LION, Adaptive Optimizer, adaptive Lernraten]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-nb02-mnist-regularization]]", "[[q-lab02-pytorch-optimizer-comparison]]", "[[q-abschlussprojekt-abgabe]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-16
---

# Adaptive Optimierer: AdaGrad, AdaDelta, RMSprop, Adam, LION

#flashcards/statistik

> [!info] Relevanz
> In der Auswahl „essentiell" stehen die Motivation und die **Vergleichstabelle** (VL0, F. 89–90) → `hoch`. Die Detailformeln stammen aus VL4 (F. 45–51, 62–63).

## Kurzdefinition
Adaptive Verfahren führen **laufende Statistiken vergangener Gradienten** und skalieren damit die Lernrate **pro Koordinate** (→ [[q-vl00-essentiell|VL0]], F. 89).

## Intuition – warum adaptiv? (→ VL0, F. 89)
- Verschiedene Parameter können Gradienten auf **sehr unterschiedlichen Skalen** haben.
- **Seltene (sparse) Features** brauchen eventuell größere effektive Lernraten.
- Adaptive Methoden reduzieren oft den Tuning-Aufwand; die beste finale Generalisierung ist aber problemabhängig.

## Formal

### AdaGrad (→ [[q-vl04-map-resampling-optimierung|VL4]], F. 46–47)
$$g_t = \nabla_\theta\ell_{B_t}(\theta_{t-1}), \qquad G_t = G_{t-1} + g_t\odot g_t, \qquad \theta_t = \theta_{t-1} - \eta\,\frac{g_t}{\sqrt{G_t + \varepsilon}}$$
Koordinatenweise: $\theta_{t,i} = \theta_{t-1,i} - \eta\, g_{t,i}/\sqrt{G_{t,i} + \varepsilon}$.
- Summe der quadrierten Gradienten **pro Parameter**; große oder häufige Gradienten → Nenner wächst → kleinere Schritte; seltene Features behalten vergleichsweise große Lernraten.
- Stark bei **sparse, hochdimensionalen konvexen** Problemen (z. B. Text-Features).
- **Lernraten fallen nur**; nach vielen Iterationen werden die Updates eventuell zu klein. $\eta$ und $\varepsilon$ bleiben wichtig.
- **Key message:** AdaGrad ist ein **diagonaler Vorkonditionierer** – jede Koordinate wird mit ihrer historischen Gradientengröße reskaliert.

### AdaDelta (→ VL4, F. 48–49)
Ersetzt die kumulative Summe durch **exponentiell gleitende Mittel** (EMA) – das Verfahren „vergisst" alte Gradienten, gesteuert durch $\rho$:
$$E[g^2]_t = \rho\,E[g^2]_{t-1} + (1-\rho)\,g_t^2, \qquad \mathrm{RMS}[g]_t = \sqrt{E[g^2]_t + \varepsilon}$$
$$\Delta\theta_t = -\frac{\mathrm{RMS}[\Delta\theta]_{t-1}}{\mathrm{RMS}[g]_t}\,g_t, \qquad \theta_t = \theta_{t-1} + \Delta\theta_t, \qquad E[\Delta\theta^2]_t = \rho\,E[\Delta\theta^2]_{t-1} + (1-\rho)\,\Delta\theta_t^2$$
- Original-AdaDelta braucht **keine globale Lernrate** $\eta$.
- $\rho$ nahe 1 = langes Gedächtnis, typisch $\rho\approx 0{,}95$.
- $\mathrm{RMS}[\Delta\theta]$ hält die Einheiten des Updates vergleichbar mit den Parametereinheiten.
- robuster als AdaGrad bei langem Training; heute ist Adam verbreiteter.

### RMSprop (→ [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitt „The optimizers we compare")
RMSprop passt die Lernrate **pro Parameter** über ein **gleitendes Mittel der quadrierten Gradienten** an. Die [[q-lab-abschlussprojekt-aufgabe|Kaggle-Aufgabe]] zählt es zu den Optimierern aus der Vorlesung (S. 2). In den Folien steht es laut aktuellem Wiki-Stand nicht.

> [!ergaenzung] Ergänzung (nicht aus der VL) – RMSprop-Update
> $$v_t = \alpha\,v_{t-1} + (1-\alpha)\,g_t^2, \qquad \theta_t = \theta_{t-1} - \eta\,\frac{g_t}{\sqrt{v_t} + \varepsilon}$$
> PyTorch-Default $\alpha = 0{,}99$. RMSprop ist AdaGrad mit gleitendem Mittel statt Summe (dieselbe Idee wie der Nenner von AdaDelta) und entspricht Adam **ohne** erstes Moment und ohne Bias-Korrektur. Damit fällt die effektive Lernrate nicht mehr monoton wie bei AdaGrad.

### Adam (→ VL4, F. 50–51)
$$m_t = \beta_1 m_{t-1} + (1-\beta_1)\,g_t \quad\text{(erstes Moment)}, \qquad v_t = \beta_2 v_{t-1} + (1-\beta_2)\,g_t^2 \quad\text{(zweites Moment)}$$
$$\hat m_t = \frac{m_t}{1-\beta_1^t}, \qquad \hat v_t = \frac{v_t}{1-\beta_2^t}, \qquad \theta_t = \theta_{t-1} - \alpha\,\frac{\hat m_t}{\sqrt{\hat v_t + \varepsilon}}$$
- $m_t$ wirkt wie **Momentum**: glättet verrauschte Gradienten, gibt die Richtung.
- $v_t$ **reskaliert** Koordinaten mit großen jüngeren quadrierten Gradienten.
- **Bias-Korrektur** kompensiert $m_0 = v_0 = 0$ in den ersten Iterationen.
- **Defaults:** $\alpha = 0{,}001$, $\beta_1 = 0{,}9$, $\beta_2 = 0{,}999$, $\varepsilon = 10^{-8}$.
- starker Default für neuronale Netze und verrauschte Minibatches; für regularisiertes Deep Learning ist **AdamW** meist besser als L2 innerhalb von Adam; Lernraten-Schedules bleiben wichtig, die Validierungs-Performance entscheidet.
- **Key message:** Adam kombiniert den richtungsstabilisierenden Effekt von Momentum mit koordinatenweiser Normierung durch jüngere Gradienten.

> [!beispiel] Ergänzung (nicht aus der VL) – erster Adam-Schritt
> Für $t = 1$: $\hat m_1 = g_1$, $\hat v_1 = g_1^2$, also $\Delta\theta \approx -\alpha\cdot g_1/\lvert g_1\rvert = -\alpha\,\mathrm{sign}(g_1)$. Der erste Schritt hat in jeder Koordinate ungefähr Betrag $\alpha$ – **unabhängig von der Gradientengröße**. Ohne Bias-Korrektur wäre er wegen $m_1 = 0{,}1\,g_1$ und $v_1 = 0{,}001\,g_1^2$ um den Faktor $0{,}1/\sqrt{0{,}001}\approx 3{,}2$ größer. Rechenweg: [[adam-adagrad-erster-schritt]].

> [!ergaenzung] Ergänzung (nicht aus der VL) – AdamW vs. L2 in Adam
> L2 im Loss landet im Gradienten und wird dann von $\sqrt{\hat v}$ geteilt – Parameter mit großen Gradienten werden kaum geschrumpft. AdamW zieht den Weight-Decay-Term $\eta\lambda\theta$ **separat** ab, entkoppelt von der adaptiven Skalierung. In [[q-nb02-mnist-regularization|NB2]] und [[q-lab01-pytorch-introduction|Lab 1]] wird L2 über `torch.optim.Adam(weight_decay=…)` umgesetzt, also die gekoppelte Variante.
> $$\text{AdamW: } \theta_t = \theta_{t-1} - \alpha\Big(\frac{\hat m_t}{\sqrt{\hat v_t + \varepsilon}} + \lambda\,\theta_{t-1}\Big)$$

**Im Lab** (→ [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitte „The optimizers we compare", „Summary"): AdamW ist Adam mit **entkoppeltem Weight Decay** und wird bevorzugt, wenn Weight Decay als Regularisierung dient. Auch die LION-Implementierung im Lab schrumpft die Parameter entkoppelt mit dem Faktor $1 - \eta\lambda$, „ähnlich wie AdamW".

### LION (→ VL4, F. 62–63)
```
m₀ = 0
für t = 1, 2, …:
    gₜ = stochastic_gradient(θₜ₋₁)
    uₜ = β₁ mₜ₋₁ + (1 − β₁) gₜ
    θₜ = θₜ₋₁ − η · sign(uₜ) − η λ θₜ₋₁
    mₜ = β₂ mₜ₋₁ + (1 − β₂) gₜ
```
- nutzt das **Vorzeichen** eines momentumartigen Updates statt einer Skalierung mit zweiten Momenten
- **speichereffizient** (weniger Zustände als Adam)
- grobe Update-Richtungen, funktionieren aber teils gut in großen Netzen; Performance empirisch und abhängig von Architektur, Daten und Tuning

## Beispiel
AdaGrad-Schritte bei unterschiedlich großen Gradienten und der erste Adam-Schritt: [[adam-adagrad-erster-schritt]].

**Lab 2, MNIST, 8 Epochen, gleiches Netz und gleicher Seed** (→ [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitt „Test accuracy table"): Adam ($10^{-3}$) 0,9226, AdamW ($10^{-3}$, Weight Decay $10^{-4}$) 0,9228, RMSprop ($10^{-3}$) 0,9149, LION ($10^{-4}$) 0,8939. Alle adaptiven Verfahren liegen hier **hinter SGD mit Momentum** (0,9353). Das Notebook folgert, dass ein fairer Vergleich Lernraten-Tuning braucht und kein Optimierer immer der beste ist. Das passt zur VL-Vorsicht, dass Adam schlechter generalisieren kann als SGD. Bei der Lernrate reagiert Adam deutlich: nach 5 Epochen 0,846 ($10^{-4}$), 0,920 ($10^{-3}$), 0,931 ($10^{-2}$).

> [!beispiel] Anwendungsfall (nicht aus der VL) – eigenes Abschlussprojekt: Adam vs. AdamW
> In der [[q-abschlussprojekt-abgabe|eigenen Fashion-MNIST-Abgabe]] (CNN, 12 Epochen, Early Stopping) liefern Adam und AdamW **ohne** Weight Decay exakt dieselben Ergebnisse, ein guter Test der Implementierung. Mit Weight Decay $10^{-4}$ erreicht AdamW 0,9247 Val-Accuracy, Adam nur 0,9207. AdamW hat dabei nicht gewonnen, denn ohne Weight Decay lag es ebenfalls bei 0,9247; die gekoppelte L2-Variante hat Adam verschlechtert. Wichtig für faire Vergleiche: AdamW hat in PyTorch den Default `weight_decay = 0.01`, ist also ohne explizite 0 versteckt regularisiert.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – Adam glättet einen Vorzeichenwechsel
> Defaults $\alpha = 0{,}001$, $\beta_1 = 0{,}9$, $\beta_2 = 0{,}999$; Gradienten $g_1 = 1$, $g_2 = -1$ (verrauschte Richtung):
>
> | $t$ | $m_t$ | $v_t$ | $\hat m_t$ | $\hat v_t$ | $\Delta\theta_t$ |
> |---|---|---|---|---|---|
> | 1 | 0,1 | 0,001 | 1 | 1 | $-0{,}001$ |
> | 2 | $0{,}09 - 0{,}1 = -0{,}01$ | 0,001999 | $-0{,}01/0{,}19 \approx -0{,}053$ | 1 | $+0{,}00005$ |
>
> Plain SGD würde im zweiten Schritt voll zurückspringen. Bei Adam hebt das erste Moment die beiden Gradienten fast auf – der zweite Schritt ist 20-mal kleiner. Das ist der Momentum-Anteil von Adam.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Textmodelle
> - **Bag-of-Words-Klassifikator** für Support-Tickets mit 50 000 Wortmerkmalen: Seltene, aber aussagekräftige Wörter („Kündigung") tauchen nur in wenigen Batches auf. AdaGrad gibt ihnen eine vergleichsweise große Lernrate, häufige Wörter („bitte") werden schnell gebremst.
> - **Fine-Tuning eines Transformers:** AdamW mit kleiner Lernrate (Größenordnung $10^{-5}$), Weight Decay separat vom adaptiven Schritt und Warm-up – der robuste Standard für neuronale Netze (VL4, F. 51).

## Vergleich (→ VL0, F. 90)
| Methode | Hauptzustand | Stärke | Typische Vorsicht |
|---|---|---|---|
| AdaGrad | Summe quadrierter Gradienten | sparse und seltene Features | Lernrate kann zu stark abfallen |
| AdaDelta | EMA von Gradienten und Updates | weniger manuelles Lernraten-Tuning | im modernen Deep Learning seltener |
| Adam | EMA von Gradient und quadriertem Gradient | robuster Default für viele neuronale Netze | kann in manchen Settings schlechter generalisieren als SGD |

## Typische Prüfungsfragen & Fallstricke
- „Warum adaptive Optimierer?" → drei Gründe aus VL0, F. 89.
- Adam-Update vollständig aufschreiben inkl. Bias-Korrektur und Defaults.
- „Welches Problem von AdaGrad löst AdaDelta?" → monoton schrumpfende Schritte → gleitendes Fenster.
- Fallstrick: $\varepsilon$ in der Folie **unter** der Wurzel ($\sqrt{G+\varepsilon}$); andere Quellen schreiben $\sqrt G + \varepsilon$ – beides dient nur der numerischen Stabilität.
- Fallstrick: Adam ist nicht automatisch besser als SGD (Generalisierung).

## Verwandt
- [[sgd]] – Grundlage (stochastische Gradienten)
- [[momentum-und-nesterov]] – erstes Moment in Adam
- [[newton-und-quasi-newton]] – volle statt diagonaler Vorkonditionierung
- [[ridge-regression]] – Weight Decay
- [[optimierer-uebersicht]]

## Karteikarten
Drei Gründe für adaptive Optimierer?::Gradienten auf sehr unterschiedlichen Skalen; sparse Features brauchen größere effektive Lernraten; weniger Tuning-Aufwand.
AdaGrad-Update?::$G_t=G_{t-1}+g_t\odot g_t$, $\theta_t=\theta_{t-1}-\eta\,g_t/\sqrt{G_t+\varepsilon}$.
Was ist die Key Message zu AdaGrad?::AdaGrad ist ein diagonaler Vorkonditionierer: jede Koordinate wird mit ihrer historischen Gradientengröße reskaliert.
Hauptschwäche von AdaGrad?::Die Lernraten fallen nur; nach vielen Iterationen werden die Updates zu klein.
Was ändert AdaDelta gegenüber AdaGrad?::Exponentiell gleitende Mittel statt kumulativer Summe; braucht im Original keine globale Lernrate.
Adam-Update mit Bias-Korrektur?::$m_t=\beta_1m_{t-1}+(1-\beta_1)g_t$, $v_t=\beta_2v_{t-1}+(1-\beta_2)g_t^2$, $\hat m_t=m_t/(1-\beta_1^t)$, $\hat v_t=v_t/(1-\beta_2^t)$, $\theta_t=\theta_{t-1}-\alpha\hat m_t/\sqrt{\hat v_t+\varepsilon}$.
Adam-Defaults?::$\alpha=0{,}001$, $\beta_1=0{,}9$, $\beta_2=0{,}999$, $\varepsilon=10^{-8}$.
Wozu die Bias-Korrektur bei Adam?::Sie kompensiert die Initialisierung $m_0=v_0=0$ in den ersten Iterationen.
Was verwendet LION statt zweiter Momente?::Das Vorzeichen eines momentumartigen Updates (sign-based).
Welche Vorsicht nennt die VL bei Adam?::Kann in manchen Settings schlechter generalisieren als SGD.
Was macht RMSprop?::Es passt die Lernrate pro Parameter über ein gleitendes Mittel der quadrierten Gradienten an (wie AdaGrad, aber mit Vergessen).
Unterschied Adam mit `weight_decay` vs. AdamW?::Bei Adam landet der L2-Term im Gradienten und wird adaptiv skaliert (gekoppelt); AdamW zieht den Weight Decay separat ab (entkoppelt) und wird bei Weight-Decay-Regularisierung bevorzugt.
Welcher Optimierer gewann in Lab 2 auf MNIST, und was folgt daraus?::SGD mit Momentum (Test-Acc 0,935) vor AdamW/Adam (0,923); ein fairer Vergleich braucht Lernraten-Tuning, kein Optimierer ist immer der beste.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 89–90
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 44–52, 62–63
- [[q-nb02-mnist-regularization|NB2]]
- [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitte „The optimizers we compare", „A small LION optimizer implementation", „Test accuracy table", „Learning rate sensitivity"
- [[q-lab-abschlussprojekt-aufgabe|Kaggle-Aufgabe]], S. 2
- [[q-abschlussprojekt-abgabe|eigene Abgabe]], S. 2–3
