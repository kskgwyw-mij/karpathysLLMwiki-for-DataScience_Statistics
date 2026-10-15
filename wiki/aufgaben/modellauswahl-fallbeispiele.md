---
typ: aufgabe
themen: [modellwahl]
quellen: ["[[q-modellauswahl-abgaben]]", "[[q-vl00-essentiell]]", "[[q-vl02-bias-varianz-modellwahl]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Modellauswahl an zwei Fallbeispielen

Beide Fälle stammen aus den eigenen Workshop-Abgaben ([[q-modellauswahl-abgaben]]). Hier werden sie systematisch gegen den 4-Phasen-Ablauf aus [[modellauswahl-vorgehen]] gespiegelt – so lässt sich eine offene Klausurfrage „Schlagen Sie ein Modell vor und begründen Sie" strukturiert beantworten.

## Fall A · Predictive Quality Control (OK/NOK-Bauteile)

| Phase | Antwort |
|---|---|
| **1 Kriterien** | Binäre Klassifikation während der Produktion. Fachziel: Ausschuss und Nacharbeit früh vermeiden. Ein NOK-Teil als OK durchzulassen ist teurer als umgekehrt → **Recall der NOK-Klasse** als Hauptmetrik, Precision als Nebenbedingung. Einsatz: zwischen Produktionsschritten, also nahezu Echtzeit, niedrige Latenz. |
| **2 Daten/Problem** | Tabellarische Sensordaten (Temperatur, Abkühlung, Maschinendaten), großes N, viele Features, Rauschen und Ausfälle durch Sensorverschleiß, **NOK selten** (Class Imbalance). |
| **3 Baselines** | Dummy: „immer OK" – hat hohe Accuracy, aber Recall 0 für NOK → zeigt, warum Accuracy ungeeignet ist. ML-Baseline: logistische Regression (schnell, interpretierbar). |
| **4 Shortlist** | Gradient Boosted Trees (tabellarisch, nichtlinear, robust bei großen verrauschten Daten – VL0 F. 23/26), Random Forest als Vergleich, regularisierte logistische Regression. NOK über Klassengewichte höher gewichten. |
| **Danach** | Gleiche stratifizierte CV für alle Kandidaten; Schwellenwert über die Kosten (FN ≫ FP) wählen; Fehleranalyse der übersehenen NOK-Teile. |

## Fall B · Fake-Shop-Erkennung

| Phase | Antwort |
|---|---|
| **1 Kriterien** | Binäre Klassifikation bzw. Wahrscheinlichkeit „betrügerisch". Einsatz z. B. als Browser-Warnung → niedrige Latenz, **Erklärbarkeit** gegenüber Nutzer und Shopbetreiber, wenige Fehlalarme gegen seriöse Shops. Metrik: z. B. Recall bei fester False-Positive-Rate, gut **kalibrierte** Wahrscheinlichkeiten. |
| **2 Daten/Problem** | Tabellarisch gemischt (Domain-Alter, SSL, WHOIS, Textqualität, Rabattmuster, Zahlungsoptionen, Bewertungen), fehlende Werte, Rauschen, Class Imbalance, **Verteilungsverschiebung**, weil Fake-Shops echte nachahmen. |
| **3 Baselines** | Dummy: „alle seriös". Regelbasiert: z. B. Domain-Alter < x Tage. ML-Baseline: regularisierte logistische Regression. |
| **4 Shortlist** | Gradient Boosted Trees (fehlende Werte, nichtlineare Muster), logistische Regression mit L1 (sparse, erklärbar, siehe [[lasso-regression]]), ggf. Textmodell für Inhaltsmerkmale per Transfer Learning. |
| **Danach** | Zeitlich getrennte Validierung (neuere Shops als Testdaten), regelmäßiges Re-Training; lokale Erklärungen für einzelne Warnungen ([[lime]], [[shap]]). |

> [!info] Ergänzung (nicht aus der VL)
> Die konkreten Metrikvorschläge (Recall bei fester FPR, Kalibrierung, zeitlich getrennte Validierung) sind eigene Ergänzungen, die zur VL-Logik passen (Phase 1: „klare Metrik", Phase 2: „Class Imbalance").

## Prüfungsschema für offene Fragen
1. Problemtyp benennen (Klassifikation/Regression/…).
2. Datentyp, N, p/N, Qualität, Imbalance.
3. Metrik und Constraints aus dem Fachziel ableiten.
4. Dummy- und ML-Baseline.
5. 2–4 Kandidaten mit je einem Satz Begründung aus den Faustregeln.
6. Evaluierungsprotokoll (CV, stratifiziert, Test nur einmal).

## Benötigte Konzepte
- [[modellauswahl-vorgehen]]
- [[kreuzvalidierung]]
- [[train-val-test-split]]

## Quellen
- [[q-modellauswahl-abgaben]]
- [[q-vl00-essentiell|VL0]], Folien 17–34
