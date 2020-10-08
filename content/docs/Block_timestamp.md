---
title: "ブロックタイムスタンプ"
date: 2020-10-06T20:03:21+09:00
draft: true
---

(未翻訳)

Each block contains a [http://en.wikipedia.org/wiki/Unix_time Unix time] timestamp. In addition to serving as a source of variation for the [[Block_hashing_algorithm|block hash]], they also make it more difficult for an adversary to manipulate the [[block chain]].

A timestamp is accepted as valid if it is greater than the median timestamp of previous 11 blocks, and less than the network-adjusted time + 2 hours. "Network-adjusted time" is the median of the timestamps returned by all nodes connected to you. As a result block timestamps are not exactly accurate, and they do not need to be. Block times are accurate only to within an hour or two.

Whenever a node connects to another node, it gets a UTC timestamp from it, and stores its offset from node-local UTC. The network-adjusted time is then the node-local UTC plus the median offset from all connected nodes. Network time is never adjusted more than 70 minutes from local system time, however.

Bitcoin uses an unsigned integer for the timestamp, so the [[:Wikipedia:Year_2038_problem|year 2038 problem]] is delayed for another 68 years.

See also [[epoch]], [[genesis block]].

[[Category:Technical]]
