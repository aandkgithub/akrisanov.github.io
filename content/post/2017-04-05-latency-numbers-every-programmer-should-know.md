---
title: "Latency Numbers Every Programmer Should Know"
date: "2017-04-05T11:49:29Z"
tags: ["Cheatsheet", "Latency", "Should Know"]
comments: "true"
flag: "gb"
---

<blockquote>
Latency is the time interval between initiating a query, transmission, or process, and receiving or detecting the results, often given as an average value over a large number of events.
</blockquote>

I'm sure that the most of the developers don't care about latency these days.
It's fair behavior because of the software itself.
To be honest, there are no high load, no real-time, no network extensive operations or even the huge amount of customers. Even so, it always helpful to know how latency can affect your applications.

Below is a cheat sheet of most frequent latency numbers every programmer "should know" (just know that they exist):

<!--more-->

| Task                               |          ns |      us |  ms |                               |
|:-----------------------------------|------------:|--------:|----:|------------------------------:|
| L1 cache reference                 |         0.5 |         |     |                               |
| Branch mispredict                  |           5 |         |     |                               |
| L2 cache reference                 |           7 |         |     |  14x L1 cache                 |
| Mutex lock/unlock                  |          25 |         |     |                               |
| Main memory reference              |         100 |         |     |  20x L2 cache, 200x L1 cache  |
| Compress 1K bytes with Zippy       |       3,000 |       3 |     |                               |
| Send 1K bytes over 1 Gbps network  |      10,000 |      10 |     |                               |
| Read 4K randomly from SSD*         |     150,000 |     150 |     | ~1GB/sec SSD                  |
| Read 1 MB sequentially from memory |     250,000 |     250 |     |                               |
| Round trip within same datacenter  |     500,000 |     500 |     |                               |
| Read 1 MB sequentially from SSD*   |   1,000,000 |   1,000 |   1 | ~1GB/sec SSD, 4X memory       |
| Disk seek                          |  10,000,000 |  10,000 |  10 | 20x datacenter roundtrip      |
| Read 1 MB sequentially from disk   |  20,000,000 |  20,000 |  20 | 80x memory, 20X SSD           |
| Send packet<br>CA->Netherlands->CA | 150,000,000 | 150,000 | 150 |                               |

### Notes

* 1 = 10^-9 seconds
* 1 = 10^-6 seconds = 1,000 ns
* 1 ms = 10^-3 seconds = 1,000 = 1,000,000 ns

### Credit

* [Jeff Dean](http://research.google.com/people/jeff/)
* [Originally by Peter Norvig](http://norvig.com/21-days.html#answers)
* [Interactive Latency](https://people.eecs.berkeley.edu/~rcs/research/interactive_latency.html)