---
title: Internet of Things for High-Speed Railways
authors: [Zhong Guidong, Xiong Ke, Zhong Zhangdui, Ai Bo]
year: 2021
date: 2024-08-16 15:01
tags: [literature, iot, networking, cloud, ai, blockchain, rail]
---

This paper focuses on IoT applications in HSR (high-speed railway) environment,
investigating its characteristics, technical perspectives, and possible
integration with technological trends (as of 2021).

Intelligent HSR must have the following capabilities: (1) a **three-dimensional
safety guarantee**, i.e., real-time monitoring and timely discovery and handling
of existing safety hazards at all stages and locations; (2) **comprehensive
energy saving**, e.g., consuming not more than 9600 kWh when operating at 350
km/h, and 4800 kWh when operating at 250 km/h; (3) **strong information
management platform**, i.e., realizing cross-system, cross-profession and
cross-industry application sharing, and integration and management of the huge
number of front-end intelligent hardware resources; (4) **comfort service
experience**, e.g., providing comprehensive efficient transfer service by
integrating various transport systems including HSRs, highways, waterways,
shipping, and pipelines, reducing the logistical costs and accelerating the
healthy competition among various transport systems. (pg. 2, Section 2)

The goals of HSR IoT include (1) **digitised transportation safety**, (2)
**networked interconnection** with peoples, things, and environment, and (3)
**intelligent operation services** where HSR IoT takes advantage of big data,
AI, microservice architecture, and other technologies, by employing ubiquitous
perception, intelligent monitoring, augmented reality (AR), video understanding,
and fault prediction technology, which is capable of providing comprehensive and
accurate analysis, forecasting, decision-making, and service recommendation.
(pg. 2, Section 2)

Characteristics of IoT in HSR can be reflected in three aspects: (1)
**comprehensive perception** (collection of external and internal environment
data, including the information of the coach such as temperature, humidity, and
vacant seats, information from the carriage body such as braking devices, power
receiving equipment, wheel and rail equipment, the shape of the roadbed
including rails and sleepers, the state of power lines such as transmission
lines, overhead contact systems and voltage imbalance, and status information of
the natural environment including rivers, mountains, tunnels, stations, and
passenger flows along the railway), (2) **reliable transmission** (real time and
remote, protected from large-scale fading caused by path loss, small-scale
fading caused by multipaths, shadow fading caused by occlusion, penetration loss
caused by metal car bodies, electromagnetic interference from the high-voltage
power lines and by vehicle body friction, interference of ground communication
networks along the way and interference caused by high connection density in
urban areas), and (3) intelligent processing (automated using cloud computing,
big data, and AI). (pg. 2-3, Section 3)

The authors divide HSR IoT into four layers: (1) **perception layer** (data
collection), (2) **network layer** (connection and data transmission), (3)
**platform layer** (cloud-based, responsible for information aggregation and
processing, provides services for application development and system
integration), and (4) **application layer** (automatic train driving,
intelligent operation and maintenance of infrastructure, intelligent dispatch
transportation, intelligent stations for construction workers, train operators,
managers, and passengers). (pg. 4-5, Section 4)

HSR IoT implementation must be aware of DoS attacks and SQL injection as they
are highly critical to prevent. (pg. 7, Section 5, Application Security)

Important technologies of HSR IoT involve are (1) **sensor detection
technology**, (2) **identification technology** (RFID, bar, 2D codes,
biometrics), (3) **positioning technology** (large-scale: GPS, Beidou, and
indoor: Wi-Fi, UWB), (4) **wired communication technology** (CAN, RS485), (5)
**wireless communication technology** (WPAN (NFC, Bluetooth), WLAN (ZigBee,
Wi-Fi), WWAN (GSM-R/LTE-R, 4G/5G), LPWAN (NB-IoT, Lora), satellite), (6) **data
processing technology** (data analysis: descriptive, diagnostic, predictive,
command, big data: ML, DL, data mining, storage: Hadoop, Spark), (7)
**intelligent decision-making** (AI), and (8) **information security
technology**. (pg. 5-7, Section 5)

Applications of HSR IoT include (1) **transportation logistics condition
monitoring** (trip passenger identity recognition, electronic ticket
identification, cargo identification, station safety monitoring, station
operation monitoring, station video surveillance), (2) **infrastructure
condition** monitoring (rail condition monitoring, subgrade/roadbed settlement
monitoring, bridge health monitoring, tunnel safety monitoring), (3) **ground
equipment condition monitoring** (signal ground equipment monitoring,
communication tower monitoring, communication leakage cable status monitoring,
contact network and power supply equipment monitoring, computer room condition
monitoring, passenger station structural health monitoring), (4) **environmental
disaster condition monitoring** (gale/wind speed monitoring alarm, rainfall
monitoring alarm, snow depth monitoring alarm, construction environment
monitoring), (5) **rolling stock condition monitoring** (locomotive condition
monitoring, locomotive positioning, identification of key components, driver's
state monitoring, vehicle equipment condition monitoring), (6) **construction
condition monitoring** (raw material construction identification, construction
operation monitoring, construction equipment condition monitoring, project
quality risk monitoring). (pg. 8, Table 1)

Example of using high pixel infrared cameras with computing module to capture
rail conditions. (pg. 12, Section 7.1)

Experiment results show that handover region of 5G is around 1.4 km to 1.6 km,
with a reduction of 20% and 15% on failure probability and communication
interruption probability respectively in hard-handover scenario (previous
technologies achieve almost 100% for both statistic). (pg. 13, Section 7.2)

UAV can be used to provide wireless communication and computing services to HSR
IoT. (pg. 14, Section 7.4)

Blockchain to enhance authenticity and fault-tolerant of HSR IoT. (pg. 14-15,
Section 7.5)

## Inference made from the paper

Enabler technologies for IoT in HSR includes big data, AI, microservice
architecture, 5G, cloud computing, and edge computing.
