---
title: "難易度"
date: 2020-10-06T20:08:33+09:00
draft: true
---

''See also: [[target]]''

=== What is "difficulty"? ===

Difficulty is a measure of how difficult it is to find a hash below a given target.

The Bitcoin network has a global block difficulty. Valid blocks must have a hash below this target.
Mining pools also have a pool-specific share difficulty setting a lower limit for shares.

=== How often does the network difficulty change? ===

Every 2016 [[block|blocks]].

=== What is the formula for difficulty? ===

difficulty = difficulty_1_target / current_target 

(target is a 256 bit number)

difficulty_1_target can be different for various ways to measure difficulty.
Traditionally, it represents a hash where the leading 32 bits are zero and the rest are one (this is known as "pool difficulty" or "pdiff").
The Bitcoin protocol represents targets as a custom floating point type with limited precision; as a result, Bitcoin clients often approximate difficulty based on this (this is known as "bdiff").

===How is difficulty stored in blocks?===

Each block stores a packed representation (called "Bits") for its actual hexadecimal [[target]]. The target can be derived from it via a predefined formula. For example, if the packed target in the block is 0x1b0404cb (stored in little-endian order: <code>cb 04 04 1b</code>), the hexadecimal target is
 0x0404cb * 2**(8*(0x1b - 3)) = 0x00000000000404CB000000000000000000000000000000000000000000000000

Note that this packed format contains a sign bit in the 24th bit, and for example the negation of the above target would be 0x1b<b>8</b>404cb in packed format. Since targets are never negative in practice, however, this means the largest legal value for the lower 24 bits is 0x7fffff. Additionally, 0x008000 is the smallest legal value for the lower 24 bits since targets are always stored with the lowest possible exponent.

===How is difficulty calculated? What is the difference between bdiff and pdiff?===

The highest possible target (difficulty 1) is defined as 0x1d00ffff, which gives us a hex target of
 0x00ffff * 2**(8*(0x1d - 3)) = 0x00000000FFFF0000000000000000000000000000000000000000000000000000

It should be noted that pooled mining often uses non-truncated targets, which puts "pool difficulty 1" at
 0x00000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF

So the difficulty at 0x1b0404cb is therefore:
 0x00000000FFFF0000000000000000000000000000000000000000000000000000 /
 0x00000000000404CB000000000000000000000000000000000000000000000000 
 = 16307.420938523983 (bdiff)
And:
 0x00000000FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF /
 0x00000000000404CB000000000000000000000000000000000000000000000000 
 = 16307.669773817162 (pdiff)

Here's a fast way to calculate bitcoin difficulty. It uses a modified Taylor series for the logarithm (you can see tutorials on flipcode and wikipedia) and relies on logs to transform the difficulty calculation:

<source lang="cpp">
#include <iostream>
#include <cmath>

inline float fast_log(float val)
{
   int * const exp_ptr = reinterpret_cast <int *>(&val);
   int x = *exp_ptr;
   const int log_2 = ((x >> 23) & 255) - 128;
   x &= ~(255 << 23);
   x += 127 << 23;
   *exp_ptr = x;

   val = ((-1.0f/3) * val + 2) * val - 2.0f/3;
   return ((val + log_2) * 0.69314718f);
} 

float difficulty(unsigned int bits)
{
    static double max_body = fast_log(0x00ffff), scaland = fast_log(256);
    return exp(max_body - fast_log(bits & 0x00ffffff) + scaland * (0x1d - ((bits & 0xff000000) >> 24)));
}

int main()
{
    std::cout << difficulty(0x1b0404cb) << std::endl;
    return 0;
}
</source>

To see the math to go from the normal difficulty calculations (which require large big ints bigger than the space in any normal integer) to the calculation above, here's some python:

<source lang="python">
import decimal, math
l = math.log
e = math.e

print 0x00ffff * 2**(8*(0x1d - 3)) / float(0x0404cb * 2**(8*(0x1b - 3)))
print l(0x00ffff * 2**(8*(0x1d - 3)) / float(0x0404cb * 2**(8*(0x1b - 3))))
print l(0x00ffff * 2**(8*(0x1d - 3))) - l(0x0404cb * 2**(8*(0x1b - 3)))
print l(0x00ffff) + l(2**(8*(0x1d - 3))) - l(0x0404cb) - l(2**(8*(0x1b - 3)))
print l(0x00ffff) + (8*(0x1d - 3))*l(2) - l(0x0404cb) - (8*(0x1b - 3))*l(2)
print l(0x00ffff / float(0x0404cb)) + (8*(0x1d - 3))*l(2) - (8*(0x1b - 3))*l(2)
print l(0x00ffff / float(0x0404cb)) + (0x1d - 0x1b)*l(2**8)
</source>

