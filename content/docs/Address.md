---
title: "アドレス"
date: 2020-10-07T00:38:52+09:00
draft: true
---

A '''Bitcoin address''', or simply '''address''', is an identifier of 26-35 alphanumeric characters, beginning with the number <code>1</code>, <code>3</code> or <code>bc1</code> that represents a possible destination for a bitcoin payment.
Addresses can be generated at no cost by any user of Bitcoin.
For example, using [[Bitcoin Core]], one can click "New Address" and be assigned an address.
It is also possible to get a Bitcoin address using an account at an exchange or online wallet service.

There are currently three [[List_of_address_prefixes|address formats]] in use:
# [[Transaction#Pay-to-PubkeyHash|P2PKH]] which begin with the number <code>1</code>, eg: <code>1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2</code>.<!-- tails donation address -->
# [[Pay_to_script_hash|P2SH]] type starting with the number <code>3</code>, eg: <code>3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy</code><!-- anyone-can-spend, null script -->.
# [[Bech32]] type starting with <code>bc1</code>, eg: <code>bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq</code>.<!--some example LN wallet-->

==A Bitcoin address is a single-use token==
Like e-mail addresses, you can send bitcoins to a person by sending bitcoins to one of their addresses.
However, ''unlike'' e-mail addresses, people have many different Bitcoin addresses and [[Address reuse|for privacy and security reasons]] a unique address should be used for each transaction.
Most Bitcoin software and websites will help with this by generating a brand new address each time you create an invoice or payment request.

A naive way to accept bitcoin as a merchant is to tell your customers to send money to a single address. However this does not work because Bitcoin transactions are public on the [[block chain]], so if a customer Alice sends you bitcoins then a malicious agent Bob could see that same transaction and send you an email claiming that he paid. You would have no way of knowing whether it was Alice or Bob who send coins to your address. This is why each customer must be given a brand new address.

==Addresses can be created offline==
Creating addresses can be done without an Internet connection and does not require any contact or registration with the Bitcoin network.
It is possible to create large batches of addresses offline using freely available software tools.
Generating batches of addresses is useful in several scenarios, such as e-commerce websites where a unique pre-generated address is dispensed to each customer who chooses a "pay with Bitcoin" option.
Newer [[Deterministic wallet | "HD wallets"]] can generate a "master public key" token which can be used to allow untrusted systems (such as webservers) to generate an unlimited number of addresses without the ability to spend the bitcoins received.

==Addresses are often case sensitive and exact==
Old-style Bitcoin addresses are case-sensitive.  Bitcoin addresses should be copied and pasted using the computer's clipboard wherever possible. If you hand-key a Bitcoin address, and each character is not transcribed exactly - including capitalization - the incorrect address will most likely be rejected by the Bitcoin software.  You will have to check your entry and try again.

The probability that a mistyped address is accepted as being valid is 1 in 2<sup>32</sup>, that is, approximately 1 in 4.29 billion.

New-style [[bech32]] addresses are case insensitive.

==Proving you receive with an address==
Most Bitcoin wallets have a function to "sign" a message, proving the entity receiving funds with an address has agreed to the message.
This can be used to, for example, finalise a contract in a cryptographically provable way prior to making payment for it.

Some services will also piggy-back on this capability by dedicating a specific address for authentication only, in which case the address should never be used for actual Bitcoin transactions.
When you login to or use their service, you will provide a signature proving you are the same person with the pre-negotiated address.

It is important to note that these signatures only prove one receives with an address.
Since Bitcoin transactions do not have a "from" address, you cannot prove you are the ''sender'' of funds.

Current standards for message signatures are only compatible with "version zero" bitcoin addresses (that begin with the number 1).

==Address validation==
If you would like to validate a Bitcoin address in an application, it is advisable to use a method from [https://bitcointalk.org/index.php?topic=1026.0 this thread] rather than to just check for string length, allowed characters, or that the address starts with a 1 or 3.  Validation may also be done using open source code available in [http://rosettacode.org/wiki/Bitcoin/address_validation various languages] or with an [http://lenschulwitz.com/base58 online validating tool]. 

==[[Multisignature|Multi-signature]] addresses==
Addresses can be created that require a combination of multiple private keys.
Since these take advantage of newer features, they begin with the newer prefix of 3 instead of the older 1.
These can be thought of as the equivalent of writing a check to two parties - "pay to the order of somebody AND somebody else" - where both parties must endorse the check in order to receive the funds.

The actual requirement (number of private keys needed, their corresponding public keys, etc.) that must be satisfied to spend the funds is decided in advance by the person generating this type of address, and once an address is created, the requirement cannot be changed without generating a new address.

==What's in an address==
Most Bitcoin addresses are 34 characters.
They consist of random digits and uppercase and lowercase letters, with the exception that the uppercase letter "O", uppercase letter "I", lowercase letter "l", and the number "0" are never used to prevent visual ambiguity.

Some Bitcoin addresses can be shorter than 34 characters (as few as 26) and still be valid.
A significant percentage of Bitcoin addresses are only 33 characters, and some addresses may be even shorter.
Every Bitcoin address stands for a number.
These shorter addresses are valid simply because they stand for numbers that happen to start with zeroes, and when the zeroes are omitted, the encoded address gets shorter.

Several of the characters inside a Bitcoin address are used as a checksum so that typographical errors can be automatically found and rejected.
The checksum also allows Bitcoin software to confirm that a 33-character (or shorter) address is in fact valid and isn't simply an address with a missing character.

==Testnet==
Addresses on the Bitcoin Testnet are generated with a different address version, which results in a different prefix.
See [[List of address prefixes]] and [[Testnet]] for more details.

==Misconceptions==
===Address reuse===

Addresses are not intended to be used more than once, and doing so has numerous problems associated.
See the dedicated article on [[address reuse]] for more details.

===Address balances===

Addresses are not wallets nor accounts, and do not carry balances.
They only receive funds, and you do not send "from" an address at any time.
Various confusing services and software display ''bitcoins received with an address, minus bitcoins sent in random unrelated transactions'' as an "address balance", but this number is not meaningful: it does not imply the recipient of the bitcoins sent to the address has spent them, nor that they still have the bitcoins received.

An example of bitcoin loss resulting from this misunderstanding is when people believed their address contained 3btc. They spent 0.5btc and believed the address now contained 2.5btc when actually it contained zero. The remaining 2.5btc was transferred to a change address which was not backed up and therefore lost. This has happened on a few occasions to users of [[Paper wallets]].

==="From" addresses===
Bitcoin transactions do not have any kind of origin-, source- or "from" address. See the dedicated article on "[[From address|from address]]" for more details.


==Address map==
[[File:Address map.jpg|700px]]


==See Also==
* [[Technical background of Bitcoin addresses]]
* [[List of address prefixes]]
* [[Exit Address]]

== References ==
<references/>

[[Category:Vocabulary]]

[[es:Dirección]]
[[de:Adresse]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020
This page content is translated by Bitcoin wiki. original right is reserved by them.
