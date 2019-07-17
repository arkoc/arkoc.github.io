---
layout: post
title: Building UWP file manager with fluent design in a month
---

It is possible. Even in working on free time. Yeah, yeah, now you think that this idiot is lying, or he built something horrible. Ok have a look at this:
![screenshot](https://i.imgur.com/PtB1V4P.png) 

You think this is something horrible? Dumb asshole. I don’t understand, what is a purpose of searching something horrible…ok, ok, if it is very important for you, search through [source code]( https://github.com/kommanderapp/kmd-uwp). I’m pretty sure you will find something horrible for you. 

How did it start? My friend informed me about [Windows Developer Day Sweepstakes]( https://developer.microsoft.com/en-us/windows/projects/campaigns/windows-developer-day-sweepstakes) and, and have you seen prizes? “Custom Xbox controller with engraving” this was my motivation for starting development of just another file manager who believes he is the best in the world, like you, me and another guy next to you. Oh, sorry for calling file manager he, maybe she? Fuck this sexism, It…It!

<!--more-->

First, I created GitHub organization and a repo for kommander. Yeah, meat him, his name is kommander. We started developing and learning UWP platform in a row and parallelly adding features to issues list, that we think should be included in the first release. After 1,5-2 week of intensive development, we find out that we have approximately [40 issues/features](https://github.com/kommanderapp/kmd-uwp/milestone/1) to fix/implement.  You know, if you are reading this text, it means that we handled this challenge. 

### What was fun?

* Fluent Design is awesome. You know, I am that kind of developers that spent hours to choose the right size for text-box, so Fluent Design is awesome. Any control you put, they look awesome, nothing to do with them.
* [Windows.Storage](https://docs.microsoft.com/en-us/uwp/api/windows.storage) abstraction is awesome. I guess it is one of best implementations of abstracting files and folders. ( Like IStorageFolder1, IStorageFodler2, very descriptive)
* [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio) rocks. It gives you a really good boost to get familiar with UWP. Thank you, guys.
* Development of the FluentKeybord (you can find it in Hotkey's tab in Settings page). I spent a whole night and 6 cups of coffee to make it happen. [@babgev](https://twitter.com/babgev)


### What was interesting?
* `Windows.Storage` is an abstraction for files and folders, but it is part of UWP. Why? 
* We believe that Microsoft will buy our explorer and replace the ugly one in Windows. 
* Kommander is about 6000 lines of code.
* I was planning to write incredible code that will blow your mind with his cleanness. Now it can blow your mind, but not for cleanness. 
* While working on kommander, I had to implement the following feature: while dragging an element over a tab header (in UWP terms it's a Pivot control), you can drop it there so your file will be moved to the folder contained in that target tab. Technically it meant that I should somehow subscribe to some events etc.... While doing it I encountered a strange exception while instantiating an object. Something that should be impossible. And when I posted a question about it in StackOverflow, I got an answer from an MSFT employee, that it was by design. By design dude? By design, the constructor is throwing an unintelligible exception? Well, I really think that's interesting. U+1F643 [@babgev](https://twitter.com/babgev)

### What was terrible?

* I fucking didn't understand, what does it mean you need to get permission to the folder by opening it with FilePicker, are you serious? Yeah, beside this you also need Privacy Policy for using FilePicker :))
* Restarting VisualStudio while developing UWP application is an ordinary thing.
* Crashing VisualStudio? Thank you UWP.
* When my cigarettes run out while doing the fun part. [@babgev](https://twitter.com/babgev)


I was listening to this music last month for hundreds of time. So, I wish same to you. [Here is it]( https://www.youtube.com/watch?v=3aLyiI2odhU)

I write some random shit on twitter too. [Follow @bot_insane](https://twitter.com/bot_insane)

Meet Babken, he writes shit too. [Follow @babgev](https://twitter.com/babgev).

### Download

<a href="https://www.microsoft.com/store/apps/9PGK1FQJDMJG?ocid=badge"><img src="https://assets.windowsphone.com/85864462-9c82-451e-9355-a3d5f874397a/English_get-it-from-MS_InvariantCulture_Default.png" alt="Get it from Microsoft"  width="250" height="100" /></a>