=== What is the current difficulty? ===
[http://blockexplorer.com/q/getdifficulty Current difficulty], as output by Bitcoin's getDifficulty.

[http://bitcoin.sipa.be Graphs]

=== What is the maximum difficulty? ===

There is no minimum target. The maximum difficulty is roughly: maximum_target / 1 (since 0 would result in infinity), which is a ridiculously huge number (about 2^224).

The actual maximum difficulty is when current_target=0, but we would not be able to calculate the difficulty if that happened. (fortunately it never will, so we're ok.)

=== Can the network difficulty go down? ===
Yes it can. See discussion in [[target]].

=== What is the minimum difficulty? ===
The minimum difficulty, when the target is at the maximum allowed value, is 1.

=== What network hash rate results in a given difficulty? ===
The difficulty is adjusted every 2016 blocks based on the [[Block_timestamp|time]] it took to find the previous 2016 blocks.  At the desired rate of one block each 10 minutes, 2016 blocks would take exactly two weeks to find.  If the previous 2016 blocks took more than two weeks to find, the difficulty is reduced.  If they took less than two weeks, the difficulty is increased.  The change in difficulty is in proportion to the amount of time over or under two weeks the previous 2016 blocks took to find.

To find a block, the hash must be less than the target.  The hash is effectively a random number between 0 and 2**256-1.  The offset for difficulty 1 is
 0xffff * 2**208
and for difficulty D is
 (0xffff * 2**208)/D

The expected number of hashes we need to calculate to find a block with difficulty D is therefore
 D * 2**256 / (0xffff * 2**208)
or just
 D * 2**48 / 0xffff

The difficulty is set such that the previous 2016 blocks would have been found at the rate of one every 10 minutes, so we were calculating (D * 2**48 / 0xffff) hashes in 600 seconds.  That means the hash rate of the network was
 D * 2**48 / 0xffff / 600
over the previous 2016 blocks.  Can be further simplified to
 D * 2**32 / 600
without much loss of accuracy.

At difficulty 1, that is around 7 Mhashes per second.

At the time of writing, the difficulty is 22012.4941572, which means that over the previous set of 2016 blocks found the average network hash rate was
 22012.4941572 * 2**32 / 600 = around 157 Ghashes per second.

=== How soon might I expect to generate a block? ===
(The [https://www.bitcointalk.org/index.php?topic=1682.0 eternal question].)

The average time to find a block can be approximated by calculating:
 time = difficulty * 2**32 / hashrate
where difficulty is the current difficulty, hashrate is the number of hashes your miner calculates per second, and time is the average in seconds between the blocks you find.

For example, using Python we calculate the average time to generate a block using a 1Ghash/s mining rig when the difficulty is 20000:
 $ python -c "print 20000 * 2**32 / 10**9 / 60 / 60.0"
 23.85
and find that it takes just under 24 hours on average.

* Any one grinding of the hash stands the same chance of "winning" as any other.  The numbers game is how many attempts your hardware can make per second.
* You need to know the difficulty (above) and your khash/sec rate (reported by the client).
** [[Mining Hardware Comparison]] has some stats that may help you predict what you could get.
* Visit a calculator or perform the maths yourself,
** http://www.alloscomp.com/bitcoin/calculator.php
** http://www.vnbitcoin.org/bitcoincalculator.php
** https://bitknock.com/calculator
**https://99bitcoins.com/bitcoin-mining-calculator
* Remember it's just probability!  There are no guarantees you will win every N days.

==Related Links==

* [https://docs.google.com/spreadsheet/ccc?key=0AiFMBvXvL2dtdEZkR2J4eU5rS3B4ei1iUmJxSWNlQ0E Bitcoin Difficulty History]
* [https://www.bitcoinmining.com/what-is-bitcoin-mining-difficulty/ What is Bitcoin Mining Difficulty?]
* See also: [[target]]

[[Category:Technical]]
[[Category:Vocabulary]]

[[pl:Trudność]]

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020
This page content is translated by Bitcoin wiki. original right is reserved by them.
