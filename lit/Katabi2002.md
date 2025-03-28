---
title: Internet Congestion Control for Future High Bandwidth-Delay Product Environments
authors: [Dina Katabi, Mark Handley, Charlie Rohrs]
year: 2002
date: 2025-03-19 21:20
tags: [literature, networking, performance]
---

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

XCP informs senders the **degrees of congestion** (precise) and decouples
**utilisation control** from **fairness control**. Utilisation or aggressiveness
is adjusted according to available bandwidth and feedback delay to improve
stability by preventing oscillations. Fairness is controlled through reclaiming
bandwidth from flows whose rate is higher than their fair share and reallocates
that bandwidth to other flows.

There are no needs to store pre-flow state in router as control state is stored
in XCP packet. As such, XCP has **great scalability** in terms of number of
flows.

Decoupling of fairness control from utilisation control allows [service differentiation](../202209301020.md)
using schemes that provide desired bandwidth apportioning, yet too aggressive or
weak for controlling congestion.

Explicit and precise congestion feedback is done by telling the source to
decrease its congestion window's size. This allows the distinguishing of error
losses and congestion losses.
