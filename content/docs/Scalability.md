---
title: "スケーラビリティ"
date: 2020-10-07T00:31:56+09:00
draft: true
---

'''''Note: This page is seriously outdated and largely unmaintained; due to past incidents of edit-warring it has not been subject to much peer review.'''''

A Bitcoin [[full node]] could be modified to scale to much higher transaction rates than are seen today, assuming that said node is running on a high end servers rather than a desktop. Bitcoin was designed to support lightweight clients that only process small parts of the block chain (see ''simplified payment verification'' below for more details on this).

''Please note that this page exists to give calculations about the scalability of a Bitcoin full node and transactions on the block chain without regards to network security and decentralization. It is not intended to discuss the scalability of alternative protocols or try and summarise philosophical debates. Create alternative pages if you want to do that.''

==Note to Readers==

When techies hear about how bitcoin works they frequently stop at the word "flooding" and say "Oh-my-god! that can't scale!".  The purpose of this article is to take an extreme example, the peak transaction rate of Visa, and show that bitcoin ''could'' technically reach that kind of rate without any kind of questionable reasoning, changes in the core design, or non-existent overlays.  As such, it's merely an extreme example— not a plan for how bitcoin will grow to address wider needs (as a decentralized system it is the bitcoin using public who will decide how bitcoin grows)— it's just an argument that shows that bitcoin's core design can scale much better than an intelligent person might guess at first.

Dan rightly criticizes the analysis presented here— pointing out that operating at this scale would significantly reduce the decentralized nature of bitcoin: If you have to have many terabytes of disk space to run a "full validating" node then fewer people will do it, and everyone who doesn't will have to trust the ones who do to be honest. Dan appears (from his slides) to have gone too far with that argument: he seems to suggest that this means bitcoins will be controlled by the kind of central banks that are common today. His analysis fails for two reasons (and the second is the fault of this page being a bit misleading):

First, even at the astronomic scale presented here the required capacity is well within the realm of (wealthy) private individuals, and certainly would be at some future time when that kind of capacity was required.  A system which puts private individuals, or at least small groups of private parties, on equal footing with central banks could hardly be called a centralized one, though it would be less decentralized than the bitcoin we have today. The system could also not get to this kind of scale without bitcoin users agreeing collectively to increase the maximum block size, so it's not an outcome that can happen without the consent of bitcoin users.

Second, and most importantly, the assumed scaling described here deals with Bitcoin replacing visa.  This is a poor comparison because bitcoin alone is not a perfect replacement for visa for reasons completely unrelated to scaling:  Bitcoin does not offer instant transactions, credit, or various anti-fraud mechanisms (which some people want, even if not everyone does), for example. Bitcoin is a more complete replacement for checks, wire transfers, money orders, gold coins, CDs, savings accounts, etc. and if widely adopted probably replace the uses of credit cards which would be better served by these other things if they worked better online.

Bitcoin users sometimes gloss over this fact too quickly because people are too quick to call it a flaw but this is unfair. No one system is ideal for all usage and Bitcoin has a broader spectrum of qualities than most monetary instruments. If the bitcoin community isn't willing to point out some things would better be done by other systems then it becomes easy to make strawman arguments: If we admit that bitcoin could be used as a floor wax and desert topping, someone will always point out that it's not the best floorwax or best desert topping.

It's trivial to build payment processing and credit systems _on top_ of bitcoin, both classic ones (like Visa itself!) as well as self-proclaimed "decentralized" ones like Ripple. These systems could handle higher transaction volumes with lower costs, and settle frequently to the bitcoin that backs them. These could use other techniques with different tradeoffs than bitcoin, but still be backed and denominated by bitcoin so still enjoy its lack of central control. We see the beginnings of this today with bitcoin exchange and wallet services allowing instant payments between members.

