---
title: "オフチェーントランザクション"
date: 2020-10-07T00:31:00+09:00
draft: true
---

An off-chain transaction is the movement of value outside of the [[block chain]]. While an [[Transactions|on-chain transaction]] - usually referred to as simply 'a transaction' - modifies the blockchain and depends on the blockchain to determine its validity an off-chain transaction relies on other methods to record and validate the transaction. Like on-chain transactions all parties must agree to accept the particular method by which the transaction occurs, the question then being, how can those parties be convinced that the movement of value has actually happened, will not be reversed, and can be exchanged in the future for something of value?

With an on-chain transaction those questions are answered by the parties faith in the Bitcoin system as a whole. For instance a transaction (after some number of [[Confirmation|confirmations]]) can only be reversed if a majority of hashing power agrees to reverse the transaction. The parties to the transaction are trusting that the majority of hashing power in existence is controlled by "honest" parties who will not attempt to reverse the transaction.

== Rationale ==

On-chain transactions have disadvantages that make them unsuitable for some applications:

=== Speed ===

On-chain transactions take some time to accumulate enough [[Confirmation|confirmations]] to ensure that they can-not be reversed; accepting a transaction without any confirmations is potentially risky. Confirmations take time and the time they take to accumulate is random. Off-chain transaction systems can record that a transaction has happened immediately, and, subject to the guarantees of the system itself, immediately guarantee it won't be reversed.

=== Privacy/Anonymity ===

