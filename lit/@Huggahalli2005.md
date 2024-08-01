---
title: Direct Cache Access for High Bandwidth Network I/O
authors: [Ram Huggahalli, Ravi Iyer, Scott Tetrick]
year: 2005
date: 2024-07-28 10:44
tags: [literature, hardware, caching, performance, networking, io]
---

This paper focuses on optimising network I/O traffic processing
([Network Interface Card (NIC)](../202210012104.md)) by CPU from processor's,
cache hierarchy's, and system interconnect's point of view. The authors proposed
[Direct Cache Access (DCA)](../202407272249.md) as the solution to minimise
memory subsystem dependencies and improve the balance between processing and I/O
capabilities. They primarily look into improve processing speed in
[TCP/IP](../202206151238.md) packets. DCA improves the performance or reduces
memory latency by eliminating status read, header read, TCB read, and payload
read.

The idea is to have inbound I/O data be cached into [CPU Caches](../202403191017.md).
DCA provides two opportunities for optimisation: (1) timely availability of data
in the cache which reduce the average memory latency and (2) lower memory
bandwidth requirement.

## System Specification

The baseline system has a bandwidth of 2.6 GBps to 3.8 GBps at the receiver end
and 2.6 GBps to 3.0 GBps at the transmitter end, with 10 GBps incoming
[Ethernet](../202207051550.md) traffic speed. It has an Intel Pentium III Xeon
with 2M L2 cache. Traces (long set of instructions) are collected on the Front
Side Bus (FSB).

## Performance Analysis Tool

Huggahalli et al. used five tools for different I/O scenarios and cache
configuration, including NTTTCP (measure [network
throughput](../202304111957.md)), SPECweb99 (emulating web server), TPC-W
(emulating e-commerce site), TPC-C (emulating online transaction, mostly about
disk I/O), and CASPER (cache simulation, analyse cache structure and
hierarchies). In this paper, they had simulated 3 different sizes of cache (4MB,
8MB, and 16MB) with 64 byte line size and 8-way [set
associativity](../202403202309.md).

## Observation

Without DCA, the baseline system got performance hit of 74% and 92% respectively
with 256 bytes and 1518 bytes packet size. They have observed that most (more
than 90%) cache misses are unrelated to I/O write (coming from NIC) in transmit
intensive operations which include SPECweb99 and TCP-W, and I/O write referenced
by CPU has reached near 100% except for TPC-C (7.1%) since disk I/O is
application-dependent. Huggahalli et al. concludes that DCA is not suited in
transmit-intensive environment since it plays insignificant factor.

In SPECweb99, TPC-W, and NTTTCP, the system inhibits similar temporal behaviour,
that is 80% of I/O writes was read within 30k bus cycles. In TPC-C however, the
distance between I/O write and processor read is too large to benefit from DCA.

Data read cache misses is reduced significantly in receive-intensive traffic.
Small improvement on data read cache misses has also been observed in this paper
in transmit-intensive traffic, SPECweb99, and TPC-W. This is with the exception
of TPC-C, where it takes a performance penalty when the cache is marked as "M"
([modified](../202403271116.md)) and evicted as [write-back](../202403271105.md)
later.

Huggahalli et al. had also observed that larger cache can increase DCA
effectiveness.

## Conclusion

Huggahalli et al. concluded in this paper that DCA is suitable to be deployed in
an environment that anticipates strong producer-consumer relationships with
other I/O peripherals especially the NIC driver.
