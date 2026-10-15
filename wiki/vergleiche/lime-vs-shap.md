---
typ: vergleich
themen: [xai, lokal]
aliases: [LIME vs SHAP, Lokale Erklärungen im Vergleich]
quellen: ["[[q-vl06-xai-lokal-fairness]]", "[[q-nb07-local-xai]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Vergleich: LIME vs. SHAP (vs. Kontrafaktuale)

#flashcards/statistik

Leitfrage der VL: „Wie unterscheiden sich LIME und SHAP in Annahmen und Ausgaben?" (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 1)

| | [[lime\|LIME]] | [[shap\|SHAP]] | [[kontrafaktische-erklaerungen\|Kontrafaktuale]] |
|---|---|---|---|
| Frage | Wie verhält sich das Modell **in der Nähe** von $x_0$? | Wie viel trägt jedes Merkmal zur **Differenz zur Baseline** bei? | Welche **kleinste Änderung** kippt die Entscheidung? |
| Prinzip | lokales gewichtetes Surrogat $g$ | Spieltheorie, Mittel über Koalitionen | Optimierung $\min d(x,x')$ u. d. N. $f(x')\ge$ Schwelle |
| Ausgabe | Koeffizienten von $g$ | additive Beiträge $\phi_i$, $f(x) = \text{baseline} + \sum\phi_i$ | ein alternativer Punkt $x'$ |
| Wichtigste Annahmen | Nachbarschaft, Kernbreite, Störungsstrategie, Komplexität von $g$ | Baseline/Hintergrund, Behandlung fehlender Merkmale (interventional/conditional), Skala | Distanz, veränderbare Merkmale, Plausibilität |
| Qualitätsprüfung | lokales R², Stabilität über Seeds | Effizienz (Summe = Differenz), Stabilität über Hintergrunddaten | Sparsamkeit, Plausibilität, Robustheit, Recourse-Fairness |
| Aggregierbar global? | eingeschränkt | ja (mean \|SHAP\|, Summary-Plot) | nein |

**Gemeinsam** (→ VL6, F. 13): Alle drei erklären die **Modellvorhersage**, nicht die reale Kausalität. Lokale Erklärungen brauchen Stabilitätsprüfungen und müssen mit globalen und Subgruppenanalysen verbunden werden.

**Fallauswahl** für lokale Erklärungen ([[q-nb07-local-xai|NB7]]): typisch korrekte Vorhersage, Grenzfall, Fehlklassifikation – drei Fälle, an denen sich zeigt, wann die Methoden übereinstimmen und wann nicht.

## Karteikarten
Kernunterschied LIME vs. SHAP?::LIME fittet ein lokales gewichtetes Surrogat; SHAP verteilt die Differenz zur Baseline spieltheoretisch über alle Koalitionen additiv auf die Merkmale.
Welche drei Fälle wählt man für lokale Erklärungen aus?::Typisch korrekte Vorhersage, Grenzfall, Fehlklassifikation.
Was erklären LIME, SHAP und Kontrafaktuale gemeinsam nicht?::Reale Kausalität – sie erklären nur die Modellvorhersage.

## Verwandt
- [[lime]]
- [[shap]]
- [[kontrafaktische-erklaerungen]]
- [[erklaerbarkeit-grundlagen]]
