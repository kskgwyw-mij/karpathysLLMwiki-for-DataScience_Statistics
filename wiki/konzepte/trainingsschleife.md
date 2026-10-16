---
typ: konzept
themen: [optimierung, neuronale-netze, lab]
aliases: [Training Loop, Trainingsloop, Autograd, automatische Differenzierung, zero_grad, Closure]
quellen: ["[[q-lab01-pytorch-introduction]]", "[[q-lab02-pytorch-optimizer-comparison]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-16
---

# Trainingsschleife und automatische Differenzierung

#flashcards/statistik

> [!info] Relevanz
> Stammt aus dem Lab, nicht aus den Folien. In einer Papierklausur werden kaum PyTorch-Befehle gefragt, wohl aber die Mechanik dahinter: Was berechnet der Gradient, warum zurücksetzen, was ändert sich zwischen Training und Auswertung → `mittel`.

## Kurzdefinition
Ein neuronales Netz wird trainiert, indem man für jeden Minibatch **Vorhersage → Loss → Gradienten zurücksetzen → Gradienten berechnen → Parameter aktualisieren** wiederholt. Die Gradienten liefert die **automatische Differenzierung** (Autograd), die Aktualisierung übernimmt der Optimierer (→ [[q-lab01-pytorch-introduction|Lab 1]], Abschnitt „Key takeaways").

## Intuition
Ein Netz ist eine Funktion mit lernbaren Parametern. Für das Training will man wissen: Wie ändert sich der Loss, wenn ich ein Gewicht ein wenig ändere? Das ist der Gradient. PyTorch merkt sich beim Vorwärtsrechnen alle Operationen als **Rechengraph** und wendet beim Rückwärtsrechnen die **Kettenregel** an. Man muss also keine Ableitungen von Hand bilden (→ Lab 1, Abschnitt „Automatic differentiation in PyTorch"). Welche Update-Regel aus dem Gradienten einen Schritt macht, entscheidet der Optimierer (→ [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitt „Optimization in neural networks").

## Formal
**Das Trainingsmuster** (→ Lab 1, Abschnitte „The training loop", „Key takeaways"):

| Schritt | PyTorch | Mathematisch |
|---|---|---|
| 1 Vorwärts | `logits = model(images)` | $\hat y = f_\theta(x_B)$ |
| 2 Loss | `loss = criterion(logits, labels)` | $\ell_B(\theta)$ |
| 3 Zurücksetzen | `optimizer.zero_grad()` | alte Gradienten löschen |
| 4 Rückwärts | `loss.backward()` | $\nabla_\theta\ell_B(\theta)$ per Kettenregel |
| 5 Update | `optimizer.step()` | z. B. $\theta \leftarrow \theta - \eta\nabla_\theta\ell_B$ ([[sgd]]) |

**Bausteine:**
- **Autograd:** Tensoren mit `requires_grad=True` werden verfolgt; nach `backward()` steht der Gradient im Attribut `.grad` (→ Lab 1, Abschnitt „Automatic differentiation in PyTorch").
- **Gradienten akkumulieren:** Jeder `backward()`-Aufruf **addiert** zu den vorhandenen Gradienten. Für den Gradienten des aktuellen Batches muss man vorher zurücksetzen (→ Lab 1, Abschnitt „Why gradients are reset to zero").
- **Modus:** `model.train()` im Training, `model.eval()` in der Auswertung; das schaltet z. B. Dropout an und aus (→ Lab 1, Abschnitt „Dropout"; [[dropout]]). Ausgewertet wird zusätzlich unter `torch.no_grad()`, damit kein Rechengraph entsteht (→ Lab 1, Abschnitt „Writing an evaluation function").
- **Logits und Loss:** Die letzte Schicht liefert Logits, keine Wahrscheinlichkeiten; `CrossEntropyLoss` verarbeitet sie direkt (→ Lab 1, Abschnitt „A simple neural network").
- **Regularisierung im Loop:** L2 über den Optimizer-Parameter `weight_decay`, L1 von Hand **vor** `backward()` zum Loss addieren, Label Smoothing als Argument von `CrossEntropyLoss` (→ Lab 1, Abschnitte „L2 regularization", „L1 regularization", „Label smoothing"). Early Stopping merkt sich nach jeder Epoche den Zustand mit der besten Val-Loss und lädt ihn am Ende (→ Lab 1, Funktion `train_model`; [[early-stopping]]).
- **Closure bei L-BFGS:** L-BFGS kann pro Update Loss und Gradient **mehrfach** auswerten. Deshalb bekommt `optimizer.step(closure)` eine Funktion, die Zurücksetzen, Vorwärtsrechnung, Loss und `backward()` selbst ausführt (→ Lab 2, Abschnitte „The optimizers we compare", „Training function"; [[newton-und-quasi-newton]]).
- **Epoche und Minibatch:** Der `DataLoader` teilt die Daten in Minibatches (z. B. 128 Bilder) und mischt die Trainingsdaten (→ Lab 1, Abschnitt „DataLoader"; [[sgd]]).

## Annahmen & Voraussetzungen
- Der Loss ist differenzierbar nach den Parametern (sonst kein Gradient).
- Val- und Testdaten werden im Modus `eval()` und ohne Parameter-Update ausgewertet.

## Beispiel
**Ein Gewicht lernen** (→ Lab 1, Abschnitt „Automatic differentiation for a tiny learning problem"): Modell $\hat y = w\cdot 3$, Ziel 6, Loss $(\hat y - 6)^2$, Start $w = 0{,}5$, Lernrate 0,05. Ausgabe: $w$ = 1,85; 1,985; 1,9985; 1,9998; ab Schritt 4 2,0000. Der Loss fällt von 20,25 auf 0.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – die Lab-Zahlen von Hand
> $\ell(w) = (3w - 6)^2 = 9(w-2)^2$, also $\ell'(w) = 18(w - 2)$.
> - Schritt 0: $\ell'(0{,}5) = 18\cdot(-1{,}5) = -27$ ⇒ $w = 0{,}5 + 0{,}05\cdot 27 = 1{,}85$ ✓
> - Schritt 1: $\ell'(1{,}85) = -2{,}7$ ⇒ $w = 1{,}85 + 0{,}135 = 1{,}985$ ✓
> - Allgemein: $w_{t+1} - 2 = (1 - 0{,}05\cdot 18)(w_t - 2) = 0{,}1\,(w_t - 2)$. Der Abstand zum Optimum schrumpft pro Schritt auf ein Zehntel.
>
> Das ist dieselbe Rechnung wie für $f(\theta) = \theta^2$ in [[gradientenabstieg]]: Konvergenz für $0 < \eta < 2/18 \approx 0{,}111$. Mit $\eta = 0{,}12$ würde $w$ oszillierend divergieren (Faktor $-1{,}16$).

> [!beispiel] Rechenbeispiel (nicht aus der VL) – was ohne `zero_grad()` passiert
> Gleiches Modell, gleiche Lernrate, aber ohne Zurücksetzen. Nach Schritt 0 ist `.grad` = −27, $w = 1{,}85$. In Schritt 1 kommt der neue Gradient −2,7 **dazu**: `.grad` = −29,7, $w = 1{,}85 + 0{,}05\cdot 29{,}7 = 3{,}335$. Das Gewicht schießt weit über 2 hinaus, weil ein veralteter Gradient weiterwirkt. Die Übung im Lab („Remove `optimizer.zero_grad()` and observe what goes wrong") zielt genau darauf.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Gradient Accumulation
> Passt ein Batch von 256 Bildern nicht in den GPU-Speicher, rechnet man vier Batches à 64 Bilder und ruft nach jedem Batch `backward()` auf, aber erst nach dem vierten `step()` und `zero_grad()`. Die akkumulierten Gradienten wirken dann wie ein großer Batch. Teilt man jeden Loss durch 4, entspricht das genau dem Mittelwert über 256 Bilder. Das Akkumulieren, das sonst ein Fehler ist, wird hier bewusst genutzt.

## Typische Prüfungsfragen & Fallstricke
- „Beschreiben Sie die Schritte eines Trainingsschritts." → die fünf Schritte oben.
- „Warum setzt man die Gradienten zurück?" → Sie akkumulieren standardmäßig.
- „Was ändert `model.eval()`?" → Dropout aus (alle Neuronen aktiv); für die Auswertung zusätzlich `no_grad`.
- „Warum braucht L-BFGS eine Closure, SGD und Adam nicht?" → L-BFGS wertet Loss und Gradient pro Update eventuell mehrfach aus (Übungsfrage 10 in Lab 2).
- Fallstrick: L1 erst nach `backward()` addieren → geht nicht in den Gradienten ein.
- Fallstrick: Test-Accuracy im Modus `train()` messen → Dropout verfälscht das Ergebnis.

## Verwandt
- [[gradientenabstieg]] – das Update in Schritt 5 im einfachsten Fall
- [[sgd]] – Minibatches und Epochen
- [[adaptive-optimierer]] – andere Update-Regeln in `optimizer.step()`
- [[newton-und-quasi-newton]] – L-BFGS und die Closure
- [[dropout]] – Grund für den Unterschied zwischen `train()` und `eval()`
- [[early-stopping]] – im Loop über die Val-Loss umgesetzt
- [[q-lab01-pytorch-introduction|Lab 1]], [[q-lab02-pytorch-optimizer-comparison|Lab 2]] – Kurs-Notebooks

## Karteikarten
Die fünf Schritte eines Trainingsschritts?::Vorhersage (Forward), Loss berechnen, Gradienten zurücksetzen (`zero_grad`), Gradienten berechnen (`backward`), Parameter aktualisieren (`step`).
Warum ruft man vor `backward()` `zero_grad()` auf?::Weil PyTorch Gradienten standardmäßig akkumuliert; ohne Zurücksetzen würden alte Gradienten weiterwirken.
Was macht Autograd?::Es zeichnet die Rechenoperationen als Rechengraph auf und berechnet bei `backward()` die Gradienten per Kettenregel; sie stehen dann in `.grad`.
Was ändert sich zwischen `model.train()` und `model.eval()`?::Im Training ist Dropout aktiv, in der Auswertung nicht.
Wie baut man L1 bzw. L2 im Lab ein?::L2 über `weight_decay` im Optimizer, L1 von Hand als Zusatzterm zum Loss vor `backward()`.
Warum braucht L-BFGS in PyTorch eine Closure?::Weil es pro Update Loss und Gradient mehrfach auswerten kann; die Closure rechnet beides neu.
Sind Logits Wahrscheinlichkeiten?::Nein, Rohwerte der letzten Schicht; `CrossEntropyLoss` verarbeitet sie direkt.

## Quellen
- [[q-lab01-pytorch-introduction|Lab 1]], Abschnitte „Automatic differentiation in PyTorch" bis „Key takeaways"
- [[q-lab02-pytorch-optimizer-comparison|Lab 2]], Abschnitte „Optimization in neural networks", „The optimizers we compare", „Training function", „Try"
