---
title: "代替ノード"
date: 2020-10-07T00:34:35+09:00
draft: true
---

This is a list of nodes which are considered reliable.

== How to use this list ==

=== Connect to nodes ===

You can connect to these nodes with the ''-addnode=ip'' switch instead of the usual node harvesting process (through IRC or via the embedded nodelist). You can connect to more than one node by using ''-addnode=ip'' more than once. It is usually a good idea to connect to more than one of these nodes.

==== Nodes without a fixed ip ====

If the node IP is not fixed (see "Fixed" column), you will have to resolve the node's name (first column) each time the IP changes. Some nodes may have their ip change once a day, some others once a month, and some others may stay on the same IP for years. Still, as long as the IP is not fixed, there is no guarantee it will stay the same.

In order to enable hostname lookups for the ''-addnode'' and ''-connect'' parameters, you must additionally provide the ''-dns'' parameter. Example:
 bitcoind -dns -addnode=bitcoin.es

Versions prior to 0.3.22 do not support hostnames to the ''-addnode'' parameter, so you must do the resolving part for it. For example on linux:
 bitcoind -addnode=$(dig +short bitcoin.es)

=== IP Transactions ===

[[Bitcoin Core]] versions prior to 0.8.0 also could send [[IP Transactions]] to these nodes. If you included your bitcoin address in the "message" field, you might have had your coins back.

=== Tor network ===

To use Bitcoin-Qt over Tor hidden services, in a terminal/console enter:
 bitcoin-qt -proxy=127.0.0.1:9050 -onlynet=tor

To use Bitcoin with one specific Tor node, run
 bitcoin-qt -proxy=127.0.0.1:9050 -connect=abcde.onion
