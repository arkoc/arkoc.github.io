---
layout: post
title: .NET Reverse Enginering - Part 1
---

Do you ever have the feeling that you are a piece of shit and you are just a man who knows how to combine source codes found from StackOverflow?

No ? Then go fuck yourself. Personally, I periodically have that feeling.

This feeling challenges me to learn new things, find ways to test my brain and playing with reverse engineering was a just the right thing for me to start feeling like a hard-core developer (or just something bit different from shit) again.

Here is my secret of learning new things:

1. Feel like a shit.
2. Do some "cool" stuff.
3. Wait 15 minutes and go to point 1. ( Don't use gotos in source codes. )

Ok, when you are in the right mood, let's start with the main topic. 

Lastelly we have been obfuscating a crypter that was written with VB.NET. Our main goal was to find out the logic and methods of encryption. I think that one of the hardest parts of reverse enginneering is determining logic, not just writing a patch or finding out a secret key from the program. 

In this part:

1. Removing various anti-reversing protections.
2. Make program decompilable and runnable.

<!--more-->

Here is a perfect place for a huge text about trying multiple deobfuscators with no luck, let's just skip this step and let's try with our hands. (right hand is the most productive, and feels better).

Let's load our assembly with [dnSpy](https://github.com/0xd4d/dnSpy). Then let's look through our module and classes and what we notice is that methods defined in the assembly aren't beeing decompiled. 

![messed methods](http://arkoc.github.io/images/re_part1_1.png)

This means that our methods are encrypted in some hidden section (not in IL section) and when the program runs it decrypts methods from a section and puts them in right place. To be sure let's open our assembly with CFF Explorer and navigate to PE sections.

![cff explorer](http://arkoc.github.io/images/re_part1_2.png)

Yes, we are right. Now we must find a place in the source code where functions decryption and fixing occurs. After some observations it becomes clear that the process occures in `<Module>.ctor`. This constructor is called before application's main entry point. This means that it is a perfect place for implementing this kind of work.

Reopen dnSpy, then right Click on Assembly -> Go to <Module>.cctor.

![module ctor](http://arkoc.github.io/images/re_part1_3.png)

Because of the fact that there are functions in `<Module>` class that are not decompiled as well, we came to a conclusion that methods fixing must should occure during the  first call, so let's dive in.

Here we see a call to the function that is imported from kernel32.dll `VirtualProtect`. This function is used for setting access attributes of memory blocks.  Ok, what this gives us: the program gets sections address (where the real methods are) sets `PAGE_EXECUTE` attribute on it and....we don't want to know more. We have found the exact place where methods are beeing fixed, that's it.

![virtual protect](http://arkoc.github.io/images/re_part1_4.png)

Let's set a breakpoint after that call and then dump the assembly from memory.

![dump assembly](http://arkoc.github.io/images/re_part1_5.png)

Save it and open the newly saved module with dnSpy.
Now we see that our methods are decompiled and are feeling good. We did it!

But there is still a problem, our .exe is broken, because we have forgotten to remove the "methods fixing function". The program still thinks that it needs to fix the methods (but methods are already fixed ) and completely messes up the things.

Now let's remove that function and save assembly again.

![saving assembly](http://arkoc.github.io/images/re_part1_6.png)
![saving assembly](http://arkoc.github.io/images/re_part1_7.png)

Then click on module then File -> Save Module.
Don't forget to set MD Writer options like this:

![md_writer](http://arkoc.github.io/images/re_part1_8.png)

Ok, now we have a runnable executable with decompiled methods.

Here is a perfect [ReverseEnginering](https://www.youtube.com/watch?v=Itt1nn9aWz0) music for you.

In the next part we will try to hack activation.
