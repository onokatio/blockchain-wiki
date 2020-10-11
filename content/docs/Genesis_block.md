---
title: "ジェネシスブロック"
date: 2020-10-06T20:03:53+09:00
draft: true
---

A '''genesis block''' is the first block of a [[block chain]]. Modern versions
of Bitcoin number it as '''block 0''', though very early versions counted it as
block 1. The genesis block is almost always hardcoded into the software of the
applications that utilize its block chain. It is a special case in that it does
not reference a previous block, and for [[Bitcoin]] and almost all of its
derivatives, it produces an unspendable subsidy.

== Main network genesis block == Here is a representation of the genesis
block<ref name="block">{{cite block|hash=000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f|0|year=2009|month=01|day=03}}</ref>
as it appeared in a comment in an old version of Bitcoin
([http://sourceforge.net/p/bitcoin/code/133/tree/trunk/main.cpp#l1613 line
1613]). The first section defines exactly all of the variables necessary to
recreate the block. The second section is the block in standard printblock
format, which contains shortened versions of the data in the first section.

GetHash() = 0x000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f
hashMerkleRoot =
0x4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b
txNew.vin[0].scriptSig = 486604799 4
0x736B6E616220726F662074756F6C69616220646E6F63657320666F206B6E697262206E6F20726F6C6C65636E61684320393030322F6E614A2F33302073656D695420656854
txNew.vout[0].nValue = 5000000000 txNew.vout[0].scriptPubKey =
0x5F1DF16B2B704C8A578D0BBAF74D385CDE12C11EE50455F3C438EF4C3FBCF649B6DE611FEAE06279A60939E028A8D65C10B73071A6F16719274855FEB0FD8A6704
OP_CHECKSIG block.nVersion = 1 block.nTime = 1231006505 block.nBits = 0x1d00ffff
block.nNonce = 2083236893

CBlock(hash=000000000019d6, ver=1, hashPrevBlock=00000000000000,
hashMerkleRoot=4a5e1e, nTime=1231006505, nBits=1d00ffff, nNonce=2083236893,
vtx=1) CTransaction(hash=4a5e1e, ver=1, vin.size=1, vout.size=1, nLockTime=0)
CTxIn(COutPoint(000000, -1), coinbase
04ffff001d0104455468652054696d65732030332f4a616e2f32303039204368616e63656c6c6f72206f6e206272696e6b206f66207365636f6e64206261696c6f757420666f722062616e6b73)
CTxOut(nValue=50.00000000, scriptPubKey=0x5F1DF16B2B704C8A578D0B) vMerkleTree:
4a5e1e ===Hash=== The hash of the genesis block,
'''000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f''',<ref name="block"/>
has two more leading hex zeroes than were required for an early block.

===Coinbase=== [[File:jonny1000thetimes.png|thumb|256px|The Times 03/Jan/2009]]
The [[coinbase]] parameter (seen above in hex) contains, along with the normal
data, the following
text:<ref>[http://web.archive.org/web/20140309004338/http://uk.reuters.com/article/2009/01/03/idUKPTIP32510920090103
Reuters' reference on The Financial Times article (archive.org cached
copy)]</ref>

<blockquote>The Times 03/Jan/2009 Chancellor on brink of second bailout for banks<ref name="block"/></blockquote>

