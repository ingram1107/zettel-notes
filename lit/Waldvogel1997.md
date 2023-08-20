---
title: Scalable High Speed IP Routing Lookups
authors: ['Marcel Waldvogel', 'George Varghese', 'Jon Turner', 'Bernhard Plattner']
year: 1997
date: 2023-08-19 12:37
tags: [literature, networking]
---

Waldvogel et al. proposed #algorithm includes [binary search](../202112102110.md)
on [Hash Tables](../202112122035.md) organised by prefix lengths to improve
[packet forwarding rates](../202207150848.md). He argues that this approach will
scale as the [IP Address](../202206281021.md) and table sizes increase, and
independent of table size, its worst runtime performance will be $O(\lg(W))$
where $W$ corresponds to the length of the address in bits.

Aggressive address aggregation needed to reduce table space which increase the
processing power requirement. [Router's](../202207061800.md) database has to
store address prefixes. Therefore, best matching prefix or longest matching
prefix algorithm should be used in order to accommodate various levels of
address hierarchies, allowing different parts of network to have different views
of the network hierarchy. With authors' algorithm, lookup with [IPv4 Address](../202206151453.md)
could be done at most 5 steps and with [IPv6 Address](../202206281129.md) at
most 7 steps. Further improvement could be done by doing the first hash by
simple index table lookup.

The paper first examines two classic algorithms by Lampson and Sklower. The
former is a binary search algorithm that has the worst runtime of $O(lg(2N)$
where $N$ denotes the number of table entries, or in other words, at most 17
lookups. Sklower approached this with radix tree implementation, but can only
achieve at best $O(W)$ runtime. Hardware solution such as content addressable
memories (CAMs) are larger, slower, and more expensive than RAM, usually small
in terms of the number of bits per entry and the number of entries, with the
additional risk of being obsolete. There are also protocol-based solutions like
IP switching with [Asynchronous Transfer Mode (ATM)](../202209221012.md)
[Virtual Circuit](../202208301234.md) or [tag switching](../202207150852.md),
but needing ingress router to perform full routing decision due to trust issues,
scarce resources, or different network view. Although tag switching addresses
the problem of different network view via hierarchical stacking of tags, it
still affects routing scalability since every original network has to know tags
in the destination network. Both protocol-based doesn't do well at hierarchy
boundaries without scaling problems. Caching could potentially dilute the cache
with hundreds of addresses that map to the same prefix due to information is
cached on entire address. Not withstanding that most caching solutions are based
on large, fully associative cache implemented with CAMs, which obviously has the
same problem describe before.

Instead, Waldvogel et al. proposed an algorithm composed of hashing that check
addresses whether they match any prefix of a particular length, $O(N)$ runtime
complexity of binary search, and precomputation to prevent backtracking in case
of failures. The routing table will be built by using array of records organised
by prefix length, each element points to a corresponded hash table. Markers are
needed in order to direct binary search to greater prefix lengths starting from
the median of the array. If $P$ is the index of the array and $level(P)$ is
written as $a_1, a_2, \ldots, a_n$, then it needs a marker at each level $a_1
a_2, \ldots, a_k, 0, 0, \ldots, 0$ such that $a_k = 1$. Thus, a hash entry
consists of a marker with real prefixes. The number of markers is limited by the
number of 1's bits in $level(P)$. For precomputation, every marker node will
have a variable that store the value of the best matching prefix of the marker.
The value will be precomputed when the node is inserted into the hash table.
During the search on the lower half (longer prefix lengths) of the tree,
client's best matching prefix's value will be updated.
