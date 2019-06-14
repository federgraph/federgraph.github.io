---
layout: post
title: "SK Screenshots"
tags: SK
description: "Screenshots of SKK Test setup with explanation"
---

# Historic Screenshots of SKK apps

## Data sender

The data sender in the picture below is a simulator used for testing.
It should be replaced with a real data sender.

Messages are generated and sent when you scroll the bars.

With On / Off you can change the mode of operation,
so that a message is generated either
- at every **MouseMove** event,
- or only at the **EndDrag** event.

In the On state there will be more *music* on the wire!

![SK07](images/SK/SK-Input.png)

## Server

The server can be a normal desktop application.

![SK04](images/SK/SK-Server-04.png)

Or almost a normal Delphi application.

It does not show any modal dialog windows.
When you have a modal dialog open,
the message loop of your app behaves differently
and processing of messages received via sockets can stop.

This application will always process network traffic as it happens,
it is both a server AND a client.

## Server without UI

SK06 removes most of the graphical UI.
When it starts up it will write to the memo control once, so that you can see a listing of open ports.

![SK06](images/SK/SK-Server-06.png)

## Web

The app has a built in web site, which could / can serve Silverlight clients.

![SK Web](images/SK/SK-Web.png)

( In the example image above the server is running locally. )

## Viewer

The viewer can visualize data.

- Use the **Plugin** button to connect manually.
- If you press the Sound button before you connect you will receive audio feedback if the operation succeeds.
- The client will be configured by the server so no mistake can be made.
- The connection is expected to succeed.

![SK Viewer](images/SK/SK-Viewer.png)

The intersections of the two circles are computed on the client side.

The computation is an example, the example that I am always using.

## Editor

The Silverlight client (Editor) can send data back to the server.
It replaces the desktop simulator.

![SK Editor](images/SK/SK-Editor.png)

Note that the positions of the input elements do not auto update,
as it would not make sense.

## Plotter

The client can be enhanced to be a plotter / logger.

Here in the example the update of the graph happens
- each time a messages arrives (Off mode) or
- in sampled mode at fixed time intervals (On mode).

![SK Plotter](images/SK/SK-Plotter.png)

- In the example changes in parameter R2 of line 1 are shown in upper curve.
- The lower curve is hooked to an internal simulation.

Therefore the lower curve keeps moving even if the current measured value of R2 is constant and appears in the diagram as a straight line.

## Gauge

The virtual instrument in the picture is a freely available Silverlight control.

![SK Gauge](images/SK/SK-Gauge.png)

## Tabbed client

The application shown in the image uses tabs and was used during debugging.

> Almost the entire SKK application has been ported to the Silverlight platform.

![SK Thin Client 01](images/SK/SK-Client-01.png)

- The count of rows / parameters or channels can be changed.
- The graphical display corresponds to the selected row.
- The user can change the selected row with F4 key.

## A fat client

In this variation everything remains in view,
including the memo with the log of received messages.

![SK Fat Client 02](images/SK/SK-Client-02.png)

The design of the UI is a task for the designer, they say.

***

Lots of things are possible.

[SK Base Collaboration](sk-base-collaboration.html)

The screenshots were up to date in June 2011.