These services would gain the benefit of the stable inflation resistant bitcoin currency, users would gain the benefits of instant transactions, credit, and anti-fraud, bitcoin overall would enjoy improved scaling from offloaded transaction volume without compromising its decentralized nature. In a world where bitcoin was widely used payment processing systems would probably have lower prices because they would need to compete with raw-bitcoin transactions, they also could be afford lower price because frequent bitcoin settling (and zero trust bitcoin escrow transactions) would reduce their risk. This is doubly true because bitcoin could conceivably scale to replace them entirely, even if that wouldn't be the best idea due to the resulting reduction in decentralization.

==Scalability targets==

VISA handles on average around 2,000 transactions per second (tps), so call it a daily peak rate of 4,000 tps. It has a peak capacity of around 56,000 transactions per second, [https://usa.visa.com/dam/VCOM/download/corporate/media/visa-fact-sheet-Jun2015.pdf] however they never actually use more than about a third of this even during peak shopping periods. [http://www.visa.com/blogarchives/us/2013/10/10/stress-test-prepares-visanet-for-the-most-wonderful-time-of-the-year/index.html]

PayPal, in contrast, handled around 10 million transactions per day for an average of 115 tps in late 2014. [https://web.archive.org/web/20141226073503/https://www.paypal-media.com/about]

Let's take 4,000 tps as starting goal. Obviously if we want Bitcoin to scale to all economic transactions worldwide, including cash, it'd be a lot higher than that, perhaps more in the region of a few hundred thousand tps. And the need to be able to withstand DoS attacks (which VISA does not have to deal with) implies we would want to scale far beyond the standard peak rates. Still, picking a target let us do some basic calculations even if it's a little arbitrary.

Today the Bitcoin network is restricted to a sustained rate of 7 tps due to the bitcoin protocol restricting block sizes to 1MB.

===CPU===

The protocol has two parts. Nodes send "inv" messages to other nodes telling them they have a new transaction. If the receiving node doesn't have that transaction it requests it with a getdata.

The big cost is the crypto and block chain lookups involved with verifying the transaction. Verifying a transaction means some hashing and some [[ECDSA]] signature verifications. RIPEMD-160 runs at 106 megabytes/sec (call it 100 for simplicity) and SHA256 is about the same. So hashing 1 megabyte should take around 10 milliseconds and hashing 1 kilobyte would take 0.01 milliseconds - fast enough that we can ignore it.

Bitcoin is currently able (with a couple of simple optimizations that are prototyped but not merged yet) to perform around 8000 signature verifications per second on an quad core [http://ark.intel.com/products/53469 Intel Core i7-2670QM 2.2Ghz processor]. The average number of inputs per transaction is around 2, so we must halve the rate. This means 4000 tps is easily achievable CPU-wise with a single fairly mainstream CPU.

As we can see, this means as long as Bitcoin nodes are allowed to max out at least 4 cores of the machines they run on, we will not run out of CPU capacity for signature checking unless Bitcoin is handling 100 times as much traffic as PayPal. As of late 2015 the network is handling 1.5 transactions/second, so even assuming enormous growth in popularity we will not reach this level for a long time.

Of course Bitcoin does other things beyond signature checking, most obviously, managing the database. We use LevelDB which does the bulk of the heavy lifting on a separate thread, and is capable of very high read/write loads. Overall Bitcoin's CPU usage is dominated by ECDSA.

===Network===

Let's assume an average rate of 2000tps, so just VISA. Transactions vary in size from about 0.2 kilobytes to over 1 kilobyte, but it's averaging half a kilobyte today.

That means that you need to keep up with around 8 megabits/second of transaction data (2000tps * 512 bytes) / 1024 bytes in a kilobyte / 1024 kilobytes in a megabyte = 0.97 megabytes per second * 8 = 7.8 megabits/second.

This sort of bandwidth is already common for even residential connections today, and is certainly at the low end of what colocation providers would expect to provide you with.

When blocks are solved, the current protocol will send the transactions again, even if a peer has already seen it at broadcast time. Fixing this to make blocks just list of hashes would resolve the issue and make the bandwidth needed for block broadcast negligable. So whilst this optimization isn't fully implemented today, we do not consider block transmission bandwidth here.

===Storage===

At very high transaction rates each block can be over half a gigabyte in size.

It is not required for most fully validating nodes to store the entire chain. In Satoshi's paper he describes "pruning", a way to delete unnecessary data about transactions that are fully spent. This reduces the amount of data that is needed for a fully validating node to be only the size of the current unspent output size, plus some additional data that is needed to handle re-orgs. As of October 2012 (block 203258) there have been 7,979,231 transactions, however the size of the unspent output set is less than 100MiB, which is small enough to easily fit in RAM for even quite old computers.

Only a small number of archival nodes need to store the full chain going back to the genesis block. These nodes can be used to bootstrap new fully validating nodes from scratch but are otherwise unnecessary.

The primary limiting factor in Bitcoin's performance is disk seeks once the unspent transaction output set stops fitting in memory. Once hard disks are phased out in favour of SSDs, it is quite possible that access to the [[UTXO]] set never becomes a serious bottleneck.

==Optimizations==

The description above applies to the current software with only minor optimizations assumed (the type that can and have been done by one man in a few weeks). 

However there is potential for even greater optimizations to be made in future, at the cost of some additional complexity.

===CPU===

Algorithms exist to accelerate batch verification over elliptic curve signatures. It's possible to check their signatures simultaneously for a 2x speedup. This is a somewhat more complex implementation.

===Simplified payment verification===
<!-- "Simplified payment verification" redirects here. Update the redirect if you change the section title -->

It's possible to build a Bitcoin implementation that does not verify everything, but instead relies on either connecting to a trusted node, or puts its faith in high difficulty as a proxy for proof of validity. [[bitcoinj]] is an implementation of this mode.

In Simplified Payment Verification (SPV) mode, named after the section of Satoshi's paper that describes it, clients connect to an arbitrary full node and download only the block headers. They verify the chain headers connect together correctly and that the difficulty is high enough. They then request transactions matching particular patterns from the remote node (ie, payments to your addresses), which provides copies of those transactions along with a Merkle branch linking them to the block in which they appeared. This exploits the Merkle tree structure to allow proof of inclusion without needing the full contents of the block. 

As a further optimization, block headers that are buried sufficiently deep can be thrown away after some time (eg. you only really need to store as low as 2016 headers).

The level of difficulty required to obtain confidence the remote node is not feeding you fictional transactions depends on your threat model. If you are connecting to a node that is known to be reliable, the difficulty doesn't matter. If you want to pick a random node, the cost for an attacker to mine a block sequence containing a bogus transaction should be higher than the value to be obtained by defrauding you. By changing how deeply buried the block must be, you can trade off confirmation time vs cost of an attack.

Programs implementing this approach can have fixed storage/network overhead in the null case of no usage, and resource usage proportional to received/sent transactions.

See also: [[Thin Client Security]].

== Related work ==
There are a few proposals for optimizing Bitcoin's scalability. Some of these require an alt-chain / hard fork.

* [https://bitcointalk.org/index.php?topic=88208.0 Ultimate blockchain compression] - the idea that the blockchain can be compressed to achieve "trust-free lite nodes"
* [http://cryptonite.info/files/mbc-scheme-rev2.pdf The Finite Blockchain] paper that describes splitting the blockchain into three data structures, each better suited for its purpose. The three data structures are a finite blockchain (keep N blocks into the past), an "account tree" which keeps account balance for every address with a non-zero balance, and a "proof chain" which is an (ever growing) slimmed down version of the blockchain.
* [https://lightning.network/ Lightning Network], an alternative protocol for transaction clearance in which nodes set up micropayment channels between each other and settle up on the block chain occasionally. Ordinary users interact primarily or only with payment channels and only use the blockchain for large transfers and cold storage.

[[Category:Technical]]
[[Category:Scalability]]
