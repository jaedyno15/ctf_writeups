---
layout: post
title: "Escape The Matrix - Summit CTF"
date: 2023-04-16 01:22:34 -0400
categories: summit
---


Category: Forensics

Points: 150

---


<h1> Description </h1>
We see some bad bits escaping our grasp. Their trying to climb away with intel! Go find out what they are doing...

Pcap file: [EsacpingTheMatrix.pcap](/ctf_writeups/assets/challenges/EscapingTheMatrix.pcap)

---

When we open it up we see a series of DNS packets that all contain data. Since we're intercepting communications from the enemy I assume the data I can see is part of the message. So following the first stream (stream0) I can see that the data is in the format (encrypted data).(repeated encrypted data for multiple packets).summit.

![stream1](/ctf_writeups/assets/images/escape_matrix_stream_1.png)

I'm pretty sure that the first set of encrypted data is what we're looking for so just for testing on a small stream first I export this stream from Wireshark. Then isolating just the unique parts inside VS code (I'm sure there is a more efficient way to do this with tshark but that's fine) gives:

```
VGhlIGZsYWcg
eW91IGhhdmUg
YmVlbiB3YWl0
aW5nIGZvciBp
cyAuLi4gUGF1
c2luZyBmb3Ig
ZHJhbWF0aWMg
ZWZmZWN0IC4u
LiA6IFN1bW1p
dENURntTdXNf
RDBtYTFuX240
bWVzfQ==
```

So now I'm left with what looks like a base64 encryption (if you're not familiar with encryptions, using [dcode] [dcode] to identify the cipher/encryption is really helpful). Heading over to [cyberchef] [cyberchef] and decoding from base64 we get ```The flag you have been waiting for is ... Pausing for dramatic effect ... : SummitCTF{Sus_D0ma1n_n4mes}```. So we did not end up needing any of the other packet streams.

---

Note: Out of curiosity I repeated this process with a different stream and it just returned jibberish so I guess I was lucky that I tried the correct stream first. I also determined that the second set of base64 data that was repeating (```Tscc1QuycZN4```) just indicated what stream it was a part of. Decoding it did not result in readable data either. 



[dcode]: https://www.dcode.fr/cipher-identifier
[cyberchef]: https://gchq.github.io/CyberChef/
