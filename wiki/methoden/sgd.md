---
typ: methode
themen: [optimierung]
aliases: [Stochastic Gradient Descent, SGD, Minibatch, Batch Size, Epoche]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Stochastischer Gradientenabstieg (SGD)

#flashcards/statistik

## Kurzdefinition
Statt des vollen Gradienten über alle Daten wird ein **Minibatch-Gradient** verwendet: $\theta_{t+1} = \theta_t - \eta_t\nabla\ell_B(\theta_t)$. Jedes Update nutzt nur eine kleine Teilmenge $B$ der Beobachtungen (→ [[q-vl00-essentiell|VL0]], F. 86).

## Intuition
Der volle Gradient ist ein Mittelwert über alle $N$ Beobachtungen – bei Millionen Datenpunkten teuer. Ein Minibatch liefert eine verrauschte, aber erwartungstreue Schätzung zu einem Bruchteil der Kosten. Man macht viele billige, ungenaue Schritte statt weniger teurer, genauer. Das Rauschen kann sogar helfen, flachen lokalen Minima und Sattelregionen zu entkommen.

## Formal
$$\theta_{t+1} = \theta_t - \eta_t\,\nabla\ell_B(\theta_t), \qquad \nabla\ell_B = \frac{1}{\lvert B\rvert}\sum_{z\in B}\nabla\ell(\theta; z)$$
**Pseudocode** (→ VL0, F. 88):
```
Input: Datensatz D, Loss ℓ, Start θ₀, Lernraten ηₜ, Batchgröße m
für epoch = 1, 2, …:
    D mischen
    für jeden Minibatch B der Größe m:
        g = (1/|B|) Σ_{z∈B} ∇ℓ(θ; z)
        θ = θ − ηₜ g
return θ
```
- **Full-Batch-GD** ist der Spezialfall $B = D$.
- **Online-SGD** ist der Spezialfall $\lvert B\rvert = 1$.

Eigenschaften (→ VL0, F. 86): Rauschen kann helfen, flachen lokalen Minima und Sattelregionen zu entkommen; braucht Lernraten-Schedules und sorgfältiges Batching; **dominante Methode** für das Training großer neuronaler Netze.

### Praktische Entscheidungen (→ VL0, F. 87)
| Wahl | Effekt | Faustregel |
|---|---|---|
| Batchgröße | steuert Gradientenrauschen und Hardware-Effizienz | mit Zweierpotenzen starten; größere Batches brauchen oft größere Lernraten |
| Lernraten-Decay | reduziert Rauschen nahe dem Optimum | Validierungskurven nutzen; cosine oder step decay üblich |
| Shuffling | vermeidet systematischen Update-Bias | jede Epoche mischen (i. i. d.-Training) |
| Early Stopping | wirkt als Regularisierung | Validierungs-Loss überwachen, nicht nur Trainings-Loss |

> [!ergaenzung] Ergänzung (nicht aus der VL)
> Bei konstanter Lernrate „zittert" SGD um das Optimum, weil das Gradientenrauschen nicht verschwindet – deshalb Decay. Eine **Epoche** = einmal alle Daten gesehen = $\lceil N/m\rceil$ Updates (in NB2: 5 000 Bilder, Batch 128 → 40 Updates pro Epoche).

## Annahmen & Voraussetzungen
- Zielfunktion ist eine Summe/ein Mittel über Beobachtungen; Minibatches annähernd repräsentativ (deshalb Shuffling).

## Typische Prüfungsfragen & Fallstricke
- Unterschied GD, SGD, Online-SGD.
- „Warum mischt man jede Epoche?" → kein systematischer Update-Bias.
- „Warum Lernraten-Decay bei SGD?" → Rauschen nahe dem Optimum reduzieren.
- Fallstrick: Summe statt Mittelwert über den Batch → Lernrate hängt von der Batchgröße ab.

## Verwandt
- [[gradientenabstieg]] – Full-Batch-Spezialfall
- [[adaptive-optimierer]] – AdaGrad, Adam auf Basis stochastischer Gradienten
- [[momentum-und-nesterov]] – häufig kombiniert
- [[early-stopping]] – Regularisierung im SGD-Training
- [[modellauswahl-vorgehen]] – „SGD-Minimierung des linearen Modells" bei großen Datenmengen

## Karteikarten
Wie lautet das SGD-Update?::$\theta_{t+1}=\theta_t-\eta_t\nabla\ell_B(\theta_t)$ mit Minibatch-Gradient $\nabla\ell_B$.
Welche Spezialfälle von SGD nennt die VL?::Full-Batch-GD ($B=D$) und Online-SGD ($\lvert B\rvert=1$).
Welchen Vorteil kann das Rauschen bei SGD haben?::Es kann helfen, flachen lokalen Minima und Sattelregionen zu entkommen.
Warum Shuffling in jeder Epoche?::Um systematischen Update-Bias zu vermeiden.
Faustregel Batchgröße und Lernrate?::Mit Zweierpotenzen starten; größere Batches brauchen oft größere Lernraten.
Was wird beim Early Stopping im SGD-Training überwacht?::Der Validierungs-Loss, nicht nur der Trainings-Loss.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 85–88
- [[q-vl04-map-resampling-optimierung|VL4]], Folien 40–43
