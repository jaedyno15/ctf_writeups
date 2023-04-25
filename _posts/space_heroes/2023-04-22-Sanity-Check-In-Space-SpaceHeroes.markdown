---
layout: post
title: "Sanity Check In Space - Space Heroes CTF"
date: 2023-04-22 21:22:34 -0400
categories: Space Heroes
---
<style>
    img{
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 50%;
    }
</style>

Category: web

Points: 100

---

Loading up the website we see an image of a robot which hints at needing to check the robots.txt file (file listing the urls that webscrapers like google are allowed/disallowed to display). So adding it to the url takes you there and we see that the disallowed site is **/humans.txt**.
<img src="/ctf_writeups/assets/images/robot.jpeg"  width="30%" height="30%">

Once we head to that page, we see an image of an astronaut holding a cookie and saying "You look pretty human, but we have to be sure. Go eat something and come back here". Again the image is a clue and we know to look at the cookies of the site. 


![cookie](/ctf_writeups/assets/images/cookie.jpg)

I use burpsuite for all of this because I find it easier but you can modify the cookie from the inspect features directly in your browser. We see one cookie which is ``` human=false ```. Changing that value to true and sending it will show the message *"Wow, you really are human, celebrate with us by visiting arrakis"*. 

Taking a guess we can move to the **/arrakis** page. There we see that a password is required but since I push all my requests and responses through burpsuite I saw that there's a comment on the page saying ``` <!-- The password is "FearIsTheMindKiller" --> ``` (again this can be done through the inspect tool of the browser). Putting that in, a new message appears: *"Excellent job, one ultimate challenge awaits you, on krypton"*.

Assuming that this is just another page to head to, we go to **/krypton** where we find a user input box and the message *"This tool pings websites, but in space"*. Because it takes input from us and the input is expected to be a bash command, I know it's probably a command injection. This means we can enter different bash commands and they should execute on the server's OS. My default check is to enter ``` id||ls; ``` which causes the second command (ls) to execute. By doing so we see that flag.txt exists. Now all we need to do is read that file and we should be set. Inputting ``` id||cat flag.txt; ``` gives us the flag: **shctf{exp01ting_w3bs1tes_1N_SP@C3}**