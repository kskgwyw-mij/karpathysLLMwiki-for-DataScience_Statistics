---
typ: uebersicht
themen: [klausur]
---

# Dashboard

> Benötigt die Community-Plugins **Dataview** (mit aktiviertem *JavaScript Queries*) und optional **Spaced Repetition**.

```dataviewjs
const exam = new Date("2026-11-13T11:00:00");
const days = Math.ceil((exam - new Date()) / 86400000);
dv.header(2, days > 0 ? `${days} Tage bis zur Klausur` : "Klausurtag");
dv.paragraph("Fr, 13.11.2026, 11:00 Uhr");
```

## Stand nach Beherrschung

```dataview
TABLE WITHOUT ID beherrschung AS "Stand", length(rows) AS "Seiten"
FROM "wiki/konzepte" OR "wiki/methoden"
GROUP BY beherrschung
```

## Als Nächstes lernen
Hohe Relevanz zuerst, noch nicht sicher beherrscht.

```dataview
TABLE relevanz, beherrschung, length(quellen) AS "Quellen", aktualisiert
FROM "wiki/konzepte" OR "wiki/methoden"
WHERE beherrschung != "sicher"
SORT choice(relevanz = "hoch", 0, choice(relevanz = "mittel", 1, 2)) ASC, choice(beherrschung = "neu", 0, 1) ASC
```

## Quellen

```dataview
TABLE aktualisiert
FROM "wiki/quellen"
SORT file.name ASC
```

## Seiten ohne eingehende Links (Lint-Hinweis)

```dataview
LIST
FROM "wiki"
WHERE length(file.inlinks) = 0 AND file.name != "index" AND file.name != "log" AND file.name != "dashboard"
```

## Letzte Log-Einträge
Siehe [[log]].
