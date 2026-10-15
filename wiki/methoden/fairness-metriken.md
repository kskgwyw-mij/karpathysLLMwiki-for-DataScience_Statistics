---
typ: methode
themen: [fairness, metriken]
aliases: [Fairness-Metriken, Demographic Parity, Statistical Parity, Equal Opportunity, Equalized Odds, Predictive Parity, Kalibrierung, Calibration, Impossibility]
quellen: ["[[q-vl06-xai-lokal-fairness]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Fairness-Metriken

#flashcards/statistik

## Kurzdefinition
Gruppen-Fairness-Metriken vergleichen bedingte Raten zwischen Gruppen $A = a$: **Auswahlraten** (Demographic Parity), **Fehlerraten** (Equal Opportunity, Equalized Odds), **Vorhersagewerte** (Predictive Parity) oder die **Bedeutung von Scores** (Kalibrierung). Sie sind im Allgemeinen **nicht gleichzeitig** erfüllbar (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 19–22).

## Intuition
Jede Metrik schützt vor einem anderen Schaden: gleiche Chancen auf eine Einladung, gleiche Chancen für tatsächlich Qualifizierte, gleiche Verlässlichkeit einer positiven Entscheidung. Sind die **Basisraten** (Anteil tatsächlich Positiver) in den Gruppen verschieden, kann man nicht alles gleichzeitig angleichen – man muss entscheiden, welcher Schaden zählt.

## Formal
Notation: $\hat Y$ Modellentscheidung, $Y$ wahres Label, $A$ Gruppenattribut. Aus der Konfusionsmatrix je Gruppe: TPR $= \frac{TP}{TP+FN}$, FPR $= \frac{FP}{FP+TN}$, PPV $= \frac{TP}{TP+FP}$, Auswahlrate $= \frac{TP+FP}{n}$.

| Metrik | Definition (gleich für alle $a$) | gleicht an | nützlich wenn | Einschränkung |
|---|---|---|---|---|
| **Demographic Parity** | $P(\hat Y = 1\mid A = a)$ | Auswahlraten | gleicher Zugang/Repräsentation ist Ziel | ignoriert $Y$; kollidiert mit Accuracy bei ungleichen Basisraten |
| **Equal Opportunity** | $P(\hat Y = 1\mid Y = 1, A = a)$ | TPR | verpasste Chancen für Berechtigte sind der Hauptschaden | kontrolliert False Positives nicht |
| **Equalized Odds** | $P(\hat Y = 1\mid Y = y, A = a)$ für $y = 0, 1$ | TPR **und** FPR | FN und FP sind beide wichtig | kann Kalibrierung oder Accuracy reduzieren |
| **Predictive Parity** | $P(Y = 1\mid\hat Y = 1, A = a)$ | PPV | positive Klassifikation löst knappe Ressourcen/schwere Maßnahmen aus | kollidiert mit Equalized Odds bei ungleichen Basisraten |
| **Kalibrierung** | unter Personen mit Score $s$ liegt die beobachtete Ergebnisfrequenz in jeder Gruppe bei ≈ $s$ | Bedeutung des Scores | Scores unterstützen Priorisierung statt einer binären Entscheidung | garantiert keine gleichen Fehler- oder Auswahlraten |

(→ VL6, F. 19–21, 23). Equal Opportunity und Equalized Odds brauchen **vertrauenswürdige Labels** – verzerrte Labels machen Fehlerraten-Fairness irreführend (→ VL6, F. 20).

### Warum Metriken kollidieren (→ VL6, F. 22)
- Im Allgemeinen ist es **unmöglich, Kalibrierung, Equalized Odds und Predictive Parity gleichzeitig** zu erfüllen, außer in Spezialfällen.
- Schwellenwerte zu ändern kann eine Metrik verbessern und eine andere verschlechtern.
- Eine einzelne Metrik zu optimieren kann Schaden von einem Fehlertyp oder einer Gruppe auf eine andere verlagern.
- Daher braucht Fairness eine **normative Entscheidung**: Welcher Schaden ist in diesem Kontext am wichtigsten?
- Ein verantwortlicher Bericht dokumentiert gewählte Metrik, verworfene Alternativen und erwartete Nebenwirkungen.

> [!ergaenzung] Ergänzung (nicht aus der VL) – warum die Kollision zwingend ist
> Für jede Gruppe gilt $\mathrm{FPR} = \frac{p}{1-p}\cdot\frac{1-\mathrm{PPV}}{\mathrm{PPV}}\cdot\mathrm{TPR}$ mit Basisrate $p$ (Chouldechova 2017). Sind PPV **und** TPR in beiden Gruppen gleich, aber $p$ verschieden, müssen die FPR verschieden sein – Predictive Parity und Equalized Odds schließen sich dann aus (außer bei perfekter Klassifikation).

### Metrikwahl nach Schaden (→ VL6, F. 23)
| Situation | Primäres Anliegen | Mögliche Metrik | Wichtigste Einschränkung |
|---|---|---|---|
| Zugang zu Chancen | unterschiedliche Auswahlraten | Demographic Parity | kann Qualifikationslabels ignorieren |
| Verpasste Chancen vermeiden | False Negatives bei Berechtigten | Equal Opportunity | kontrolliert False Positives nicht |
| Beide Fehlertypen kontrollieren | ungleiche TPR und FPR | Equalized Odds | kann Kalibrierung oder Accuracy reduzieren |
| Vertrauen in Risiko-Score | Bedeutung des Scores unterscheidet sich nach Gruppe | Kalibrierung | kann dennoch ungleiche Fehlerraten haben |
| Knappe Ressourcen | positive Entscheidungen sollen ähnlichen tatsächlichen Bedarf bedeuten | Predictive Parity | kollidiert mit Equalized Odds bei ungleichen Basisraten |

**Kernaussagen** (→ VL6, F. 24): Fairness beginnt mit Problemformulierung, Labels und Deployment-Kontext. Gruppenmetriken zeigen Verteilungen von Fehlern und Chancen, lösen die normative Frage aber nicht. Basisraten, Kalibrierung und Fehlerratenparität können mathematisch kollidieren. Eine gute Analyse dokumentiert Metrikwahl, Annahmen, Unsicherheit und Restrisiko.

## Beispiel
> [!beispiel] Rechenbeispiel (nicht aus der VL) – alle Metriken aus zwei Konfusionsmatrizen
> Gruppe A: TP 40, FN 10, FP 20, TN 130; Gruppe B: TP 15, FN 15, FP 5, TN 65:
>
> | | Basisrate | Auswahlrate | TPR | FPR | PPV |
> |---|---|---|---|---|---|
> | A | 0,25 | 0,30 | 0,80 | 0,133 | 0,667 |
> | B | 0,30 | 0,20 | 0,50 | 0,071 | 0,750 |
>
> Keine Metrik ist erfüllt; B hat trotz **höherer** Basisrate die **niedrigere** Auswahlrate und TPR (Equal-Opportunity-Verletzung). Rechenweg: [[fairness-metriken-rechnen]].

> [!beispiel] Rechenbeispiel (nicht aus der VL) – die Kollision in Zahlen
> Beide Gruppen erfüllen Predictive Parity (PPV = 0,7) **und** Equal Opportunity (TPR = 0,8), die Basisraten sind aber 0,3 bzw. 0,5. Mit $\mathrm{FPR} = \frac{p}{1-p}\cdot\frac{1-\mathrm{PPV}}{\mathrm{PPV}}\cdot\mathrm{TPR}$:
> - Gruppe 1: $\frac{0{,}3}{0{,}7}\cdot\frac{0{,}3}{0{,}7}\cdot0{,}8 \approx 0{,}147$
> - Gruppe 2: $\frac{0{,}5}{0{,}5}\cdot\frac{0{,}3}{0{,}7}\cdot0{,}8 \approx 0{,}343$
>
> Die FPR unterscheiden sich zwangsläufig um mehr als das Doppelte – Equalized Odds ist **unmöglich**, solange PPV und TPR gleich bleiben (VL6, F. 22).

> [!beispiel] Anwendungsfall (nicht aus der VL) – Rückfallprognosen in der Justiz
> In der öffentlichen Debatte um das US-Risikotool COMPAS (2016) zeigte eine Journalistenrecherche, dass Angeklagte einer Gruppe bei gleichem tatsächlichem Verhalten häufiger fälschlich als „hohes Risiko" eingestuft wurden (ungleiche FPR). Der Hersteller hielt dagegen, die Scores seien in beiden Gruppen gleich **kalibriert** bzw. hätten gleichen PPV. Beide Seiten hatten mit ihrer Metrik recht – bei unterschiedlichen Basisraten geht nicht beides (Rechnung oben). Die Frage, welcher Schaden zählt, ist normativ (VL6, F. 23).

## Typische Prüfungsfragen & Fallstricke
- Definitionen als bedingte Wahrscheinlichkeiten aufschreiben.
- Aus Konfusionsmatrizen je Gruppe Metriken berechnen und bewerten.
- Für ein Szenario die passende Metrik begründen.
- Fallstrick: $P(\hat Y=1\mid Y=1)$ (TPR) mit $P(Y=1\mid\hat Y=1)$ (PPV) verwechseln.
- Fallstrick: annehmen, ein kalibriertes Modell sei automatisch fair.

## Verwandt
- [[fairness-grundlagen]] – Schadensarten
- [[fairness-mitigation]] – Schwellenwerte, Pre-/In-/Post-processing
- [[logistische-regression]] – Kalibrierung
- [[fairness-metriken-rechnen]] – Rechenübung

## Karteikarten
Definition Demographic Parity?::$P(\hat Y=1\mid A=a)$ ist für alle Gruppen gleich (gleiche Auswahlraten).
Definition Equal Opportunity?::$P(\hat Y=1\mid Y=1,A=a)$ gleich für alle Gruppen (gleiche True-Positive-Raten).
Definition Equalized Odds?::$P(\hat Y=1\mid Y=y,A=a)$ gleich für $y=0$ und $y=1$ (gleiche TPR und FPR).
Definition Predictive Parity?::$P(Y=1\mid\hat Y=1,A=a)$ gleich für alle Gruppen (gleicher positiver Vorhersagewert).
Was bedeutet Kalibrierung über Gruppen?::Unter Personen mit Score $s$ liegt die beobachtete Ergebnisfrequenz in jeder Gruppe ungefähr bei $s$.
Welche drei Kriterien sind im Allgemeinen nicht gleichzeitig erfüllbar?::Kalibrierung, Equalized Odds und Predictive Parity.
Hauptschwäche von Demographic Parity?::Ignoriert das wahre Label $Y$; kollidiert mit Accuracy bei ungleichen Basisraten.
Wann Equal Opportunity?::Wenn verpasste Chancen für tatsächlich Berechtigte (False Negatives) der Hauptschaden sind.
Wann Predictive Parity?::Wenn eine positive Klassifikation knappe Ressourcen oder schwere Maßnahmen auslöst.
Was brauchen fehlerratenbasierte Metriken zwingend?::Vertrauenswürdige Labels.

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 19–24
