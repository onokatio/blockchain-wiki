---
title: "P2Pool"
date: 2020-10-06T20:08:52+09:00
draft: true
---

{{Infobox company
|name=P2Pool
|trading_name=P2Pool
|industry=[[Mining pool]]
|foundation=July 17, 2011
|hashrate=1.6 Phash/s
|website= http://p2pool.in
}}
[[Image:P2pool_chain.png‎|thumb|350px|right|Visualization of the P2Pool share chain]]

'''P2Pool''' is a decentralized [[Bitcoin]] [[Bitcoin Pool|mining pool]] that works by creating a peer-to-peer network of miner nodes.

<blockquote>P2Pool creates a new block chain in which the difficulty is adjusted so a new block is found every 30 seconds. The blocks that get into the P2Pool block chain (called the "share chain") are the same blocks that would get into the Bitcoin block chain, only they have a lower difficulty target. Whenever a peer announces a new share found (new block in the P2Pool block chain), it is received by the other peers, and the other peers verify that this block contains payouts for all the previous miners who found a share (and announced it) that made it into the P2Pool share chain. This continues until some peer finds a block that has a difficulty that meets the Bitcoin network's difficulty target. This peer announces this block to the Bitcoin network and miners who have submitted shares for this block are paid in the generation transaction, proportionally to how many shares they have found in the last while. - Unknown author</blockquote>

Decentralized payout pooling solves the problem of centralized mining pools degrading the decentralization of Bitcoin and avoids the risk of hard to detect theft by pool operators.

Miners are configured to connect to a P2Pool node that can be run locally, alongside the miner. P2Pool users must run a full Bitcoin node which serves the purpose of independently validating transactions and the Bitcoin blockchain. P2Pool also supports merged mining and several alternative blockchains.

P2Pool nodes work on a chain of shares similar to Bitcoin's blockchain. Each node works on a block that includes payouts to the previous shares' owners and the node itself, which can also result in a share if it meets P2Pool's difficulty.

Because of the importance of strengthening Bitcoin's decentralization, some Bitcoin supporters donate to P2Pool miners, resulting in average returns above 100% of the expected reward.
However, it should be noted that there are other pools (such as [[BitPenny]] and [[Eligius]]) which can provide this same level of decentralization.

== Overview ==

P2Pool shares form a "sharechain" with each share referencing the previous share's hash. Each share contains a standard Bitcoin block header, some P2Pool-specific data that is used to compute the generation transaction (total subsidy, payout script of this share, a nonce, the previous share's hash, and the current target for shares), and a Merkle branch linking that generation transaction to the block header's Merkle hash.

The chain continuously regulates its target to keep generation around one share every thirty seconds, just as Bitcoin regulates it to generate one block every ten minutes.
This means that finding shares becomes more difficult (resulting in higher variance) the more people mine on P2Pool, though large miners have the option to raise their difficulty, and so reduce the impact of their mining on P2Pool's minimum difficulty.

