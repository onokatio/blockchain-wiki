---
title: "ブロック"
date: 2020-10-06T19:41:24+09:00
draft: true
---

ここでは、ビットコインのブロックについて書いてあります。ビットコイン以外のブロックチェーンでは別の構造の場合があります。

ビットコイン取引データはブロックと呼ばれるファイルに永久に記録されます。ブロックは、土地の登記簿や株式取引台帳の個々のページのように考えることができます。ブロックは、時間の経過とともに直線的な順序に編成されています（[ブロックチェーン]({{< ref "blockchain.md" >}})としても知られています）。新しい取引は常に[[マイニング]]によって新しいブロックに処理され、チェーンの末端に追加されます。ブロックがブロックチェーンの中に[[確認|埋まっていく]]ことで、変更や削除が難しくなるため、ビットコインの[[不可逆取引]]が発生します。

## Description

各ブロックには、[[Block timestamp|current time]]、いくつかまたはすべての最近の[[transaction]]の記録、そして直前のブロックへの参照が含まれています。また、難解な数学パズルの答えも含まれています。その答えは各ブロックに固有です。正しい答えがなければ、新しいブロックをネットワークに提出することはできません。[[マイニング]]のプロセスは、本質的には、現在のブロックを「解く」答えを見つけるために、次に来るものを競うプロセスです。各ブロックの数学的な問題は、解くのが非常に難しい[[難易度|difficult]]のですが、一度有効な解が見つかると、その解が正しいことをネットワークの残りの部分が確認するのは非常に簡単です。どのブロックにも複数の有効な解が存在します。ブロックを解くためには1つの解だけを見つける必要があります．

Because there is a reward of brand new bitcoins for solving each block, every block also contains a record of which [[Bitcoin address]]es or [[script]]s are entitled to receive the reward. This record is known as a generation transaction, or a [[coinbase]] transaction, and is always the first transaction appearing in every block. The number of [[Bitcoins]] generated per block starts at 50 and is [[controlled supply|halved]] every 210,000 blocks (about four years).

Bitcoin transactions are [[transaction broadcasting|broadcast]] to the [[network]] by the sender, and all peers trying to solve blocks collect the transaction records and add them to the block they are working to solve. Miners get incentive to include transactions in their blocks because of attached transaction fees.

The [[difficulty]] of the mathematical problem is automatically adjusted by the network, such that it targets a goal of solving an average of 6 blocks per hour.  Every 2016 blocks (solved in about two weeks), all Bitcoin clients compare the actual number created with this goal and modify the target by the percentage that it varied. The network comes to a consensus and automatically increases (or decreases) the difficulty of generating blocks.

Because each block contains a reference to the prior block, the collection of all blocks in existence can be said to form a chain.  However, it's possible for the chain to have temporary splits - for example, if two miners arrive at two different valid solutions for the same block at the same time, unbeknownst to one another.  The peer-to-peer network is designed to resolve these splits within a short period of time, so that only one branch of the chain survives.

The client accepts the 'longest' chain of blocks as valid. The 'length' of the entire [[block chain]] refers to the chain with the most combined difficulty, not the one with the most blocks. This prevents someone from forking the chain and creating a large number of low-difficulty blocks, and having it accepted by the network as 'longest'.

## Common Questions about Blocks

### How many blocks are there?
[http://blockexplorer.com/q/getblockcount Current block count]

### What is the maximum number of blocks?
There is no maximum number, blocks just keep getting added to the end of the chain at an average rate of one every 10 minutes.

#### Even when all 21 million coins have been generated?
Yes. The blocks are for proving that transactions existed at a particular time. Transactions will still occur once all the coins have been generated, so blocks will still be created as long as people are trading Bitcoins.

### How long will it take me to generate a block?
No one can say exactly. There is a [[Generation Calculator|generation calculator]] that will tell you how long it '''might''' take.

### What if I'm 1% towards calculating a block and...?
There's no such thing as being 1% towards solving a block.  You don't make progress towards solving it.  After working on it for 24 hours, your chances of solving it are equal to what your chances were at the start or at any moment. Believing otherwise is what's known as the Gambler's fallacy [http://en.wikipedia.org/wiki/Gambler's_fallacy].

It's like trying to flip 53 coins at once and have them all come up heads.  Each time you try, your chances of success are the same.

### Where can I find more technical detail?
There is more technical detail on the [[block hashing algorithm]] page.

## See Also

* [[Protocol rules#"block" messages|Protocol rules - "block" messages]]
* [https://bitcointalk.org/index.php?topic=101514.0 Format of the BDB-style block files]

[[Category:Technical]]
[[Category:Vocabulary]]

[[fr:Blocs]][[pl:Bloki]][[zh-cn:Block]]
[[de:Block]]
