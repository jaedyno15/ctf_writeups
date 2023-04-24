---
layout: post
title:  "Galactic Federation - Space Heroes CTF"
date:   2023-04-23 18:10:18 -0400
categories: space heroes
---



Category: RE 

Points: 221 

---


We're given a bin file that brings us to a login page when we try running it. Pulling up Ghidra to decompile the file we can see what the login error checking is. 

{% highlight c++ %}
    bVar1 = false;
    obfuscate(local_78);
    bVar2 = std::operator==(local_78,"hktpu");
    if (bVar2) {
      obfuscate(local_58);
      bVar1 = true;
      bVar2 = std::operator==(local_58,"8fs7}:f~Y;unS:yfqL;uZ");
      if (!bVar2) goto LAB_00403dc9;
      bVar2 = true;
    }
    else {
LAB_00403dc9:
      bVar2 = false;
    }

{% endhighlight %}

We see in the login_page function that it compares the user (var local_78) to **'hktpu'** and the password (local_58) to **'8fs7}:f~Y;unS:yfqL;uZ'**. We can also see here that the inputs are going through a function called *obfuscate()* which is altering them.

So using ghidra to open that function we can see that each character gets the char *'\a'* added to it. 

{% highlight c++ %}
basic_string * obfuscate(basic_string *param_1)

{
  char *pcVar1;
  ulong uVar2;
  ulong in_RSI;
  int local_1c;
  
  local_1c = 0;
  while( true ) {
    uVar2 = std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::length();
    if (uVar2 <= (ulong)(long)local_1c) break;
    pcVar1 = (char *)std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::
                     operator[](in_RSI);
    *pcVar1 = *pcVar1 + '\a'; // this is the line we care about 
    local_1c = local_1c + 1;
  }
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::basic_string
            (param_1);
  return param_1;
}

{% endhighlight %}

All we need to do is write a quick script to subtract this value from each of the chars (or do it manually by converting the ascii values to integers first).

{% highlight c++ %}
#include <iostream>
#include <string>

// all written in c++

std::string deobfuscate(const std::string& input){
    std::string output;
    output.reserve(input.size());
    for (char x : input){
        output += static_cast<char>(x - '\a');
    }
    
    return output;
}

int main(){
    std::string obfuscated_user = "hktpu";
    std::string obfuscated_password = "8fs7}:f~Y;unS:yfqL;uZ";
    std::string deobfuscated_user = deobfuscate(obfuscated_user);
    std::string deobfuscated_password = deobfuscate(obfuscated_password);
    std::cout << "Deobfuscated user: " << deobfuscated_user << std::endl;
    std::cout << "Deobfuscated password: " << deobfuscated_password << std::endl;
    return 0;
}
{% endhighlight %}

```
Deobfuscated user: admin
Deobfuscated password: 1_l0v3_wR4ngL3r_jE4nS
``` 

Now that we're in, we're given lots of options and each of those options have even more choices. So instead of blindly trying to find something we can go back to ghidra and look for a function that prints the flag. From there we can usually work backwards and get the desired output. 

We see a *print_flag* function so now we can find all references to that function (rick-clicking and selecting 'show references to'). Following the first reference we see the *'collapse_economy'* function. Following that function too we finally see the one we need to interact with, which is *'adjust_economy'*. 

{% highlight c++ %}
    if ((currency == 0) &&
        (bVar1 = std::operator==((basic_string *)galactic_currency[abi:cxx11],"usd"), bVar1)) {
        bVar1 = true;
    }
    else {
        bVar1 = false;
    }
    if (bVar1) {
        collapse_economy();
    }
{% endhighlight %}

Within the *'adjust_economy'* function, *'collapse_economy'* will be called if currency == 0 and the galactic_currency == 'usd'. 

So starting with the easy one we can change galactic currency to usd. Either keep using ghidra to find the right function or just play around with the program until you find the *presidential_decree* function and from there we can use the *change_galactic_currency* option which takes any string input and set it to usd. 

Then we can use head back to the *'adjust_economy'* function and use *inflate_currency* which takes an integer as a percentage to change the currency value. Before we try adding any values though we can check what our input will do. We see {% highlight c++ %} currency = currency + (var / 100) * currency {% endhighlight %} where var is the percentage we input (var is not the original variable name, ghidra lets you rename variables which is what I have done to make reading the code easier). Given this info, we can input **-100** or any value you want that will cause the currency to drop to 0 or below.

As soon as both changes are made, a series of text dialogues print out along with the flag: **shctf{w4it_uH_wh0s_P4y1Ng_m3_2_y3L1_@_tH15_gUy?}**