---
layout: post
title: "SK Base Collaboration"
tags: SK
description: "How the SK sample apps collaborate over the network."
---

# The Axis of Good

The message flow in the system is according to the main axis in the [message map](sk-msg-map.html),
therefore the name base collaboration.

```
// SK main axis, very simple
Input Client --> Server -->  Output Client
```

The SK base collaboration is as cool as in the year 2010.

![SK Base Collaboration](images/SK/SKK-060708.png)

Actually, it is from 2003, but I am using it all the time.
You cannot see it in the static image,
but the graph keeps moving if you change the value of input parameters using SKK07.

All three visible windows are the respective main window of three collaborating applications,
which can be located on separate machines.

The green *LED* shows that a network connection to the server SKK06 could be established.

Theoretically, an arbitrary number of input clients (here SKK07) or output clients (here SKK08) can be attached.

There are Win32, .Net and Java implementations.
All implementations only use the default built in communication components of the environment.
I have accumulated a considerable amount of experience using these standard components,
and I am confident that it will just work.

To customize the reference application you
- interface with your sensor at the input side
- use your algorithms to compute calculated values on the server side
- use a special graph on the output side
- and in some cases change the protocol.

Alongside the base collaboration there is also an advanced collaboration,
which is called the SK bridge collaboration.

[SK Bridge Collaboration](sk-bridge-collaboration.html)