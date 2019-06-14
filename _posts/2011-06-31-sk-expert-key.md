---
layout: post
title: "Export Key Test"
tags: SK
description: "SK Einsatzbeispiel für die Anbindung eines Messgerätes"
lang: de_DE
---

# EK Test

Der [Sensorkit Artikel](sk-sensor-kit.html) beschreibt allgemein,
wie Messwerte über einen Server zum Browser übertragen und dort angezeigt werden können.

In diesem Artikel wird die konkrete Realisierung der Anbindung eines *Expert Key* Gerätes beschrieben.

Abweichend vom SK Beispiel soll neben der Anbindung eines konkreten Messgerätes auch das Protokoll der Anwendung angepasst werden,
so dass Telegramme der Form `A.B.Zn.Yn.Xn=Value` ausgetauscht werden.

**A.B** ist dabei der Namespace der Anwendung und mit **Zn.Yn.Xn** wird eine Speicherzelle im Raum ausgewählt.
Sie können sich die Achsen XYZ als Spalte, Zeile und Ebene vorstellen.
Das Protokoll kann verändert werden,
so dass es als domainspezifisches Protokoll auf die Terminologie Ihrer Anwendung passt.

```
AB Beispiel: A.B.Z1.Y1.X2 = 42
Schnitt-Kreis-Kreis Beispiel: SKK.Kreise.W1.Bib1.R2 = 42
Ihre Anwendung: Firma.Anwendung.Blatt1.Zeile1.Spalte2 = 42
```

## Gerät und DataService

Das Expert Key Gerät wird über das Netzwerk oder USB an den PC angeschlossen.

![Screenshot vom Expert Key 100C Testgerät](images/ek/ab-01.png)

Dann werden mit dem DataService Konfigurator die Kanäle konfiguriert.
Im Beispiel sollen zwei analoge Kanäle beobachtet und die gemessenen Werte im Browser sichtbar gemacht werden.
Im einfachsten Fall sind die Testkanäle mit Sinus- und Rechtecksignal ausreichend.

![Screenshot vom DataService Konfigurator](images/ek/ab-07.png)

Die Signale können von beliebigen Sensoren stammen, die am Gerät angeschlossen sind.

Für reale Projekte werden später die Messbereiche entsprechend den Anforderungen eingestellt
und die Validierung der Telegramme angepasst.
Stellen Sie für den Test das Mapping so ein,
dass Sie das Anbindungsbeispiel sofort nutzen können.

Das Programm bezieht die Eingangswerte (Value) unter Verwendung der API vom DataService,
einer Softwarekomponente die mit dem Gerät geliefert wird.

## Programm

Das Programm liegt als Adapter zwischen dem Delphin DataService und dem RiggVar Sensorkit Server,
hier vorliegend als AB Server,
der mit dem neu definierten Protokoll für die Beispielanwendung AB arbeitet.

![Screenshot von AB12, the Data Service Client](images/ek/ab-02.png)

Das Programm kann sich mit beiden Partnern über das Netzwerk verbinden.
Es übernimmt in beiden Fällen die Rolle des Clienten,
der die Verbindung zu den hörenden (listening) Sockets herstellt.

### Verbindung zum DataService

Die Verbindung zum DataService konfiguriert eine real time subscription für einen Kanal oder mehrere Kanäle,
hier im Beispiel zwei Kanäle:

```
X2: Analoger Eingang, zum Beispiel Sinus Testkanal.
X3: Analoger Eingang, zum Beispiel Rechteck Testkanal.
```

Für die Wertebereiche sollte im DataService folgendes eingestellt werden:

```
10 <= X2 <= 100
 0 <= X3 <= 300
```

Die Wahl von X2 und X3 hängt mit der Beispielberechnung zusammen, die vom Kreis Beispiel abgeleitet wurde.
X3 entspricht dem Mittelpunkt des Kreises 1 (M1X) und X2 entspricht dem Radius des Kreises 2 (R2).

So nehmen Sie die Eingangsseite mit dem DataService in Betrieb:

- Sensoren anschließen und Gerät einschalten
- DataService starten und Kanäle konfigurieren
- DataService Client starten und Verbindung herstellen
- Es werden die aktiven Kanäle aufgelistet
- Jetzt können die gewünschten Kanäle in den Comboboxen ausgewählt werden
- mit button Read wird der kontinuierliche Lesevorgang gestartet

