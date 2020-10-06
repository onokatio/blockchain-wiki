---
title: "Payment_channels"
date: 2020-10-07T00:31:19+09:00
draft: true
---

A '''Micropayment Channel''' or '''Payment Channel''' is class of techniques designed to allow users to make multiple Bitcoin transactions without commiting all of the transactions to the Bitcoin block chain.<ref name="bitcoinorg_dev_guide"/>  In a typical payment channel, only two transactions are added to the block chain but an unlimited or nearly unlimted number of payments can be made between the participants.

Several channel designs have been proposed or implemented over the years.  This article describes some of them.  Many designs are vulnerable to [[Transaction Malleability|transaction malleability]].  Specifically, many designs require a way to be able to spend an unsigned transaction, in order to ensure that the channel can be opened atomically.  Thus, these designs require a malleability fix that separates the signatures from the part of the transaction that is hashed to form the txid.

=== Nakamoto high-frequency transactions ===

Implemented in Bitcoin 0.1 were features such as [[Transaction Replacement|transaction replacement]]<ref name="replacement_in_original_code" />, input sequence numbers (nSequence), and [[nLockTime]] that would allow two or more parties to repeatedly update the state of an unconfirmed transaction prior to it becoming confirmed.

Satoshi Nakamoto described the technique to a Bitcoin developer in a personal email:<ref name="hearn_hft_quote" />

<blockquote>
An unrecorded open transaction can keep being replaced until nLockTime.  It may contain payments by multiple parties.  Each input owner signs their input.  For a new version to be written, each must sign a higher sequence number (see IsNewerThan).  By signing, an input owner says "I agree to put my money in, if everyone puts their money in and the outputs are this." There are other options in SignatureHash such as SIGHASH_SINGLE which means "I agree, as long as this one output (i.e. mine) is what I want, I don't care what you do with the other outputs.".  If that's written with a high nSequenceNumber, the party can bow out of the negotiation except for that one stipulation, or sign SIGHASH_NONE and bow out completely.  

The parties could create a pre-agreed default option by creating a higher nSequenceNumber tx using OP_CHECKMULTISIG that requires a subset of parties to sign to complete the signature.  The parties hold this tx in reserve and if need be, pass it around until it has enough signatures.

One use of nLockTime is high frequency trades between a set of parties.  They can keep updating a tx by unanimous agreement.  The party giving money would be the first to sign the next version.  If one party stops agreeing to changes, then the last state will be recorded at nLockTime.  If desired, a default transaction can be prepared after each version so n-1 parties can push an unresponsive party out.  Intermediate transactions do not need to be broadcast.  Only the final outcome gets recorded by the network.  Just before nLockTime, the parties and a few witness nodes broadcast the highest sequence tx they saw.
</blockquote>

This design was not secure:{{citation needed}} one party could collude with a miner to commit a non-final version of the transaction, possibly stealing funds from the other party or parties.

=== Spillman-style payment channels===

Discussed on the bitcoin-development mailing list<ref name="spillman_channel_description"/> and implemented in BitcoinJ,<ref name="channels_bitcoinj"/> this one transaction to create a secured deposit and a second transaction to release the deposit funds in the manner agreed to by both parties, preventing miners from being able to commit a non-final version of the transaction.  However, opening a channel in the Spillman model exposed the depositor to malleability risk where the counter party would be able to hold the depositor's funds hostage.<ref name="bip65"/>