All on-chain transactions are recorded publicly on the block chain; Bitcoin transactions are not inherently [[Anonymity|anonymous]]. It may be possible for a third-party to use the block chain transaction data to determine the source and/or destination of a transaction if they can gather enough information linking addresses to identities. Because off-chain transactions do not happen on the block chain they need not be public. Using cryptographic techniques such as [http://en.wikipedia.org/wiki/Blind_signature chaum tokens] it can be made impossible for even the operators of the system itself to determine who participated in a transaction.

=== Cost/Scalability ===

Miners usually charge [[Transaction fees|fees]] to confirm a transaction. While currently the demand for transactions is sufficiently low that fees are relatively small, and transactions can often be confirmed for free, for many applications even paying a few cents per transaction is unaffordable.<ref>[https://bitcointalk.org/index.php?topic=156334.0 How to send Bitcoins with LOW TX FEE (Not No TX Fee)]</ref> In addition Bitcoin currently has a limit of 7 transactions per second, the [[blocksize limit]]. This limit is related to the [[Scalability|scalability]] of the system as a whole, and one option to achieve higher transaction volumes is to keep the blocksize limit as is and use off-chain transactions for lower-value transactions; with higher volumes fees for transactions done on-chain will rise due to supply and demand.

== Methods ==

=== Payment Channels ===

The most promising by far method of building a off-chain transaction system is [[Lightning Network]]. It is a proposed implementation of [[Hashed Timelock Contracts]] (HTLCs) with bi-directional [[payment channels]] which allows payments to be securely routed across multiple peer-to-peer payment channels. This allows the formation of a network where any peer on the network can pay any other peer even if they don't directly have a channel open between each other. Very little third-party trust is required.

Main article: [[Lightning Network]]

=== Sidechains ===

Another potential technology for off-chain transaction is [[sidechain]]s, which is where bitcoins are moved onto another blockchain which can support transactions with different properties to bitcoin's blockchain.

=== Credit-Based Solutions ===

The most simple example of an off-chain transaction is perhaps two friends who agree on a debt between them. The "transaction" happens by the act of agreeing that the debt exists, and the validity of it is based solely on the trust that one friend has in the other. Further transactions can be agreed upon, possibly in exchange for something of value such one friend buying the other a meal. Multiple mutually trusting parties can participate, creating a network of value owed from one to the other. As an example the [http://en.wikipedia.org/wiki/Ripple_monetary_system Ripple monetary system] takes this concept, and adds to it an automated ledger to record all the mutual debts between participating parties. However actually acting upon those debts is still a matter of trust between the parties; the system only records debts and can-not by itself cause Bitcoins or some other object of value to change hands. In theory, the use of multi-signature techniques offers the promise of secure Off-Chain transactions <ref>[http://www.bincoin.com/cryptocubic.pdf A CryptoCubic Protocol for Hacker-Proof Off-Chain Bitcoin Transactions]</ref>. However, the practical applications of such "CryptoCubic" approaches have yet to be confirmed.

==== Trusted Third Parties ====

If the sender and recipient do not trust each other, or would simply prefer someone else record and guarantee the transaction, they can use a [http://en.wikipedia.org/wiki/Trusted_third_party trusted third party] to record and guarantee the transaction. The vast majority of conventional banking and electronic payment systems work this way. For instance in the PayPal system, PayPal is trusted to keep an accurate record of all transactions, including within the PayPal system, as well as transactions that move funds to and from PayPal. Within Bitcoin [[Redeemable_code|redeemable code]] systems exist where a third party, such as Mt. Gox, records codes issued and promise to redeem them for either new codes, balances within the system, or Bitcoins via on-chain transactions. In addition [[E-Wallet]] services such as [[Easywallet.org]] often allow users to transfer funds between addresses within the system without creating an on-chain transaction.

The difficulty with third-parties is achieving that trust. Outside of Bitcoin PayPal has been criticized<ref>[http://en.wikipedia.org/wiki/PayPal#Criticism PayPal - Criticism]</ref> for arbitrarily freezing accounts. Within Bitcoin multiple E-Wallet services such as [[MyBitcoin]] and [[Instawallet]] have failed due to hacks as well as technical mistakes resulting in the loss of some or all funds held on behalf of their customers.

==== Auditing ====

In addition to hacks, currently no trusted third party payment systems in Bitcoin provide any way for users to determine if the services actually hold the Bitcoins they claim to hold. Conventionally banks and payment processors are [http://en.wikipedia.org/wiki/Financial_audit audited] regularly by third-parties - because Bitcoin is based on cryptography auditing can be done in a cryptographically provable way.

Gregory Maxwell has proposed<ref>private communication on IRC (gmaxwell: do you have a writeup somewhere?)</ref> to use [[merkle-sum trees]] of accounts to audit funds held by third parties. Each account with the service is assigned a number, such as a SHA256 digest, and those digests are formed into a merkle tree. Additionally for every node in the tree the sum of the account balances on both leaves is computed, and that sum becomes part of the data hashed by the parent node. The tip of the tree is then the sum of all balances for all accounts.

The service proves they control the Bitcoins they claim to by signing statements with the private keys capable of spending transaction outputs present on the blockchain, and in addition regularly sign statements attesting what is the current tip of the account merkle-sum tree.

Clients check that their account is included in that tree by regularly demanding proof, in the form of a merkle path, that their account leads to the claimed tip. Any discrepancy is evidence of fraud on by the service, or at least poor record-keeping.

==== Proving Fraud ====

If the communication protocol between client and service is designed correctly fraud by the service can be proven to others. For instance if the service cryptographically signs all communications an inconsistency between the claimed merkle-tip of the accounts held by the service and the merkle-path from a particular account to that tip can be proven by providing the signed tip, and the signed merkle-path. This fraud proof can be self-authenticating, and thus anyone who comes in possession of such a proof can broadcast it to their peers. With appropriate software all participating clients of the service can be informed of any fraud immediately taking "advantage of the nature of information being easy to spread but hard to stifle" - a core concept underlying the security of Bitcoin itself.<ref>[http://sourceforge.net/mailarchive/message.php?msg_id=30482839 Satoshi]</ref>

===Hardware-Based===

* [https://opendime.com OpenDime]
* [https://tangem.com Tangem SmartNotes]

== References ==

<references/>

[[Category:Technical]]
[[Category:Scalability]]
[[Category:Privacy]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020
This page content is translated by Bitcoin wiki. original right is reserved by them.
