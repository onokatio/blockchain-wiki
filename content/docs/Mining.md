---
title: "マイニング"
date: 2020-10-07T00:37:21+09:00
draft: true
---

<!-- This page is designed to be short and simple! It should provide only a very brief explanation of things that have their own page and should link to other pages whenever possible. This page should serve as an entry point and a place to organize most of our mining articles. Thank You! (-Atheros) -->

[[File:Quick-and-dirty-4x5970-cooling.jpg|thumb|right|A home-made "[[Mining
rig|mining rig]]"]] == Introduction == '''Mining''' is the process of adding
transaction records to Bitcoin's public ledger of past transactions (and a
"[[Mining rig|mining rig]]" is a colloquial metaphor for a single computer
system that performs the necessary computations for "mining". This ledger of
past transactions is called the [[block chain]] as it is a chain of
[[block|blocks]]. The blockchain serves to [[Confirmation|confirm]] transactions
to the rest of the network as having taken place. Bitcoin nodes use the
blockchain to distinguish legitimate Bitcoin transactions from attempts to
re-spend coins that have already been spent elsewhere.

Mining is intentionally designed to be resource-intensive and difficult so that
the number of blocks found each day by miners remains steady. Individual
[[blocks]] must contain a [[proof of work|proof of work]] to be considered
valid. This proof of work is verified by other Bitcoin nodes each time they
receive a block. Bitcoin uses the [[hashcash]] proof-of-work function.

The primary purpose of mining is to set the history of [[transactions]] in a way
that is [[Irreversible Transactions|computationally impractical to modify by any
one entity]]. By downloading and verifying the blockchain, bitcoin [[full
node|nodes]] are able to reach consensus about the ordering of events in
bitcoin.

Mining is also the mechanism used to [[Controlled supply|introduce Bitcoins]]
into the system: Miners are paid any [[transaction fees]] as well as a "subsidy"
of newly created coins. This both serves the purpose of disseminating new coins
in a decentralized manner as well as motivating people to provide security for
the system.

Bitcoin mining is so called because it resembles the mining of other
commodities: it requires exertion and it slowly makes new units available to
anybody who wishes to take part. An important difference is that the
[[Controlled supply|supply]] does not depend on the amount of mining. In general
changing total miner hashpower does not change how many bitcoins are created
over the long term.

== Difficulty == === The Computationally-Difficult Problem === Mining a block is
difficult because the SHA-256 hash of a block's header must be lower than or
equal to the [[Target|target]] in order for the block to be accepted by the
network. This problem can be simplified for explanation purposes: The hash of a
block must start with a certain number of zeros. The probability of calculating
a hash that starts with many zeros is very low, therefore many attempts must be
made. In order to generate a new hash each round, a [[Nonce|nonce]] is
incremented. See [[Proof of work]] for more information.

=== The Difficulty Metric === The [[Difficulty|difficulty]] is the measure of
how difficult it is to find a new block compared to the easiest it can ever be.
The rate is recalculated every 2,016 blocks to a value such that the previous
2,016 blocks would have been generated in exactly one fortnight (two weeks) had
everyone been mining at this difficulty. This is expected yield, on average, one
block every ten minutes.

As more miners join, the rate of block creation increases. As the rate of block
generation increases, the difficulty rises to compensate, which has a balancing
of effect due to reducing the rate of block-creation. Any blocks released by
malicious miners that do not meet the required [[Target|difficulty target]] will
simply be rejected by the other participants in the network.

=== Reward === When a block is discovered, the discoverer may award themselves a
certain number of bitcoins, which is agreed-upon by everyone in the network.
Currently this bounty is 6.25 bitcoins; this value will halve every 210,000
blocks. See [[Controlled Currency Supply]].

Additionally, the miner is awarded the fees paid by users sending transactions.
The fee is an incentive for the miner to include the transaction in their block.
In the future, as the number of new bitcoins miners are allowed to create in
each block dwindles, the fees will make up a much more important percentage of
mining income.

== The mining ecosystem ==

=== Hardware === [[File:Usb-fpga module 1.15x-hs-800.jpg|thumb|right|FPGA
Module]] Users have used various types of hardware over time to mine blocks.
Hardware specifications and performance statistics are detailed on the [[Mining
Hardware Comparison]] page. ==== CPU Mining ==== Early Bitcoin client versions
allowed users to use their CPUs to mine. The advent of GPU mining made CPU
mining financially unwise as the hashrate of the network grew to such a degree
that the amount of bitcoins produced by CPU mining became lower than the cost of
power to operate a CPU. The option was therefore removed from the core Bitcoin
client's user interface.

