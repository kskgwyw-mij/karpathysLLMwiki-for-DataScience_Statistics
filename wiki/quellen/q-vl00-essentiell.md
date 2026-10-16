---
typ: quelle
themen: [klausur, glm, bias-varianz, regularisierung, modellwahl, resampling, optimierung]
aliases: [VL0, Folienauswahl essentiell, Gekürzte Version]
datei: "raw/vorlesung/vl00-essentiell.pdf"
original: "7_Folienauswahl/1_Statistisches_Lernen_2_bbm_essentiell.pdf"
relevanz: hoch
aktualisiert: 2026-10-16
---

# VL0 · Folienauswahl „essentiell" (94 Folien)

> [!tip] Wichtigste Quelle für die Klausur
> Der Dozent hat hier „die essentiellen Konzepte zusammengefasst". Wörtlich sinngemäß (Folie 2): In der Prüfung können Teilaufgaben kommen, die zusätzliches Wissen aus der Vorlesung brauchen – eine **gute Note ist aber möglich, wenn diese Folien gut verstanden sind**.

Folie = PDF-Seite.

## Zusammenfassung
Gekürzte Fassung der Kapitel 1–4 von Statistical Learning 2: allgemeine lineare Modelle mit Basisfunktionen, Bias-Varianz-Zerlegung, Vorgehen bei der Modellauswahl, Regularisierung (Taxonomie, L2/L0/L1, Output-Regularisierung, Early Stopping, Dropout, Label Smoothing, Hyperparameterwahl), Kreuzvalidierung, Bootstrap und Bagging sowie Optimierung (Konvexität, Gradientenverfahren, Momentum, Nesterov, SGD, adaptive Verfahren, Newton/Quasi-Newton).

**Nicht enthalten:** B-Spline-Konstruktion (Cox-de-Boor), probabilistische Sicht/MAP, restringierte Optimierung, Initialisierung, AdaGrad/AdaDelta/Adam im Detail, LION und das gesamte Kapitel XAI & Fairness.

## Gliederung
| Folien | Inhalt | Wiki-Seiten |
|---|---|---|
| 3–11 | Lernproblem, lineare Regression, Designmatrix, Normalgleichung, Basisfunktionen | [[statistisches-lernproblem]], [[lineare-regression]], [[basisfunktionen-regression]] |
| 12–16 | Ziel Datenfit vs. Generalisierung, Fehlerzerlegung, Methoden zur Balance | [[bias-varianz-tradeoff]] |
| 17–34 | Wie wähle ich die passende ML-Methode? Datenmenge, p vs. N, Datentyp, Phasen 1–4 | [[modellauswahl-vorgehen]] |
| 35–43 | Regularisierung: Definition, Taxonomie | [[regularisierung]] |
| 44–52 | Beispiele: L2, L0, L1/Lasso, Output-Reg., Early Stopping, Dropout, Label Smoothing | [[ridge-regression]], [[lasso-regression]], [[output-regularisierung]], [[early-stopping]], [[dropout]], [[label-smoothing]] |
| 53–57 | Entscheidungsschema, Hyperparameterwahl, Diagnose Unter-/Über-Regularisierung | [[regularisierung]], [[train-val-test-split]] |
| 58–61 | Cross-Validation und Varianten | [[kreuzvalidierung]] |
| 62–68 | Bootstrap, Konfidenzintervalle, OOB-Evaluation, Bagging (nur Grafik) | [[bootstrap]], [[bagging]] |
| 69–76 | Lernen als Optimierung, Minima, Max→Min, Konvexität | [[optimierung-grundlagen]] |
| 77–84 | Gradientenabstieg, Schrittweite, Momentum, Nesterov | [[gradientenabstieg]], [[momentum-und-nesterov]] |
| 85–90 | SGD, adaptive Optimierer (Vergleichstabelle) | [[sgd]], [[adaptive-optimierer]] |
| 91–94 | Newton, Quasi-Newton, Vergleich 1. vs. 2. Ordnung | [[newton-und-quasi-newton]], [[optimierer-uebersicht]] |

## Prüfungsrelevante Aussagen
- Die [[q-lernhilfe-sl2|Lernhilfe eines Studierenden]] folgt diesen Folien Abschnitt für Abschnitt; ihre „Exam focus"-Hinweise (laut Jan klausurnah) stehen dort unter „Klausurschwerpunkte".
- Alles auf diesen Folien → `relevanz: hoch` im Wiki.
- Fehlerzerlegung: Gesamtfehler = Bias² + Varianz + irreduzibler Fehler; gegen den irreduziblen Fehler kann man nichts tun (Folie 14).
- Normalgleichung $\hat a = (X^TX)^{-1}X^Ty$ bzw. mit Basismatrix $B$ (Folien 10–11).
- Faustregeln zur Modellwahl nach Datenmenge (N < 1000, 1000–100 000, > 100 000) und Verhältnis p/N (Folien 22–25).
- L2 → $\lambda = 0$ keine Regularisierung, $\lambda\to\infty$ ergibt $\theta = 0$ (Folie 45); L0 ist keine Norm, nicht differenzierbar (Folie 46); L1 als Konvexifizierung (Folie 47).
- Early Stopping ist für ein simples lineares Modell äquivalent zu L2-Regularisierung (Folie 50).
- Konvex ⇒ jedes lokale Minimum ist global; Deep-Learning-Ziele sind meist nicht konvex (Folie 76).

## Widersprüche / Abweichungen zu anderen Quellen
- Folie 52 (Label Smoothing) enthält im unteren Teil versehentlich den Text der Dropout-Folie („Das Weglassen der Bestandteile …"). Inhaltlich gehört er zu [[dropout]], nicht zu [[label-smoothing]].
- Folie 81 schreibt das Momentum-Update ohne Lernraten-Index; Folie 82 und 83 sind konsistent. Siehe [[momentum-und-nesterov]].
- Folie 91: Newton-Update als $\theta_{t+1} = -H(\theta_t)^{-1}\nabla f(\theta_t)$ – es fehlt der Summand $\theta_t$. Korrekt: $\theta_{t+1} = \theta_t - H(\theta_t)^{-1}\nabla f(\theta_t)$ (vgl. [[q-vl04-map-resampling-optimierung|VL4]], Folie 57). Siehe [[newton-und-quasi-newton]].

## Offene Fragen
- Kommen XAI & Fairness (VL5–VL6) in der Klausur vor? Sie fehlen in dieser Auswahl vollständig.
