---
typ: methode
themen: [regularisierung]
aliases: [Output Regularisierung, Output Regularization, Total Variation, TV-Regularisierung, Shape Prior]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Output-Regularisierung

#flashcards/statistik

## Kurzdefinition
Bei der Output-Regularisierung werden nicht die Parameter des Modells direkt eingeschränkt oder bestraft, sondern der **Output** des Modells. So lassen sich gewollte Eigenschaften der Ausgabe erreichen (→ [[q-vl00-essentiell|VL0]], F. 41, 48).

## Intuition
Oft weiß man mehr über die **Ausgabe** als über sinnvolle Parameter: Ein rekonstruiertes Bild soll glatt sein mit scharfen Kanten, eine Segmentierung zusammenhängend, eine Kurve monoton. Diese Eigenschaften direkt zu bestrafen ist natürlicher, als sie in Gewichtsnormen zu übersetzen. Im Machine-Learning-Kontext wird das häufig nicht erwähnt (→ VL0, F. 41).

## Formal
$$\mathcal L(\theta) = \frac1N\sum_{i=1}^N\|f_\theta(x_i) - y_i\|^2 + \lambda\,\mathcal R\big(f_\theta(x_i)\big)$$
Besonders hilfreich, wenn sich Eigenschaften der Output-Signalklasse gut beschreiben lassen (z. B. Bilder). Kombinierbar mit den anderen Regularisierungsmethoden (→ VL0, F. 48).

### Beispiele (→ [[q-vl03-regularisierung-cv|VL3]], F. 25)
- **Transformationen des Outputs**, z. B. Differentialoperatoren: $\mathcal R(x) = \|T(x)\|^2$ (Glattheit über Ableitungen)
- **Shape Priors:** Die Vorhersage soll bestimmte Formen bevorzugen.
- **Topologie-Priors:** Vorwissen über Zusammenhangskomponenten, Löcher …
- **Gelernte Priors:** vortrainierte neuronale Netze als Regularisierungsfunktional
- im Entscheidungsschema: **TV/Laplace** für strukturierte Outputs (→ VL0, F. 54)

> [!info] Ergänzung (nicht aus der VL)
> **Total Variation** in 1D: $\mathrm{TV}(u) = \sum_k\lvert u_{k+1} - u_k\rvert$ (L1 der Differenzen → stückweise konstante Ausgaben mit scharfen Sprüngen). **Laplace/Quadratisch:** $\sum_k(u_{k+1} - u_k)^2$ (L2 der Differenzen → glatte Ausgaben, Kanten verschmieren).

## Hyperparameter
Output-Regularisierer durch die Anzahl der Ausgabekomponenten (z. B. Pixel) teilen → $\lambda$ wird skaleninvariant (→ VL0, F. 55).

**Diagnose** (→ VL0, F. 57):
- zu viel TV: überglättete, verkürzte Strukturen, zu wenig feine Details
- zu wenig TV: Zacken/Rauschen, „flatternde" Grenzen

## Annahmen & Voraussetzungen
- Das Vorwissen über die Ausgabe muss sich als Funktional $\mathcal R$ formulieren lassen.

## Typische Prüfungsfragen & Fallstricke
- Unterschied Parameter- vs. Output-Regularisierung erklären.
- Zwei Beispiele für Output-Priors nennen.
- Fallstrick: $\lambda$ nicht normiert → Wert hängt von der Bildgröße ab.

## Verwandt
- [[regularisierung]] – Taxonomie, Entscheidungsschema
- [[label-smoothing]] – ebenfalls auf der Zielseite
- [[lasso-regression]] – TV ist L1 auf Differenzen
- [[splines-und-b-splines]] – Glattheit über die Basis statt über einen Strafterm

## Karteikarten
Was wird bei der Output-Regularisierung bestraft?::Der Output $f_\theta(x)$ des Modells, nicht die Parameter.
Wann ist Output-Regularisierung besonders hilfreich?::Wenn sich Eigenschaften der Output-Signalklasse gut beschreiben lassen, z. B. bei Bildern.
Nenne vier Beispiele für Output-Regularisierer.::Transformation/Differentialoperator $\|T(x)\|^2$, Shape Priors, Topologie-Priors, gelernte Priors (vortrainierte Netze).
Symptom von zu viel TV-Regularisierung?::Überglättete, verkürzte Strukturen, zu wenig Details.
Symptom von zu wenig TV-Regularisierung?::Zacken, Rauschen, flatternde Grenzen.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 41, 48, 54–57
- [[q-vl03-regularisierung-cv|VL3]], Folien 8, 24–25
