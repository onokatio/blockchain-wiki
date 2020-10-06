---
title: "プロトコル関連"
date: 2020-10-07T00:35:11+09:00
draft: true
---

This page ''describes'' the behavior of the [[Original Bitcoin client|reference client]]. The Bitcoin protocol is specified by the behavior of the reference client, not by this page. In particular, while this page is quite complete in describing the [[network]] protocol, it does not attempt to list all of the rules for block or transaction validity.

Type names used in this documentation are from the C99 standard.

For protocol used in mining, see [[getblocktemplate]].

==Common standards==

=== Hashes ===

Usually, when a hash is computed within bitcoin, it is computed twice. Most of the time [http://en.wikipedia.org/wiki/SHA-2 SHA-256] hashes are used, however [http://en.wikipedia.org/wiki/RIPEMD RIPEMD-160] is also used when a shorter hash is desirable (for example when creating a bitcoin address).

Example of double-SHA-256 encoding of string "hello":

 hello
 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824 (first round of sha-256)
 9595c9df90075148eb06860365df33584b75bff782a510c6cd4883a419833d50 (second round of sha-256)

For bitcoin addresses (RIPEMD-160) this would give:

 hello
 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824 (first round is sha-256)
 b6a9c8c230722b7c748331a8b450f05566dc7d0f (with ripemd-160)

=== Merkle Trees ===

Merkle trees are binary trees of hashes. Merkle trees in bitcoin use a '''double''' SHA-256, the SHA-256 hash of the SHA-256 hash of something.

If, when forming a row in the tree (other than the root of the tree), it would have an odd number of elements, the final double-hash is duplicated to ensure that the row has an even number of hashes.

First form the bottom row of the tree with the ordered double-SHA-256 hashes of the byte streams of the transactions in the block.

Then the row above it consists of half that number of hashes.  Each entry is the double-SHA-256 of the 64-byte concatenation of the corresponding two hashes below it in the tree.

This procedure repeats recursively until we reach a row consisting of just a single double-hash.  This is the '''Merkle root''' of the tree.

For example, imagine a block with three transactions ''a'', ''b'' and ''c''.   The Merkle tree is: 

 d1 = dhash(a)
 d2 = dhash(b)
 d3 = dhash(c)
 d4 = dhash(c)            # a, b, c are 3. that's an odd number, so we take the c twice
 
 d5 = dhash(d1 concat d2)
 d6 = dhash(d3 concat d4)
 
 d7 = dhash(d5 concat d6)

where
 
 dhash(a) = sha256(sha256(a))

''d7'' is the Merkle root of the 3 transactions in this block.

Note: Hashes in Merkle Tree displayed in the [[Block Explorer]] are of little-endian notation. For some implementations and [http://www.fileformat.info/tool/hash.htm calculations], the bytes need to be reversed before they are hashed, and again after the hashing operation.

=== Signatures ===

Bitcoin uses [http://en.wikipedia.org/wiki/Elliptic_curve_cryptography Elliptic Curve] [http://en.wikipedia.org/wiki/Digital_Signature_Algorithm Digital Signature Algorithm] ([http://en.wikipedia.org/wiki/Elliptic_Curve_DSA ECDSA]) to sign transactions. 

For [[ECDSA]] the secp256k1 curve from http://www.secg.org/sec2-v2.pdf is used.

Public keys (in scripts) are given as 04 <x> <y> where ''x'' and ''y'' are 32 byte big-endian integers representing the coordinates of a point on the curve or in compressed form given as <sign> <x> where <sign> is 0x02 if ''y'' is even and 0x03 if ''y'' is odd.

Signatures use [http://en.wikipedia.org/wiki/Distinguished_Encoding_Rules DER encoding] to pack the ''r'' and ''s'' components into a single byte stream (this is also what OpenSSL produces by default).

=== Transaction Verification ===
Transactions are cryptographically signed records that reassign ownership of Bitcoins to new addresses.  Transactions have ''inputs'' - records which reference the funds from other previous transactions - and ''outputs'' - records which determine the new owner of the transferred Bitcoins, and which will be referenced as inputs in future transactions as those funds are respent.

Each ''input'' must have a cryptographic digital signature that unlocks the funds from the prior transaction.  Only the person possessing the appropriate [[private key]] is able to create a satisfactory signature; this in effect ensures that funds can only be spent by their owners.

Each ''output'' determines which Bitcoin address (or other criteria, see [[Script]]) is the recipient of the funds.

In a transaction, the sum of all inputs must be equal to or greater than the sum of all outputs.  If the inputs exceed the outputs, the difference is considered a [[transaction fee]], and is redeemable by whoever first includes the transaction into the block chain.

A special kind of transaction, called a [[coinbase transaction]], has no inputs.  It is created by [[miners]], and there is one coinbase transaction per block.  Because each block comes with a reward of newly created Bitcoins (e.g. 50 BTC for the first 210,000 blocks), the first transaction of a block is, with few exceptions, the transaction that grants those coins to their recipient (the miner).  In addition to the newly created Bitcoins, the coinbase transaction is also used for assigning the recipient of any transaction fees that were paid within the other transactions being included in the same block.  The coinbase transaction can assign the entire reward to a single Bitcoin address, or split it in portions among multiple addresses, just like any other transaction.  Coinbase transactions always contain outputs totalling the sum of the block reward plus all transaction fees collected from the other transactions in the same block.

The [[coinbase transaction]] in block zero cannot be spent. This is due to a quirk of the reference client implementation that would open the potential for a block chain fork if some nodes accepted the spend and others did not<ref>[http://bitcointalk.org/index.php?topic=119645.msg1288552#msg1288552 Block 0 Network Fork]</ref>.

Most Bitcoin outputs encumber the newly transferred coins with a single ECDSA private key.  The actual record saved with inputs and outputs isn't necessarily a key, but a ''script''.  Bitcoin uses an interpreted scripting system to determine whether an output's criteria have been satisfied, with which more complex operations are possible, such as outputs that require two ECDSA signatures, or two-of-three-signature schemes.  An output that references a single Bitcoin address is a ''typical'' output; an output actually contains this information in the form of a script that requires a single ECDSA signature (see [[OP_CHECKSIG]]).  The output script specifies what must be provided to unlock the funds later, and when the time comes in the future to spend the transaction in another input, that input must provide all of the thing(s) that satisfy the requirements defined by the original output script.

=== Addresses ===

A bitcoin address is in fact the hash of a ECDSA public key, computed this way:

 Version = 1 byte of 0 (zero); on the test network, this is 1 byte of 111
 Key hash = Version concatenated with RIPEMD-160(SHA-256(public key))
 Checksum = 1st 4 bytes of SHA-256(SHA-256(Key hash))
 Bitcoin Address = Base58Encode(Key hash concatenated with Checksum)

The Base58 encoding used is home made, and has some differences. Especially, leading zeroes are kept as single zeroes when conversion happens.

== Common structures ==

Almost all integers are encoded in little endian. Only IP or port number are encoded big endian.

=== Message structure ===

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || magic || uint32_t || Magic value indicating message origin network, and used to seek to next message when stream state is unknown
|-
| 12 || command || char[12] || ASCII string identifying the packet content, NULL padded (non-NULL padding results in packet rejected)
|-
| 4 || length || uint32_t || Length of payload in number of bytes
|-
| 4 || checksum || uint32_t || First 4 bytes of sha256(sha256(payload))
|-
| ? || payload || uchar[] || The actual data
|}


Known magic values:

{|class="wikitable"
! Network !! Magic value !! Sent over wire as
|-
| main || 0xD9B4BEF9 || F9 BE B4 D9
|-
| testnet/regtest || 0xDAB5BFFA || FA BF B5 DA
|-
| testnet3 || 0x0709110B || 0B 11 09 07
|-
| signet(default) || 0x40CF030A || 0A 03 CF 40
|-
| namecoin || 0xFEB4BEF9 || F9 BE B4 FE
|}

=== Variable length integer ===

Integer can be encoded depending on the represented value to save space.
Variable length integers always precede an array/vector of a type of data that may vary in length.
Longer numbers are encoded in little endian.

{|class="wikitable"
! Value !! Storage length !! Format
|-
| < 0xFD || 1 || uint8_t
|-
| <= 0xFFFF || 3 || 0xFD followed by the length as uint16_t
|-
| <= 0xFFFF FFFF || 5 || 0xFE followed by the length as uint32_t
|-
| - || 9 || 0xFF followed by the length as uint64_t
|}

If you're reading the Satoshi client code (BitcoinQT) it refers to this encoding as a "CompactSize". Modern Bitcoin Core also has the VARINT macro which implements an even more compact integer for the purpose of local storage (which is incompatible with "CompactSize" described here). VARINT is not a part of the protocol.

=== Variable length string ===

Variable length string can be stored using a variable length integer followed by the string itself.

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 1+ || length || [[Protocol_documentation#Variable_length_integer|var_int]] || Length of the string
|-
| ? || string || char[] || The string itself (can be empty)
|}

=== Network address ===

When a network address is needed somewhere, this structure is used. Network addresses are not prefixed with a timestamp in the version message.

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || time || uint32 || the Time (version >= 31402). '''Not present in version message.'''
|-
| 8 || services || uint64_t || same service(s) listed in [[#version|version]]
|-
| 16 || IPv6/4 || char[16] || IPv6 address. Network byte order. The original client only supported IPv4 and only read the last 4 bytes to get the IPv4 address. However, the IPv4 address is written into the message as a 16 byte [http://en.wikipedia.org/wiki/IPv6#IPv4-mapped_IPv6_addresses IPv4-mapped IPv6 address]
(12 bytes ''00 00 00 00  00 00 00 00  00 00 FF FF'', followed by the 4 bytes of the IPv4 address).
|-
| 2 || port || uint16_t || port number, network byte order
|}

Hexdump example of Network address structure

<pre>
0000   01 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
0010   00 00 FF FF 0A 00 00 01  20 8D                    ........ .

Network address:
 01 00 00 00 00 00 00 00                         - 1 (NODE_NETWORK: see services listed under version command)
 00 00 00 00 00 00 00 00 00 00 FF FF 0A 00 00 01 - IPv6: ::ffff:a00:1 or IPv4: 10.0.0.1
 20 8D                                           - Port 8333
</pre>

=== Inventory Vectors ===

Inventory vectors are used for notifying other nodes about objects they have or data which is being requested.

Inventory vectors consist of the following data format:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || type || uint32_t || Identifies the object type linked to this inventory
|-
| 32 || hash || char[32] || Hash of the object
|}


The object type is currently defined as one of the following possibilities:

{|class="wikitable"
! Value !! Name !! Description
|-
| 0 || ERROR || Any data of with this number may be ignored
|-
| 1 || MSG_TX || Hash is related to a transaction
|-
| 2 || MSG_BLOCK || Hash is related to a data block
|-
| 3 || MSG_FILTERED_BLOCK || Hash of a block header; identical to MSG_BLOCK.  Only to be used in getdata message. Indicates the reply should be a merkleblock message rather than a block message; this only works if a bloom filter has been set.
|-
| 4 || MSG_CMPCT_BLOCK || Hash of a block header; identical to MSG_BLOCK.  Only to be used in getdata message. Indicates the reply should be a cmpctblock message. See BIP 152 for more info.
|}

Other Data Type values are considered reserved for future implementations.

=== Block Headers ===

Block headers are sent in a headers packet in response to a getheaders message.

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || version || int32_t || Block version information (note, this is signed)
|-
| 32 || prev_block || char[32] || The hash value of the previous block this particular block references
|-
| 32 || merkle_root || char[32] || The reference to a Merkle tree collection which is a hash of all transactions related to this block
|-
| 4 || timestamp || uint32_t || A timestamp recording when this block was created (Will overflow in 2106<ref>https://en.wikipedia.org/wiki/Unix_time#Notable_events_in_Unix_time</ref>)
|-
| 4 || bits || uint32_t || The calculated difficulty target being used for this block
|-
| 4 || nonce || uint32_t || The nonce used to generate this block… to allow variations of the header and compute different hashes
|-
| 1+ || txn_count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of transaction entries, this value is always 0
|}

cf. [[Block hashing algorithm]]

=== Differential encoding === 
Several uses of CompactSize below are "differentially encoded". For these, instead of using raw indexes, the number encoded is the difference between the current index and the previous index, minus one. For example, a first index of 0 implies a real index of 0, a second index of 0 thereafter refers to a real index of 1, etc.

=== PrefilledTransaction ===

A PrefilledTransaction structure is used in HeaderAndShortIDs to provide a list of a few transactions explicitly.

{|class="wikitable"
! Field Name !! Type !! Size !! Encoding || Purpose
|-
| index || [[Protocol_documentation#Variable_length_integer|CompactSize]] || 1, 3 bytes || Compact Size, differentially encoded since the last PrefilledTransaction in a list || The index into the block at which this transaction is
|-
| tx || Transaction || variable || As encoded in [[Protocol_documentation#tx|tx messages]] || The transaction which is in the block at index index.
|}

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

=== HeaderAndShortIDs ===

A HeaderAndShortIDs structure is used to relay a block header, the short transactions IDs used for matching already-available transactions, and a select few transactions which we expect a peer may be missing.

{|class="wikitable"
! Field Name !! Type !! Size !! Encoding || Purpose
|-
| header || Block header || 80 bytes || First 80 bytes of the block as defined by the encoding used by "block" messages	|| The header of the block being provided
|-
| nonce	|| uint64_t || 8 bytes || Little Endian || A nonce for use in short transaction ID calculations
|-
| shortids_length || [[Protocol_documentation#Variable_length_integer|CompactSize]] || 1 or 3 bytes || As used to encode array lengths elsewhere || The number of short transaction IDs in shortids (ie block tx count - prefilledtxn_length)
|-
| shortids || List of 6-byte integers || 6*shortids_length bytes || Little Endian || The [[Protocol_documentation#Short_transaction_ID|short transaction IDs]] calculated from the transactions which were not provided explicitly in prefilledtxn
|-
| prefilledtxn_length || [[Protocol_documentation#Variable_length_integer|CompactSize]] || 1 or 3 bytes || As used to encode array lengths elsewhere || The number of prefilled transactions in prefilledtxn (ie block tx count - shortids_length)
|-
| prefilledtxn || List of PrefilledTransactions || variable size*prefilledtxn_length || As defined by [[Protocol_documentation#PrefilledTransaction|PrefilledTransaction]] definition, above || Used to provide the coinbase transaction and a select few which we expect a peer may be missing
|}

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

=== BlockTransactionsRequest ===

A BlockTransactionsRequest structure is used to list transaction indexes in a block being requested.

{|class="wikitable"
! Field Name !! Type !! Size !! Encoding || Purpose
|-
| blockhash || Binary blob || 32 bytes || The output from a double-SHA256 of the block header, as used elsewhere || The blockhash of the block which the transactions being requested are in
|-
| indexes_length || [[Protocol_documentation#Variable_length_integer|CompactSize]] || 1 or 3 bytes || As used to encode array lengths elsewhere || The number of transactions being requested
|-
| indexes || List of [[Protocol_documentation#Variable_length_integer|CompactSizes]] || 1 or 3 bytes*indexes_length || [[Protocol_documentation#Differential_encoding|Differentially encoded]] || The indexes of the transactions being requested in the block
|}

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

=== BlockTransactions ===

A BlockTransactions structure is used to provide some of the transactions in a block, as requested.

{|class="wikitable"
! Field Name !! Type !! Size !! Encoding || Purpose
|-
| blockhash || Binary blob || 32 bytes || The output from a double-SHA256 of the block header, as used elsewhere || The blockhash of the block which the transactions being provided are in
|-
| transactions_length || [[Protocol_documentation#Variable_length_integer|CompactSize]] || 1 or 3 bytes || As used to encode array lengths elsewhere || The number of transactions provided
|-
| transactions || List of Transactions || variable || As encoded in [[Protocol_documentation#tx|tx messages]] || The transactions provided
|}

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

=== Short transaction ID ===

Short transaction IDs are used to represent a transaction without sending a full 256-bit hash. They are calculated by:

# single-SHA256 hashing the block header with the nonce appended (in little-endian)
# Running SipHash-2-4 with the input being the transaction ID and the keys (k0/k1) set to the first two little-endian 64-bit integers from the above hash, respectively.
# Dropping the 2 most significant bytes from the SipHash output to make it 6 bytes.

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

== Message types ==

=== version ===

When a node creates an outgoing connection, it will immediately [[Version Handshake|advertise]] its version. The remote node will respond with its version. No further communication is possible until both peers have exchanged their version.

Payload:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || version || int32_t || Identifies protocol version being used by the node
|-
| 8 || services || uint64_t || bitfield of features to be enabled for this connection
|-
| 8 || timestamp || int64_t || standard UNIX timestamp in seconds
|-
| 26 || addr_recv || [[#Network address|net_addr]] || The network address of the node receiving this message
|-
|colspan="4"| Fields below require version ≥ 106
|-
| 26 || addr_from || [[#Network address|net_addr]] || The network address of the node emitting this message
|-
| 8 || nonce || uint64_t || Node random nonce, randomly generated every time a version packet is sent. This nonce is used to detect connections to self.
|-
| ? || user_agent || [[#Variable length string|var_str]] || [https://github.com/bitcoin/bips/blob/master/bip-0014.mediawiki User Agent] (0x00 if string is 0 bytes long)
|-
| 4 || start_height || int32_t || The last block received by the emitting node
|-
|colspan="4"| Fields below require version ≥ 70001
|-
| 1 || relay || bool || Whether the remote peer should announce relayed transactions or not, see [https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki BIP 0037]
|}

A "verack" packet shall be sent if the version packet was accepted.

The following services are currently assigned:

{|class="wikitable"
! Value !! Name !! Description
|-
| 1 || NODE_NETWORK || This node can be asked for full blocks instead of just headers.
|-
| 2 || NODE_GETUTXO || See [https://github.com/bitcoin/bips/blob/master/bip-0064.mediawiki BIP 0064]
|-
| 4 || NODE_BLOOM   || See [https://github.com/bitcoin/bips/blob/master/bip-0111.mediawiki BIP 0111]
|-
| 8 || NODE_WITNESS   || See [https://github.com/bitcoin/bips/blob/master/bip-0144.mediawiki BIP 0144]
|-
| 1024 || NODE_NETWORK_LIMITED   || See [https://github.com/bitcoin/bips/blob/master/bip-0159.mediawiki BIP 0159]
|}

Hexdump example of version message (OBSOLETE EXAMPLE: This example lacks a checksum and user-agent):
<pre>
0000   F9 BE B4 D9 76 65 72 73  69 6F 6E 00 00 00 00 00   ....version.....
0010   55 00 00 00 9C 7C 00 00  01 00 00 00 00 00 00 00   U....|..........
0020   E6 15 10 4D 00 00 00 00  01 00 00 00 00 00 00 00   ...M............
0030   00 00 00 00 00 00 00 00  00 00 FF FF 0A 00 00 01   ................
0040   20 8D 01 00 00 00 00 00  00 00 00 00 00 00 00 00   ................
0050   00 00 00 00 FF FF 0A 00  00 02 20 8D DD 9D 20 2C   .......... ... ,
0060   3A B4 57 13 00 55 81 01  00                        :.W..U...

Message header:
 F9 BE B4 D9                                                                   - Main network magic bytes
 76 65 72 73 69 6F 6E 00 00 00 00 00                                           - "version" command
 55 00 00 00                                                                   - Payload is 85 bytes long
                                                                               - No checksum in version message until 20 February 2012. See https://bitcointalk.org/index.php?topic=55852.0
Version message:
 9C 7C 00 00                                                                   - 31900 (version 0.3.19)
 01 00 00 00 00 00 00 00                                                       - 1 (NODE_NETWORK services)
 E6 15 10 4D 00 00 00 00                                                       - Mon Dec 20 21:50:14 EST 2010
 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 FF FF 0A 00 00 01 20 8D - Recipient address info - see Network Address
 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 FF FF 0A 00 00 02 20 8D - Sender address info - see Network Address
 DD 9D 20 2C 3A B4 57 13                                                       - Node random unique ID
 00                                                                            - "" sub-version string (string is 0 bytes long)
 55 81 01 00                                                                   - Last block sending node has is block #98645
</pre>

And here's a modern (60002) protocol version client advertising itself to a local peer...

Newer protocol includes the checksum now, this is from a mainline (satoshi) client during 
an outgoing connection to another local client, notice that it does not fill out the 
address information at all when the source or destination is "unroutable".

<pre>

0000   f9 be b4 d9 76 65 72 73 69 6f 6e 00 00 00 00 00  ....version.....
0010   64 00 00 00 35 8d 49 32 62 ea 00 00 01 00 00 00  d...5.I2b.......
0020   00 00 00 00 11 b2 d0 50 00 00 00 00 01 00 00 00  .......P........
0030   00 00 00 00 00 00 00 00 00 00 00 00 00 00 ff ff  ................
0040   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
0050   00 00 00 00 00 00 00 00 ff ff 00 00 00 00 00 00  ................
0060   3b 2e b3 5d 8c e6 17 65 0f 2f 53 61 74 6f 73 68  ;..]...e./Satosh
0070   69 3a 30 2e 37 2e 32 2f c0 3e 03 00              i:0.7.2/.>..

Message Header:
 F9 BE B4 D9                                                                   - Main network magic bytes
 76 65 72 73 69 6F 6E 00 00 00 00 00                                           - "version" command
 64 00 00 00                                                                   - Payload is 100 bytes long
 35 8d 49 32                                                                   - payload checksum (little endian)

Version message:
 62 EA 00 00                                                                   - 60002 (protocol version 60002)
 01 00 00 00 00 00 00 00                                                       - 1 (NODE_NETWORK services)
 11 B2 D0 50 00 00 00 00                                                       - Tue Dec 18 10:12:33 PST 2012
 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 FF FF 00 00 00 00 00 00 - Recipient address info - see Network Address
 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 FF FF 00 00 00 00 00 00 - Sender address info - see Network Address
 3B 2E B3 5D 8C E6 17 65                                                       - Node ID
 0F 2F 53 61 74 6F 73 68 69 3A 30 2E 37 2E 32 2F                               - "/Satoshi:0.7.2/" sub-version string (string is 15 bytes long)
 C0 3E 03 00                                                                   - Last block sending node has is block #212672
</pre>

=== verack ===

The ''verack'' message is sent in reply to ''[[#version|version]]''.  This message consists of only a [[#Message structure|message header]] with the command string "verack".

Hexdump of the verack message:

<pre>
0000   F9 BE B4 D9 76 65 72 61  63 6B 00 00 00 00 00 00   ....verack......
0010   00 00 00 00 5D F6 E0 E2                            ........

Message header:
 F9 BE B4 D9                          - Main network magic bytes
 76 65 72 61  63 6B 00 00 00 00 00 00 - "verack" command
 00 00 00 00                          - Payload is 0 bytes long
 5D F6 E0 E2                          - Checksum (little endian)
</pre>

=== addr ===

Provide information on known nodes of the network. Non-advertised nodes should be forgotten after typically 3 hours

Payload:
{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 1+ || count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of address entries (max: 1000)
|-
| 30x? || addr_list || (uint32_t + [[#Network address|net_addr]])[] || Address of other nodes on the network. version < 209 will only read the first one. The uint32_t is a timestamp (see note below).
|}

'''Note''': Starting version 31402, addresses are prefixed with a timestamp. If no timestamp is present, the addresses should not be relayed to other peers, unless it is indeed confirmed they are up.

Hexdump example of ''addr'' message:
<pre>
0000   F9 BE B4 D9 61 64 64 72  00 00 00 00 00 00 00 00   ....addr........
0010   1F 00 00 00 ED 52 39 9B  01 E2 15 10 4D 01 00 00   .....R9.....M...
0020   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 FF   ................
0030   FF 0A 00 00 01 20 8D                               ..... .

Message Header:
 F9 BE B4 D9                                     - Main network magic bytes
 61 64 64 72  00 00 00 00 00 00 00 00            - "addr"
 1F 00 00 00                                     - payload is 31 bytes long
 ED 52 39 9B                                     - payload checksum (little endian)

Payload:
 01                                              - 1 address in this message

Address:
 E2 15 10 4D                                     - Mon Dec 20 21:50:10 EST 2010 (only when version is >= 31402)
 01 00 00 00 00 00 00 00                         - 1 (NODE_NETWORK service - see version message)
 00 00 00 00 00 00 00 00 00 00 FF FF 0A 00 00 01 - IPv4: 10.0.0.1, IPv6: ::ffff:10.0.0.1 (IPv4-mapped IPv6 address)
 20 8D                                           - port 8333
</pre>

=== inv ===

Allows a node to advertise its knowledge of one or more objects. It can be received unsolicited, or in reply to ''getblocks''.

Payload (maximum 50,000 entries, which is just over 1.8 megabytes):

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 1+ || count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of inventory entries
|-
| 36x? || inventory || [[Protocol specification#Inventory Vectors|inv_vect]][] || Inventory vectors
|}

=== getdata ===

getdata is used in response to inv, to retrieve the content of a specific object, and is usually sent after receiving an ''inv'' packet, after filtering known elements. It can be used to retrieve transactions, but only if they are in the memory pool or relay set - arbitrary access to transactions in the chain is not allowed to avoid having clients start to depend on nodes having full transaction indexes (which modern nodes do not).

Payload (maximum 50,000 entries, which is just over 1.8 megabytes):

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 1+ || count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of inventory entries
|-
| 36x? || inventory || [[Protocol specification#Inventory Vectors|inv_vect]][] || Inventory vectors
|}

=== notfound ===

notfound is a response to a getdata, sent if any requested data items could not be relayed, for example, because the requested transaction was not in the memory pool or relay set.

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 1+ || count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of inventory entries
|-
| 36x? || inventory || [[Protocol specification#Inventory Vectors|inv_vect]][] || Inventory vectors
|}

=== getblocks ===

Return an ''inv'' packet containing the list of blocks starting right after the last known hash in the block locator object, up to hash_stop or 500 blocks, whichever comes first. 

The locator hashes are processed by a node in the order as they appear in the message. If a block hash is found in the node's main chain, the list of its children is returned back via the ''inv'' message and the remaining locators are ignored, no matter if the requested limit was reached, or not.

To receive the next blocks hashes, one needs to issue getblocks again with a new block locator object. Keep in mind that some clients may provide blocks which are invalid if the block locator object contains a hash on the invalid branch.

Payload:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || version || uint32_t || the protocol version
|-
| 1+ || hash count || [[Protocol_documentation#Variable_length_integer|var_int]] || number of block locator hash entries
|-
| 32+ || block locator hashes || char[32] || block locator object; newest back to genesis block (dense to start, but then sparse)
|-
| 32 || hash_stop || char[32] || hash of the last desired block; set to zero to get as many blocks as possible (500)
|}

To create the block locator hashes, keep pushing hashes until you go back to the genesis block. After pushing 10 hashes back, the step backwards doubles every loop:

<pre>
// From libbitcoin which is under AGPL
std::vector<size_t> block_locator_indexes(size_t top_height)
{
    std::vector<size_t> indexes;

    // Modify the step in the iteration.
    int64_t step = 1;

    // Start at the top of the chain and work backwards.
    for (auto index = (int64_t)top_height; index > 0; index -= step)
    {
        // Push top 10 indexes first, then back off exponentially.
        if (indexes.size() >= 10)
            step *= 2;

        indexes.push_back((size_t)index);
    }

    //  Push the genesis block index.
    indexes.push_back(0);
    return indexes;
}
</pre>

Note that it is allowed to send in fewer known hashes down to a minimum of just one hash. However, the purpose of the block locator object is to detect a wrong branch in the caller's main chain. If the peer detects that you are off the main chain, it will send in block hashes which are earlier than your last known block. So if you just send in your last known hash and it is off the main chain, the peer starts over at block #1.

=== getheaders ===

Return a ''headers'' packet containing the headers of blocks starting right after the last known hash in the block locator object, up to hash_stop or 2000 blocks, whichever comes first. To receive the next block headers, one needs to issue getheaders again with a new block locator object. Keep in mind that some clients may provide headers of blocks which are invalid if the block locator object contains a hash on the invalid branch.

Payload:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || version || uint32_t || the protocol version
|-
| 1+ || hash count || [[Protocol_documentation#Variable_length_integer|var_int]] || number of block locator hash entries
|-
| 32+ || block locator hashes || char[32] || block locator object; newest back to genesis block (dense to start, but then sparse)
|-
| 32 || hash_stop || char[32] || hash of the last desired block header; set to zero to get as many blocks as possible (2000)
|}

For the block locator object in this packet, the same rules apply as for the [[Protocol_documentation#getblocks|getblocks]] packet.

=== tx ===

''tx'' describes a bitcoin transaction, in reply to ''[[#getdata|getdata]]''. When a bloom filter is applied ''tx'' objects are sent automatically for matching transactions following the <code>merkleblock</code>.


{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || version || int32_t || Transaction data format version (note, this is signed)
|-
| 0 or 2 || flag || optional uint8_t[2] || If present, always 0001, and indicates the presence of witness data
|-
| 1+ || tx_in count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of Transaction inputs (never zero)
|-
| 41+ || tx_in || tx_in[] || A list of 1 or more transaction inputs or sources for coins
|-
| 1+ || tx_out count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of Transaction outputs
|-
| 9+ || tx_out || tx_out[] || A list of 1 or more transaction outputs or destinations for coins
|-
| 0+ || tx_witnesses || tx_witness[] || A list of witnesses, one for each input; omitted if ''flag'' is omitted above
|-
| 4 || lock_time || uint32_t || The block number or timestamp at which this transaction is unlocked:
{|class="wikitable"
! Value !! Description
|-
| 0 || Not locked
|-
| < 500000000  || Block number at which this transaction is unlocked
|-
| >= 500000000 || UNIX timestamp at which this transaction is unlocked
|}
If all TxIn inputs have final (0xffffffff) sequence numbers then lock_time is irrelevant. Otherwise, the transaction may not be added to a block until after lock_time (see [[NLockTime]]).

|}

TxIn consists of the following fields:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 36 || previous_output || outpoint || The previous output transaction reference, as an OutPoint structure
|-
| 1+ || script length || [[Protocol_documentation#Variable_length_integer|var_int]] || The length of the signature script
|-
| ? || signature script || uchar[] || Computational Script for confirming transaction authorization
|-
| 4 || [http://bitcoin.stackexchange.com/q/2025/323 sequence] || uint32_t || Transaction version as defined by the sender. Intended for "replacement" of transactions when information is updated before inclusion into a block.
|}

The OutPoint structure consists of the following fields:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 32 || hash || char[32] || The hash of the referenced transaction.
|-
| 4 || index || uint32_t || The index of the specific output in the transaction. The first output is 0, etc.
|}

The Script structure consists of a series of pieces of information and operations related to the value of the transaction.

(Structure to be expanded in the future… see script.h and script.cpp and [[Script]] for more information)

The TxOut structure consists of the following fields:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 8 || value || int64_t || Transaction Value
|-
| 1+ || pk_script length || [[Protocol_documentation#Variable_length_integer|var_int]] || Length of the pk_script
|-
| ? || pk_script || uchar[] || Usually contains the public key as a Bitcoin script setting up conditions to claim this output.
|}

The TxWitness structure consists of a [[Protocol_documentation#Variable_length_integer|var_int]] count of witness data components, followed by (for each witness data component) a [[Protocol_documentation#Variable_length_integer|var_int]] length of the component and the raw component data itself.

Example ''tx'' message:
<pre>
000000	F9 BE B4 D9 74 78 00 00  00 00 00 00 00 00 00 00   ....tx..........
000010	02 01 00 00 E2 93 CD BE  01 00 00 00 01 6D BD DB   .............m..
000020	08 5B 1D 8A F7 51 84 F0  BC 01 FA D5 8D 12 66 E9   .[...Q........f.
000030	B6 3B 50 88 19 90 E4 B4  0D 6A EE 36 29 00 00 00   .;P......j.6)...
000040	00 8B 48 30 45 02 21 00  F3 58 1E 19 72 AE 8A C7   ..H0E.!..X..r...
000050	C7 36 7A 7A 25 3B C1 13  52 23 AD B9 A4 68 BB 3A   .6zz%;..R#...h.:
000060	59 23 3F 45 BC 57 83 80  02 20 59 AF 01 CA 17 D0   Y#?E.W... Y.....
000070	0E 41 83 7A 1D 58 E9 7A  A3 1B AE 58 4E DE C2 8D   .A.z.X.z...XN...
000080	35 BD 96 92 36 90 91 3B  AE 9A 01 41 04 9C 02 BF   5...6..;...A....
000090	C9 7E F2 36 CE 6D 8F E5  D9 40 13 C7 21 E9 15 98   .~.6.m...@..!...
0000A0	2A CD 2B 12 B6 5D 9B 7D  59 E2 0A 84 20 05 F8 FC   *.+..].}Y... ...
0000B0	4E 02 53 2E 87 3D 37 B9  6F 09 D6 D4 51 1A DA 8F   N.S..=7.o...Q...
0000C0	14 04 2F 46 61 4A 4C 70  C0 F1 4B EF F5 FF FF FF   ../FaJLp..K.....
0000D0	FF 02 40 4B 4C 00 00 00  00 00 19 76 A9 14 1A A0   ..@KL......v....
0000E0	CD 1C BE A6 E7 45 8A 7A  BA D5 12 A9 D9 EA 1A FB   .....E.z........
0000F0	22 5E 88 AC 80 FA E9 C7  00 00 00 00 19 76 A9 14   "^...........v..
000100	0E AB 5B EA 43 6A 04 84  CF AB 12 48 5E FD A0 B7   ..[.Cj.....H^...
000110	8B 4E CC 52 88 AC 00 00  00 00                     .N.R......


Message header:
 F9 BE B4 D9                                       - main network magic bytes
 74 78 00 00 00 00 00 00 00 00 00 00               - "tx" command
 02 01 00 00                                       - payload is 258 bytes long
 E2 93 CD BE                                       - payload checksum (little endian)

Transaction:
 01 00 00 00                                       - version

Inputs:
 01                                                - number of transaction inputs

Input 1:
 6D BD DB 08 5B 1D 8A F7  51 84 F0 BC 01 FA D5 8D  - previous output (outpoint)
 12 66 E9 B6 3B 50 88 19  90 E4 B4 0D 6A EE 36 29
 00 00 00 00

 8B                                                - script is 139 bytes long

 48 30 45 02 21 00 F3 58  1E 19 72 AE 8A C7 C7 36  - signature script (scriptSig)
 7A 7A 25 3B C1 13 52 23  AD B9 A4 68 BB 3A 59 23
 3F 45 BC 57 83 80 02 20  59 AF 01 CA 17 D0 0E 41
 83 7A 1D 58 E9 7A A3 1B  AE 58 4E DE C2 8D 35 BD
 96 92 36 90 91 3B AE 9A  01 41 04 9C 02 BF C9 7E
 F2 36 CE 6D 8F E5 D9 40  13 C7 21 E9 15 98 2A CD
 2B 12 B6 5D 9B 7D 59 E2  0A 84 20 05 F8 FC 4E 02
 53 2E 87 3D 37 B9 6F 09  D6 D4 51 1A DA 8F 14 04
 2F 46 61 4A 4C 70 C0 F1  4B EF F5

 FF FF FF FF                                       - sequence

Outputs:
 02                                                - 2 Output Transactions

Output 1:
 40 4B 4C 00 00 00 00 00                           - 0.05 BTC (5000000)
 19                                                - pk_script is 25 bytes long

 76 A9 14 1A A0 CD 1C BE  A6 E7 45 8A 7A BA D5 12  - pk_script
 A9 D9 EA 1A FB 22 5E 88  AC

Output 2:
 80 FA E9 C7 00 00 00 00                           - 33.54 BTC (3354000000)
 19                                                - pk_script is 25 bytes long

 76 A9 14 0E AB 5B EA 43  6A 04 84 CF AB 12 48 5E  - pk_script
 FD A0 B7 8B 4E CC 52 88  AC

Locktime:
 00 00 00 00                                       - lock time
</pre>

=== block ===

The '''block''' message is sent in response to a getdata message which requests transaction information from a block hash.

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || version || int32_t || Block version information (note, this is signed)
|-
| 32 || prev_block || char[32] || The hash value of the previous block this particular block references
|-
| 32 || merkle_root || char[32] || The reference to a Merkle tree collection which is a hash of all transactions related to this block
|-
| 4 || timestamp || uint32_t || A Unix timestamp recording when this block was created (Currently limited to dates before the year 2106!)
|-
| 4 || bits || uint32_t || The calculated [[Difficulty|difficulty target]] being used for this block
|-
| 4 || nonce || uint32_t || The nonce used to generate this block… to allow variations of the header and compute different hashes
|-
| 1+ || txn_count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of transaction entries
|-
| ? || txns || tx[] || Block transactions, in format of "tx" command
|}

The SHA256 hash that identifies each block (and which must have a run of 0 bits) is calculated from the first 6 fields of this structure (version, prev_block, merkle_root, timestamp, bits, nonce, and standard SHA256 padding, making two 64-byte chunks in all) and ''not'' from the complete block. To calculate the hash, only two chunks need to be processed by the SHA256 algorithm. Since the ''nonce'' field is in the second chunk, the first chunk stays constant during mining and therefore only the second chunk needs to be processed. However, a Bitcoin hash is the hash of the hash, so two SHA256 rounds are needed for each mining iteration.
See [[Block hashing algorithm]] for details and an example.

=== headers ===

The ''headers'' packet returns block headers in response to a ''getheaders'' packet. 

Payload:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 1+ || count || [[Protocol_documentation#Variable_length_integer|var_int]] || Number of block headers
|-
| 81x? || headers || [[Protocol_documentation#Block_Headers|block_header]][] || [[Protocol_documentation#Block_Headers|Block headers]]
|}

Note that the block headers in this packet include a transaction count (a var_int, so there can be more than 81 bytes per header) as opposed to the block headers that are hashed by miners.

=== getaddr ===

The getaddr message sends a request to a node asking for information about known active peers to help with finding potential nodes in the network. The response to receiving this message is to transmit one or more addr messages with one or more peers from a database of known active peers. The typical presumption is that a node is likely to be active if it has been sending a message within the last three hours.

No additional data is transmitted with this message.

=== mempool ===

The mempool message sends a request to a node asking for information about transactions it has verified but which have not yet confirmed. The response to receiving this message is an inv message containing the transaction hashes for all the transactions in the node's mempool.

No additional data is transmitted with this message.

It is specified in [https://github.com/bitcoin/bips/blob/master/bip-0035.mediawiki BIP 35]. Since [https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki BIP 37], if a [[Protocol_documentation#filterload.2C_filteradd.2C_filterclear.2C_merkleblock|bloom filter]] is loaded, only transactions matching the filter are replied.

=== checkorder ===

This message was used for [[IP Transactions]]. As IP transactions have been deprecated, it is no longer used.

=== submitorder ===

This message was used for [[IP Transactions]]. As IP transactions have been deprecated, it is no longer used.

=== reply ===

This message was used for [[IP Transactions]]. As IP transactions have been deprecated, it is no longer used.

=== ping ===

The ''ping'' message is sent primarily to confirm that the TCP/IP connection is still valid. An error in transmission is presumed to be a closed connection and the address is removed as a current peer.

Payload:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 8 || nonce || uint64_t || random nonce
|}

=== pong ===

The ''pong'' message is sent in response to a ''ping'' message. In modern protocol versions, a ''pong'' response is generated using a nonce included in the ping.

Payload:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 8 || nonce || uint64_t || nonce from ping
|}


=== reject===

The ''reject'' message is sent when messages are rejected.

Payload:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 1+ || message || var_str || type of message rejected
|-
| 1 || ccode || char || code relating to rejected message
|-
| 1+ || reason || var_str || text version of reason for rejection
|-
| 0+ || data || char || Optional extra data provided by some errors.  Currently, all errors which provide this field fill it with the TXID or block header hash of the object being rejected, so the field is 32 bytes.
|}

CCodes

{|class="wikitable"
! Value !! Name !! Description
|-
| 0x01 || REJECT_MALFORMED|| 
|-
| 0x10 || REJECT_INVALID ||
|-
| 0x11 || REJECT_OBSOLETE ||
|-
| 0x12 || REJECT_DUPLICATE ||
|-
| 0x40 || REJECT_NONSTANDARD ||
|-
| 0x41 || REJECT_DUST ||
|-
| 0x42 || REJECT_INSUFFICIENTFEE ||
|-
| 0x43 || REJECT_CHECKPOINT ||
|}

=== filterload, filteradd, filterclear, merkleblock ===

These messages are related to Bloom filtering of connections and are defined in [https://github.com/bitcoin/bips/blob/master/bip-0037.mediawiki BIP 0037].


The <code>filterload</code> command is defined as follows:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| ? || filter || uint8_t[] || The filter itself is simply a bit field of arbitrary byte-aligned size. The maximum size is 36,000 bytes.
|-
| 4 || nHashFuncs || uint32_t || The number of hash functions to use in this filter. The maximum value allowed in this field is 50.
|-
| 4 || nTweak || uint32_t || A random value to add to the seed value in the hash function used by the bloom filter.
|-
| 1 || nFlags || uint8_t || A set of flags that control how matched items are added to the filter.
|}

See below for a description of the Bloom filter algorithm and how to select nHashFuncs and filter size for a desired false positive rate.

Upon receiving a <code>filterload</code> command, the remote peer will immediately restrict the broadcast transactions it announces (in inv packets) to transactions matching the filter, where the matching algorithm is specified below. The flags control the update behaviour of the matching algorithm.

The <code>filteradd</code> command is defined as follows:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| ? || data || uint8_t[] || The data element to add to the current filter.
|}

The data field must be smaller than or equal to 520 bytes in size (the maximum size of any potentially matched object).

The given data element will be added to the Bloom filter. A filter must have been previously provided using <code>filterload</code>. This command is useful if a new key or script is added to a clients wallet whilst it has connections to the network open, it avoids the need to re-calculate and send an entirely new filter to every peer (though doing so is usually advisable to maintain anonymity).

The <code>filterclear</code> command has no arguments at all.

After a filter has been set, nodes don't merely stop announcing non-matching transactions, they can also serve filtered blocks. A filtered block is defined by the <code>merkleblock</code> message and is defined like this:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || version || int32_t || Block version information, based upon the software version creating this block (note, this is signed)
|-
| 32 || prev_block || char[32] || The hash value of the previous block this particular block references
|-
| 32 || merkle_root || char[32] || The reference to a Merkle tree collection which is a hash of all transactions related to this block
|-
| 4 || timestamp || uint32_t || A timestamp recording when this block was created (Limited to 2106!)
|-
| 4 || bits || uint32_t || The calculated difficulty target being used for this block
|-
| 4 || nonce || uint32_t || The nonce used to generate this block… to allow variations of the header and compute different hashes
|-
| 4 || total_transactions || uint32_t || Number of transactions in the block (including unmatched ones)
|-
| 1+ || hash_count || [[Protocol_documentation#Variable_length_integer|var_int]] || The number of hashes to follow
|-
| 32x? || hashes || char[32] || Hashes in depth-first order
|-
| 1+ || flag_bytes || [[Protocol_documentation#Variable_length_integer|var_int]] || The size of flags (in bytes) to follow
|-
| ? || flags || byte[] || Flag bits, packed per 8 in a byte, least significant bit first. Extra 0 bits are padded on to reach full byte size.
|}

After a <code>merkleblock</code>, transactions matching the bloom filter are automatically sent in ''[[#tx|tx]]'' messages.

A guide to creating a bloom filter, loading a merkle block, and parsing a partial merkle block tree can be found in the [https://bitcoin.org/en/developer-examples#creating-a-bloom-filter Developer Examples].

=== alert ===

'''Note:''' Support for [[Alert system|alert messages]] has been removed from bitcoin core in March 2016. Read more [https://bitcoin.org/en/alert/2016-11-01-alert-retirement here]


An [[Alert system|'''alert''']] is sent between nodes to send a general notification message throughout the network. If the alert can be confirmed with the signature as having come from the core development group of the Bitcoin software, the message is suggested to be displayed for end-users. Attempts to perform transactions, particularly automated transactions through the client, are suggested to be halted. The text in the Message string should be relayed to log files and any user interfaces.

Alert format:

{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| ? || payload || uchar[] || Serialized alert payload
|-
| ? || signature || uchar[] || An ECDSA signature of the message
|}

The developers of Satoshi's client use this public key for signing alerts:
 04fc9702847840aaf195de8442ebecedf5b095cdbb9bc716bda9110971b28a49e0ead8564ff0db22209e0374782c093bb899692d524e9d6a6956e7c5ecbcd68284
 (hash) 1AGRxqDa5WjUKBwHB9XYEjmkv1ucoUUy1s

The payload is serialized into a uchar[] to ensure that versions using incompatible alert formats can still relay alerts among one another. The current alert payload format is:
{|class="wikitable"
! Field Size !! Description !! Data type !! Comments
|-
| 4 || Version || int32_t || Alert format version
|-
| 8 || RelayUntil || int64_t || The timestamp beyond which nodes should stop relaying this alert
|-
| 8 || Expiration || int64_t || The timestamp beyond which this alert is no longer in effect and should be ignored
|-
| 4 || ID || int32_t || A unique ID number for this alert
|-
| 4 || Cancel || int32_t || All alerts with an ID number less than or equal to this number should be cancelled: deleted and not accepted in the future
|-
| ? || setCancel || set<int32_t> || All alert IDs contained in this set should be cancelled as above
|-
| 4 || MinVer || int32_t || This alert only applies to versions greater than or equal to this version. Other versions should still relay it.
|-
| 4 || MaxVer || int32_t || This alert only applies to versions less than or equal to this version. Other versions should still relay it.
|-
| ? || setSubVer || set<string> || If this set contains any elements, then only nodes that have their subVer contained in this set are affected by the alert. Other versions should still relay it.
|-
| 4 || Priority || int32_t || Relative priority compared to other alerts
|-
| ? || Comment || string || A comment on the alert that is not displayed
|-
| ? || StatusBar || string || The alert message that is displayed to the user
|-
| ? || Reserved || string || Reserved
|}

Note: '''set<''type''>''' in the table above is a [[#Variable length integer | variable length integer]] followed by the number of fields of the given ''type'' (either int32_t or [[#Variable length string | variable length string]])

Sample alert (no message header):
 73010000003766404f00000000b305434f00000000f2030000f1030000001027000048ee0000
 0064000000004653656520626974636f696e2e6f72672f666562323020696620796f75206861
 76652074726f75626c6520636f6e6e656374696e672061667465722032302046656272756172
 79004730450221008389df45f0703f39ec8c1cc42c13810ffcae14995bb648340219e353b63b
 53eb022009ec65e1c1aaeec1fd334c6b684bde2b3f573060d5b70c3a46723326e4e8a4f1
 
 Version: 1
 RelayUntil: 1329620535
 Expiration: 1329792435
 ID: 1010
 Cancel: 1009
 setCancel: <empty>
 MinVer: 10000
 MaxVer: 61000
 setSubVer: <empty>
 Priority: 100
 Comment: <empty>
 StatusBar: "See bitcoin.org/feb20 if you have trouble connecting after 20 February"
 Reserved: <empty>

=== sendheaders ===

Request for Direct headers announcement. 

Upon receipt of this message, the node is be permitted, but not required, to announce new blocks by '''headers''' command (instead of '''inv''' command).

This message is supported by the protocol version >= 70012 or Bitcoin Core version >= 0.12.0.

See [https://github.com/bitcoin/bips/blob/master/bip-0130.mediawiki BIP 130] for more information.

No additional data is transmitted with this message.


=== feefilter ===

The payload is always 8 bytes long and it encodes 64 bit integer value (LSB / little endian) of '''feerate'''.
The value represents a minimal fee and is expressed in satoshis per 1000 bytes.

Upon receipt of a "feefilter" message, the node will be permitted, but not required, to filter transaction invs for transactions that fall below the feerate provided in the feefilter message interpreted as satoshis per kilobyte.

The fee filter is additive with a bloom filter for transactions so if an SPV client were to load a bloom filter and send a feefilter message, transactions would only be relayed if they passed both filters.

Inv's generated from a mempool message are also subject to a fee filter if it exists.

Feature discovery is enabled by checking protocol version >= 70013

See [https://github.com/bitcoin/bips/blob/master/bip-0133.mediawiki BIP 133] for more information.

=== sendcmpct ===

# The sendcmpct message is defined as a message containing a 1-byte integer followed by a 8-byte integer where pchCommand == "sendcmpct".
# The first integer SHALL be interpreted as a boolean (and MUST have a value of either 1 or 0)
# The second integer SHALL be interpreted as a little-endian version number. Nodes sending a sendcmpct message MUST currently set this value to 1.
# Upon receipt of a "sendcmpct" message with the first and second integers set to 1, the node SHOULD announce new blocks by sending a cmpctblock message.
# Upon receipt of a "sendcmpct" message with the first integer set to 0, the node SHOULD NOT announce new blocks by sending a cmpctblock message, but SHOULD announce new blocks by sending invs or headers, as defined by BIP130.
# Upon receipt of a "sendcmpct" message with the second integer set to something other than 1, nodes MUST treat the peer as if they had not received the message (as it indicates the peer will provide an unexpected encoding in 
# cmpctblock, and/or other, messages). This allows future versions to send duplicate sendcmpct messages with different versions as a part of a version handshake for future versions.
# Nodes SHOULD check for a protocol version of >= 70014 before sending sendcmpct messages.
# Nodes MUST NOT send a request for a MSG_CMPCT_BLOCK object to a peer before having received a sendcmpct message from that peer.

This message is only supported by protocol version >= 70014

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

=== cmpctblock ===

# The cmpctblock message is defined as as a message containing a serialized [[Protocol_documentation#HeaderAndShortIDs|HeaderAndShortIDs]] message and pchCommand == "cmpctblock".
# Upon receipt of a cmpctblock message after sending a sendcmpct message, nodes SHOULD calculate the [[Protocol_documentation#Short_transaction_ID|short transaction ID]] for each unconfirmed transaction they have available (ie in their mempool) and compare each to each short transaction ID in the cmpctblock message.
# After finding already-available transactions, nodes which do not have all transactions available to reconstruct the full block SHOULD request the missing transactions using a getblocktxn message.
# A node MUST NOT send a cmpctblock message unless they are able to respond to a getblocktxn message which requests every transaction in the block.
# A node MUST NOT send a cmpctblock message without having validated that the header properly commits to each transaction in the block, and properly builds on top of the existing chain with a valid proof-of-work. A node MAY send a cmpctblock before validating that each transaction in the block validly spends existing [[UTXO]] set entries.

This message is only supported by protocol version >= 70014

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

=== getblocktxn ===

# The getblocktxn message is defined as as a message containing a serialized [[Protocol_documentation#BlockTransactionsRequest|BlockTransactionsRequest]] message and pchCommand == "getblocktxn".
# Upon receipt of a properly-formatted getblocktxnmessage, nodes which recently provided the sender of such a message a cmpctblock for the block hash identified in this message MUST respond with an appropriate [[Protocol_documentation#blocktxn|blocktxn]] message. Such a blocktxn message MUST contain exactly and only each transaction which is present in the appropriate block at the index specified in the getblocktxn indexes list, in the order requested.

This message is only supported by protocol version >= 70014

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

=== blocktxn ===

# The blocktxn message is defined as as a message containing a serialized [[Protocol_documentation#BlockTransactions|BlockTransactions]] message and pchCommand == "blocktxn".
# Upon receipt of a properly-formatted requested blocktxn message, nodes SHOULD attempt to reconstruct the full block by:
# Taking the prefilledtxn transactions from the original [[Protocol_documentation#cmpctblock|cmpctblock]] and placing them in the marked positions.
# For each short transaction ID from the original [[Protocol_documentation#cmpctblock|cmpctblock]], in order, find the corresponding transaction either from the blocktxn message or from other sources and place it in the first available position in the block.
# Once the block has been reconstructed, it shall be processed as normal, keeping in mind that short transaction IDs are expected to occasionally collide, and that nodes MUST NOT be penalized for such collisions, wherever they appear.

This message is only supported by protocol version >= 70014

See [https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki BIP 152] for more information.

== Scripting ==

See [[script]].

==See Also==

* [[Network]]
* [[Protocol rules]]
* [[Hardfork Wishlist]]
* [https://bitcoin.org/en/developer-documentation Developer Documentation on bitcoin.org]
* Bitcoin dissectors for Wireshark: https://github.com/lbotsch/wireshark-bitcoin https://github.com/op-sig/bitcoin-wireshark-dissector

==References==
<references />

[[zh-cn:协议说明]]
[[Category:Technical]]
[[Category:Developer]]

{{Bitcoin Core documentation}}
