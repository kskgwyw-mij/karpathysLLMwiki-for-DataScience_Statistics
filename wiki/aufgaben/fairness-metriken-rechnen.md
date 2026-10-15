---
typ: aufgabe
themen: [fairness, metriken]
quellen: ["[[q-vl06-xai-lokal-fairness]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Fairness-Metriken aus Konfusionsmatrizen

> [!beispiel] Selbst erstellte Übungsaufgabe
> Definitionen aus [[q-vl06-xai-lokal-fairness|VL6]], F. 19–23; Zahlen ergänzt und nachgerechnet.

## Aufgabenstellung
Ein Modell entscheidet über Einladungen zu einem Förderprogramm ($\hat Y = 1$: Einladung, $Y = 1$: tatsächlich geeignet). Konfusionsmatrizen je Gruppe:

| Gruppe | TP | FN | FP | TN | $n$ |
|---|---|---|---|---|---|
| A | 40 | 10 | 20 | 130 | 200 |
| B | 15 | 15 | 5 | 65 | 100 |

a) Berechnen Sie Basisrate, Auswahlrate, TPR, FPR und PPV je Gruppe.
b) Welche Fairness-Kriterien sind verletzt?
c) Welche Metrik passt, wenn der Hauptschaden „geeignete Personen werden nicht eingeladen" ist? Was wäre eine Maßnahme?

## Lösungsweg
a)

| | Basisrate $\frac{TP+FN}{n}$ | Auswahlrate $\frac{TP+FP}{n}$ | TPR $\frac{TP}{TP+FN}$ | FPR $\frac{FP}{FP+TN}$ | PPV $\frac{TP}{TP+FP}$ | Accuracy |
|---|---|---|---|---|---|---|
| A | 50/200 = 0,25 | 60/200 = 0,30 | 40/50 = 0,80 | 20/150 = 0,133 | 40/60 = 0,667 | 0,85 |
| B | 30/100 = 0,30 | 20/100 = 0,20 | 15/30 = 0,50 | 5/70 = 0,071 | 15/20 = 0,750 | 0,80 |

b)
- **Demographic Parity** verletzt: 0,30 vs. 0,20.
- **Equal Opportunity** verletzt: TPR 0,80 vs. 0,50 – in B wird die Hälfte der Geeigneten übersehen.
- **Equalized Odds** verletzt: TPR und FPR unterschiedlich.
- **Predictive Parity** verletzt: PPV 0,667 vs. 0,750 (Einladungen in B sind „verlässlicher").
- Auffällig: B hat die **höhere** Basisrate, aber die **niedrigere** Auswahlrate – das Modell ist für B zu streng.

c) Hauptschaden = verpasste Chancen für Berechtigte ⇒ **Equal Opportunity** (VL6, F. 23). Maßnahme z. B. Post-processing über Schwellenwerte (Threshold Sweep), sodass die TPR angeglichen werden – mit Dokumentation der Nebenwirkungen (mehr FP, andere PPV) und rechtlicher Prüfung gruppenspezifischer Schwellen ([[fairness-mitigation]]).

> [!ergaenzung] Kontrolle (Ergänzung)
> Identität $\mathrm{FPR} = \frac{p}{1-p}\cdot\frac{1-\mathrm{PPV}}{\mathrm{PPV}}\cdot\mathrm{TPR}$: Gruppe A: $\frac{0{,}25}{0{,}75}\cdot\frac{1/3}{2/3}\cdot 0{,}8 = 0{,}133$ ✓.

> [!warning] Typischer Fehler
> - TPR (Nenner: tatsächlich Positive) und PPV (Nenner: vorhergesagt Positive) verwechseln.
> - Aus einer gleichen Gesamt-Accuracy auf Fairness schließen.

## Benötigte Konzepte
- [[fairness-metriken]]
- [[fairness-mitigation]]

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 19–23, 28
