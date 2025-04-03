---
title: Internet Congestion Control for Future High Bandwidth-Delay Product Environments
authors: [Dina Katabi, Mark Handley, Charlie Rohrs]
year: 2002
date: 2025-03-19 21:20
tags: [literature, networking, performance]
---

# Limitation of TCP

[Transmission Control Protocol (TCP)](../202206151232.md) reacts adversely to
increasing bandwidth or delay. As [Delay-Throughput Product](../202304251528.md)
increases, TCP becomes more oscillatory and prone to instability.

"As capacity increases, the majority of flows will become *short* flows which
never exit slow start." (pg 1) And this could lead to instability. For now, it
is manageable since most short flows are long-lived, but this would not remain
the case as the fraction of flows in slow start grows which could result in
exponential increase.

The increase of Delay-Throughput Product will bring down the network
performance.

The TCP additive-increase policy limits its ability to acquire spare bandwidth
to 1 packet per RTT. If Delay-Throughput Product of a single flow has thousands
of packets, TCP might waste thousands of RTTs ramping up to full utilisation
which will follow with a congestion burst.

Since TCP [Throughput](../202304111957.md) is inversely proportional to RTT,
fairness might be a problem as more flows in the Internet traverse [satellite links](../202302161850.md)
or wireless [WANs](../202207150833.md). Currently, this is mitigated by ack
spacing, split connection, or performance enhancing proxies.

# XCP: Its Design and Implementation

As stated in the paper, the authors didn't intend for
[XCP](../202502110822.md) to be backward compatible (to TCP), that is, its
implementation would be done from scratch.

TCP uses loss as the indicator for [Network Congestion](../202209302043.md),
which is binary in nature (either loss or not loss). This results in imprecise
feedbacks on the state of network congestion, which ultimately leads to
conservative increase policy and aggressive decrease policy. Instead, XCP opts
for providing senders the **degrees of congestion** which is explicit and
precise.

XCP also decouples **utilisation control** (involves aggregate traffic
behaviours) from **fairness control** (involves relative throughput of flows
sharing a link). A router will now employ an *efficiency controller (EC)* and a
*fairness controller (FC)*. Utilisation or aggressiveness of sources is adjusted
according to available bandwidth and feedback delay to improve stability by
preventing oscillations. The dynamics of [Network Congestion Control](../202304261436.md)
may be abstracted as a control loop with feedback delay which is unstable for
some large feedback delay. "... as delay increases, the sources change their
sending rates more slowly." (pg 2) Congestion feedback based on rate-mismatch is
inversely proportional to the delay and feedback based on queue-mismatch
inversely proportional to square of delay. (TODO: Control Theory)

The authors stated that the "robustness to congestion should be independent of
unknown and **quickly changing parameters**, such as the number of flows". The
controller must react as quickly as the dynamics of the controller signal, or
else the controller will always lag behind the controlled system and become
ineffective. (TODO: Control Theory) Katabi *et al.* proposed the deployment of
Active Queue Management scheme (AQM) as the controller for XCP, where the
aggregate traffic that traverse the link will be the controlled signal. The
controller will then match input traffic to link capacity. This can't be done in
TCP as its aggregate depends on the **number of flows**, which is not constant,
and AQM is simply not fast enough for that. Thus, this is reason as why the
authors advocated for the decoupling utilisation control from fairness control.

Fairness is controlled through reclaiming bandwidth from flows whose rate is
higher than their fair share and reallocates that bandwidth to other flows.

There are no needs to store pre-flow state in router as control state is stored
in XCP packet. As such, XCP has **great scalability** in terms of number of
flows.

Decoupling of fairness control from utilisation control allows [service differentiation](../202209301020.md)
using schemes that provide desired bandwidth apportioning, yet too aggressive or
weak for controlling congestion.

Explicit and precise congestion feedback is done by telling the source to
decrease its congestion window's size. This allows the distinguishing of error
losses and congestion losses.
