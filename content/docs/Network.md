---
title: "ネットワーク"
date: 2020-10-06T20:07:35+09:00
draft: true
---

Bitcoin uses a simple broadcast network to propagate transactions and blocks.
All communications are done over TCP. Bitcoin is fully able to use ports other
than 8333 via the -port parameter. IPv6 is
[https://bitcointalk.org/index.php?topic=81378.0 supported] with
Bitcoind/Bitcoin-Qt v0.7. Using bitcoin over [[tor]] is also supported.

== Messages ==

- ''version'' - Information about program version and block count. Exchanged
  when first connecting.
- ''verack'' - Sent in response to a version message to acknowledge that we are
  willing to connect.
- ''addr'' - List of one or more IP addresses and ports.
- ''inv'' - "I have these blocks/transactions: ..." Normally sent only when a
  ''new'' block or transaction is being relayed. This is only a list, not the
  actual data.
- ''getdata'' - Request a single block or transaction by hash.
- ''getblocks'' - Request an ''inv'' of all blocks in a range.
- ''getheaders'' - Request a ''headers'' message containing all block headers in
  a range.
- ''tx'' - Send a transaction. This is sent only in response to a ''getdata''
  request.
- ''block'' - Send a block. This is sent only in response to a ''getdata''
  request.
- ''headers'' - Send up to 2,000 block headers. Non-generators can download the
  headers of blocks instead of entire blocks.
- ''getaddr'' - Request an ''addr'' message containing a bunch of known-active
  peers (for bootstrapping).
- ''submitorder'', ''checkorder'', and ''reply'' - Used when performing an [[IP
  address|IP transaction]].
- ''alert'' - Send a network alert.
- ''ping'' - Does nothing. Used to check that the connection is still online. A
  TCP error will occur if the connection has died.

More information and in-depth technical information is in the [[Protocol
Specification]].

== Connection ==

To connect to a peer, you send a ''version'' message containing your version
number, block count, and current time. The remote peer will send back a
''verack'' message and his own ''version'' message if he is accepting
connections from your version. You will respond with your own ''verack'' if you
are accepting connections from his version.

The time data from all of your peers is collected, and the median is used by
Bitcoin for all network tasks that use the time (except for other version
messages).

You then exchange ''getaddr'' and ''addr'' messages, storing all addresses that
you don't know about. ''addr'' messages often contain only one address, but
sometimes contain up to 1000. This is most common at the beginning of an
exchange.

== Standard relaying ==

When someone sends a transaction, they send an ''inv'' message containing it to
all of their peers. Their peers will request the full transaction with
''getdata''. If they consider the transaction valid after receiving it, they
will also broadcast the transaction to all of their peers with an ''inv'', and
so on. Peers ask for or relay transactions only if they don't already have them.
A peer will never rebroadcast a transaction that it already knows about, though
transactions will eventually be forgotten if they don't get into a block after a
while. The sender and receiver of the transaction will rebroadcast, however.

Anyone who is generating will collect valid received transactions and work on
including them in a block. When someone does find a block, they send an ''inv''
containing it to all of their peers, as above. It works the same as
transactions.

Everyone broadcasts an ''addr'' containing their own IP address every 24 hours.
Nodes relay these messages to a couple of their peers and store the address if
it's new to them. Through this system, everyone has a reasonably clear picture
of which IPs are connected to the network at the moment. After connecting to the
network, you get added to everyone's address database almost instantly because
of your initial ''addr''.

Network alerts are broadcast with ''alert'' messages. No ''inv''-like system is
used; these contain the entire alert. If a received alert is valid (signed by
one of the people with the private key), it is relayed to all peers. For as long
as an alert is still in effect, it is rebroadcast at the start of every new
connection.

== Initial block download ==

At the start of a connection, you send a ''getblocks'' message containing the
hash of the latest block you know about. If the peer doesn't think that this is
the latest block, it will send an ''inv'' that contains up to 500 blocks ahead
of the one you listed. You will then request all of these blocks with
''getdata'', and the peer will send them to you with ''block'' messages. After
you have downloaded and processed all of these blocks, you will send another
''getblocks'', etc., until you have all of the blocks.

== Thin SPV Clients ==

[[BIP 0037]] introduced support for thin or lite clients by way of Simple
Payment Verification. SPV clients do not need to download the full block
contents to verify the existence of funds in the blockchain, but rely on the
chain of block headers and bloom filters to obtain the data they need from other
nodes. This method of client communication allows high security trustless
communication with full nodes, but at the expensive of some privacy as the peers
can deduce which addresses the SPV client is seeking information about.

[[MultiBit]] and [[Bitcoin Wallet]] work in this fashion using the library
[[bitcoinj]] as their foundation.

== Bootstrapping ==

You choose which peers to connect to by sorting your address database by the
time since you last saw the address and then adding a bit of randomization.

Bitcoin has three methods of finding peers.

=== Addr ===

The ''addr'' messages described above create an effect similar to the IRC
bootstrapping method. You know reasonably quickly whenever a peer joins, though
you won't know for a while when they leave.

Bitcoin comes with a list of addresses known as "seed nodes". If you are unable
to connect to IRC and you've never connected to the network before, the client
will update the address database by connecting to one of the nodes from this
list.

The -addnode command line option can be used to manually add a node. The
-connect option can force bitcoin to connect only to a specific node.

=== DNS ===

Bitcoin looks up the IP Addresses of several host names and adds those to the
list of potential addresses. This is the default seeding mechanism, as of v0.6.x
and later.

=== IRC ===

As-of version 0.6.x of the Bitcoin client IRC bootstrapping is no longer enabled
by default, and as of version 0.8.2 support for IRC bootstrapping has been
removed completely. The information below is accurate for most versions prior.

Bitcoin joins a random channel between #bitcoin00 and #bitcoin99 on
irc.lfnet.org. Your nick is set to an encoded form of your IP address. By
decoding all the nicks of all users on the channel, you get a list of all IP
addresses currently connected to Bitcoin.

For hosts that cannot make outbound connections on port 6667, the lfnet servers
are also [[FAQ#Do_I_need_to_configure_my_firewall_to_run_bitcoin?|listening on
port 7777]].

== Heartbeat ==

If thirty minutes or more has passed since the client has transmitted any
messages it will transmit a message to keep the connection to the peer node
alive.

If ninety minutes has passed since a peer node has communicated any messages,
then the client will assume that connection has closed.

== See Also ==

- [[Protocol Specification]]
- [[Satoshi Client Node Discovery]]
- [http://bitcoinstatus.rowit.co.uk/ Historical Network Status (no longer
  updated)]
- [http://getaddr.bitnodes.io/ Bitnodes.io's network size estimate]
- [http://fullnode.info/howto.html How to run your own cheap full bitcoin node]
- [https://www.bitcoinmining.com/ Bitcoin Mining]
- [https://www.youtube.com/watch?v=GmOzih6I1zs Video: What is Bitcoin Mining]

[[Category:Technical]]

[[pl:Sieć]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
