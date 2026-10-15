---
typ: methode
themen: [xai, lokal, fairness]
aliases: [Kontrafaktuale, Counterfactual Explanations, Kontrafaktische Erklärung, Algorithmischer Recourse, Recourse]
quellen: ["[[q-vl06-xai-lokal-fairness]]", "[[q-nb07-local-xai]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Kontrafaktische Erklärungen und algorithmischer Recourse

#flashcards/statistik

## Kurzdefinition
Eine kontrafaktische Erklärung beantwortet: **Welche kleine Änderung an $x$ würde die Modellentscheidung kippen** (z. B. von negativ zu positiv)? **Algorithmischer Recourse** fragt weiter, ob eine Person das Ergebnis unter institutionellen Regeln **realistisch** ändern kann (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 10–11).

## Intuition
„Ihr Kredit wäre bewilligt worden, wenn Ihr Einkommen 300 € höher gewesen wäre." Das ist für Betroffene oft nützlicher als eine Liste von Gewichten – aber nur, wenn die Änderung machbar, legal und fair ist.

## Formal
**Optimierungssicht** (→ VL6, F. 10):
$$\min_{x'}\ d(x, x') \quad\text{u. d. N.}\quad f(x') \ge \text{Zielschwellenwert}$$
Das ist ein restringiertes Optimierungsproblem ([[restringierte-optimierung]]).

**Gute Kontrafaktuale sind** (→ VL6, F. 10; [[q-nb07-local-xai|NB7]]):

| Eigenschaft | Bedeutung |
|---|---|
| sparsam | nur wenige Merkmale ändern sich |
| plausibel | geänderte Werte liegen im beobachteten Datenbereich |
| handlungsfähig | keine unveränderlichen oder geschützten Attribute |
| legal | rechtlich zulässig |
| robust | kleine Störungen kehren das Ergebnis nicht sofort um |

- **Unveränderliche Merkmale** wie Alter oder geschützte Attribute dürfen **nicht** als Handlungen vorgeschlagen werden.
- Kontrafaktuale erklären **Modell-Entscheidungsgrenzen**, keine garantierten realen Interventionen.

### Algorithmischer Recourse (→ VL6, F. 11)
- Machbarkeit hängt von **Veränderbarkeit, Kosten, Zeit, Zugang und kausalen Nebenbedingungen** ab.
- Ein technisch gültiges Kontrafaktual kann **schlechter Recourse** sein, wenn es für manche Gruppen unbezahlbar oder unerreichbar ist.
- **Recourse-Fairness:** Sind Handlungskosten und Erfolgswahrscheinlichkeiten zwischen Gruppen vergleichbar?
- Empfehlungen vor der Kommunikation auf rechtliche und ethische Akzeptabilität prüfen.

## Annahmen & Voraussetzungen
- Festlegen, welche Merkmale veränderbar sind (`mutable_features` in NB7) und welche Distanz $d$ gilt.

## Beispiel ([[q-nb07-local-xai|NB7]])
Zufallssuche über veränderbare Merkmale des Churn-Modells (4 000 Kandidaten), Auswahl des Kandidaten mit kleinster Distanz, der die Schwelle überschreitet. In [[zwei-erklaerungen-pruefen]] ist der Satz „Wenn Sie mehr Kontakte am Hauptstandort aufgebaut hätten, wären Ihre Chancen sicher besser gewesen" ein kontrafaktisches **Überversprechen** („sicher") und eventuell schlechter Recourse für Teilzeitkräfte an Regionalstandorten.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – Kontrafaktuale an einem linearen Score
> Score $s = -4 + 0{,}002\cdot\text{Einkommen} + 0{,}5\cdot\text{Jahre im Job} - 1{,}5\cdot\text{Zahlungsverzüge}$, Zusage bei $s \ge 0$. Antrag: 1 500 €, 2 Jahre, 1 Verzug ⇒ $s = -4 + 3 + 1 - 1{,}5 = -1{,}5$ → Ablehnung. Fehlende 1,5 Punkte, jeweils nur ein Merkmal geändert:
>
> | Änderung | nötig | Bewertung |
> |---|---|---|
> | Einkommen | $+1{,}5/0{,}002 = +750$ € | sparsam, handlungsfähig, aber teuer |
> | Jahre im Job | $+1{,}5/0{,}5 = +3$ Jahre | handlungsfähig nur mit Zeit |
> | Verzüge | $1 \to 0$ | Vergangenheit, **nicht** handlungsfähig |
>
> Ein Kontrafaktual genau auf der Schwelle ($s = 0$) ist nicht robust – besser mit Puffer kommunizieren, z. B. +800 €.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Recourse-Fairness
> Dasselbe Kontrafaktual „+750 € Monatseinkommen" bedeutet für eine Teilzeitkraft mit 1 500 € eine Steigerung um 50 %, für jemanden mit 5 000 € nur 15 %. Technisch ist es für beide gleich gültig, die **Handlungskosten** sind aber sehr verschieden. Systematisch unterschiedliche Kosten zwischen Gruppen sind nach VL6, F. 11 ein Fairnessproblem, auch wenn das Modell selbst gleiche Fehlerraten hat.

## Typische Prüfungsfragen & Fallstricke
- Optimierungsproblem aufschreiben.
- Fünf Eigenschaften guter Kontrafaktuale.
- Unterschied Kontrafaktual vs. Recourse.
- Fallstrick: Kontrafaktual als kausale Garantie kommunizieren.
- Fallstrick: unveränderliche Merkmale (Alter, Herkunft) als Handlung vorschlagen.

## Verwandt
- [[lime]], [[shap]] – andere lokale Erklärungen
- [[restringierte-optimierung]] – Formulierung
- [[fairness-grundlagen]] – Recourse-Fairness
- [[erklaerbarkeit-grundlagen]]

## Karteikarten
Welche Frage beantwortet eine kontrafaktische Erklärung?::Welche kleine Änderung an $x$ würde die Modellentscheidung kippen?
Optimierungssicht von Kontrafaktualen?::Minimiere $d(x,x')$ unter der Nebenbedingung, dass $f(x')$ einen Zielschwellenwert überschreitet.
Fünf Eigenschaften guter Kontrafaktuale?::Sparsam, plausibel, handlungsfähig, legal, robust.
Was dürfen Kontrafaktuale nicht vorschlagen?::Änderungen unveränderlicher Merkmale wie Alter oder geschützter Attribute.
Was erklären Kontrafaktuale – und was nicht?::Modell-Entscheidungsgrenzen; keine garantierten realen Interventionen.
Was ist algorithmischer Recourse?::Die Frage, ob eine Person das Ergebnis unter institutionellen Regeln realistisch ändern kann (Veränderbarkeit, Kosten, Zeit, Zugang).
Was ist Recourse-Fairness?::Vergleichbare Handlungskosten und Erfolgswahrscheinlichkeiten zwischen Gruppen.

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 10–11, 13
- [[q-nb07-local-xai|NB7]]
