---
title: "核戦争とビットコイン"
date: 2020-10-07T00:30:05+09:00
draft: true
---

A nuclear war would be problematic for Bitcoin in several ways, though maybe it
would not be the end of the world.**NOTOC**

===Attack scope===

It is first necessary to consider the scope of a nuclear attack. Some
possibilities are:

- Usage of tactical nuclear weapons, which are mainly just large bombs
- A targeted attack on a few cities, which will destroy those cities, but might
  not have much ''direct'' wider impact.
- An EMP-focused attack, in which a small number of nuclear weapons are
  detonated at high altitude in order to destroy a very large amount of
  electronic equipment.
- All-out war, in which the belligerent countries empty their reserves of
  nuclear weapons, and most primary and secondary targets within non-neutral
  countries are destroyed

A lot of people think that everyone is just going to quickly die in a nuclear
war, and so it's no use planning for post-war scenarios. This is wrong; in most
of the scenarios listed above, the vast majority of people would not be directly
harmed at all by the war. Even in the worst-case all-out-war scenario, probably
at least half of the affected populations would survive the blast and immediate
radiation, and could potentially live long lives if they take the correct
actions in the aftermath to avoid excessive fallout exposure.

===Bitcoin's reliance on the Internet===

For Bitcoin to function:

# All miners have to be connected in such a way that there is low latency between them. Miners that can't connect to the majority of mining power over a low-latency connection cannot mine.

# Anyone wishing to transact must be able to connect to the same Internet that the majority of miners are using ''at some point''. There are no latency requirements for sending or receiving transactions.

Many nuclear scenarios could damage cables and backbone routers, segmenting
parts of the Internet. In this case, it would be necessary for people wishing to
transact to find some way of communicating with the single segment containing
the most mining power, such as by using satellite communication, dial-up
connections, or even by writing transactions down on paper and moving them
physically. This would make Bitcoin more cumbersome to use -- perhaps too
cumbersome to be realistic during or immediately after a nuclear war --, but
still usable.

A likely scenario is that Bitcoin would become frozen and practically unusable -
but not destroyed - until the Internet was restored to roughly its former
capabilities.

===Destruction of mining hardware===

If a large amount of mining hardware is destroyed either directly by a nuclear
blast or by an EMP, [[difficulty]] could get stuck at a too-high level for an
extended period of time. The correct response to this is to [[hardfork]] to a
lower difficulty.

===Effects of EMPs===

When detonated, nuclear bombs release electromagnetic pulses (EMPs), which are
harmful to electronic equipment. A nuclear bomb meant to destroy a ground target
will only affect nearby areas with an EMP, but a mere handful of nuclear bombs
detonated in the air at strategic points could cause an entire continent to be
affected by EMPs.

EMPs are not dangerous to humans, but they have the following effects on
electronics:

- Anything connected to the power grid will be affected as if struck by
  lightning, but surge protectors will probably not provide protection.
- All semiconductors are likely to be destroyed. This includes CPUs and SSDs.
  Small semiconductors that are not connected to anything at the time of the EMP
  (eg. disconnected USB flash drives) ''may'' survive. The larger the item and
  connected wires/metal, the more it acts as an antenna for the EMP.
- Magnetic hard drives should preserve their data, though their controller chips
  are semiconductors which will probably be destroyed by the EMP. So they will
  probably require manual repairs before being operational post-EMP
- CDs/DVDs/BDs will be unaffected, as will paper.

An EMP strike could conceivably destroy almost all of a country's, continent's,
or the entire world's computers. Bitcoin requires computers to function, so a
widespread EMP strike would prevent Bitcoin from functioning in the affected
areas until the destroyed computers could be replaced. Even if the entire world
was affected, Bitcoin would be frozen with its pre-EMP ledger, not destroyed.

An EMP strike could also destroy private keys. You should have backups of your
private keys (or mnemonic) on paper or CD/DVD/BD, which are immune to EMPs.

The Bitcoin block chain is so widely distributed that someone will undoubtedly
have a safe copy of it somewhere.

[[Category:Technical]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