, where abcde.onion needs to be substituted with one of the [[Fallback_Nodes#Tor_nodes|Tor nodes below]]. These parameters can be added to [[Running_Bitcoin#Bitcoin.conf_Configuration_File|bitcoin.conf]] to make them permanent.

You can find detailed information on running clients and hidden services within Tor in the [https://github.com/bitcoin/bitcoin/blob/master/doc/tor.md documentation].

== Nodes list ==

=== IPv4 Nodes ===

This entire list was last checked on 2017-11-15.

{|class="wikitable sortable"
! Hostname !! Owner !! IP !! Fixed !! Status !! Last Seen (GMT) !! Accepts IP transactions
|-
<!-- BEGIN NODELIST -->
|-
| bitcoin.moneypot.com || [https://www.moneypot.com moneypot] || 212.47.228.216 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} || 2015-09-15 || No
|-
| node.bitcoin.xxx || [http://www.bitcoin.xxx Bitcoin.xxx] || 66.228.49.201 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} || 2014-08-28 || Yes
|-
| bitcoin.coinprism.com || [[Coinprism]] || 137.116.225.142 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} || 2014-04-26 || Yes
|-
| btcnode1.evolyn.net || Evolyn || 85.214.251.25 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} || 2014-01-26 || Yes
|-
| InductiveSoul.US || [[User:Inductivesoul|Inductive Soul]] || 67.186.224.85 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} || 2013-11-13 || Yes
|-
| archivum.info || Ferraro Ltd.|| 88.198.58.172 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| 62.75.216.13 || exMULTI, Inc. || 62.75.216.13 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || No
|-
| 69.64.34.118 || exMULTI, Inc. || 69.64.34.118 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || No
|-
| 79.160.221.140 || K-Norway || 79.160.221.140 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| netzbasis.de || unknown3 || 81.169.129.25 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| btc.turboadmin.com || osmosis || 98.143.152.14 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || No
|-
| fallback.bitcoin.zhoutong.com || Zhou Tong || 117.121.241.140 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || No
|-
| bauhaus.csail.mit.edu || imsaguy || 128.30.96.44 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| jun.dashjr.org || Luke-Jr || 173.242.112.53 || {{Table Value Yes}} || {{Fallback Nodes/Node Up}} || 2017-11-15 || 
|-
| cheaperinbitcoins.com || Xenland/Shane || 184.154.36.82 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| django.webflows.fr || unknown2 || 188.165.213.169 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| 204.9.55.71 || toasty || 204.9.55.71 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| btcnode.novit.ro || ovidiusoft - novit.ro || 93.187.142.114 || {{Table Value No}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| porgressbar.sk || progressbar hackerspace || 91.210.181.21 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| faucet.bitcoin.st || bitcoin street || 64.27.57.225 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| bitcoin.securepayment.cc || SecurePayment CC || 63.247.147.163 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| www.dcscdn.com || [[User:Danw12|Danw12]] || 199.115.228.181 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  ||
|-
| ns2.dcscdn.com || [[User:Danw12|Danw12]] || 199.115.228.182 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  ||
|-
| coin.soul-dev.com || Soul-Dev || || || {{Fallback Nodes/Node Down}} ||  ||
|-
| messier.bzfx.net || BZFX/[[User:A Meteorite|A Meteorite]] || 91.121.205.50 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  ||
|-
| btcnode1.bitgroup.cc || BitGroup || 198.211.116.191 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| btcnode2.bitgroup.cc || BitGroup || 162.243.120.138 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| btcnode3.bitgroup.cc || BitGroup || 95.85.8.237 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| btcnode.xiro.co || Xiro Labs || 91.121.108.61 || {{Table Value Yes}} || {{Fallback Nodes/Node Up}} || 2017-11-15 || No
|-
| stuff.liam-w.io || [[User:liamwli|Liam W]] || 185.122.57.203 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || No
|-
| bitcoin.bitdonut.co || James Hartig ||  ||  || {{Fallback Nodes/Node Down}} ||  ||
|-
| coinno.de  || jaknam ||  ||  || {{Fallback Nodes/Node Down}} ||  ||
|-
| 82.165.44.44 || anonymous ||  ||  || {{Fallback Nodes/Node Down}} ||  ||
|-
| bitcoin1.dassori.me || gdassori ||  ||  || {{Fallback Nodes/Node Down}} ||  ||
|-
| bitcoin2.dassori.me || gdassori ||  ||  || {{Fallback Nodes/Node Down}} ||  ||
|-
| blockchainnode.meulie.net || [[User:Evert|Evert]] ||  ||  || {{Fallback Nodes/Node Up}} || 2017-11-15 ||
|-
| fullnode.fybsg.com || Nagato ||  ||  || {{Fallback Nodes/Node Down}} ||  ||
|-
| n.bitcoin-fr.io || [[User:Arthur|Arthur]] || 62.210.66.227 ||  || {{Fallback Nodes/Node Up}} || 2017-11-15 ||
|-
| homeplex.tk || [[User:Victorsueca|Victorsueca]] || 90.165.120.190 ||  || {{Fallback Nodes/Node Down}} ||  ||
|-
| mars.jordandoyle.uk || [https://doyle.wf Jordan Doyle] || 91.121.83.82 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
<!-- END NODELIST -->
|}

=== IPv6 Nodes ===
{|class="wikitable sortable"
! Hostname !! Owner !! IP !! Fixed !! Status !! Last Seen (GMT) !! Accepts IP transactions
|-
<!-- BEGIN NODELIST -->
| InductiveSoul.US || [[User:Inductivesoul|Inductive Soul]] || 2601:7:6680:2ac:4d29:40ff:7513:fcc7 || {{Table Value Yes}} || {{Fallback Nodes/Node Up}} || 11-13-2013 (MDY) || Yes
|-
| caffeinator.net || [[User:Atrophy|Atrophy]] ||  ||  || {{Fallback Nodes/Node Up}} || 2013-05-10 ||
|-
| 2001:470:8:2e1::40 || ? || 2001:470:8:2e1::40 || {{Table Value Yes}} || {{Fallback Nodes/Node Down}} ||  || Yes
|-
| messier.bzfx.net || BZFX/[[User:A Meteorite|A Meteorite]] || 2001:41d0:1:d632::1 || {{Table Value Yes}} || {{Fallback Nodes/Node Up}} ||  ||
|-
| stuff.liam-w.io || [[User:liamwli|Liam W]] || 2a06:8ec0:3::1:2e47 || {{Table Value Yes}} || {{Fallback Nodes/Node Up}} ||  ||  No
|-
| bitcoin.bitdonut.co || James Hartig ||  ||  ||  ||  ||
|-
| n.bitcoin-fr.io || [[User:Arthur|Arthur]] || 2001:bc8:c087:2001::1 ||  ||  ||  ||
|-
| mars.jordandoyle.uk || [https://doyle.wf Jordan Doyle] ||  ||  ||  ||  ||
<!-- END NODELIST -->
|}

=== Tor nodes ===

This entire list was last checked on 2018-10-24.

{|class="wikitable sortable"
! Hostname !! Owner !! Status !! Last Seen (GMT) !! Accepts IP transactions
|-
| nkf5e6b7pl4jfd4a.onion || BlueMatt || Up || 2018-10-24 || No
|}

== Adding a node ==

Before adding yourself as a fallback node, you should be sure your node will stay online for a long time. If a node is offline for more than 24 hours it will be removed from the list.

To add a node in this list, you just need the ip/hostname and your name, the other fields will be filled automatically. Insert the following lines before the <tt>END NODELIST</tt> line:

 <nowiki>|-
| ip || your name</nowiki>

==See Also==

* [[Network|Bitcoin Network]]
* [http://nodes.bitcoin.st Fallback Nodes] List of longest running Bitcoin Nodes listed by Country.
* [https://getaddr.bitnodes.io/ Bitnodes project]
* [https://blockchain.info/connected-nodes Recently connected nodes at blockchain.info]

{{Bitcoin Core documentation}}
