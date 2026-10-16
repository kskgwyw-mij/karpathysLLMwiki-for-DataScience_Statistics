---
typ: quelle
themen: [klausur, glm, bias-varianz, regularisierung, modellwahl, resampling, optimierung]
aliases: [Lernhilfe, Study Guide, Lernhilfe SL2, Exam focus]
datei: "raw/literatur/lernhilfe-sl2.pdf"
original: "study_guide.pdf (Upload von Jan, 16.10.)"
relevanz: hoch
aktualisiert: 2026-10-16
---

# Lernhilfe eines Studierenden · „Deep-Understanding Study Guide" (15 Seiten)

> [!tip] Wofür diese Quelle zählt
> Laut Jan (16.10.) hat die Lernhilfe Absolventen des Fachs sehr geholfen, und ihre „Exam focus"-Kästen weisen auf **echte Klausuren** hin. Inhaltlich bringt sie wenig Neues, denn sie erklärt die [[q-vl00-essentiell|Folienauswahl „essentiell"]] Abschnitt für Abschnitt. Ihr Wert liegt in den [[#Klausurschwerpunkte|Klausurschwerpunkten]].

> [!note] Status im Wiki
> Die Lernhilfe ist **kein Kursmaterial**. Sie stammt von einem Studierenden; das PDF wurde am 19.06.2026 mit LaTeX erzeugt, ist englisch mit deutschen Fachbegriffen und nennt keinen Autor. Was nur hier steht, kommt im Wiki in grüne Callouts. Wo sie die Folien wiedergibt, bleibt die Folie der Beleg.

Seite = PDF-Seite.

## Zusammenfassung
Die Lernhilfe folgt der Gliederung von VL0 (Kapitel 1–4) und ergänzt zu jeder Folie das „Warum": Herleitungen, Intuition und Abwägungen. Grüne Kästen markieren Merkstoff, rote Kästen („Exam focus") wahrscheinliche Prüfungspunkte. Am Ende steht ein einseitiger Spickzettel mit Kernformeln und „Konzept-Reflexen". Der Abgleich mit den Folien (16.10.) ergab: Alle Formeln und Kernaussagen decken sich mit VL0, und zwei Folienfehler (Newton-Update, `model.test`) sind stillschweigend korrigiert. XAI, Fairness, die probabilistische Sicht und B-Splines fehlen – genau wie in VL0.

## Gliederung und Abgleich mit den Folien
| Seiten | Abschnitt | VL0-Folien | Wiki-Seiten | Abgleich |
|---|---|---|---|---|
| 2 | 1 Lernproblem | 4–6 | [[statistisches-lernproblem]] | deckungsgleich |
| 2–3 | 2.1–2.3 Lineare Regression, Designmatrix, Normalgleichung | 7–10 | [[lineare-regression]] | deckungsgleich; Herleitung wie VL1, F. 22–23; **neu:** Deutung als Orthogonalprojektion (grün ergänzt) |
| 4 | 2.4 Basisfunktionen | 11 | [[basisfunktionen-regression]] | deckungsgleich |
| 4–5 | 3 Bias-Varianz | 13–16 | [[bias-varianz-tradeoff]] | Formel wie VL2, F. 7; **neu:** Zielscheiben-Bild und U-Kurve (grün ergänzt) |
| 5–7 | 4 Modellwahl | 18–34 | [[modellauswahl-vorgehen]], [[modellauswahl-fallbeispiele]] | deckungsgleich; **neu:** Gen-Szenario (grün ergänzt als Fall C) |
| 7–10 | 5 Regularisierung | 36–57 | [[regularisierung]], [[ridge-regression]], [[lasso-regression]], [[l0-l1-l2-vergleich]], [[output-regularisierung]], [[early-stopping]], [[dropout]], [[label-smoothing]], [[train-val-test-split]] | deckungsgleich bis in die Tabellen (Entscheidungsschema, λ-Vorgehen, Diagnose); **neu:** hart ↔ weich über Lagrange (grün ergänzt); MAP-Hinweis steht schon in [[probabilistische-sicht-map]] |
| 10 | 6 Kreuzvalidierung | 59–61 | [[kreuzvalidierung]] | deckungsgleich; die Abwägung über $K$ steht im Wiki schon als ESL-Ergänzung |
| 10–12 | 7 Bootstrap | 62–67 | [[bootstrap]], [[bootstrap-oob-anteil]] | deckungsgleich; 36,8 % OOB steht nicht auf den Folien (im Wiki als ESL-Ergänzung) |
| 12 | 8 Bagging | 68 (nur Grafik) | [[bagging]] | vereinfacht, siehe Abweichungen |
| 12–15 | 9 Optimierung | 70–94 | [[optimierung-grundlagen]], [[gradientenabstieg]], [[momentum-und-nesterov]], [[sgd]], [[adaptive-optimierer]], [[newton-und-quasi-newton]], [[optimierer-uebersicht]] | deckungsgleich; Newton korrekt mit $\theta_t$ |
| 15 | 10 Spickzettel | – | [[formelsammlung]] | alle Formeln dort enthalten |

## Klausurschwerpunkte
Laut Jan decken sich diese Hinweise mit echten Klausuren. Sinngemäß übersetzt, in VL-Notation:

| S. | Was man können soll | Aufgabentyp | Wiki |
|---|---|---|---|
| 2 | $Y = f(x) + \varepsilon$ aufschreiben und jedes Symbol erklären; Vorhersage und Inferenz abgrenzen; $\varepsilon$ als Grund, warum keine perfekte Vorhersage möglich ist | erklären | [[statistisches-lernproblem]] |
| 3 | $\hat a = (X^TX)^{-1}X^Ty$ angeben und über $\nabla_a\lVert y - Xa\rVert^2 = 0$ **herleiten**; wann die Formel versagt ($X^TX$ singulär: mehr Unbekannte als Beobachtungen oder perfekt korrelierte Merkmale) und dass Ridge mit $+\lambda I$ hilft | herleiten | [[lineare-regression]], [[ridge-regression]] |
| 4 | warum Basisfunktions-Regression „linear" ist (linear in $a$, nicht in $x$); mehr oder flexiblere Basisfunktionen → weniger Bias, mehr Varianz | erklären | [[basisfunktionen-regression]] |
| 5 | Fehlerzerlegung in drei Terme aufschreiben und jeden in Worten erklären; Komplexität ↑ → Bias ↓, Varianz ↑, Gesamtfehler U-förmig, $\sigma^2$ konstant | Formel + erklären | [[bias-varianz-tradeoff]] |
| 7 | **Szenario → Methode empfehlen und begründen**, z. B. 500 Patienten, 2000 Gen-Merkmale, Erklärbarkeit nötig. Denkweg: Problemtyp → N vs. p → Datentyp → Anforderungen; immer mit einer Baseline beginnen | offene Fallaufgabe | [[modellauswahl-vorgehen]], [[modellauswahl-fallbeispiele#Fall C · Genexpression]] |
| 10 | weiche Regularisierung $\text{Loss} + \lambda\mathcal R(\theta)$ mit den Grenzfällen $\lambda = 0$ und $\lambda\to\infty$; L1 → Sparsity, L2 → glattes Schrumpfen, L0 ideal, aber nicht handhabbar; Early Stopping ≙ L2 beim linearen Modell; aus der Lücke Train/Val ablesen, ob $\lambda$ rauf oder runter muss | Formel + Diagnose | [[regularisierung]], [[l0-l1-l2-vergleich]], [[early-stopping]] |
| 10 | K-Fold Schritt für Schritt beschreiben; warum stratifizieren (Klassenverhältnis) und warum mitteln (nicht von einem Glücks-Split abhängen); Test-Set nur einmal ganz am Ende | Verfahren beschreiben | [[kreuzvalidierung]], [[train-val-test-split]] |
| 12 | Ziehen **mit Zurücklegen** auf dieselbe Größe $n$; zwei Intervalltypen (Perzentil, Normal ± 1,96 SE); ca. 63,2 % in-bag und 36,8 % OOB; Bootstrap = Unsicherheit, CV = Generalisierung | Begriffe + Zahl | [[bootstrap]], [[bootstrap-oob-anteil]], [[cv-vs-bootstrap]] |
| 12 | Bagging als Ensemble-Hebel der Bias-Varianz-Folie, umgesetzt per Bootstrap; senkt Varianz, nicht Bias; Gegensatz Boosting (sequentiell, senkt Bias) | abgrenzen | [[bagging]] |
| 15 | GD-Update und Lernraten-Abwägung; was Momentum und Nesterov beheben (Zickzack, schlechte Konditionierung) samt Geschwindigkeits-Update; SGD vs. GD; erste vs. zweite Ordnung ($H^{-1}$ teuer); Adam als robuster Default; warum Konvexität „lokal = global" garantiert | Formel + abgrenzen | [[gradientenabstieg]], [[momentum-und-nesterov]], [[sgd]], [[newton-und-quasi-newton]], [[optimierer-uebersicht]], [[optimierung-grundlagen]] |

**Aufgabentypen daraus:** Formeln aufschreiben und Terme erklären, herleiten (Normalgleichung), Verfahren schrittweise beschreiben, Begriffe abgrenzen, Fallszenario mit begründetem Modellvorschlag, Diagnose aus Train- und Val-Fehlern. Zahlenrechnungen nennen die Kästen nicht ausdrücklich.

## „Konzept-Reflexe" vom Spickzettel (S. 15)
- Komplexität ↑ ⇒ Bias ↓, Varianz ↑ – die Mitte wählen.
- L1 = Sparsity und Merkmalsauswahl, L2 = glattes Schrumpfen, L0 = ideal, aber nicht handhabbar.
- Early Stopping ≙ L2 (lineare Modelle).
- CV → Generalisierung schätzen und tunen; Bootstrap → Unsicherheit quantifizieren (mit Zurücklegen).
- Bagging senkt Varianz, Boosting senkt Bias.
- Konvex ⇒ jedes lokale Minimum ist global.
- Adam = robuster Default; SGD-Rauschen hilft aus Sattelbereichen; Newton schnell, aber $H^{-1}$ teuer.
- $p \gg N$ ⇒ linear, starke Regularisierung, Merkmalsauswahl; $N \gg p$ ⇒ komplexe Modelle sind weniger riskant.

## Was über die Folien hinausgeht – und wo es im Wiki steht
| Lernhilfe | Wiki | Beleg der Ergänzung |
|---|---|---|
| $\hat y = X\hat a$ als Orthogonalprojektion von $y$ auf den Spaltenraum von $X$ (S. 3) | [[lineare-regression]], grün | ESL 3.2, S. 46 |
| Zielscheiben-Bild für Bias und Varianz, U-förmiger Gesamtfehler (S. 5) | [[bias-varianz-tradeoff]], grün | ESL 2.9, Abb. 2.11, S. 38 |
| harte und weiche Regularisierung über Lagrange-Dualität verknüpft (S. 8) | [[regularisierung]], grün | ESL 3.4.1, Gl. (3.41)/(3.42), S. 63 |
| Gen-Szenario für die Modellwahl (S. 7) | [[modellauswahl-fallbeispiele]], Fall C, grün | ESL 7.10.2, S. 245 (CV-Fallstrick bei Gendaten) |

Schon vorher im Wiki: Herleitung der Normalgleichung ([[q-vl01-glm-basisfunktionen-splines|VL1]], F. 22–23), Abwägung über $K$ bei der CV und 36,8 % OOB (ESL-Ergänzungen), Bagging vs. Boosting, MAP-Deutung ([[probabilistische-sicht-map]]), Monte-Carlo-Dropout.

## Widersprüche / Abweichungen zu anderen Quellen
- **Keine inhaltlichen Widersprüche** zu den Folien.
- **Newton (S. 14):** korrekt $\theta_{t+1} = \theta_t - H(\theta_t)^{-1}\nabla f(\theta_t)$. VL0, F. 91 lässt $\theta_t$ weg; das Wiki hatte den Folienfehler schon korrigiert ([[newton-und-quasi-newton]]).
- **Dropout (S. 8):** `model.train()` vs. `model.eval()`. VL0, F. 51 schreibt `model.test`; das Wiki verwendet schon `model.eval()` ([[dropout]]).
- **Bagging (S. 12):** Die Varianz sinkt „gegen $\sigma^2/B$" nur bei unabhängigen Modellen; die Lernhilfe schreibt selbst „roughly independent". Das Wiki behält die allgemeinere Formel $\rho\sigma^2 + \frac{1-\rho}{B}\sigma^2$ (ESL 15.2, [[bagging]]).
- **Notation (S. 3, Exam focus):** „p > K" mischt die Bezeichnungen. In VL-Notation ($n$ Merkmale, $K$ Beobachtungen) ist $X^TX$ singulär, sobald $n + 1 > K$.
- **Output-Regularisierung (S. 8):** gibt beide Folienformen wieder, VL0, F. 41 ($\mathcal R(y_i)$ ohne $\lambda$) und F. 48 ($\lambda\mathcal R(f_\theta(x_i))$). Gemeint ist dasselbe Prinzip ([[output-regularisierung]]).

## Offene Fragen
- Kamen XAI, Fairness oder die probabilistische Sicht in der Klausur vor? Die Lernhilfe schweigt dazu, weil sie nur VL0 abdeckt. Vielleicht wissen es die Absolventen ([[overview#Offene Fragen an Jan]]).
- Gab es Rechenaufgaben mit Zahlen (GD-Schritte, Normalgleichung, Shapley-Werte)? Die Kästen nennen vor allem Herleiten, Erklären und Begründen.
