---
typ: quelle
themen: [bias-varianz, modellwahl]
aliases: [VL2]
datei: "raw/vorlesung/vl02-bias-varianz-modellwahl.pdf"
original: "3_Slides/2_Statistisches_Lernen_2_v2.pdf"
relevanz: hoch
aktualisiert: 2026-10-15
---

# VL2 · Bias-Varianz-Trade-off und Modellwahl (27 Folien)

Folie = PDF-Seite.

## Zusammenfassung
Wiederholung der Basisfunktions-Regression, dann Kapitel 2 Teil 1: Zielkonflikt zwischen Datenfit und Generalisierung, Zerlegung des Fehlers in Bias², Varianz und irreduziblen Fehler inklusive vollständiger Herleitung (als „nur für Interessierte" markiert), Methoden zur Balance. Danach ein ausführlicher Leitfaden „Wie wähle ich die passende ML-Methode?" mit Faustregeln und einem 4-Phasen-Ablauf sowie der Workshop-Anleitung zur Modellauswahl.

## Gliederung
| Folien | Inhalt | Wiki-Seiten |
|---|---|---|
| 1 | Wiederholung Basisfunktionen | [[basisfunktionen-regression]] |
| 3–4 | Ziel/Problem, Fehlerzerlegung | [[bias-varianz-tradeoff]] |
| 5–6 | Herleitung MSE-Zerlegung („nur für Interessierte") | [[bias-varianz-herleitung]] |
| 7–8 | Ergebnisformel, Methoden zur Balance | [[bias-varianz-tradeoff]] |
| 9–26 | Modellwahl: Problem, Daten, Faustregeln, Phasen 1–4, danach | [[modellauswahl-vorgehen]] |
| 27 | Workshop Modellauswahl (5 Schritte) | [[modellauswahl-fallbeispiele]] |

## Prüfungsrelevante Aussagen
- $\mathrm{MSE}(x) = \big(f(x) - \mathbb E[\hat f(x)]\big)^2 + \mathbb E\big[(\mathbb E[\hat f(x)] - \hat f(x))^2\big] + \sigma^2$ (Folie 7).
- Bias = Abweichung des „mittleren" geschätzten Modells vom wahren Modell; Varianz = Sensitivität bezüglich der Trainingsdaten (Folie 7).
- Balance über Cross-Validation, Regularisierung, Feature-Selektion, Ensembles (Folie 8).

## Widersprüche / Abweichungen
- Folie 7 (und VL0 Folie 15): Im Satz „Bias Term: Der … des geschätzten Modells" fehlt ein Wort (gemeint: Abstand/Abweichung).

## Offene Fragen
- Wird die Herleitung (Folien 5–6) geprüft? Sie ist ausdrücklich „nur für Interessierte" markiert → `relevanz: niedrig` für die Herleitung, `hoch` für das Ergebnis.
