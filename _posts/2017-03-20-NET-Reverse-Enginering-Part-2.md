---
layout: post
title: .NET Reverse Enginering - Part 2
---

In [Part 1](http://codepool.me/NET-Reverse-Enginering-Part-1/) we cleaned our protected assembly and now it 
is decompilable and runnable. 

In this part, we will try to remove activation checking.

Ok, Let's do it. 

Do you know what is fun in RE? It's like playing chess with a developer of the program. 
It is necessary to guess his steps or start thinking like a developer. 

In my case it isn't just a ordinary developer, it is an author of a strong crypter. 

Does it make sense? 
If you answered yes, I have some bad news for you, you are talking with text.

<!--more-->

Let's run the application and visually observe when activation occurs. 
I will not show a screenshot of this, but activation message box shows right after the form is loaded/rendered. 
After clicking ok program exits. 

Stop reading here, and start thinking, where can it be done? ( You really stopped reading?
I think you are getting this text too serious )

Of course, it will be done in `OnLoad` event. Go Search it: DnSpy > Edit > Search Assemblies > OnLoad. 

![onload event](http://arkoc.github.io/images/re_part2_1.png)

Here it is. Now lets find usage of `VerifyTypeAttributes` function. 
(Funny name yes?) Right click on the function name and click analyze.

![onload function analyse](http://arkoc.github.io/images/re_part2_2.png)

Here we see that it is used in the class constructor, navigate to that constructor by double clicking on it.

![onload event](http://arkoc.github.io/images/re_part2_3.png)

Right-click on it and choose Edit IL Instructions.

![editing IL](http://arkoc.github.io/images/re_part2_4.png)

Modify selected instructions and place `nop` instead.

Run the application again and boonzaaay, now you can reopen your incognito mode browser tab again.

Now let me alone.

Here is a [Music](https://www.youtube.com/watch?v=psjWrGkAil4).



