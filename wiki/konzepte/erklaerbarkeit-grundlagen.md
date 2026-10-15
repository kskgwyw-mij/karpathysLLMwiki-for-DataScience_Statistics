---
typ: konzept
themen: [xai, interpretierbarkeit]
aliases: [Explainable AI, XAI, Erklärbare KI, Interpretierbarkeit, Erklärbarkeit, Globale Erklärung, Lokale Erklärung, Kluger Hans, Clever Hans]
quellen: ["[[q-vl05-xai-transparente-globale-modelle]]", "[[q-vl06-xai-lokal-fairness]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Erklärbarkeit: Grundbegriffe, Taxonomie, Qualität

#flashcards/statistik

## Kurzdefinition
**Erklärbarkeit** fragt: „Warum hat sich das Modell so verhalten?" **Fairness** fragt: „Wer ist betroffen, und ist der Trade-off akzeptabel?" (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 2)

## Intuition – warum das Thema? (→ VL5, F. 2)
- Moderne Modelle sagen präzise vorher, bleiben aber schwer zu untersuchen.
- Erklärbarkeit hilft, Modelle zu **debuggen**, Unsicherheit zu kommunizieren und irreführende Muster zu hinterfragen.
- Fairness-Analyse fragt, ob Fehler, Vorteile und Belastungen akzeptabel über Gruppen verteilt sind.
- Verantwortlicher Einsatz braucht technische Evidenz **und** explizite Annahmen und Grenzen.

**Leitfragen** (→ VL5, F. 3): Was genau wird erklärt – Daten, Modell, Vorhersage oder Entscheidungsprozess? Für wen – Analyst:in, Fachexpert:in, betroffene Person, Prüfer:in? Wie kann eine plausible Erklärung trotzdem technisch oder ethisch irreführend sein?

## Zentrale Begriffe (→ VL5, F. 4)
| Begriff | Bedeutung | Beispiel |
|---|---|---|
| **Interpretierbarkeit** | Die Modellstruktur selbst ist verständlich | dünn besetztes lineares Modell, Scorecard, flacher Baum |
| **Erklärbarkeit** | Eine **zusätzliche Methode** erzeugt eine Erklärung für ein Modell oder eine Vorhersage | LIME, SHAP, PDP |
| **Globale** Erklärung | durchschnittliches/aggregiertes Verhalten über viele Beobachtungen | Permutation Importance, PDP |
| **Lokale** Erklärung | eine Vorhersage oder eine kleine Menge ähnlicher Fälle | LIME, SHAP-Waterfall, Kontrafaktuale |

Ein klares Erklärungsziel verhindert, dass Modellverhalten, reale Kausalität und organisationale Entscheidungsregeln verwechselt werden.

## Taxonomie der Erklärungsmethoden (→ VL5, F. 5)
| Dimension | Optionen | Typische Frage |
|---|---|---|
| Umfang | global / lokal | gesamte Modelllogik oder eine Entscheidung? |
| Zeitpunkt | ante-hoc / post-hoc | von Anfang an transparent oder nach dem Training erklärt? |
| Modellbezug | spezifisch / agnostisch | erfordert es eine bestimmte Modellfamilie? |
| Ausgabeform | Wichtigkeit, Effekte, Regeln, Beispiele, Kontrafaktuale | welches Artefakt nützt der Zielgruppe? |
| Qualitätskriterium | Fidelity, Stabilität, Handlungsfähigkeit, Fairness | wann ist die Erklärung gut genug? |

**Einordnung der Methoden im Wiki:**

| Methode | Umfang | Zeitpunkt | Modellbezug |
|---|---|---|---|
| [[transparente-modelle]], [[logistische-regression]], [[entscheidungsbaeume]] | global + lokal | ante-hoc | spezifisch |
| [[permutation-feature-importance]] | global | post-hoc | agnostisch |
| [[pdp-ice-ale]] | global (ICE: individuell) | post-hoc | agnostisch |
| [[lime]] | lokal | post-hoc | agnostisch |
| [[shap]] (TreeSHAP) | lokal, aggregierbar | post-hoc | agnostisch (TreeSHAP: Bäume) |
| [[kontrafaktische-erklaerungen]] | lokal | post-hoc | agnostisch |
| [[grad-cam]] | lokal | post-hoc | spezifisch (CNN) |

## Qualität und häufige Fallen (→ VL5, F. 6)
- Eine gute Erklärung ist für ihre **Zielgruppe verständlich**, dem **Modell treu** (fidelity), unter plausiblen Störungen **stabil** und macht **Grenzen explizit**.
- **Feature-Attribution ist kein kausaler Effekt:** „wichtig für das Modell" heißt nicht „wirksame Intervention".
- Lokale Erklärungen nicht ohne Evidenz auf das ganze Modell verallgemeinern.
- Visuelle Erklärungen brauchen **Verteilungsprüfungen**: nicht unterstützte Bereiche und Ausreißer können Diagramme irreführend machen.
- Eine plausible Erklärung kann Proxy-Diskriminierung, Label-Bias oder ungleiche Fehlerraten verbergen.

## Globale Erklärungen: Können und Grenzen (→ VL5, F. 20)
Fassen Verhalten über einen Datensatz zusammen, erklären nicht jede Einzelentscheidung. Ausgaben: Feature-Importance-Rankings, Effektkurven, Verteilungen lokaler Effekte. Nützlich für Debugging, Modellvergleich, Stakeholder-Reporting, Hypothesengenerierung. **Nicht kausal**, außer gestützt durch Studiendesign, Annahmen oder externe Evidenz. Immer Datenausschnitt, Metrik, Preprocessing und Modellversion berichten.

## Lokale Erklärungen (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 2)
Fokus auf eine Vorhersage oder wenige Fälle, oft nahe der Entscheidungsgrenze oder bei Fehlern. Unterstützen Fallprüfung, Kommunikation, Debugging, Recourse. Hängen ab von Preprocessing, Baseline, Nachbarschaftsdefinition und Annahmen zur Merkmalsabhängigkeit; sollen mit globalem Verhalten und Subgruppen-Fairness abgeglichen werden; bei hohen Auswirkungen Stabilitätsprüfungen berichten und sagen, was die Erklärung **nicht** beweist.

> [!ergaenzung] Ergänzung (nicht aus der VL) – Bild auf VL5, Folie 19
> Die Folie zeigt nur ein historisches Foto eines Pferdes vor Publikum. Sehr wahrscheinlich ist der **Kluge Hans** gemeint: Das Pferd „rechnete" scheinbar, reagierte aber auf unbewusste Signale des Publikums. In ML steht der **Clever-Hans-Effekt** für Modelle, die richtige Vorhersagen aus Scheinmerkmalen treffen (z. B. Wasserzeichen im Bild) – genau das sollen globale Erklärungen aufdecken.

## Typische Prüfungsfragen & Fallstricke
- Interpretierbarkeit vs. Erklärbarkeit, global vs. lokal definieren.
- Die fünf Taxonomie-Dimensionen nennen und eine Methode einordnen.
- Vier Qualitätskriterien guter Erklärungen.
- Fallstrick: Attribution als Kausalität formulieren („weil die Person in Teilzeit arbeitet, ist sie weniger geeignet") → siehe [[zwei-erklaerungen-pruefen]].

## Verwandt
- [[transparente-modelle]] – ante-hoc
- [[permutation-feature-importance]], [[pdp-ice-ale]] – global post-hoc
- [[lime]], [[shap]], [[kontrafaktische-erklaerungen]] – lokal post-hoc
- [[fairness-grundlagen]] – zweite Hälfte des Kapitels
- [[statistisches-lernproblem]] – Inferenz braucht Interpretierbarkeit

## Karteikarten
Welche Frage stellt Erklärbarkeit, welche Fairness?::Erklärbarkeit: „Warum hat sich das Modell so verhalten?" Fairness: „Wer ist betroffen, und ist der Trade-off akzeptabel?"
Unterschied Interpretierbarkeit vs. Erklärbarkeit?::Interpretierbarkeit: Modellstruktur selbst verständlich. Erklärbarkeit: zusätzliche Methode erzeugt eine Erklärung.
Unterschied globale vs. lokale Erklärung?::Global: aggregiertes Verhalten über viele Beobachtungen. Lokal: eine Vorhersage oder wenige ähnliche Fälle.
Fünf Dimensionen der Taxonomie von Erklärungsmethoden?::Umfang, Zeitpunkt (ante-/post-hoc), Modellbezug (spezifisch/agnostisch), Ausgabeform, Qualitätskriterium.
Vier Eigenschaften einer guten Erklärung?::Für die Zielgruppe verständlich, dem Modell treu, unter plausiblen Störungen stabil, Grenzen explizit.
Ist Feature-Attribution ein kausaler Effekt?::Nein – „wichtig für das Modell" heißt nicht „wirksame Intervention".
Was bedeutet ante-hoc vs. post-hoc?::Ante-hoc: Modell von Anfang an transparent. Post-hoc: Erklärung nach dem Training erzeugt.

## Quellen
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folien 2–6, 18–20
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 1–2
