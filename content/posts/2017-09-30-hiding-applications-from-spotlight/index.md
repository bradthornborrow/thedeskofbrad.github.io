+++
title = "Hiding Applications from Spotlight in macOS"
date = 2017-09-30
tags = [ "macos" ]
+++

I have been using OS X (now macOS) for years, yet this simple trick somehow alluded me. Regularly, I would use the standard `command` + `space` combination to open Spotlight search, then type `photo` and press `enter`, and for whatever reason Photo Booth would open. Similarly, typing type `text` in Spotlight would open TextEdit instead of Textastic (my current text editor of choice). This was both annoying and perplexing, as I rarely used either application. Why would Spotlight suddenly choose these applications instead? I tried the obvious solution of trashing the apps, but would see the standard macOS warning about deleting an application which is required by macOS.

{{< image src="application_required.png" alt="Application required by macOS" position="left" style="border-radius: 4px; margin-left: 2em;" >}}

Of course, I could go ahead and use `rm -r` to delete the application from terminal, but one never knows what hooks are in the backend of macOS. Being a lazy [System Administrator](http://www.thegeekstuff.com/2011/07/lazy-sysadmin/), it was never aggravating enough to the point of researching a solution, so I just lived with it...until now. In the most recent iOS release (iOS 11), Apple finally provided the ability to delete Photo Booth on the iPad (hallelujah!). This alone motivated me to search for a solution to the issue. Little did I know how simple and obvious it would be.

To permanently remove an app from Spotlight search, open System Preferences, Spotlight, then click the Privacy tab. In this window, click the `+` button and browse to the Applications folder and select the application to hide. Alternatively, open the Applications folder in Finder and drag the application to the window. Once an application has been added to the list, it may take a moment for Spotlight to update the index, but shortly you will notice the application no longer appears in searches. 

{{< image src="spotlight_privacy_pane.png" alt="Spotlight Privacy pane" position="left" style="border-radius: 4px; margin-left: 2em;" >}}

Seriously, how did I miss this for all these years? Either way, it's fixed and I'm happy, that's what matters, no? If you would like to contact me or provide feedback on this post, use the [Contact](/contact) page, or message me on [Twitter](http://twitter.com/bftsystems). Thanks!