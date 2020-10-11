---
title: "Target"
date: 2020-10-06T20:06:20+09:00
draft: true
---

# Target

(未翻訳)

See also [[Difficulty]]

The '''target''' is a 256-bit number (extremely large) that all Bitcoin clients share. The SHA-256 [[hash]] of a [[block]]'s header must be lower than or equal to the current target for the block to be accepted by the network. The lower the target, the more [[difficulty|difficult]] it is to generate a block.

It's important to realize that block generation is not a long, set problem (like doing a million hashes), but more like a lottery. Each hash basically gives you a random number between 0 and the maximum value of a 256-bit number (which is huge). If your hash is below the target, then you win. If not, you increment the nonce (completely changing the hash) and try again. 

For reasons of stability and low latency in transactions, the network tries to produce one block every 10 minutes. Every 2016 blocks (which should take two weeks if this goal is kept perfectly), every Bitcoin client compares the actual time it took to generate these blocks with the two week goal and modifies the target by the percentage difference. This makes the proof-of-work problem more or less difficult. A single retarget never changes the target by more than a factor of 4 either way to prevent large changes in difficulty.

### What is the target now?
* [http://blockexplorer.com/q/getdifficulty Current difficulty], as output by Bitcoin's getDifficulty

### When does the target change next?

* [http://blockexplorer.com/q/nextretarget Next retarget]

### What has the target been in the past?

* [http://blockexplorer.com/q/nethash Difficulty history, (at column 5)]
* [http://www.bitcointalk.org/index.php?topic=2345.msg31405#msg31405 Re: difficulty over time data?]
* [http://bitcoin.sipa.be/ Bitcoin network graphs] 

### What is the maximum target?
The maximum target used by SHA256 mining devices is:
 0x00000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF
Because Bitcoin stores the target as a floating-point type, this is truncated:
 0x00000000FFFF0000000000000000000000000000000000000000000000000000

Since a lower target makes Bitcoin generation more difficult, the maximum target is the ''lowest'' possible [[difficulty]].

## Related Links
See also [[Difficulty]]

[[Category:Technical]]
[[Category:Vocabulary]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020
This page content is translated by Bitcoin wiki. original right is reserved by them.
