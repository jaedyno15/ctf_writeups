---
layout: post
title:  "Pokeball Escape - UMDCTF"
date:   2023-04-30 17:21:45 -0400
categories: UMDCTF
---

Category: Mobile

Points: 425

Solves: 83


---

<h1> Description </h1>

You are stuck in a Pokeball, break out!

Hint: I do not mean exit the app

Download File: [pokeball_escape.apk](/ctf_writeups/assets/challenges/pokeball_escape.apk)

---

Loading up the apk file in [bluestacks][bluestacks] to see what we're working with, we are given a single screen and a message pops up every few seconds saying "!! CONDITIONS NOT MET TO ESCAPE !!". 


<img  src="/ctf_writeups/assets/images/pokeball_escape.png"  style="display: block; margin: 0 auto">


Heading over to my vm and using apktool to decompile the apk with ``` apktool d pokeball_escape.apk -o pokeball-escape-smali ```. Then using dex2jar we can get the java files and read those.

{% highlight bash%}
    d2j-dex2jar pokeball_escape.apk -o pokeball-escape-jar.jar
    Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
    dex2jar pokeball_escape.apk -> pokeball-escape-jar.jar
{% endhighlight %}

Now opening up jd-gui, we can view all of the apk's code. 

![jd_gui](/ctf_writeups/assets/images/pokeball_escape_jd_gui.png)

Right as we open the MainActivity.class file there is an if-statement that stands out. It is checking the systemInfo of our device (in this case the bluestacks emulator) for the name "Devon Corporation". Knowing what needs to be changed, we head back to our emulator and go to Settings->Phone->Create a custom profile and then enter our known value as the manufacturer, brand, and model (I don't know which value it is checking so may as well test them all at once).

![bluestacks_modification](/ctf_writeups/assets/images/change_device_info_UMDCTF.png)

Exiting the settings, the game brings us to a new screen with the flag **UMDCTF{c0ngrAtz_0N_th3_e5s@pe!}**

![pokeball_escape_flag](/ctf_writeups/assets/images/pokeball_escape_flag.png)





[bluestacks]: https://www.bluestacks.com/