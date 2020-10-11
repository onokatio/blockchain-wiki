---
title: "Raw Transactions API"
date: 2020-10-07T00:39:50+09:00
draft: true
---

== Overview ==

The "raw transaction API" was introduced with [[Bitcoind# History of official
bitcoind (and predecessor) releases | Bitcoin-Qt/bitcoind version 0.7]]. It
gives developers or very sophisticated end-users low-level access to transaction
creation and broadcast.

== JSON-RPC API == === listunspent [minconf=1][maxconf=999999] === Returns an
array of unspent transaction outputs in the wallet that have between minconf and
maxconf (inclusive) confirmations. Each output is a 5-element object with keys:
txid, output, scriptPubKey, amount, confirmations. txid is the hexadecimal
transaction id, output is which output of that transaction, scriptPubKey is the
hexadecimal-encoded CScript for that output, amount is the value of that output
and confirmations is the transaction's depth in the chain. === lockunspent
unlock? [{"txid":txid,"vout":n},...] === Temporarily lock (unlock=false) or
unlock (unlock=true) specified transaction outputs. A locked transaction output
will not be chosen by automatic coin selection, when spending bitcoins. Locks
are stored in memory only. Nodes start with zero locked outputs, and the locked
output list is always cleared (by virtue of process exit) when a node stops or
fails.

=== listlockunspent === List all temporarily locked transaction outputs. ===
createrawtransaction [{"txid":txid,"vout":n},...] {address:amount,...} ===
Create a transaction spending given inputs (array of objects containing
transaction outputs to spend), sending to given address(es). Returns the
hex-encoded transaction in a string. Note that the transaction's inputs are not
signed, and it is not stored in the wallet or transmitted to the network.

Also note that NO transaction validity checks are done; it is easy to create
invalid transactions or transactions that will not be relayed/mined by the
network because they contain insufficient fees. === decoderawtransaction
<hex string> === Returns JSON object with information about a serialized,
hex-encoded transaction. === getrawtransaction <txid> [verbose=0] === If
verbose=0, returns serialized, hex-encoded data for transaction txid. If verbose
is non-zero, returns a JSON Object containing information about the transaction.
Returns an error if <txid> is unknown. === signrawtransaction <hex string>
[{"txid":txid,"vout":n,"scriptPubKey":hex},...][<privatekey1>,...]
[sighash="ALL"] === Sign as many inputs as possible for raw transaction
(serialized, hex-encoded). The first argument may be several variations of the
same transaction concatenated together; signatures from all of them will be
combined together, along with signatures for keys in the local wallet. The
optional second argument is an array of parent transaction outputs, so you can
create a chain of raw transactions that depend on each other before sending them
to the network. Third optional argument is an array of base58-encoded private
keys that, if given, will be the only keys used to sign the transaction. The
fourth optional argument is a string that specifies how the [[OP
CHECKSIG|signature hash]] is computed, and can be "ALL", "NONE", "SINGLE",
"ALL|ANYONECANPAY", "NONE|ANYONECANPAY", or "SINGLE|ANYONECANPAY". Returns json
object with keys:

- hex : raw transaction with signature(s) (hex-encoded string)
- complete : 1 if rawtx is completely signed, 0 if signatures are missing. If no
  private keys are given and the wallet is locked, requires that the wallet be
  unlocked with walletpassphrase first.

=== sendrawtransaction <hex string> === Submits raw transaction (serialized,
hex-encoded) to local node and network. Returns transaction id, or an error if
the transaction is invalid for any reason.

== Motivating use cases == === Multisignature transactions === Funds are sitting
in one or more multisignature transaction outputs, and it is time to gather
signatures and spend them.

Assumption: you know the multisignature outputs' {txid, outputNumber, amount}.

- Create a raw transaction to spend, using createrawtransaction.
- Use signrawtransaction to add your signatures (after unlocking the wallet, if
  necessary).
- Give the transaction to the other person(s) to sign.
- You or they submit the transaction to the network using sendrawtransaction.
  '''You must be careful to include an appropriate transaction fee''', or the
  sendrawtransaction method is likely to fail (either immediately or, worse, the
  transaction will never confirm). === Debugging/testing === These lower-level
  routines will be useful for debugging and testing; listunspent gives a
  detailed list of the state of the wallet, and sendrawtx might be used to test
  double-spend-handling.

=== Input selection control === You want fine-grained control over exactly what
coins in the wallet are spent.

- Get a list of not-yet-spent outputs with listunspent
- Create a transaction using createrawtransaction
- Apply signatures using signrawtransaction
- Submit it using sendrawtransaction Note that you are responsible for
  preventing accidental double-spends.

=== Control over payment of fees and/or transaction re-transmission === You want
to specify, on a per-transaction basis, how much to pay in fees. Or you want to
implement your own policy for how often transactions that are not immediately
included in blocks are re-broadcast to the network.

- Maintain a list of not-yet-spent, confirmed outputs with listunspent
  (refreshed every time a new block is found, using the -blocknotify feature).
- Create a transaction with exactly the amount of fees you wish with
  createrawtransaction
- Apply signatures using signrawtransaction
- Submit it with sendrawtransaction
- Re-submit it periodicially with sendrawtransaction if it does not get into a
  block.

== Other, non-obvious use cases == === Re-broadcast a transaction === If you
want to re-broadcast a transaction right away, you can use the getrawtransaction
and sendrawtransaction API calls to do that. As a bash shell-script one-liner it
would be:

- sendrawtransaction $(getrawtransaction $TXID) (note that Bitcoin-Qt/bitcoind
  automatically re-transmit wallet transactions periodically until they are
  accepted into a block).

=== Validate a transaction without broadcasting it === If you have a raw
transaction and want to make sure all of its signatures are correct, you can use
the signrawtransaction API call. Pass in the hex-encoded raw transaction, any
inputs that bitcoind doesn't yet know about, and an empty array of private keys
to use to sign the transaction. Passing an empty array of private keys will
prevent signrawtransaction from doing any signing; if it returns "complete":1
then all of the existing signatures are valid and there are no signatures
missing.

==See Also==

- [[Coin analogy]]
- [[Original_Bitcoin_client/API_calls_list|API calls list]]
- https://people.xiph.org/~greg/signdemo.txt example of signing offline<BR>
- https://people.xiph.org/~greg/escrowexample.txt example for escrow<BR>
- https://gist.github.com/3966071 example for offline multisig (not supported as
  of 0.7.1)<BR>

[[Category:Technical]] [[Category:Developer]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
