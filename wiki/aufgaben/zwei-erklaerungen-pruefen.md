---
typ: aufgabe
themen: [xai, fairness]
quellen: ["[[q-aktivitaet-zwei-erklaerungen]]", "[[q-vl05-xai-transparente-globale-modelle]]", "[[q-vl06-xai-lokal-fairness]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Aufgabe · Zwei Erklärungen kritisch prüfen (Aktivität 2)

Szenario und Arbeitsauftrag: [[q-aktivitaet-zwei-erklaerungen]]. Fiktive Senior-Analystin, 80 % Teilzeit nach Elternzeit, Regionalstandort, sehr gute Bewertungen, Score 68 bei Cut-off 72 → Warteliste.

> [!info] Lösungsvorschlag (eigene Ausarbeitung)
> Die Klassifikationskategorien und Kriterien stammen aus der VL ([[q-vl05-xai-transparente-globale-modelle|VL5]], F. 6, 8). Die konkrete Einordnung jedes Satzes ist eine eigene Lösung und keine Musterlösung des Dozenten.

## 1 · Aussagen klassifizieren
Kategorien: **beobachtbar** (Fakt aus den Daten), **modellbasiert** (Output/Attribution des Modells), **normativ** (Wertung, Regel), **spekulativ** (unbelegte Vermutung), **unzulässig** (diskriminierend oder kausal falsch).

### Erklärung 1 – intern technisch
| Aussage (gekürzt) | Kategorie | Kommentar |
|---|---|---|
| Score 68, Cut-off 72 → Warteliste | modellbasiert + normativ | Score ist Modelloutput, Cut-off eine Organisationsregel; Unsicherheit um den Cut-off fehlt |
| drei Bewertungen, zwei Projektbeiträge, Mentoring dokumentiert | beobachtbar | ok |
| stärkste Beiträge: fehlende formale Führung, weniger Netzwerk, kürzere Vollzeit-Verfügbarkeit | modellbasiert | Attribution, kein kausaler Effekt; Skala/Methode nicht genannt |
| „Führungserfolg entsteht vor allem durch Vollzeitpräsenz, Teilzeit ist belastbarer Hinweis auf geringere Eignung" | **unzulässig** | kausale Überbehauptung; Teilzeit ist Proxy für Elternschaft/Geschlecht |
| „würde ein intensives Programm wahrscheinlich weniger gut durchhalten" | **spekulativ** | nicht durch Daten gestützt, stigmatisierend |
| Plätze bevorzugt an kurzfristig Einsetzbare | normativ | legitime Strategiefrage, muss aber offen als Regel benannt werden |
| „Deshalb ist die Warteliste fair" | normativ, unbegründet | Fairness wird behauptet, nicht geprüft |
| Teilzeit/Standort/Elternzeit-Einfluss auf Netzwerkdaten noch nicht geprüft | beobachtbar (Grenze) | **guter Satz** – macht eine Grenze explizit |

### Erklärung 2 – extern für die Betroffene
| Aussage (gekürzt) | Kategorie | Kommentar |
|---|---|---|
| Warteliste statt direkter Aufnahme | beobachtbar | ok |
| Grundlage: Projekte, Feedback, Führungserfahrung, Zusammenarbeit | beobachtbar | ok, aber ohne Gewichtung |
| keine formale Teamleitung, weniger standortübergreifende Zusammenarbeit berücksichtigt | modellbasiert | verständlich formuliert |
| „bedeutet nicht, dass Sie ungeeignet sind" | normativ | gut |
| Komitee priorisiert eindeutig belegte Einsetzbarkeit | normativ | Regel offen benannt – gut |
| „mehr Kontakte am Hauptstandort → Chancen **sicher** besser" | **spekulativ** / kausale Überbehauptung | Kontrafaktual als Garantie; schlechter Recourse für Regionalstandort/Teilzeit ([[kontrafaktische-erklaerungen]]) |
| „Mitarbeitende aus regionalen Standorten passen erfahrungsgemäß weniger gut" | **unzulässig** | Gruppenstereotyp, Proxy-Diskriminierung |
| Empfehlung: in sechs Monaten standortübergreifendes Projekt leiten, Feedback einholen | normativ (Recourse) | Machbarkeit bei 80 % Teilzeit prüfen |
| Revision möglich, falls Informationen fehlen | beobachtbar/prozedural | **gut** – Anfechtbarkeit (gegen prozeduralen Schaden) |

**Ampelurteil:** Erklärung 1 **ablehnen** (unzulässige und spekulative Kernsätze). Erklärung 2 **überarbeiten** (zwei Sätze streichen bzw. umformulieren, Rest tragfähig).

## 2 · Prüffragen aus drei Perspektiven
- **Data Science:** Wie stabil ist der Score 68 nahe dem Cut-off 72 (Bootstrap-Intervall, Seeds)? Ändert sich die Empfehlung, wenn Netzwerk- und Verfügbarkeitsmerkmale auf Arbeitszeit normiert werden?
- **Betroffene Person:** Welche Daten wurden konkret genutzt, und wie kann ich fehlende Projektleistungen nachreichen? Wurde meine Teilzeit negativ gewertet?
- **Unabhängige Prüfung:** Unterscheiden sich Auswahlraten und TPR nach Teilzeit, Geschlecht und Standort (inkl. Schnittmengen)? Ist der Cut-off dokumentiert und begründet? Sagen die Merkmale tatsächlich späteren Führungserfolg vorher (Label-Validität)?

## 3 · Schwächsten Satz umformulieren
Original: „Da Führungserfolg in unserem Unternehmen vor allem durch Vollzeitpräsenz entsteht, ist Teilzeit ein belastbarer Hinweis auf geringere Führungseignung."

Vorschlag: „Das Modell gewichtet die dokumentierte Verfügbarkeit für Vollzeitprojekte negativ für die Einstufung. Ob dieses Merkmal Führungseignung misst oder Mitarbeitende in Teilzeit bzw. nach Elternzeit systematisch benachteiligt, ist nicht belegt und wird vor der Freigabe geprüft."

## 4 · Zwei zusätzliche Evidenzstücke
1. **Fairness-Audit** der Auswahlregel: Auswahlraten und Equal Opportunity nach Teilzeit, Geschlecht und Standort mit Unsicherheitsintervallen ([[fairness-metriken]], [[fairness-mitigation]]).
2. **Validitäts- und Sensitivitätsprüfung:** Zusammenhang der Merkmale mit späterem Führungserfolg sowie Kontrafaktual-Check, ob die Empfehlung kippt, wenn Netzwerkdaten arbeitszeitbereinigt werden.

## Benötigte Konzepte
- [[erklaerbarkeit-grundlagen]]
- [[fairness-grundlagen]]
- [[kontrafaktische-erklaerungen]]

## Quellen
- [[q-aktivitaet-zwei-erklaerungen]]
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folien 6–8
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 10–11, 17–18