A full description of the protocol is in [[Contract#Example_7:_Rapidly-adjusted_.28micro.29payments_to_a_pre-determined_party|example 7 of the Contract page]].

Spillman payment channels are unidirectional (there is a payer and a payee, and it is not possible to transfer money back in the reverse direction).

Spillman payment channels expire after a specific time, and the receiver needs to close the channel before the expiration.

=== CLTV-style payment channels ===

Were made possible in Decemember 2015 by the activation of the CLTV soft fork{{citation needed}} after discussion that began in the #bitcoin-wizards IRC channel{{citation needed}}, moved to the bitcoin-development and bitcoin-dev mailing lists<ref name="bitcoin_dev_bip65"/>, and included a design specification in [https://github.com/bitcoin/bips/blob/master/bip-0065.mediawiki BIP65].  Channels constructed using the new OP_CLTV opcode were resistant to the malleability problem inherent in the Spillman-style construction.<ref name="bip65" />

Like Spillman payment channels, CLTV-style payment channels are unidirectional and expire after a specific time.

=== Poon-Dryja payment channels ===

Poon-Dryja payment channels were presented in the paper<ref name="ln_pdf">[https://lightning.network/lightning-network-paper.pdf Lightning Network paper, v0.5.9.1]<br>Joseph Poon & Thaddeus Dryja<br>''Retrieved 2016-04-10''</ref> that also introduced the [[Lightning Network]].  Channel backing funds are locked into a 2-of-2 multisig, but before the funding transaction is even signed, commitment transactions for each party are first written and signed.  As it requires referring to transactions that have not been signed yet, it requires using a transaction format that separates signatures from the part of the transaction that is hashed to generate the txid, such as [[Segregated Witness]].

Poon-Dryja channels may be closed unilaterally (requires the participation of only one party) or bilaterally (requires the participation of both parties).  When closed bilaterally Poon-Dryja channels are indistinguishable on-chain from 2-of-2 multisig address spends.  When closed unilaterally, the funds of the party that closed the channel is temporarily timelocked; this allows the other party to dispute the state transmitted by the closing party (who might have given old state on closing).

Poon-Dryja payment channels have indefinite lifetime.  They are also bidirectional, unlike Spilman and CLTV payment channels.

The [https://github.com/lightningnetwork/lightning-rfc/blob/master/03-transactions.md Lightning BOLT] specifications include recommended implementation of Poon-Dryja payment channels.

=== Decker-Wattenhofer duplex payment channels ===

Duplex payment channels were presented in a paper<ref name='duplex_pdf'>[https://tik-old.ee.ethz.ch/file/716b955c130e6c703fac336ea17b1670/duplex-micropayment-channels.pdf A Fast and Scalable Payment Network with Bitcoin Duplex Micropayment Channels] Decker, C.; Wattenhofer, R.</ref> by Christian Decker and Roger Wattenhofer.  This type of payment channel requires the new BIP68<ref name='bip68'>[https://github.com/bitcoin/bips/blob/master/bip-0068.mediawiki BIP68]</ref> meaning of nSequence.  As the name implies, a duplex payment channel is composed of two unidirectional payment channels, one in both directions.  The unidirectional payment channels are essentially Spillman channels, but using relative lock time (nSequence) instead of nLockTime.

However, instead of funding unidirectional payment channels directly from an on-chain funding transaction, there is an "invalidation tree" of off-chain transactions between the funding transaction and the payment channel finalization transactions.  The invalidation tree transactions also use relative lock time; the first version of the transaction has a large relative lock time, and the next version of the transaction (which ''invalidates'' the first) uses a slightly smaller relative lock time, and so on.  There is also a "kick-off" transaction that starts the timeout for the relative locktime.  The sequence of transactions is thus: funding -> kickoff -> invalidation tree -> payment channel.

Initially, the invalidation transaction may have a relative lock time of 100 days, and then its outputs go to two unidirectional payment channels, one in either direction.  Both parties may then use the payment channels until one channel is exhausted.  The parties may then reset the payment channels, creating a new invalidation transaction with a relative lock time of 99 days that redistributes the money correctly, but with the unidirectional payment channels reset.

The payment channel may be closed at any time by either party (without the help of the other) by broadcasting the kickoff transaction on the blockchain.  In case of such a unilateral close, both parties must wait out the relative lock times until they can broadcast the payment channel finalization transactions.

Parties should prefer the bilateral (cooperative) close, which collapses the kickoff, invalidation tree, and payment channel transactions into a single simple transaction that pays out the funds to both parties.

Duplex payment channels have indefinite lifetime, but there is a limit on number of updates possible due to the invalidation tree.  This limit can be multiplied by adding additional layers to the invalidation tree, with resetting of the lower layers.  Finally, in case of a unilateral close duplex payment channels require significant wait times and significant number of transactions to be published on the blockchain.

The invalidation tree structure may actually have more than two participants; it would be possible for groups of 3 or more parties to build multiple payment channels between them that are backed by this invalidation tree structure, and to rebalance their payment channels without hitting the blockchain.<ref name='scaling_funding_pdf'>[https://www.tik.ee.ethz.ch/file/a20a865ce40d40c8f942cf206a7cba96/Scalable_Funding_Of_Blockchain_Micropayment_Networks%20(1).pdf Scalable Funding of Bitcoin Micropayment Channel Networks]</ref>  It would also be possible for the invalidation tree structure to fund Poon-Dryja rather than pairs of unidirectional payment channels.

=== Decker-Russell-Osuntokun eltoo Channels ===

Eltoo channels were presented in a paper<ref name='eltoo_pdf'>[https://blockstream.com/eltoo.pdf eltoo: A Simple Layer2 Protocol for Bitcoin] Decker, C.; Russell, R., Osuntokun, O. '''Retrieved 2018-05-02'''</ref> by Christian Decker, Rusty Russell, and Olauluwa Osuntokun in April 30, 2018. This type of channel requires a new <code>SIGHASH_NOINPUT</code><ref name='bip_sighash_noinput'>[https://github.com/cdecker/bips/blob/noinput/bip-xyz.mediawiki <code>SIGHASH_NOINPUT</code>]</ref> signing flag.

Roughly, each update of the channel requires creating two transactions: an update transaction and a CSV-encumbered settlement transaction that spends the update transaction. Via <code>SIGHASH_NOINPUT</code> and an unusual use of <code>OP_CHECKLOCKTIMEVERIFY</code>, any later update transaction can spend any earlier update transaction during the time that the settlement transaction is encumbered; only the latest update transaction cannot be spent by another update transaction (since it has no later update) and can only be spent by its corresponding settlement transaction after the CSV delay has passed.  Thus even if an attacker or disrupter publishes an old update transaction, it cannot get the old state published since the latest transaction can still be published while the CSV-delay on the old settlement transaction hasn't completed yet.

The <code>OP_CHECKLOCKTIMEVERIFY</code> is not used to enforce a particular future time, but rather to enforce an ordering on the update transactions, such that a later update transaction can spend an earlier update transaction, but not vice versa.  This is because <code>OP_CHECKLOCKTIMEVERIFY</code> ensures that the spending transaction has a <code>nLockTime</code> equal to, '''or greater than''', the value specified in the spent output SCRIPT.  Each update transaction's output SCRIPT has an "update" branch that allows any update transaction to spend it, as long as that update transaction has a higher <code>nLockTime</code> than it currently has.  We use past <code>nLockTime</code> Unix timestamps, which start at 500,000,000 and reach up to about ~1,500,000,000 (somewhat less than the Unix timestamp as of time of writing), giving a limit of about 1,000,000,000 (1 billion) updates.  If an old update transaction is spent, it will allow any later update transaction to spend it, thus preventing publication of old settlement transactions.

This has the advantage of not requiring a punishment branch (unlike Poon-Dryja), simplifying watchtower design and reducing the punishment of accidentally using old state (which would lead to total loss of funds under Poon-Dryja, but will only lead to wasting of fees in Decker-Russell-Osuntokun).  However, it requires a new feature in the base blockchain (<code>SIGHASH_NOINPUT</code>).

=== Hashed Time-Locked Contracts (HTLCs) ===

''Full article: [[Hashed Timelock Contracts]]''

A technique that can allow payments to be securely routed across multiple payment channels.{{citation needed}} For example, if Alice has a channel open to Bob and Bob has a channel open to Charlie, Alice can use a HTLC to pay Charlie through Bob without any risk of Bob stealing the payment in transit.  HTLCs are integral to the design of more advanced payment channels such as those used by the [[Lightning Network]].

=== References ===
<references>

<ref name="bitcoinorg_dev_guide">[https://bitcoin.org/en/developer-guide#micropayment-channel Micropayment channel], Bitcoin.org Developer Guide</ref>

<ref name="replacement_in_original_code">[https://github.com/trottier/original-bitcoin/blob/master/src/main.cpp#L434 Replacement in original Bitcoin code]<br>Satoshi Nakamoto, GitHub</ref>

<ref name="hearn_hft_quote">[https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2013-April/002417.html Anti DoS for tx replacement], Mike Hearn, bitcoin-development mailing list, 17 April 2013</ref>

<ref name="spillman_channel_description">[https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2013-April/002433.html Re: Anti DoS for tx replacement], Jeremy Spillman, bitcoin-development mailing list, 20 April 2013</ref>

<ref name="channels_bitcoinj">[https://bitcointalk.org/index.php?topic=244656.0 Micro-payment channels implementation now in bitcoinj], Mike Hearn & Matt Corallo, BitcoinTalk.org, 27 June 2013</ref>

<ref name="bip65">[https://github.com/bitcoin/bips/blob/master/bip-0065.mediawiki BIP65], Peter Todd, BIPs repository, retrieved 28 October 2016</ref>

<ref name="bitcoin_dev_bip65">[https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2015-September/011197.html [bitcoin-dev] Let's deploy BIP65 CHECKLOCKTIMEVERIFY!], Peter Todd, bitcoin-dev mailing list, 27 September 2015</ref>
</references>

[[Category:Technical]]
