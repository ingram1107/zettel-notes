---
title: A hop-by-hop Congestion Control Mechanisms in NDN Networks - A Survey
authors: [EL-Bakkouchi, Asmaa and Bouayad, Anas and Bekkali, Moulhime EL]
year: 2019
date: 2023-05-12 20:08
tags: [literature, networking, performance]
---
# Introduction of the Journal Paper

This article will perform a study on the journal paper that focus on hop-by-hop
congestion control mechanism in Named Data Network (NDN). From the paper's
definition, NDN is a content-based communication connectionless Internet
architecture paradigm intended to replace the de-facto standard TCP/IP, that
focuses on host-based communications instead. More differences between NDN and
TCP/IP are explained in the initial proposal by [@zhang2010named], including
opaque name-based routing where the routers do not need to know the meaning of
the name and mandatory data signature mechanism. The journal paper solely focus
on the analysis of hop-by-hop method implementations on how they detect the
congestion and comparison between different methods.

# Summary

The paper paints a clear picture on what is NDN exactly. It provides
several new features, such as receiver-driven, one-Interest-one-Data,
in-network caching, multi-source, and multi-path. In a NDN network, it
involves three nodes which are consumer node, intermediate node or
router, and producer node, and packets can be categorised into interest
packet and data packet. A consumer sends out an interest packet to
request content to the network, and any nodes that has the content will
send it via a data packet that follows the reverse route of the
correspondent interest packet. Any intermediate nodes that are along the
path could cache the data packet for future interest requests. Each NDN
node maintains three data structures: Content Store (CS) that caches
content, Pending Interest Table (PIT) that contains interests which are
forwarded but yet satisfied, and Forwarding Interest Table (FIT) that
provides name-based routing. NDN router's output faces have multiple
virtual queues and for every active input face there will be a virtual
queue with defined capacity.

Data packets is the main factor causing congestion in NDN networks since
it has a larger size that interest packet. Therefore, the approaches to
ease the congestion is by regulating interest sending rate, that is the
rate of sending interest packet to the network, to control the data
packet return rate. Three main approaches to approach NDN congestion
control, i.e., receiver-based method, hop-by-hop method, and hybrid
method. Receiver-based method control the congestion by detecting and
controlling the interest sending rate at the consumer nodes. On the
other hand, hop-by-hop method done it at each intermediate node. Hybrid
method is a combination of both receiver-based method and hop-by-hop
method, that detects and controls the interest sending rate at both
consumer and intermediate nodes.

Hop-by-hop congestion control is easier to deploy it in NDN networks since
intermediate routers may handle the traffic load by managing the PIT size.
Additionally, because of the short distance between intermediate routers,
hop-by-hop congestion control can quickly detect congestion and react to it due
to fast information feedback. There are two main ways to achieve hop-by-hop
congestion control, they are rate-based mechanism and window-based mechanism.
Currently, the main research efforts are directed to rate-based mechanism due to
the reason that will be explained in below. The following table shows the
current implementations for each mechanism.

| Rate-based Mechanism                 | Window-based Mechanism |
| ---                                  | ---                    |
| HoBHIS, HIS, SF, CC, PCON, IRNA, HBH | HWCC                   |

# Congestion Control Mechanism and Traffic Management Details

Rate-based mechanism controls the interest sending rate based on the
local node's information like link bandwidth. Most of the
implementations tend to let the intermediate node monitors the length of
the incoming data queue, determines its congestion state, and adjusts
the shaping rate accordingly. Window-based mechanism relies on other
metrics, that is the window size. The consumer node will adjust its
interest sending rate to be within the limitation of the window size
which is determined according to the bandwidth and round-trip time
(RTT).

HoBHIS is a rate-based mechanism predicts the network congestion and
adjusts the shaping rate accordingly before forwarding the interest
packets to the upstream nodes via timer expiration. The shaping rate is
calculated based on the data queuing occupancy. However, there are flaws
related to handling fairness and congestion processing time. With the
compliment of NACK, HoBHIS is able to inform the downstream node
whenever there is a congestion, and the upstream nodes will know that
they can't transfer interest packet. This improves performance on
managing black hole hijacks, link failure, and network congestion, but
with the drawback of decreasing interest sending rate due to propagation
delay of NACK packets.

HIS is another rate-based mechanism monitors the incoming interest rate
and adjusts the interest transferring rate based on the difference
between optimal expected rate and actual incoming interest rate in an
intermediate router. It does not require heavy uses of network resources
like link bandwidth. That being said, there could be unspecified
behaviour when encountering unknown link bandwidth.

HBH takes a different approach than HIS. Instead of monitoring the
incoming interest rate, it identifies the congestion by monitoring the
outgoing queues' occupancy at each intermediate node to see if it
reaches the minimum or maximum specified threshold. The interest output
face capacity is shared fairly among all active input faces. An explicit
notification will be sent to the routers and downstream node to increase
or decrease the interest sending rate once any intermediate node reach
the threshold.

