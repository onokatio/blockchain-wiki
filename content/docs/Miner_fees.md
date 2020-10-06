---
title: "マイニング手数料"
date: 2020-10-07T00:29:44+09:00
draft: true
---

'''Miner fees''' are a fee that spenders may include in any Bitcoin on-chain [[transaction]].  The fee may be collected by the miner who includes the [[transaction]] in a [[block]].

== Overview ==

Every Bitcoin transaction spends zero or more bitcoins to zero or more recipients.  The difference between the amount being spent and the amount being received is the transaction fee (which must be zero or more).

Bitcoin's design makes it easy and efficient for the spender to specify how much fee to pay, whereas it would be harder and less efficient for the recipient to specify the fee, so by custom the spender is almost always solely responsible for paying all necessary Bitcoin transaction fees.

When a miner creates a [[block proposal]], the miner is entitled to specify where all the fees paid by the transactions in that block proposal should be sent.  If the proposal results in a valid block that becomes a part of the [[best block chain]], the fee income will be sent to the specified recipient.  If a valid block does not collect all available fees, the amount not collected are permanently destroyed; this has happened on more than 1,000 occasions from 2011 to 2017,<ref>[https://medium.com/@alcio/how-to-destroy-bitcoins-255bb6f2142e How to Destroy Bitcoins] by Antoine Le Calvez, Medium.com, retrieved 2018-01-03</ref><ref>[https://twitter.com/random_walker/status/948590112576868354 "Looks like back in 2012, when tx fees started becoming common, some miners were claiming the standard 50 BTC and leaving all tx fees unclaimed."], Arvind Narayanan, Twitter.com, posted 2018-01-03, retrieved 2018-01-03</ref> with decreasing frequency over time.  


==The market for block space==

[[File:fee.png|thumb|Receiving the fees from hundreds of transactions (0.44 BTC)]]

The minimum fee necessary for a transaction to confirm varies over time and arises from the intersection of supply and demand in Bitcoin's free market for block space.<ref>[https://medium.com/@bramcohen/how-wallets-can-handle-transaction-fees-ff5d020d14fb Bram Cohen blog post with helpful background to the market for block space]</ref>  On the supply size, Bitcoin has a [[maximum block size]] (currently one million vbytes) that limits the maximum amount of transaction data that can be added to a block.

However, Bitcoin blocks are not produced on a fixed schedule—the system targets an average of one block every 10 minutes over long periods of time but, over short periods of time, a new block can arrive in less than a second or more than an hour after the previous block.  As the number of blocks received in a period of time varies, so does the effective maximum block size.  For example, in the illustration below we see the average time between blocks based on the time they were received by a node during a one day period (left axis) and the corresponding effective maximum block size implied by that block production rate (right axis, in million [[vbytes]]):

[[File:Time-between-blocks.png|center]]

During periods of higher effective maximum block sizes, this natural and unpredictable variability means that transactions with lower fees have a higher than normal chance of getting confirmed—and during periods of lower effective maximum block sizes, low-fee transactions have a lower than normal chance of getting confirmed.

On the demand side of Bitcoin's free market for block space, each spender is under unique constraints when it comes to spending their bitcoins.  Some are willing to pay high fees; some are not.  Some desire fast confirmation; some are content with waiting a while.  Some use wallets with excellent dynamic fee estimation; some do not.  In addition, demand varies according to certain patterns, with perhaps the most recognizable being the weekly cycle where fees increase during weekdays and decrease on the weekend:

[[File:Block-fees-dow.png|center]]

Another less recognizable cycle is the intra-day cycle where fees wax and wane during the day:

[[File:Block-fees-hod.png|center]]

These variations in supply and demand create a market for block space that allows users to make a trade-off between confirmation time and cost. Users with high time requirements may pay a higher than average transaction fee to be confirmed quickly, while users under less time pressure can save money by being prepared to wait longer for either a natural (but unpredictable) increase in supply or a (somewhat predictable) decrease in demand.

It is envisioned that over time the cumulative effect of collecting transaction fees will allow those creating new blocks to "earn" more bitcoins than will be mined from new bitcoins created by the new block itself.  This is also an incentive to keep trying to create new blocks as the creation of new bitcoins from the mining activity goes towards [[Controlled supply|zero in the future]].<ref>[https://bitcoincore.org/bitcoin.pdf Bitcoin: A Peer-to-Peer Electronic Cash], section 6: Incentive, Satoshi Nakamoto, 2008-11-01, retrieved 2018-01-22</ref>

== Feerates ==

Perhaps the most important factor affecting how fast a transaction gets confirmed is its fee rate (often spelled feerate).  This section describes why feerates are important and how to calculate a transaction's feerate.

Bitcoin transaction vary in size for a variety of reasons.  We can easily visualize that by drawing four transactions side-by-side based on their size (length) with each of
our examples larger than the previous one:

[[File:Feerate1.png|400px|center]]

This method of illustrating length makes it easy to also visualize an example maximum block size limit that constrains how much transaction data a miner can add to an individual block:

[[File:Feerate2.png|400px|center]]

Since Bitcoin only allows whole transactions to be added to a particular block, at least one of the transactions in the example above can't be added to the next block.  So how does a miner select which transactions to include?  There's no required selection method (called ''policy'') and no known way to make any particular policy required, but one strategy popular among miners is for each individual miner to attempt to maximize the amount of fee income they can collect from the transactions they include in their blocks.

We can add a visualization of available fees to our previous illustration by keeping the length of each transaction the same but making the area of the transaction equal to its fee.  This makes the height of each transaction equal to the fee divided by the size, which is called the ''feerate:''

[[File:Feerate3.png|400px|center]]

Although long (wide) transactions may contain more total fee, the high-feerate (tall) transactions are the most profitable to mine because their area is greatest compared to the amount of space (length) they take up in a block.  For example, compare transaction B to transaction D in the illustration above.  This means that miners attempting to maximize fee income can get good results by simply sorting by feerate and including as many transactions as possible in a block:

[[File:Feerate4.png|400px|center]]

Because only complete transactions can be added to a block, sometimes (as in the example above) the inability to include the incomplete transaction near the end of the block frees up space for one or more smaller and lower-feerate transactions, so when a block gets near full, a profit-maximizing miner will often ignore all remaining transactions that are too large to fit and include the smaller transactions that do fit (still in highest-feerate order):

[[File:Feerate5.png|400px|center]]

Excluding some rare and rarely-significant edge cases, the feerate sorting described above maximizes miner revenue for any given block size as long as none of the transactions depend on any of the other transactions being included in the same block (see the next section, ''feerates for dependent transactions,'' for more information about that).

To calculate the feerate for your transaction, take the fee the transaction pays and divide that by the size of the transaction (currently based on [[weight units]] or [[vbytes]] but no longer based on [[bytes]]).  For example, if a transaction pays a fee of 2,250 nanobitcoins and is 225 vbytes in size, its feerate is 2,250 divided by 225, which is 10 nanobitcoins per vbyte (this happens to be the minimum fee [[Bitcoin Core]] Wallet will pay by default).

When comparing to the feerate between several transactions, ensure that the units used for all of the measurements are the same.  For example, some tools calculate size in weight units and others use vbytes; some tools also display fees in a variety of [[Units|denominations]].

== Feerates for dependent transactions (child-pays-for-parent) ==

Bitcoin transactions can depend on the inclusion of other transactions in the same block, which complicates the feerate-based transaction selection described above.  This section describes the rules of that dependency system, how miners can maximize revenue while managing those dependencies, and how bitcoin spenders can use the dependency system to effectively increase the feerate of unconfirmed transactions.

Each transaction in a block has a sequential order, one transaction after another.  Each block in the block chain also has a sequential order, one block after another.  This means that there's a single sequential order to every transaction in the [[best block chain]].

[[File:Ancestor-feerate1.png|center]]

One of Bitcoin's [[consensus rules]] is that the transaction where you receive bitcoins must appear earlier in this sequence than the transaction where you spend those bitcoins.  For example, if Alice pays Bob in transaction A and Bob uses those same bitcoins to pay Charlie in transaction B, transaction A must appear earlier in the sequence of transactions than transaction B.  Often this is easy to accomplish because transaction A appears in an earlier block than transaction B:

[[File:Ancestor-feerate2.png|center]]

But if transaction A and B both appear in the same block, the rule still applies: transaction A must appear earlier in the block than transaction B.

This complicates the task of maximizing fee revenue for miners.  Normally, miners would prefer to simply sort transactions by feerate as described in the ''feerate'' section above.   But if both transaction A and B are unconfirmed, the miner cannot include B earlier in the block than A even if B pays a higher feerate.  This can make sorting by feerate alone less profitable than expected, so a more complex algorithm is needed.  Happily, it's only slightly more complex.

For example, consider the following four transactions that are similar to those analyzed in the preceding ''feerate'' section:

[[File:Ancestor-feerate3.png|center]]

To maximize revenue, miners need a way to compare groups of related transactions to each other as well as to individual transactions that have no unconfirmed dependencies.  To do that, every transaction available for inclusion in the next block has its feerate calculated for it and all of its unconfirmed ancestors.  In the example, this means that transaction B is now considered as a combination of transaction B plus transaction A:

[[File:Ancestor-feerate4.png|center]]

Note that this means that unconfirmed ancestor transactions will be considered twice or more, as in the case of transaction A in our example which is considered once as part of the transaction B+A group and once on its own.  We'll deal with this complication in a moment.

These transaction groups are then sorted in feerate order as described in the previous ''feerate'' section:

[[File:Ancestor-feerate5.png|center]]

Any individual transaction that appears twice or more in the sorted list has its redundant copies removed.  In the example case, we remove the standalone version of transaction A since it's already part of the
transaction B+A group:

[[File:Ancestor-feerate6.png|center]]

Finally, we see if we can squeeze in some smaller transactions into the end of the block to avoid wasting space as described in the previous ''feerate'' section.  In this case, we can't, so no changes are made.

Except for some edge cases that are rare and rarely have a significant impact on revenue, this simple and efficient transaction sorting algorithm maximizes miner feerate revenue after factoring in transaction dependencies.

Note: to ensure the algorithm runs quickly, implementations such as Bitcoin Core limit the maximum number of related transactions that will be collected together for consideration as one group.  As of Bitcoin Core 0.15.0 (released late 2017), this is a maximum of 25 transactions, although there have been proposals to increase this amount somewhat.

For spenders, miner use of transaction grouping means that if you're waiting for an unconfirmed transaction that pays too low a feerate (e.g.  transaction A), you can create a child transaction spending an output of that transaction and which pays a much higher feerate (e.g. transaction B) to encourage miners to confirm both transactions in the same block.  Wallets that explicitly support this feature often call it ''child pays for parent (CPFP)'' because the child transaction B helps pay for the parent transaction A.

To calculate the feerate for a transaction group, sum the fees paid by all the the group's unconfirmed transactions and divide that by the sum of the sizes for all those same transactions (in [[weight units]] or [[vbytes]]).  For example, if transaction A has a fee of 1,000 nanobitcoins and a size of 250 vbytes and transaction B has a fee of 3,000 nanobitcoins and a size of 150 vbytes, the combined feerate is (1,000 + 3,000)/(250 + 150), which is 10 nanobitcoins per vbyte.

The idea behind ancestor feerate grouping goes back to at least 2013 and saw several different proposals to add it to Bitcoin Core, with it finally becoming available for production with the August 2016 release of Bitcoin Core 0.13.0.<ref>[https://bitcoincore.org/en/2016/08/23/release-0.13.0/#more-intelligent-transaction-selection-for-mining More intelligent transaction selection for mining], Bitcoin Core 0.13.0 release notes, BitcoinCore.org, 2016-08-23, retrieved 2017-01-14</ref>

==Reference Implementation==

The following sections describe the behavior of the [[Original Bitcoin client|reference implementation]] as of version 0.12.0. Earlier versions treated fees differently, as do other popular implementations (including possible later versions).

===Sending===

Users can decide to pay a predefined fee rate by setting `-paytxfee=<n>`
(or `settxfee <n>` rpc during runtime). A value of `n=0` signals Bitcoin
Core to use floating fees. By default, Bitcoin Core will use floating
fees.

Based on past transaction data, floating fees approximate the fees
required to get into the `m`th block from now. This is configurable
with `-txconfirmtarget=<m>` (default: `2`).

Sometimes, it is not possible to give good estimates, or an estimate
at all. Therefore, a fallback value can be set with `-fallbackfee=<f>`
(default: `0.0002` BTC/kB).

At all times, Bitcoin Core will cap fees at `-maxtxfee=<x>` (default:
0.10) BTC.
Furthermore, Bitcoin Core will never create transactions smaller than
the current minimum relay fee.
Finally, a user can set the minimum fee rate for all transactions with
`-mintxfee=<<nowiki>i</nowiki>>`, which defaults to 1000 satoshis per kB.


Note that a typical transaction is 500 bytes.

===Including in Blocks===

This section describes how the reference implementation selects which transactions to put into new blocks, with default settings. All of the settings may be changed if a miner wants to create larger or smaller blocks containing more or fewer free transactions.

Then transactions that pay a fee of at least 0.00001 BTC/kb are added to the block, highest-fee-per-kilobyte transactions first, until the block is not more than 750,000 bytes big.

The remaining transactions remain in the miner's "memory pool", and may be included in later blocks if their priority or fee is large enough.

For Bitcoin Core 0.12.0 zero bytes<ref>https://github.com/bitcoin/bitcoin/blob/v0.12.0/doc/release-notes.md#relay-and-mining-priority-transactions</ref> in the block are set aside for the highest-[[Transaction_fees#Priority_transactions|priority transactions]]. Transactions are added highest-priority-first to this section of the block.

===Relaying===

The reference implementation's rules for relaying transactions across the peer-to-peer network are very similar to the rules for sending transactions, as a value of 0.00001 BTC is used to determine whether or not a transaction is considered "Free". However, the rule that all outputs must be 0.01 BTC or larger does not apply. To prevent "penny-flooding" denial-of-service attacks on the network, the reference implementation caps the number of free transactions it will relay to other nodes to (by default) 15 thousand bytes per minute.

===Settings===

{| class="wikitable"
|-
! Setting !! Default Value (units)
|-
| txconfirmtarget || 2 (blocks)
|-
| paytxfee || 0 (BTC/kB)
|-
| mintxfee || 0.00001 (BTC/kB)
|-
| limitfreerelay || 15 (thousand bytes per minute)
|-
| minrelaytxfee || 0.00001 (BTC/kB)
|-
| blockmaxsize || 750000 (bytes)
|-
| blockminsize || 0 (bytes)
|-
| blockprioritysize || 0 (bytes)
|}

==Fee Plotting Sites==

As of May 2016, the following sites seem to plot the required fee, in satoshi per (kilo)byte, required to get a transaction mined in a certain number of blocks. Note that all these algorithms work in terms of probabilities.

* https://bitcoinfees.21.co/
* https://bitcoinfees.github.io/
* https://statoshi.info/dashboard/db/fee-estimates
* http://p2sh.info/dashboard/db/fee-estimation
* https://www.bitcoinqueue.com/2d.html
* https://bitcoinfees.net/

=== Other Useful Sites ===

* https://anduck.net/bitcoin/fees/ - Shows what kind of fees filled up recent blocks
* https://btc.com/stats/unconfirmed-tx - the state of the website's mempool broken down by fee rate
* https://jochen-hoenicke.de/queue/#1w - another mempool broken down by fee rate site
* https://esotericnonsense.com/ - yet another mempool site, also very good
* https://estimatefee.com/ - You can click any of the transactions in that graph to get more info (like its "effective fee rate" which uses recursive ancestors/descendants for CPFP) and an estimated amount of blocks it'll take to confirm. It works with any transaction in the top 100MB of the bitcoin mempool. And you can enter in any transaction txid for info when you'er wondering why it doesn't confirm.
* https://mempool.space/ - Shows how the next few blocks are shaping up and the range of fees per block.  Helps estimate the low end fee to use to get into the next block.
* https://whatthefee.io/ - Shows the probability of a particular fee rate being confirmed within a particular timeframe.

==Priority transactions==

Historically it was not required to include a fee for every transaction. A large portion of miners would mine transactions with no fee given that they had enough "priority". Today, low priority is mostly used as an indicator for spam transactions and almost all miners expect every transaction to include a fee. Today miners choose which transactions to mine only based on fee-rate.

Transaction priority was calculated as a value-weighted sum of input age, divided by transaction size in bytes:
 priority = sum(input_value_in_base_units * input_age)/size_in_bytes
Transactions needed to have a priority above 57,600,000 to avoid the enforced limit (as of client version 0.3.21).  This threshold was written in the code as COIN * 144 / 250, suggesting that the threshold represents a one day old, 1 btc coin (144 is the expected number of blocks per day) and a transaction size of 250 bytes.

So, for example, a transaction that has 2 inputs, one of 5 btc with 10 confirmations, and one of 2 btc with 3 confirmations, and has a size of 500bytes, will have a priority of
 (500000000 * 10 + 200000000 * 3) / 500 = 11,200,000

===Historic rules for free transactions===
A transaction was safe to send without fees if these conditions were met:

* It is smaller than 1,000 bytes.
* All outputs are 0.01 BTC or larger.
* Its priority is large enough (see above)

==See Also==

* [[Techniques to reduce transaction fees]]
* [[Free transaction relay policy]]
* [[Fee bumping]]
* [[Replace by fee]]
* [http://bitcoinexchangerate.org/fees Unconfirmed transactions fee chart]
* [https://jochen-hoenicke.de/queue/ historic fees and mempools]

==References==
<references />

[[Category:Technical]]
[[Category:Vocabulary]]
[[Category:Mining]]
[[de:Transaktionsgebühren]]

{{Bitcoin Core documentation}}
