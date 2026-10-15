---
typ: methode
themen: [fairness]
aliases: [Fairness-Mitigation, Fairness-Audit, Reweighting, Resampling, Pre-processing, In-processing, Post-processing, Schwellenwerte, Threshold, Model Cards]
quellen: ["[[q-vl06-xai-lokal-fairness]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Fairness in der Praxis: Audit und Mitigation

#flashcards/statistik

## Kurzdefinition
Ein Fairness-Audit braucht **Umfang, Datenverständnis, Subgruppenmetriken, Unsicherheit und eine Entscheidungsempfehlung**. Mitigation setzt **vor** (pre-processing), **während** (in-processing) oder **nach** dem Training (post-processing) an; jede Variante hat eigene technische und Governance-Kosten (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 34).

## Intuition
Fairness lässt sich nicht einmalig „einschalten". Man muss dokumentieren, wen das Modell betrifft, für Subgruppen mit Unsicherheit messen, bewusst einen Trade-off wählen und nach dem Deployment weiter überwachen.

## Daten- und Modelldokumentation (→ VL6, F. 26)
- **Zielpopulation** definieren: Wer ist einbezogen, unterrepräsentiert, ausgeschlossen?
- **Label-Erstellung** dokumentieren: Wer hat gelabelt, nach welchen Kriterien, mit welchem historischen Bias-Risiko?
- **Proxy-Merkmale** und Preprocessing-Schritte identifizieren, die Gruppen unterschiedlich betreffen.
- Train/Val/Test-Splits, Zeiträume, Feature- und Modellversionen dokumentieren.
- **Model Cards** oder Auditberichte: beabsichtigte Nutzung, untersagte Nutzung, bekannte Grenzen.

## Subgruppen, Schnittmengen, Unsicherheit (→ VL6, F. 27)
- **Schnittmengen** analysieren (z. B. Alter × Geschlecht), nicht nur ein Attribut nach dem anderen.
- Kleine Subgruppen → breite Unsicherheitsintervalle; verrauschte Punktschätzungen nicht überbewerten.
- **Bootstrap-Intervalle** bei kleinen Stichproben ([[bootstrap]]).
- Aggregierte Fairness kann schwere Subgruppenschäden **verbergen**.
- Sehr kleine Gruppen: quantitative Audits mit qualitativer Prüfung und Datenstrategie kombinieren.

## Schwellenwerte als Fairness-Hebel (→ VL6, F. 28)
- Viele Modelle erzeugen Scores; die Klassifikationsentscheidung entsteht erst durch die **Wahl eines Schwellenwerts**.
- Ein einzelner globaler Schwellenwert kann unterschiedliche TPR, FPR, PPV oder Auswahlraten über Gruppen erzeugen.
- **Threshold Sweeps** zeigen den Performance-Fairness-Trade-off über mögliche Entscheidungsrichtlinien.
- **Gruppenspezifische Schwellenwerte** können manche Metriken verbessern, werfen aber rechtliche, ethische und kommunikative Bedenken auf.
- Schwellenwertentscheidungen mit Kostenannahmen, Dokumentation und Verantwortlichkeit treffen.

## Mitigationsstrategien (→ VL6, F. 29–31)
| Ansatz | Was wird verändert | Mechanismus | Vorteile | Risiken |
|---|---|---|---|---|
| **Pre-processing** | Datenverteilung oder Stichprobengewichte | **Reweighting** (größere Gewichte für unterrepräsentierte Gruppe-Label-Kombinationen), **Resampling** (Over-/Undersampling, synthetische Daten) | modellagnostisch, leicht mit transparenten Baselines kombinierbar | Overfitting kleiner Gruppen, verzerrte Kalibrierung, schlechte Entscheidungsgrenzen |
| **In-processing** | Trainingsziel | Strafterm $\text{loss} + \lambda\times\text{fairness\_violation}$ oder Nebenbedingung | macht den Trade-off explizit | braucht teils differenzierbare Approximationen; modellabhängig |
| **Post-processing** | Scores, Schwellen, Entscheidungen | Schwellen anpassen, Scores kalibrieren, Entscheidungen randomisieren | nützlich für Legacy-Modelle | kann individuelle Erklärungen erschweren |

Jede Strategie kann eine Fairness-Metrik verbessern und zugleich Kalibrierung, Accuracy oder Interpretierbarkeit verschlechtern. **Mitigation auf Hold-out-Daten validieren und nach dem Deployment überwachen** (→ VL6, F. 29). Dokumentieren: $\lambda$-Werte, Schwellenwerte, Kalibrierungsmethode, ob Gruppeninformation verwendet wurde (→ VL6, F. 31).

## Trade-off-Kurven statt Einzelzahlen (→ VL6, F. 32)
- Leistungs- und Fairness-Metriken über Schwellenwerte, $\lambda$-Werte oder Mitigation-Einstellungen plotten.
- **Validierungsdaten** zur Auswahl einer Richtlinie, **Testdaten** für das finale Reporting ([[train-val-test-split]]).
- Unsicherheitsintervalle hinzufügen, insbesondere für Subgruppenraten.
- Eine Kurve macht die normative Entscheidung **sichtbar**, statt sie in einem gewählten Schwellenwert zu verstecken.

## Validierung nach der Mitigation (→ VL6, F. 33)
- Mitigation kann Validierungsdaten overfitten → Robustheit über Seeds, Zeitfenster und Modellklassen prüfen.
- Mit **transparenten Baselines** vergleichen, nicht nur mit dem ursprünglichen komplexen Modell.
- **Kalibrierung erneut prüfen** – Fairness-Anpassungen können die Bedeutung von Scores verzerren.
- Lokale Erklärungen nach der Mitigation untersuchen: Ist die Entscheidungslogik schwerer kommunizierbar?
- Restrisiken und Bedingungen für ein **Re-Audit** dokumentieren.

## Typische Prüfungsfragen & Fallstricke
- Pre-, In- und Post-processing unterscheiden mit je einem Mechanismus und einem Risiko.
- „Warum Trade-off-Kurven statt eines Schwellenwerts berichten?"
- Fallstrick: Subgruppenmetriken ohne Unsicherheit bei kleinen Gruppen.
- Fallstrick: Mitigation auf dem Test-Set tunen.

## Verwandt
- [[fairness-metriken]] – was gemessen wird
- [[fairness-grundlagen]] – Schadensarten, Proxys
- [[restringierte-optimierung]] – Fairness als Nebenbedingung
- [[bootstrap]] – Intervalle für Subgruppen
- [[transparente-modelle]] – Vergleichsbaseline

## Karteikarten
Was braucht ein Fairness-Audit laut VL?::Umfang, Datenverständnis, Subgruppenmetriken, Unsicherheit und eine Entscheidungsempfehlung.
Was sind Pre-, In- und Post-processing?::Pre: Daten/Gewichte vor dem Training ändern; In: Fairness-Strafterm/Nebenbedingung im Training; Post: Scores, Schwellen oder Entscheidungen nach dem Training anpassen.
Was ist Reweighting?::Beobachtungen aus unterrepräsentierten Gruppe-Label-Kombinationen bekommen größere Gewichte.
Risiken von Reweighting/Resampling?::Overfitting kleiner Gruppen, verzerrte Kalibrierung, schlechte Entscheidungsgrenzen.
Wie sieht In-processing typischerweise aus?::$\text{loss}+\lambda\times\text{fairness\_violation}$.
Nachteil von Post-processing?::Kann individuelle Erklärungen erschweren.
Warum Schnittmengen analysieren?::Aggregierte Fairness kann schwere Schäden in Subgruppen (z. B. Alter × Geschlecht) verbergen.
Warum Trade-off-Kurven statt Einzelzahlen?::Sie machen die normative Entscheidung sichtbar, statt sie in einem Schwellenwert zu verstecken.
Was gehört in eine Model Card?::Beabsichtigte Nutzung, untersagte Nutzung, bekannte Grenzen.

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 25–34
