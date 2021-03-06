---
title: "ライトニングネットワーク"
date: 2020-10-06T20:11:31+09:00
draft: true
---

'''Lightning Network''' is a proposed implementation of
[[Hashed Timelock Contracts]](HTLCs) with bi-directional [[payment channels]]
which allows payments to be securely routed across multiple peer-to-peer payment
channels.<ref name="ln_pdf">[https://lightning.network/lightning-network-paper.pdf
Lightning Network paper, v0.5.9.1]<br>Joseph Poon & Thaddeus
Dryja<br>''Retrieved 2016-04-10''</ref> This allows the formation of a network
where any peer on the network can pay any other peer even if they don't directly
have a channel open between each other. As of March 2019, there were more than
37,000 channels carrying more than 764
bitcoins.<ref name="p2sh_data">[https://p2sh.info/dashboard/db/lightning-network?orgId=1&from=1514764800000&to=1550915801588
Lightning Network - Sum of channels value]<br>Lightning Network - Sum of
channels value<br>''Retrieved 2019-03-08''</ref>

== Key features == Key features of the Lightning Network proposal include,

- '''Rapid payments:''' payments within an established channel can be made
  almost as fast as data can travel over the Internet between the two peers.
- '''No third-party trust:''' the two peers in a channel pay each other directly
  using regular Bitcoin transactions (of which only one is broadcast) so at no
  point does any third party control their funds.
- '''Reduced blockchain load:''' only channel open transactions, channel close
  transactions, and (hopefully infrequent) anti-fraud respends need to be
  committed to the blockchain, allowing all other payments within Lightning
  Network channels to remain uncommitted. This allows Lightning Network users to
  make frequent payments secured by Bitcoin without placing excessive load on
  full nodes which must process every transaction on the blockchain.
- '''Channels can stay open indefinitely:''' as long as the two parties in the
  channel continue to cooperate with each other, the channel can stay open
  indefinitely -- there is no mandatory timeout period. This can further reduce
  the load on the blockchain as well as allow the fees for opening and closing
  the channel to be amortized over a longer period of time.
- '''Rapid cooperative closes:''' if both parties cooperate, a channel can be
  closed immediately (with the parties likely wanting to wait for one or more
  confirmations to ensure the channel closed in the correct state).
  Non-cooperative closes (such as when one party disappears) are also possible
  but they take longer.
- '''Outsourceable enforcement:''' if one party closes a channel in an old state
  in an attempt to steal money, the other party has to act within a defined
  period of time to block the attempted theft. This function can be outsourced
  to a third-party without giving them control over any funds, allowing wallets
  to safely go offline for periods longer than the defined period.
- '''Onion-style routing:''' payment routing information can be encrypted in a
  nested fashion so that intermediary nodes only know who they received a
  routable payment from and who to send it to next, preventing those
  intermediary nodes from knowing who the originator or destination is (provided
  the intermediaries didn't compare records).
- '''Multisignature capable:''' each party can require that their payments into
  the channel be signed by multiple
  keys<ref name="poon_multisig">[https://lists.linuxfoundation.org/pipermail/lightning-dev/2016-January/000403.html
  2-of-3 Instant Escrow, or How to Do "2-of-3 Multisig Contract" Equivalent on
  Lightning]<br>Joseph Poon<br>''Retrieved 2016-04-11''</ref>, giving them
  access to additional security techniques.
- '''Securely cross blockchains:''' payments can be routed across more than one
  blockchain (including altcoins and sidechains) as long as all the chains
  support the same hash function to use for the hash lock, as well as the
  ability to create time locks.
- '''Sub-satoshi payments:''' payments can be made conditional upon the outcome
  of a random event, allowing probabilistic
  payments.<ref name="dryja_directed_graph"/> For example, Alice can pay Bob 0.1
  satoshi by creating a 1-satoshi payment with 10-to-1 odds so that 90% of the
  time she does this she pays him 0 satoshis and 10% of the time she pays him 1
  satoshi for an average payment of 0.1 satoshis.
- '''Single-funded channels:''' when Alice needs to send a payment to Bob and
  doesn't currently have a way to pay him through the Lightning Network (whether
  because she can't reach him or because she doesn't have enough money in an
  existing channel), she can make a regular on-chain payment that establishes a
  channel without Bob needing to add any of his funds to the channel. Alice only
  uses 12 bytes more than she would for a non-Lightning direct payment and Bob
  would only need about 25 more [[segwit]] virtual bytes to close the channel
  than he would had he received a non-Lightning direct
  payment.<ref name="dryja_directed_graph"/>

