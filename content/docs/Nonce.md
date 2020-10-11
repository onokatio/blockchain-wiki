---
title: "Nonce"
date: 2020-10-06T20:08:02+09:00
draft: true
---

The "nonce" in a bitcoin [[block]] is a 32-bit (4-byte) field whose value is
adjusted by miners so that the [[hash]] of the block will be less than or equal
to the current [[target]] of the network. The rest of the fields may not be
changed, as they have a defined meaning.

Any change to the block data (such as the nonce) will make the block hash
completely different. Since it is [[wikipedia:Cryptographic hash
function|believed infeasible]] to predict which combination of bits will result
in the right hash, many different nonce values are tried, and the hash is
recomputed for each value until a hash less than or equal to the current
[[target]] of the network is found. The [[target]] required is also represented
as the [[difficulty]], where a higher [[difficulty]] represents a lower
[[target]]. As this iterative calculation requires time and resources, the
presentation of the block with the correct nonce value constitutes [[proof of
work]].

== Golden Nonce == A ''golden nonce'' in Bitcoin [[mining]] is a nonce which
results in a [[block hashing algorithm|hash]] value lower than the [[target]].
In many practical mining applications, this is simplified to any nonce which
results in a block [[block hashing algorithm|hash]] which has 32 leading
zeroes<ref>https://bitcointalk.org/index.php?topic=75609.msg837556#msg837556</ref>,
with a secondary test checking if the actual value is lower than the target
difficulty.

=== Etymology === The term ''golden nonce'' most likely evolved from the term
''[http://en.wikipedia.org/wiki/Charlie_and_the_Chocolate_Factory#Plot golden
ticket]'' as used to refer to a nonce satisfying the mining requirements as
early as April 8th,
2011<ref>https://github.com/progranism/Bitcoin-JavaScript-Miner/blob/master/miner.js#L5</ref>

== References == <references />

[[Category:Technical]] [[Category:Vocabulary]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
