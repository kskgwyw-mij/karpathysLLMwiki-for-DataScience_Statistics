---
typ: konzept
themen: [fairness]
aliases: [Fairness, Algorithmischer Schaden, Geschützte Attribute, Proxy-Variablen, Proxy-Diskriminierung, Fairness through unawareness]
quellen: ["[[q-vl06-xai-lokal-fairness]]", "[[q-vl05-xai-transparente-globale-modelle]]", "[[q-nb05-tree-exam-grade]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Fairness: Grundlagen, Schadensarten, Proxys

#flashcards/statistik

## Kurzdefinition
Fairness-Analyse fragt, ob **Fehler, Vorteile und Belastungen** eines Modells akzeptabel über Gruppen verteilt sind (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 2). Sie ist eine **Design- und Governance-Frage**, nicht nur eine Post-hoc-Metrik (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 16).

## Intuition – warum Fairness im statistischen Lernen? (→ VL6, F. 16)
- Modelle lernen aus historischen Daten, und diese spiegeln oft **ungleiche Institutionen und Messprozesse** wider.
- Fehler sind nicht neutral, wenn sie Gruppen **systematisch unterschiedlich** treffen.
- Technische Analyse muss an rechtliche, ethische und domänenspezifische Anforderungen anschließen.
- **Der relevante Schaden bestimmt, welche Metrik sinnvoll ist** → [[fairness-metriken]].

**Leitfragen** (→ VL6, F. 15): Welche Arten von Schaden können ML-Systeme erzeugen? Welche Gruppen-Fairness-Metrik passt zu welchem Entscheidungsproblem? Warum können Fairness-Metriken selbst bei einem kalibrierten Modell kollidieren?

## Arten algorithmischen Schadens (→ VL6, F. 17)
| Schadensart | Beschreibung | Beispiel (eigene Ergänzung) |
|---|---|---|
| **Allokation** | Ressourcen, Chancen oder Dienstleistungen ungleich verteilt | Kredit, Stelle, Förderprogramm |
| **Quality of Service** | System funktioniert für eine Gruppe schlechter | Spracherkennung für Dialekte |
| **Repräsentation** | Gruppen stereotypisiert, ausgelöscht oder falsch dargestellt | Bildsuche, Textgenerierung |
| **Prozedural** | Betroffene können Entscheidungen nicht verstehen, anfechten oder korrigieren | Blackbox-Ablehnung ohne Begründung |
| **Feedback** | Entscheidungen verändern zukünftige Daten und verstärken Ungleichheit | Wer nie einen Kredit bekommt, erzeugt keine Rückzahlungsdaten |

## Geschützte Attribute und Proxys (→ VL6, F. 18)
- Geschützte Attribute hängen vom **rechtlichen und institutionellen Kontext** ab, z. B. Geschlecht, Alter, Behinderung, Ethnizität, Religion, Migrationshintergrund.
- **Das Entfernen eines geschützten Attributs garantiert keine Fairness**, weil Proxy-Variablen verbleiben können.
- Häufige Proxys: Postleitzahl, Sprache, Schule, Berufshistorie, Gerätetyp, Verhaltensspuren.
- **„Fairness durch Nichtbeachtung"** scheitert meist, wenn korrelierte Variablen die Gruppenzugehörigkeit rekonstruieren.
- Audits müssen klären, welche Attribute für die Fairness-Evaluation gemessen werden **dürfen oder müssen** – ohne das Attribut kann man Gruppenmetriken gar nicht berechnen.

## Beispiele aus den Unterlagen
- [[q-nb05-tree-exam-grade|NB5]]: Der Baum splittet früh nach `cohort` (BB/VZ); `work_hours_week`, `commute_minutes` und die Selbstauskunft `exam_anxiety_self_report` sind mögliche Proxy- bzw. sensible Merkmale.
- [[q-aktivitaet-zwei-erklaerungen]]: Teilzeit, Elternzeit und Regionalstandort beeinflussen „Netzwerkdaten" und „Verfügbarkeit" – Proxys für Geschlecht/Elternschaft. Siehe [[zwei-erklaerungen-pruefen]].

## Typische Prüfungsfragen & Fallstricke
- Fünf Schadensarten nennen und je ein Beispiel geben.
- „Warum reicht es nicht, das Geschlecht aus den Features zu entfernen?" → Proxys rekonstruieren es.
- Fallstrick: Fairness als rein technische Metrik behandeln – die Metrikwahl ist eine **normative** Entscheidung.

## Verwandt
- [[fairness-metriken]] – Gruppenmetriken und ihre Kollision
- [[fairness-mitigation]] – Audits und Gegenmaßnahmen
- [[transparente-modelle]] – versteckte Proxys in Scorecards
- [[kontrafaktische-erklaerungen]] – Recourse-Fairness
- [[modellauswahl-vorgehen]] – Fairness als Constraint in Phase 1

## Karteikarten
Fünf Arten algorithmischen Schadens?::Allokation, Quality of Service, Repräsentation, prozeduraler Schaden, Feedback-Schaden.
Was ist ein Allokationsschaden?::Ressourcen, Chancen oder Dienstleistungen werden ungleich verteilt.
Was ist ein Feedback-Schaden?::Entscheidungen verändern zukünftige Daten und können Ungleichheit verstärken.
Warum garantiert das Entfernen geschützter Attribute keine Fairness?::Proxy-Variablen (z. B. Postleitzahl, Sprache, Schule) können die Gruppenzugehörigkeit rekonstruieren.
Nenne vier häufige Proxys.::Postleitzahl, Sprache, Schule, Berufshistorie, Gerätetyp, Verhaltensspuren.
Warum ist Fairness „eine Design- und Governance-Frage"?::Weil sie Problemformulierung, Labels und Deployment betrifft und eine normative Entscheidung über den relevanten Schaden verlangt.

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 15–18
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folien 2, 15
- [[q-nb05-tree-exam-grade|NB5]]
