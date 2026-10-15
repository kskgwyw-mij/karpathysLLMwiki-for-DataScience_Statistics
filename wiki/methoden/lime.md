---
typ: methode
themen: [xai, lokal]
aliases: [LIME, Local Interpretable Model-agnostic Explanations, Lokales Surrogat, Surrogatmodell]
quellen: ["[[q-vl06-xai-lokal-fairness]]", "[[q-nb07-local-xai]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# LIME (lokales Surrogatmodell)

#flashcards/statistik

## Kurzdefinition
LIME erklärt **eine** Vorhersage, indem es das komplexe Modell $f$ in der **Umgebung** des zu erklärenden Punktes $x_0$ durch ein **einfaches, interpretierbares Modell** $g$ (oft linear) approximiert; die Koeffizienten von $g$ sind die lokale Erklärung (→ [[q-vl06-xai-lokal-fairness|VL6]], F. 3).

## Intuition
Eine kurvige Funktion sieht aus der Nähe fast linear aus. Man stört den Punkt leicht, fragt die Blackbox nach den neuen Vorhersagen und legt eine gewichtete Gerade durch diese Punkte – nahe Punkte zählen mehr.

## Formal

### Algorithmus (→ VL6, F. 3)
1. Datenpunkt $x_0$ wählen, der erklärt werden soll.
2. **Gestörte Stichproben** um $x_0$ im Merkmalsraum erzeugen.
3. Das Blackbox-Modell $f$ auf den gestörten Stichproben auswerten.
4. Stichproben mit einem **Kern** nach ihrer Nähe zu $x_0$ gewichten.
5. Ein simples, interpretierbares Modell $g$ anpassen (oft linear).
6. Koeffizienten von $g$ als lokale Erklärung verwenden.

### Zielfunktion (→ VL6, F. 4–5)
$$F(x_0) = \arg\min_{g\in G}\ \mathcal L(f, g, \pi_{x_0}) + \Omega(g), \qquad \mathcal L(f, g, \pi_{x_0}) = \sum_{j}\pi_{x_0}(x_j)\,\big(f(x_j) - g(x_j)\big)^2$$
- $f$: komplexes Modell; $g$: simples Modell aus einer Familie interpretierbarer Modelle $G$
- $\pi_{x_0}$: definiert die lokale Nachbarschaft (Abstandsmaß/Kern)
- $\Omega$: Regularisierungsfunktional (Komplexität von $g$)
- Die neuen Punkte $x_1, \dots, x_P$ bekommen die **Outputs des komplexen Modells** als Labels.

> [!info] Ergänzung (nicht aus der VL)
> Üblicher Kern: $\pi_{x_0}(x) = \exp\big(-d(x, x_0)^2/\sigma^2\big)$ mit Kernbreite $\sigma$ – so auch in [[q-nb07-local-xai|NB7]] (Kernbreite 3, gewichtete Ridge-Regression als $g$, also $\Omega$ = L2).

### Stellschrauben und Diagnostik (→ VL6, F. 6)
- **Nachbarschaft:** euklidischer, Kosinus- oder domänenspezifischer Abstand beeinflusst die Erklärung.
- **Kernbreite:** zu schmal → hohe Varianz; zu breit → Lokalität geht verloren.
- **Störungsstrategie** muss Merkmalstypen und Abhängigkeiten berücksichtigen; unrealistische Stichproben senken die Genauigkeit.
- **Komplexität von $g$** beeinflusst die Erklärbarkeit und kann relevante Wechselwirkungen verschleiern.
- **Diagnostik:** lokales R² oder Genauigkeit, Stabilität über Startwerte, Sensitivität gegenüber Merkmalsmengen, Vergleich mit ähnlichen Fällen.

## Annahmen & Voraussetzungen
- $f$ ist in der gewählten Nachbarschaft durch $g$ gut approximierbar (Fidelity, prüfbar über lokales R²).

## Beispiel ([[q-nb07-local-xai|NB7]])
Für typischen Fall, Grenzfall und Fehlklassifikation werden je 1 200 Nachbarn erzeugt (numerisch: Normalrauschen mit halber Standardabweichung, auf 1–99-%-Quantile begrenzt; kategorial: mit 75 % Originalwert behalten), mit dem Gradient-Boosting-Modell bewertet und mit gewichteter Ridge-Regression approximiert. Berichtet werden Top-Koeffizienten und lokales R².

## Typische Prüfungsfragen & Fallstricke
- Sechs Schritte des Algorithmus.
- Zielfunktion erklären (jedes Symbol).
- „Was passiert bei zu kleiner/zu großer Kernbreite?"
- Fallstrick: LIME-Koeffizienten als **globale** Koeffizienten lesen.
- Fallstrick: unrealistische Störungen bei korrelierten oder kategorialen Merkmalen.

## Verwandt
- [[shap]] – additive Attribution mit Spieltheorie
- [[lime-vs-shap]] – Gegenüberstellung
- [[kontrafaktische-erklaerungen]] – andere lokale Frage
- [[transparente-modelle]] – $g$ ist ein transparentes Modell
- [[ridge-regression]] – typisches $\Omega$

## Karteikarten
Grundidee von LIME?::Das komplexe Modell lokal um $x_0$ durch ein simples interpretierbares Modell approximieren; dessen Koeffizienten sind die Erklärung.
Sechs Schritte von LIME?::Punkt wählen, Störungen erzeugen, Blackbox auswerten, nach Nähe gewichten, simples Modell fitten, Koeffizienten als Erklärung.
LIME-Zielfunktion?::$F(x_0)=\arg\min_{g\in G}\mathcal L(f,g,\pi_{x_0})+\Omega(g)$ mit $\mathcal L=\sum\pi_{x_0}(x_j)(f(x_j)-g(x_j))^2$.
Was bewirkt eine zu schmale bzw. zu breite Kernbreite?::Zu schmal: hohe Varianz; zu breit: Lokalität geht verloren.
Wie prüft man die Güte einer LIME-Erklärung?::Lokales R²/Genauigkeit, Stabilität über Seeds, Sensitivität gegenüber Merkmalsmengen, Vergleich mit ähnlichen Fällen.
Welche Labels bekommt das Surrogat?::Die Vorhersagen des komplexen Modells auf den gestörten Punkten.

## Quellen
- [[q-vl06-xai-lokal-fairness|VL6]], Folien 1–6, 13
- [[q-nb07-local-xai|NB7]]
