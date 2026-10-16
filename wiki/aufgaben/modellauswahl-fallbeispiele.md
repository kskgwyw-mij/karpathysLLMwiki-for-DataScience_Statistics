---
typ: aufgabe
themen: [modellwahl]
quellen: ["[[q-modellauswahl-abgaben]]", "[[q-vl00-essentiell]]", "[[q-vl02-bias-varianz-modellwahl]]", "[[q-vl03-regularisierung-cv]]", "[[q-lernhilfe-sl2]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-16
---

# Aufgabe · Modellauswahl an drei Fallbeispielen

Fall A und B stammen aus den eigenen Workshop-Abgaben ([[q-modellauswahl-abgaben]]), Fall C ist ein klausurtypisches Szenario aus der [[q-lernhilfe-sl2|Lernhilfe]]. Hier werden die Fälle systematisch gegen den 4-Phasen-Ablauf aus [[modellauswahl-vorgehen]] gespiegelt – so lässt sich eine offene Klausurfrage „Schlagen Sie ein Modell vor und begründen Sie" strukturiert beantworten.

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

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Die konkreten Metrikvorschläge (Recall bei fester FPR, Kalibrierung, zeitlich getrennte Validierung) sind eigene Ergänzungen, die zur VL-Logik passen (Phase 1: „klare Metrik", Phase 2: „Class Imbalance").

## Fall C · Genexpression

> [!beispiel] Klausurszenario (nicht aus der VL) – laut [[q-lernhilfe-sl2|Lernhilfe]], S. 7
> **Aufgabe:** 500 Patienten, 2000 Gen-Merkmale, das Ergebnis muss erklärbar sein. Schlagen Sie eine Methode vor und begründen Sie sie.
>
> Der Zielwert ist nicht genannt. Hier angenommen: Ansprechen auf eine Therapie ja/nein (Klassifikation). Für einen stetigen Zielwert gilt dasselbe mit linearer statt logistischer Regression. In der Klausur die Annahme hinschreiben.
>
> | Phase | Antwort |
> |---|---|
> | **1 Kriterien** | Binäre Klassifikation. Erklärbarkeit ist Pflicht, denn die Klinik will wissen, welche Gene zählen → kleines, interpretierbares Modell (VL0, F. 28). Metrik z. B. AUC oder Recall der Responder, je nach Kosten der Fehler; Klassenverhältnis prüfen. |
> | **2 Daten/Problem** | Tabellarisch, $N = 500$, $p = 2000$: **wenig Daten** im Sinn von $N < 1000$ **und** $N < p$ (VL0, F. 22). Viele Gene sind korreliert, die meisten irrelevant; dazu Messrauschen. |
> | **3 Baselines** | Dummy: immer die häufigere Klasse. ML-Baseline: logistische Regression mit L2 – ohne Regularisierung gibt es bei $p > N$ keine eindeutige bzw. endliche Lösung. |
> | **4 Shortlist** | **L1-regularisierte logistische Regression (Lasso):** sparse, wählt wenige Gene aus und bleibt erklärbar ([[lasso-regression]]). **Elastic Net** (L1 + L2, VL3, F. 23), wenn korrelierte Gene gemeinsam ausgewählt werden sollen. Als Vergleich ein kleiner Entscheidungsbaum oder eine SVM (VL0, F. 22). |
> | **Danach** | $\lambda$ per stratifizierter 5- bis 10-facher CV auf einem Log-Gitter wählen (VL0, F. 22, 55–56), über 3–5 Seeds mitteln, Test-Set nur einmal verwenden. Stabilität der ausgewählten Gene per [[bootstrap]] prüfen: Wie oft bleibt ein Gen ausgewählt? (VL0, F. 66) |
>
> **Begründungskette** (Denkweg der Lernhilfe): Klassifikation → $p \gg N$ → lineares Modell, starke Regularisierung, Merkmalsauswahl (VL0, F. 25) → Erklärbarkeit → kleines lineares Modell (F. 28) → L1 erfüllt beides auf einmal. Mit einer Baseline beginnen.
>
> **Fallstrick:** Die Gene **vor** der CV auf allen Daten vorzusortieren (z. B. die 100 mit der höchsten Korrelation zum Label) ist Leakage. ESL zeigt das an 50 Proben mit 5000 reinen Rauschmerkmalen: Die CV schätzt 3 % Fehler, der wahre Fehler liegt bei 50 %. Die Merkmalsauswahl gehört in jeden CV-Fold (ESL 7.10.2, S. 245; [[train-val-test-split]]).

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
- [[q-vl00-essentiell|VL0]], Folien 17–34, 55–56, 66
- [[q-vl03-regularisierung-cv|VL3]], Folie 23 (Elastic Net)
- [[q-lernhilfe-sl2|Lernhilfe]], S. 7 (Szenario Fall C)
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 7.10.2, S. 245
