---
layout: post
title: "SK Bridge Collaboration"
tags: SK
description: "Extended SK collaboration use case using Bridge and AG Client."
lang: de_DE
---

# SKK Brückenkollaboration

First read about [Base Collaboration](sk-base-collaboration.html).

( The Bridge collaboration topic is an older topic and there has been an Englisch version as well,
but in the new blog setup I will maintain one version only, in this case the German version. )

## Einleitung

Die SKK Brückenkollaboration mit Anschluss eines Silverlight Client
unter Verwendung einer Bridge ist eine aktuelle Anwendung im Jahr 2010.

## Desktopanwendung

Die im Bild gezeigte Anwendung hat einen internen Inputclient (Scrollbars) und einen internen Output Client (Grafik).
Es können auch externe Input- und Output Clienten angeschlossen werden,
so wie bei der [Basiskollaboration](sk-base-collaboration.html).

![SKK62 screenshot](images/SK/SKK62.png)

Zur Synchronisation von Daten zwischen gleichen oder kompatiblen Anwendungen
enthält die Applikation SKK62 die Funktion einer Client Bridge und einer Server Bridge.
Server Bridge und Client Bridge werden aber niemals gleichzeitig aktiviert.
Die Server Bridge existiert auch als eigenständige Anwendung,
aber die Einbettung hat sich als praktisch erwiesen.

Der Einbau der Server Bridge sowie des internen Inputs und Outputs in die Anwendung SKK62 erleichtert das Testen des Bridge Konzeptes,
da nicht so viele Einzelanwendungen gestartet werden müssen.

## Kollaboration

Nachdem eine Instanz der Anwendung als Bridge Server auf einem Rechner im Netzwerk gestartet wurde
können beliebig viele gleiche oder kompatible Instanzen als Client eine Verbindung zum Server aufbauen.

Als kompatible Anwendungen gelten die verschiedenen Implementierung von SKK62.
Es gibt zur Zeit Implementierungen in Win32, Java, .Net und Silverlight.
Die Silverlight Variante ist immer ein Bridgeclient.

![Brückenkollaboration](images/SK/Collab-Bridge-01.png)

Stellen Sie sich SKK62 als Blackbox vor mit folgenden Netzwerkverbindungspunkten
Input links, Output rechts, Server Bridge unten und Client Bridge oben.
Wenn zwei gleiche Anwendungen zusammenwirken,
dann sind sie vertikal angeordnet.
Die Sensorsignale kommen jeweils von der linken, externen oder internen Verbindung.

## Die Silverlight Variante

Nach Start der Desktop Anwendung als Bridge Server auf einer Maschine kann man mit dem Browser zur Desktop Anwendung surfen
und die zurück gelieferte Rich-Internet-Applikation mittels Plugin (Bridge Client) anschließen.
So lässt sich nicht nur ein Funktionstest aufbauen,
auch für den Praxisbetrieb ist dies eine interessante Betriebsart.

Die Verbindung zum Bridge Server (Desktopanwendung) wird mit der Operationen Plugin hergestellt
und mit der Operation Plugout wieder geschlossen.
Plugin und Plugout sind Bridgeoperationen.
(Mit diesen Bezeichnung wird der Unterschied zur Netzwerkverbindung über den Input- oder Output Socket deutlich gemacht,
wo die Operationen mit Connect und Disconnect bezeichnet werden.)

![SKIA03 screenshot](images/SK/SKIA03.png)

Die im Bild gezeigte Variante SKIA03 dient nur zur Anzeige.
Die anderen Implementierungen SKIA01 und SKIA02 besitzen interne Scroll Bars und können Daten senden.

Im Gegensatz zu den Desktop Varianten wird die Silverlight Variante nicht manuell konfiguriert.
Die Silverlight Variante wird über Http ausgeliefert und dabei dynamisch konfiguriert.

## Sonstiges

Wenn SKK62 als Bridge Client konfiguriert wurde,
unter Verwendung konkreter Verbindungsparameter	zu einem Bridge Server,
dann möchte man diese Konfiguration	nicht ständig ändern,
nur um eine Instanz des Programms auf der gleichen Maschine im Bridgeservermodus zu starten.
Sie benutzen den Szenario Manager,
um eine Instanz von SKK62 mit festverdrahteten Einstellungen als Bridgeserver zu öffnen.

In der Praxis müssen Störfaktoren berücksichtigt werden.
Standardmäßig wird ein Sicherheitskonzept immer über die Anwendung *drübergelegt*.
Dadurch behalten Sie in Fragen der Sicherheit die Kontrolle.

Die Lösungen, die mit der Beispielanwendung SKK bereitgestellt werden,
werden auch in der Produktfamilie FR verwendet.

## Zusammenfassung

Stellen Sie sich vor, dass Sie eine Bridgeserverinstanz von SKK62 öffnen,
über das Netzwerk einen	Sensor anschließen,
in einem Browser die Silverlight Variante öffnen und diese mit dem Server verbinden.
Dann können Sie (weltweit) vom Browser aus sehen, was der Sensor macht.

[Tagged in SK](tag/SK.html)