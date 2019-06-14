---
layout: post
title: "SK Nachnutzung"
tags: SK
description: "Just in case you wanted to take advantage of the work."
lang: de_DE
---

# Labor

Old topic, which I present as part of the history.
It has been written by the original developer himself, so it must be true!

## Willkommen im Bereich Labor

Anwender in Industrie und Labor (Forschung/Entwicklung/Fertigung) können bei der Lösung folgender Aufgaben unterstützt werden:

- Bereitstellung von Sensordaten (Zahlen) als Datenstrom im Netz.
- Empfang und Verarbeitung des Datenstroms auf mehreren Plattformen.
- Einbindung existierender Berechnungsmodule.
- Visualisierung im Browser (aktiv und passiv).

Die Grundlage für dieses Angebot besteht in der **Nachnutzung** der für das Referenzprodukt entwickelten Lösungen.

Im [Blog](sk-sensor-kit.html) finden Sie einen Eintrag,
der als Diskussionsgrundlage für die Realisierung einer Visualisierung von Sensorsignalen im Browser dienen kann.

Die Verwendung dieser Lösungsvariante sollte in Betracht gezogen werden,
wenn auf der Clientseite umfangreiche Berechnungen ergänzt werden müssen
und die Datenübertragung in Echtzeit unter Minimierung der Menge der zu übertragenden Daten erfolgen soll.

# Nachnutzung

Die Nachnutzung der für die Referenzanwendung entwickelten Lösungen könnte ein Schwerpunkt für RiggVar Software werden.

Einige Merkmale der Referenzanwendung:

- Textbasiertes Protokoll mit DSL (domain specific language)
- Verbindungsorientiert: TCP
- Eigene MOM eingebettet (message oriented middleware)
- Eigene Datenbindung für Grid
- Robuste Validierung
- Dreifachimplementierung: C#, Delphi, Java
- Serveranwendung mit zuschaltbarem Bridge Server, Policy Server, Web Server
- RIA Komponenten für statische und dynamische Daten
- Konfigurationsfreie Varianten.

Die wichtigsten Komponenten:

- Delphi Result Server mit embedded IdHttpServer
- Java Result Server mit embedded Jetty
- Browserbasierter Timing Client (Ajax)
- Browserbasierte Reports (Html)
- Silverlight RIA für die Anzeige von Livedaten
- Webapplikation auf Basis ASP.NET MVC
- Silverlight RIA für die Anzeige von archivierten Daten

Der Einsatz erfolgt:

- auf USB Stick (abgerüstete Variante)
- auf einer Maschine in Ihrem Intranet
- auf Windows Home Server
- direkt auf der Root Partition von Windows Hyper-V Server
- auf einem im Web sichtbaren VPS

Außerdem ist normalerweise folgendes gültig:

- Minimierung der Verwendung von 3rd-Party Komponenten.
- Damit können immer die aktuellen Entwicklungsumgebungen verwendet werden.
- Die Serveranwendung kann eine grafische Oberflächen haben.
- Die Serveranwendung ist damit zugleich der beste Client.
- Mehrere Serveranwendungen können vernetzt werden.
  Das ergibt sich durch den konsequent nachrichtenbasierten Systemaufbau.

## MsgCube Server

Das Produkt, der RiggVar MsgCube Server, ist für im Umfang kleine, überschaubare Anwendungen optimal geeignet.

### Schnittstellen

An der Oberfläche sind folgende Schnittstellen sichtbar:

- Ein Input Server, der die Daten empfängt
- Ein Output Server, der die Daten weitersendet
- Ein Web Server, der die Clientanwendung ausliefert
- Ein Bridge Server, der stehende Verbindungen zu den Clienten ermöglicht
- Ein Policy Server, der bestimmt, wer sich mit dem Server verbinden darf
- Eine Client Bridge, mit der zwei Serverknoten direkt verbunden werden können

### Interne Struktur

Die Anwendung und damit auch die Daten verbleiben während der gesamten Laufzeit im Speicher.
Dadurch reagiert der Server schnell auf eingehende Daten.
Es erfolgt eine robuste Validierung.
Gültige Daten werden unverändert weitergesendet.
Das Protokoll ist systemweit gültig.

### Vorteile

- Es gibt keine Lösung mit gleicher Funktionalität, die mit weniger Aufwand realisiert wurde.
- Der Server liegt in einer kompakten Form vor, die alle notwendigen Funktionen enthält.
- Der Server lässt sich sowohl lokal als auch in der Cloud betreiben.
- Eine konfigurationslose Variante ist verfügbar. Damit sind keine Vorkenntnisse zum Betrieb erforderlich.
- Sie benötigen keine zusätzlichen Lizenzen oder vertraglichen Bindungen.
- Die Clientkomponenten stehen auf allen gängigen Plattformen zur Verfügung.
- Das System unterstützt ein einfach zu verstehendes Protokoll, welches systemweit verwendet wird.
- Durch die Verwendung von Streaming Sockets erfolgt die Visualisierung des Prozesses live in Echtzeit.
- Beliebige kundenspezifische Berechnungen können auf Anfrage direkt im Datenendgerät implementiert werden.

[SK sensor kit](sk-sensor-kit.html)