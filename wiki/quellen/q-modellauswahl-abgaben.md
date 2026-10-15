---
typ: quelle
themen: [modellwahl, uebung]
aliases: [Modellauswahl-Abgaben, Workshop Modellauswahl]
datei: "raw/uebungen/modellauswahl/"
original: "5_Modellauswahl/1_Problembeschreibung (docx), 5_Modellauswahl/2_Kurzpräsentation Modellauswahl (pptx)"
relevanz: mittel
aktualisiert: 2026-10-15
---

# Eigene Abgaben zum Workshop Modellauswahl

Zwei Abgaben aus dem Workshop ([[q-vl02-bias-varianz-modellwahl|VL2]], Folie 27): eine eigene Problembeschreibung (Schritt 1) und eine Kurzpräsentation zur Modellauswahl (Schritt 4).

## 1 · Problembeschreibung „Fake-Shop-Erkennung" (`problembeschreibung-fake-shop.docx`)
Beurteilen, ob ein Online-Shop seriös oder betrügerisch ist; Ziel ist eine Wahrscheinlichkeit/Bewertung, z. B. für Browser-Warnungen. Merkmale: technisch (Domain-Alter, SSL, WHOIS), inhaltlich (Textqualität, sprachliche Konsistenz, rechtliche Hinweise), Preismuster (extreme Rabatte, Zahlungsoptionen), Vertrauenssignale (Bewertungen, Zertifizierungen). Herausforderungen: Fake-Shops ahmen echte nach (Verteilungsverschiebung), Rauschen, fehlende Werte, **Klassenungleichgewicht**.

## 2 · Kurzpräsentation „Predictive Quality Control" (`kurzpraesentation-qualitaetskontrolle.pptx`, 4 Folien)
- **Problem:** Bauteile werden erst am Prozessende als OK/NOK bewertet → binäre Klassifikation während der Produktion auf Basis von Sensordaten (Temperatur, Abkühlung, Maschinendaten).
- **Herausforderungen:** Sensorverschleiß → Rauschen, fehlende Werte; große Datenmengen bei vielen Features; ein NOK-Teil als OK zu bewerten ist schlimmer als umgekehrt → **NOK höher gewichten**; NOK ist selten.
- **Modellwahl:** Fokus auf **Recall** der NOK-Klasse; **Gradient Boosted Trees** (tabellarisch, nichtlinear, robust bei großen verrauschten Datensätzen); **Baseline: logistische Regression** (schnell, interpretierbar).

## Einordnung
Beide Fälle sind im Wiki gegen den 4-Phasen-Ablauf gespiegelt: [[modellauswahl-fallbeispiele]].
