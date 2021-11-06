+++
title = "Binding Automator Scripts to Shortcut keys in macOS"
date = 2017-08-06
tags = [ "macos" ]
+++

For years after moving from Windows to Mac, I used [Karabiner](https://karabiner-elements.pqrs.org) to make my trusty Logitech S510 keyboard act normal in OS X, at least normal from an ex-Windows users perspective.  Karabiner also had the added benefit of allowing a standard PC keyboard to mimic other keys, such as the MacBook `Eject` key.  For example, I could bind the eject function to `Print Screen`, allowing `Ctrl` + `Shift` + `Print Screen` to lock my MacBook quickly from the keyboard.

After a time, my Windows habits slowly faded and OS X became first nature to me.  I found myself rarely using any of the PC key-bindings, preferring native Mac functionality.  The final straw came with macOS Sierra, which broke Karabiner once and for all.  The only feature I missed was the ability to bind PC keys to functions such as `Eject`.  Yes, there are other utilities available, such as [Keyboard Maestro](https://www.keyboardmaestro.com/main/), but they seemed like overkill, and I didn't want any external dependencies.

After some research, I discovered the ability in macOS to bind Automator scripts to a shortcut in the Keyboard System Preferences panel.  For example, I could create a simple Automator script to run the following AppleScript block.  This script will open the standard macOS shutdown prompt as if I had pressed `Ctrl` + `Eject` on my MacBook keyboard.

```applescript
on run {input, parameters}
	tell application "loginwindow" to «event aevtrsdn»
end run
```


Once I saved this Automator script in the Services folder (`~/Library/Services/`), it could be bound to a shortcut key combination in Keyboard Preferences under Shortcuts, Services.  See the screenshot below for an example of several I have setup on my own system.

{{< image src="automator_services.jpg" alt="Keyboard Services System Preferences" position="center" style="border-radius: 4px; width: 95%;" >}}

Another advantage to this solution is that it works everywhere, whether I'm working on my MacBook keyboard, or any old PC keyboard I happen to have plugged in.

If you would like to contact me or provide feedback on this post, use the [Contact](/contact) page, or message me on [Twitter](http://twitter.com/bftsystems). Thanks!