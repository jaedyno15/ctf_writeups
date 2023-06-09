---
layout: post
title:  "Whos That Pokemon - UMDCTF"
date:   2023-04-30 15:52:23 -0400
categories: UMDCTF
---

Category: Mobile

Points: 352

Solves: 116

---

<h1> Description </h1>

Who's That Pokemon? Enter the Pokemon's name to find the flag!

Download File: [whos_that_pokemon.apk](/ctf_writeups/assets/challenges/whos_that_pokemon.apk)

---

We have an apk file which is used to run apps on android devices. So to run it, we need an android emulator and a decompiler for the file. [Android Studio][android studio] is probably the best tool for android app development and comes with a built in emulator while giving you easy access to all the code. After playing around with it for a bit, I couldn't get my emulator working so I had to try a different route. 

Moving over to the [bluestacks][bluestacks] emulator which had a pretty quick process to upload local apk files although it took a few tries to realize it will only run with administrator privilege. 

![whos_that_pokemon_load_screen](/ctf_writeups/assets/images/whos_that_pokemon_load_screen.png)

Right when I launch the game, there is a screen that takes one input and that is the guess of the pokemon. Inputting a random answer doesn't do anything beyond making a sound. This doesn't seem to have any other info, so it is time to decompile it with APKTool to get the individual files and can then delve further.

The syntax to decompile this file after installing apktool is ``` apktool d apk_file.apk -o output_folder_name ```

![whos_that_pokemon_smali](/ctf_writeups/assets/images/whos_that_pokemon_smali.png)


From there I began to just search around all the files and see what kind of stuff I could find. After a while of searching around, I came across the *strings.smali* file which looked promising (smali_classes2/com/example/whosthatpokemon/R$strings.smali). 

It contained a list of memory addresses and variable names of the different strings in hex. We see that there's a **pokemon** string with a corresponding value of ***0x7f100096***. I assumed that if we could then find a reference to this hex address or the string name itself then we would find the answer. While researching basic reverse engineering of android applications, I saw mention that these values can be found within the res/values folder.

![smali res/values folder](/ctf_writeups/assets/images/smali_res_files.png)


So either grepping this entire folder for 'pokemon' and/or the hex address or narrowing down the search by seeing what kind of data is held in each of the files is the next step. I let grep run in the background and decided to look around the files myself while I waited. Starting easy, I assumed that things such as the integers, drawables, bools, and colors wouldn't be of much help. The only files that I found anything interesting in was the public.xml and strings.xml files. The public.xml gave me the information I already had so I guess this could have been a good place to start instead of where I did. 

{% highlight bash %}
    grep "pokemon" public.xml
    <public type="drawable" name="whos_that_pokemon" id="0x7f0700df" />
    <public type="raw" name="whosthatpokemon" id="0x7f0f0003" />
    <public type="string" name="pokemon" id="0x7f100096" />
    <public type="style" name="Theme.Whosthatpokemon" id="0x7f110262" />
{% endhighlight %}

Repeating the above step with strings.xml showed not only the string names but also their values. Using grep to see if the pokemon string is there, we get the name of the pokemon, *Terrapulseonic*.

{% highlight bash %}
    grep "pokemon" strings.xml
    <string name="app_name">whos_that_pokemon</string>
    <string name="pokemon">Terrapulseonic</string>
{% endhighlight %}

Heading back over to the our game, we enter the name and we get a new screen with the flag **UMDCTF{Andr01d_$triNgs_@re_n0T_secUr3}**

![whos_that_pokemon flag](/ctf_writeups/assets/images/whos_that_pokemon_flag.png)



---

Notes: I went in with a basic understanding of android development but no practice with it for CTFs, I referenced this writeup by [Harshit Maheshwari][reference writeup] and they go much more in-depth about the tools to use and good practices for reverse engineering apk files.  

I did not get around to looking at this category until the final day and I knew a major patch had been issued. I am not 100% about what it was but after looking at the code, I believe the images that contained the flag were not encrypted before. This meant that as soon as you opened it in a decompiler and found the image you could get the flag, completely bypassing the actual challenge. Now the image is encoded/encrypted so that is no longer a viable option but is still a good one to look out for. 




[android studio]: https://developer.android.com/studio
[bluestacks]: https://www.bluestacks.com/
[reference writeup]: https://infosecwriteups.com/android-ctf-kgb-messenger-d9069f4cedf8



