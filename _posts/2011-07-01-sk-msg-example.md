---
layout: post
title: "SK Message Example"
tags: SK
description: "Das SK Nachrichten Beispiel."
lang: de_DE
---

# Nachrichtenbeispiel

## Beispielanwendung SKK

### Beschreibung

Als Beispiel dient die Referenzanwendung SKK (Schnitt-Kreis-Kreis):

- Das Modell besteht aus zwei Kreisen, die durch Ihre Parameter Mittelpunkt und Radius gegeben sind.
- Die Kernanwendung berechnet die Schnittpunkte, sofern vorhanden.
- Eingangsseitig können die Parameter mit Hilfe von Scroll Bars verändert werden.
- Ausgangsseitig wird eine Grafik gezeichnet, in der die Schnittpunkte gekennzeichnet sind.
- Die Anwendung eignet sich hervorragend als Beispielanwendung für das Framework.
- Die Geschwindigkeit der Datenübermittlung kann durch Beobachten der Grafik sehr einfach sichtbar gemacht werden.

### Kernanwendung

- Elektronischer Zirkel.
- Berechnet die Schnittpunkte zweier Kreise.
- Unterscheidet systematisch die verschiedenen Fälle bezüglich der relative Lage.
- Wurde portiert von Delphi nach C# und weiter nach Java.

### Parameter

- Anzahl der Zeilen

### Sensoren

- Als Sensoren/Zeilenparameter gelten bestimmte Felder einer Zeile.
- Im Beispiel die Mittelpunkte und Radien, insgesamt sechs Parameter pro Zeile.

## Beschreibung der Nachrichten

### Einzeilige Message

Die Daten der Beispielanwendung SKK liegen in der Regel im Textformat vor, als Liste von einzeiligen Nachrichten.

Die einzeiligen Nachrichten entsprechen dem Key=Value Muster.

So ist es gedacht (Indizes in Klammern):

```
SKK.Kreise.W[1].Bib[1].M1X=50
```

Real müssen die Klammern weggelassen werden (spart Platz und lässt sich einfacher schreiben):

```
SKK.Kreise.W1.Bib1.M1X=50
```

- Links vom Gleichheitszeichen steht der Key.
- SKK kennzeichnet die Anwendung (den Typ)
- Im zweiten Abschnitt steht Kreise für die Kategorie.
- W1 kennzeichnet die Tabelle 1 (immer 1 bei SKK)
- Bib1 kennzeichnet die Zeile in der Tabelle
- M1X bedeutet, dass ein Wert für Parameter M1X (Mittelpunkt 1, X-Koordinate) gesendet wird.
- 50 rechts vom Gleichheitszeichen ist der Value.

### Protokoll

Die einzeiligen Nachrichten entsprechen dem selbst definierten Protokoll.
Das Programm enthält einen robusten Parser für das Protokoll.

Der Key (links vom Gleichheitszeichen) ist hierarchisch aufgebaut.
Die einzelnen Ebenen werden immer durch Punkt getrennt.
Der Entwurf für das Protokoll kann übersichtlich in einer Tabelle dargestellt werden.
Die mit (X) endenden Einträge sind indizierte Token.

| Level1 | Level2 | Level3 | Level4 | Level5 | Description    |
| SKK    | Kreise | W1     | STL    | Count  | Anzahl Zeilen  |
|        |        | W(X)   | Bib(X) | XX     | Testmessage    |
|        |        |        |        | M1X    | Mittelpunkt1.x |
|        |        |        |        | M1Y    | Mittelpunkt1.y |
|        |        |        |        | M2X    | Mittelpunkt2.x |
|        |        |        |        | M2Y    | Mittelpunkt2.y |
|        |        |        |        | R1     | Radius1        |
|        |        |        |        | R2     | Radius2        |

STL initialisiert die Zeilen in Tabelle W bzw. in den Tabellen W(X).
Es wird nur eine Tabelle verwendet.
Es gibt aber andere Anwendungen des Frameworks, die mehrere Tabellen verwenden.

Wie in Level 4 definiert, können die für eine Zeile in W(X) bestimmten Nachrichten
auf die Bib (eindeutige Kennzeichnung der Zeile in Tabelle W) zugewiesen werden.

### Textformat

Das Textformat ist im Hauptteil eine Liste von einzeiligen Messages,
die so auch einzeln über das Netzwerk (oder eine interne Verbindung) an das Programm gesendet werden können.
Vorangestellt sind die Parameter und Properties für den Event, hier nur die Anzahl der reservierten Zeilen.

```
Event.StartlistCount = 2
SKK.Kreise.W1.STL.Count=2

SKK.Kreise.W1.Bib1.R1=58
SKK.Kreise.W1.Bib1.R2=58
SKK.Kreise.W1.Bib1.M1X=104
SKK.Kreise.W1.Bib1.M1Y=150
SKK.Kreise.W1.Bib1.M2X=146
SKK.Kreise.W1.Bib1.M2Y=82

SKK.Kreise.W1.Bib2.R1=41
SKK.Kreise.W1.Bib2.R2=100
SKK.Kreise.W1.Bib2.M1Y=120
```

[SK Msg Flow](sk-msg-flow.html)