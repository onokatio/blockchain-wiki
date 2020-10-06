---
title: "Block_chain_browser"
date: 2020-10-07T00:39:07+09:00
draft: true
---

==Block Explorer==

A Blockchain Explorer is a web application to view and query [https://en.bitcoin.it/wiki/Block blocks] <ref>Blockchain for dummies p 99 -  Tiana Laurence - ISBN 978-1119365594</ref> working as a  [https://en.bitcoin.it/wiki/Web_browser web browser] that is not connected to the internet, like Google Chrome, but to the [https://en.bitcoin.it/wiki/Blockchain Blockchain]. Their primary function is to allow everyone with an internet connection to track in real-time all the [https://en.bitcoin.it/wiki/Transaction_confirmation transactions] or interactions made by each [https://en.bitcoin.it/wiki/Cryptocurrency cryptocurrencies] holders, regardless of his or her level of knowledge and expertise. <ref>Blockchain Enabled Application p.21 - David Metcalf -  ISBN 978-1484230800</ref>. However, although there are plenty of Explorer available, only a tiny minority are able to show the transaction using [https://en.bitcoin.it/wiki/Segregated_Witness SegWit] (Bitcoin bech32 addresses start with “bc1”) <ref>Blockchain: A Practical Guide to Developing Business, Law, and Technology Solutions 1st Edition p.19- Joseph J. Bambara - ISBN 978-1260115871</ref>

==The default view==

The default view in the main page generally provides in real time information about of all the last blocks that are being added to the blockchain, most often separated into six columns with this following informations:

'''Block height:''' <ref>Mastering Bitcoin p.28 - Andreas M. Antonopoulos - ISBN 978-1491954386</ref>  block number since the Bitcoin [https://en.bitcoin.it/wiki/Genesis_block genesis block] was [https://en.bitcoin.it/wiki/Mining mined]. Each new mined block assumes an increase of one at the height of the block. Thus, the number 492800 in this column means this is the 492800th block mined in the whole history.

'''Age:''' the moment when the block was accepted by a miner and not when the user initiated a payment or transfer to another wallet.

'''[https://en.bitcoin.it/wiki/Transaction_confirmation Confirmation]:''' The most relevant information visible at a glance, the transaction status. If a transaction is «unconfirmed» or «pending», that means the transaction is in the Blockchain, but has not included in a block yet. In average, it takes typically 10 minutes for a bitcoin transaction to be confirmed. It will take about one hour to get «confirmed» by six confirmations.

'''Transaction list (TX):''' number of transactions included in the block. <ref>Blockchain: A Practical Guide to Developing Business, Law, and Technology Solutions 1st Edition p.18- Joseph J. Bambara - ISBN 978-1260115871</ref>

'''Reward:''' the reward for those (miners) who mined the block.

'''Mined by:''' name of the miner who mined this block. Usually a pool of miners who had been involved in.

'''Size:''' size of the transaction obtained by adding the sizes of each transactions included in the block. This data is always provided in bytes.

==The browser==

In the same way as with an Internet browser, the user needs to enter a request to begin his search. It can be a transaction hash or TXID <ref>beginning-blockchain-guide-building-solutions p.278 - Bikramaditya Singhal - ISBN 978-1484234433</ref>(sequence of 64 characters hexadecimal (0 to 9 and a to f) numbers used to uniquely identify a particular transaction) or a block height for example. Automatically, a page containing all the details about the transaction is loaded according to the request. Advanced information contains all the movements in this transaction and is only available at the user's request. Most of the time, a bitcoin transaction has several input and output [https://en.bitcoin.it/wiki/Bitcoin_address addresses] insofar as it allows a sender to save time and money by sending to several addresses at once. All the following data will be displayed:

'''TXID:''' as mentioned above

'''Input:'''  transaction inputs are pointers to UTXO <ref>Blockchain Enabled Application p.18 - David Metcalf -  ISBN 978-1484230800</ref>. They point to a specific UTXO by reference to the transaction hash and sequence number where the UTXO is recorded in the blockchain. 

'''Output:''' contains instructions for sending bitcoins <ref>Blockchain Enabled Application p.18 - David Metcalf -  ISBN 978-1484230800</ref>. 

'''Value:''' the number of Satoshi (1 BTC = 100,000,000 Satoshi) that this output will be worth when claimed. 

'''ScriptPubKey:''' the second half of a script. There can be more than one output. Because each output from one transaction can only ever be referenced once by an input of a subsequent transaction, the entire combined input value needs to be sent in an output if the user doesn't want to lose it. <ref>beginning-blockchain-guide-building-solutions p.201-202 - Bikramaditya Singhal - ISBN 978-1484234433</ref> If the input is worth 50 BTC but a consumer only want to send 25 BTC, Bitcoin will create two outputs worth 25 BTC. This section is divided into two parts. The first, the top line, showing the total amount that the recipient has to receive in BTC, known as the destination. And the other part, below, showing the amount of bitcoins back to the user that remains in the wallet and which can be used as a new input in a subsequent transaction. Any input bitcoins not redeemed in an output is considered a transaction fee.

'''Transaction Fees:''' most transactions include transaction fees, which compensate the bitcoin miners for securing the network. Transaction fees serve as an incentive to include (mine) a transaction into the next block and also as a disincentive against “spam” transactions or any kind of abuse of the system, by imposing a small cost on every transaction. Transaction fees are collected by the miner who mines the block that records the transaction on the blockchain. <ref>Mastering Bitcoin p.127 - Andreas M. Antonopoulos - ISBN 978-1491954386</ref>

'''Coinbase Data:''' This field must be between 2 and 100 bytes. Except for the first few bytes the rest of the coinbase data can be used by miners in any way they want; it is arbitrary data. In the genesis block, for example, Satoshi Nakamoto added the text “The Times 03/Jan/ 2009 Chancellor on brink of second bailout for banks” in the coinbase data, using it as a proof of the date and to convey a message. Currently, miners use the coinbase data to include extra nonce values and strings identifying the mining pool. <ref>Mastering Bitcoin p.225 - Andreas M. Antonopoulos - ISBN 978-1491954386</ref>

==List of blockchain explorers==
* https://mempool.space    Open-source Bitcoin Explorer
* https://bitaps.com	Explorer that supports Segwit
* https://apirone.com/btc/	Explorer that supports Segwit
* https://blockstream.info/	
* https://bitupper.com/en/explorer/bitcoin	Explorer that supports Segwit
* http://btcblockexplorer.financialplugins.com	
* https://bitcoinchain.com	
* https://bitinfocharts.com/bitcoin
* https://blockchain.info	
* https://blockchair.com/	Explorer that supports Segwit, privacy centric
* https://blockexplorer.com/	
* https://btc.com/	Explorer that supports Segwit	
* https://chain.so/BTC	
* https://chainz.cryptoid.info/btc	Explorer that supports Segwit
* https://explorer.bitcoin.com/btc	
* https://insight.bitpay.com
* https://live.blockcypher.com/btc	
* https://multihash.net	
* https://btc.blockdozer.com	
* https://www.blockonomics.co	Explorer that supports Segwit
* https://www.blocktrail.com/BTC	
* https://www.smartbit.com.au
* https://explorer.poolin.com/ Explorer that supports Segwit

==References==
	<references />
