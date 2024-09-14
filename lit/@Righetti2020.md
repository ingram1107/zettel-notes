---
title: Failure management strategies for IoT-based railways systems
author: [Righetti, Francesca and Vallati, Carlo and Anastasi, Giuseppe and Masetti, Giulio and di Giandomenico, Felicita]
year: 2020
date: 2024-09-12 17:59
tags: [literature, rail, iot]
---

This paper studies possible failure management strategies on rail-road switches,
including handling network and sensor failures.

The authors introduce the project STINGRAY that aims to integrate railway system
into smart city which notices the isolation of different transportation system.
STINGRAY aims to develop local network over individual station plants with Power
Line Communication (PLC) and/or wireless technologies. It also wants to provide
a single control and monitoring system and added value services.

The authors notice that railway stations are excluded from modern technologies
of energy saving and failure management. As such, they want it integrate them
through a single communication infrastructure.

In Italy, rail-road switches are susceptible to malfunction due to low
temperature from snow especially during winter. Electric heaters are deployed,
though the authors would like them to be remotely controlled through Module for
Data Acquisition (MDA) and Coordinator MDA (CMDA). MDA sends data to CMDA
whereas CMDA is responsible for decision-making and telling MDA to carry out
actions. If for some reason communication broke down, MDA will use default
threshold to heat the switches accordingly.

PLC is easy to break down, and wireless communication are susceptible to
interference.

Local communication between MDAs will be initiated if network is broken down.
This is done by first detecting network failure through CMDA beacon and
candidate election through Paxos algorithm. Sensor faulty can be detected by
comparisons and test with Dison test. Then, such faulty will be notified to the
responsible personnel.