Die eigentliche Verbindungsaufnahme ist ein zweistufiger Prozess.
Zuerst werden die verfügbaren Kanäle aufgelistet,
dann wird das fortlaufende Auslesen der Werte gestartet.

### Verbindung zum Sensorkit Server

Die Verbindungsaufnahme zum Sensorkit Server ist auch ein zweistufiger Prozess,
zuerst in der Combobox den Server auswählen, dann die Verbindung öffnen.

- Server auswählen mit Combobox
- Auswahl mit Button aktivieren
- Verbinden mit Connect
- (Verbindung schließen mit Disconnect)
- Free drücken, um einen anderen Server auszuwählen

Das Beispielprogramm kommt ohne gespeicherte Konfiguration aus.
Es kann zwischen lokalem Server und dem Server im Internet gewählt werden.
Die entsprechenden Verbindungsparameter sind im Programm hinterlegt.
Um eine lokale Instanz des Servers zu benutzen müssen Sie den Server selbst starten.

Änderungen der gemessenen Werte werden über die Verbindung zum Sensorkit Server als Telegramm weitergesendet.
Dabei wird das für die Anwendung AB definierte Protokoll verwendet.

Außerdem wird sichergestellt, dass die Momentanwerte für die Signale X2 und X3 mit einer garantierten Pause gesendet werden.
Die erzwungene Pause kann im Beispiel zwischen 100 ms und 10 Sekunden in Stufen eingestellt werden,
analog zur Abtastrate für die analogen Eingänge des Gerätes.
Je nachdem, ob sich der Server im lokalen Netz oder im Internet befindet kann damit der geeignete Betriebsmodus eingestellt werden.

Kurzzeitig können Sie die Senderate hochschrauben, um zu sehen,
wie leistungsfähig die Netzwerkverbindung bzw. deren schwächste Stelle ist.
Für den unbeaufsichtigten Dauerbetrieb sollten sinnvolle Werte gewählt werden.

## Test

Stellen Sie für den Test sicher, dass sich die Signale verändern, so dass man was sehen kann.
Die DataService Testsignale verändern sich automatisch.
Es bietet sich an, ein Potentiometer an das Gerät anzuschließen,
der Konstantstrom dafür wird vom Gerät an speziellen Ausgängen bereitgestellt.

Nachdem Sie vom Programm aus die beiden Verbindungen zum DataService und zum Sensorkit Server hergestellt haben
können Sie mit einem Browser zum Server browsen,
mit einem Link das Silverlight Applet laden,
sich vom Applet aus mit dem Server verbinden und die Veränderungen der Werte verfolgen.

Der Momentanwert von X2 kann in einem virtuellen Instrument dargestellt werden.

![Screenshot vom virtuellen Instrument](images/ek/ab-03.png)

Eine einfache Trendgrafik stellt den zeitlichen Verlauf von X2 dar.

![Screenshot der selbstgemachten, einfachen Trendgrafik](images/ek/ab-09.png)

Die Grafik wird später bei realen Projekten an die Anforderungen angepasst.
Je nach Bedarf wird das Diagramm durch eine Drittparteikomponente ersetzt.

![Screenshot der Grafik mit Drittpartei-Komponente](images/ek/ab-16.png)

## Zusammenfassung

Um ein konkretes Projekt zu realisieren benötigen Sie:

- eine Idee/Aufgabenstellung mit Berechnung und Visualisierung
- ein Expert Key oder vergleichbares Gerät für die Datenerfassung
- einen PC mit dem DataService (lizensiert und aktiviert)
- das DataService Client Programm, welches die DLL (API) benutzt und Daten weitersendet
- den Server, der die Daten verteilt
- den Silverlight Client, der Berechnungen durchführt und die Daten anzeigt
- einen Browser mit Unterstützung für Silverlight
- einen aktuellen PC, wo der Browser läuft
- zwei schnelle Breitbandzugänge zum Internet (optional)

Neben den Silverlight Clienten, die im Mittelpunkt des Artikels stehen, befinden sich Desktop Clienten im Angebot.

- Wenn Sie ein Expert Key Messgerät im Einsatz haben können Sie das konkrete Einsatzbeispiel direkt nachnutzen.
- Der AB Server läuft bei Bedarf im Internet, so dass es möglich ist Testdaten zu senden und zu beobachten.
- Teilen Sie mir Ihre Anforderungen mit.

[sk msg example](sk-msg-example.html)