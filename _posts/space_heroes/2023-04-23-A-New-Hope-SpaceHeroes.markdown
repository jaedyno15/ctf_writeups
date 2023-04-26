---
layout: post
title: "A New Hope - Space Heroes CTF"
date: 2023-04-23 20:56:54 -0400
categories: Space Heroes
---


Category: Forensics

Points: 184

---

<h1> Description </h1>

Princess Leia has been kidnapped! She managed to send a message to this droid we have recovered. It was damaged while we were recovering it however. It seems that sometimes you have to tear something down, in order to build them back up.

Can you recover the message?

Download File: [A_New_Hope.pptx](/ctf_writeups/assets/challenges/A_New_Hope.pptx)

---

We get a ppt file to download which contains a single slide that looks like this.

![initial_ppt](/ctf_writeups/assets/images/new_hope_init.png)


An easy way to see if there is anything hidden is just to start moving stuff around on the editor. For this challenge it does help, we can see that behind the two images there is a third one but it doesn't load. The next step is to try and get that image file.

![found_image](/ctf_writeups/assets/images/new_hope_found_image.png)

ppt files are actually just zip files so we can extract the data and we're left with three directories: _rels, docProps, and ppt. Heading into ppt then media we see the three images.

If we try to open image1.png we get an error so the file must be corrupt. There is a website for pretty much everything, so I pull up a png repairer site (I used [officerecovery] [recovery] but there are lots of others). It returned the file and now it opens and we can see the flag (this site leaves a watermark that could have blocked the flag so I'm just lucky it didn't).

![png_solve](/ctf_writeups/assets/images/new_hope_solve.png)

The flag is **shctf{help_m3_ob1_y0u’re_my_0n1y_hope}**.



[recovery]: https://online.officerecovery.com/