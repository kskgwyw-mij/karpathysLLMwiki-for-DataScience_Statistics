---
typ: methode
themen: [resampling, modellwahl]
aliases: [Cross-Validation, Kreuzvalidierung, CV, K-Fold, Leave-One-Out, LOOCV, Stratified CV, Hold-Out]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Kreuzvalidierung (Cross-Validation)

#flashcards/statistik

## Kurzdefinition
Techniken, um herauszufinden, wie ein Machine-Learning-Modell auf **ungesehenen Daten** performt. Der Datensatz wird in Teile geteilt; das Modell wird auf einigen trainiert und auf den anderen getestet; das wird mit wechselnden Teilen wiederholt; die finale Performance ist der **Durchschnitt** (→ [[q-vl00-essentiell|VL0]], F. 59–60).

## Intuition
Ein einzelner Train/Test-Split ist Glückssache – je nachdem, welche Punkte im Test landen, schwankt der Fehler. Durch Rotieren wird jeder Punkt einmal zum Testpunkt, und das Mittel über mehrere Splits ist eine stabilere Schätzung der Generalisierung.

## Einsatzzwecke (→ VL0, F. 59)
- Modell **evaluieren**
- Modell aus verschiedenen Modellen **auswählen**
- **Parameter-Tuning** (z. B. $\lambda$)
- **Overfitting vermeiden**

## Varianten (→ VL0, F. 61)
| Variante | Vorgehen | Trainingsläufe |
|---|---|---|
| **Hold-Out** | einmalige Teilung, typischerweise 70–80 % Fit, 20–30 % Test | 1 |
| **K-Fold** | Datensatz in $K$ gleiche Teile; auf $K-1$ Folds trainieren, auf dem übrigen testen; $K$-mal mit wechselndem Test-Fold; Durchschnitt der $K$ Durchgänge | $K$ |
| **Stratified** | wie K-Fold, aber der Prozentsatz der Klassen ist in allen Splits gleich | $K$ |
| **Leave-One-Out** | jeder Datenpunkt einmal Test, Rest Training | $N$ |

**K-Fold-Schätzer:**
$$\mathrm{CV}_K = \frac1K\sum_{k=1}^K \mathrm{Err}_k, \qquad \mathrm{Err}_k = \text{Fehler auf Fold } k \text{ eines Modells, das ohne Fold } k \text{ trainiert wurde}$$

> [!info] Ergänzung (nicht aus der VL)
> - LOOCV hat wenig Bias (Training auf $N-1$ Punkten), aber hohe Varianz und hohe Kosten; $K = 5$ oder $10$ ist ein üblicher Kompromiss (ESL 7.10).
> - Für Zeitreihen nicht zufällig mischen, sondern zeitlich geordnet splitten.
> - Bei Hyperparameter-Tuning **und** ehrlicher Bewertung: verschachtelte (nested) CV oder separates Test-Set.

## Annahmen & Voraussetzungen
- Beobachtungen (annähernd) unabhängig und gleich verteilt.
- **Gesamte Pipeline** (Standardisierung, Feature-Selektion) in jedem Fold neu fitten, sonst Leakage ([[train-val-test-split]]).

## Beispiel
Faustregel der VL bei wenig Daten: „viel Cross-Validation (z. B. 5–10-fold)" (→ VL0, F. 22). Für die Regularisierung: $\lambda$ per CV bestimmen (→ VL0, F. 54; [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 13). Bei 100 Punkten und 5-Fold: fünfmal auf 80 trainieren, auf 20 testen, fünf Fehler mitteln.

## Typische Prüfungsfragen & Fallstricke
- K-Fold beschreiben und den Unterschied zu Stratified und LOOCV erklären.
- „Wie oft wird bei LOOCV trainiert?" → $N$-mal.
- „Wofür CV, wofür Bootstrap?" → [[cv-vs-bootstrap]]: CV wird für die Modellauswahl oft bevorzugt (→ VL0, F. 67).
- Fallstrick: Standardisierung vor dem Split auf allen Daten.
- Fallstrick: Bei Klassifikation mit seltenen Klassen nicht stratifizieren → Folds ohne Positive.

## Verwandt
- [[train-val-test-split]] – Hyperparameterprozess, Leakage
- [[bootstrap]] – alternative Resampling-Methode
- [[cv-vs-bootstrap]] – Gegenüberstellung
- [[bias-varianz-tradeoff]] – CV als Methode zur Balance
- [[modellauswahl-vorgehen]] – fairer Vergleich mit identischem Protokoll

## Karteikarten
Was versteht man unter Cross-Validation?::Techniken, um abzuschätzen, wie ein Modell auf ungesehenen Daten performt, durch wiederholtes Trainieren und Testen auf wechselnden Teilen.
Vier Einsatzzwecke von CV?::Modell evaluieren, Modell auswählen, Parameter-Tuning, Overfitting vermeiden.
Wie funktioniert K-Fold-CV?::Daten in $K$ gleiche Teile; $K$-mal auf $K-1$ Teilen trainieren, auf dem übrigen testen; Ergebnis mitteln.
Was ist Stratified CV?::K-Fold, bei dem der Klassenanteil in allen Splits gleich ist.
Wie oft wird bei LOOCV trainiert?::$N$-mal (jeder Punkt einmal Test).
Typische Aufteilung beim Hold-Out?::70–80 % Training, 20–30 % Test.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 58–61
- [[q-vl03-regularisierung-cv|VL3]], Folien 45–48
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 12–15
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 7.10, S. 241
