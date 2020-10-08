---
title: "Base58Check"
date: 2020-10-06T20:02:49+09:00
draft: true
---

(未翻訳)

A modified Base 58 [http://en.wikipedia.org/wiki/Binary-to-text_encoding binary-to-text encoding] known as '''Base58Check''' is used for encoding [[Bitcoin address|Bitcoin addresses]].

More generically, Base58Check encoding is used for encoding byte arrays in Bitcoin into human-typable strings.

# Background
The original Bitcoin client source code explains the reasoning behind base58 encoding:

base58.h:
 // Why base-58 instead of standard base-64 encoding?
 // - Don't want 0OIl characters that look the same in some fonts and
 //      could be used to create visually identical looking account numbers.
 // - A string with non-alphanumeric characters is not as easily accepted as an account number.
 // - E-mail usually won't line-break if there's no punctuation to break at.
 // - Doubleclicking selects the whole number as one word if it's all alphanumeric.

# Features of Base58Check
Base58Check has the following features:
* An arbitrarily sized payload.
* A set of 58 alphanumeric symbols consisting of easily distinguished uppercase and lowercase letters (0OIl are not used) 
* One byte of version/application information.  Bitcoin addresses use 0x00 for this byte (future ones may use 0x05).
* Four bytes (32 bits) of SHA256-based error checking code.  This code can be used to automatically detect and possibly correct typographical errors.
* An extra step for preservation of leading zeroes in the data.

# Creating a Base58Check string
A Base58Check string is created from a version/application byte and payload as follows.
- Take the version byte and payload bytes, and concatenate them together (bytewise).
- Take the first four bytes of SHA256(SHA256(results of step 1))
- Concatenate the results of step 1 and the results of step 2 together (bytewise).
- Treating the results of step 3 - a series of bytes - as a single big-endian bignumber, convert to base-58 using normal mathematical steps (bignumber division) and the base-58 alphabet described below.  The result should be normalized to not have any leading base-58 zeroes (character '1').
- The leading character '1', which has a value of zero in base58, is reserved for representing an entire leading zero '''byte''', as when it is in a leading position, has no value as a base-58 symbol.  There can be one or more leading '1's when necessary to represent one or more leading zero bytes.  Count the number of leading zero bytes that were the result of step 3 (for old Bitcoin addresses, there will always be at least one for the version/application byte; for new addresses, there will never be any).  Each leading zero byte shall be represented by its own character '1' in the final result.
- Concatenate the 1's from step 5 with the results of step 4.  '''This is the Base58Check result.'''
A more detailed example is provided on the page describing the [[Technical_background_of_version_1_Bitcoin_addresses#How_to_create_Bitcoin_Address | technical background]] of the bitcoin address.

# Encoding a Bitcoin address
Bitcoin addresses are implemented using the Base58Check encoding of the hash of either:
* Pay-to-script-hash (p2sh): payload is: <code>[[RIPEMD160]]([[SHA256]]('''redeemScript'''))</code> where '''redeemScript''' is a script the wallet knows how to spend; version <code>0x05</code> (these addresses begin with the digit '3')
* Pay-to-pubkey-hash (p2pkh): payload is <code>[[RIPEMD160]]([[SHA256]]('''ECDSA_publicKey'''))</code> where '''ECDSA_publicKey''' is a public key the wallet knows the private key for; version <code>0x00</code> (these addresses begin with the digit '1')

The resulting hash in both of these cases is always exactly 20 bytes.
These are big-endian (most significant byte first).  (Beware of [[bignumber]] implementations that clip leading 0x00 bytes, or prepend extra 0x00 bytes to indicate sign - your code must handle these cases properly or else you may generate valid-looking addresses which can be sent to, but cannot be spent from - which would lead to the permanent loss of coins.)

# Encoding a private key
Base58Check encoding is also used for encoding [[private key|ECDSA private keys]] in the [[wallet import format]].
This is formed exactly the same as a Bitcoin address, except that 0x80 is used for the version/application byte, and the payload is 32 bytes instead of 20 (a private key in Bitcoin is a single 32-byte unsigned big-endian integer).
For private keys associated with an uncompressed public key, such encodings will always yield a 51-character string that starts with '5', or more specifically, either '5H', '5J', or '5K'.

# Base58 symbol chart
The Base58 symbol chart used in Bitcoin is specific to the Bitcoin project and is not intended to be the same as any other Base58 implementation used outside the context of Bitcoin (the characters excluded are: 0, O, I, and l).
{| class="wikitable" 
|-
!Value
!Character
!Value
!Character
!Value
!Character
!Value
!Character
|-
|0
|1
|1
|2
|2
|3
|3
|4
|-
|4
|5
|5
|6
|6
|7
|7
|8
|-
|8
|9
|9
|A
|10
|B
|11
|C
|-
|12
|D
|13
|E
|14
|F
|15
|G
|-
|16
|H
|17
|J
|18
|K
|19
|L
|-
|20
|M
|21
|N
|22
|P
|23
|Q
|-
|24
|R
|25
|S
|26
|T
|27
|U
|-
|28
|V
|29
|W
|30
|X
|31
|Y
|-
|32
|Z
|33
|a
|34
|b
|35
|c
|-
|36
|d
|37
|e
|38
|f
|39
|g
|-
|40
|h
|41
|i
|42
|j
|43
|k
|-
|44
|m
|45
|n
|46
|o
|47
|p
|-
|48
|q
|49
|r
|50
|s
|51
|t
|-
|52
|u
|53
|v
|54
|w
|55
|x
|-
|56
|y
|57
|z
|}

The algorithm for encoding address_byte_string (consisting of 1-byte_version + hash_or_other_data + 4-byte_check_code) is

    code_string = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
    x = convert_bytes_to_big_integer(hash_result)
    
    output_string = ""
    
    while(x > 0) 
        {
            (x, remainder) = divide(x, 58)
            output_string.append(code_string[remainder])
        }
    
    repeat(number_of_leading_zero_bytes_in_hash)
        {
        output_string.append(code_string[0]);
        }
    
    output_string.reverse();

==Version bytes==
Here are some common version bytes:

{| class="wikitable" 
|-
!Decimal version
!Leading symbol
!Use
|-
|0
|1
|Bitcoin pubkey hash
|-
|5
|3
|Bitcoin script hash
|-
|21
|4
|Bitcoin (compact) public key (proposed)
|-
|52
|M or N
|Namecoin pubkey hash
|-
|128
|5
|Private key
|-
|111
|m or n
|Bitcoin testnet pubkey hash
|-
|196
|2
|Bitcoin testnet script hash
|}
[[List of address prefixes]] is a more complete list.

== See Also ==
* [http://lenschulwitz.com/base58 Online Base58 Decoder, Encoder, and Validator]

== Source code ==
* [https://github.com/bitcoin/bitcoin/blob/master/src/base58.cpp "Satoshi" C++ codebase (decode and encode, no external libraries needed)]
* [https://github.com/luke-jr/libbase58 libbase58 C code (decode and encode, no external libraries needed)]
* [http://lenschulwitz.com/b58/base58perl.txt Base58 Decode, Encode, and Validate in Perl]

[[Category:Technical]]

[[es:Codificación Base58Check]]