== Glossary ==

This section attempts to document the most frequently used terms found in
Lightning Network literature that may not be familiar to a general technical
audience, including both the new terms created by Lightning Network designers as
well as pre-existing terms that may not be well known from Bitcoin,
cryptography, network routing, and other fields.

The list below should be in alphabetical order. Any commonly-used synonyms or
analogs for a term are placed in parenthesis after the term.

- '''Bi-directional payment channel:'''<ref name="ln_pdf"/> a payment channel
  where payments can flow both directions, from Alice to Bob and back to Alice.
  This is contrasted with Spillman-style and CLTV-style payment channels where
  payments can only go one direction and once Alice has paid Bob all of the
  bitcoins she deposited in the channel funding transaction, the channel is no
  longer useful and so will be closed.
- '''Breach Remedy Transaction:'''<ref name="ln_pdf"/> the transaction Alice
  creates when Mallory attempts to steal her money by having an old version of
  the channel state committed to the blockchain. Alice's breach remedy
  transaction spends all the money that Mallory received but which Mallory can't
  spend yet because his unilateral spend is still locked by a relative locktime
  using <code>OP_CSV</code>. This is the third of the maximum of three on-chain
  transactions needed to maintain a Lightning channel; it only needs to be used
  in the case of attempted fraud (contract breach).
- '''Channel''' (Lightning channel<ref name="ln_pdf"/>, payment
  channel<ref name="spillman_channel">[https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2013-April/002433.html
  Anti DoS for tx replacement]<br>Jeremy Spillman<br>Retrieved 2016-04-17</ref>)
  a communication channel that allows two parties to make many secure payments
  between each other in exchange for making only a few transactions on the
  blockchain.
- '''Commitment Transaction:'''<ref name="ln_pdf"/> a transaction created
  collaboratively by Alice and Bob each time they update the state of the
  channel; it records their current balances within the channel. The '''Initial
  Commitment Transaction'''<ref name="ln_pdf"/> is the first of these
  transactions; it records the initial balances within the channel. This is the
  second of the maximum of three on-chain transactions needed to maintain a
  Lightning channel; it can be combined with a ''funding transaction'' for a new
  channel under the cooperative conditions necessary to create an ''exercise
  settlement transaction''.
- '''Contract:'''<ref name="szabo_smart_contracts">[http://szabo.best.vwh.net/smart_contracts_idea.html
  The Idea of Smart Contracts]<br>Nick Szabo<br>Retrieved 2016-04-17</ref> an
  agreement between two or more entities to use Bitcoin transactions in a
  certain way, usually a way that allows Bitcoin's automated consensus to
  enforce some or all terms in the contract. Often called a ''smart contract''.
- '''CSV:''' (<code>OP_CheckSequenceVerify</code>,
  <code>OP_CSV</code>)<ref name="bip68"/> a opcode that allows an output to
  conditionally specify how long it must be part of the blockchain before an
  input spending it may be added to the blockchain. See ''relative locktime.''
- '''Delivery Transaction:'''<ref name="ln_pdf"/> not really a transaction but
  rather the name for the outputs in the ''commitment transaction'' which Alice
  and Bob receive if one of them closes the channel unilaterally in the correct
  (current) state. If the channel is closed in an old state (indicating possible
  fraud), a ''breach remedy transaction'' will be generated from the output that
  would have paid the party closing the channel. If the channel is closed
  cooperatively, they'll create an ''exercise settlement transaction'' instead.
- '''Dispute period:''' (dispute resolution
  period<ref name="poon_time_bitcoin_ln">[https://lightning.network/lightning-network-presentation-time-2015-07-06.pdf
  Time, Bitcoin, and the Lightning Network]<br>Joseph Poon<br>Retrieved
  2016-04-17</ref>) the period of time that Alice has to get her ''breach remedy
  transaction'' added to the blockchain after Mallory has an old ''commitment
  transaction'' added to the blockchain. If the dispute period ends without a
  breach remedy transaction being added to the blockchain, Mallory can spend the
  funds he received from the old commitment transaction.
- '''Dual-funded
  channel:'''<ref name="dryja_directed_graph">[https://docs.google.com/presentation/d/1G4xchDGcO37DJ2lPC_XYyZIUkJc2khnLrCaZXgvDN0U/edit?pref=2&pli=1#slide=id.g85f425098_0_2
  LN as a Directed Graph: Single-Funded Channel Topology]<br>Thaddeus
  Dryja<br>Retrieved 2016-04-17</ref> a channel opened by a ''funding
  transaction'' containing inputs from both Alice and Bob. Compare to a
  ''single-funded channel'' where only Alice's inputs contribute to the balance
  of the channel.
