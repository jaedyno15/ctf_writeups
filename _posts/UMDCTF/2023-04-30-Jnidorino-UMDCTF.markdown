---
layout: post
title:  "Jnidorino - UMDCTF"
date:   2023-04-30 18:43:21 -0400
categories: UMDCTF
---

Category: Mobile

Points: 481

Solves: 43


---

<h1> Description </h1>

I hate using native libraries so much! I think I forgot a function call somewhere. Can you find it?

Flag Format: UMDCTF{...}

Download File: [pokeball_escape.apk](/ctf_writeups/assets/challenges/pokeball_escape.apk)

---

As with all of the other mobile challenges I have done for this CTF I run the apk file on an android emulator and the intro screen had nothing to interact with or any hints about actually interacting with the app. So moving on, I decompiled it with apktools and then used jd gui to analyze the code. 

We see that the entire MainActivity.class file is encrypted in what looks like base64 so I tried decoding a few just to see if they were real values but they only returned random chars. 

![main_activity_jnidorino](/ctf_writeups/assets/images/main_activity_jnidorino.png)


The description mentions native libraries and as we can see in the MainActivity.class file one of the only plaintext lines is 'System.loadLibrary("jnidorino");' so I went looking for that library and found it at (JNIdorino-smali/lib/x86_64/libjnidorino.so). Opening it in IDA which is a binary disassembler lets us see all the strings by going View->Open Subviews->Strings.

![ida_strings](/ctf_writeups/assets/images/IDA_Strings.png)

Now we filter by Java_com_example_jnidorino_MainActivity to see only those strings that we were seeing before. We get 501 results but when I get a count of the strings in the evolve() function from the MainActivity.class file there is only 500. So saving both of those sets of data as python list variables and checking for the difference between them with a quick script we see that the one string that is different is *GhLagoBPGjAdjYQldkhrYdgky*.

{% highlight python %}
    difference = []
    MainActivityStrings = [...]
    LibJnidorinoStrings = [...]

    for string in LibJnidorinoStrings:
        if string not in MainActivityStrings:
            difference.append(string)
    
    print("Missing string is: " + str(difference))

{% endhighlight %}

{% highlight bash %}
    Missing string is: ['GhLagoBPGjAdjYQldkhrYdgky']
{% endhighlight %}

So now lets look at that string within IDA and after following it for a bit we see a new string *"VU1EQ1RGe2wwdjNfbkB0MXZlX2ZVbnN9"*. 

![goal_string](/ctf_writeups/assets/images/goal_string_UMDCTF.png)


Again, I'll try converting it from base64 to ascii and this time we get the flag UMDCTF{l0v3_n@t1ve_fUns}
