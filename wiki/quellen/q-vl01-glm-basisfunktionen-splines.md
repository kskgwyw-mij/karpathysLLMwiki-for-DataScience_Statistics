---
typ: quelle
themen: [organisation, glm, basisfunktionen, splines]
aliases: [VL1]
datei: "raw/vorlesung/vl01-glm-basisfunktionen-splines.pdf"
original: "3_Slides/1_Statistisches_Lernen_2_L1.pdf"
relevanz: hoch
aktualisiert: 2026-10-15
---

# VL1 · Organisation, General Linear Models, Basisfunktionen, Splines (44 Folien)

Folie = PDF-Seite.

## Zusammenfassung
Organisatorischer Einstieg (Dozent, Prüfungsmodus, Literatur, Terminplan, Kapitelübersicht) und Kapitel 1 „General Linear Models": das allgemeine Lernproblem $Y = f(x) + \varepsilon$, lineare Regression in Matrixform mit vollständiger Herleitung der Normalgleichung, Erweiterung auf beliebige Basisfunktionen (Polynom, multivariates Polynom, Gauß-RBF, Sigmoid, Fourier, stückweise konstant) und Splines mit rekursiver B-Spline-Konstruktion nach Cox-de Boor. Zwei Übungen: Basisfunktions-Regression im Notebook und B-Splines vom Grad 2 von Hand.

## Gliederung
| Folien | Inhalt | Wiki-Seiten |
|---|---|---|
| 2–9 | Dozent, Forschung, Masterarbeiten | – |
| 10 | **Leistungsbeurteilung** | [[overview]] |
| 11 | Literatur | [[q-esl-elements-of-statistical-learning]] |
| 12–13 | Terminplan und Kapitel 1–6 | [[overview]] |
| 15–17 | Lernproblem, Vorhersage vs. Inferenz | [[statistisches-lernproblem]] |
| 18–23 | Lineare Regression, Designmatrix, Ableitung, Normalgleichung | [[lineare-regression]] |
| 24–29 | Basisfunktionen und Basismatrix B | [[basisfunktionen-regression]] |
| 30 | Übung Basisfunktionen | [[basisfunktionen-uebung-folie-30]] |
| 31–40 | Splines, kubische Spline-Regression, B-Splines, Cox-de Boor, Eigenschaften | [[splines-und-b-splines]] |
| 41 | Übung B-Splines Grad 2 | [[b-splines-grad-2-von-hand]] |
| 42–43 | Matrixform der Spline-Regression | [[splines-und-b-splines]] |

## Prüfungsrelevante Aussagen
- **Prüfungsmodus (Folie 10):** Abschlussprüfung zählt 75 %, positive Prüfung (> 60 %) ist Voraussetzung; Papier-und-Stift-Prüfung **ohne Hilfsmittel**. Die restlichen 25 % (Mitarbeit, Aufgaben) können **nur** die Prüfungsnote verbessern.
- Gradient der Fehlerfunktion: $\partial L/\partial a = -2X^Ty + 2X^TXa$ (Folie 22) → Normalgleichung (Folie 23).
- Hochgestellte Indizes bei $x_j^i$ sind **Komponenten**, keine Potenzen (Folie 19).
- B-Spline-Eigenschaften: kompakter Träger, Zerlegung der Eins, Nichtnegativität, Stetigkeit (Folie 40).

## Widersprüche / Abweichungen zu anderen Quellen
- Gauß-RBF auf Folie 27 mit Nenner $(2\sigma)^2$; üblich ist $2\sigma^2$. Siehe Notation-Callout in [[basisfunktionen-regression]].
- Folie 12 nennt als Klausurtermin Fr, 03.07., 12:00–14:00. Aktueller Termin ist laut Jan (15.10.) der **13.11.2026, 11:00**; der 03.07. ist überholt. Ob die Dauer von 2 Stunden gleich bleibt, ist offen → [[overview]].
- Indexierung: Folie 21 verwendet $K$ Beobachtungen und $n$ Merkmale, Folie 42 dagegen $n$ Beobachtungen und $p$ Basisfunktionen.

## Offene Fragen
- Dauer der Klausur am 13.11. (2 Stunden wie am 03.07.?)
