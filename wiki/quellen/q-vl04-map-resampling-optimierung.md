---
typ: quelle
themen: [map, regularisierung, resampling, optimierung]
aliases: [VL4]
datei: "raw/vorlesung/vl04-map-resampling-optimierung.pdf"
original: "3_Slides/4_Statistisches_Lernen_2_v4.pdf"
relevanz: hoch
aktualisiert: 2026-10-15
---

# VL4 · Probabilistische Sicht, Resampling, Optimierung (81 Folien)

Folie = PDF-Seite. Weitgehend englisch.

## Zusammenfassung
Englische Wiederholung der probabilistischen Sicht und der Hyperparameter-Folien aus VL3, Cross-Validation, dann **Bootstrapping** (Idee, Pseudocode, Konfidenzintervalle, Einsatz im statistischen Lernen, OOB-Evaluation, Bagging). Der größte Teil ist **Kapitel 3 Optimierung**: Lernen als Optimierung, Minima-Typen, Max→Min, Konvexität (mit Definition), Gradientenabstieg, Schrittweitenstrategien, Momentum, Nesterov, SGD, adaptive Verfahren im Detail (AdaGrad, AdaDelta, Adam) mit Pseudocode, Newton, gedämpftes Newton, BFGS, L-BFGS, LION, Initialisierung, restringierte Optimierung (Lagrange/KKT, projizierter und proximaler Gradient, Algorithmenfamilien) und Takeaways.

## Gliederung
| Folien | Inhalt | Wiki-Seiten |
|---|---|---|
| 1–4 | Probabilistische Sicht (englisch) | [[probabilistische-sicht-map]] |
| 5–11 | Entscheidungsschema, Hyperparameter, Diagnose, Shortcuts | [[regularisierung]] |
| 12–15 | Cross-Validation | [[kreuzvalidierung]] |
| 16–22 | Bootstrap, CIs, OOB, Bagging | [[bootstrap]], [[bagging]] |
| 23–30 | Lernen als Optimierung, Minima, Konvexität | [[optimierung-grundlagen]] |
| 31–39 | GD, Schrittweite, Momentum, Nesterov (Pseudocode) | [[gradientenabstieg]], [[momentum-und-nesterov]] |
| 40–53 | SGD, AdaGrad, AdaDelta, Adam, Vergleich | [[sgd]], [[adaptive-optimierer]] |
| 54–61 | Newton, gedämpftes Newton, BFGS, L-BFGS, 1. vs. 2. Ordnung | [[newton-und-quasi-newton]] |
| 62–63 | LION | [[adaptive-optimierer]] |
| 64–67 | Initialisierung | [[initialisierung]] |
| 68–78 | Restringierte Optimierung | [[restringierte-optimierung]] |
| 79–81 | Algorithmenübersicht, Takeaways | [[optimierer-uebersicht]] |

## Prüfungsrelevante Aussagen
- Konvexität: $f(\lambda\alpha + (1-\lambda)\beta) \le \lambda f(\alpha) + (1-\lambda) f(\beta)$ für $0\le\lambda\le 1$ (Folie 29).
- Adam-Defaults: $\alpha = 0{,}001$, $\beta_1 = 0{,}9$, $\beta_2 = 0{,}999$, $\varepsilon = 10^{-8}$; AdamW meist besser als L2 in Adam (Folie 50).
- AdaGrad ist ein **diagonaler Vorkonditionierer** (Folie 46).
- Proximaler Operator: $\mathrm{prox}_{\eta h}(v) = \arg\min_\theta\, h(\theta) + \frac{1}{2\eta}\|\theta - v\|^2$; L1 → Soft-Thresholding, Indikatorfunktion → Projektion (Folie 74).
- KKT: Stationarität, primale und duale Zulässigkeit, komplementärer Schlupf (Folie 70).

## Widersprüche / Abweichungen
- Folie 55: Newton-Update als $\theta_{t+1} = -H(\theta_t)^{-1}\nabla f(\theta_t)$ geschrieben; korrekt ist $\theta_{t+1} = \theta_t - H(\theta_t)^{-1}\nabla f(\theta_t)$ (vgl. Pseudocode Folie 57). Siehe [[newton-und-quasi-newton]].
- Folie 2 ist mit „Probabilistische Sicht" (deutsch) betitelt, Inhalt englisch.

## Offene Fragen
- Die Teile Initialisierung, restringierte Optimierung, Details zu AdaGrad/AdaDelta/LION fehlen in [[q-vl00-essentiell|VL0]] → `relevanz: mittel`.