==== GPU Mining ==== GPU Mining is drastically faster and more efficient than
CPU mining. See the main article: [[Why a GPU mines faster than a CPU]]. A
variety of popular [[Mining rig|mining rigs]] have been documented. ==== FPGA
Mining ==== FPGA mining is a very efficient and fast way to mine, comparable to
GPU mining and drastically outperforming CPU mining. FPGAs typically consume
very small amounts of power with relatively high hash ratings, making them more
viable and efficient than GPU mining. See [[Mining Hardware Comparison]] for
FPGA hardware specifications and statistics. ==== ASIC Mining ==== An
application-specific integrated circuit, or ''ASIC'', is a microchip designed
and manufactured for a very specific purpose. ASICs designed for Bitcoin mining
were first released in 2013. For the amount of power they consume, they are
vastly faster than all previous technologies and already have made GPU mining
financially.

==== Mining services (Cloud mining) ==== [[:Category:Mining_contractors|Mining
contractors]] provide mining services with performance specified by contract,
often referred to as a "Mining Contract." They may, for example, rent out a
specific level of mining capacity for a set price at a specific duration.

=== Pools === As more and more miners competed for the limited supply of blocks,
individuals found that they were working for months without finding a block and
receiving any reward for their mining efforts. This made mining something of a
gamble. To address the variance in their income miners started organizing
themselves into [[Pooled mining|pools]] so that they could share rewards more
evenly. See [[Pooled mining]] and [[Comparison of mining pools]].

=== History === Bitcoin's public ledger (the "block chain") was started on
January 3rd, 2009 at 18:15 UTC presumably by [[Satoshi Nakamoto]]. The first
block is known as the [[genesis block]]. The first transaction recorded in the
first block was a single transaction paying the reward of 50 new bitcoins to its
creator.

== Staking ==

Staking is a concept in the [[Delegated proof of stake]] coins, closely
resembling pooled mining of proof of work coins. According to the [[Proof of
Stake|proof of share]] principle, instead of computing powers, the partaking
users are pooling their ''stakes'', certain amounts of money, blocked on their
wallets and delegated to the pool’s staking balance.

The network periodically selects a pre-defined number of top staking pools
(usually between 20 and 100), based on their staking balances, and allows them
to validate transactions in order to get a reward. The rewards are then shared
with the delegators, according to their stakes with the pool.

Although staking doesn’t require lots of computing power as mining, it still
needs very stable and fast Internet connection in order to collect, verify and
sign all transactions in the queue within a small timespan, which can be as
short as one second. If a pool fails to do so, it doesn’t get the reward, and it
may be shared with the next pool in order.

A lot of [[altcoin]]s are using staking. Staking is often marketed as a much
more efficient alternative. Unfortunately staking has the potential to not be
much different than politics. A good example is that it's easy for a big actor
to take over the network by simply buying enough coins. This actually happened
in 2020 when TRON's Justin Sun took over the Steem "forum" network and then did
some things that made some people unhappy.

==See Also==

- [https://99bitcoins.com/beginners-guide-to-mining/ Beginner's Guide to Bitcoin
  Mining]
- [https://www.zpool.ca Bitcoin Multipool]
- [https://www.bitcoinmining.com Bitcoin Mining]
- [http://codinginmysleep.com/bitcoin-mining-in-plain-english Bitcoin Mining in
  Plain English] by David Perry
- [https://www.weusecoins.com/en/mining-guide/ Getting Started With Bitcoin
  Mining]
- [[Automatically mine when computer is locked|Tutorial to automatically start
  mining when you lock your computer]]. (Windows 7)
- [http://bitcoinminer.com Bitcoin Miner]
- [http://www.bitcongress.org/bitcoin/best-bitcoin-mining-hardware/ Bitcoin
  Mining Hardware Comparison]
- [http://www.reddit.com/r/Bitcoin/comments/18q2jx/eli5_bitcoin_mining_xpost_in_eli5/
  Simplified Explanation of Bitcoin Mining] by reddit user
  [http://www.reddit.com/user/azotic azotic]
- [https://bitcoinchain.com/pools Bitcoin Mining Pools Comparison]
- [http://www.bitcoinmining.com/best-bitcoin-cloud-mining-contract-reviews/
  Research, Review and Compare Cloud Mining Contracts]
- [https://www.youtube.com/watch?v=GmOzih6I1zs Video: What is Bitcoin Mining?]
- [http://yogh.io/#mine:last Mining Simulator]
  ([https://github.com/JornC/bitcoin-transaction-explorer GitHub source])
- [http://bitcoindaily.org/bitcoin-guides/what-is-bitcoin-mining/ Bitcoin Mining
  Explained][ru:Mining]] [[Category:Mining]][[Category:Vocabulary]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
