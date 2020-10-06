---
title: "Bitcoind"
date: 2020-10-07T00:29:26+09:00
draft: true
---

'''bitcoind''' is a program that implements the Bitcoin protocol for remote procedure call (RPC) use. It is also the second Bitcoin [[Clients|client]] in the network's history. It is available under the [http://www.opensource.org/licenses/mit-license.php MIT license] in 32-bit and 64-bit versions for Windows, GNU/Linux-based OSes, and Mac OS X.

As part of Bitcoin Core, bitcoind has been bundled with the original client from version 0.2.6 to 0.4.9, and with [[Bitcoin-Qt]] since 0.5.0.

=== Running ===

See [[Running_bitcoind|running bitcoind]] for more detail and an example of the configuration file.

Bitcoind is a headless daemon, and also bundles a testing tool for the same daemon.  It provides a JSON-RPC interface, allowing it to be controlled locally or remotely which makes it useful for integration with other software or in larger payment systems.  [[Original Bitcoin client/API Calls list|Various commands]] are made available by the API.

To use locally, first start the program in daemon mode:
:bitcoind -daemon


Then you can execute [[Original Bitcoin client/API Calls list|API commands]], e.g.:
:bitcoin-cli getinfo
:bitcoin-cli listtransactions


To stop the bitcoin daemon, execute:
:bitcoin-cli stop

