+++
title = "Open New Terminal at Folder in macOS"
date = 2017-10-20
tags = [ "macos" ]
+++

Previously when I was a Windows user (back in the dark times), I used the __Command Prompt Here__ tool from the Windows 95 PowerToys collection offered by Microsoft. When I transitioned to OS X, I was disappointed there was not a similar option or tool available. Of course, after some research I discovered this was built into the system. I just needed to look in the right place.

To add this function in Finder, open System Preferences, Keyboard, then click on the Shortcuts tab. Scroll through the list until you find the _New Terminal at Folder_ option. Once this is checked, it will now be available under _Services_ in Finder when you right-click on a folder. Also, while you are in this tab, I recommend you review all the options in the list, as there are many interesting tools to be found here.

{{< image src="new_terminal_tab.jpg" alt="New Terminal Tab at Folder" position="left" style="border-radius: 4px; margin-left: 2em;" >}}

As you can see in the screenshot, I have checked the _New Terminal Tab at Folder_ option (my preferred setting). With this option, if there is no Terminal window open, it opens a new shell at the folder. If Terminal is already open, the shell is opened in a new tab, rather than a separate window. Here is how the option appears in Finder when you right-click on a folder.

{{< image src="finder_services.jpg" alt="Finder Services" position="center" style="border-radius: 4px; margin-left: 2em;" >}}

Now that you've know how to open a folder directly in Terminal, you may have noticed it opens using the default macOS shell (black text on white background). If you are like me and prefer a different shell, you must change several settings to ensure Terminal always uses your preferred options. My personal preference is the _Homebrew_ shell, as I grew up using green screen terminals, and I find the green text on black background easier on my eyes. To ensure Terminal opens using your preferred shell, look under Terminal Preferences, General, select _On startup, open: New window with profile_ and select the profile you wish to use. 

{{< image src="terminal_startup_profile.jpg" alt="Terminal Startup Profile" position="center" style="border-radius: 5px; margin-left: 2em;" >}}

Interestingly, this does not change the _default_ options. If you open a separate Terminal window or tab, it will still use the macOS default shell. To change the default, look under Terminal Preferences, Profiles, select your preferred shell, then click the _Default_ button at the bottom of the list. This will ensure all new shells open using your default options. 

{{< image src="change_default_shell.jpg" alt="Change Default Shell" position="left" style="border-radius: 4px; margin-left: 2em;" >}}

There are many other Terminal options available, I suggest you take the time to play around and see what works best for you. I plan to cover more macOS Terminal tricks in future posts.

If you would like to contact me or provide feedback on this post, use the [Contact](/contact) page, or message me on [Twitter](http://twitter.com/bftsystems). Thanks!