This was probably intended as proof that the block was created on or after
January 3, 2009, as well as a comment on the instability caused by
fractional-reserve banking. Additionally, it suggests that [[Satoshi Nakamoto]]
may have lived in the United
Kingdom.<ref>{{cite web|author=Davis, J.|year=2011|title=The Crypto-Currency|publisher=''The New Yorker''|url=http://www.newyorker.com/magazine/2011/10/10/the-crypto-currency}}</ref>

This detail, "second bailout for banks" could also suggest that the fact a
supposedly liberal and capitalist system, rescuing banks like that, was a
problem for satoshi . . . the choosen topic could have a meaning about bitcoin s
purpose . . .

===Block reward=== The first 50 BTC block reward went to [[address]]
''1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa'',<ref name="block"/> though this reward
can't be spent due to a quirk in the way that the genesis block is expressed in
the code. It is not known if this was done intentionally or
accidentally.<ref>http://bitcoin.stackexchange.com/questions/10009/why-can-t-the-genesis-block-coinbase-be-spent</ref><ref>https://www.reddit.com/r/Bitcoin/comments/1nc13r/the_first_50btc_block_reward_cant_be_spend_why/</ref><ref>https://github.com/bitcoin/bitcoin/blob/9546a977d354b2ec6cd8455538e68fe4ba343a44/src/main.cpp#L1668 -
Genesis block transaction treated as a special case in the reference code</ref>
It is believed that other outputs sent to this address are spendable, but it is
unknown if Satoshi Nakamoto has the private key for this particular address, if
one existed at all.

===Timestamp=== Although the average time between Bitcoin blocks is 10 minutes,
the [[block timestamp|timestamp]] of the next block is a full 6 days after the
genesis block. One interpretation is that Satoshi was working on bitcoin for
some time beforehand and the ''The Times'' front page prompted him to release it
to the public. He then mined the genesis block with a timestamp in the past to
match the headline. It is also possible that, since the block's hash is so low,
he may have spent 6 days mining it with the same timestamp before proceeding to
block 1. The [[prenet hypothesis]] suggests that the genesis block was solved on
January 3, but the software was tested by Satoshi Nakamoto using that genesis
block until January 9, when all the test blocks were deleted and the genesis
block was reused for the main network.

===Raw block data===

The [https://bitcointalk.org/index.php?topic=52706 raw hex version] of the
Genesis block looks like: 00000000 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 ................ 00000010 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
................ 00000020 00 00 00 00 3B A3 ED FD 7A 7B 12 B2 7A C7 2C 3E
....;£íýz{.²zÇ,> 00000030 67 76 8F 61 7F C8 1B C3 88 8A 51 32 3A 9F B8 AA
gv.a.È.ÃˆŠQ2:Ÿ¸ª 00000040 4B 1E 5E 4A 29 AB 5F 49 FF FF 00 1D 1D AC 2B 7C
K.^J)«*Iÿÿ...¬+| 00000050 01 01 00 00 00 01 00 00 00 00 00 00 00 00 00 00
................ 00000060 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
................ 00000070 00 00 00 00 00 00 FF FF FF FF 4D 04 FF FF 00 1D
......ÿÿÿÿM.ÿÿ.. 00000080 01 04 45 54 68 65 20 54 69 6D 65 73 20 30 33 2F ..EThe
Times 03/ 00000090 4A 61 6E 2F 32 30 30 39 20 43 68 61 6E 63 65 6C Jan/2009
Chancel 000000A0 6C 6F 72 20 6F 6E 20 62 72 69 6E 6B 20 6F 66 20 lor on brink of
000000B0 73 65 63 6F 6E 64 20 62 61 69 6C 6F 75 74 20 66 second bailout f
000000C0 6F 72 20 62 61 6E 6B 73 FF FF FF FF 01 00 F2 05 or banksÿÿÿÿ..ò.
000000D0 2A 01 00 00 00 43 41 04 67 8A FD B0 FE 55 48 27 \*....CA.gŠý°þUH'
000000E0 19 67 F1 A6 71 30 B7 10 5C D6 A8 28 E0 39 09 A6 .gñ¦q0·.\Ö¨(à9.¦
000000F0 79 62 E0 EA 1F 61 DE B6 49 F6 BC 3F 4C EF 38 C4 ybàê.aÞ¶Iö¼?Lï8Ä
00000100 F3 55 04 E5 1E C1 12 DE 5C 38 4D F7 BA 0B 8D 57 óU.å.Á.Þ\8M÷º..W
00000110 8A 4C 70 2B 6B F1 1D 5F AC 00 00 00 00 ŠLp+kñ.*¬....

Broken down it looks like this:

01000000 - version
0000000000000000000000000000000000000000000000000000000000000000 - prev block
3BA3EDFD7A7B12B27AC72C3E67768F617FC81BC3888A51323A9FB8AA4B1E5E4A - merkle root
29AB5F49 - timestamp FFFF001D - bits 1DAC2B7C - nonce 01 - number of
transactions 01000000 - version 01 - input
0000000000000000000000000000000000000000000000000000000000000000FFFFFFFF - prev
output 4D - script length
04FFFF001D0104455468652054696D65732030332F4A616E2F32303039204368616E63656C6C6F72206F6E206272696E6B206F66207365636F6E64206261696C6F757420666F722062616E6B73 -
scriptsig FFFFFFFF - sequence 01 - outputs 00F2052A01000000 - 50 BTC 43 -
pk_script length
4104678AFDB0FE5548271967F1A67130B7105CD6A828E03909A67962E0EA1F61DEB649F6BC3F4CEF38C4F35504E51EC112DE5C384DF7BA0B8D578A4C702B6BF11D5FAC -
pk_script 00000000 - lock time ==References== <references/>

[[zh-cn:创世 block]] [[es:Bloque Génesis]] **NOTOC**

[[Category:Technical]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
