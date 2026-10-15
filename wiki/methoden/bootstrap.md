---
typ: methode
themen: [resampling, unsicherheit]
aliases: [Bootstrapping, Bootstrap, Out-of-Bag, OOB, Bootstrap-Konfidenzintervall, BCa]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]", "[[q-esl-elements-of-statistical-learning]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Bootstrap

#flashcards/statistik

## Kurzdefinition
Resampling-Verfahren, um **Unsicherheit zu quantifizieren, ohne eine geschlossene Stichprobenverteilung** zu kennen: Man behandelt den beobachteten Datensatz als empirische Population, zieht wiederholt $n$ Beobachtungen **mit Zurücklegen**, fittet das Modell neu und berechnet die Statistik neu. Die Streuung über die Bootstrap-Replikate approximiert die Unsicherheit des Schätzers (→ [[q-vl00-essentiell|VL0]], F. 62–63).

## Intuition
Wüsste man die wahre Verteilung, könnte man beliebig viele neue Datensätze ziehen und sehen, wie stark ein Schätzer schwankt. Man hat aber nur einen Datensatz – also tut man so, als wäre er die Population, und „simuliert" neue Datensätze durch Ziehen mit Zurücklegen. Manche Punkte kommen mehrfach vor, andere gar nicht.

## Formal

**Nichtparametrischer Bootstrap** (→ VL0, F. 64):
```
Input: Daten D = {z₁, …, zₙ}, Statistik S, Anzahl Replikate B
für b = 1, …, B:
    Dᵇ ← n Beobachtungen aus D mit Zurücklegen
    sᵇ ← S(Dᵇ)
return {s¹, …, sᴮ}
```
- Standardfehler $= \mathrm{std}(\{s^b\})$
- Perzentil-Konfidenzintervall = Quantile von $\{s^b\}$
- **Die gesamte Lernpipeline gehört in $S$**: Preprocessing, Feature-Selektion, ggf. Tuning.

**Konfidenzintervalle** (→ VL0, F. 65):
| Typ | Formel / Idee | Annahme / Einsatz |
|---|---|---|
| Normal-Approximation | Statistik $\pm 1{,}96\times$ Bootstrap-Standardfehler | Normalverteilung |
| Perzentil-Intervall | untere/obere Bootstrap-Quantile direkt (z. B. 2,5 % und 97,5 %) | – |
| BCa | bias-corrected and accelerated; genauer, komplexer | wenn die Bootstrap-Verteilung verzerrt oder schief ist |

Bei Performance-Schätzungen muss das Resampling Datenstruktur und Leakage-Risiken respektieren.

**Bootstrap-Modellevaluation mit Out-of-Bag** (→ VL0, F. 67):
```
für b = 1, …, B:
    D_trainᵇ ← Bootstrap-Stichprobe aus D
    D_oobᵇ   ← Beobachtungen, die nicht gezogen wurden
    θᵇ ← A(D_trainᵇ);  mᵇ ← M(θᵇ, D_oobᵇ)
return mean({mᵇ}), CI({mᵇ})
```
OOB-Evaluation ist bequem, für die **Modellauswahl wird aber oft Cross-Validation bevorzugt**.

> [!info] Ergänzung (nicht aus der VL) – Anteil Out-of-Bag
> Eine bestimmte Beobachtung wird bei einem Zug mit Wahrscheinlichkeit $1 - 1/n$ **nicht** gezogen, bei $n$ Zügen also mit $(1 - 1/n)^n \to e^{-1} \approx 0{,}368$. Eine Bootstrap-Stichprobe enthält damit im Mittel nur ca. **63,2 %** der verschiedenen Beobachtungen; ca. **36,8 %** sind OOB (ESL 7.11). Rechenweg: [[bootstrap-oob-anteil]].

## Einsatz im statistischen Lernen (→ VL0, F. 66)
- **Koeffizienten-Unsicherheit:** Wie stabil sind die geschätzten Parameter?
- **Feature Importance:** Wie oft bleibt eine Variable über die Stichproben wichtig?
- **Vorhersage-Unsicherheit:** Verteilung der Vorhersagen über Bootstrap-Modelle.
- **Modellevaluation:** Bootstrap-Verteilung von AUC, Accuracy, RMSE, MAE …
- **Bagging:** Bootstrap-trainierte Modelle mitteln, um die Varianz zu reduzieren → [[bagging]].
- Fairness: Bootstrap-Intervalle für kleine Subgruppen ([[fairness-mitigation]]); Stabilität von Baumpfaden ([[entscheidungsbaeume]]).

## Annahmen & Voraussetzungen
- Beobachtungen i. i. d.; bei Zeitreihen oder gruppierten Daten Block- bzw. Gruppen-Bootstrap (Datenstruktur respektieren).

## Typische Prüfungsfragen & Fallstricke
- Pseudocode des nichtparametrischen Bootstraps aufschreiben.
- Drei Arten von Bootstrap-Konfidenzintervallen nennen.
- „Was ist Out-of-Bag?" → nicht gezogene Beobachtungen, ca. 36,8 %.
- Fallstrick: **ohne** Zurücklegen ziehen → immer derselbe Datensatz.
- Fallstrick: Preprocessing nur einmal auf allen Daten statt in jedem Replikat → Leakage, zu schmale Intervalle.

## Verwandt
- [[bagging]] – Bootstrap + Aggregation
- [[kreuzvalidierung]] – Alternative für die Modellauswahl
- [[cv-vs-bootstrap]] – Gegenüberstellung
- [[transparente-modelle]] – Bootstrap-Intervalle für Koeffizienten
- [[probabilistische-sicht-map]] – Bayes'sche Alternative

## Karteikarten
Was ist die Kernidee des Bootstraps?::Den beobachteten Datensatz als empirische Population behandeln, wiederholt $n$ Beobachtungen mit Zurücklegen ziehen, Statistik neu berechnen; die Streuung approximiert die Unsicherheit.
Wie berechnet man den Bootstrap-Standardfehler?::Standardabweichung der Bootstrap-Statistiken $\{s^b\}$.
Was ist ein Perzentil-Konfidenzintervall?::Unteres und oberes Quantil der Bootstrap-Verteilung direkt als Grenzen.
Wann verwendet man BCa-Intervalle?::Wenn die Bootstrap-Verteilung verzerrt oder schief ist (bias-corrected and accelerated).
Normal-Approximation des Bootstrap-CI?::Statistik $\pm 1{,}96\times$ Bootstrap-Standardfehler (Annahme Normalverteilung).
Was gehört in die Statistik $S$ beim Bootstrap?::Die gesamte Lernpipeline: Preprocessing, Feature-Selektion, ggf. Tuning.
Was ist die Out-of-Bag-Menge?::Die Beobachtungen, die in einer Bootstrap-Stichprobe nicht gezogen wurden (ca. 36,8 %).
Fünf Einsatzzwecke des Bootstraps im statistischen Lernen?::Koeffizienten-Unsicherheit, Feature-Importance-Stabilität, Vorhersage-Unsicherheit, Modellevaluation, Bagging.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 62–67
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 16–21
- [[q-esl-elements-of-statistical-learning|ESL]], Abschnitte 7.11 (S. 249) und 8.2 (S. 261)
