---
typ: methode
themen: [xai, lokal, neuronale-netze]
aliases: [Grad-CAM, Gradient-weighted Class Activation Mapping, Saliency Map, Heatmap]
quellen: ["[[q-vl06-xai-lokal-fairness]]"]
relevanz: niedrig
beherrschung: neu
aktualisiert: 2026-10-15
---

# Grad-CAM

#flashcards/statistik

## Kurzdefinition
Klassenspezifische visuelle Erklärung für **faltende neuronale Netze (CNNs)**: Gradienten des Klassenscores, die in die **letzte Faltungsschicht** fließen, gewichten deren Feature-Maps; das Ergebnis ist eine grobe **Lokalisierungs-Heatmap**, die hochskaliert über das Eingabebild gelegt wird (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 12).

## Intuition
„Wohin hat das Bildmodell für diese Klasse geschaut?" Feature-Maps der letzten Faltungsschicht sind räumlich noch zugeordnet; die Gradienten sagen, welche Maps den Klassenscore treiben.

## Formal
> [!info] Ergänzung (nicht aus der VL) – Formeln
> Gewichte $\alpha_k^c = \frac{1}{Z}\sum_{i,j}\frac{\partial y^c}{\partial A^k_{ij}}$ (global gemittelte Gradienten des Scores $y^c$ nach Feature-Map $A^k$); Karte $L^c = \mathrm{ReLU}\big(\sum_k\alpha_k^cA^k\big)$, danach auf Bildgröße hochskalieren.

## Grenzen (→ VL6, F. 12)
Grad-CAM erklärt den **Ort der Evidenz** für einen Klassenscore; es **beweist keine Kausalität oder Korrektheit**. Eine Heatmap auf dem „richtigen" Objekt heißt nicht, dass das Modell aus den richtigen Gründen entscheidet – sie kann z. B. auch auf ein Wasserzeichen zeigen (Clever-Hans-Effekt, siehe [[erklaerbarkeit-grundlagen]]).

## Typische Prüfungsfragen & Fallstricke
- Für welche Modelle ist Grad-CAM gedacht? → CNNs (modellspezifisch).
- Welche Schicht? → letzte Faltungsschicht.
- Fallstrick: Heatmap als Beweis für korrektes Verhalten.

## Verwandt
- [[erklaerbarkeit-grundlagen]] – Taxonomie (lokal, post-hoc, spezifisch)
- [[lime]], [[shap]] – modellagnostische Alternativen

## Karteikarten
Wofür ist Grad-CAM?::Klassenspezifische visuelle lokale Erklärung für CNNs.
Welche Information nutzt Grad-CAM?::Gradienten des Klassenscores, die in die letzte Faltungsschicht fließen, um deren Feature-Maps zu gewichten.
Was beweist eine Grad-CAM-Heatmap nicht?::Weder Kausalität noch Korrektheit – nur den Ort der Evidenz für den Klassenscore.

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folie 12
