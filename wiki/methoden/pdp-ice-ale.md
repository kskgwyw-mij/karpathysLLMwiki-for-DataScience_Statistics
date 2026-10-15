---
typ: methode
themen: [xai, global]
aliases: [PDP, Partial Dependence Plot, ICE, Individual Conditional Expectation, zentriertes ICE, c-ICE, ALE, Accumulated Local Effects, Akkumulierte lokale Effekte]
quellen: ["[[q-vl05-xai-transparente-globale-modelle]]", "[[q-nb06-global-xai]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# PDP, ICE und ALE

#flashcards/statistik

## Kurzdefinition
Effektkurven, die zeigen, wie die Modellvorhersage über den Wertebereich eines Merkmals variiert:
- **PDP** – mittlere Vorhersage, wenn $x_j$ bei allen Beobachtungen auf $z$ gesetzt wird
- **ICE** – eine Kurve pro Beobachtung
- **ALE** – akkumulierte lokale Vorhersagedifferenzen nur in beobachteten Intervallen
(→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 23–27)

## Intuition
PDP fragt: „Was würde das Modell im Durchschnitt vorhersagen, wenn dieses Merkmal den Wert $z$ hätte?" ICE zerlegt diesen Durchschnitt in Einzelkurven und zeigt, ob alle gleich reagieren. ALE vermeidet das Hauptproblem des PDP – das Erfinden unrealistischer Datenpunkte bei korrelierten Merkmalen.

## Formal

### Partial Dependence Plot (→ VL5, F. 23)
$$\mathrm{PDP}_j(z) = \frac1N\sum_{i=1}^N f\big(x_{i,1}, \dots, x_{i,j-1},\ z,\ x_{i,j+1}, \dots, x_{i,p}\big)$$
Für jeden Gitterwert $z$: $x_j$ bei **allen** Beobachtungen durch $z$ ersetzen und die Vorhersagen mitteln.
- PDP setzt voraus, dass die konstruierten Kombinationen sinnvoll sind – das **scheitert bei stark abhängigen Merkmalen** (Extrapolation).
- Immer **Rug-Plot oder Histogramm** zeigen, wo tatsächlich Daten liegen.

> [!note] Notation
> Die Folie schreibt die Formel als Summe ohne $\frac1N$ und mit $x_N$ als letztem Merkmal; gemeint ist der Mittelwert über die $N$ Beobachtungen mit $p$ Merkmalen (der Folientext sagt „average the model predictions").

### ICE und zentriertes ICE (→ VL5, F. 24)
- ICE hält **eine Beobachtung fest** und variiert ein Merkmal über ein Raster → eine Linie pro Beobachtung. Der PDP ist der Mittelwert der ICE-Kurven.
- **Auseinanderlaufende ICE-Linien** deuten auf **Interaktionen** oder heterogenes Modellverhalten hin.
- **Zentriertes ICE** zieht die Vorhersage an einem Referenzwert ab → Formunterschiede besser vergleichbar.
- Vorsicht: ICE erzeugt weiterhin kontrafaktische Zeilen; unrealistische Rasterwerte können irreführen. Bei vielen Linien Sampling oder Clustering.

### ALE – Accumulated Local Effects (→ VL5, F. 26)
1. Merkmal in **Intervalle** teilen (z. B. Quantile).
2. Für die Beobachtungen **innerhalb** eines Intervalls: $f(\text{obere Grenze}, x_{-j}) - f(\text{untere Grenze}, x_{-j})$.
3. Lokale Differenzen mitteln, **akkumulieren** und um 0 **zentrieren**.
- reduziert Extrapolationsprobleme gegenüber PDP bei korrelierten Merkmalen
- Interpretation **relativ**: Abweichungen von der mittleren Vorhersage, keine absoluten Vorhersagen

## Vergleich (→ VL5, F. 27)
| Methode | Berechnung | Beste Verwendung | Wichtigste Vorsicht |
|---|---|---|---|
| PDP | mittlere Vorhersage nach Setzen des Merkmals auf Rasterwerte | einfache Kommunikation globaler Effekte | kann unrealistische Zeilen mitteln |
| ICE | eine Antwortkurve pro Beobachtung | Heterogenität, Interaktionen | kann verrauscht und dicht sein |
| Zentriertes ICE | ICE auf gemeinsame Referenz verschoben | Kurvenformen vergleichen | verbirgt Unterschiede im Basisrisiko |
| ALE | akkumulierte lokale Differenzen in beobachteten Intervallen | korrelierte Merkmale | weniger intuitive Skala |

**Kernaussage** (→ VL5, F. 28): PDP ist intuitiv, kann aber extrapolieren; ICE zeigt Heterogenität; ALE ist bei korrelierten Merkmalen oft sicherer. Globale Methoden brauchen Verteilungsprüfungen, wiederholte Schätzungen und sorgfältige Formulierungen; danach lokale Fälle und Subgruppen untersuchen.

## Beispiel ([[q-nb06-global-xai|NB6]])
ALE für `tenure_months` (Gradient Boosting, Churn):

| Laufzeit (Intervallmitte) | 4 | 10,5 | 18 | 32 | 51,5 | 67,3 |
|---|---|---|---|---|---|---|
| ALE-Effekt auf p(churn) | +0,103 | +0,055 | −0,007 | −0,004 | −0,010 | −0,053 |

Lesart: Kurze Vertragslaufzeit geht im Modell mit ca. 10 Prozentpunkten **höherer** vorhergesagter Abwanderung als im Durchschnitt einher. **Nicht** daraus folgern, dass eine Verlängerung der Laufzeit Abwanderung verhindert.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – PDP versteckt eine Interaktion
> Modell $f(x_1, x_2) = x_1\cdot x_2$, vier Beobachtungen mit $x_2 = (-1, -1, 1, 1)$.
> - **ICE** für $x_1$: Beobachtungen 1–2 ergeben die Linie $-z$, Beobachtungen 3–4 die Linie $+z$ – zwei gegenläufige Effekte.
> - **PDP:** $\mathrm{PDP}_1(z) = \frac14\big(-z - z + z + z\big) = 0$ – eine flache Linie.
>
> Nach dem PDP wäre $x_1$ bedeutungslos, obwohl es jede einzelne Vorhersage stark bewegt. Auseinanderlaufende ICE-Linien zeigen die Interaktion mit $x_2$ (VL5, F. 24).

> [!beispiel] Anwendungsfall (nicht aus der VL) – Kfz-Versicherung mit korrelierten Merkmalen
> Ein Schadenmodell nutzt Alter und Jahre mit Führerschein (stark korreliert). Der PDP für „40 Jahre Fahrerfahrung" setzt dafür auch 20-Jährige auf 40 Jahre Erfahrung – unmögliche Zeilen, das Modell extrapoliert, und die Kurve kann beliebig aussehen. ALE vergleicht dagegen nur Personen, die tatsächlich im Intervall (z. B. 38–42 Jahre Erfahrung) liegen, und ändert die Erfahrung nur innerhalb dieses Intervalls. Für die Präsentation gehört ein Rug-Plot dazu, der zeigt, wo Daten liegen.

## Typische Prüfungsfragen & Fallstricke
- PDP-Formel aufschreiben und erklären.
- „Warum ist PDP bei korrelierten Merkmalen problematisch, und was hilft?" → unrealistische Kombinationen; ALE.
- „Was zeigen auseinanderlaufende ICE-Kurven?" → Interaktionen/Heterogenität.
- Fallstrick: ALE-Werte als absolute Wahrscheinlichkeiten lesen.
- Fallstrick: Bereiche ohne Datenabdeckung interpretieren.

## Verwandt
- [[permutation-feature-importance]] – Stärke statt Form
- [[shap]] – lokale additive Zerlegung, Dependence-Plots
- [[kontrafaktische-erklaerungen]] – ebenfalls „was wäre, wenn"
- [[erklaerbarkeit-grundlagen]]

## Karteikarten
Wie berechnet man den PDP für Merkmal $j$ am Wert $z$?::$x_j$ bei allen Beobachtungen durch $z$ ersetzen und die Vorhersagen mitteln.
Hauptproblem des PDP?::Bei stark abhängigen Merkmalen mittelt er über unrealistische Kombinationen (Extrapolation).
Was ist ICE?::Eine Antwortkurve pro Beobachtung: ein Merkmal variieren, Rest fest; PDP = Mittel der ICE-Kurven.
Was zeigen auseinanderlaufende ICE-Linien?::Interaktionen oder heterogenes Modellverhalten.
Wie funktioniert ALE?::Merkmal in Intervalle teilen, lokale Vorhersagedifferenzen innerhalb beobachteter Intervalle mitteln, akkumulieren, zentrieren.
Wann ist ALE dem PDP vorzuziehen?::Bei korrelierten Merkmalen (weniger Extrapolation).
Was sollte man jedem PDP beifügen?::Einen Rug-Plot oder ein Histogramm der tatsächlichen Datenverteilung.
Was verbirgt zentriertes ICE?::Unterschiede im Basisrisiko (Niveau der Kurven).

## Quellen
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folien 23–28
- [[q-nb06-global-xai|NB6]]
