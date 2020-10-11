---
title: "Testnet"
date: 2020-10-06T20:09:09+09:00
draft: true
---

The '''testnet''' is an alternative Bitcoin [[block chain]], to be used for
testing. Testnet coins are separate and distinct from actual bitcoins, and are
never supposed to have any value. This allows application developers or bitcoin
testers to experiment, without having to use real bitcoins or worrying about
breaking the main bitcoin chain.

Run bitcoin-qt or bitcoind with the -testnet flag to use the testnet (or put
testnet=1 in the bitcoin.conf file).

There have been three generations of testnet. Testnet2 was just the first
testnet reset with a different genesis block, because people were starting to
trade testnet coins for real money. '''Testnet3''' is the current test network.
It was introduced with the 0.7 release, introduced a third genesis block, a new
rule to avoid the "difficulty was too high, is now too low, and transactions
take too long to verify" problem, and contains blocks with edge-case
transactions designed to test implementation compatibility. On the December 21
of 2015 SegNet was deployed, to test the Wuille's Segregated Witness proposal.

==Differences==

- Default Bitcoin network protocol listen port is 18333 (instead of 8333)
- Default RPC connection port is 18332 (instead of 8332)
- Bootstrapping uses different DNS seeds.
- A different value of ADDRESSVERSION field ensures no testnet Bitcoin addresses
  will work on the production network. (0x6F rather than 0x00)
- The protocol message header bytes are 0x0B110907 (instead of 0xF9BEB4D9)
- Minimum [[difficulty]] of 1.0 on testnet is equal to difficulty of 0.5 on
  mainnet. This means that the mainnet-equivalent of any testnet difficulty is
  half the testnet difficulty. In addition, if no block has been found in 20
  minutes, the difficulty automatically resets back to the minimum for a single
  block, after which it returns to its previous value.
- A new genesis block
- The IsStandard() check is disabled so that non-standard transactions can be
  experimented with.

==Genesis Block==

Testnet uses a different genesis block to the main network. You can find it
[https://www.biteasy.com/testnet/blocks/000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943
here] or [http://blockexplorer.com/testnet/b/0 here]. The testnet was
[https://github.com/gavinandresen/bitcoin-git/commit/feeb761ba07af74a7cd78b8c8f7c2a961fd9ea1c
reset with a new genesis block] for the 0.7 bitcoin release.

==Size== Testnet receives less transactions than the main block chain and is
typically much smaller in size. As of January 2018 the size of the data on disk
was 14GB, containing data for about 6 years worth of testnet activity.
Downloading this data required about 12GB of network activity peaking at 2MB/s
rate of transfer.

==External links==

- [https://bitcointalk.org/?topic=4483.0 Testnet in a box forum topic]
- [https://sourceforge.net/projects/bitcoin/files/Bitcoin/testnet-in-a-box/
  Testnet-In-A-Box self-contained testnet]
- [https://github.com/freewil/bitcoin-testnet-box Forked/Updated testnet-box]

===Wallets===

Online testnet wallets to help you test your application.

- [http://testnetwallet.com/ TestnetWallet.com]
- [https://CoPay.io/ CoPay.io] wallet supports TestNet accounts

===Faucets===

Once you're done with your test coins, it is a nice gesture to send them back to
the faucets, so they become available to other developers.

- [http://tbtc.bitaps.com bitaps.com Testnet Faucet + double spend test tool]
- [http://bitcoinfaucet.uo1.net/ UO1 Testnet Faucet]
- [https://play.google.com/store/apps/details?id=com.mycelium.testnetwallet
  Mycelium Testnet Wallet for Android with integrated Testnet "faucet" function
  (Local Trader)]
- [https://testnet-faucet.mempool.co mempool.co testnet3 Faucet]
- [http://kuttler.eu/bitcoin/btc/faucet/ nkuttler's Bitcoin Testnet Faucet]

Offline (2018-09-06):

- [http://tpfaucet.appspot.com/ TP's TestNet Faucet]
- [https://testnet.manu.backend.hamburg/faucet flyingkiwi's TestNet Faucet]

Offline (2016-08-07):

- [http://faucet.luis.im/ luis.im Mojocoin Testnet3 Faucet]
- [https://accounts.blockcypher.com/testnet-faucet BlockCypher Testnet Faucet],
  also provided as a [http://dev.blockcypher.com/#faucets Testnet faucet API]
  for test automation

===Block explorers===

- [http://tbtc.bitaps.com/ Bitcoin Testnet Explorer on bitaps.com]
- [https://www.biteasy.com/testnet/blocks Biteasy.com Testnet Blockexplorer]
- [http://testnet.blockchain.info Blockchain.info Testnet Explorer]
- [http://tbtc.blockr.io/ Bitcoin Testnet on Blockr.io]
- [https://test-insight.bitpay.com/ Bitcoin Testnet on insight.bitpay.com]
- [https://www.blocktrail.com/tBTC BlockTrail Testnet Explorer, Testnet API and
  Testnet Faucet]
- [https://live.blockcypher.com/btc-testnet/ BlockCypher Testnet
  Explorer][Category:Technical]] [[Category:Developer]]

{{Bitcoin Core documentation}}

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020 This page content is translated by
Bitcoin wiki. original right is reserved by them.
