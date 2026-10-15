---
typ: konzept
themen: [resampling, modellwahl, hyperparameter]
aliases: [Train/Val/Test, Hyperparameter-Auswahl, Hyperparameter Tuning, Data Leakage, Leakage]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-nb05-tree-exam-grade]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Train/Val/Test-Split und Hyperparameter-Auswahl

#flashcards/statistik

## Kurzdefinition
Die Daten werden sauber in **Training** (Parameter fitten), **Validierung** (Hyperparameter und Modell wählen) und **Test** (einmaliges, unverzerrtes Reporting) getrennt; bei Klassifikation **stratifiziert** (→ [[q-vl00-essentiell|VL0]], F. 56).

## Intuition
Jede Entscheidung, die man anhand einer Datenmenge trifft, „verbraucht" diese Daten für eine ehrliche Bewertung. Wählt man $\lambda$ nach dem Testfehler, ist der Testfehler zu optimistisch. Deshalb drei Töpfe – und der Test-Topf wird erst ganz am Ende einmal geöffnet.

## Formal: Auswahlprozess (→ VL0, F. 55–56)
1. **Split:** Train/Val/Test sauber trennen; bei Klassifikation stratifizieren.
2. **Grob sweepen:** pro Regularisierer separat, $\lambda$ auf einem **logarithmischen** Gitter (z. B. $10^{-6}, 10^{-5}, \dots, 10^{-2}$).
3. **Verfeinern:** Kombinationen nur in den Top-2–3-Bereichen.
4. **Mitteln:** über 3–5 Seeds bei kleinem N.
5. **Final:** Modell auf Train+Val neu trainieren; Test-Set **nur einmalig** reporten.

**Normierung und Suche** (→ VL0, F. 55): Output-Regularisierer durch Anzahl der Ausgabekomponenten teilen ($\lambda$ wird skaleninvariant); bei Summen über Samples den Mittelwert verwenden; coarse-to-fine; für die Grobsuche wenige Epochen (budgetiert); stabile Optimierer und Early Stopping.

**Praktische Shortcuts** (→ [[q-vl03-regularisierung-cv|VL3]], F. 44): Inputs standardisieren; Skalen von Datenterm und Regularisierungsterm prüfen; Regularisierungsterm pro Batch mitteln, nicht summieren; alle $\lambda$ und Seeds loggen, Trainingskurven speichern, reproduzierbare Splits.

## Data Leakage
Informationen aus dem Ziel oder aus Test-/Validierungsdaten gelangen ins Training.
- Im Baum-Notebook werden `exam_points` und `passed` ausgeschlossen, weil sie direkt aus der Zielvariable `exam_grade` abgeleitet sind (→ [[q-nb05-tree-exam-grade|NB5]], Zelle 6).
- Beim Bootstrap/Resampling muss die **gesamte** Pipeline (Preprocessing, Feature-Selektion, Tuning) innerhalb jeder Wiederholung laufen (→ VL0, F. 64–65).

> [!info] Ergänzung (nicht aus der VL)
> Typische Leakage-Quellen: Standardisierung auf dem gesamten Datensatz statt nur auf dem Training, Feature-Selektion vor der CV, zeitliche Daten zufällig statt chronologisch gesplittet.

## Annahmen & Voraussetzungen
- Train, Val und Test stammen aus derselben Verteilung wie die späteren Einsatzdaten.

## Typische Prüfungsfragen & Fallstricke
- „Beschreiben Sie den Auswahlprozess für $\lambda$." → fünf Schritte oben.
- „Warum logarithmisches Gitter?" → $\lambda$ wirkt über Größenordnungen; linear würde man fast nur große Werte testen.
- Fallstrick: Hyperparameter auf dem Test-Set wählen.
- Fallstrick: Das finale Modell nicht auf Train+Val neu trainieren (verschenkte Daten).

## Verwandt
- [[kreuzvalidierung]] – ersetzt den festen Val-Split durch mehrere Folds
- [[regularisierung]] – wichtigster Hyperparameter $\lambda$
- [[early-stopping]] – nutzt den Val-Split als Stoppkriterium
- [[bootstrap]] – Pipeline innerhalb der Resamples

## Karteikarten
Wozu dienen Train-, Val- und Test-Set?::Train: Parameter fitten; Val: Hyperparameter/Modell wählen; Test: einmaliges unverzerrtes Reporting.
Wie oft darf das Test-Set ausgewertet werden?::Nur einmal, am Ende.
Auf welchem Gitter sucht man $\lambda$?::Auf einem logarithmischen Gitter, coarse-to-fine.
Was tut man nach der Hyperparameterwahl mit Train und Val?::Finales Modell auf Train+Val neu trainieren.
Über wie viele Seeds soll man bei kleinem N mitteln?::3–5 Seeds.
Was ist Data Leakage?::Information aus dem Ziel oder aus Val/Test gelangt ins Training, z. B. ein aus dem Ziel abgeleitetes Feature.
Warum Regularisierungsterm pro Batch mitteln statt summieren?::Damit $\lambda$ nicht von Batchgröße bzw. Anzahl Komponenten abhängt (skaleninvariant).

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 55–57, 64–65
- [[q-vl03-regularisierung-cv|VL3]], Folien 40–44
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 7–11
- [[q-nb05-tree-exam-grade|NB5]]
