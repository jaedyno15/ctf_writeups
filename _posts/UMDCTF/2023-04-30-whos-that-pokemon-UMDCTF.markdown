---
layout: post
title:  "whos that pokemon - UMDCTF"
date:   2023-04-30 15:52:23 -0400
categories: UMDCTF
---

Category: Mobile

Points: 352


---

<h1> Description </h1>

Who's That Pokemon? Enter the Pokemon's name to find the flag!

Download File: [whos_that_pokemon.apk](/ctf_writeups/assets/challenges/whos_that_pokemon.apk)

---

We have an apk file which is used to run apps on android devices. So to run it, we need an android emulator and a decompiler for the file. [Android Studio][android studio] is probably the best tool for android app development and comes with built in emulators for android devices while giving you easy access to all the code. After playing around with it for a bit, I couldn't get my emulator working so I had to try a different route to emulate it. 

So moving over to [bluestacks][bluestacks] which is an emulator used for running android games (among others) on you desktop. The process to upload local games is pretty quick although it took a few tries to get the file to upload without error. 

![whos_that_pokemon_load_screen](/ctf_writeups/assets/images/whos_that_pokemon_load_screen.png)

Right away when we launch the game, we have a screen that only takes one input and that is to guess the pokemon. Inputting a random answer doesn't do anything beyond making a sound. This doesn't seem to have any other info for us, so it is time to decompile it with APKTools so that we get the individual files and can then be further looked into. 

![whos_that_pokemon_smali](/ctf_writeups/assets/images/whos_that_pokemon_smali.png)



From there I began to just search around all the files and see what kind of stuff I could find. After a while of searching around, I can across the strings.smali file (smali_directory/smali_classes2/com/example/whosthatpokemon/R$strings.smali) which looked promising. It contained a list of memory addresses and variable names of the different strings in hex. We see that there's a **pokemon** string with a corresponding value of ***0x7f100096***. I assumed that if we could then find a reference to this hex address or the string name itself then we would find the answer. While researching basic reverse engineering of android applications, I saw mention that these values can be found witin the res/values folder.

![smali res/values folder](/ctf_writeups/assets/images/smali_res_files.png)


So you can either grep this entire folder for 'pokemon' and/or the hex address or you can narrow down your search by seeing what kind of data is held in each of the files. I let grep run in the background and decided to look around the files myself while I waited. Starting easy, I assumed that things such as the Integers, drawables, bools, and colors wouldn't be of much help. The only files that I found anything interesting on was the public.xml and strings.xml files. The public.xml just gave me the information I already had so I guess this could have been a good place to start instead of where I did. However, the strings.xml showed not only the string name but also its value. Using grep to see if the pokemon string is there, we get the name of the pokemon, *Terrapulseonic*.


{% highlight bash %}
    grep "pokemon" strings.xml
    <string name="app_name">whos_that_pokemon</string>
    <string name="pokemon">Terrapulseonic</string>
{% endhighlight %}

Heading back over to the our game, we enter the name and we get a new screen with the flag **UMDCTF{Andr01d_$triNgs_@re_n0T_secUr3}**

![whos_that_pokemon flag](/ctf_writeups/assets/images/whos_that_pokemon_flag.png)



---

Notes: I went in with a basic understanding of android development but no practice with it for CTFs, I referenced this writeup by [Harshit Maheshwari][reference writeup] and they go much more in-depth about the tools to use and good practices reverse engineers these files. 

I did not get around to looking at this category until the final day and I knew a major patch had been issued. I am not 100% about what it was but after looking at the code, I believe the images that contained the flag were not encoded before. This meant that as soon as you opened it in a decompiler and found the image you could get the flag, completely bypassing the actual challenge. Now the image is encoded/encrypted so that is no longer a viable option but is still a good one to look out for. 




[android studio]: https://developer.android.com/studio
[bluestacks]: https://www.bluestacks.com/
[reference writeup]: https://infosecwriteups.com/android-ctf-kgb-messenger-d9069f4cedf8



