---
title: A Stream Input-Output System
authors: D. M. Ritchie
year: 1983
date: 2023-06-26 13:43
tags: [literature, operating-system, io]
---

Stream is defined to be a full-duplex connection between a user's process and a
device or pseudo-device which could consists of several linearly connected
processing modules and is similar to a shell [Pipeline](../202210280908.md),
except that data flows in both directions. Modules in a stream communicate
mostly by passing messages to their neighbours, but don't require access to the
neighbours' storage unless for some conventional variables used for [Flow Control](../202209302245.md).
A module provides at most one entry point, called routine, to each neighbour, to
accept messages from them. In general, the medium of communication between the
user, the stream, and the device is through messages. The device's modules are
connected to the stream automatically when the device is opened, and attached
dynamically by request of the user's program. Stream processing modules are
symmetrical in that their read and write interfaces are identical.

Each stream processing module consists of a pair of
[Queues](../202112101836.md), one for each direction. A queue data structure
contains the data queue, two routines, and some status information (includes a
pointer to the next queue downstream, various flags, and a pointer to additional
state information require by the instantiation of the queue). The two routines
are put procedure and service procedure. The former is called by its neighbour
to place messages on the data queue, the latter is scheduled to execute whenever
there is work for it to do. Queues are allocated in such a way that the
routines associated with one half of a stream module may find the queue
associated with the other half.
