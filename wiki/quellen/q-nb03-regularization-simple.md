---
typ: quelle
themen: [regularisierung, notebook]
aliases: [NB3]
datei: "raw/uebungen/nb03-regularization-simple.ipynb"
original: "4_Jupyter Notebooks/3_regularization_pytorch_simple.json"
relevanz: niedrig
aktualisiert: 2026-10-15
---

# NB3 · Regularisierung, vereinfachte Version (MNIST, PyTorch)

## Zusammenfassung
Vereinfachte Fassung von [[q-nb02-mnist-regularization|NB2]] mit Standard-`nn.Linear`, 5 500 Trainings-, 500 Validierungs- und 500 Testbildern. Regularisierungsparameter werden in einer Zelle von Hand gesetzt; gespeicherter Lauf: L1 mit $\lambda = 10^{-5}$, 10 Epochen, Test-Accuracy 0,938.

## Auffälligkeiten im Code (gut zum Üben von Code-Review)
> [!warning] Fehler im Notebook
> - `if 'use_early_stopping'==True:` vergleicht einen **String** mit `True` → immer falsch. Early Stopping wird nie ausgeführt, obwohl die Variable `early_stopping` existiert.
> - Der L2-Term nutzt `torch.norm(param)`, also $\|\theta\|_2$ und nicht $\|\theta\|_2^2$ wie in der Vorlesung.
> - Ausgegeben wird „best val loss", tatsächlich ist es die Val-Loss der **letzten** Epoche.

## Prüfungsrelevante Aussagen
- L1 als Summe der Beträge aller Parameter, zum Loss addiert → [[lasso-regression]].
- Sparsity-Anteil ($\lvert w\rvert < 10^{-3}$) in fc1 mit L1 ($\lambda = 10^{-5}$): 6,4 %. Ein direkter Vergleich mit [[q-nb02-mnist-regularization|NB2]] ist wegen anderer Datenmengen und Epochen nur grob möglich.

## Offene Fragen
- Keine.
