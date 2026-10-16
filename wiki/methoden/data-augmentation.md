---
typ: methode
themen: [regularisierung, neuronale-netze, lab]
aliases: [Data Augmentation, Datenaugmentation, Datenerweiterung]
quellen: ["[[q-lab-abschlussprojekt-aufgabe]]", "[[q-vl00-essentiell]]", "[[q-abschlussprojekt-abgabe]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-16
---

# Data Augmentation (Datenaugmentation)

#flashcards/statistik

> [!info] Relevanz
> In den Folien kommt Datenaugmentation nur als Maßnahme bei Über-Regularisierung vor (→ [[q-vl00-essentiell|VL0]], F. 57). Die Lab-Aufgabe zählt sie ausdrücklich zu den Regularisierungsmethoden (→ [[q-lab-abschlussprojekt-aufgabe|Kaggle-Aufgabe]], S. 1). Eine eigene Folie mit Definition gibt es laut aktuellem Wiki-Stand nicht → `mittel`.

## Kurzdefinition
Regularisierungsmethode, die in der Lab-Aufgabe neben L1, L2/Weight Decay, Dropout, Early Stopping, Label Smoothing und Output-Regularisierung steht (→ [[q-lab-abschlussprojekt-aufgabe|Kaggle-Aufgabe]], S. 1). In der Diagnose-Tabelle der VL ist „Datenaugmentation prüfen" eine Maßnahme bei Über-Regularisierung, also bei hohem Bias (→ [[q-vl00-essentiell|VL0]], F. 57; [[regularisierung]]).

> [!ergaenzung] Ergänzung (nicht aus der VL) – Definition
> Data Augmentation erweitert die Trainingsdaten um **zufällig transformierte Kopien**, deren Label sich durch die Transformation nicht ändert: Bilder spiegeln, verschieben, zuschneiden oder drehen, bei Audio Rauschen hinzufügen. Das Modell sieht in jeder Epoche leicht andere Varianten und lernt, gegenüber diesen Transformationen **invariant** zu sein. Das Vorwissen „ein gespiegelter Schuh ist immer noch ein Schuh" fließt so in das Training ein, ähnlich wie bei der Regularisierung durch Design ([[regularisierung]]).

## Intuition
> [!ergaenzung] Ergänzung (nicht aus der VL) – warum Augmentation regularisiert
> Mehr (künstliche) Daten senken die Varianz, ohne die Modellkapazität zu beschneiden ([[bias-varianz-tradeoff]]). Das passt zur VL-Diagnose: Wenn das Modell schon zu stark eingeschränkt ist (Train **und** Val schlecht), hilft ein kleineres $\lambda$ zusammen mit Augmentation, weil man Varianz über die Daten statt über Strafterme kontrolliert.

## Formal
> [!ergaenzung] Ergänzung (nicht aus der VL)
> Statt $\frac1N\sum_i \mathcal D(f_\theta(x_i), y_i)$ minimiert man den erwarteten Loss über eine Verteilung zulässiger Transformationen $T$:
> $$\frac1N\sum_{i=1}^N \mathbb E_{T}\Big[\mathcal D\big(f_\theta(T(x_i)), y_i\big)\Big]$$
> In der Praxis wird pro Minibatch eine zufällige Transformation gezogen. Es gibt keinen Strafterm und kein $\lambda$; die Stärke steckt in der Wahl und im Umfang der Transformationen.

## Annahmen & Voraussetzungen
> [!ergaenzung] Ergänzung (nicht aus der VL)
> - Die Transformation muss **semantikerhaltend** sein, also das Label nicht ändern.
> - Nur auf den **Trainingssplit** anwenden; Val und Test bleiben unverändert, sonst misst man eine andere Verteilung.
>
> Beides ist in der [[q-abschlussprojekt-abgabe|eigenen Abgabe]] (S. 1) so umgesetzt und begründet.

## Beispiel
> [!beispiel] Anwendungsfall (nicht aus der VL) – eigenes Abschlussprojekt (Fashion-MNIST)
> In der [[q-abschlussprojekt-abgabe|eigenen Abgabe]] besteht die Augmentation aus horizontalem Spiegeln und RandomCrop mit Padding 2, nur im Training. Vertikales Spiegeln wurde bewusst weggelassen: Kopfstehende Kleidung kommt im Datensatz nicht vor.
> - Ergebnis: Augmentation ist im Mittel über die vier Optimierer das **einzige** Setup unter der unregularisierten Referenz (Val-Acc 0,9197 gegenüber 0,9213).
> - Grund laut Abgabe: Bei höchstens 12 Epochen konvergieren die augmentierten Läufe langsamer und sind am Ende noch nicht auskonvergiert. Augmentation braucht ein größeres Trainingsbudget.

> [!beispiel] Rechenbeispiel (nicht aus der VL) – wie viele Varianten ein Bild hat
> RandomCrop auf 28×28 nach Padding 2 (Bild 32×32): Der Ausschnitt kann um 0 bis 4 Pixel horizontal und vertikal verschoben werden, also $5\cdot 5 = 25$ Positionen. Mit oder ohne Spiegeln ergibt das $25\cdot 2 = 50$ Varianten pro Bild. Bei 54 000 Trainingsbildern sieht das Modell damit bis zu $2{,}7$ Mio. verschiedene Eingaben, in 12 Epochen aber höchstens $12\cdot 54\,000 = 648\,000$ davon. Auch daran sieht man, warum Augmentation mehr Epochen braucht.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Röntgenbilder
> Ein Klassifikator für Thorax-Röntgenbilder mit 2 000 Trainingsbildern: Kleine Drehungen (±5°) und Helligkeitsänderungen sind sinnvoll. Horizontales Spiegeln dagegen nicht, weil das Herz auf der linken Seite liegt und eine gespiegelte Aufnahme medizinisch eine andere Situation zeigt. Die Wahl der Transformationen ist Vorwissen über die Daten.

## Vor- und Nachteile
> [!ergaenzung] Ergänzung (nicht aus der VL)
> | Vorteile | Nachteile |
> |---|---|
> | kein zusätzlicher Hyperparameter $\lambda$, Kapazität bleibt erhalten | längeres Training nötig |
> | bringt Invarianzen als Vorwissen ein | falsche Transformation zerstört die Semantik |
> | kombinierbar mit L2, Dropout, Label Smoothing | vor allem für Bild-, Audio- und Textdaten naheliegend, bei Tabellendaten schwierig |

## Typische Prüfungsfragen & Fallstricke
- „Nennen Sie Regularisierungsmethoden für neuronale Netze." → Data Augmentation gehört laut Lab-Aufgabe dazu.
- „Was tun bei Über-Regularisierung?" → $\lambda$ reduzieren, Kapazität leicht erhöhen, Datenaugmentation prüfen (VL0, F. 57).
- Fallstrick: Val- oder Testdaten augmentieren.
- Fallstrick: Augmentation mit zu kleinem Epochenbudget bewerten (siehe eigenes Projekt).

## Verwandt
- [[regularisierung]] – Taxonomie und Diagnose-Tabelle, aus der die Maßnahme stammt
- [[label-smoothing]] – verändert ebenfalls die Trainingsdaten statt der Parameter, aber die Labels statt der Eingaben
- [[dropout]] – weitere Zufallsregularisierung, die nur im Training wirkt
- [[early-stopping]] – Trainingsbudget und Augmentation hängen zusammen
- [[bias-varianz-tradeoff]] – mehr Daten senken die Varianz

## Karteikarten
Zählt Data Augmentation zu den Regularisierungsmethoden?::Ja, die Lab-Aufgabe nennt sie neben L1, L2, Dropout, Early Stopping, Label Smoothing und Output-Regularisierung.
Bei welcher Diagnose nennt die VL „Datenaugmentation prüfen"?::Bei Über-Regularisierung ($\lambda$ zu groß, Train und Val schlecht): $\lambda$ reduzieren, Kapazität leicht erhöhen, Datenaugmentation prüfen.
Auf welche Splits wendet man Data Augmentation an?::Nur auf den Trainingssplit, nie auf Val oder Test.
Welche Bedingung muss eine Augmentations-Transformation erfüllen?::Sie muss semantikerhaltend sein, das Label darf sich nicht ändern.
Warum schnitt Augmentation im eigenen Fashion-MNIST-Projekt schlechter ab als die Referenz?::Bei höchstens 12 Epochen waren die augmentierten Läufe noch nicht auskonvergiert; Augmentation braucht mehr Trainingsbudget.

## Quellen
- [[q-lab-abschlussprojekt-aufgabe|Kaggle-Aufgabe]], S. 1
- [[q-vl00-essentiell|VL0]], Folie 57
- [[q-abschlussprojekt-abgabe|eigene Abgabe]], S. 1–2
