---
layout: post
title: "Live Geometry Beispiel"
tags: RG SK
description: "mit Daten für RG und SK"
lang: de
---

# Live Geometry

Mit [Live Geometry](https://github.com/KirillOsenkov/LiveGeometry) können geometrische Konstruktionen untersucht werden.
Die Beispiele enthalten Konstruktionsvorschriften (mit einigen konkreten Abmessungen) in einer Xml Datei.

Ein interessantes Projekt, leider auch *nur* ein Silverlight Projekt, aber das kann sich ja noch ändern!

Nachfolgend die *Dokumentation* der eigenen Beispiele:

*screenshot of example rgg 02*<br>
![Screenshot of drawing after rgg02.xml was loaded into Live-Geometry Silverlight-application.](images/SK/dg-rgg-02.png)

- DG-Kurbelschwinge.xml
- DG-rgg01.xml
- DG-rgg02.xml
- DG-skk01.xml

Sie können die Live Geometry Silverlight Applikation auch außerhalb des Browsers installieren, siehe Bild.

![skk01.xml loaded into Live-Geometry, screenshot](images/SK/dg-skk-01.png)

Am einfachsten könnend die Beispiele geladen werden,
wenn Sie das xml in einer Datei auf der Festplatte speichern und dann mit Drawing/Open (und Dateifilter \*.\*) öffnen.

Im Internet Explorer (IE 11) konnte / kann **Live Geometry** von livegeometry.com im Vollbildmodus geladen werden.

## DG-skk01.xml

```xml
<Drawing>
    <Figures>
        <FreePoint Name="FreePoint339" X="-11.9791666667" Y="7.9583333333"/>
        <FreePoint Name="FreePoint340" X="-8.0208333333" Y="8"/>
        <Segment Name="Segment342">
            <Dependency Name="FreePoint339"/>
            <Dependency Name="FreePoint340"/>
        </Segment>
        <PointOnFigure Name="R1" X="-10.7312786209548" Y="7.97146899694971"
                Parameter="0.315255927340844">
            <Dependency Name="Segment342"/>
        </PointOnFigure>
        <FreePoint Name="FreePoint351" X="-6.8958333333" Y="2.9375000001"/>
        <FreePoint Name="FreePoint352" X="-12" Y="7"/>
        <FreePoint Name="FreePoint353" X="-8" Y="7"/>
        <Segment Name="Segment355">
            <Dependency Name="FreePoint352"/>
            <Dependency Name="FreePoint353"/>
        </Segment>
        <PointOnFigure Name="R2" X="-9.8958333333" Y="7"
                Parameter="0.526041666675">
            <Dependency Name="Segment355"/>
        </PointOnFigure>
        <FreePoint Name="FreePoint361" X="-4.2083333333" Y="4.2083333334"/>
        <CircleByRadius Name="K2">
            <Dependency Name="FreePoint352"/>
            <Dependency Name="R2"/>
            <Dependency Name="FreePoint361"/>
        </CircleByRadius>
        <CircleByRadius Name="K1">
            <Dependency Name="FreePoint339"/>
            <Dependency Name="R1"/>
            <Dependency Name="FreePoint351"/>
        </CircleByRadius>
        <IntersectionPoint Name="S1" Algorithm="IntersectCircleAndCircle1">
            <Dependency Name="K2"/>
            <Dependency Name="K1"/>
        </IntersectionPoint>
        <IntersectionPoint Name="S2" Algorithm="IntersectCircleAndCircle2">
            <Dependency Name="K2"/>
            <Dependency Name="K1"/>
        </IntersectionPoint>
        <Label Name="Title" Text="SKK01" X="-6.1458333334" Y="8.6666666667"/>
    </Figures>
</Drawing>
```

## DG-rgg02.xml

```xml
<Drawing>
    <Figures>
        <FreePoint Name="FreePoint346" X="-12.1249999998" Y="-2.8541666667"/>
        <FreePoint Name="FreePoint349" X="-7.7083333334" Y="-3.0000000001"/>
        <FreePoint Name="FreePoint355" X="-11.2083333332" Y="-4.8958333332"/>
        <Segment Name="Segment354">
            <Dependency Name="FreePoint355"/>
            <Dependency Name="FreePoint346"/>
        </Segment>
        <Segment Name="Segment353">
            <Dependency Name="FreePoint349"/>
            <Dependency Name="FreePoint355"/>
        </Segment>
        <IntersectionPoint Name="IntersectionPoint396"
                Algorithm="IntersectLineAndLine">
            <Dependency Name="Segment354"/>
            <Dependency Name="Segment353"/>
        </IntersectionPoint>
        <FreePoint Name="FreePoint413" X="-1.0416666665" Y="-5.4375"/>
        <CircleByRadius Name="CircleByRadius417">
            <Dependency Name="IntersectionPoint396"/>
            <Dependency Name="FreePoint346"/>
            <Dependency Name="FreePoint413"/>
        </CircleByRadius>
        <PointOnFigure Name="PointOnFigure424"
                X="-1.86137034972015" Y="-3.3550095614942"
                Parameter="1.94578805391933">
            <Dependency Name="CircleByRadius417"/>
        </PointOnFigure>
        <CircleByRadius Name="CircleByRadius443">
            <Dependency Name="IntersectionPoint396"/>
            <Dependency Name="FreePoint349"/>
            <Dependency Name="FreePoint413"/>
        </CircleByRadius>
        <CircleByRadius Name="CircleByRadius428">
            <Dependency Name="FreePoint346"/>
            <Dependency Name="FreePoint349"/>
            <Dependency Name="PointOnFigure424"/>
        </CircleByRadius>
        <IntersectionPoint Name="IntersectionPoint444" Algorithm="IntersectCircleAndCircle1">
            <Dependency Name="CircleByRadius443"/>
            <Dependency Name="CircleByRadius428"/>
        </IntersectionPoint>
        <FreePoint Name="FreePoint510" X="-12.0624999998" Y="6.2708333333"/>
        <FreePoint Name="FreePoint513" X="-7.4791666664" Y="-2.875"/>
        <FreePoint Name="FreePoint607" X="-12.4583333331" Y="-2.375"/>
        <FreePoint Name="FreePoint610" X="-13.1874999998" Y="6.875"/>
        <Segment Name="Segment609">
            <Dependency Name="FreePoint607"/>
            <Dependency Name="FreePoint610"/>
        </Segment>
        <PointOnFigure Name="PointOnFigure611" X="-12.9654771343346" Y="4.05848136393958"
                Parameter="0.695511498804279">
            <Dependency Name="Segment609"/>
        </PointOnFigure>
        <FreePoint Name="FreePoint619" X="-11.4583333331" Y="2.6041666667"/>
        <FreePoint Name="FreePoint636" X="-11.1249999998" Y="-4.625"/>
        <CircleByRadius Name="CircleByRadius644">
            <Dependency Name="FreePoint607"/>
            <Dependency Name="FreePoint610"/>
            <Dependency Name="PointOnFigure424"/>
        </CircleByRadius>
        <CircleByRadius Name="CircleByRadius640">
            <Dependency Name="FreePoint513"/>
            <Dependency Name="FreePoint510"/>
            <Dependency Name="IntersectionPoint444"/>
        </CircleByRadius>
        <IntersectionPoint Name="IntersectionPoint645" Algorithm="IntersectCircleAndCircle2">
            <Dependency Name="CircleByRadius644"/>
            <Dependency Name="CircleByRadius640"/>
        </IntersectionPoint>
        <CircleByRadius Name="CircleByRadius653">
            <Dependency Name="FreePoint636"/>
            <Dependency Name="FreePoint619"/>
            <Dependency Name="FreePoint413"/>
        </CircleByRadius>
        <CircleByRadius Name="CircleByRadius649">
            <Dependency Name="FreePoint610"/>
            <Dependency Name="FreePoint619"/>
            <Dependency Name="IntersectionPoint645"/>
        </CircleByRadius>
        <IntersectionPoint Name="IntersectionPoint654" Algorithm="IntersectCircleAndCircle1">
            <Dependency Name="CircleByRadius653"/>
            <Dependency Name="CircleByRadius649"/>
        </IntersectionPoint>
        <CircleByRadius Name="CircleByRadius664">
            <Dependency Name="FreePoint610"/>
            <Dependency Name="PointOnFigure611"/>
            <Dependency Name="IntersectionPoint645"/>
        </CircleByRadius>
        <CircleByRadius Name="CircleByRadius660">
            <Dependency Name="FreePoint619"/>
            <Dependency Name="PointOnFigure611"/>
            <Dependency Name="IntersectionPoint654"/>
        </CircleByRadius>
        <IntersectionPoint Name="IntersectionPoint673" Algorithm="IntersectCircleAndCircle1">
            <Dependency Name="CircleByRadius664"/>
            <Dependency Name="CircleByRadius660"/>
        </IntersectionPoint>
        <Polygon Name="Polygon351">
            <Dependency Name="FreePoint346"/>
            <Dependency Name="FreePoint349"/>
            <Dependency Name="FreePoint355"/>
        </Polygon>
        <Segment Name="Segment352">
            <Dependency Name="FreePoint346"/>
            <Dependency Name="FreePoint349"/>
        </Segment>
        <Polygon Name="Polygon448">
            <Dependency Name="FreePoint413"/>
            <Dependency Name="PointOnFigure424"/>
            <Dependency Name="IntersectionPoint444"/>
        </Polygon>
        <Segment Name="Segment449">
            <Dependency Name="FreePoint413"/>
            <Dependency Name="PointOnFigure424"/>
        </Segment>
        <Segment Name="Segment450">
            <Dependency Name="PointOnFigure424"/>
            <Dependency Name="IntersectionPoint444"/>
        </Segment>
        <Segment Name="Segment451">
            <Dependency Name="IntersectionPoint444"/>
            <Dependency Name="FreePoint413"/>
        </Segment>
        <Segment Name="Segment512">
            <Dependency Name="FreePoint510"/>
            <Dependency Name="FreePoint513"/>
        </Segment>
        <Polygon Name="Polygon615">
            <Dependency Name="FreePoint610"/>
            <Dependency Name="PointOnFigure611"/>
            <Dependency Name="FreePoint619"/>
        </Polygon>
        <Segment Name="Segment616">
            <Dependency Name="FreePoint610"/>
            <Dependency Name="PointOnFigure611"/>
        </Segment>
        <Segment Name="Segment617">
            <Dependency Name="PointOnFigure611"/>
            <Dependency Name="FreePoint619"/>
        </Segment>
        <Segment Name="Segment618">
            <Dependency Name="FreePoint619"/>
            <Dependency Name="FreePoint610"/>
        </Segment>
        <Segment Name="Segment635">
            <Dependency Name="FreePoint619"/>
            <Dependency Name="FreePoint636"/>
        </Segment>
        <Segment Name="Segment656">
            <Dependency Name="FreePoint413"/>
            <Dependency Name="IntersectionPoint654"/>
        </Segment>
        <Polygon Name="Polygon677">
            <Dependency Name="IntersectionPoint645"/>
            <Dependency Name="IntersectionPoint673"/>
            <Dependency Name="IntersectionPoint654"/>
        </Polygon>
        <Segment Name="Segment678">
            <Dependency Name="IntersectionPoint645"/>
            <Dependency Name="IntersectionPoint673"/>
        </Segment>
        <Segment Name="Segment679">
            <Dependency Name="IntersectionPoint673"/>
            <Dependency Name="IntersectionPoint654"/>
        </Segment>
        <Segment Name="Segment680">
            <Dependency Name="IntersectionPoint654"/>
            <Dependency Name="IntersectionPoint645"/>
        </Segment>
    </Figures>
</Drawing>
```

[SK Elektronischer Zirkel](sk-zirkel.html)