==History of official bitcoind (and predecessor) releases==
{| class="wikitable"
|-
! Version
! Date
! Supported platforms
! Reference
|-
| 0.16.0
| 2018-Feb-26
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.16.0 Bitcoin Core version 0.16.0 released]</ref>
|-
| 0.15.1
| 2017-Nov-11
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.15.1 Bitcoin Core version 0.15.1 released]</ref>
|-
| 0.15.0.1
| 2017-Sep-19
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.15.0.1 Bitcoin Core version 0.15.0.1 released]</ref>
|-
| 0.15.0
| 2017-Sep-14
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.15.0 Bitcoin Core version 0.15.0 released]</ref>
|-
| 0.14.2
| 2017-Jun-17
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.14.2 Bitcoin Core version 0.14.2 released]</ref>
|-
| 0.14.1
| 2017-Apr-22
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.14.1 Bitcoin Core version 0.14.1 released]</ref>
|-
| 0.14.0
| 2017-Mar-08
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.14.0 Bitcoin Core version 0.14.0 released]</ref>
|-
| 0.13.2
| 2017-Jan-03
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.13.2 Bitcoin Core version 0.13.2 released]</ref>
|-
| 0.13.1
| 2016-Oct-16
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.13.1 Bitcoin Core version 0.13.1 released]</ref>
|-
| 0.13.0
| 2016-Aug-23
| Windows32/64, Linux, ARM Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.13.0 Bitcoin Core version 0.13.0 released]</ref>
|-
| 0.12.1
| 2016-Apr-15
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.12.1 Bitcoin Core version 0.12.1 released]</ref>
|-
| 0.12.0
| 2016-Feb-23
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.12.0 Bitcoin Core version 0.12.0 released]</ref>
|-
| 0.11.2 
| 2015-Nov-13
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.11.2 Bitcoin Core version 0.11.2 released]</ref>
|-
| 0.11.1
| 2015-Oct-15
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.11.1 Bitcoin Core version 0.11.1 released]</ref>
|-
| 0.10.3
| 2015-Oct-14
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.10.3 Bitcoin Core version 0.10.3 released]</ref>
|-
| 0.11.0
| 2015-Jul-12
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.11.0 Bitcoin Core version 0.11.0 released]</ref>
|-
| 0.10.2
| 2015-May-19
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.10.2 Bitcoin Core version 0.10.2 released]</ref>
|-
| 0.10.1
| 2015-Apr-27
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.10.1 Bitcoin Core version 0.10.1 released]</ref>
|-
| 0.10.0
| 2015-Feb-16
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.10.0 Bitcoin Core version 0.10.0 released]</ref>
|-
| 0.9.3
| 2014-Sep-27
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.9.3 Bitcoin Core version 0.9.3 released]</ref>
|-
| 0.9.2.1
| 2014-Jun-19
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.9.2.1 Bitcoin Core version 0.9.2.1 released]</ref>
|-
| 0.9.2
| 2014-Jun-16
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.9.2 Bitcoin Core version 0.9.2 released]</ref>
|-
| 0.9.1
| 2014-Apr-8
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.9.1 Bitcoin Core version 0.9.1 released]</ref>
|-
| 0.9.0
| 2014-Mar-19
| Windows32/64, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.9.0 Bitcoin Core version 0.9.0 released]</ref>
|-
| 0.8.6
| 2013-Dec-9
| Windows32, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.8.6 Bitcoin-Qt version 0.8.6 released]</ref>
|-
| 0.8.5
| 2013-Sep-13
| Windows32, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.8.5 Bitcoin-Qt version 0.8.5 released]</ref>
|-
| 0.8.4
| 2013-Sep-3
| Windows32, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.8.4 Bitcoin-Qt version 0.8.4 released]</ref>
|-
| 0.8.3
| 2013-Jun-25
| Windows32, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.8.3 Bitcoin-Qt version 0.8.3 released]</ref>
|-
| 0.8.2
| 2013-May-29
| Windows32, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.8.2 Bitcoin-Qt version 0.8.2 released]</ref>
|-
| 0.8.1
| 2013-Mar-18
| Windows32, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.8.1 Bitcoin-Qt version 0.8.1 released]</ref>
|-
| 0.8.0
| 2013-Feb-19
| Windows32, Linux, MacOS X
| <ref>[https://bitcoin.org/en/release/v0.8.0 Bitcoin-Qt version 0.8.0 released]</ref>
|-
| 0.7.2
| 2012-Dec-14
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=130819.msg1399721#msg1399721 Bitcoin-Qt/bitcoind version 0.7.2 released]</ref>
|-
| 0.5.7
| 2012-Nov-23
| 
| 
|-
| 0.7.1
| 2012-Oct-19
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=119277.msg1283232#msg1283232 Bitcoin-Qt/bitcoind version 0.7.1 released]</ref> 
|-
| 0.7.0
| 2012-Sep-17
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=110243.msg1199467 Bitcoin-Qt/bitcoind version 0.7.0 released]</ref>
|-
| 0.5.6
| 2012-Jul-22
| Windows32, Linux, MacOS X
| 
|-
| 0.4.7
| 2012-Jul-22
| Windows32 
| 
|-
| 0.6.0.9
| 2012-Jul-08
| 
| 
|-
| 0.6.3
| 2012-Jun-25
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=89877.msg989356#msg989356 Bitcoin-Qt / bitcoind version 0.6.3 released]</ref> 
|-
| 0.6.2
| 2012-May-08
| Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=78829.msg888578#msg888578 Re: Version 0.6.1 release candidate 2]</ref> 
|-
| 0.6.1
| 2012-May-04
| Windows32, Linux, MacOS X
|
|-
| 0.6.0.7
| 2012-May-04
| 
| 
|-
| 0.5.5
| 2012-May-04
| Windows32 
| <ref>[https://bitcointalk.org/index.php?topic=79651 Version 0.5.5 and 0.4.6 released]</ref>
|-
| 0.4.6
| 2012-May-04
| Windows32 
| <ref>[https://bitcointalk.org/index.php?topic=79651 Version 0.5.5 and 0.4.6 released]</ref>
|-
| 0.5.4
| 2012-Apr-15
| Windows32, Linux
| <ref>[https://bitcointalk.org/index.php?topic=76808.0 Version 0.5.4 released]</ref>
|-
| 0.4.5
| 2012-Apr-15
| 
| 
|-
| 0.6.0
| 2012-Mar-30
| Windows32, Linux, MacOS X
| <ref>[http://www.bitcoinforum.com/bitcoin-discussion/bitcoin-org-bitcoin-version-0-6-0-released/ bitcoin.org: Bitcoin version 0.6.0 released]</ref>
|-
| 0.5.3.1
| 2012-Mar-17
| Windows32
| <ref>[https://bitcointalk.org/index.php?topic=69120.0 URGENT: Windows Bitcoin-Qt update]</ref> 
|-
| 0.5.3
| 2012-Mar-14
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=68895.0 Bitcoin-Qt, bitcoind version 0.5.3 released]</ref>
|-
| 0.4.4
| 2012-Mar-14
| Windows32 
| <ref>[https://bitcointalk.org/index.php?topic=70566.0 bitcoind version 0.4.4 released]</ref>
|-
| 0.5.2
| 2012-Jan-09
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=60146.0 Bitcoin-Qt, bitcoind version 0.5.2 released]</ref>
|-
| 0.4.3
| 2012-Jan-09
| Windows32, Linux
| <ref>[https://bitcointalk.org/index.php?topic=57734.0 bitcoind version 0.4.3 released]</ref>
|-
| 0.5.1
| 2011-Dec-15
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=54717.0 Bitcoin-Qt, bitcoind version 0.5.1 released]</ref> 
|-
| 0.4.2
| 2011-Dec-12
| 
| 
|-
| 0.5.0
| 2011-Nov-21
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=52480.0 Bitcoin-Qt/bitcoind version 0.5.0]</ref>
|-
| 0.4.1
| 2011-Nov-21
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=52503.0 wxBitcoin/bitcoind version 0.4.1]</ref>
|- 
| 0.4.0
| 2011-Sep-23
| Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=45410.0 Bitcoin version 0.4.0 released]</ref>
|-
| 0.3.24
| 2011-Jul-08
| Windows32, Linux, MacOS X
| <ref>[http://bitcointalk.org/index.php?topic=27187.0 Bitcoin version 0.3.24 released]</ref>
|-
| 0.3.23
| 2011-Jun-13
| Windows32, Linux, MacOS X
| <ref>[http://bitcointalk.org/index.php?topic=16553.0 Bitcoin version 0.3.23 released]</ref>
|-
| 0.3.22 
| 2011-Jun-05
| Windows32, Linux, MacOS X
| <ref>[http://bitcointalk.org/index.php?topic=12269.0 Bitcoin version 0.3.22]</ref>
|-
| 0.3.21 
| 2011-Apr-27
| Windows32, Linux, MacOS X
| <ref>[http://bitcointalk.org/?topic=6642.0 Bitcoin version 0.3.21]</ref>
|-
| 0.3.20
| 2011-Feb-21
| Windows32, Linux, MacOS X
| <ref>[http://bitcointalk.org?topic=3704.0 Version 0.3.20]</ref>
|-
|0.3.19 
|2010-12-12 
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=2228.msg29479#msg29479 Added some DoS limits, removed safe mode (0.3.19)]</ref> 
|-
|0.3.18
|2010-12-08
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=2162.0 Version 0.3.18]</ref>
|-
|0.3.17
|2010-11-25
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=1946.0 Version 0.3.17]</ref>
|-
|0.3.15
|2010-11-13
|Windows32, Linux
|<ref>[https://bitcointalk.org/index.php?topic=1780.0	Version 0.3.15]</ref> 
|-
|0.3.14
|2010-10-21
|Windows32, Linux
|<ref>[https://bitcointalk.org/index.php?topic=1528.0 Version 0.3.14]</ref>
|-
|0.3.13
|2010-10-01
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=1327.0 Version 0.3.13, please upgrade]</ref> 
|-
|0.3.12
|2010-09-07
|Windows32, Linux
|<ref>[https://bitcointalk.org/index.php?topic=999.0 Version 0.3.12 is now available.]</ref>
|-
|0.3.11
|2010-08-27
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=941.0 Version 0.3.11 is now available.]</ref>
|-
|0.3.10
|2010-08-15
|Windows32, Linux32/64, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=820.msg9452#msg9452 tcatm's 4-way SSE2 for Linux 32/64-bit is in 0.3.10]</ref>
|-
|0.3.8.1
|2010-08-09
|Linux64
|<ref>[https://bitcointalk.org/index.php?topic=765.0 Version 0.3.8.1 update for Linux 64-bit]</ref> 
|-
|0.3.8
|2010-08-03
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=696.0 Please upgrade to 0.3.8!]</ref> 
|-
|0.3.7
|2010-08-01
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=664.0 0.3.7 Changes]</ref> 
|-
|0.3.6
|2010-07-29
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=626.0 *** ALERT *** Upgrade to 0.3.6]</ref>
|-
|0.3.3
|2010-07-25
|Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=570.0 Bitcoin 0.3.3 released -- PLEASE UPGRADE]</ref>
|-
|0.3.2.5
|2010-07-24
|Windows32, Linux
| <ref>[https://bitcointalk.org/index.php?topic=556.0 Version 0.3.2.5 -- please test!]</ref>
|-
|0.3.2
|2010-07-17
|Windows32, Linux, MacOS X
| <ref>[https://bitcointalk.org/index.php?topic=437.0 Bitcoin 0.3.2 released]</ref> 
|-
|0.3.1
|2010-07-15
|Windows32, Linux
|<ref>[https://bitcointalk.org/index.php?topic=383.0 Bitcoin 0.3.1 released]</ref> 
|-
|0.3.0
|2010-07-06
|Windows32, Linux, MacOS X
|<ref>[https://bitcointalk.org/index.php?topic=238.0 Bitcoin 0.3 released!]</ref><ref>[http://sourceforge.net/mailarchive/message.php?msg_id=25686730 Bitcoin 0.3 released!]</ref>
|-
|0.2.0
|2009-12-17 06:52
|Windows XP /Vista, Linux
|<ref>[https://bitcointalk.org/index.php?topic=16.0 Bitcoin 0.2 released!]</ref><ref>[http://sourceforge.net/mailarchive/message.php?msg_id=24205662 Bitcoin 0.2 released]</ref>
|-
|0.1.5
|2009-02-04 19:46
|Windows NT/2000/XP
|<ref>[http://sourceforge.net/mailarchive/message.php?msg_id=21500063 Bitcoin v0.1.5 released]</ref>
|-
|0.1.3
|2009-01-12 22:48
|Windows NT/2000/XP
|<ref>[http://sourceforge.net/mailarchive/message.php?msg_id=21313152 Bitcoin v0.1.3]</ref>
|-
|0.1.2
|2009-01-11 22:32
|Windows NT/2000/XP
|<ref>[http://sourceforge.net/mailarchive/message.php?msg_id=21303153 Bitcoin v0.1.2 now available]</ref>
|-
|0.1.0
|2009-01-09
|Windows NT/2000/XP
|
|}

Up to and including version 0.3.19 is the "Satoshi code". The founder retired from development with end of 2010. [https://bitcointalk.org/index.php?topic=68121.10;wap2/  Here] are three URLs given where you still(!) (2013-01-04) can download one of the [[Satoshi client|"original Satoshi codes"]]. So also this [http://www.antepedia.com/detail/p/237812136.html Bitcoin release history].

==Theory of Operation==

bitcoind is a multithreaded C++ program. It is designed to be portable across Windows, Mac, and Linux systems. The multithreaded aspect leads to some complexity and the use of certain code patterns to deal with concurrency that may be unfamiliar to many programmers. Also, the code is aggressive in the use of C++ constructs, so it will help to be fluent with map, multimap, set, string, vector, iostream, and templates. As is typical of a C++ program, a lot of code tends to end up in the header files so be sure to search both the .cpp and .h files when looking for a function.

The client is oriented around several major operations, which are described in separate detailed articles and summarized in the following sections.


===[[Satoshi Client Initialization and Startup|Initialization and Startup]]===
Upon startup, the client performs various initialization routines including starting multiple threads to handle concurrent operations.

===[[Satoshi Client Node Discovery|Node Discovery]]===
The client uses various techniques to find out about other Bitcoin nodes that are currently connected to the network.

===[[Satoshi Client Node Connectivity|Node Connectivity]]===
The client initiates and maintains connections to other nodes.

===[[Satoshi Client Sockets and Messages|Sockets and Messages]]===
The client processes messages from other nodes and sends messages to other nodes using socket connections.

===[[Satoshi Client Block Exchange|Block Exchange]]===
Nodes advertise their inventory of blocks to each other and exchange blocks to build block chains.

===[[Satoshi Client Transaction Exchange|Transaction Exchange]]===
Nodes exchange and relay transactions with each other. The client associates transactions with bitcoin addresses in the local wallet.

===Wallet Services===
The client can create transactions using the local wallet. The client associates transactions with bitcoin addresses in the local wallet. The client provides a service for managing the local wallet.

===RPC Interface===
The client offers an JSON-RPC interface over HTTP over sockets to perform various operational functions and to manage the local wallet.

===User Interface===
Bitcoind's current user interface is the command line while it used to be based on [http://www.wxwidgets.org wxWidgets]. A graphical user interface is now provided by [[Bitcoin-qt]] in version 0.5+ for the reference client.

==Troubleshooting==

===I get "Error loading blkindex.dat" when I try to run the client===

<code>blkindex.dat</code> is part of the database that stores the local copy of the blockchain which may have become corrupted. 

Open the Bitcoin [[data directory]]:

* Windows: <code>%APPDATA%\Bitcoin</code>
* Linux: <code>~/.bitcoin</code>
* Mac: <code>~/Library/Application Support/Bitcoin/</code>

Make a backup of that entire folder, then delete everything EXCEPT <code>[[Wallet|wallet.dat]]</code>. [[Running Bitcoin|Start bitcoin]] again and it will download a fresh copy of the [[blockchain]] (WARNING: This will take a long time).

==See Also==

* [[Original Bitcoin client/API calls list]]
* [[Protocol specification|Bitcoin network protocol]]
* [[Development process]]
* [[Release process]]
* [[Changelog]]

==External Links==
* [https://github.com/bitcoin/bitcoin/ Bitcoin Core project on GitHub]

==References==
<references />
`
[[es:Bitcoind]]

[[Category:Nodes]]
[[Category:Wallets]]
[[Category:User Interfaces]]
[[Category:Clients]]
[[Category:Developer]]
[[Category:Technical]]
[[Category:Free Software]]
[[Category:License/MIT-X11]]
[[Category:Open Source]]
{{lowercase}}