- '''Encumbrance:'''<ref>[http://chimera.labs.oreilly.com/books/1234000001802/ch02.html
  Mastering Bitcoin, Chapter 2: How Bitcoin Works]<br>Andreas
  Antonopoulos<br>Retrieved 2016-04-17</ref> a generic name for any conditions
  that must be satisfied before a bitcoin output may be spent. Early Bitcoin
  transactions placed all their conditions in the scriptPubKey; later the
  introduction of P2SH allowed conditions to be added in a redeemScript which
  the scriptPubKey committed to; the introduction of soft fork [[segwit]] will
  add a similar mechanism for detached conditions that the scriptPubKey commits
  to; in addition, there are even more novel ways to add conditions to outputs
  that are discussed but rarely used. The term &quot;encumbrance&quot; allows
  specifying what the conditions do without fussing over exactly where the
  conditions appear in a serialized transaction.
- '''Exercise Settlement Transaction:'''<ref name="ln_pdf"/> a form of the
  ''commitment transaction'' created cooperatively by Alice and Bob when they
  want to close their channel together. Unlike a regular commitment transaction,
  none of the outputs on an exercise settlement transaction are time locked,
  allowing them to be immediately respent.
- '''Exhausted:'''<ref name="dryja_directed_graph"/> (exhausted channel) a
  payment channel where no additional payments can be made in one direction
  (such as from Alice to Bob). The person controlling the exhausted side of a
  Lightning channel loses nothing from fraudulently trying to commit an old
  channel state, so allowing a channel to become exhausted (or too near to being
  exhausted) is unpreferable. (Exception: channels can be securely started in an
  exhausted state, such as a ''single-funded channel.''
- '''Full push:'''<ref name="dryja_directed_graph"/> when Alice pays the full
  amount of the channel to Bob in the ''initial commitment transaction'', which
  ''exhausts'' the channel without incentivizing fraud because Alice doesn't
  have a previous ''commitment transaction'' that she can broadcast. This term
  is used in the context of a ''single-funded transaction'' and stands in
  contrast to an ''overpayment'' where Alice deposits more than she pays Bob in
  that initial payment so that she can continue to use the channel without
  needing to ''rebalance''.
- '''Funding Transaction:'''<ref name="ln_pdf"/> (deposit transaction) a
  transaction created collaboratively by Alice and Bob to open a Lightning
  channel. In a single-funded channel, Alice provides all the
  funding;<ref name="dryja_directed_graph"/> in a dual-funded channel, Alice and
  Bob both provide some funding. This is the first of the maximum of three
  on-chain transactions needed to maintain a Lightning channel; it can be
  combined with a commitment transaction from a previous channel being closed
  under cooperative conditions.
- '''Half-signed:'''<ref name="ln_pdf"/> a transaction input which requires two
  signatures to be added to the blockchain but which only has one signature
  attached. (More generally, this could be any input that has fewer signatures
  attached than it needs to be added to the blockchain.)
- '''Hash
  lock:'''<ref>[https://www.mail-archive.com/bitcoin-development@lists.sourceforge.net/msg05135.html
  BIP - Hash Locked Transaction]<br>Tier Nolan<br>Retrieved 2016-04-17</ref> an
  encumbrance to a transaction output that requires the pre-image used to
  generate a particular hash be provided in order to spend the output. In
  Lightning, this is used to allow payments to be routable without needing to
  trust the intermediaries.
- '''HTLC:''' (Hashed Timelocked
  Contract<ref name="russell_deployable_lightning">[https://github.com/ElementsProject/lightning/raw/master/doc/deployable-lightning.pdf
  Reaching the Ground with Lightning]<br>Rusty Russell<br>Retrieved
  2016-04-17</ref>) a contract such as that used in a Lightning Channel where
  both a ''hash lock'' and a ''time lock'' are used, the hash lock being used to
  allow Alice to route payments to Bob even through a Mallory that neither of
  them trust, and the time lock being used to prevent Mallory from stealing back
  any payments he made to Alice within the channel (provided Alice enforces the
  contract).
- '''Intermediary:'''<ref name="ln_pdf"/> When Bob has one channel open with
  Alice and another channel open with Charlie, Bob can serve as an intermediary
  for transferring payments between Alice and Charlie. With Lightning payments
  being secured with a ''hash lock,'' Bob can't steal the payment from Alice to
  Charlie when it travels through Bob's node. Lightning payments can securely
  travel through a theoretically unlimited number of intermediaries.
- '''Limbo channel:'''<ref name="dryja_directed_graph"/> an optional special
  state for a Lightning channel where it cannot be immediately closed by one or
  both of the parties unilaterally (it can still be immediately closed
  cooperatively). This is used in particular for ''PLIPPs.''
- '''Multisig:'''<ref name="bitcoin_0_1_code"/> (multisignature, m-of-n
  multisig) a transaction output that requires signatures from at least one of a
  set of two or more different private keys. Used in Lightning to give both
  Alice and Bob control over their individual funds within a channel by
  requiring both of them sign ''commitment transactions''.
- '''Node:''' (Lightning node<ref name="ln_pdf"/>) a wallet with one or more
  open Lightning channels. This should not be confused with a Bitcoin [[full
  node]] that validates Bitcoin blocks, although a full node's wallet may also
  be simultaneously used as a Lightning node to the advantage of the Lightning
  network user.
- '''Overfunding:'''<ref name="dryja_directed_graph"/> in a ''single-funded
  channel,'' Alice deposits more bitcoins into the channel than she pays Bob in
  the initial payment, allowing her to make additional payments through the
  Lightning network. This stands in contrast to a ''full push'' where Alice only
  deposits enough to pay Bob in the initial payment.
- '''PILPP:''' (Pre-Image Length Probabilistic
  Payments<ref name="dryja_directed_graph"/>) a specific type of ''probabilistic
  payment'' within a payment channel where Alice creates string with a random
  length and Bob guesses the length; if he guesses correctly, Alice has to pay
  him; if he guesses incorrectly, Alice gets to keep her money.
- '''Pre-image:'''<ref>[https://en.wikipedia.org/wiki/Image_%28mathematics%29#Inverse_image
  Image (mathematics)]<br>English Wikipedia contributors<br>Retrieved
  2016-04-17</ref> (R<ref name="ln_pdf"/>) data input into a hash function,
  which produces a hash of the pre-image. Inputting the same pre-image into the
  same hash function will always produce the same hash; Lightning uses this
  feature to create ''hash locks''.
- '''Probabilistic Payment:'''<ref>[[Nanopayments]]<br>Bitcoin Wiki
  contributors<br>Retrieved 2016-04-17</ref> a payment where Alice only pays Bob
  if some event outside of Alice's and Bob's control occurs in Bob's favor.
  Probabilistic payments are usually proposed for scenarios where payments can't
  conveniently be made small enough for technical reasons (such as not being
  able to pay less than 1 satoshi) or economic reasons (such as having to pay a
  transaction fee for every on-chain payment, making small payments
  uneconomical). See ''PLIPP'' for a specific type of probabilistic payment
  possible within a Lightning channel.
- '''R:''' the variable commonly used<ref name="ln_pdf"/> in formulas to
  represent a ''pre-image''.
- '''Rebalance:'''<ref>[https://web.archive.org/web/20160121161716/https://blockstream.com/2015/09/01/lightning-network/
  The Lightning Network: What Is It and What's Happening?]<br>Rusty
  Russell<br>Retrieved 2016-04-17</ref> a cooperative process between Alice and
  Bob when they adjust their balances within the channel. This happens with
  every payment in a Lightning channel and is only noteworthy because
  single-directional channels (such as Spillman-style and CLTV-style channels)
  are unable to rebalance and so must close as soon as Alices has paid Bob all
  the bitcoins she deposited into the channel. See ''bi-directional payment
  channels.''
- '''Relative locktime:'''<ref name="bip68"/> the ability to specify when a
  transaction output may be spent relative to the block that included that
  transaction output. Enabled by BIP68 and made scriptable by BIP112. Lightning
  uses relative locktime to ensure ''breach remedy transactions'' may be
  broadcast within a time period starting from when an old ''commitment
  transaction'' is added to the blockchain; by making this a relative locktime
  (instead of an absolute date or block height), Lightning channels don't have a
  hard deadline for when they need to close and so can stay open indefinitely as
  long as the participants continue to cooperate.
- '''Revocable Sequence Maturity Contract (RSMC):'''<ref name="ln_pdf"/> a
  ''contract'' used in Lightning to revoke the previous ''commitment
  transaction''. This is allowed through mutual consent in Lightning by both
  parties signing a new commitment transaction and releasing the data necessary
  to create ''breach remedy transactions'' for the previous commitment
  transaction. This property allows Lightning to support ''bi-directional
  payment channels'', recover from failed ''HTLC'' routing attempts without
  needing to commit to the blockchain, as well as provide advanced features such
  as ''PILPPs.''
- '''Single-funded channel:'''<ref name="dryja_directed_graph"/> a channel
  opened by a ''funding transaction'' containing only inputs from Alice. Compare
  to a ''dual-funded channel'' where Alice and Bob both contribute inputs to the
  initial balance of the channel.
- '''Timelock:'''<ref>[http://rusty.ozlabs.org/?p=450 Lightning Networks Part I:
  Revocable Transactions]<br>Rusty Russell<br>2016-04-17</ref> either an
  encumbrance to a transaction that prevents that transaction from being added
  to the blockchain before a particular time or block height (as is the case
  with [[nLockTime]], or an encumbrance that prevents a spend from a transaction
  output from being added to the blockchain before a particular time or block
  height (as is the case of OP_CLTV, consensus enforced sequence number relative
  locktime, and OP_CSV). In Lightning, this is used to prevent malicious
  intermediaries from holding other users' funds hostages as well as to allow
  victims of attempted theft to submit breach remedy transactions before the
  thief can respend the funds he stole.
- '''TTL:''' (Time To
  Live<ref>[http://lists.linuxfoundation.org/pipermail/lightning-dev/2015-July/000019.html
  Re: Routing on the Lightning Network?]<br>Rusty Russell<br>Retrieved
  2016-04-17</ref>) when Alice pays Bob with a ''hash locked'' in-channel
  payment that's ultimately intended for Charlie, she specifies how long Bob has
  to deliver the payment (its time to live) before the payment becomes invalid.
  When Bob pays Charlie with his own in-channel payment that has the same hash
  lock, Bob specifies a slightly shorter amount of time that Charlie has to
  reveal the pre-image that unlocks the hash lock before Bob's payment becomes
  invalid. This ensures that either Bob receives the data necessary to remove
  the hash lock from the payment he received from Alice or the payment he made
  to Charlie is invalidated; Alice gets the same guarantee that either the
  payment she made to Bob ultimate goes through to Charlie or her payment to Bob
  is invalidated.
- '''Unilateral:'''<ref name="ln_pdf"/> any action performed by only one of the
  participants in a channel without requesting or needing permission from the
  other participant. Lightning allows channels to be closed unilaterally (so
  Alice can close the channel by herself if Bob becomes unresponsive) and
  attempted fraud can be penalized unilaterally (so Alice can take any bitcoins
  Mallory tried to steal when he broadcast an old ''commitment transaction'').
- '''UTXO:'''<ref>[https://bitcoin.org/en/glossary/unspent-transaction-output
  Unspent Transaction Output (UTXO)]<br>Bitcoin.org Developer
  Glossary<br>Retrieved 2016-04-17</ref> (Unspent Transaction Output) spendable
  bitcoins. A transaction output lists a bitcoin amount and the conditions
  (called an ''encumbrance'') that need to be fulfilled in order to spend those
  bitcoins. Once those bitcoins have been spent on the blockchain, no other
  transaction in the same blockchain can spend the same bitcoins, so an Uspent
  Transaction Output (UTXO) is bitcoins that can be spent.

== See Also ==

- [[Payment channels]]
- [[Hashed Timelock Contracts]]
- [[Off-Chain Transactions]]
- [http://dev.lightning.community/resources/index.html Lightning Lab's list of
  resources]
- [https://github.com/dan-da/lightning-nodes/blob/master/README.md List of
  network nodes].
- [https://www.reddit.com/r/Bitcoin/comments/7pwna9/lightning_network_megathread/
  Lightning Network Megathread on Reddit]

== References == <references>
<ref name="bitcoin_0_1_code">[http://satoshi.nakamotoinstitute.org/code/ Bitcoin
0.1 code]<br>Satoshi Nakamoto<br>Retrieved 2016-04-11</ref>
<ref name="bip68">[https://github.com/bitcoin/bips/blob/master/bip-0068.mediawiki
BIP68: Relative lock-time using consensus-enforced sequence numbers]<br>Mark
Friedenbach, BtcDrak, Nicolas Dorier, and kinoshitajona<br>Retrieved
2016-04-12</ref> <references/>

[[Category:Technical]] [[Category:Scalability]] [[Category:Privacy]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
