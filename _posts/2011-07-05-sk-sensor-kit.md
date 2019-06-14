---
layout: post
title: "Product SK (Sensor Kit)"
tags: Delphi SK
description: "Watch sensor data in the Silverlight client."
---

# Sensor Kit

About visualization of sensor signals within the browser using a server and a Silverlight client.

> A recycled blog post, from about 2011.

This topic is old because of Silverlight, but there should be a good replacement for Silverlight,
if not now then in the future.
Using the Silverlight browser plugin I could open a tcp connection to the desktop application,
this was kind of cool.

## Intro

I have written a sports timing and result system, Fleetrace or FR.
This system contains reusable parts.
Project SK is a derived example project, which contains part of the FR framework.
Then, for display of measured data, I use SK to create customized products.

- In many cases it is not necessary to change the server in order to quickly adapt the system to new requirements.
- Sometimes it is sufficient to modify the Silverlight client, where the data is visualized.
- Often a *box product* can be used unchanged as a toolkit.

This blog post describes how SK can be used without modification for remote display of sensor signals.

## Test

The typical deployment scenario looks like this:

- Start up the server.
  When the server is started for the first time on a machine,
  Windows will prompt you to agree with default Firewall entries.
- Open a browser and surf to the website embedded in the server.
  Surf to one of the Silverlight applications.
  From within the Silverlight application, manually connect back to the server.
- Configure and open a data sender.
  This can be a simulator or a real application.
  Connect the data sender to the server.
- Watch any change of data in the browser.

Check out the demo video, the live test, or a series of [screenshots](sk-screenshots.html).
 
## Concept

> An easy to use product must have an easy to understand concept.

The combination of server and Silverlight client is at the core of the product.
In all cases a data sender in form of a sensor / probe and a partner program on the PC must be provided.

The concept is depicted in the following picture:

*server-sketch*<br>
![Memo Board Sketch of Sensor Kit Setup](images/SK/server-sketch.jpg)

- From the point of view of SK the probe / **Fühler** is an arbitrary sensor element.
- The **Program** is running on a PC and reading data from the sensor.
  Optionally the program will save measured and / or computed values to the **DB**,
  but the most important task - from SK's point of view,
  is the provision of measured data as a stream of messages on the network.
  SK in this case is used unmodified as a product,
  and the format and the protocol of all messages must conform to the pattern that has been specified for the SK system.
  This pattern is easy to understand and easy to implement within the program.
- Product SK consists of the **Server** and Silverlight clients.
  The server passes all valid input messages on, to all clients connected to output.
  Both, program and clients, connect via tcp to the server.
- There will be an html page with a link to the Silverlight **Client**.
  This link will initialize the download of the client, from the server to the browser.
  Once loaded, the client can connect back to the server and display measured data.
- The display of archived data from the database can be achieved via the ASP.NET **web application**,
  but real time data should always be routed through the server.

## Network connection points

The server application opens listening sockets so that clients can connect via the network to the server.

A typical usage of ports is given in the following table:

| Port | Usage | Purpose |
| TCP 943 | Policy server | Agreement to establish a connection (fixed given port number) |
| TCP 3427 | Input socket | data sender |
| TCP 3428 | Output socket | desktop clients, server cascade |
| TCP 4530 | Bridge | Silverlight clients (port in range 4502-4534) |
| TCP 8086 | Web | Deployment of Silverlight client, download of data |

Which ports the application is listening on can be seen in resource monitor.

*SK Open Ports*<br>
![Screenshot of resource monitor utility showing ports opened by SK server app](images/SK/sk-open-ports.png)

Bridge and Web are shown as using TCP protocol, instead of UDP protocol, but from the developer point of view,
Bridge and Web are using http, on top of TCP. Input and Output are using tcp directly,
meaning that framed plain text messages will be put on the wire.

## Configuration less Operation

The system can be configured for different situations.

For the special but important **product use case**,
a configuration free variation was derived, where all settings are hard coded.

You don't need to provide any settings and the application will behave in the same way every time you start it up.
The known and reproducible behavior is the wanted effect in the case of the product.

The configuration less operation refers to the server itself and the Silverlight client,
which is configured automatically by the server.

The data sender needs to set up / configure connection parameters host and port of the server
in order to connect.

The easy to use configuration free server product is assuming the simplest case
where only one instance of the server program is running on a machine.

If several instances are started up on the same machine, the ports need to be different,
and one server program must provide the policy server function for all server instances running on the machine.
In this case and in other special cases you should consult the developer.

## Conclusion

With configuration free example application SK,
a product has been created that can be used to get results quickly.

> The positive (optimistic) way of thinking makes it possible.

If it is working, then you know it and can use it.

( If it doesn't - let's pretend that Internet is currently not available -
plan A is coming into effect as always,
you need to physically go and see if everything is still OK at the setup of the equipment with the sensor. )

A customized solution can be developed based on the box product.

[SK Screenshots](sk-screenshots.html)