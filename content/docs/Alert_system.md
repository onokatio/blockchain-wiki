---
title: "アラートシステム"
date: 2020-10-07T00:34:19+09:00
draft: true
---

[[File:Prefinal alert.png|thumb|300px|The prefinal alert message in the status bar of the GUI client]]Bitcoin versions 0.3.10 introduced an '''alert system''' which allowed messages about critical network problems to be broadcast to all clients.

The alert system has been completely retired.<ref name="org">https://bitcoin.org/en/alert/2016-11-01-alert-retirement</ref> The code was removed since 0.13.0 and since 0.14.0 any old nodes will receive a static hard-coded "Alert Key Compromised" message.

When an alert was in effect, the message it contains would appear in the status bar of all clients and in the "errors" field of RPC ''getinfo''. Any script registered with the <code>-alertnotify</code> command-line option will be notified.

===Alert message===
Alerts are broadcast using the same [[network|TCP relay system]] as ''block'' and ''tx'' messages. They are not encoded in a special transaction. Unlike block and tx relaying, alerts are sent at the start of every new connection for as long as the alert is in effect. This ensures that everyone receives the alert.

Alerts contain this information:
* How long to relay the alert.
* How long to consider the alert valid.
* An alert ID number.
* A list of alerts that should be canceled upon receipt of this alert.
* Exactly which versions of Bitcoin are affected by the alert. Unaffected versions still relay the alert for the benefit of older versions.
* Alert priority.
* The alert text.

Only alerts that are signed by a specific ECDSA public key are considered valid. Some known private key holders were [[Satoshi Nakamoto]], [[User:gavinandresen|Gavin Andresen]] and [[User:Theymos|theymos]], but the private key was made public after the alert system was retired, so there is no longer any special group of alert-key-holders.

===Safe mode===
Until version 0.3.20, Bitcoin went into safe mode when a valid alert was received. In safe mode, all RPC commands that send BTC or get info about received BTC return an error. Current Bitcoin versions no longer go into safe mode in response to alerts, though Bitcoin ''will'' still go into safe mode when it detects on its own that something is seriously wrong with the network.

Even though Bitcoin no longer automatically disables RPC when an alert is live, it is wise for Bitcoin sites to shut down when an alert has been issued. To detect an active alert, poll the "errors" field of ''getinfo''.

To test safe mode, run Bitcoin with the -testsafemode switch. To override a real safe mode event, run Bitcoin with the -disablesafemode switch.

==History==
{{multiple image|align=vertical|width=300
|image1=alert.png
|image2=bitcoin-alert-cli.png
|footer=An alert appearing on bitcoin clients
}}

The alert system was hastily implemented by [[Satoshi Nakamoto]] after the [[value overflow incident]] on August 15, 2010. Satoshi never actually used this system; it remained dormant until the [[February 20, 2012 protocol change]], for which an alert was issued on February 18.<ref>[https://bitcoin.org/en/alert/2012-02-18-protocol-change February 20, 2012 Protocol Changes] at bitcoin.org</ref>

In 2016, the alert system was retired<ref name="org"/> because of the possibility of privileged users sending political alert messages, <!-- because of consensus that there shouldn't be privileged users in a decentralized system in the first place, [reword?]--> and because of the possibility of the alert key having been taken from [[Mark Karpelès]] by the Japanese police in 2014.<ref>https://github.com/bitcoin/bitcoin/pull/7692</ref>

The alert key was scheduled to be released to the public in May 2017, but this was postponed.<ref name="org"/> It was eventually published in July 2018.

===Past alerts===
{| class="wikitable" border="1"
|-
! ID
! Sent date
! Expires (UTC)
! Versions
! Priority
! Message
|-
| 1010
| Feb 18, 2012
| Feb 21 02:47:15
| All
| 100
| See bitcoin.org/feb20 if you have trouble connecting after 20 February
|-
| 1011
| Mar 16, 2012
| cancelled May 15, 2012
| 0.5 - 0.5.3
| 5000
| URGENT: security fix for Bitcoin-Qt on Windows: http://bitcoin.org/critfix
|-
| 1012
| Mar 16, 2012
| cancelled May 15, 2012
| 6.0
| 5000
| URGENT: security fix for Bitcoin-Qt on Windows: http://bitcoin.org/critfix
|-
| 1013
| Mar 16, 2012
| cancelled May 15, 2012
| 5.99
| 5000
| URGENT: security fix for Bitcoin-Qt on Windows: http://bitcoin.org/critfix
|-
| 1015
| May 15, 2012
| May 16, 2013
| 0.1 - 0.4.5
| 5000
| URGENT: upgrade required, see http://bitcoin.org/dos for details
|-
| 1016
| May 15, 2012
| May 16, 2013
| 0.4.99 - 0.5.4
| 5000
| URGENT: upgrade required, see http://bitcoin.org/dos for details
|-
| 1020
| May 15, 2012
| May 16, 2013
| 0.6.0
| 5000
| URGENT: upgrade required, see http://bitcoin.org/dos for details
|-
| 1032
| March 12, 2013
| March 13, 2013
| 0.8.0
| 5000
| URGENT: chain fork, stop mining on version 0.8
|-
| 1033
| March 19, 2013
| March 20, 2013
| 0.1 - 0.7.2
| 10
| See http://bitcoin.org/may15.html for an important message
|-
| 1034
| May 9, 2013
| June 8, 2013
| 0.1 - 0.7.2
| 10
| Action required: see http://bitcoin.org/may15.html for more information
|-
| 1040
| April 11, 2014
| cancelled
| 0.9.0
| 5000
| URGENT: Upgrade required: see https://www.bitcoin.org/heartbleed/
|-
| 1041
| April 11, 2014
| April 11, 2015
| 0.9.0
| 5000
| URGENT: Upgrade required: see https://www.bitcoin.org/heartbleed
|}

==See Also==

* https://github.com/bitcoin/bitcoin/pull/7692 - The pull request where removing the alert system from [[Bitcoin Core]] was discussed

[[Category:Technical]]
[[Category:Vocabulary]]

==References==
<references/>

==External links==
https://bitcoin.org/en/alerts

{{Bitcoin Core documentation}}

Ⓒ Bitcoin Wiki https://en.bitcoin.it/ 2020
This page content is translated by Bitcoin wiki. original right is reserved by them.
