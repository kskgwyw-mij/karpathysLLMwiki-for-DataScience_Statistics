---
typ: methode
themen: [xai, baeume, klassifikation]
aliases: [Decision Tree, Entscheidungsbaum, CART, Baumtiefe, Pruning]
quellen: ["[[q-vl05-xai-transparente-globale-modelle]]", "[[q-nb05-tree-exam-grade]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Entscheidungsbäume

#flashcards/statistik

## Kurzdefinition
Ein Entscheidungsbaum **partitioniert den Merkmalsraum rekursiv**; jeder Split wählt einen Schwellenwert, der ein Fehlermaß reduziert. Die Erklärung einer Vorhersage ist der **Pfad von der Wurzel zum Blatt** – eine Konjunktion von Bedingungen (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 14).

## Intuition
Wie ein Fragenkatalog: „Probeklausur ≤ 64,5 Punkte? Wenn ja: Übungsschnitt ≤ 60? …" Jede Antwort grenzt die Gruppe ein, am Ende steht eine Vorhersage.

## Formal
- Split: Merkmal $x_j$ und Schwelle $s$ so, dass die Unreinheit (Klassifikation) bzw. der Fehler (Regression) in den beiden Kindknoten möglichst stark sinkt.
- Steuergrößen: **Tiefe, minimale Blattgröße, Pruning** → steuern Varianz und Interpretierbarkeit.
- **Feature Importance** eines Baums = Beitrag zur Reduktion der Unreinheit; **nicht kausal** ([[q-nb05-tree-exam-grade|NB5]], Zelle 18).

**Eigenschaften** (→ VL5, F. 14):
- Bäume sind **Schätzer mit hoher Varianz**: Kleine Datenänderungen können frühe Splits und damit die „Story" verändern.
- Flache Bäume eignen sich besser zur Erklärung; **wiederholte Splits oder Bootstraps** prüfen die Pfadstabilität.

## Annahmen & Voraussetzungen
- keine Linearitätsannahme, keine Skalierung nötig; achsenparallele Splits.

## Beispiel ([[q-nb05-tree-exam-grade|NB5]])
Note 1–5 aus synthetischen Lerndaten, `class_weight="balanced"`, `min_samples_leaf=12`:

| Tiefe | Accuracy | Balanced Acc. | Blätter |
|---|---|---|---|
| 2 | 0,461 | 0,495 | 4 |
| 3 | 0,561 | 0,514 | 8 |
| 4 | 0,516 | 0,554 | 16 |

Tiefe 2 als Regeln:
```
mock_exam_score ≤ 64.5
├─ assignment_avg ≤ 60.0 → Note 5
└─ assignment_avg > 60.0 → Note 4
mock_exam_score > 64.5
├─ cohort_VZ ≤ 0.5 → Note 1
└─ cohort_VZ > 0.5 → Note 2
```
Beobachtungen: Tiefe 2 sagt Note 3 nie vorher; Accuracy und Balanced Accuracy empfehlen verschiedene Tiefen; der Split nach **Kohorte** (berufsbegleitend vs. Vollzeit) ist fachlich fragwürdig und ein mögliches Proxy-/Fairnessrisiko.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – besten Split mit Gini wählen
> Gini-Unreinheit $G = 1 - \sum_k p_k^2$. Knoten mit 10 Kunden: 6 bleiben, 4 kündigen ⇒ $G = 1 - (0{,}6^2 + 0{,}4^2) = 0{,}48$.
>
> | Split | links (bleibt/kündigt) | rechts | $G_\text{links}$ | $G_\text{rechts}$ | gewichtet | Reduktion |
> |---|---|---|---|---|---|---|
> | Vertrag monatlich? | 1 / 4 | 5 / 0 | 0,32 | 0 | $0{,}5\cdot0{,}32 = 0{,}16$ | **0,32** |
> | Alter > 40? | 4 / 2 | 2 / 2 | 0,444 | 0,5 | $0{,}6\cdot0{,}444 + 0{,}4\cdot0{,}5 \approx 0{,}467$ | 0,013 |
>
> Der Baum wählt „Vertrag monatlich". Die Summe dieser Reduktionen über alle Splits eines Merkmals ist seine Baum-Feature-Importance – sie misst Beitrag zur Reinheit, keinen kausalen Effekt.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Triage-Hilfe in der Notaufnahme
> Ein Baum der Tiefe 3 ordnet Patienten Prioritäten zu, z. B. „Sauerstoffsättigung < 92 % → hoch; sonst Alter > 75 **und** Fieber → mittel; sonst niedrig". Pflegekräfte können jede Einstufung nachvollziehen und Fehler melden. Vor dem Einsatz: auf Bootstrap-Stichproben neu trainieren – wechselt der erste Split zwischen Sättigung und Atemfrequenz, ist die „Geschichte" instabil (VL5, F. 14) und sollte nicht als feste Regel kommuniziert werden.

## Typische Prüfungsfragen & Fallstricke
- Pfad einer Person durch einen gegebenen Baum verfolgen.
- „Warum sind Bäume instabil?" → hohe Varianz, frühe Splits hängen stark von den Daten ab; Abhilfe: [[bagging]]/Random Forest (aber weniger interpretierbar).
- Reflexion aus NB5: Reicht ein Baumpfad als kausaler Beweis? → Nein.
- Fallstrick: Feature Importance als Ursache lesen („Arbeit neben dem Studium verursacht schlechtere Noten").

## Verwandt
- [[transparente-modelle]] – Transparenzspektrum
- [[bagging]] – Varianzreduktion
- [[bias-varianz-tradeoff]] – Tiefe als Komplexität
- [[fairness-grundlagen]] – Proxys in Splits
- [[permutation-feature-importance]] – modellagnostische Alternative zur Impurity-Importance

## Karteikarten
Wie funktioniert ein Entscheidungsbaum?::Er partitioniert den Merkmalsraum rekursiv; jeder Split wählt eine Schwelle, die ein Fehlermaß reduziert.
Was ist die Erklärung einer Baumvorhersage?::Der Pfad von der Wurzel zum Blatt – eine Konjunktion von Bedingungen.
Welche Größen steuern Varianz und Interpretierbarkeit eines Baums?::Tiefe, minimale Blattgröße, Pruning.
Warum sind Bäume Schätzer mit hoher Varianz?::Kleine Datenänderungen können frühe Splits und damit die ganze Struktur verändern.
Wie prüft man die Stabilität eines Baumpfads?::Mit wiederholten Splits oder Bootstrap-Stichproben.
Ist die Feature Importance eines Baums kausal?::Nein, sie misst den Beitrag zur Unreinheitsreduktion.

## Quellen
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folien 14, 17
- [[q-nb05-tree-exam-grade|NB5]]
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitt 9.2, S. 305