IRNA is yet another rate-based mechanism but based on explicitly
interest rate notification. It monitors the outgoing interest queue's
occupancy on each intermediate node like HBH to detect and signal
congestion in advance. Two threshold value are set for the output face:
`qmin` which its value is near zero to prevent the queue from being
empty and `qmax` which its value is close the maximum queuing capacity
to prevent the queue from being full. Utilising these two variables, it
prevents the congestion before the buffers reach their limits and
assures an equitable distribution per input face. Both HBH and IRNA are
suitable in simultaneous interest requests from different consumer
nodes, but with the drawback of eliminating the response delay parameter
and does not handle the situation of multiple producers like HBH.

PCON is a rate-based hop-by-hop congestion control mechanism that
implements CoDel AQM algorithm to detect congestion. CoDel done it by
measuring the packet queue time and signalling this state to the
consumer nodes by explicit marking some packets. If there is a
congestion, PCON partially diverts the subsequent interest packets to
other non-congested routers, and the consumer nodes will reduce their
interest sending rates. This allows PCON to reach a higher total
throughput compares to other hop-by-hop congestion control mechanisms.
In addition, it can adapt to the evolution capacity of IP tunnels and
wireless links. Nevertheless, it has a drawback on taking much time on
adjusting the transmission percentage to optimal level in order to
determine the most suitable interface.

CC is different from any other rate-based approaches by eliminating the
excessive decrease of interest sending rate caused by continuous NACK
packets. It does not modify the interest sending rate for NACK packets
after the rate reduction for congestion until the rate reduction is
effective. Thus, it can reach a higher throughput. There are some flaws
related to fairness though.

HWCC is a window-based mechanism controls the window size for per-hop
interest transmission using H-ACK between routers and a queue to store
packets of interest when the window is shut off. Based on this
implementation, the interest window size of a specific flow can be
determined based on the interest sending rate related with each flow and
RTT link. This in turn allows the data packet to utilise their maximum
link bandwidth. HWCC could reach higher throughput performance, thus
maximum-minimum fairness while avoiding congestion efficiently. But
there are some downsides since it separates the window size for every
flow and neglects the queuing size.

The following table summarises all mechanisms discussed above alongside
with their advantages and disadvantages.

  | Mechanisms       | Advantages                                                                                                                                                                        | Disadvantages                                                                                                       |
  | ---              | ---                                                                                                                                                                               | ---                                                                                                                 |
  | HoBHIS           | Shaping the interest at the router before forwarding to upstream nodes                                                                                                            | Lack of handling fairness and congestion processing time                                                            |
  | HoBHIS with NACK | Deliver high performance in managing black hole hijacks, link failure, and network congestion                                                                                     | Decrease of interest sending rate due to propagation delay of NACK packets                                          |
  | HIS              | Avoid congestion and optimise the network resources usage                                                                                                                         | Unspecified behaviour on unknown link bandwidth                                                                     |
  | HBH              | Avoid congestion in the interest output faces and share fairly the interest output link capacity among all active input faces                                                     | Eliminate the response delay parameter and ignore multi-producer scenario                                           |
  | IRNA             | Prevent congestion before the buffers reach their limit, assure an equitable distribution per input face, and allow a correct bandwidth utilisation for simultaneous consumers | Eliminate the response delay parameter and ignore multi-producer scenario                                           |
  | PCON             | Reach a higher total throughput and can be adapted to the evolution capacity of IP tunnels and wireless links                                                                     | Spend more time to adjust the transmission percentage to the optimal level to determine the most suitable interface |
  | CC               | Eliminate the excessive reduction of the interest sending rate caused by continuous NACK packets and achieve a higher throughput                                                  | Flaws related to the fairness                                                                                       |
  | HWCC             | The data packet may efficiently exploit the network bandwidth, and can reach high throughput performance and better fairness mechanism                                            | Separate the window size for every flow and neglect the queuing size                                                |

Overall, the throughput of window-based mechanisms is inferior to of
rate-based mechanisms. Thus, most research efforts are focused on
improving rate-based mechanisms.

# Conclusion

This journal paper explores many possible solutions of congestion
control mechanisms, primarily in the direction of hop-by-hop congestion
control, in NDN networks which is a new Internet architecture paradigm
aim to replace the TCP-IP architecture. There are many rate-based
approaches trying to detect the congestion by looking at the local
information instead of observing the window size like window-based
approaches do. In practice, the overall throughput of a rate-based
mechanism is higher than the throughput of a window-based mechanism.
Therefore, there are more efforts put into rate-based mechanism trying
to figure out the most optimised way to minimise the congestion problem
in NDN network.
