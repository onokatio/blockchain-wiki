---
title: "サイドチェーン"
date: 2020-10-06T20:09:48+09:00
draft: true
---

A ''sidechain'' or ''pegged sidechain'' enables bitcoins and other ledger assets
to be transferred between multiple blockchains. This gives users access to new
and innovative cryptocurrency systems using the assets they already own. By
reusing Bitcoin's currency, these systems can more easily interoperate with each
other and with Bitcoin, avoiding the liquidity shortages and market fluctuations
associated with new currencies. Since sidechains are separate systems, technical
and economic innovation is not hindered. Despite bidirectional transferability
between Bitcoin and pegged sidechains, they are isolated: in the case of a
cryptographic break (or malicious design) in a sidechain, the damage is entirely
confined to the sidechain itself.

== Discussion ==

A sidechain is a mechanism that allows you to move your bitcoins to another,
completely independent blockchain, trade them there, and they can then be moved
back to main bitcoin blockchain I will try to explain how it should work:

You may have already encountered the idea of a one-way peg. This means that you
destroy some BTC, and you gain some other currency for doing that. Example: when
Counterparty started, people could “burn” some of their BTC by sending them to
an unspendable address, and for doing it, they received newly created XCP
tokens. This is called a one-way peg, and it cannot be reversed. You can only
move the money one way, you cannot turn it back to BTC later.

Sidechains extends this idea, and creates a two-way peg, that lets you move it
back and forth. Instead of destroying BTC by making them unspendable, you lock
them in little boxes. These boxes do not belong to any address, they are instead
controlled by a bitcoin script. For each box you lock this way, you get newly
created tokens on the sidechain (which is another blockchain, complete separate
network). You can then give these tokens to someone, pay with them, and when he
wants to bring them back to the Bitcoin blockchain, he can just destroy the
tokens in the same way. Providing a cryptographic proof that he destroyed the
tokens will allow him to open a locked box and collect the Bitcoin.

So the value is pegged, because you can freely turn one into the other at any
time.

The purpose of this trick is to allow people to safely experiment with different
rules, networks and consensus mechanisms, that may be suitable for different
purposes, without putting the main Bitcoin network at risk. In other words, it
creates an area where you can bring some of the BTC you have, try some crazy
experimental stuff, and then bring them
back<ref>https://www.reddit.com/r/Bitcoin/comments/32w9he/eli5_how_do_sidechains_work/cqfb0te/</ref>.

== External links ==

- https://blockstream.com/sidechains.pdf

==References== <references />

[[Category:Technical]] [[Category:Scalability]] [[Category:Privacy]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
