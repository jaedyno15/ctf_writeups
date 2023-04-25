---
layout: post
title: "Bynary Encoding - Space Heroes CTF"
date: 2023-04-22 21:51:30 -0400
categories: Space Heroes
---


Category: crypto

Points: 148

---


We have a txt file that appears to be empty. However, if you try and highlight the file contents you see that there are a bunch of whitespaces.

![whitespaces](/ctf_writeups/assets/images/whitespace.png)

I just competed in a previous CTF that also had a challenge about this so I knew that the [whitespace programming language] [whitespace_wiki] consists of tabs, spaces, and linefeeds. I tried putting it into [naokikp's decoder] [whitespace_decoder] because it has the option to make my input 'visible' (translates the spaces, tabs, and linefeeds to chars) but it wasn't giving me any results in terms of converting to binary. So, I decided to do the conversion myself.

![decoder](/ctf_writeups/assets/images/whitespace_unconverted.png)

I put them into vs code, converting all the S's into 0s, the T's to 1s, and the L's into spaces using the find-replace-all function. This gives a nice series of binary values which can be converted to ASCII.

Heading over to [cyberchef] [cyberchef], we can input the binary and convert it to ASCII using the 'from binary' option. 

![cyberchef_bynary](/ctf_writeups/assets/images/cyberchef_bynary.png)

This gives us the flag: **shctf{a_bl1nd_m4n_t3aching_an_4ndr0id_h0w_to_pa1nt}**.


[whitespace_wiki]: https://en.wikipedia.org/wiki/Whitespace_(programming_language)
[whitespace_decoder]: https://naokikp.github.io/wsi/whitespace.html
[cyberchef]: https://gchq.github.io/CyberChef/