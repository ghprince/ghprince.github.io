---
layout: post
title : Getting Rid of Annoying "(2)" after Mac Computer Name
date  : 2014-10-28 10:31:22
---

I had this problem ever since I upgraded to Yosemite. After I searched around, people had the similar issue on different releases of OSX.

Here are the basic symptoms.

- *Computer Name* has `(2)` at the end, while the hostname stays normal, e.g. my MacBook Air has hostname of `gogao-mba.local`, but the *Computer Name* becomes `gogao-mba (2)`.

- After manually changing the *Computer Name* at *System Preferences* -> *Sharing* (removing `(2)`), `(2)` comes back automatically after several seconds.

    * However it might work in some network environments. For me, I don't have the problem at my office network, but `(2)` pops up again after I get home.

- The number might go up day after day, I read someone has it up to `(629)`!

Here are some solutions I found online. I tried all of them and proved them not working.

- > Manually change *Computer Name* and restart Mac.

    `(2)` comes back before I restart!

- > Disable one of your two network interfaces.

    I only have one network interface. I <3 MacBook Air.

- > Reset your router.

    Same issue.

- > Pick a different name for NetBIOS at System Preferences -> Network -> Advanced -> WINS

    Seriously?

- > Choose Never Sleep at System Preference -> Energy Saver

    WTF?!

OK, let me debug it myself. In *Console.app*, I found this interesting line timestamped when `(2)` came back.

    discoveryd[53]: Basic Bonjour Changing computer name from 'gogao-mba' to 'gogao-mba (2)'

Mmmm...might be something is wrong with *discoveryd* and it kept the old records by mistake?

So what is the rule of thumb when it comes debugging? Restarting the suspecious daemons/services!

    sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.discoveryd.plist
    sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.discoveryd.plist

Then I removed the `(2)` and waited...

...

and it didn't come back ever since!!!

Problem solved?!! I cannot believe how much time I have been wasted searching around and trial-and-error. I guess the lesson learned was

**You are better than you think you are.**

Note: If you are on OSX before Yosemite, replace `discoveryd` with `mDNSResponder`.