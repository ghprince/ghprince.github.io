---
layout: post
title : Run Rubymine and Minecraft with Java 7/8 in Yosemite
date  : 2014-10-22 17:37:30
---

Apple recently released OSX 10.10 Yosemite, and after upgrading (because I just cannot stand the red dot on any App icon) I found two of my Java-based daily apps refused to start.

- RubyMine (6.3.3)
- Minecraft (Launcher 1.5.3, Game 1.8)

It asked me to "install the legacy Java SE 6 runtime".

![Java SE 6 required](/assets/screenshot_2014-10-22-20-15-26.png)

Note that it actually provided a link to download the "legacy" Java SE 6 if you clicked the "More Info..." button. After installing that, both apps should start as usual. Some of my friends did that, and I also re-call I had done similar thing when upgrading from OSX 10.8 Mountain Lion to OSX 10.9 Mavericks last year.

However, why should I do that again? Java 6 is **legacy**, and both RubyMine and Minecraft are **modern** applications, it should be able to run on Java 7 (if not the latest Java 8)! well, unless the developers were lazy, and sometimes they are, and I am going to find that out.

It turned out that I have to use two different approachs to make these two apps work.

#### Note:

1. Here I assume the applications are installed at `/Applications`

2. Check your version and location of your installation of Java.

    Here is mine.

        % java -version
        java version "1.8.0_25"
        Java(TM) SE Runtime Environment (build 1.8.0_25-b17)
        Java HotSpot(TM) 64-Bit Server VM (build 25.25-b02, mixed mode)
        % which java
        /usr/bin/java

### RubyMine

1. Use editor-of-your-choice to open `/Applications/RubyMine.app/Contents/Info.plist`

2. Search for the key of `JVMVersion`

        <key>JVMVersion</key>
        <string>1.6*</string>

3. Change that to your Java version (1.7 for Java 7 or 1.8 for Java 8) and save.

        <key>JVMVersion</key>
        <string>1.8*</string>

4. Open RubyMine and start coding!

### Minecraft

1. Open *Terminal* and run the following command.

        /usr/bin/java -d64 -Xmx1024M -jar /Applications/Minecraft.app/Contents/Resources/Java/Bootstrap.jar

    Here `-d64` forces Java to run in 64-bit mode, and `-Xmx1024M` sets the maxium available memory to the app to be 1024M.

2. Start playing!

*(everything below this line is non-sense and you may stop reading now)*

---

Wait a minute! That's it? So everytime I want to play Minecraft I need to type this long command or copy & paste from somewhere like this webpage?

Well, of course you can set an alias for that. Add the following line your `.bashrc`/`.zshrc`/whatever rc file.

    alias minecraft="/usr/bin/java -d64 -Xmx1024M -jar /Applications/Minecraft.app/Contents/Resources/Java/Bootstrap.jar"

Now you can start playing by just typing `minecraft`.

Nope! I don't want to open an app (in this case, *Terminal*) to start another app, I want the old-fashion double-click-and-play way!

Alright, let's see if what we did on RubyMine also works on Minecraft.

1. Use editor-of-your-choice to open `/Applications/Minecraft.app/Contents/Info.plist`

2. You will find following key string pairs.

        <key>VMOptions</key> <string>-Xmx128m</string>
        <key>JVMVersion</key> <string>1.6+</string>

3. Let's try change them to

        <key>VMOptions</key> <string>-d64 -Xmx1024m</string>
        <key>JVMVersion</key> <string>1.8+</string>

4. > To open "Minecraft.app" you need to install the legacy Java 6 SE runtime.

So that's *NOT* working. I tried another workaround found online which is to replace the value of `CFBundleExecutable` key with a customzied shell script, and that didn't work either. I am no Java expert, in fact I haven't written a single line of Java in my career, so to be honest, I have no idea why these workarounds don't work.

However I do have an not-so-pretty workaround --- *AppleScript*!

1. Open *Script Editor*, and create new file.

2. Add this single line into the file

        do shell script "/usr/bin/java -d64 -Xmx1024M -jar /Applications/Minecraft.app/Contents/Resources/Java/Bootstrap.jar"

3. Save the file to Desktop. Remember to choose the file format of "Application".

4. Double click the "Minecraft.app" icon on Desktop and start playing!

    ![Minecraft.app](/assets/screenshot_2014-10-23-01-39-40.png)

### One Last Thing

Q: Why after so many year, in the era of Java 8, apps like RubyMine (all IntelliJ apps actually) and Minecraft are still using Java 6 by default?

A: I believe that is due to a [bug][1] of Java 7/8 which forces the use of discrete GPU on your Mac and hence more battery drain. But I love my MacBook Air, which does not have a discrete GPU! LOL!

[1]: https://bugs.openjdk.java.net/browse/JDK-8041900
