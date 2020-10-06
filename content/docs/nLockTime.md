---
title: "NLockTime"
date: 2020-10-07T00:30:25+09:00
draft: true
---

{{stub}}

'''nLockTime''' is a parameter of a transaction, that, if any input indicates so (by having nSequence not equal to UINT_MAX), mandates a minimal time (specified in either unix time or block height), before which the transaction cannot be accepted into a block.  If all inputs in a transaction have nSequence equal to UINT_MAX, then nLockTime is ignored.

* If <code>nLockTime < 500000000</code>
** Specifies the block number after which this transaction can be included in a block.
* Otherwise
** Specifies the UNIX timestamp after which this transaction can be included in a block.

Note that since the adoption of BIP 113, the time-based nLockTime is compared to the 11-block [[median time past]] (the median timestamp of the 11 blocks preceding the block in which the transaction is mined), and not the block time itself. The median time past tends to lag the current unix time by about one hour (give or take), but unlike block time it increases monotonically.

For transaction relay, nLockTime must be <= the current block's height (block-based) or <= the current median time past (if time based). This ensures that the transaction can be included in the next block.

==See Also==
* lock_time in [[Protocol_specification#tx|the protocol specification]]
* [[Timelock]]
* [https://github.com/bitcoin/bips/blob/master/bip-0113.mediawiki BIP113]
* The CHECKLOCKTIMEVERIFY opcode in [https://github.com/bitcoin/bips/blob/master/bip-0065.mediawiki BIP65]

[[Category:Technical]]
{{lowercase}}