Unlike Bitcoin, nodes do not know the entire chain - instead they only hold the last 8640 shares (the last 3 day's worth). In order to prevent an attacker from working on a chain in secret and then releasing it, overriding the existing chain, chains are judged by how much work they have since a point in the past. To ascertain that the work has been done since that point, nodes look at the Bitcoin blocks that the shares reference, establishing a provable timestamp. (If a share points to a block, it was definitely made after that block was made.)

=== Payout logic ===

Each share contains a generation transaction that pays to the previous ''n'' shares, where ''n'' is the number of shares whose total work is equal to 3 times the average work required to solve a block, or 8640 (= 72 hours of shares), whichever is smaller. Payouts are weighted based on the amount of work each share took to solve, which is proportional to the p2pool difficulty at that time.

The block reward (currently 6.25 BTC) and the transaction fees are combined and apportioned according to these rules:

A subsidy of 0.5% is sent to the node that solved the block in order to discourage not sharing solutions that qualify as a block. (A miner with the aim to harm others could withhold the block, thereby preventing anybody from getting paid. He can NOT redirect the payout to himself.) The remaining 99.5% is distributed evenly to miners based on work done recently.

In the event that a share qualifies as a block, this generation transaction is exposed to the Bitcoin network and takes effect, transferring each node its payout.

=== Stales ===

On P2Pool stales refer to shares which can't make it into the sharechain.  Because the sharechain is 20 times faster than the Bitcoin chain many stales are common and expected. However, because the payout is [[Comparison_of_mining_pools|PPLNS]] only your stale rate relative to other nodes is relevant; the absolute rate is not.

There are two reported kinds of stales in P2Pool: "DEAD ON ARRIVAL" shares and orphan shares. Dead shares were too old by the time they arrived at your local P2Pool. Very high dead rates can indicate miner misconfiguration. Orphan shares are shares which were not extended by the rest of the P2Pool network, because some other miner's share was accepted first. Very high orphan rates may indicate network connectivity problems. 

The P2Pool console output shows your relative stale rate compared to other P2Pool miners in the 'Own efficiency' column:
<pre>
2012-01-07 20:57:51.797420 Pool stales: 13% Own: 13±2% Own efficiency: 100±2%
</pre>

When you first start P2Pool claimed efficiency will be low and the error bounds on this estimate will be large, but as it runs the numbers will converge to their correct values.

If your efficiency is unusually low, make sure your network connection isn't overloaded, that your miners support long polling and are not set to work for excessive amounts of time, and that your bitcoind has multiple connections.

== Joining the pool ==

Follow these steps to join the pool:

* Run Bitcoin with the RPC interface enabled: edit bitcoin.conf to include:
 rpcuser=USER
 rpcpassword=LONG_RANDOM_SECRET_VALUE
 server=1
**'''Replace LONG_RANDOM_SECRET_VALUE with something long and random like the output of smashing your keyboard for a bit like fju4M78yAj3ds39pak92raK'''. You don't need to be able to remember it. If your RPC port becomes exposed to the internet a thief could steal your bitcoin if they could guess it, or use a brute force attack in order to find it.
** Bitcoin 0.8.5 or later is required
** It's important that your Bitcoin client be fully synchronized before starting. It's also better if you have the Bitcoin port forwarded
* Download p2pool:
** Windows binary: See http://bitcointalk.org/index.php?topic=18313.0
** Source download: https://github.com/forrestv/p2pool/tags
** git: git clone git://github.com/forrestv/p2pool.git
* Run p2pool: (See below for additional options.)
** Windows py2exe: run_p2pool.exe
** Source: python run_p2pool.py
* Run a miner daemon with long polling connecting to 127.0.0.1 (or the IP of the host running p2pool if it isn't on the same computer as the miner) on port 9332 with any username and password
** bfgminer -O u:p -o http://127.0.0.1:9332/ --submit-stale
* Subscribe to the [https://groups.google.com/forum/#!forum/p2pool-notifications P2Pool notifications] mailing list for urgent pool status updates

Dependencies if running from source:
* Python 2.6 or higher (but not 3.x)
* python-argparse
* Twisted (Ubuntu package python-twisted)

=== Frequently Asked Questions ===

'''Q:''' "Why does my miner report so many longpoll events when mining on p2pool? - P4Man"<br />
'''A:''' "Once every ~30 seconds is normal. That is how often p2pool shares are generated (as opposed to ~10 min for bitcoin blocks) - cabin"

'''Q:''' "Do the 'orphan' and 'dead' shares in P2Pool's status display hurt me?"<br />
'''A:''' They shouldn't - It's normal for some fraction of everyone's shares to end up orphaned or dead. Because payouts are calculated by counting how many shares you have relative to others, everyone with normal configurations is equally "hurt" by this. However, if you have a large proportion of stales, your payout will be hurt. You can see how well you're doing by looking at P2Pool's "Efficiency" (ex: ''Efficiency: ~110.6% (40-111%)''). If 100% doesn't lie within the [http://en.wikipedia.org/wiki/Confidence_interval confidence interval] at the end, something is probably wrong (with 95% confidence).

'''Q:''' "What do I do if my efficiency is low?"<br />
'''A:''' Make sure the computers you're running P2Pool and the miner on have enough memory and CPU time. If you have a lot of dead shares or the "Local dead on arrival" number is higher than a few percent, that means that something is wrong with your miner. Check to make sure that it is one of the working versions in the ''Miners'' section on this page. Lower the intensity or raise the FPS of your miner. If you have a lot of orphan shares, something is wrong with P2Pool's P2P connection. Decrease the load on your internet connection or enable QoS (Quality of Service) on your router.

'''Q:''' What is PPLNS?<br />
'''A:''' Pay-Per-Last-N-Shares is a payout method that is completely resistant to pool hoppers.

'''Q:''' Why am I not getting very many shares?!<br />
'''A:''' The P2Pool difficulty is hundreds of times higher than on other pools. It can take time to get a share. P2Pool displays an estimate of how long you have to wait in the console output.

'''Q:''' Why does my miner say it has found a lot of shares but p2pool say I have only found a few?!<br />
'''A:''' The real P2Pool difficulty is hundreds of times higher than on normal pools, but p2pool essentially lies to your miner and tells it to work on relatively easy shares so that it submits shares every few seconds instead of every few hours.  P2Pool then ignores any submitted shares that don't match the real share difficulty.  By doing this, P2Pool can more accurately report your local hash rate and you can see if you are having problems with too many stale shares quickly

'''Q:''' Why am I getting so many rejects?<br />
'''A:''' You're using an incompatible miner. See the miners section here, increase your FPS on the miner, decrease the intensity, upgrade your miner, or try a different miner.

'''Q:''' What stops the pool operator or the block finder from stealing a block?<br />
'''A:''' A block solution is only worth anything because its hash matches Bitcoin's target. Altering anything within the block will change its hash and make it worthless. If you are concerned about the pool operator stealing a block, you should try to inspect the source code of each new version.

'''Q:''' Why does it say "Generated?" I want to spend my coins now!<br />
'''A:''' P2Pool includes payouts in generation transactions, which must mature (taking 120 blocks or 20 hours) before they can be spent. The reason for this is that a block could be orphaned, which would make its payout invalid and could reverse transactions.

'''Q:''' Do I get paid transaction fees?<br />
'''A:''' Yes. They are split among P2Pool miners.

'''Q:''' What are these payments I'm getting that aren't generated?<br />
'''A:''' These are subsidies that people who support the idea of P2Pool send to miners.

'''Q:''' Cool Subsidies sound like an awesome idea! How do I send some BTC to these awesome miners?<br />
'''A:''' See end of this page.

'''Q:''' Do I really need the WHOLE blockchain?<br />
'''A:''' Yes. Your node needs to be able to independently make decisions about what transactions to mine.

'''Q:''' How do merged mining payments work?<br />
'''A:''' Merged mining is handled entirely by namecoind, so you're solo mining and payouts will go into namecoind's wallet.


=== Miners ===

This is all for the latest p2pool version, as it includes several new workarounds. 

With all miners, using a HIGH FPS target (100?) or a LOW intensity (8 for bfgminer?) helps a lot with reducing stales.

* bfgminer, cgminer, and ufasoft work perfectly without any extra options.
* DiabloMiner works fine after commit 3b731b9.
* Phoenix works fine after commit a658ef2.
* Poclbm works fine after commit 5e994e7.

P2Pool uses higher difficulty shares than most centralized pools, so you'll see fewer shares reported. This is normal and doesn't reduce your payments.  It's also normal to see longpoll messages once per every ten seconds on average.

====Tips to configure bfgminer to reduce stale/doa:====
* "gpu-threads" : "1", (2 by default)
* "queue" : "0", (1 by default)

Because of fast longpooling in p2pool it is better not NOT fetch work ahead.

On non-dedicated machines intensity=3 allows normal usage of PC, set it to 7 or more to get full hashrate.

On most cards best is diablo and phatk kernel, looks like poclbm kernel have unstable rate.

=== Useful features ===

* If upgrading P2Pool or changing its configuration, you can start another instance of P2Pool in parallel with the first. It will start normally, but realize that the worker and P2P listening ports are busy and keep trying to bind to them in the background. Thus, you can do almost-completely-seamless upgrades of P2Pool.
* If you run multiple P2Pool nodes or have trusted friends that run P2Pool, you can use ''-n'' to establish a constant extra P2P connection to them.
* You can make P2Pool use a configuration file by running run_p2pool.py @FILENAME, with FILENAME being the path to a file containing the command-line arguments (newlines are ignored) Example:
<pre>
--net bitcoin
-n 1.2.3.4
</pre>
* Setting the username of your miner connecting to P2Pool to a Bitcoin address will make it mine to that address instead of the one requested from bitcoind or set by -a
* Appending "/1000" to a miner's username will increase the difficulty of producing a P2Pool share to at most 1000. This is useful to large miners because doing this can make it easier for small miners while minimally impacting the large miners themselves. See [https://bitcointalk.org/index.php?topic=18313.msg816322#msg816322 recommended values].
** Appending "+1" (for example) after that will make P2Pool always give your miners work with a difficulty of 1

=== Web interface ===

Lots of data and useful tools are available at http://127.0.0.1:9332/something:
* /static/ - Lots of information from shares to graphs to payouts.
* /rate
* /users
* /fee
* /current_payouts
* /patron_sendmany - Gives sendmany outputs for fair donations to P2Pool
* /global_stats
* /local_stats
* /peer_addresses
* /payout_addr
* /recent_blocks
* /uptime
* /web/log - Some different stats collected over the last day

=== Option Reference ===
<pre>
usage: run_p2pool.py [-h] [--version] [--net {bitcoin,litecoin}] [--testnet]
                     [--debug] [-a ADDRESS] [--datadir DATADIR]
                     [--logfile LOGFILE] [--merged MERGED_URLS]
                     [--give-author DONATION_PERCENTAGE] [--iocp]
                     [--irc-announce] [--no-bugreport] [--p2pool-port PORT]
                     [-n ADDR[:PORT]] [--disable-upnp] [--max-conns CONNS]
                     [-w PORT or ADDR:PORT] [-f FEE_PERCENTAGE]
                     [--bitcoind-address BITCOIND_ADDRESS]
                     [--bitcoind-rpc-port BITCOIND_RPC_PORT]
                     [--bitcoind-p2p-port BITCOIND_P2P_PORT]
                     [BITCOIND_RPCUSERPASS [BITCOIND_RPCUSERPASS ...]]

p2pool (version 0.11.1-8-ged9359d)

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  --net {bitcoin,litecoin}
                        use specified network (default: bitcoin)
  --testnet             use the network's testnet
  --debug               enable debugging mode
  -a ADDRESS, --address ADDRESS
                        generate payouts to this address (default: <address
                        requested from bitcoind>)
  --datadir DATADIR     store data in this directory (default: <directory
                        run_p2pool.py is in>/data)
  --logfile LOGFILE     log to this file (default: data/<NET>/log)
  --merged MERGED_URLS  call getauxblock on this url to get work for merged
                        mining (example:
                        http://ncuser:ncpass@127.0.0.1:10332/)
  --give-author DONATION_PERCENTAGE
                        donate this percentage of work towards the development
                        of p2pool (default: 0.5)
  --iocp                use Windows IOCP API in order to avoid errors due to
                        large number of sockets being open
  --irc-announce        announce any blocks found on
                        irc://irc.freenode.net/#p2pool
  --no-bugreport        disable submitting caught exceptions to the author
  --disable-upnp        don't attempt to use UPnP to forward p2pool's P2P port
                        from the Internet to this computer

p2pool interface:
  --p2pool-port PORT    use port PORT to listen for connections (forward this
                        port from your router!) (default: bitcoin:9333,
                        litecoin:9338)
  -n ADDR[:PORT], --p2pool-node ADDR[:PORT]
                        connect to existing p2pool node at ADDR listening on
                        port PORT (defaults to default p2pool P2P port) in
                        addition to builtin addresses
  --max-conns CONNS     maximum incoming connections (default: 40)

worker interface:
  -w PORT or ADDR:PORT, --worker-port PORT or ADDR:PORT
                        listen on PORT on interface with ADDR for RPC
                        connections from miners (default: all interfaces,
                        bitcoin:9332, litecoin:9327)
  -f FEE_PERCENTAGE, --fee FEE_PERCENTAGE
                        charge workers mining to their own bitcoin address (by
                        setting their miner's username to a bitcoin address)
                        this percentage fee to mine on your p2pool instance.
                        Amount displayed at http://127.0.0.1:WORKER_PORT/fee
                        (default: 0)

bitcoind interface:
  --bitcoind-address BITCOIND_ADDRESS
                        connect to this address (default: 127.0.0.1)
  --bitcoind-rpc-port BITCOIND_RPC_PORT
                        connect to JSON-RPC interface at this port (default:
                        bitcoin:8332, litecoin:9332 <read from bitcoin.conf if
                        password not provided>)
  --bitcoind-p2p-port BITCOIND_P2P_PORT
                        connect to P2P interface at this port (default:
                        bitcoin:8333, litecoin:9333 <read from bitcoin.conf if
                        password not provided>)
  BITCOIND_RPCUSERPASS  bitcoind RPC interface username, then password, space-
                        separated (only one being provided will cause the
                        username to default to being empty, and none will
                        cause P2Pool to read them from bitcoin.conf)
</pre>

== Interoperability table ==
P2pool works fine with most hardware. This lists some of the hardware confirmed to work and any special configuration required.

* ASICminer blade 10GH/s (Requires adding +1 to username or proxy)
* Avalon 110nm 60-110 GH/s (All batches)
* Avalon based 55nm 200 GH/s (specific makers?)
* Avalon prototype 55nm 120GH/s (~ 20 exist)
* Icarus FPGA
* Bitfury strikes back H-card and M-card ([https://bitcointalk.org/index.php?topic=18313.msg4424081#msg4424081 instructions])
* Bitmain Antminer S1 180GH/s ([https://github.com/AntMiner/AntGen1/tree/master/firmware Requires 20131226 firmware.])
* Bitmain Antminer S3 440GH/s
* BFL SC Jalapeno, SC Single 30, 50, & 60 GH/s
* Spondoolies Tech SP 10
* Spondoolies Tech SP 30

(Various GPU and most FPGAs other than BFL single FPGAs work fine too)

This is a list of hardware with known issues that should not be used on p2pool.

* Cointerra Terraminer IV (10-20% hash rate loss when mining on p2pool)
* Btimain Antminer S2 (10-20% hash rate loss when mining on p2pool, the S1 & S3 both work well on p2pool)

== Protocol description ==

P2Pool's protocol mirrors Bitcoin's P2P protocol in many ways. It uses the same framing (prefix, command, length, checksum, payload) and similar commands:

* '''version''' - sent to establish a connection - contains (''version'', ''services'', ''addr_to'', ''addr_from'', ''nonce'', ''sub_version'', ''mode'', ''best_share_hash'')
* '''setmode''' - sent to update the ''mode'' sent in the '''version''' message - contains (''mode'')
* '''ping''' - sent to keep connection alive - contains ()
* '''addrme''' - request that the receiving node send out an addr for the sending node - contains (''port'')
* '''addrs''' - broadcast list of nodes' addresses - contains (''addrs'')
* '''getaddrs''' - request that the receiving node send ''count'' addrs - contains (''count'')
* '''getshares''' - request that the receiving node send the shares referenced by ''hashes'' and ''parents'' of their parents, stopping at any share referenced by ''stops'' - contains (''hashes'', ''parents'', ''stops'')
* '''shares''' - broadcast message of the contents of shares - contains (''shares'')

==History==

This project was announced on June 17, 2011 by Forrest Voight<ref>[https://bitcointalk.org/index.php?topic=18313.0 p2pool: Decentralized, DoS-resistant, Hop-Proof - Now active on mainnet!]</ref>.

The pool began testing against mainnet in mid-July, 2011.  The pool was reviewed on a [[Bitcoin Miner]] post on July 26, 2011<ref>[http://www.bitcoinminer.com/post/8101660461 P2Pool Decentralized Pool Nearly Ready For Prime-Time]</ref>.

The software author's address for donations can be found in the signature section of his [http://bitcointalk.org/index.php?action=profile;u=6447 forum profile].

==Donating to P2Pool miners==
In order to encourage people to mine to P2Pool you can donate to the recent miners in proportion using a sendmany:

For example, a bash script to donate 10 btc is:
<pre>
~/src/bitcoin/src/bitcoind sendmany "" "$(GET http://127.0.0.1:9332/patron_sendmany/10)"
</pre>

You can replace "" with "accountname" if you want to pay from some specific bitcoind account, and you need to replace 127.0.0.1 with the address of your P2Pool node if you're not running one locally.

Note that the amount you donate will be allocated to recent miners in proportion to the amount of work they've done in the last 24 hours or so, but all the miner whose shares of the donated amount are less than 0.01 BTC will have their shares combined into a single amount which is awarded to one of them at random, with the chance of winning this 'lottery' weighted by the miner's recent amount of work done.  You can change this 0.01 BTC threshold like this, for example, which says to pay 10 BTC, but to share it amongst more miners that the default, cutting off at 0.001 BTC instead of at 0.01 BTC.
<pre>
~/src/bitcoin/src/bitcoind sendmany "" "$(GET http://127.0.0.1:9332/patron_sendmany/10/0.001)"
</pre>

If you decide to donate you should announce it on the forums so that your donations provide the most incentive possible.

==Sponsors==

Thanks to:

* The [https://bitcoinfoundation.org/ Bitcoin Foundation] for its generous support of P2Pool
* The [https://litecoin.org/ Litecoin Project] for its generous donations to P2Pool

== Lightning P2Pool proposal ==

The existing p2pool has issues with dust. As more miners join the pool each individual miners payout becomes smaller, so eventually the cost to spend such outputs become significant. Lightning p2pool is an idea which would result in p2pool share payouts being sent over [[payment channels]]<ref>[https://www.coindesk.com/hub-and-spoke-could-bitcoins-lightning-network-decentralize-mining Lightning’s Next Act: Decentralizing Bitcoin Mining?]</ref><ref>[https://archive.is/WUjKM Payment Channel Payouts: An Idea for Improving P2Pool Scalability]</ref>.

==See Also==

* [[Comparison of mining pools]]
* [[Pooled Mining]]
* [[P2Pool code documentation]]

==External Links==

* [http://bitcointalk.org/index.php?topic=18313.0 P2Pool forum]
* [https://github.com/forrestv/p2pool p2pool] project on GitHub
* {{Freenode IRC|p2pool}} discussion and support
* [http://minefast.coincadence.com/p2pool-stats.php P2Pool Global Stats] stats page
* Up-to-date P2Pool mining stats: [http://minefast.coincadence.com/p2pool-stats.php Minefast.CoinCadence.com P2Pool stats]
* [http://poolnode.info poolnode.info] Public list of P2Pool BTC/LTC nodes.
* [https://www.bitcoinmining.com/bitcoin-mining-pools/ Bitcoin Mining Pools]
* [http://whatisp2pool.com whatisp2pool.com] An easy introduction to mining and P2Pool.
* [https://bitcointalk.org/index.php?topic=18313 Bitcointalk thread]
* [http://organofcorti.blogspot.com/2012/11/52-p2pool-achieving-expectations.html?m=1 Statistical analysis of P2Pool from Neighborhood Pool Watch]
* [http://chimera.labs.oreilly.com/books/1234000001802/ch08.html#mining_pools P2Pool section] of <i>[[Mastering Bitcoin]]</i> by [[Wikipedia:Andreas Antonopoulos|Andreas M. Antonopoulos]]
* [https://bitcointalk.org/index.php?topic=153232.0 A guide for mining efficiently on P2Pool, includes FUD repellent and FAQ]

==References==
<references />

[[Category:Pool Operators]]
{{Pools}}
[[Category:Technical]]
[[Category:Mining]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020
This page content is translated by Bitcoin wiki. original right is reserved by them.
